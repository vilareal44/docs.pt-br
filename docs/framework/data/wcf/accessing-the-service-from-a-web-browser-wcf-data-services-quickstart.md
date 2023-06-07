---
title: Acessando o serviço de um navegador da Web (WCF Data Services Quickstart)
description: Aprenda a iniciar o WCF Data Services no Visual Studio e a desabilitar a leitura do feed em um navegador. Obtenha o documento de definição de serviço e acesse os recursos do serviço de dados.
ms.date: 03/30/2017
ms.assetid: 5a6fa180-3094-4e6e-ba2b-8c80975d18d1
ms.openlocfilehash: 713436c31bc3f622c4f44a83e33fff3fcbba1c1c
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247772"
---
# <a name="accessing-the-service-from-a-web-browser-wcf-data-services-quickstart"></a>Acessando o serviço de um navegador da Web (WCF Data Services Quickstart)

Esta é a segunda tarefa do guia de início rápido do WCF Data Services. Nesta tarefa, você inicia o WCF Data Services do Visual Studio e, opcionalmente, desabilita a leitura do feed no navegador da Web. Em seguida, você recupera o documento de definição de serviço, bem como os recursos do serviço de dados de acesso enviando solicitações HTTP GET por meio de um navegador da Web para os recursos expostos.

> [!NOTE]
> Por padrão, o Visual Studio atribui automaticamente um número de porta para o `localhost` URI no seu computador. Essa tarefa usa o número da porta `12345` nos exemplos do URI. Para obter mais informações sobre como definir um número de porta específico em seu projeto do Visual Studio, consulte [criando o serviço de dados](creating-the-data-service.md).

## <a name="to-request-the-default-service-document-by-using-internet-explorer"></a>Para solicitar o documento padrão de serviço usando o Internet Explorer

1. No Internet Explorer, no menu **ferramentas** , selecione **Opções da Internet**, clique na guia **conteúdo** , clique em **configurações**e desmarque a **exibição do feed**.

     Isso garantirá que a leitura de feeds estará desabilitada. Se você não desabilitar essa funcionalidade, o navegador da Web tratará o documento codificado AtomPub retornado como um feed XML em vez de exibir os dados XML brutos.

    > [!NOTE]
    > Se seu navegador não puder exibir o feed como dados XML brutos, você ainda deverá ser capaz de exibir o feed como código-fonte para a página.

2. No Visual Studio, pressione a tecla **F5** para iniciar a depuração do aplicativo.

3. Abra um navegador da Web no computador local. Na barra de endereços, digite a seguinte URL:

    ```http
    http://localhost:12345/northwind.svc
    ```

     Isso retorna o documento padrão de serviço, que contém uma lista de conjuntos de entidades que são expostos por esse serviço de dados.

## <a name="to-access-entity-set-resources-from-a-web-browser"></a>Para acessar os recursos do conjunto de entidades de um navegador da Web

1. Na barra de endereços do seu navegador da Web, digite a seguinte URL:

    ```http
    http://localhost:12345/northwind.svc/Customers
    ```

     Isso retorna um conjunto de todos os clientes no banco de dados de exemplo Northwind.

2. Na barra de endereços do seu navegador da Web, digite a seguinte URL:

    ```http
    http://localhost:12345/northwind.svc/Customers('ALFKI')
    ```

     Isso retorna uma instância de entidade para o cliente específico, `ALFKI`.

3. Na barra de endereços do seu navegador da Web, digite a seguinte URL:

    ```http
    http://localhost:12345/northwind.svc/Customers('ALFKI')/Orders
    ```

     Isso percorre a relação entre clientes e pedidos para retornar um conjunto de todos os pedidos para o cliente específico `ALFKI`.

4. Na barra de endereços do seu navegador da Web, digite a seguinte URL:

    ```http
    http://localhost:12345/northwind.svc/Customers('ALFKI')/Orders?$filter=OrderID eq 10643
    ```

     Isso filtra os pedidos que pertencem ao cliente específico `ALFKI` de modo que somente uma ordem específica seja retornada com base no valor `OrderID` fornecido.

## <a name="next-steps"></a>Próximas etapas

Você acessou com êxito a WCF Data Services de um navegador da Web, com o navegador emitindo solicitações HTTP GET para os recursos especificados. Um navegador da Web fornece uma maneira fácil para experimentar a sintaxe de endereçamento de solicitações e exibir os resultados. No entanto, um serviço de dados de produção geralmente não é acessado por esse método. Normalmente, os aplicativos interagem com o serviço de dados por meio de código de aplicativo ou linguagens de script. Em seguida, você criará um aplicativo cliente que usa bibliotecas de cliente para acessar os recursos do serviço de dados como se eles fossem Common Language Runtime (CLR):

> [!div class="nextstepaction"]
> [Criando o aplicativo cliente do .NET Framework](creating-the-dotnet-client-application-wcf-data-services-quickstart.md)

## <a name="see-also"></a>Veja também

- [Acessando recursos do serviço de dados](accessing-data-service-resources-wcf-data-services.md)
