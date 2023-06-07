---
title: Evento ETW de pilha
description: Leia sobre o evento ETW de pilha, que deve ser usado em conjunto com outros eventos para gerar rastreamentos de pilha depois que um evento é gerado.
ms.date: 03/30/2017
helpviewer_keywords:
- stack event [.NET Framework]
- ETW, stack event (CLR)
ms.assetid: f612fa5b-4b62-4593-a19e-85c9b1018dce
ms.openlocfilehash: cab496615c4ef17831895b72c8987917e3c06e77
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474131"
---
# <a name="stack-etw-event"></a>Evento ETW de pilha
O evento de pilha deve ser usado em conjunto com outros eventos para gerar rastreamentos de pilha depois que um evento é acionado. Ele é registrado quando o provedor de runtime está habilitado. Esse é um evento de alta frequência porque é acionado sempre que outro evento de runtime é acionado. Por esse motivo, recomendamos ter cautela ao usar esse evento.  
  
 A tabela a seguir mostra a palavra-chave e o nível. (Para obter mais informações, consulte [Palavras-chaves e níveis CLR ETW](clr-etw-keywords-and-levels.md).)  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`StackKeyword` (0x40000000)|LogAlways(0)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Acionado quando|  
|-----------|--------------|-----------------|  
|`CLRStackWalk`|82|Em conjunto com outros eventos para gerar rastreamentos de pilha após um evento.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de Dados|Descrição|  
|----------------|---------------|-----------------|  
|ClrInstanceID|win:Uint16|Identificador exclusivo do runtime.|  
|Reserved1|win:UInt8|Reservado.|  
|Reserved2|win:UInt8|Reservado.|  
|FrameCount|win:UInt32|O número de quadros no rastreamento de pilha.|  
|Pilha|win:Pointer|Colunas de ponteiros de instrução.|  
  
## <a name="see-also"></a>Consulte também

- [Eventos ETW no CLR](clr-etw-events.md)
