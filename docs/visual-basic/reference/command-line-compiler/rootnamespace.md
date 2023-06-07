---
title: -rootnamespace
ms.date: 03/13/2018
f1_keywords:
- /rootnamespace
- rootnamespace
helpviewer_keywords:
- /rootnamespace compiler option [Visual Basic]
- -rootnamespace compiler option [Visual Basic]
- rootnamespace compiler option [Visual Basic]
ms.assetid: e9245edf-6bef-420d-a7c7-324117752783
ms.openlocfilehash: b6a2f3ba0772d8f8c8c6a762a1be01703d21b778
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403129"
---
# <a name="-rootnamespace"></a>-rootnamespace
Especifica um namespace para todas as declarações de tipo.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-rootnamespace:namespace  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`namespace`|O nome do namespace no qual incluir todas as declarações de tipo para o projeto atual.|  
  
## <a name="remarks"></a>Comentários  
 Se você usar o arquivo executável do Visual Studio (devenv. exe) para compilar um projeto criado no ambiente de desenvolvimento integrado do Visual Studio, use `-rootnamespace` para especificar o valor da <xref:VSLangProj80.VBProjectProperties3.RootNamespace%2A> propriedade. Consulte [Opções de linha de comando do devenv](/visualstudio/ide/reference/devenv-command-line-switches) para obter mais informações.  
  
 Use o Common Language Runtime o desmontador MSIL ( `Ildasm.exe` ) para exibir os nomes de namespace em seu arquivo de saída.  
  
|Para Set-RootNamespace no ambiente de desenvolvimento integrado do Visual Studio|  
|---|  
|1. ter um projeto selecionado em **Gerenciador de soluções**. No menu **Projeto** , clique em **Propriedades**. <br />2. Clique na guia **aplicativo** .<br />3. modifique o valor na caixa **namespace raiz** .|  
  
## <a name="example"></a>Exemplo  
 O código a seguir compila `In.vb` e inclui todas as declarações de tipo no namespace `mynamespace` .  
  
```console
vbc -rootnamespace:mynamespace in.vb  
```  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [ILDASM. exe (desmontador de IL)](../../../framework/tools/ildasm-exe-il-disassembler.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
