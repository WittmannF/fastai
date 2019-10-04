---
Título: Suporte
---

## Visão global

apoio fastai é fornecida através [github issue tracker](https://github.com/fastai/fastai/issues) eo [forums](https://forums.fast.ai/).

A maioria dos problemas, em particular os problemas com seu código, deve ser discutido na [forums](https://forums.fast.ai/). Quer encontrar um fio existente que já está discutindo problemas semelhantes, ou iniciar um novo tópico.

Se você é certeza que você encontrou um bug no software `fastai` envie um relatório de bug usando [github issue tracker](https://github.com/fastai/fastai/issues).

solicitações de recursos são melhor discutidas na [forums](https://forums.fast.ai/).

É sempre uma boa idéia para pesquisar no fórum para ver se alguém já relatou um problema semelhante. Normalmente, ele irá ajudá-lo a encontrar uma solução muito mais rápida.

Se o problema não está estritamente relacionada com o `base de código fastai`, mas para os módulos que ele depende (por exemplo,` pytorch`, `torchvision`,` spacy`, `numpy`), muitas vezes você pode encontrar soluções, procurando o erro mensagens a partir do rastreamento de pilha de erro no Google ou o seu motor de busca favorito.



## problemas de relatórios

Antes de fazer um novo relatório de assunto, por favor:

1. Certifique-se de que você tem a última `conda` e / ou` pip`, dependendo do gerenciador de pacotes que você usa:
    `` `
    pip instalar pip -U
    Conda instalar Conda
    `` `
    e, em seguida, repita os passos e ver se o problema que você queria relatório ainda existe.

2. Certifique-se [your platform is supported by `pytorch-1x`](https://github.com/fastai/fastai/blob/master/README.md#is-my-system-supported). Você pode ter que construir `pytorch` de origem se ele não é.

3. Certifique-se que você siga [the exact installation instructions](https://github.com/fastai/fastai/blob/master/README.md#installation). Se você improvisa e funciona isso é ótimo, se não agradar RTFM;)

4. Verifique o documento [Troubleshooting](/troubleshoot.html).

5. Pesquisa [forums](https://forums.fast.ai/) por problemas semelhantes já notificados.

Se você ainda não conseguiu encontrar uma resolução, por favor poste o seu problema em:

* Se é um problema de instalação em
[this thread](https://forums.fast.ai/t/fastai-v1-install-issues-thread/24111/1).
* Para todos os outros questão quer encontrar um segmento relevante existente ou criar uma nova.

Quando você faz um post, certifique-se de incluir em sua mensagem:

1. um breve resumo do problema
2. a backtrace pilha completa, se você receber um erro ou exceção (e não apenas o erro).
3. Como pode ser reproduzido
4. a saída do script a seguir (incluindo o \ '\' \ `abertura texto e fechamento \ '\' \ 'para que seja formatado corretamente em seu post):
   `` `
   git clone https://github.com/fastai/fastai
   cd fastai
   python -c 'fastai.utils de importação; fastai.utils.show_install (1)'
   `` `

   O script de relatórios não funcionará se `pytorch` não foi instalado, por isso, se esse é o caso, em seguida, enviar os seguintes detalhes:
   * Saída do `pitão --version`
   * O seu sistema operacional: Linux / OSX / windows / e linux distro + versão se relevante
   * Saída de `nvidia-smi` (ou dizer CPU se não houver)

5. Só se é um problema de instalação, a instalação exata etapas seguidas. Não há necessidade de listar os pacotes instalados, que geralmente é muito barulhento, uma vez que podem conter centenas de dependências nele. Apenas o seu Conda / PIP instalar comandos que você fez.

Se a saída resultante é super longo, por favor, colá-lo para https://pastebin.com/ e incluir um link para o seu colar, mas só se for centenas e centenas de linhas de saída - caso contrário a postar todas as informações em seu post é uma bondade, para que, no futuro, outros leitores podem comparar suas notas com a deles e posts pastebin são susceptíveis de desaparecer.



## e Don'ts

* Por favor não envie imagens com mensagens traceback / erro de pilha - não podemos copiar-n-paste das imagens, em vez colá-los na íntegra em seu post.

* Código e rastreamento nas mensagens devem ser `code`-formatado. Se você não sabe remarcação, você pode selecionar o trecho que você quer fazer `code`-formatado e, em seguida, apertar o botão de código no menu de remarcação GUI do post. Quando você faz isso, ele usará tamanho fixo fonte monoespaçada o que torna muito mais fácil de ler.

* Se o seu sistema está configurado para usar uma localidade não-Inglês, e sua mensagem de erro inclui resultado não-Inglês, se possível, voltar a executar o código problemático após a execução:

   `Exportação LC_ALL = en_US.UTF-8`

    De modo que as mensagens de erro será em Inglês. Você pode executar `locale` para ver quais locales você instalou.



## PRs

Se você encontrou um bug e saber como corrigi-lo, por favor, enviar um PR com a correção [here](https://github.com/fastai/fastai/pulls).

Se você gostaria de contribuir com um novo recurso, por favor, discuti-lo na [forums](https://forums.fast.ai/) primeiro.

Certifique-se ler [CONTRIBUTING](https://github.com/fastai/fastai/blob/master/CONTRIBUTING.md).

Obrigado.
