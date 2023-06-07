---
title: Adicionando uma referência de serviço em uma solução de fluxo de trabalho
ms.date: 03/30/2017
ms.assetid: 83574cf3-9803-49bc-837f-432936dc9c76
ms.openlocfilehash: 577026249e00b528cf5c6e7ccd9527e02cb22a28
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597664"
---
# <a name="add-a-service-reference-in-a-workflow-solution"></a>Adicionar uma referência de serviço em uma solução de fluxo de trabalho

Adicionar uma referência de serviço em um aplicativo de fluxo de trabalho funciona um pouco diferente do que um aplicativo WCF regular. Quando você seleciona **Adicionar**  >  **referência de serviço** e especifica a URL para o serviço, os metadados são baixados e as atividades personalizadas são geradas que permitem chamar esse serviço WCF ou o serviço de fluxo de trabalho WCF. Depois de adicionar uma referência de serviço, recompile a solução para que as atividades geradas sejam criadas. Eles serão exibidos na caixa de ferramentas do designer de fluxo de trabalho. Isso só funcionará se você estiver adicionando uma referência de serviço dentro de uma solução de fluxo de trabalho. A seguinte conversão da Web mostra como adicionar uma referência de serviço em outros tipos de projetos: [chamar um serviço WCF de um fluxo de trabalho em um projeto Web](https://docs.microsoft.com/archive/blogs/endpoint/how-to-consume-a-wcf-service-from-a-wf4-workflow).

Adicionar duas ou mais referências de serviço a serviços que contêm o mesmo nome de operação causará um problema. As atividades geradas chamarão apenas a primeira operação de serviço. Para contornar esse problema, renomeie as operações de serviço para que elas sejam diferentes ou altere o nome de configuração do ponto de extremidade dentro de cada atividade gerada.

## <a name="see-also"></a>Consulte também

- [Serviços de fluxo de trabalho](workflow-services.md)
- [Chamando um serviço WCF de um fluxo de trabalho em um projeto Web](https://docs.microsoft.com/archive/blogs/endpoint/how-to-consume-a-wcf-service-from-a-wf4-workflow)
