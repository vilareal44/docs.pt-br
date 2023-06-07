---
title: Como declarar eventos personalizados para conservar memória
ms.date: 07/20/2015
helpviewer_keywords:
- declaring events [Visual Basic], custom
- events [Visual Basic], custom
- custom events [Visual Basic]
ms.assetid: 87ebee87-260c-462f-979c-407874debd19
ms.openlocfilehash: c9a049d3f15d5620152f064888a97bd0be5d46b0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84405125"
---
# <a name="how-to-declare-custom-events-to-conserve-memory-visual-basic"></a>Como declarar eventos personalizados para conservar memória (Visual Basic)
Há várias circunstâncias em que é importante que um aplicativo Mantenha seu uso de memória baixo. Os eventos personalizados permitem que o aplicativo use a memória apenas para os eventos que ele manipula.  
  
 Por padrão, quando uma classe declara um evento, o compilador aloca memória para um campo para armazenar as informações do evento. Se uma classe tiver muitos eventos não utilizados, eles ocupam desnecessariamente a memória.  
  
 Em vez de usar a implementação padrão de eventos que Visual Basic fornece, você pode usar eventos personalizados para gerenciar o uso de memória com mais cuidado.  
  
## <a name="example"></a>Exemplo  
 Neste exemplo, a classe usa uma instância da <xref:System.ComponentModel.EventHandlerList> classe, armazenada no `Events` campo, para armazenar informações sobre os eventos em uso. A <xref:System.ComponentModel.EventHandlerList> classe é uma classe de lista otimizada projetada para manter delegados.  
  
 Todos os eventos na classe usam o `Events` campo para controlar quais métodos estão manipulando cada evento.  
  
 [!code-vb[VbVbalrEvents#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#22)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.ComponentModel.EventHandlerList>
- [Eventos](index.md)
- [Como declarar eventos personalizados para evitar bloqueio](how-to-declare-custom-events-to-avoid-blocking.md)
