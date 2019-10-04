---
Título: aprendizagem profunda em um Shoestring
---

## Introdução

A maioria das tarefas de aprendizado de máquina são muito computação intensiva e muitas vezes exigem uma grande quantidade de hardware caro, o que nunca tem o suficiente, já que é quase sempre o caso que se poderia fazer melhor se houvesse mais hardware. GPU RAM em um particular é o principal recurso que está sempre faltando. Este tutorial irá focar vários temas que irá explicar como você pode realizar feitos impressionantes sem precisar gastar mais dinheiro na aquisição de hardware.

** Nota: este tutorial é um trabalho em progresso e precisa de mais conteúdo e polimento, ainda, que já é bastante útil. Ele vai ficar melhorou ao longo do tempo e as contribuições são bem-vindos também. **

## enxuto codificação notebook

Um notebook aprendizagem de máquina típico consome mais e mais GPU RAM, e, muitas vezes, a fim de completar a formação é preciso reiniciar o kernel notebook várias vezes, em seguida, executar novamente cuidadosamente algumas células, saltando sobre muitas outras células, em seguida, executando mais algumas células, atingindo a memória limite, e repetindo este processo novamente, desta vez de pular cuidadosamente ainda outros conjuntos de células e correndo ainda mais algumas células até que novamente o OOM terrível é encontrado.

Então, a primeira coisa a entender é que, como você fazer o treinamento do consumo GPU RAM aumenta gradualmente. Mas em qualquer fase você pode recuperar a maior parte da RAM GPU utilizada até agora, salvando o seu modelo em formação, libertando todos os outros objetos não são mais necessários e, em seguida, recarregar o modelo salvo.

Isso geralmente não é um problema em um ambiente de programação normal, onde o bem-comportado funções limpeza após a si mesmos e vazar nenhuma memória, mas no caderno jupyter a mentalidade é diferente - não é apenas uma função de espalhar-se para muitas células e cada célula pode consumir memória , mas desde que suas variáveis ​​nunca saem do âmbito da memória é amarrado até o kernel notebook é reiniciado.

Assim, um notebook típico CNN vai da seguinte forma:

```
# imports
import ...

# init main objects
model = ... # custom or pretrained
data = ...  # create databunch
learn = cnn_learner(data, model, metrics)

# train
learn.fit_one_cycle(epochs)

# finetune
learn.unfreeze
learn.fit_one_cycle(epochs)

# predict
preds,y = learn.get_preds(...)
```

Normalmente, se você escolheu o direito tamanho do lote e seu modelo não é muito profundo e denso, você pode ser capaz de completar todo este notebook, digamos, 8 GB de RAM GPU. Se o modelo que você escolheu é intensivo de memória, você pode completar com êxito a formação da cabeça NN adicionado por fastai (a parte antes de descongelar), mas não será capaz de afinar porque seus requisitos de memória são muito maiores. Finalmente, você pode ser capaz de ter memória suficiente para afinar, mas, infelizmente, nenhuma memória será deixado para a fase de previsão (especialmente se for uma previsão complexo).

Mas, é claro, este foi um exemplo muito básico. Uma abordagem mais avançada irá, por exemplo, começar com o treinamento básico, seguido por ajustes finos palco, em seguida, irá alterar o tamanho das imagens de entrada e repita a formação cabeça e ajuste fino, e depois novamente. E há muitas outras etapas que podem ser adicionados.

A chave é ser capaz de recuperar GPU e RAM geral entre todas essas etapas para que um determinado notebook poderia fazer muitas muitas coisas sem precisar ser reiniciado.

Em uma função normal, uma vez que sua acabado, todas as suas variáveis ​​locais será destruído, o que deve recuperar qualquer memória usada por aqueles. Se uma variável é um objeto que tem referências circulares, não vai liberar sua memória até que uma chamada programada de `gc.collect` vai chegar e fazer o lançamento.

No caso dos notebooks não há destruição variável automática, uma vez que todo o notebook é apenas um escopo único e só desligar o kernel notebook irá liberar a memória automaticamente. E, em seguida, o objeto `Learner` é um animal muito complexo que não pode evitar referências circulares e, portanto, mesmo se você` del learn` não irá liberar seus recursos automaticamente. Não podemos nos permitir esperar para `gc.collect` chegar em algum momento no futuro, precisamos da RAM agora, ou não seremos capazes de continuar. Portanto, temos de chamar manualmente `gc.collect`.

Portanto, se quisermos memória a meio caminho libertar através do notebook, a maneira estranha de fazer isso é:

```
del learn
gc.collect()
learn = ... # reconstruct learn
```

Há também [ipyexperiments](https://github.com/stas00/ipyexperiments/) que foi inicialmente criado para resolver este problema exato - para automatizar esse processo de variáveis-exclusão de auto criando âmbitos artificiais (o próprio módulo evoluído desde para fazer muito mais).

nota: você também pode querer chamar `torch.cuda.empty_cache` se você realmente quer ver a memória liberada com` nvidia-smi` - isto é devido à pytorch cache. A memória é liberada, mas você não pode dizer que a partir de `nvidia-smi`. Você pode se você usar [pytorch memory allocation functions](https://pytorch.org/docs/stable/notes/cuda.html#memory-management), e é outra função a ser chamada) - `ipyexperiments` irá fazê-lo automaticamente para você.

Mas a parte chata e propenso a erros é que de qualquer forma você tem que reconstruir o objeto `Learner` e potencialmente outros objetos intermediários.

Vamos acolher uma nova função: `learn.purge` que resolve este problema de forma transparente. O último trecho de código é, assim, é substituído com apenas:

```
learn.purge()
```
que remove qualquer um dos `coragem Learner` que não são mais necessários e recarrega o modelo de GPU, o que também ajuda a reduzir [memory fragmentation](/dev/gpu.html#gpu-ram-fragmentation). Portanto, sempre que você precisar os expurgos de memória já não, esta é a maneira de fazê-lo.

Além disso, a funcionalidade de purga está incluído no `learn.load` e é realizada por padrão. Você pode substituir o comportamento padrão de não para purgar com `purga = argumento false`).

Então, em vez de precisar fazer:

```
learn.fit_one_cycle(epochs)

learn.save('saved')
learn.load('saved')
learn.purge()
```

não é necessária a chamada para `learn.purge`.

Você não precisa injetar `learn.purge` entre os ciclos de formação da mesma configuração:
```
learn.fit_one_cycle(epochs=10)
learn.fit_one_cycle(epochs=10)
```
As invocações subsequentes do função de treinamento não consomem mais GPU RAM. Lembre-se, quando você treina você apenas alterar os números nos nós, mas toda a memória que é necessário para esses números já foram atribuídos.



### nuances Optimizer

O objeto `learn` pode ser reposto entre os ciclos de ajuste para economizar memória. Mas você precisa saber quando é a coisa certa e prático de fazer. Por exemplo, em uma situação como esta:

```
learn.fit_one_cycle(epochs=10)
learn.fit_one_cycle(epochs=10)
```

purgar o objeto `learn` entre dois` chamadas fit` não fará diferença para o consumo GPU RAM. Mas se você tivesse outras coisas acontecendo no meio, como o congelamento / descongelamento ou alterar o tamanho da imagem, você deve ser capaz de recuperar um monte de GPU RAM.

Como explicado na seção anterior `learn.load` internamente chama` learn.purge`, mas por padrão ele faz learn.opt` não está claro `(o estado otimizador). É um padrão seguro, porque, alguns modelos como GAN quebrar se `learn.opt` fica limpo entre` ciclos fit`.

No entanto, `learn.purge` faz clara` learn.opt` por padrão.

Portanto, se o seu modelo é sensível a limpar `learn.opt`, você deve estar usando uma dessas 2 maneiras:

1.
   ```
   learn.load(name) # which calls learn.load(name, with_opt=True)
   ```
2.
   ```
   learn.purge(clear_opt=False)
   ```

either of which will reset everything in `learn`, except `learn.opt`.

If your model is not sensitive to `learn.opt` resetting between fit cycles, and you
want to reclaim more GPU RAM, you can clear `learn.opt` via one of the following 3 ways:

1.
   ```
   learn.load('...', with_opt=False)
   ```
2.
   ```
   learn.purge()  # which calls learn.purge(clear_opt=True)
   ```
3.
   ```
   learn.opt.clear()
   ```

### Learner release

If you no longer need the `learn` object, you can release the memory it is consuming by calling:

```
del aprender
GC.Collect ()
```
`learn` is a very complex object with multiple sub-objects, with unavoidable circular references, so `del learn` won't free the memory until `gc.collect` arrives some time in the future, so since we need the memory now, we call it directly.

If the above code looks a bit of an eye-sore, do the following instead:

```
learn.destroy ()
```
`destroy` will release all the memory consuming parts of it, while leaving an empty shell - the object will still be there, but you won't be able to do anything with it.


### Inference

For inference we only need the saved model and the data to predict on, and nothing else that was used during the training. So to use even less memory (general RAM this time), the lean approach is to `learn.export` and `learn.destroy` at the end of the training, and then to `load_learner` before the prediction stage is started.

```
# Fim de um cenário de treinamento potencial
learn.unfreeze ()
learn.fit_one_cycle (épocas)
learn.freeze ()
learn.export (destruir = True) # ou learn.export () + learn.destroy ()

# Início de inferência
aprender = load_learner (caminho, ensaio = ImageList.from_folder (caminho / 'teste'))
Preds = learn.get_preds (DS_TYPE = DatasetType.Test)
```


### Conclusion

So, to conclude, when you finished your `learn.fit` cycles and you are changing to a different image size, or you unfreeze, or you do anything else that no longer requires previous structures on GPU, you either call `learn.purge` or `learn.load('saved_name')` and you should have most of your GPU RAM back as it was where you have just started, plus the allocated memory for the model. That's of course, if you haven't created some other variables that hold some GPU RAM tied up.

And for inferences the  `learn.export`, `learn.destroy` and `load_learner` sequence will require even less RAM.

Therefore now you should be able to do a ton of things without needing to restart your notebook.

### Local variables

python releases memory when variable's reference count goes to 0 (and in special cases where circular references are referring to each other but have no "real" variables that refer to them). Sometimes you could be trying hard to free some memory, including calling `learn.destroy` and some memory refuses to go. That usually means that you have some local variables that hold memory. Here is an example:

```
classe FeatureLoss (nn.Module): [...]
feat_loss = FeatureLoss(vgg_m, blocks[2:5], [5,15,2])
aprender = unet_learner (dados, arco, loss_func = feat_loss, ...)
[...]
learn.destroy()
```
In this example, `learn.destroy` won't be able to release a very large chunk of GPU RAM, because the local variable `feat_loss` happens to hold a reference to the loss function which, while originally takes up almost no memory, tends to grow pretty large during `unet` training. Once you delete it, python will be able to release that memory. So it's always best to get rid of intermediate variables of this kind (not simple values like `bs=64`) as soon as you don't need them anymore. Therefore, in this case the more efficient code would be:

```
classe FeatureLoss (nn.Module): [...]
feat_loss = FeatureLoss(vgg_m, blocks[2:5], [5,15,2])
aprender = unet_learner (dados, arco, loss_func = feat_loss, ...)
del feat_loss
[...]
learn.destroy()
`` `


## CUDA sem memória

Um dos principais culpados que levam a uma necessidade de reiniciar o notebook é quando o notebook ficar sem memória com o conhecido de todos `CUDA fora do memory` (OOM) exceção. Este é abordado neste [section](/troubleshoot.html#cuda-out-of-memory-exception).

E também você precisa saber sobre o bug atual em ipython que podem impedi-lo de ser capaz de continuar a usar o notebook em OOM. Este problema é principalmente tomada cuidado de automaticamente no fastai, e é explicado em detalhes [here](/troubleshoot.html#memory-leakage-on-exception).


## Anatomy uso da memória GPU

1. Sobre 0,5GB por processo é usado pelo contexto CUDA, ver [Unusable GPU RAM per process](/dev/gpu.html#unusable-gpu-ram-per-process). Esta memória é consumida durante a primeira chamada para `.cuda`, quando o primeiro tensor é movido para GPU. Você pode testar seu cartão com:

   `` `
   -c python 'de fastai.utils.mem import *; b = gpu_mem_get_used_no_cache (); preload_pytorch (); print (gpu_mem_get_used_no_cache () - b);'
   `` `

2. Um modelo pré-treinados consome um pouco de GPU RAM. Embora existam muitos modelos diferentes lá fora, que podem variar muito em tamanho, um modelo pré-formados recentemente carregado como ResNet tipicamente consome algumas centenas de MB de RAM GPU.

3. Modelo de treinamento é onde a maior parte da GPU RAM está sendo consumido. Quando o primeiro lote da primeira época passa pelo modelo, os picos de uso GPU RAM porque ele precisa definir as coisas e muito mais memória temporária é usado do que em lotes subseqüentes. No entanto, o alocador pytorch é muito eficiente e se houver pouca GPU RAM disponível, o pico será mínimo. A partir do lote de 2 em diante e para todas as épocas seguintes do mesmo `fit` chamar o consumo de memória é constante. Assim, se os primeiros segundos de `fit` foram bem sucedidos, ele será executado até sua conclusão.

   Dica: se você está ajustando hiperparâmetros para caber na RAM GPU da sua placa, é suficiente para executar somente uma época de cada `chamada fit`, de modo que você pode escolher rapidamente` bs` e outros parâmetros para ajustar a RAM disponível. Depois de feito isso, aumentar o número de épocas em `fit` chama para obter o treinamento real indo.

Se você gostaria de ter uma noção de quanta memória cada usos de palco (ignorando caching pytorch, que você não pode com `nvidia-smi`), aqui estão as ferramentas para usar:

* Para per-notebook e por célula: [ipyexperiments](https://github.com/stas00/ipyexperiments/).
* Para per-época utilização: [`PeakMemMetric`](/callbacks.mem.html#PeakMemMetric).
* Para ainda mais refinado profiling: [GPUMemTrace](/utils.mem.html#GPUMemTrace).


** uso de memória Real **

* Caches pytorch memória através de seu alocador de memória, então você não pode usar ferramentas como `nvidia-smi` para ver quanta memória real está disponível. Então você nem precisa usar funções de gerenciamento de memória do pytorch para obter essa informação ou se você quer contar com `nvidia-smi` você tem que liberar o cache. Consulte esta [document](/dev/gpu.html#cached-memory) para mais detalhes.


## TODO / Help Wanted

Este tutorial é um trabalho em andamento, algumas áreas parcialmente coberto, alguns não em todos. Então se você tem o know-how ou ver algo está faltando envie um PR.

Aqui está uma lista de desejos:

* [`torch.utils.checkpoint`](https://pytorch.org/docs/stable/checkpoint.html) pode ser usado para usar menos RAM GPU por gradientes de computação re-. Aqui está uma boa [article](https://medium.com/tensorflow/fitting-larger-networks-into-memory-583e3c758ff9) em profundidade explicando esse recurso no tensorflow e [another one](https://medium.com/huggingface/training-larger-batches-practical-tips-on-1-gpu-multi-gpu-distributed-setups-ec88c3e51255) que fala sobre a teoria. Aqui está a [notebook](https://github.com/prigoyal/pytorch_memonger/blob/master/tutorial/Checkpointing_for_PyTorch_models.ipynb) da pessoa que contribuiu esta função para pytorch (ainda usa pré pytorch-0,4 sintaxe). Precisamos pytorch / exemplos fastai. Contribuições são bem-vindos.
