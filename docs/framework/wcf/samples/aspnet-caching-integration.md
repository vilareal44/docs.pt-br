---
title: Integração de cache ASP.NET
ms.date: 03/30/2017
ms.assetid: f581923a-8a72-42fc-bd6a-46de2aaeecc1
ms.openlocfilehash: c541f3caad8a500b9fdb33d00b58706bac876e37
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594745"
---
# <a name="aspnet-caching-integration"></a>Integração de cache ASP.NET

Este exemplo demonstra como utilizar o cache de saída ASP.NET com o modelo de programação WEB HTTP do WCF. Este tópico se concentra no recurso de integração do cache de saída do ASP.NET.

## <a name="demonstrates"></a>Demonstra

Integração com o cache de saída ASP.NET

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Web\AspNetCachingIntegration`

## <a name="discussion"></a>Discussão

O exemplo usa o <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute> para utilizar o cache de saída do ASP.NET com o serviço do Windows Communication Foundation (WCF). O <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute> é aplicado às operações de serviço e fornece o nome de um perfil de cache em um arquivo de configuração que deve ser aplicado a respostas da operação especificada.

No arquivo Service.cs do projeto de serviço de exemplo, as `GetCustomer` operações e `GetCustomers` são marcadas com o <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute> , que fornece o nome do perfil de cache "CacheFor60Seconds". No arquivo Web. config do projeto de serviço, o perfil de cache "CacheFor60Seconds" é fornecido sob o `caching` elemento < > de < `system.web` >. Para esse perfil de cache, o valor do `duration` atributo é "60", portanto, as respostas associadas a esse perfil são armazenadas em cache no cache de saída ASP.net por 60 segundos. Além disso, para esse perfil de cache, o `varmByParam` atributo é definido como "Format" para que as solicitações com valores diferentes para o `format` parâmetro de cadeia de caracteres de consulta tenham suas respostas armazenadas em cache separadamente. Por fim, o atributo do perfil de cache `varyByHeader` é definido como "Accept", portanto, as solicitações com valores de cabeçalho Accept diferentes têm suas respostas armazenadas em cache separadamente.

Program.cs no projeto do cliente demonstra como um cliente desse tipo pode ser criado usando o <xref:System.Net.HttpWebRequest> . Observe que essa é apenas uma maneira de acessar um serviço WCF. Também é possível acessar o serviço usando outras classes de .NET Framework, como a fábrica de canais do WCF e o <xref:System.Net.WebClient> . Outros exemplos no SDK (como o exemplo de [serviço http básico](basic-http-service.md) ) ilustram como usar essas classes para se comunicar com um serviço WCF.

## <a name="to-run-the-sample"></a>Para executar a amostra

O exemplo consiste em três projetos:

- **Serviço**: um projeto de aplicativo Web que inclui um serviço http do WCF hospedado em ASP.net.

- **Cliente**: um projeto de aplicativo de console que faz chamadas para o serviço.

- **Comum**: uma biblioteca compartilhada que contém o tipo de cliente usado pelo cliente e serviço.

À medida que o aplicativo de console do cliente é executado, o cliente faz solicitações ao serviço e grava as informações pertinentes das respostas na janela do console.

#### <a name="to-run-the-sample"></a>Para executar a amostra

1. Abra a solução para o exemplo de integração do cache ASP.NET.

2. Pressione CTRL+SHIFT+B para criar a solução.

3. Se a janela de **Gerenciador de soluções** ainda não estiver aberta, pressione CTRL + W + S.

4. Na janela **Gerenciador de soluções** , clique com o botão direito do mouse no projeto de **serviço** e selecione **Iniciar nova instância**. Isso inicia o servidor de desenvolvimento do ASP.NET, que hospeda o serviço.

5. Na janela **Gerenciador de soluções** , clique com o botão direito do mouse no projeto **cliente** e selecione **Iniciar nova instância**.

6. A janela do console do cliente é exibida e fornece o URI do serviço em execução e o URI da página de ajuda HTML para o serviço em execução. Em qualquer momento, você pode exibir a página de ajuda HTML digitando o URI da página de ajuda em um navegador.

7. À medida que o exemplo é executado, o cliente grava o status da atividade atual.

8. Pressione qualquer tecla para encerrar o aplicativo de console do cliente.

9. Pressione SHIFT + F5 para parar a depuração do serviço.

10. Na área de notificação do Windows, clique com o botão direito do mouse no ícone do servidor de desenvolvimento ASP.NET e selecione **parar**.
