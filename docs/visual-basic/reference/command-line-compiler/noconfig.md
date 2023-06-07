---
title: -noconfig
ms.date: 03/13/2018
helpviewer_keywords:
- noconfig compiler option [Visual Basic]
- -noconfig compiler option [Visual Basic]
- /noconfig compiler option [Visual Basic]
ms.assetid: a7405067-bd21-4171-adf4-a126fa3ad6c3
ms.openlocfilehash: ee7cd1b8039a8d9312a8b058cc85c41ca536ed2b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401934"
---
# <a name="-noconfig"></a>-noconfig
Especifica que o compilador não deve referenciar automaticamente os assemblies de .NET Framework usados com frequência ou importar os `System` `Microsoft.VisualBasic` namespaces e.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-noconfig  
```  
  
## <a name="remarks"></a>Comentários  
 A `-noconfig` opção informa o compilador para não compilar com o arquivo Vbc. rsp, que está localizado no mesmo diretório que o arquivo Vbc. exe. O arquivo Vbc. rsp faz referência aos assemblies de .NET Framework usados com frequência e importa os `System` `Microsoft.VisualBasic` namespaces e. O compilador referencia implicitamente o assembly System. dll, a menos que a `-nostdlib` opção seja especificada. A `-nostdlib` opção informa o compilador para não compilar com Vbc. rsp ou referenciar automaticamente o assembly System. dll.  
  
> [!NOTE]
> Os assemblies mscorlib. dll e Microsoft. VisualBasic. dll são sempre referenciados.  
  
 Você pode modificar o arquivo Vbc. rsp para especificar opções adicionais do compilador que devem ser incluídas em cada compilação do Vbc. exe (exceto ao especificar a `-noconfig` opção). Para obter mais informações, confira [@ (Especificar de arquivo de resposta)](specify-response-file.md).  
  
 O compilador processa as opções passadas para o `vbc` comando por último. Portanto, qualquer opção na linha de comando substitui a configuração da mesma opção no arquivo Vbc. rsp.  
  
> [!NOTE]
> A `-noconfig` opção não está disponível no ambiente de desenvolvimento do Visual Studio; ela está disponível somente durante a compilação na linha de comando.  
  
## <a name="see-also"></a>Confira também

- [-nostdlib (Visual Basic)](nostdlib.md)
- [Compilador de linha de comando do Visual Basic](index.md)
- [@ (especificar arquivo de resposta)](specify-response-file.md)
- [-referência (Visual Basic)](reference.md)
