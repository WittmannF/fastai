---
Título: Como iniciar um treinamento distribuídos
---

Se você tiver várias GPUs, a maneira mais confiável para usar todos eles para o treinamento é usar o pacote distribuído a partir de pytorch. Para ajudá-lo, há um módulo distribuídos em fastai que tem funções auxiliares para torná-lo realmente fácil.

## Prepare o seu script

formação distribuída não funciona em um notebook, então primeiro, limpar seu experimentos notebook e preparar um script para executar o treinamento. Por exemplo, aqui é um script mínimo que treina uma ampla ResNet em CIFAR10.

`` `Pitão
de fastai.vision import *
de wrn_22 importação fastai.vision.models.wrn

caminho = untar_data (URLs.CIFAR)
ds_tfms = ([*rand_pad(4, 32), flip_lr (p = 0,5)], [])
Dados = ImageDataBunch.from_folder (caminho, = válidos 'teste', ds_tfms = ds_tfms, bs = 128) .normalize (cifar_stats)
aprender = Learner (dados, wrn_22 (), métricas = precisão)
learn.fit_one_cycle (10, 3e-3, wd = 0,4, div_factor = 10, pct_start = 0,5)
python ```

## Add the distributed initialization

Your script is going to be executed in a different process that will each happen on a different GPU. To make this work properly, add the following introduction between your imports and the rest of your code.

```
de fastai.distributed import *
argparse importação
analisador = argparse.ArgumentParser ()
parser.add_argument ( "- local_rank", type = int)
args = parser.parse_args ()
torch.cuda.set_device (args.local_rank)
torch.distributed.init_process_group (backend = 'nccl', init_method = 'env: //')
python ```

What we do here is that we import the necessary stuff from fastai (for later), we create an argument parser that will intercept an argument named `local_rank` (which will contain the name of the GPU to use), then we set our GPU accordingly. The last line is what pytorch needs to set things up properly and know that this process is part of a larger group.

## Make your learner distributed

You then have to add one thing to your learner before fitting it to tell it it's going to execute a distributed training:
```
aprender = learn.to_distributed (args.local_rank)
python ```
This will add the additional callbacks that will make sure your model and your data loaders are properly setups.

Now you can save your scriptn here is what the full example looks like:

```
de fastai.vision import *
de wrn_22 importação fastai.vision.models.wrn
de fastai.distributed import *
argparse importação
analisador = argparse.ArgumentParser ()
parser.add_argument ( "- local_rank", type = int)
args = parser.parse_args ()
torch.cuda.set_device (args.local_rank)
torch.distributed.init_process_group (backend = 'nccl', init_method = 'env: //')

caminho = untar_data (URLs.CIFAR)
ds_tfms = ([*rand_pad(4, 32), flip_lr (p = 0,5)], [])
Dados = ImageDataBunch.from_folder (caminho, = válidos 'teste', ds_tfms = ds_tfms, bs = 128) .normalize (cifar_stats)
aprender = Learner (dados, wrn_22 (), métricas = precisão) .to_distributed (args.local_rank)
learn.fit_one_cycle (10, 3e-3, wd = 0,4, div_factor = 10, pct_start = 0,5)
```

## Launch your training

In your terminal, type the following line (adapt `num_gpus` and `script_name` to the number of GPUs you want to use and your script name ending with .py).
```
python -m torch.distributed.launch --nproc_per_node = {} {num_gpus script_name}
`` `

O que vai acontecer é que o mesmo modelo será copiado em todos os seus GPUs disponíveis. Durante o treinamento, o conjunto de dados completo será aleatoriamente ser dividido entre as GPUs (que vai mudar em cada época). Cada GPU vai pegar um lote (em que dataset fracionada), passá-lo através do modelo, calcule a perda, em seguida, volta-se propagam os gradientes. Em seguida, eles irão partilhar os seus resultados e média-los, o que significa, como sua formação é o equivalente a uma formação com um tamanho de lote de `batch_size x num_gpus` (onde` batch_size` é o que você usou em seu script).

Uma vez que todos eles têm os mesmos gradientes nesta fase, eles vão al executar a mesma atualização, de modo que os modelos ainda mais será a mesma após esta etapa. Então formação continua com o próximo lote, até que o número de iterações desejados é feito.
