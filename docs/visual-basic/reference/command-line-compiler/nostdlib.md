---
title: -nostdlib
ms.date: 03/13/2018
helpviewer_keywords:
- nostdlib compiler option [Visual Basic]
- -nostdlib compiler option [Visual Basic]
- /nostdlib compiler option [Visual Basic]
ms.assetid: 140381b8-dc96-4ad5-ae11-792c9ed0be4d
ms.openlocfilehash: 0934799853323110e73087ba6d8975c30f84d8f7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84387706"
---
# <a name="-nostdlib-visual-basic"></a>-nostdlib (Visual Basic)
Faz com que o compilador não referencie automaticamente as bibliotecas padrão.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-nostdlib  
```  
  
## <a name="remarks"></a>Comentários  
 A `-nostdlib` opção remove a referência automática para o assembly System. dll e impede que o compilador Leia o arquivo Vbc. rsp. O arquivo Vbc. rsp, localizado no mesmo diretório que o arquivo Vbc. exe, faz referência aos assemblies de .NET Framework usados com frequência e importa os `System` `Microsoft.VisualBasic` namespaces e.  
  
> [!NOTE]
> Os assemblies mscorlib. dll e Microsoft. VisualBasic. dll são sempre referenciados.  
  
> [!NOTE]
> A `-nostdlib` opção não está disponível no ambiente de desenvolvimento do Visual Studio; ela está disponível somente durante a compilação na linha de comando.  
  
## <a name="example"></a>Exemplo  
 O código a seguir é compilado `T2.vb` sem referenciar as bibliotecas padrão. Você deve definir a `_MYTYPE` constante de compilação condicional para a cadeia de caracteres "Empty" para remover o `My` objeto.  
  
```console
vbc -nostdlib -define:_MYTYPE=\"Empty\" T2.vb  
```  
  
## <a name="see-also"></a>Confira também

- [-noconfig](noconfig.md)
- [Compilador de linha de comando do Visual Basic](index.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
- [Como personalizar quais objetos estão disponíveis em My](../../developing-apps/customizing-extending-my/customizing-which-objects-are-available-in-my.md)
