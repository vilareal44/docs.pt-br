---
title: '#Diretiva Region'
ms.date: 07/20/2015
f1_keywords:
- vb.Region
- vb.#Region
helpviewer_keywords:
- Visual Basic compiler, compiler directives
- '#region directive'
- region directive (#region)
- '#Region keyword [Visual Basic]'
ms.assetid: 90a6a104-3cbf-47d0-bdc4-b585d0921b87
ms.openlocfilehash: cd53a6079c1564a8c73a0a1a6273fc166d18d3e6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409928"
---
# <a name="region-directive"></a>Diretiva #Region

Recolhe e oculta seções de código em Visual Basic arquivos.  
  
## <a name="syntax"></a>Sintaxe  

```vb
#Region "identifier_string"  
#End Region  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`identifier_string`|Obrigatórios. Cadeia de caracteres que atua como o título de uma região quando ela é recolhida. As regiões são recolhidas por padrão.|  
|`#End Region`|Encerra o `#Region` bloco.|  
  
## <a name="remarks"></a>Comentários  

 Use a `#Region` diretiva para especificar um bloco de código para expandir ou recolher ao usar o recurso de estrutura de tópicos do editor de Visual Studio Code. Você pode posicionar ou *aninhar*regiões em outras regiões para agrupar regiões semelhantes.  
  
## <a name="example"></a>Exemplo  

 Este exemplo usa a `#Region` diretiva.  
  
 [!code-vb[VbVbalrConditionalComp#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConditionalComp/VB/Class1.vb#4)]  
  
## <a name="see-also"></a>Confira também

- [#If... Diretivas then... #Else](if-then-else-directives.md)
- [Estrutura de tópicos](/visualstudio/ide/outlining)
- [Como: Recolher e ocultar seções do código](../../programming-guide/program-structure/how-to-collapse-and-hide-sections-of-code.md)
