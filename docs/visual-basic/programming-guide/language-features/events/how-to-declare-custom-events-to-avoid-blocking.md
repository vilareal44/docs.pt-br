---
title: Como declarar eventos personalizados para evitar bloqueio
ms.date: 07/20/2015
helpviewer_keywords:
- declaring events [Visual Basic], custom
- events [Visual Basic], custom
- custom events [Visual Basic]
ms.assetid: 998b6a90-67c5-4d2c-8b11-366d3e355505
ms.openlocfilehash: a9f9529d468a036d81c4e436429cbdb3207efd6e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84405151"
---
# <a name="how-to-declare-custom-events-to-avoid-blocking-visual-basic"></a>Como declarar eventos personalizados para evitar bloqueio (Visual Basic)
Há várias circunstâncias em que é importante que um manipulador de eventos não bloqueie os manipuladores de eventos subsequentes. Eventos personalizados permitem que o evento chame seus manipuladores de eventos de forma assíncrona.  
  
 Por padrão, o campo de armazenamento de backup para uma declaração de evento é um delegado de multicast que combina em série todos os manipuladores de eventos. Isso significa que, se um manipulador levar muito tempo para ser concluído, ele bloqueará os outros manipuladores até que seja concluído. (Manipuladores de eventos bem comparados nunca devem executar operações demoradas ou potencialmente bloqueadas.)  
  
 Em vez de usar a implementação padrão de eventos que Visual Basic fornece, você pode usar um evento personalizado para executar os manipuladores de eventos de forma assíncrona.  
  
## <a name="example"></a>Exemplo  
 Neste exemplo, o `AddHandler` acessador adiciona o delegado para cada manipulador do `Click` evento a um <xref:System.Collections.ArrayList> armazenado no `EventHandlerList` campo.  
  
 Quando o código gera o `Click` evento, o `RaiseEvent` acessador invoca todos os delegados do manipulador de eventos de forma assíncrona usando o <xref:System.Web.Services.Protocols.LogicalMethodInfo.BeginInvoke%2A> método. Esse método invoca cada manipulador em um thread de trabalho e retorna imediatamente, portanto, os manipuladores não podem bloquear um ao outro.  
  
 [!code-vb[VbVbalrEvents#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#27)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Collections.ArrayList>
- <xref:System.Web.Services.Protocols.LogicalMethodInfo.BeginInvoke%2A>
- [Eventos](index.md)
- [Como declarar eventos personalizados para conservar memória](how-to-declare-custom-events-to-conserve-memory.md)
