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
|csv|Leitura e gravação de arquivos CSV|
|gc|Interface do coletor de lixo|
|gzip|Suporte para arquivos gzip|
|os|Interfaces diversas do sistema operacional|
|pickle|Serialização de objeto Python|
|shutil|Operações de arquivo de alto nível|
|sys|Parâmetros e funções específicos do sistema|
|warnings, warn|Controle de aviso|
|yaml|Analisador e emissor YAML|
|io, BufferedWriter,BytesIO|Ferramentas principais para trabalhar com fluxos|
|subprocess|Gerenciamento de subprocessos|
|math|Funções matemáticas|
|plt( matplotlib.pyplot)|Estrutura de plotagem semelhante ao MATLAB|
|np( numpy) , array, cos, exp,  log, sin, tan,tanh|Matrizes multidimensionais, funções matemáticas|
|pd( pandas) , Series,DataFrame|Estruturas de dados e ferramentas para análise de dados|
|random|Gere números pseudo-aleatórios|
|scipy.stats|Funções estatísticas|
|scipy.special|Funções especiais|
|abstractmethod, abstractproperty|Classes base abstratas|
|collections, Counter, defaultdict, namedtuple,OrderedDict|Tipos de dados do contêiner|
|abc( collections.abc) ,Iterable|Classes base abstratas para contêineres|
|hashlib|Hash seguros e resumos de mensagens|
|itertools|Funções criando iteradores para loop eficiente|
|json|Codificador e decodificador JSON|
|operator, attrgetter,itemgetter|Operadores padrão como funções|
|pathlib, Path|Caminhos do sistema de arquivos orientados a objetos|
|mimetypes|Mapear nomes de arquivos para tipos MIME|
|inspect|Inspecionar objetos ativos|
|typing, Any, AnyStr, Callable, Collection, Dict, Hashable, Iterator, List, Mapping, NewType, Optional, Sequence, Tuple, TypeVar, Union|Support for type hints|
|functools, partial, reduce|Higher-order functions and operations on callable objects|
|importlib|The implementatin of import|
|weakref|Weak references|
|html|HyperText Markup Language support|
|re|Regular expression operations|
|requests|HTTP for Humans™|
|tarfile|Read and write tar archive files|
|numbers, Number|Numeric abstract base classes|
|tempfile|Generate temporary files and directories|
|concurrent, ProcessPoolExecutor, ThreadPoolExecutor|Launch parallel tasks|
|copy, deepcopy|Shallow and deep copy operation|
|dataclass, field, InitVar|Data Classes|
|Enum, IntEnum|Support for enumerations|
|set_trace|The Python debugger|
|patches (matplotlib.patches), Patch|?|
|patheffects (matplotlib.patheffects)|?|
|contextmanager|Utilities for with-statement contexts|
|MasterBar, master_bar, ProgressBar, progress_bar|Simple and flexible progress bar for Jupyter Notebook and console|
|pkg_resources|Package discovery and resource access|
|SimpleNamespace|Dynamic type creation and names for built-in types|
|torch, as_tensor, ByteTensor, DoubleTensor, FloatTensor, HalfTensor, LongTensor, ShortTensor, Tensor|Tensor computation and deep learning|
|nn (torch.nn), weight_norm, spectral_norm|Neural networks with PyTorch|
|F (torch.nn.functional)|PyTorch functional interface|
|optim (torch.optim)|Optimization algorithms in PyTorch|
|BatchSampler, DataLoader, Dataset, Sampler, TensorDataset|PyTorch data utils|
