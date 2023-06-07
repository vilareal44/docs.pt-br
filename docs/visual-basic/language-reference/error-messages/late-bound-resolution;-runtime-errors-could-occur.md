---
title: Resolução de associação tardia; poderiam ocorrer erros de runtime
ms.date: 07/20/2015
f1_keywords:
- vbc42017
- BC42017
helpviewer_keywords:
- BC42017
ms.assetid: 45f552c8-57c6-44c0-97d3-e510119b257a
ms.openlocfilehash: f1dc656a09eee05080356892b280a79505f3b9cd
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397344"
---
# <a name="late-bound-resolution-runtime-errors-could-occur"></a>Resolução de associação tardia; poderiam ocorrer erros de runtime
Um objeto é atribuído a uma variável declarada para ser do [tipo de dados Object](../data-types/object-data-type.md).  
  
 Quando você declara uma variável como `Object` , o compilador deve executar a *associação tardia*, o que causa operações extras em tempo de execução. Ele também expõe seu aplicativo a possíveis erros de tempo de execução. Por exemplo, se você atribuir um <xref:System.Windows.Forms.Form> à `Object` variável e, em seguida, tentar acessar a <xref:System.Xml.XmlDocument.NameTable%2A?displayProperty=nameWithType> propriedade, o tempo de execução lançará um <xref:System.MemberAccessException> porque a <xref:System.Windows.Forms.Form> classe não expõe uma `NameTable` propriedade.  
  
 Se você declarar a variável para ser de um tipo específico, o compilador poderá executar a *ligação antecipada* no momento da compilação. Isso resulta em desempenho aprimorado, acesso controlado aos membros do tipo específico e melhor legibilidade do seu código.  
  
 Por padrão, esta mensagem é um aviso. Para obter informações sobre como ocultar avisos ou tratar avisos como erros, consulte [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **ID do erro:** BC42017  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Se possível, declare a variável para ser de um tipo específico.  
  
## <a name="see-also"></a>Confira também

- [Associação antecipada e tardia](../../programming-guide/language-features/early-late-binding/index.md)
- [Declaração de Variável do Objeto](../../programming-guide/language-features/variables/object-variable-declaration.md)
