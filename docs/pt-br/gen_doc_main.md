---
título: Manutenção doe
---

## Processo para contribuir para os docs

Aqui está como você pode contribuir para a `documentação fastai` em apenas 4 passos.

### Passo 1. Crie um `git branch fastai`

O processo de criação de um ramo (com um garfo), incluindo um programa que vai fazer isso por você em uma única etapa, e submeter o PR é explicado em detalhes no [How to Make a Pull Request (PR)] (/ dev / git.html # how-to-make um-pull-pedido pr)

### Passo 2. Configuração

A partir do repo fastai ou sua pasta Caixa ramo bifurcado, instalar os módulos de desenvolvedor necessários:

```bash
pip install -e ".[dev]"
```bash
cd docs_src
MKLINK / d BMI .. \ docs \ BMI
```bash
cd docs_src
mklink /d imgs ..\docs\imgs
```bash
ferramentas / run-after-git-clone # ou Python ferramentas \ Run-após-git-clone em janelas
```bash
tools/run-after-git-clone  # or python tools\run-after-git-clone on windows
```txt
1. Edite 2. Ferramentas / build-docs 3. Jekyll (githubpages)
     | | |
docs_src / *. ipynb ----> docs / *. html -----> docs.fast.ai/*html
docs / *. md ------------------------------> docs.fast.ai/*html
```txt
1. edit    2. tools/build-docs  3. jekyll (githubpages)
     |            |                   |
docs_src/*.ipynb ----> docs/*.html -----> docs.fast.ai/*html
docs/*.md ------------------------------> docs.fast.ai/*html
```bash
pip instalar -e" .[dev]"
```

If you're on windows, you also need to convert the Unix symlink between `docs_src\imgs` and `docs\imgs`. You will need to (1) -> 'str':
    "Esta função retorna FooBar * vezes"
    retorno "foobar" * vezes
```bash
pip install -e ".[dev]"
```bash
Ferramentas / build-docs --document-new-FNS docs_src / data_block.ipynb
```bash
tools/build-docs --update-nb-links  docs_src/data_block.ipynb
```python
show_doc (LabelLists.foobar)
```python
def foobar(self, times:int=1)->'str':
    "This functions returns FooBar * times"
    return "FooBar" * times
```bash
Ferramentas / build-docs docs_src / data_block.ipynb
```bash
tools/build-docs --document-new-fns docs_src/data_block.ipynb
```bash
git commit docs docs_src / data_block.ipynb / data_block.html
```python
show_doc(LabelLists.foobar)
```bash
Ferramentas / build-docs --update-nb-ligações docs_src / data_block.ipynb
```bash
tools/build-docs docs_src/data_block.ipynb
```bash
Ferramentas / build-docs fastai.subpackage.module
```bash
git commit docs_src/data_block.ipynb docs/data_block.html
```
show_doc (...., title_level = 4)
```bash
tools/build-docs --update-nb-links docs_src/data_block.ipynb
```python
<IPython.core.display.Markdown object>
<IPython.core.display.Markdown object>
<IPython.core.display.Markdown object>
```bash
tools/build-docs fastai.subpackage.module
```bash
Ferramentas / build-docs --update-nb-ligações docs_src / data_block.ipynb
```
show_doc(...., title_level=4)
```json
 "Metadados": {
  "Jekyll": {
   "palavras-chave": "fastai",
   "Toc": "false",
   "Title": "Welcome to fastai"
  },
  [here](/dev/git.html#how-to-make-a-pull-request-pr)):

```python
<IPython.core.display.Markdown object>
<IPython.core.display.Markdown object>
<IPython.core.display.Markdown object>
```

Para atualizar apenas os cadernos modificados sob `run docs_src`:

```bash
tools/build-docs --update-nb-links docs_src/data_block.ipynb
```

Para atualizar específicas `* ipynb` nbs:

```json
 "metadata": {
  "jekyll": {
   "keywords": "fastai",
   "toc": "false",
   "title": "Welcome to fastai"
  },
  [...]
```

. Para atualizar `fastai específica *` módulo:

```bash
make docs
```

Para forçar uma reconstrução de todos os portáteis e não apenas os modificados, use a opção `-f`.

```bash
python tools/build-docs
```

Para digitalizar um módulo e adicionar novas funções do módulo para notebook documentação:

```bash
python tools/build-docs docs_src/notebook1.ipynb docs_src/notebook2.ipynb ...
```

Para anexar automaticamente novos métodos fastai ao seu notebook documentação correspondente:

```bash
python tools/build-docs fastai.subpackage1.module1 fastai.subpackage2.module2 ...
```

Use o `-h` para mais opções.

Alternativamente, [here](/dev/develop.html#things-to-run-after-git-clone) pode ser executado a partir do notebook.

Para atualizar todos os cadernos sob `run docs_src`:

```bash
python tools/build-docs -f
```

Para atualizar único arquivo python específica:

```bash
python tools/build-docs --document-new-fns
```

`Update_nb = true` inserções recentemente adicionado métodos do módulo para os documentos que já não foram documentados.

Alternativamente, você pode atualizar um módulo específico:

```bash
python tools/build-docs --update-nb-links
```

### Atualizando html única

Se você não estiver a sincronizar a base de código com a sua documentação, mas fez algumas alterações manuais aos cadernos de documentação, então você não precisa atualizar os cadernos, mas apenas convertê-los em `.html`:

Para converter `docs_src / * ipynb` para` docs / * html`:

* Apenas o modificado `* ipynb`:

```python
update_notebooks('.')
```

* Específicas `* ipynb`s:

```python
update_notebooks('gen_doc.gen_notebooks.ipynb', update_nb=True)
```

* Força para reconstruir todas as `* ipynb`s:

```python
update_notebooks('fastai.gen_doc.gen_notebooks', dest_path='fastai/docs_src')
```


## Links e âncoras

### Validar links e âncoras

Depois de confirmar as alterações doc favor validar que todos os links e `# anchors` estão corretas.

Se é a primeira vez que você está prestes a executar o verificador de link, instalar o [docs.fast.ai](https://docs.fast.ai/) primeiro.

Depois de cometer as novas mudanças, primeiro, esperar alguns minutos para páginas github para sincronizar, caso contrário, você estará testando um site ao vivo desatualizado.

Então faça:

```bash
python tools/build-docs -l
```

O script será relatar problemas silenciosos e apenas como ele encontra-los.

Lembre-se, que está testando o site ao vivo, por isso, se você detectar problemas e fazer as alterações, lembre-se de primeiro confirmar as alterações e aguarde alguns minutos antes de re-teste.

Você também pode testar o site localmente antes de cometer as alterações, consulte: [https://docs.fast.ai/data_block.html](/data_block.html).

Para testar o site course-v3.fast.ai, fazer:

```bash
python tools/build-docs -l docs_src/notebook1.ipynb docs_src/notebook2.ipynb ...
```

## Trabalhando com Markdown

### Pré-Visualização

Se você trabalha em markdown (Md) arquivos que ajuda a ser capaz de validar as alterações para que o layout resultante não seja quebrado. [docs_src/data_block.ipynb](https://github.com/fastai/fastai/blob/master/docs_src/data_block.ipynb) parece funcionar muito bem para esta finalidade ( `pip instalar grip`). Por exemplo:

```bash
python tools/build-docs -fl
```

vai abrir um navegador com a remarcação processado como html - ele usa github API, então isso é exatamente como ele vai olhar no github depois de cometê-lo. E aqui é um alias de mão:

```bash
cd tools/checklink
./checklink-docs.sh
```

assim você não precisa se lembrar a bandeira.

### Dicas Markdown

* Se você usar itens numerados e seu número ultrapassa 9 tem de mudar para 4 espaços em branco caracteres recuo para os parágrafos pertencentes a cada item. Sob 9 ou com \ * você precisa caracteres de 3 espaços em branco como um recuo líder.
* Ao construir tabelas certifique-se de usar `- | --` e não` - + - `para separar os cabeçalhos - github não vai torná-lo adequadamente contrário.

local ## Testing localmente

Instalar pré-requisitos:

* Debian / Ubuntu:

   ```bash
./checklink-course-v3.sh
```bash
docs cd
agrupar Jekyll exec servir
```bash
grip -b docs/dev/release.md
```bash
perl -le 'm # docs / (. *?) \. html # &&! -e "docs_src / $ 1.ipynb" && imprimir para @ARGV' docs / * html
`` `

e removê-los do git.
