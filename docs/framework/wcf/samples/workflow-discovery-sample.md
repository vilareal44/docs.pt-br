---
title: Exemplo de descoberta de fluxo de trabalho
ms.date: 03/30/2017
ms.assetid: 82cc43f1-3c8f-4771-ac19-a75ac936e2c3
ms.openlocfilehash: 1c6210472b594aec02bdf47f472a1a8b1823230c
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202069"
---
# <a name="workflow-discovery-sample"></a>Exemplo de descoberta de fluxo de trabalho
Este exemplo demonstra como tornar um serviço de fluxo de trabalho detectável e como criar uma atividade de código personalizada que pesquisa um serviço específico.  
  
## <a name="demonstrates"></a>Demonstra  
 Localização da descoberta de atividades e uso de fluxo de trabalho  
  
## <a name="discussion"></a>Discussão  
 Na primeira parte do exemplo, um serviço de fluxo de trabalho torna-se detectável usando a configuração. A configuração também pode ser usada para aplicar o serviço adequadamente com metadados personalizados (como escopos). No cliente, o exemplo usa uma atividade de código personalizada, que usa a descoberta para procurar um serviço que corresponda a um contrato específico. A atividade de código gera um URI, que é usado posteriormente por uma atividade de envio.  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Este exemplo usa pontos de extremidade HTTP, que devem ter ACLs de URL adequadas para execução (consulte [Configurando http e HTTPS](../feature-details/configuring-http-and-https.md) para obter detalhes). A execução do comando a seguir em um prompt de comandos com privilégios elevados deve adicionar as ACLs apropriadas. Se o seu shell não entender o formato da variável, substitua o seu domínio e o nome de usuário pelos argumentos a seguir.  
  
    `netsh http add urlacl url=http://+:8000/ user=%DOMAIN%\%UserName%`
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Discovery\WorkflowDiscovery`
