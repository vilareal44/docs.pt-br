---
title: '@ (especificar arquivo de resposta)'
ms.date: 03/13/2018
helpviewer_keywords:
- '@ (Specify Response File) compiler option [Visual Basic]'
ms.assetid: a6847eaa-e5f9-4303-9421-45b55484b9ca
ms.openlocfilehash: 91cf1b5a55d16ab47a83fbd259dd1d83d8e9c31a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403090"
---
# <a name="-specify-response-file-visual-basic"></a>@ (especificar arquivo de resposta) (Visual Basic)

Especifica um arquivo que contém opções de compilador e arquivos de código-fonte a serem compilados.

## <a name="syntax"></a>Sintaxe

```console
@response_file
```

## <a name="arguments"></a>Argumentos

`response_file`  
Obrigatórios. Um arquivo que lista opções de compilador ou arquivos de código-fonte para compilar. Coloque o nome de arquivo entre aspas ("") se ele contiver um espaço.

## <a name="remarks"></a>Comentários

O compilador processa as opções do compilador e os arquivos de código-fonte especificados em um arquivo de resposta como se eles tivessem sido especificados na linha de comando.

Para especificar mais de um arquivo de resposta em uma compilação, especifique várias opções de arquivo de resposta, como a seguir.

```console
@file1.rsp @file2.rsp
```

Em um arquivo de resposta, várias opções de compilador e arquivos de código-fonte podem aparecer em uma linha. Uma especificação de opção de compilador único deve aparecer em uma linha (não pode abranger várias linhas). Os arquivos de resposta podem ter comentários que começam com o `#` símbolo.

Você pode combinar as opções especificadas na linha de comando com opções especificadas em um ou mais arquivos de resposta. O compilador processa as opções de comando à medida que as encontra. Portanto, argumentos de linha de comando podem substituir as opções listadas anteriormente em arquivos de resposta. Por outro lado, as opções em um arquivo de resposta substituem as opções listadas anteriormente na linha de comando ou em outros arquivos de resposta.

Visual Basic fornece o arquivo Vbc. rsp, localizado no mesmo diretório que o arquivo Vbc. exe. O arquivo Vbc. rsp é incluído por padrão, a menos que a `-noconfig` opção seja usada. Para obter mais informações, consulte [-noconfig](noconfig.md).

> [!NOTE]
> A `@` opção não está disponível no ambiente de desenvolvimento do Visual Studio; ela está disponível somente durante a compilação na linha de comando.

## <a name="example"></a>Exemplo

As linhas a seguir são de um exemplo de arquivo de resposta.

```console
# build the first output file
-target:exe
-out:MyExe.exe
source1.vb
source2.vb
```

## <a name="example"></a>Exemplo

O exemplo a seguir demonstra como usar a `@` opção com o arquivo de resposta chamado `File1.rsp` .

```console
vbc @file1.rsp
```

## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [-noconfig](noconfig.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
