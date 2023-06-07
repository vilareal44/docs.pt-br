---
ms.openlocfilehash: a44458484ea09c566defeabc9b604bd55d994e93
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617518"
---
### <a name="calls-to-systemwindowsinputpencontextdisable-on-touch-enabled-systems-may-throw-an-argumentexception"></a>Chamadas para System.Windows.Input.PenContext.Disable em sistemas habilitados para toque podem gerar ArgumentException

#### <a name="details"></a>Detalhes

Em algumas circunstâncias, chamadas para o método interno **System.Windows.Intput.PenContext.Disable** em sistemas habilitados para toque podem gerar uma `T:System.ArgumentException` sem tratamento devido à reentrância.

#### <a name="suggestion"></a>Sugestão

Esse problema foi resolvido no .NET Framework 4.7. Para evitar a exceção, atualize para uma versão do .NET Framework a partir de 4.7.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6.1       |
| Type    | Redirecionando |
