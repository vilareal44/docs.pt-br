---
title: Pontos de extremidade SOAP e HTTP
ms.date: 03/30/2017
ms.assetid: e3c8be75-9dda-4afa-89b6-a82cb3b73cf8
ms.openlocfilehash: fee1df86026716941f65dccca15d437ae917770b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600939"
---
# <a name="soap-and-http-endpoints"></a>Pontos de extremidade SOAP e HTTP
Este exemplo demonstra como implementar um serviço baseado em RPC e expô-lo no formato SOAP e o formato de "XML antigo" (POX) usando o modelo de programação Web do WCF. Consulte o exemplo de [serviço http básico](basic-http-service.md) para obter mais detalhes sobre a Associação http para o serviço. Este exemplo se concentra nos detalhes que pertencem à exposição do mesmo serviço por SOAP e HTTP usando associações diferentes.  
  
## <a name="demonstrates"></a>Demonstra  
 Expondo um serviço RPC por SOAP e HTTP usando o WCF.  
  
## <a name="discussion"></a>Discussão  
 Este exemplo consiste em dois componentes: um projeto de aplicativo Web (serviço) que contém um serviço WCF e um aplicativo de console (cliente) que invoca operações de serviço usando associações SOAP e HTTP.  
  
 O serviço WCF expõe 2 operações – `GetData` e `PutData` – isso ecoa a cadeia de caracteres que foi passada como entrada. As operações de serviço são anotadas com <xref:System.ServiceModel.Web.WebGetAttribute> e <xref:System.ServiceModel.Web.WebInvokeAttribute> . Esses atributos controlam a projeção HTTP dessas operações. Além disso, eles são anotados <xref:System.ServiceModel.OperationContractAttribute> , o que permite que eles sejam expostos em associações SOAP. O método do serviço `PutData` lança um <xref:System.ServiceModel.Web.WebFaultException> , que é enviado de volta por http usando o código de status HTTP e é enviado de volta por SOAP como uma falha de SOAP.  
  
 O arquivo Web. config configura o serviço WCF com 3 pontos de extremidade:  
  
- O ponto de extremidade ~/Service.svc/MEX que expõe os metadados de serviço para acesso por clientes baseados em SOAP.  
  
- O ponto de extremidade ~/Service.svc/http que permite que os clientes acessem o serviço usando a associação HTTP.  
  
- O ponto de extremidade ~/Service.svc/SOAP que permite que os clientes acessem o serviço usando a associação SOAP sobre HTTP.  
  
 O ponto de extremidade HTTP está configurado com um <`webHttp`> ponto de extremidade padrão que foi `helpEnabled` definido como `true` . Como resultado, o serviço expõe uma página de ajuda baseada em XHTML em ~/Service.svc/http/Help que os clientes baseados em HTTP podem usar para acessar o serviço.  
  
 O projeto cliente demonstra o acesso ao serviço usando um proxy SOAP (gerado por meio de **Adicionar referência de serviço**) e o acesso ao serviço usando o <xref:System.Net.WebClient> .  
  
 O exemplo consiste em um serviço hospedado na Web e um aplicativo de console. À medida que o aplicativo de console é executado, o cliente faz solicitações ao serviço e grava as informações pertinentes das respostas na janela do console.  
  
#### <a name="to-run-the-sample"></a>Para executar a amostra  
  
1. Abra a solução para o exemplo de pontos de extremidade SOAP e HTTP.  
  
2. Pressione CTRL+SHIFT+B para criar a solução.  
  
3. Se ainda não estiver aberto, pressione CTRL + W, S para abrir a janela de **Gerenciador de soluções** .  
  
4. Na janela **Gerenciador de soluções** , clique com o botão direito do mouse no projeto de **serviço** e coloque o cursor sobre a opção de menu de contexto de **depuração** para que o menu de contexto **Iniciar nova instância** seja exibido. Clique em **Iniciar nova instância**. Isso inicia o servidor de desenvolvimento do ASP.NET, que hospeda o serviço.  
  
5. No Gerenciador de Soluções Windows, clique com o botão direito do mouse no projeto cliente e coloque o cursor sobre a opção de menu de contexto de **depuração** para que o menu de contexto **Iniciar nova instância** seja exibido. Clique em **Iniciar nova instância**.  
  
6. A janela do console do cliente é exibida e fornece o URI do serviço em execução e o URI da página de ajuda HTML para o serviço em execução. Em qualquer momento, você pode exibir a página de ajuda HTML digitando o URI da página de ajuda em um navegador.  
  
7. À medida que o exemplo é executado, o cliente grava o status da atividade atual.  
  
8. Pressione qualquer tecla para encerrar o aplicativo de console do cliente.  
  
9. Pressione SHIFT + F5 para parar a depuração do serviço.  
  
10. Na área de notificação do Windows, clique com o botão direito do mouse no ícone do servidor de desenvolvimento ASP.NET e selecione **parar** no menu de contexto.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Web\SoapAndHttpEndpoints`
