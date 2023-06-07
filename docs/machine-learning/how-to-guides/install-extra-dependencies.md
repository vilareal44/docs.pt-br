---
title: Instalar dependências de ML.NET extras
description: Saiba como instalar todas as bibliotecas nativas das quais os pacotes ML.NET dependem, mas não são instalados com os pacotes NuGet
ms.date: 04/02/2020
author: natke
ms.author: nakersha
ms.custom: how-to
ms.openlocfilehash: c744b42b4b95681de7b0cbeaef338cc890708fd8
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008424"
---
# <a name="install-extra-mlnet-dependencies"></a>Instalar dependências de ML.NET extras

Na maioria dos casos, em todos os sistemas operacionais, a instalação do ML.NET é tão simples quanto fazer referência ao pacote NuGet apropriado.

```dotnetcli
dotnet add package Microsoft.ML
```

Em alguns casos, no entanto, há requisitos de instalação adicionais, especialmente quando os componentes nativos são necessários. Este documento descreve os requisitos de instalação para esses casos. As seções são divididas pelo `Microsoft.ML.*` pacote NuGet específico que tem a dependência adicional.

## <a name="microsoftmltimeseries-microsoftmlautoml"></a>Microsoft. ML. timeseries, Microsoft. ML. AutoML

Ambos os pacotes têm uma dependência em `Microsoft.ML.MKL.Redist` , que tem uma dependência `libiomp` .

### <a name="windows"></a>Windows

Nenhuma etapa de instalação extra é necessária. A biblioteca é instalada quando o pacote NuGet é adicionado ao projeto.

### <a name="linux"></a>Linux

1. Instalar a chave GPG para o repositório

    ```bash
    sudo bash
    # <type your user password when prompted.  this will put you in a root shell>
    # cd to /tmp where this shell has write permission
    cd /tmp
    # now get the key:
    wget https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB
    # now install that key
    apt-key add GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB
    # now remove the public key file exit the root shell
    rm GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB
    exit
    ```

2. Adicionar o repositório APT para MKL

    ```bash
    sudo sh -c 'echo deb https://apt.repos.intel.com/mkl all main > /etc/apt/sources.list.d/intel-mkl.list'
    ```

3. Atualizar pacotes

    ```bash
    sudo apt-get update
    ```

4. Instalar o MKL

    ```bash
    sudo apt-get install <COMPONENT>-<VERSION>.<UPDATE>-<BUILD_NUMBER>
    ```

    Por exemplo:

    ```bash
    sudo apt-get install intel-mkl-64bit-2020.0-088
    ```

    Determinar o local de`libiomp.so`

    ```bash
    find /opt -name "libiomp5.so"
    ```

    Por exemplo:

    ```output
    /opt/intel/compilers_and_libraries_2020.0.166/linux/compiler/lib/intel64_lin/libiomp5.so
    ```

5. Adicione este local ao caminho da biblioteca de carregamento:

    ```bash
    sudo ldconfig /opt/intel/compilers_and_libraries_2020.0.166/linux/compiler/lib/intel64_lin
    ```

### <a name="mac"></a>Mac

1. Instalar a biblioteca com`Homebrew`

    ```bash
    brew update && brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/f5b1ac99a7fba27c19cee0bc4f036775c889b359/Formula/libomp.rb && brew link libomp --force
    ```
