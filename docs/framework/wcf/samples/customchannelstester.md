---
title: CustomChannelsTester
ms.date: 03/30/2017
ms.assetid: ee1fa307-98b1-4647-8860-2e9217ba6082
ms.openlocfilehash: 9123167e0f97592592765f7b4a4aa768064fc173
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596598"
---
# <a name="customchannelstester"></a>CustomChannelsTester
O `CustomChannelsTester` é uma ferramenta que você pode usar para testar suas implementações de canal personalizadas em um conjunto de contratos de serviço predefinidos. Você pode selecionar o conjunto de contratos de serviço e passá-lo para a ferramenta usando um arquivo XML. Em seguida, a ferramenta gera o serviço e o cliente que exercitam suas implementações de canal personalizadas durante a troca de mensagens.  
  
### <a name="to-build-the-tool"></a>Para criar a ferramenta  
  
1. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
2. A criação da solução gera três arquivos: CustomChannelsTester. exe, TestSpec. xml e SampleRun. cmd. O arquivo SampleRun. cmd tem uma linha de comando de exemplo que mostra como usar essa ferramenta para testar o exemplo de [transporte: UDP](transport-udp.md) .  
  
### <a name="to-run-the-tool"></a>Para executar a ferramenta  
  
- No prompt de comando, digite o seguinte comando:  
  
    ```console  
    CustomChannelsTester.exe /binding:YourCustomBindngName /dll:TheAssemblyWhereThisTypeisDefined /testspec:XmlFileNameWhichContainsTestOptions  
    ```  
  
     O uso da `/binding` opção é obrigatório.  
  
     `/dll`será necessário se "Binding" não for uma associação fornecida pelo sistema fornecida pelo Windows Communication Foundation (WCF).  
  
     `/testspec` é opcional.  
  
     Isso cria servidores e clientes com base nas especificações de teste e na associação.  
  
     Executa o cliente e o servidor e retorna os resultados.  
  
     Este é o XML de exemplo para a descrição das especificações de teste (testspec. xml):  
  
    ```xml  
    <TestSpec xmlns="http://WCF/TestSpec" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >  
    <ServiceContract>  
    <!-- Test a contract which has oneway / twoway operations. If you set ExpandAll = true, both types of contracts are tested -->    <IsOneWay ExpandAll="true">true</IsOneWay>  
    <!-- Test a contract with Asynchronous / Synchronous Operations-->  
        <IsAsync>false</IsAsync>
    <!-- Test a sessionful / sessionless contract-->
        <IsSession ExpandAll="true">true</IsSession>  
    <!-- If the Service Contract includes a CallBack Contract-->
        <IsCallBack ExpandAll="true">true</IsCallBack>  
    </ServiceContract>  
    <TestDetails>  
    <!-- Name of the machine that runs the server - required if you want to run the test crossmachine-->  
        <ServerName>ReplaceThisWithTheServerMachineName</ServerName>  
    <!-- Port Number - Optional-->  
        <Port>8000</Port>  
    <!--URI for the callBack address for the client. The client will receive the messages from the server on this address in case of a CallBack Contract-->  
        <ClientCallBackAddress/>
    <!-- Duration (in sec) after the server has started, it times out - optional(default = 300sec) -->  
        <ServerTimeout>300</ServerTimeout>  
    <!-- Duration (in sec) before the Client initializes -optional(default = 60sec) -->  
        <ClientTimeout>60</ClientTimeout>  
    <!-- Number of clients for each service - optional(default = 1) -->  
        <NumberOfClients>1</NumberOfClients>  
    <!-- Number of messages each client sends to the service - optional(default = 1) -->  
        <MessagesPerClient>1</MessagesPerClient>  
    </TestDetails>  
    </TestSpec>  
    ```  
