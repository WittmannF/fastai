---
Título: Dicas de desempenho e Truques
---

Este documento irá mostrar-lhe como acelerar as coisas e obter mais fora de sua GPU / CPU.

## Verifica desempenho automatizado

Para verificar sua configuração para melhorias de desempenho recomendados, execute:
```
python -c "import fastai.utils; fastai.utils.check_perf()"
```

## Treinamento com precisão mixa

A combinação de variáveis floats de 16 e 32 bits pode tremendamente pode melhorar a velocidade de treinamento e usar menos memória RAM e GPU. Para a teoria por trás, confira este [fórum](https://forums.fast.ai/t/mixed-precision-training/20720/3)

Para implementação, confira [estas instruções](/callbacks.fp16.html).



## Processamento de Imagem Faster

Se você notar um gargalo em JPEG decodificação (descompressão) é o suficiente para mudar para um [`libjpeg-turbo`](#libjpeg-turbo) muito mais rápido, usando a versão normal do `Pillow`.

Se você precisar de redimensionamento mais rápida imagem, borrão, composição alfa, premultiplication, divisão por alfa, tons de cinza e outras manipulações de imagem que você precisa para mudar para [`Pillow-SIMD`](#pillow-simd).

No momento em que esta secção só é relevante se você estiver na plataforma x86.

### libjpeg-turbo

Esta é uma mais rápida compressão / descompressão `libjpeg` substituto.

[`libjpeg-turbo`](https://libjpeg-turbo.org/) é um codec de imagem JPEG que utiliza instruções SIMD (MMX, SSE2, AVX2, néon, AltiVec). Em plataformas x86 acelera compressão JPEG de linha de base e descompressão e compressão JPEG progressivo. `Libjpeg-turbo` é geralmente 2-6x tão rápido quanto` libjpeg`, tudo o resto é igual.

Quando você o instala todo o sistema que fornece uma substituição drop-in para o `biblioteca libjpeg`. Alguns pacotes que dependem desta biblioteca serão capazes de começar a usá-lo imediatamente, a maioria terá de ser recompilados contra a biblioteca de substituição.

Aqui é o seu [git-repo](https://github.com/libjpeg-turbo/libjpeg-turbo).

`Fastai` usa` Pillow` por sua processamento de imagem e você tem que reconstruir `Pillow` para aproveitar` libjpeg-turbo`.

Para saber como reconstruir `Pillow-SIMD` ou` Pillow` com `libjpeg-turbo` ver a entrada [`Pillow-SIMD`](#pillow-simd).


### Almofada-SIMD

Há um mais rápido `versão Pillow` lá fora.

#### Fundo

Primeiro, houve PIL (Python Image Library). E então o seu desenvolvimento foi abandonado.

Em seguida, [Pillow](https://github.com/python-pillow/Pillow/) bifurcada PIL como um substituto e de acordo com a sua [benchmarks](https://python-pillow.org/pillow-perf/) é significativamente mais rápido do que `ImageMagick`,` OpenCV`, `IPP` e outras bibliotecas de processamento de imagem rápido (em hardware / plataforma idêntica).

Há relativamente pouco tempo, [Pillow-SIMD](https://github.com/uploadcare/pillow-simd) nasceu para ser um substituto drop-in para descanso. Esta biblioteca por sua vez, é de 4-6 vezes mais rápido do que descanso, de acordo com o mesmo [benchmarks](https://python-pillow.org/pillow-perf/). `Pillow-SIMD` é altamente otimizado para instruções de manipulação de imagem comuns usando Single Instruction, Multiple Data (abordagem [SIMD](https://en.wikipedia.org/wiki/SIMD), onde vários pontos de dados são processados ​​simultaneamente. Este não é o processamento paralelo (pense threads), mas um único processamento instrução, apoiado por CPU , através [data-level parallelism](https://en.wikipedia.org/wiki/Data_parallelism), semelhante a operações de matriz sobre GPU, que também utilizam SIMD.

`Pillow-SIMD` atualmente funciona apenas na plataforma x86. Essa é a principal razão que é um fork do travesseiro e não portadas para `Pillow` - este último está empenhada em apoiar muitas outras plataformas / arquiteturas onde SIMD-support está faltando. A `ciclo libertação Almofada-SIMD` é feita de modo que as suas versões são idênticas Almofada de e a funcionalidade é idêntico, excepto` Almofada-SIMD` acelera a alguns deles (por exemplo redimensionamento).

#### Instalação

Esta seção explica como instalar o `Pillow-SIMD` w /` libjpeg-turbo` (mas o `parte muito complicada libjpeg-turbo` dele é idêntica relevantes para` Pillow` - basta substituir `pillow-simd` com` pillow` no código abaixo).

Aqui está o tl; dr versão para instalar `Pillow-SIMD` w /` libjpeg-turbo` e w / o `apoio TIFF`:

   ```
   conda uninstall -y --force pillow pil jpeg libtiff libjpeg-turbo
   pip   uninstall -y         pillow pil jpeg libtiff libjpeg-turbo
   conda install -yc conda-forge libjpeg-turbo
   CFLAGS="${CFLAGS} -mavx2" pip install --upgrade --no-cache-dir --force-reinstall --no-binary :all: --compile pillow-simd
   conda install -y jpeg libtiff
   ```

Here are the detailed instructions, with an optional `TIFF` support:

1. First remove `pil`, `pillow`, `jpeg` and `libtiff` packages. Also remove 'libjpeg-tubo' if a previous version is installed:

   ```
   conda uninstall -y --force pillow pil jpeg libtiff libjpeg-turbo
   pip   uninstall -y         pillow pil jpeg libtiff libjpeg-turbo
   ```
   Both conda packages `jpeg` and `libjpeg-turbo` contain a `libjpeg.so` library.
   `jpeg`'s `libjpeg.so` library will be replaced later in these instructions with `libjpeg-turbo`'s one for the duration of the build.

   `libtiff` is linked against `libjpeg.so` library from the `jpeg` conda package, and since `Pillow` will try to link against it, we must remove it too for the duration of the build. If this is not done, `import PIL` will fail.

   Note, that the `--force` `conda` option forces removal of a package without removing packages that depend on it. Using this option will usually leave your `conda` environment in a broken and inconsistent state. And `pip` does it anyway. But we are going to fix your environment in the next step. Alternatively, you may choose not to use `--force`, but then it'll uninstall a whole bunch of other packages and you will need to re-install them later. It's your call.

2. Now we are ready to replace `libjpeg` with a drop-in replacement of `libjpeg-turbo` and then replace `Pillow` with `Pillow-SIMD`:

   ```
   conda install -yc conda-forge libjpeg-turbo
   CFLAGS="${CFLAGS} -mavx2" pip install --upgrade --no-cache-dir --force-reinstall --no-binary :all: --compile pillow-simd
   ```
   Do note that since you're building from source, you may end up not having some of the features that come with the binary `Pillow` package if the corresponding libraries aren't available on your system during the build time. For more information see: [Building from source](https://pillow.readthedocs.io/en/latest/installation.html#building-from-source).

   If you add `-v` to the `pip install` command, you will be able to see all the details of the build, and one useful part of the output is its report of what was enabled and what not, in `PIL SETUP SUMMARY`:

   ```
    --- JPEG support available
    *** OPENJPEG (JPEG2000) support not available
    --- ZLIB (PNG/ZIP) support available
    *** LIBIMAGEQUANT support not available
    *** LIBTIFF support not available
    --- FREETYPE2 support available
    --- LITTLECMS2 support available
    *** WEBP support not available
    *** WEBPMUX support not available
   ```

3. Another nuance is `libtiff` which we removed, - and that means that `Pillow` was built without `LIBTIFF` support and will not be able to read TIFF files.

   You can safely skip this step if you don't care for TIFF files.

   This can be fixed by installing a `libtiff` library, linked against `libjpeg-turbo`.
   ```
   conda install -y -c zegami libtiff-libjpeg-turbo
   ```
   and then rebuilding `Pillow` as explained in the stage above. Only linux version of the `libtiff-libjpeg-turbo` package is available at the moment.

   XXX: The `libtiff-libjpeg-turbo` package could be outdated - it's currently only available on someone's personal channel. Alternatively, it'll need to be built from scratch. Pypi's `libtiff` package doesn't help - it doesn't place `libtiff.so` under conda environment's `lib` directory.

   The other option is to install system-wide `libjpeg-turbo` and `libtiff` linked against the former.

4. Assuming that `libjpeg-turbo` and `jpeg`'s `libjpeg.so.X.X.X` don't collide you can now reinstall back the `jpeg` package - other programs most likely need it. And `libtiff` too:

   ```
   conda install -y jpeg libtiff
   ```

5. Since this is a forked drop-in replacement, however, the package managers don't know they have `Pillow`-replacement package installed, so if any update happens that triggers an update of `Pillow`, `conda`/`pip` will overwrite `Pillow-SIMD` reverting to the less speedy `Pillow` solution. So it's worthwhile checking your run-time setup that you're indeed using `Pillow-SIMD` in your code.

   That means that every time you update the `fastai` conda package you will have to rebuild `Pillow-SIMD`.

#### How to check whether you're running `Pillow` or `Pillow-SIMD`?

```
python -c "do PIL importação de imagem; print (Image.PILLOW_VERSION)"
3.2.0.post3
```
According to the author, if `PILLOW_VERSION` has a postfix, it is `Pillow-SIMD`. (Assuming that `Pillow` will never make a `.postX` release).

#### Is JPEG compression SIMD-optimized?

`libjpeg-turbo` replacement for `libjpeg` is SIMD-optimized. In order to get `Pillow` or its faster fork `Pillow-SIMD` to use `libjpeg-turbo`, the latter needs to be already installed during the former's compilation time. Once `Pillow` is compiled/installed, it no longer matters which `libjpeg` version is installed in your virtual environment or system-wide, as long as the same `libjpeg` library remains at the same location as it was during the compilation time (it's dynamically linked).

However, if at a later time something triggers a conda or pip update on `Pillow` it will fetch a pre-compiled version which most likely is not built against `libjpeg-turbo` and replace your custom built `Pillow` or `Pillow-SIMD`.

Here is how you can see that the `PIL` library is dynamically linked to `libjpeg.so`:

```
cd ~ / anaconda3 / envs / fastai / lib / python3.6 / site-packages / PIL /
ldd _imaging.cpython-36m-x86_64-linux-gnu.so | grep libjpeg
        libjpeg.so.8 => ~ / anaconda3 / envs / fastai / lib / libjpeg.so.8
```

and `~/anaconda3/envs/fastai/lib/libjpeg.so.8` was installed by `conda install -c conda-forge libjpeg-turbo`. We know that from:

```
cd ~ / anaconda3 / envs / fastai / Conda-meta /
grep libjpeg.so libjpeg-turbo-2.0.1-h470a237_0.json
```

If I now install the normal `libjpeg` and do the same check on the `jpeg`'s package info:

```
Conda instalar jpeg
cd ~ / anaconda3 / envs / fastai / Conda-meta /
grep libjpeg.so jpeg-9b-h024ee3a_2.json

```
I find that it's `lib/libjpeg.so.9.2.0` (`~/anaconda3/envs/fastai/lib/libjpeg.so.9.2.0`).

Also, if `libjpeg-turbo` and `libjpeg` happen to have the same version number, even if you built `Pillow` or `Pillow-SIMD` against `libjpeg-turbo`, but then later replaced it with the default `jpeg` with exactly the same version you will end up with the slower version, since the linking happens at build time. But so far that risk appears to be small, as of this writing, `libjpeg-turbo` releases are in the 8.x versions, whereas `jpeg`'s are in 9.x's.

#### How to tell whether `Pillow` or `Pillow-SIMD` is using `libjpeg-turbo`?

You need `Pillow>=5.4.0` to accomplish the following (install from github until then:
`pip install git+https://github.com/python-pillow/Pillow`).

```
python -c "de recursos PIL importação; print (features.check_feature ( 'libjpeg_turbo'))"
Verdadeiro
```

And a version-proof check:

```
de recursos de importação PIL, Imagem
a partir da versão importação de embalagens

se version.parse (Image.PILLOW_VERSION)> = version.parse ( "5.4.0"):
    se features.check_feature ( 'libjpeg_turbo'):
        print ( "libjpeg-turbo é sobre")
    outro:
        print ( "libjpeg-turbo não é sobre")
outro:
    print (f "estado libjpeg-turbo' não pode ser derivada - necessidade Pillow (-SIMD)> = 5.4.0 para contar, versão atual {Image.PILLOW_VERSION}?")
```

### Conda packages

The `fastai` conda (test) channel has an experimental `pillow` package built against a custom build of `libjpeg-turbo`. There are python 3.6 and 3.7 linux builds:

To install:
```
Conda desinstalar -y --force travesseiro libjpeg-turbo
Conda instalar / label travesseiro -c fastai / test
```

There is also an experimental `pillow-simd-5.3.0.post0` conda package built against `libjpeg-turbo` and compiled with `avx2`. Try it only for python 3.6 on linux.
```
Conda desinstalar -y --force travesseiro libjpeg-turbo
Conda instalar -c fastai / label / test pillow-SIMD
`` `

Ele provavelmente não vai trabalhar em sua configuração a menos que seu CPU tem a mesma capacidade como aquele que foi construído em (Intel). Então, se ele não funcionar, instale `pillow-simd` de [Building from source](https://pillow.readthedocs.io/en/latest/installation.html#building-from-source) vez.

Note que `pillow-simd` vai ser substituído por` pillow` através de atualização / instalação de qualquer outro pacote dependendo `pillow`. Você pode enganar `pillow-simd` em acreditar que é` pillow` e então não vai ter dizimado. Você terá que [source](https://github.com/uploadcare/pillow-simd#installation).

Se você tiver problemas com estes pacotes experimentais por favor poste [make a local build for that](https://github.com/fastai/fastai/blob/master/builds/custom-conda-builds/pillow-simd/conda-build.txt), incluindo a saída do `python -m fastai.utils.check_perf` e` python -m fastai.utils.show_install` e as exatas problemas / erros encontrados.



## Desempenho GPU

Veja [here](https://forums.fast.ai/t/performance-improvement-through-faster-software-components/32628/1).
