---
title: Agendando threads
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- threading [.NET Framework], scheduling
- scheduling threads
ms.assetid: 67e4a0eb-3095-4ea7-b20f-908faa476277
ms.openlocfilehash: fea809168bf2f4f888466f87259497660afd13be
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291143"
---
# <a name="scheduling-threads"></a>Agendando threads

Cada thread tem uma prioridade atribuída. Os threads criados dentro do Common Language Runtime recebem inicialmente a prioridade <xref:System.Threading.ThreadPriority.Normal?displayProperty=nameWithType>. Threads criados fora do Runtime retêm a prioridade que tinham antes de entrarem no ambiente gerenciado. Você pode obter ou definir a prioridade de qualquer thread com a propriedade <xref:System.Threading.Thread.Priority?displayProperty=nameWithType>.  
  
 Threads são agendados para execução com base em sua prioridade. Mesmo que os threads sejam executados no Runtime, todos são atribuídos frações de tempo do processador pelo sistema operacional. Os detalhes do algoritmo de agendamento usado para determinar a ordem na qual os threads são executados variam de acordo com cada sistema operacional. Em alguns sistemas operacionais, o thread com a prioridade mais alta (dos threads que podem ser executados) sempre é programada para ser executado primeiro. Se vários threads com a mesma prioridade estiverem disponíveis, o agendador percorre os threads com essa prioridade, fornecendo uma fração de tempo fixa na qual executar cada thread. Enquanto houver um thread com uma prioridade mais alta estiver disponível para execução, threads com prioridades inferiores não serão executados. Quando não houver mais nenhum threads executável com determinada prioridade, o agendador passará para a próxima prioridade mais alta e agendará a execução dos threads com essa prioridade. Se um thread de prioridade mais alta se tornar executável, o thread de prioridade mais baixo será ignorado e o thread de prioridade mais alta poderá ser executado novamente. Além disso, o sistema operacional também poderá ajustar as prioridades de threads dinamicamente conforme a interface de usuário do aplicativo é movida entre o primeiro e segundo plano. Outros sistemas operacionais podem optar por usar um algoritmo de programação diferente.  
  
## <a name="see-also"></a>Veja também

- [Usando threads e threading](using-threads-and-threading.md)
- [Threads gerenciados e não gerenciados no Windows](managed-and-unmanaged-threading-in-windows.md)
