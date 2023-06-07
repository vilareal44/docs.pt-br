---
title: -recurse
ms.date: 03/13/2018
helpviewer_keywords:
- /recurse compiler option [Visual Basic]
- -recurse compiler option [Visual Basic]
- recurse compiler option [Visual Basic]
ms.assetid: 84a0b670-33ae-44c4-a46a-b90388809317
ms.openlocfilehash: fc8dfe41ea56531ff34cd5e551ef24d636227e47
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400494"
---
# <a name="-recurse"></a>-recurse
Compila os arquivos de código-fonte em todos os diretórios filho do diretório especificado ou do diretório do projeto.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-recurse:[dir\]file  
```  
  
## <a name="arguments"></a>Argumentos  
 `dir`  
 Opcional. O diretório no qual você deseja que a pesquisa comece. Se não for especificado, a pesquisa começará no diretório do projeto.  
  
 `file`  
 Obrigatórios. Os arquivos a serem pesquisados. São permitidos caracteres curinga.  
  
## <a name="remarks"></a>Comentários  
 Você pode usar caracteres curinga em um nome de arquivo para compilar todos os arquivos correspondentes no diretório do projeto sem usar o `-recurse` . Se nenhum nome de arquivo de saída for especificado, o compilador baseará o nome do arquivo de saída no primeiro arquivo de entrada processado. Esse é geralmente o primeiro arquivo na lista de arquivos compilados quando exibidos em ordem alfabética. Por esse motivo, é melhor especificar um arquivo de saída usando a `-out` opção.  
  
> [!NOTE]
> A `-recurse` opção não está disponível no ambiente de desenvolvimento do Visual Studio; ela está disponível somente durante a compilação na linha de comando.  
  
## <a name="example"></a>Exemplo  
 O comando a seguir compila todos os arquivos de Visual Basic no diretório atual.  
  
```console
vbc *.vb  
```  
  
 O comando a seguir compila todos os arquivos de Visual Basic no `Test\ABC` diretório e em todos os diretórios abaixo dele e, em seguida, gera `Test.ABC.dll` .  
  
```console
vbc -target:library -out:Test.ABC.dll -recurse:Test\ABC\*.vb  
```  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [-out (Visual Basic)](out.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
