---
title: Coleta de lixo em segundo plano
description: Saiba mais sobre a coleta de lixo em segundo plano no .NET e como ela difere na coleta de lixo da estação de trabalho e do servidor.
ms.date: 04/21/2020
helpviewer_keywords:
- garbage collection, background
- background garbage collection
ms.openlocfilehash: bf88c14b2aeed94a548b6116749fa8669576afe1
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916996"
---
# <a name="background-garbage-collection"></a>Coleta de lixo em segundo plano

Na coleta de lixo em segundo plano (GC), as gerações efêmeras (0 e 1) são coletadas conforme necessário enquanto a coleta da geração 2 está em andamento. A coleta de lixo em segundo plano é executada em um ou mais threads dedicados, dependendo se é o GC do servidor ou em segundo plano e se aplica somente às coleções da geração 2.

A coleta de lixo em segundo plano está habilitada por padrão. Ele pode ser habilitado ou desabilitado com o parâmetro de configuração [gcConcurrent](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) em aplicativos .NET Framework ou a configuração [System. GC. Decurrent](../../core/run-time-config/garbage-collector.md#background-gc) no .NET Core e no .NET 5 e em aplicativos posteriores.

> [!NOTE]
> A coleta de lixo em segundo plano substitui a [coleta de lixo simultânea](#concurrent-garbage-collection) e está disponível no .NET Framework 4 e em versões posteriores. No .NET Framework 4, há suporte apenas para coleta de lixo de *estação de trabalho* . A partir do .NET Framework 4,5, a coleta de lixo em segundo plano está disponível para a coleta de lixo da *estação de trabalho* e do *servidor* .

Uma coleção em gerações efêmeras durante a coleta de lixo em segundo plano é conhecida como coleta de lixo em *primeiro plano* . Quando as coletas de lixo em primeiro plano ocorrem, todos os threads gerenciados são suspensos.

Quando a coleta de lixo em segundo plano estiver em andamento e você tiver alocado objetos suficientes na geração 0, o CLR executará uma coleta de lixo de primeiro plano de geração 0 ou geração 1. O thread dedicado de coleta de lixo em segundo plano verifica em pontos frequentes de segurança se há uma solicitação de coleta de lixo em primeiro plano. Se houver, a coleta de plano de fundo suspende a si mesma para que a coleta de lixo em primeiro plano possa ocorrer. Após a conclusão da coleta de lixo em primeiro plano, os threads de coleta de lixo e os threads de usuário dedicados são retomados.

A coleta de lixo em segundo plano remove as restrições de alocação impostas pela coleta de lixo simultânea, pois as coletas de lixo efêmeras podem ocorrer durante a coleta de lixo em segundo plano. A coleta de lixo em segundo plano pode remover objetos inativos em gerações efêmeras. Ele também pode expandir o heap, se necessário, durante uma coleta de lixo de geração 1.

## <a name="background-workstation-vs-server-gc"></a>Estação de trabalho em segundo plano vs. servidor GC

A partir do .NET Framework 4,5, a coleta de lixo em segundo plano está disponível para GC do servidor. O GC em segundo plano é o modo padrão para a coleta de lixo do servidor.

A coleta de lixo do servidor em segundo plano funciona de modo semelhante à coleta de lixo da estação de trabalho em segundo plano, mas há algumas diferenças:

- A coleta de lixo da estação de trabalho em segundo plano usa um thread de coleta de lixo de segundo plano dedicado, enquanto a coleta de lixo do servidor de segundo Normalmente, há um thread dedicado para cada processador lógico.

- Diferentemente do thread de coleta de lixo em segundo plano da estação de trabalho, os threads GC do servidor em segundo plano não expiram

A ilustração a seguir mostra a coleta de lixo da *estação de trabalho* em segundo plano executada em um thread separado e dedicado:

![Coleta de lixo de estação de trabalho em segundo plano](media/fundamentals/background-workstation-garbage-collection.png)

A ilustração a seguir mostra a coleta de lixo *do servidor* em segundo plano executada em threads separados e dedicados:

![Coleta de lixo de servidor em segundo plano](media/fundamentals/background-server-garbage-collection.png)

## <a name="concurrent-garbage-collection"></a>Coleta de lixo simultânea

> [!TIP]
> Esta seção aplica-se a:
>
> - .NET Framework 3,5 e anteriores para coleta de lixo de estação de trabalho
> - .NET Framework 4 e anterior para coleta de lixo do servidor
>
> O lixo simultâneo é substituído pela coleta de lixo em segundo plano em versões posteriores.

Na coleta de lixo da estação de trabalho ou do servidor, você pode [habilitar a coleta de lixo simultânea](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md), o que permite que os threads sejam executados simultaneamente com um thread dedicado que executa a coleta de lixo para a maior parte da duração da coleção. Essa opção afeta apenas as coletas de lixo na geração 2; as gerações 0 e 1 são sempre não simultâneas porque elas são concluídas rapidamente.

A coleta de lixo simultânea permite que aplicativos interativos sejam mais responsivos minimizando a pausa para uma coleta. Na maioria das vezes, a execução dos threads gerenciados pode continuar enquanto o thread de coleta de lixo simultânea estiver em execução. Esse design resulta em pausas mais curtas enquanto uma coleta de lixo está ocorrendo.

A coleta de lixo simultânea é executada em um thread dedicado. Por padrão, o CLR executa a coleta de lixo da estação de trabalho com a coleta de lixo simultânea habilitada em computadores com processador único e com vários processadores.

A ilustração a seguir mostra a coleta de lixo simultânea executada em um thread dedicado separado.

![Threads de coleta de lixo simultâneos](media/gc-concurrent.png)

## <a name="see-also"></a>Consulte também

- [Coleta de lixo de estação de trabalho ou de servidor](workstation-server-gc.md)
- [Opções de configuração de tempo de execução para coleta de lixo](../../core/run-time-config/garbage-collector.md)
