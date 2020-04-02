# Compilação da API C++ do TensorFlow Lite para inferencia
Este arquivo descreve os passos para fazer a compilação da API C++ do Tensorflow Lite para  inferência com NDK para Android.

### Objetivo
Existem alguns poucos tutoriais que descrevem os passos da compilação da API C++ do TF Lite. No entanto, seguir apenas um único desses tutoriais pode não corresponder as suas necessidades e versões, você pode passar muito tempo tentando fazer isso. Pensando nessas possibilidades, criamos este passo-a-passo que nos guiará na criação dos arquivos necessário para inferência com a API C++ do TFLite. Com isso teremos mais um tutorial que poderá ajudar você na busca por informações desse seguimento.

#### Nota
Nesse tutorial assumimos que você já possui um projeto Android e também um modelo do arquivo do Tensorflow Lite ```.tflite```.
Se você precisar de um projeto Android de demonstração veja este site [Exemplos para Android com Tensorflow](https://github.com/tensorflow/examples/blob/master/lite/examples/image_classification/android/README.md). Se você ainda não converteu seu modelo do Keras ou Tensorflow para o Tensorflow Lite veja este site para a versão 2.x [Convertsor TF Lite 2.x](https://www.tensorflow.org/lite/convert) ou este para versões do TF 1.x [Conversor TF Lite 1.x](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/lite/g3doc/r1/convert/python_api.md).

### Requisitos
Para seguir com a compilação corretamente você precisará dos arquivos e versões a seguir:
#### windows
* Python 3.6
* Git bash ou MSYS64
* Bazel 2.0.0 - Siga os passos no site [aqui](https://docs.bazel.build/versions/master/install-windows.html).
* Visual C++ Redistributable for Visual Studio 2015 [aqui](https://www.microsoft.com/en-us/download/details.aspx?id=48145)
* Tensorflow 1.14.0

### Instalação
Instale o ```Bazel``` seguindo as instruções do site oficial [aqui](https://docs.bazel.build/versions/master/install-windows.html). Nesse site também é explicado a instalçao e configuração do ```MSYS64```.

Clone o repositório oficial do Tensorflow v1.14.0 [obtido aqui](https://github.com/tensorflow/tensorflow/releases/tag/v1.14.0). Se você clonar sem especificar a versão, será feito o clone da vesão mais recente na ```brach master``` do tensorflow 2.x. Logo, especifique a brach da versão 1.14.0 dando ```checkout``` como no exemplo(``` git checkout branch_name # r1.9, r1.10, etc.```) ou siga direto por [aqui](https://github.com/tensorflow/tensorflow/releases/tag/v1.14.0).

Acesse o diretório root do tensorflow e edite o arquivo ```WORKSPACE``` adicionando no final do arquivo as linhas do script abaixo. Altere os ```path``` do SDK e NDK de acordo com a sua arvore de diretórios. Observe que a ```api_level``` está configurada para ```api_level=18```. 
```
android_sdk_repository(
   name = "androidsdk",
   api_level = 28,
   build_tools_version = "28.0.3",
   path = "xxxx/Android/sdk",
)

android_ndk_repository(
   name="androidndk",
   path="xxx/ndk/18.1.5063045",
   api_level=18
)
```
Agora faça a configuração do arquivo ```configure``` para que o bazel siga as instruções. Em um terminal ```bash.exe``` (Git bash, MSYS, MingGW) execulte ```python configure.py``` ou ```python3 configure.py```. Em seguida será solicitado algumas informação de confimação, faça como no exemplo abaixo:

* Com suporte para GPU NVIDIA
```
Please specify the location of python. [Default is X:\ProgramFiles\Python3\python.exe]:

Found possible Python library paths:
  X:\ProgramFiles\Python3\lib\site-packages
Please input the desired Python library path to use.  Default is [X:\ProgramFiles\Python3\lib\site-packages]

Do you wish to build TensorFlow with XLA JIT support? [y/N]: y
XLA JIT support will be enabled for TensorFlow.

Do you wish to build TensorFlow with ROCm support? [y/N]: N
No ROCm support will be enabled for TensorFlow.

Do you wish to build TensorFlow with CUDA support? [y/N]: Y
CUDA support will be enabled for TensorFlow.

...TensorFlow only supports compute capabilities >= 3.5 [Default is: 3.5,7.0]: 7.0

specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is /arch:AVX]:
```
* Para suporte apenas com CPU selecione 
```Do you wish to build TensorFlow with CUDA support? N```
