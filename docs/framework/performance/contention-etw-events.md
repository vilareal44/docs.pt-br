---
title: Eventos ETW de contenção-.NET
description: Obtenha detalhes sobre os eventos ETW de contenção, que são gerados sempre que há contenção para System. Threading. monitor Locks ou bloqueios nativos usados pelo tempo de execução.
ms.date: 03/30/2017
helpviewer_keywords:
- contention events [.NET Framework]
- ETW, contention events (CLR)
ms.assetid: 6933e753-2f2a-425b-ae84-42138c957d76
ms.openlocfilehash: a36b091e0896562fffdb66e895d70049ce0667d7
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309593"
---
# <a name="contention-etw-events"></a>Eventos ETW de contenção

Eventos de contenção são acionados sempre que há contenção em bloqueios <xref:System.Threading.Monitor?displayProperty=nameWithType> ou bloqueios nativos usados pelo runtime. A contenção ocorre quando um thread aguarda um bloqueio, enquanto outro thread possui o bloqueio.

A tabela a seguir mostra a palavra-chave com a qual os eventos de contenção são acionados, além do nível dos eventos. Para obter mais informações, consulte [palavras-chave e níveis do ETW do CLR](clr-etw-keywords-and-levels.md).

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`ContentionKeyword` (0x4000)|Informativo (4)|

A tabela a seguir mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`ContentionStart_V1`|81|A contenção é iniciada. Esse evento não inclui o tempo de rotação antes que um thread aguarde para adquirir um bloqueio; ele é acionado apenas quando o thread aguarda para adquirir um bloqueio.|
|`ContentionStop`|91|A contenção é encerrada.|

A tabela a seguir mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|Flags|win:UInt8|0 para gerenciado; 1 para nativo.|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR.|

## <a name="see-also"></a>Confira também

- [Eventos ETW no CLR](clr-etw-events.md)
