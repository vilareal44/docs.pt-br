---
title: Eventos ETW de rastreamento JIT
description: Entenda os eventos ETW de rastreamento JIT (just-in-time). Esses eventos coletam informações relacionadas ao êxito ou à falha de chamadas de laço JIT e JIT.
ms.date: 03/30/2017
helpviewer_keywords:
- JIT tracing events [.NET Framework]
- ETW, JIT tracing events (CLR)
ms.assetid: 926adde2-c123-452e-bf4f-4b977bf06ffb
ms.openlocfilehash: 568fc942cd0e2188c530d2befb6260083757ec72
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474456"
---
# <a name="jit-tracing-etw-events"></a>Eventos ETW de rastreamento JIT
 Esses eventos coletam informações sobre o êxito ou falha de inlining JIT (Just-In-Time) e chamadas tail JIT.

## <a name="jit-inlining-events"></a>Eventos de inlining JIT

### <a name="methodjitinliningfailed-event"></a>Evento MethodJitInliningFailed
 A tabela a seguir mostra a palavra-chave e o nível. (Para obter mais informações, consulte [Palavras-chaves e níveis CLR ETW](clr-etw-keywords-and-levels.md).)  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|Detalhado (5)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Acionado quando|  
|-----------|--------------|-----------------|  
|`MethodJitInliningFailed`|186|O inlining JIT falhou.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|Namespace do método que está sendo compilado.|  
|MethodBeingCompiledName|win:UnicodeString|Nome do método que está sendo compilado.|  
|MethodBeingCompiledNameSignature|win:UnicodeString|Assinatura do método que está sendo compilado.|  
|InlinerNamespace|win:UnicodeString|O namespace do método para o qual o compilador JIT está tentando gerar código.|  
|InlinerName|win:UnicodeString|O nome do método para o qual o compilador está tentando gerar código. Isso pode não ser o mesmo que `MethodBeingCompiledName` se o compilador está tentando embutir código em `MethodBeingCompiledName` em vez de gerar uma chamada para `InlinerName`.|  
|InlinerNameSignature|win:UnicodeString|A assinatura do item que realiza o inlining.|  
|InlineeNamespace|win:UnicodeString|O namespace do item embutido.|  
|InlineeName|win:UnicodeString|O método que o compilador está tentando embutir (e não para o qual está tentando gerar uma chamada).|  
|InlineeNameSignature|win:UnicodeString|A assinatura do item embutido.|  
|FailAlways|win:Boolean|Uma dica para o compilador JIT de que o inlining sempre falhará para o item embutido.|  
|FailReason|win:UnicodeString|INLINE_NEVER significa que uma tentativa anterior de inlining determinou que o inlining nunca obterá sucesso por algum outro motivo; caso contrário, texto de forma livre.|  
|ClrInstanceID|win:UnicodeString|ID exclusiva da instância do CLR ou do CoreCLR.|  
  
### <a name="methodjitinliningsucceeded-event"></a>Evento MethodJitInliningSucceeded  
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|Detalhado (5)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Acionado quando|  
|-----------|--------------|-----------------|  
|`MethodJitInliningSucceeded`|185|O inlining do método foi bem-sucedido.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|O namespace do método sendo compilado.|  
|MethodBeingCompiledName|win:UnicodeString|O nome do método sendo compilado.|  
|MethodBeingCompiledNameSignature|win:UnicodeString|A assinatura do método sendo compilado.|  
|InlinerNamespace|win:UnicodeString|O namespace do método para o qual o compilador JIT está tentando gerar código.|  
|InlinerName|win:UnicodeString|O nome do método para o qual o compilador está tentando gerar código. Isso pode não ser o mesmo que `MethodBeingCompiledName` se o compilador está tentando embutir código em `MethodBeingCompiledName` em vez de gerar uma chamada para `InlinerName`.|  
|InlinerNameSignature|win:UnicodeString|A assinatura do item que realiza o inlining.|  
|InlineeNamespace|win:UnicodeString|O namespace do item embutido.|  
|InlineeName|win:UnicodeString|O método que o compilador está tentando embutir (e não para o qual está tentando gerar uma chamada).|  
|InlineeNameSignature|win:UnicodeString|A assinatura do item embutido.|  
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  

## <a name="jit-tail-call-events"></a>Eventos de chamada tail JIT  
  
### <a name="methodjittailcallfailed-event"></a>Evento MethodJITTailCallFailed  
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|Detalhado (5)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Acionado quando|  
|-----------|--------------|-----------------|  
|`MethodJitTailCallFailed`|189|Falha na chamada tail do método.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|Namespace do método que está sendo compilado.|  
|MethodBeingCompiledName|win:UnicodeString|Nome do método que está sendo compilado.|  
|MethodBeingCompiledNameSignature|win:UnicodeString|Assinatura do método que está sendo compilado.|  
|CallerNamespace|win:UnicodeString|O namespace do método para o qual o compilador JIT está tentando gerar código.|  
|CallerName|win:UnicodeString|O nome do método para o qual o compilador está tentando gerar código.|  
|CallerNameSignature|win:UnicodeString|A assinatura do chamador.|  
|CalleeNamespace|win:UnicodeString|O namespace do receptor.|  
|CalleeName|win:UnicodeString|O método para o qual o compilador está tentando fazer uma chamada tail (e não para o qual está tentando gerar uma chamada).|  
|CalleeNameSignature|win:UnicodeString|A assinatura do receptor.|  
|TailPrefix|win:Boolean|O prefixo da chamada tail|  
|FailReason|win:UnicodeString|O motivo pelo qual a chamada tail falhou.|  
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  
  
### <a name="methodjittailcallsucceeded-event"></a>Evento MethodJITTailCallSucceeded  
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|Detalhado (5)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Acionado quando|  
|-----------|--------------|-----------------|  
|`MethodJitTailCallSucceeded`|188|A chamada tail de método bem-sucedida.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|Namespace do método que está sendo compilado.|  
|MethodBeingCompiledName|win:UnicodeString|Nome do método que está sendo compilado.|  
|MethodBeingCompiledNameSignature|win:UnicodeString|Assinatura do método que está sendo compilado.|  
|CallerNamespace|win:UnicodeString|O namespace do método para o qual o compilador JIT está tentando gerar código.|  
|CallerName|win:UnicodeString|O nome do método para o qual o compilador está tentando gerar código.|  
|CallerNameSignature|win:UnicodeString|A assinatura do chamador.|  
|CalleeNamespace|win:UnicodeString|O namespace do receptor.|  
|CalleeName|win:UnicodeString|O método para o qual o compilador está tentando fazer uma chamada tail (e não para o qual está tentando gerar uma chamada).|  
|CalleeNameSignature|win:UnicodeString|A assinatura do receptor.|  
|TailPrefix|win:Boolean|O prefixo da chamada tail.|  
|TailCallType|win:UnicodeString|O tipo da chamada tail.|  
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  
  
## <a name="see-also"></a>Consulte também

- [Eventos ETW no CLR](clr-etw-events.md)
