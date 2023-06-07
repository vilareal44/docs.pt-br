---
title: <commonBehaviors>
ms.date: 03/30/2017
ms.assetid: 5260aeca-388d-4e82-94c0-124fa8054cf5
ms.openlocfilehash: 73bf759fd3dfa87398aa74de93160a4ffce08eba
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "61704356"
---
# \<commonBehaviors>
A `commonBehaviors` seção só pode ser definida no arquivo Machine. config. Ele define duas coleções filho chamadas `endpointBehaviors` e `serviceBehaviors` .  Cada coleção define elementos de comportamento consumidos por todos os pontos de extremidade do WCF e serviços no computador, respectivamente. Os comportamentos definidos em `endpointBehaviors` são aplicados somente aos clientes, e os comportamentos definidos no `serviceBehaviors` são aplicados somente aos serviços. Se um comportamento for definido em ambas `commonBehaviors` as `behaviors` seções e, o comportamento na `behaviors` seção terá preferência.
