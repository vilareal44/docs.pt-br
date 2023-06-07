---
title: Diretivas
ms.date: 07/20/2015
helpviewer_keywords:
- directives, Visual Basic compiler
- Visual Basic code, directives
- directives
ms.assetid: 20d5fe65-490a-4c23-88c2-ee4f490ed762
ms.openlocfilehash: b5fcf3cb8801bc99dd2096c28cc41ebefeb34592
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409992"
---
# <a name="directives-visual-basic"></a>Diretivas (Visual Basic)

Os tópicos nesta seção documentam as diretivas Visual Basic do compilador do código-fonte.  
  
## <a name="in-this-section"></a>Nesta seção  

 [Diretiva #Const](const-directive.md) – definir uma constante do compilador  
  
 [Diretiva #ExternalSource](externalsource-directive.md) --indica um mapeamento entre linhas de origem e texto externo à origem  
  
 [#If... Then... #Else diretivas](if-then-else-directives.md) – compilar blocos de código selecionados  
  
 [Diretiva #Region](region-directive.md) – recolher e ocultar seções de código no editor do Visual Studio  
  
 **#Disable, #Enable** --desabilitar e habilitar avisos específicos para regiões de código.  
  
```vb  
#Disable Warning BC42356 ' suppress warning about no awaits in this method  
    Async Function TestAsync() As Task  
        Console.WriteLine("testing")  
    End Function  
#Enable Warning BC42356  
```  
  
 Você também pode desabilitar e habilitar uma lista separada por vírgulas de códigos de aviso.  
  
## <a name="related-sections"></a>Seções relacionadas  

 [Referência de linguagem de Visual Basic](../index.md)  
  