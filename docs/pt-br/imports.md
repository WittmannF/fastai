---
title: Imports
---

## Introdução
---------------------------

Para oferecer suporte à computação interativa, o fastai fornece acesso fácil aos módulos externos comumente usados. Uma importação em estrela, como:

    from fastai.basics import *
    

preencherá o espaço de nome atual com esses módulos externos, além de funções e variáveis ​​específicas do fastai. Esta página documenta essas importações de conveniência, definidas em [fastai.imports](https://github.com/fastai/fastai/blob/master/fastai/imports) .

Nota: como este documento foi criado manualmente, ele pode estar desatualizado quando você o ler. Para obter a lista atualizada de importações, use:

    python \- c ' a = set (\[\* vars (). keys (), "a"\]); do fastai.basics import \*; print (\* classificado (set (vars (). keys ()) - a), sep = " \\ n ") '

_Os nomes em negrito são módulos. Se um objeto tiver um alias durante sua importação, o nome original será listado entre parênteses._

|Nome|Descrição|
|--- |--- |
| [**`csv`**](https://docs.python.org/3/library/csv.html) |Leitura e gravação de arquivos CSV|
| [**`gc`**](https://docs.python.org/3/library/gc.html) |Interface do coletor de lixo|
| [**`gzip`**](https://docs.python.org/3/library/gzip.html) |Suporte para arquivos gzip|
| [**`os`**](https://docs.python.org/3/library/os.html) |Interfaces diversas do sistema operacional|
| [**`pickle`**](https://docs.python.org/3/library/pickle.html) |Serialização de objeto Python|
| [**`shutil`**](https://docs.python.org/3/library/shutil.html) |Operações de arquivo de alto nível|
| [**`sys`**](https://docs.python.org/3/library/sys.html) |Parâmetros e funções específicos do sistema|
| [**`warnings`**](https://docs.python.org/3/library/warnings.html), [`warn`](https://docs.python.org/3/library/warnings.html#warnings.warn) |Controle de aviso|
| [**`yaml`**](https://pyyaml.org/wiki/PyYAMLDocumentation) |Analisador e emissor YAML|
| [**`io`**](https://docs.python.org/3/library/io.html), [`BufferedWriter`](https://docs.python.org/3/library/io.html#io.BufferedWriter), [`BytesIO`](https://docs.python.org/3/library/io.html#io.BytesIO) |Ferramentas principais para trabalhar com fluxos|
| [**`subprocess`**](https://docs.python.org/3/library/subprocess.html) |Gerenciamento de subprocessos|
| [**`math`**](https://docs.python.org/3/library/math.html) |Funções matemáticas|
| [**`plt`** (`matplotlib.pyplot`)](https://matplotlib.org/api/pyplot_api.html) |Estrutura de plotagem semelhante ao MATLAB|
| [**`np`** (`numpy`)](https://www.numpy.org/devdocs/reference/index.html) , [`array`](https://www.numpy.org/devdocs/reference/generated/numpy.array.html#numpy.array), [`cos`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.cos.html), [`exp`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.exp.html),<br/> [`log`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.log.html), [`sin`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.sin.html), [`tan`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.tan.html), [`tanh`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.tanh.html) |Matrizes multidimensionais, funções matemáticas|
| [**`pd`** (`pandas`)](http://pandas.pydata.org/pandas-docs/stable/), [`Series`](http://pandas.pydata.org/pandas-docs/stable/reference/series.html), [`DataFrame`](http://pandas.pydata.org/pandas-docs/stable/reference/frame.html) |Estruturas de dados e ferramentas para análise de dados|
| [**`random`**](https://docs.python.org/3/library/random.html) |Gere números pseudo-aleatórios|
| [**`scipy.stats`**](https://docs.scipy.org/doc/scipy/reference/stats.html) |Funções estatísticas|
| [**`scipy.special`**](https://docs.scipy.org/doc/scipy/reference/special.html) |Funções especiais|
| [`abstractmethod`](https://docs.python.org/3/library/abc.html#abc.abstractmethod), [`abstractproperty`](https://docs.python.org/3/library/abc.html#abc.abstractproperty) |Classes base abstratas|
| [**`collections`**](https://docs.python.org/3/library/collections.html), [`Counter`](https://docs.python.org/3/library/collections.html#collections.Counter), [`defaultdict`](https://docs.python.org/3/library/collections.html#collections.defaultdict), <br/>[`namedtuple`](https://docs.python.org/3/library/collections.html#collections.namedtuple), [`OrderedDict`](https://docs.python.org/3/library/collections.html#collections.OrderedDict) |Tipos de dados do contêiner|
| [**`abc`** (`collections.abc`)](https://docs.python.org/3/library/collections.abc.html#module-collections.abc), [`Iterable`](https://docs.python.org/3/library/collections.abc.html#collections.abc.Iterable) |Classes base abstratas para contêineres|
| [**`hashlib`**](https://docs.python.org/3/library/hashlib.html) |Hash seguros e resumos de mensagens|
| [**`itertools`**](https://docs.python.org/3/library/itertools.html) |Funções criando iteradores para loop eficiente|
| [**`json`**](https://docs.python.org/3/library/json.html) |Codificador e decodificador JSON|
| [**`operator`**](https://docs.python.org/3/library/operator.html), [`attrgetter`](https://docs.python.org/3/library/operator.html#operator.attrgetter), [`itemgetter`](https://docs.python.org/3/library/operator.html#operator.itemgetter) |Operadores padrão como funções|
| [**`pathlib`**](https://docs.python.org/3/library/pathlib.html), [`Path`](https://docs.python.org/3/library/pathlib.html#pathlib.Path) |Caminhos do sistema de arquivos orientados a objetos|
| [**`mimetypes`**](https://docs.python.org/3/library/mimetypes.html) |Mapear nomes de arquivos para tipos MIME|
| [**`inspect`**](https://docs.python.org/3/library/inspect.html) |Inspecionar objetos ativos|
| [**`typing`**](https://docs.python.org/3/library/typing.html), [`Any`](https://docs.python.org/3/library/typing.html#typing.Any), [`AnyStr`](https://docs.python.org/3/library/typing.html#typing.AnyStr), [`Callable`](https://docs.python.org/3/library/typing.html#typing.Callable),<br/> [`Collection`](https://docs.python.org/3/library/typing.html#typing.Collection), [`Dict`](https://docs.python.org/3/library/typing.html#typing.Dict), [`Hashable`](https://docs.python.org/3/library/typing.html#typing.Hashable), [`Iterator`](https://docs.python.org/3/library/typing.html#typing.Iterator),<br/> [`List`](https://docs.python.org/3/library/typing.html#typing.List), [`Mapping`](https://docs.python.org/3/library/typing.html#typing.Mapping), [`NewType`](https://docs.python.org/3/library/typing.html#typing.NewType), [`Optional`](https://docs.python.org/3/library/typing.html#typing.Optional),<br/> [`Sequence`](https://docs.python.org/3/library/typing.html#typing.Sequence), [`Tuple`](https://docs.python.org/3/library/typing.html#typing.Tuple), [`TypeVar`](https://docs.python.org/3/library/typing.html#typing.TypeVar), [`Union`](https://docs.python.org/3/library/typing.html#typing.Union) |Suporte para sugestões tipo|
| [**`functools`**](https://docs.python.org/3/library/functools.html), [`partial`](https://docs.python.org/3/library/functools.html#functools.partial), [`reduce`](https://docs.python.org/3/library/functools.html#functools.reduce) |funções de ordem superior e operações em objetos que podem ser chamadas|
| [**`importlib`**](https://docs.python.org/3/library/importlib.html) |A implementação de importação|
| [**`weakref`**](https://docs.python.org/3/library/weakref.html) |referências fracas|
| [**`html`**](https://docs.python.org/3/library/html.html) |apoio HyperText Markup Language|
| [**`re`**](https://docs.python.org/3/library/re.html) |operações de expressões regulares|
| [**`requests`**](http://docs.python-requests.org/en/master/) |HTTP para seres humanos ™|
| [**`tarfile`**](https://docs.python.org/3/library/tarfile.html) |Ler e escrever arquivos arquivo tar|
| [**`numbers`**](https://docs.python.org/3/library/numbers.html), [`Number`](https://docs.python.org/3/library/numbers.html#numbers.Number) |classes base abstratas numéricos|
| [**`tempfile`**](https://docs.python.org/3/library/tempfile.html) |Gerar arquivos e diretórios temporários|
| [**`concurrent`**](https://docs.python.org/3/library/concurrent.html), [`ProcessPoolExecutor`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.ProcessPoolExecutor),<br/> [`ThreadPoolExecutor`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor) |Lançar tarefas paralelas|
| [`copy`](https://docs.python.org/3/library/copy.html#copy.copy), [`deepcopy`](https://docs.python.org/3/library/copy.html#copy.deepcopy) |operação de cópia rasas e profundas|
| [`dataclass`](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass), [`field`](https://docs.python.org/3/library/dataclasses.html#dataclasses.field), `InitVar` |Classes de dados|
| [`Enum`](https://docs.python.org/3/library/enum.html#enum.Enum), [`IntEnum`](https://docs.python.org/3/library/enum.html#enum.IntEnum) |Suporte para enumerações|
| [`set_trace`](https://docs.python.org/3/library/pdb.html#pdb.set_trace) |O depurador Python|
| [**`patches`** (`matplotlib.patches`)](https://matplotlib.org/api/patches_api.html), [`Patch`](https://matplotlib.org/api/_as_gen/matplotlib.patches.Patch.html#matplotlib.patches.Patch) |?|
| [**`patheffects`** (`matplotlib.patheffects`)](https://matplotlib.org/api/patheffects_api.html) |?|
| [`contextmanager`](https://docs.python.org/3/library/contextlib.html#contextlib.contextmanager) |Utilitários para contextos com-statement|
| [`MasterBar`, `master_bar`, `ProgressBar`,<br/> `progress_bar`](https://github.com/fastai/fastprogress) |barra de progresso simples e flexível para Jupyter Notebook e consola|
| [**`pkg_resources`**](https://setuptools.readthedocs.io/en/latest/pkg_resources.html) |descoberta do pacote e acesso a recursos|
| [`SimpleNamespace`](https://docs.python.org/3/library/types.html#types.SimpleNamespace) |criação tipo dinâmico e nomes para tipos built-in|
| [**`torch`**](https://pytorch.org/docs/stable/), [`as_tensor`](https://pytorch.org/docs/stable/torch.html?highlight=as_tensor#torch.as_tensor), [`ByteTensor`](https://pytorch.org/docs/stable/tensors.html#torch.ByteTensor),<br/> [`DoubleTensor`, `FloatTensor`, `HalfTensor`,<br/> `LongTensor`, `ShortTensor`, `Tensor`](https://pytorch.org/docs/stable/tensors.html) |computação Tensor e aprendizagem profunda|
| [**`nn`** (`torch.nn`)](https://pytorch.org/docs/stable/nn.html), [`weight_norm`](https://pytorch.org/docs/stable/nn.html#torch.nn.utils.weight_norm), [`spectral_norm`](https://pytorch.org/docs/stable/nn.html#torch.nn.utils.spectral_norm) |redes neurais com PyTorch|
| [**`F`** (`torch.nn.functional`)](https://pytorch.org/docs/stable/nn.html#torch-nn-functional) |interface funcional PyTorch|
| [**`optim`** (`torch.optim`)](https://pytorch.org/docs/stable/optim.html) |algoritmos de otimização em PyTorch|
| [`BatchSampler`](https://pytorch.org/docs/stable/data.html#torch.utils.data.BatchSampler), [`DataLoader`](https://pytorch.org/docs/stable/data.html#torch.utils.data.DataLoader), [`Dataset`](https://pytorch.org/docs/stable/data.html#torch.utils.data.Dataset),<br/> [`Sampler`](https://pytorch.org/docs/stable/data.html#torch.utils.data.Sampler), [`TensorDataset`](https://pytorch.org/docs/stable/data.html#torch.utils.data.TensorDataset) |PyTorch dados utils|
