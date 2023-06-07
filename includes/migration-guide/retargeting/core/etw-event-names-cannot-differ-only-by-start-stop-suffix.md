---
ms.openlocfilehash: 0e25d5d9b545e5cb400cbf701fb13da572fadf10
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614276"
---
### <a name="etw-event-names-cannot-differ-only-by-a-start-or-stop-suffix"></a>Nomes de evento ETW não podem ser diferentes apenas por um sufixo "Start" ou "Stop"

#### <a name="details"></a>Detalhes

No .NET Framework 4,6 e 4.6.1, o tempo de execução gera um <xref:System.ArgumentException> quando dois nomes de evento ETW (rastreamento de eventos para Windows) diferem apenas por um sufixo "Start" ou "Stop" (como quando um evento é nomeado `LogUser` e outro é nomeado `LogUserStart` ). Nesse caso, o runtime não pode construir a origem do evento, que não pode emitir nenhum log.

#### <a name="suggestion"></a>Sugestão

Para evitar a exceção, verifique se nenhum nome de dois eventos difere apenas por um sufixo "Start" ou "Stop". Esse requisito é removido começando com o .NET Framework 4.6.2; o tempo de execução pode desambiguar nomes de eventos que diferem apenas pelo sufixo "Start" e "Stop".

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6         |
| Type    | Redirecionando |
