---
title: -moduleassemblyname
ms.date: 03/13/2018
helpviewer_keywords:
- moduleassemblyname compiler option [Visual Basic]
- /moduleassemblyname compiler option [Visual Basic]
- -moduleassemblyname compiler option [Visual Basic]
ms.assetid: 013a57b6-f425-4dd3-b333-512d72c42f55
ms.openlocfilehash: 99f2b9d65f3c2a128e026666c5efb384e22643f9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403142"
---
# <a name="-moduleassemblyname"></a>-moduleassemblyname
Especifica o nome do assembly do qual esse módulo fará parte.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-moduleassemblyname:assembly_name  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`assembly_name`|O nome do assembly do qual este módulo fará parte.|  
  
## <a name="remarks"></a>Comentários  
 O compilador processará a `-moduleassemblyname` opção somente se a `-target:module` opção tiver sido especificada. Isso faz com que o compilador crie um módulo. O módulo criado pelo compilador é válido somente para o assembly especificado com a `-moduleassemblyname` opção. Se você posicionar o módulo em um assembly diferente, ocorrerão erros de tempo de execução.  
  
 A `-moduleassemblyname` opção é necessária somente quando as seguintes opções são verdadeiras:  
  
- Um tipo de dados no módulo precisa de acesso a um `Friend` tipo em um assembly referenciado.  
  
- O assembly referenciado concedeu acesso de assembly Friend ao assembly no qual o módulo será compilado.  
  
 Para obter mais informações sobre como criar um módulo, consulte [-Target (Visual Basic)](target.md). Para obter mais informações sobre assemblies Friend, consulte [Friend Assemblies](../../../standard/assembly/friend.md).  
  
> [!NOTE]
> A `-moduleassemblyname` opção não está disponível no ambiente de desenvolvimento do Visual Studio; ela está disponível somente quando você compila a partir de um prompt de comando.  
  
## <a name="see-also"></a>Confira também

- [Como compilar um assembly de multiarquivos](../../../framework/app-domains/build-multifile-assembly.md)
- [Compilador de linha de comando do Visual Basic](index.md)
- [-Target (Visual Basic)](target.md)
- [-main](main.md)
- [-referência (Visual Basic)](reference.md)
- [-addmodule](addmodule.md)
- [Assemblies no .NET](../../../standard/assembly/index.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
- [Assemblies Friend](../../../standard/assembly/friend.md)
