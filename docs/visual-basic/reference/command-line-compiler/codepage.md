---
title: -codepage
ms.date: 03/09/2018
helpviewer_keywords:
- -codepage compiler option [Visual Basic]
- codepage compiler option [Visual Basic]
- -codepage compiler option [Visual Basic]
ms.assetid: be36ec33-6800-4505-838c-4124564f5cc9
ms.openlocfilehash: 34dbf36cc79a8c4715cf6a07c57d559e14f40030
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363624"
---
# <a name="-codepage-visual-basic"></a>-CodePage (Visual Basic)
Especifica a página de código a ser usada para todos os arquivos de código-fonte na compilação.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-codepage:id  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`id`|Obrigatórios. O compilador usa a página de código especificada pelo `id` para interpretar a codificação dos arquivos de origem.|  
  
## <a name="remarks"></a>Comentários  
 Para compilar o código-fonte salvo com uma codificação específica, você pode usar `-codepage` para especificar qual página de código deve ser usada. A `-codepage` opção se aplica a todos os arquivos de código-fonte em sua compilação. Para obter mais informações, consulte [codificação de caracteres no .NET Framework](../../../standard/base-types/character-encoding.md).  
  
 A `-codepage` opção não será necessária se os arquivos de código-fonte tiverem sido salvos usando a página de código ANSI atual, o Unicode ou o UTF-8 com uma assinatura. O Visual Studio salva todos os arquivos de código-fonte com a página de código ANSI atual por padrão, a menos que o usuário especifique outra codificação na caixa de diálogo **codificação** . O Visual Studio usa a caixa de diálogo **codificação** para abrir arquivos de código-fonte salvos com uma página de código diferente.  
  
> [!NOTE]
> A `-codepage` opção não está disponível no ambiente de desenvolvimento do Visual Studio; ela está disponível somente durante a compilação na linha de comando.  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
