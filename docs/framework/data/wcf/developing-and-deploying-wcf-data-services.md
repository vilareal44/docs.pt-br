---
title: Desenvolvendo e implantando WCF Data Services
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, developing
- WCF Data Services, deploying
- deploying [WCF Data Services
- developing applications [WCF Data Services]
ms.assetid: 6557c0e3-5aea-4f6e-bc14-77ad317a168b
ms.openlocfilehash: 7519dce8ed17bc623173f30222296ffaa42b4341
ms.sourcegitcommit: 3492dafceb5d4183b6b0d2f3bdf4a1abc4d5ed8c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2020
ms.locfileid: "86416064"
---
# <a name="develop-and-deploy-wcf-data-services"></a>Desenvolva e implante WCF Data Services

Este artigo fornece informações sobre como desenvolver e implantar WCF Data Services. Para obter informações mais básicas sobre WCF Data Services, consulte [introdução](getting-started-with-wcf-data-services.md) e [visão geral](wcf-data-services-overview.md).

## <a name="develop-wcf-data-services"></a>Desenvolver WCF Data Services

Ao usar WCF Data Services para criar um serviço de dados que ofereça suporte ao Protocolo Open Data (OData), você deve executar as seguintes tarefas básicas durante o desenvolvimento:

1. **Definir o modelo de dados**

     O WCF Data Services dá suporte a uma variedade de provedores de serviços de dados que permitem que você defina um modelo de dados com base em dados de uma variedade de fontes de dados, de bancos de dados relacionais a tipos de dado de associação tardia. Para obter mais informações, consulte [provedores de serviços de dados](data-services-providers-wcf-data-services.md).

2. **Criar o serviço de dados**

     O serviço de dados mais básico expõe uma classe herdada da classe <xref:System.Data.Services.DataService%601>, com um tipo `T` que é o nome qualificado para namespace do contêiner de entidade. Para obter mais informações, consulte [definindo WCF Data Services](defining-wcf-data-services.md).

3. **Configurar o serviço de dados**

     Por padrão, WCF Data Services desabilita o acesso a recursos que são expostos por um contêiner de entidade. A <xref:System.Data.Services.DataServiceConfiguration> interface permite que você configure o acesso a recursos e operações de serviço, especifique a versão com suporte do OData e defina outros comportamentos de todo o serviço, como comportamentos de envio em lote ou o número máximo de entidades que podem ser retornadas em um único feed de resposta. Para obter mais informações, consulte [Configurando o serviço de dados](configuring-the-data-service-wcf-data-services.md).

Este artigo aborda principalmente o desenvolvimento e a implantação de serviços de dados usando o Visual Studio. Para obter informações sobre a flexibilidade fornecida pelo WCF Data Services para expor seus dados como feeds OData, consulte [definindo WCF Data Services](defining-wcf-data-services.md).

### <a name="choose-a-development-web-server"></a>Escolher um servidor Web de desenvolvimento

Ao desenvolver um serviço de dados WCF como um aplicativo ASP.NET ou site ASP.NET usando o Visual Studio 2015, você tem a opção de servidores Web na qual executar o serviço de dados durante o desenvolvimento. Os seguintes servidores Web se integram com o Visual Studio para facilitar o teste e a depuração dos serviços de dados no computador local.

1. **Servidor de IIS local**

     Quando você cria um serviço de dados que é um aplicativo ASP.NET ou site ASP.NET que é executado em Serviços de Informações da Internet (IIS), recomendamos que você desenvolva e teste seu serviço de dados usando o IIS no computador local. Executar o serviço de dados no IIS facilita o rastreamento de solicitações HTTP durante a depuração. Isso também permite que você predetermine os direitos necessários exigidos pelo IIS para acessar arquivos, bancos de dados e outros recursos exigidos pelo Data Service. Para executar o serviço de dados no IIS, verifique se o IIS e o Windows Communication Foundation (WCF) estão instalados e configurados corretamente e conceda acesso a contas do IIS no sistema de arquivos e nos bancos de dados. Para obter mais informações, consulte [como: desenvolver um serviço de dados WCF em execução no IIS](how-to-develop-a-wcf-data-service-running-on-iis.md).

    > [!NOTE]
    > Você deve executar o Visual Studio com direitos de administrador para habilitar o ambiente de desenvolvimento para configurar o servidor IIS local.

2. **Visual Studio Development Server**

     O Visual Studio inclui um servidor Web interno, o Visual Studio Development Server, que é o servidor Web padrão para projetos ASP.NET. Este servidor Web foi projetado para executar projetos do ASP.NET no computador local durante o desenvolvimento. O guia de [início rápido do WCF Data Services](quickstart-wcf-data-services.md) mostra como criar um serviço de dados executado no Visual Studio Development Server.

     Lembre-se das seguintes limitações ao usar o Visual Studio Development Server para desenvolver o serviço de dados:

    - Este servidor só pode ser acessado no computador local.

    - Este servidor escuta no `localhost` e em uma porta específica, não na porta 80, que é a porta padrão para mensagens HTTP. Para obter mais informações, consulte [Servidores Web no Visual Studio para projetos Web ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/58wxa9w5(v=vs.120)).

    - Este servidor executa o serviço de dados no contexto de sua conta de usuário atual. Por exemplo, se você estiver executando como um usuário de nível de administrador, um serviço de dados em execução no Visual Studio Development Server terá privilégios de nível de administrador. Isso pode fazer o serviço de dados ser capaz de acessar recursos que ele não tem os direitos de acesso quando implantado em um servidor IIS.

    - Este servidor não inclui os recursos adicionais do IIS, como autenticação.

    - Esse servidor não pode tratar fluxos HTTP em bloco, que são enviados por padrão pelo cliente WCF Data Services ao acessar dados binários grandes do serviço de dados. Para obter mais informações, consulte [streaming Provider](streaming-provider-wcf-data-services.md).

    - Esse servidor tem problemas com o processamento do caractere de ponto ( `.` ) em uma URL, embora esse caractere tenha suporte de WCF Data Services em valores de chave.

    > [!TIP]
    > Embora você possa usar o Visual Studio Development Server para testar seus serviços de dados durante o desenvolvimento, você deve testá-los novamente após a implantação em um servidor Web que esteja executando o IIS.

3. **Ambiente de desenvolvimento do Azure**

     As ferramentas do Azure para Visual Studio incluem um conjunto integrado de ferramentas para desenvolver serviços do Azure no Visual Studio. Com essas ferramentas, você pode desenvolver um serviço de dados que pode ser implantado no Azure, e você pode testar o serviço de dados no computador local antes da implantação. Use essas ferramentas ao usar o Visual Studio para desenvolver um serviço de dados executado na plataforma do Azure. Para obter informações sobre como instalar as ferramentas, consulte [Ferramentas do Azure para Visual Studio 2015](../../../azure/vs2015-install.md). Para obter mais informações sobre como desenvolver um serviço de dados que é executado no Azure, consulte o post [implantando um serviço OData no Azure](https://docs.microsoft.com/archive/blogs/astoriateam/deploying-an-odata-service-in-windows-azure).

### <a name="development-tips"></a>Dicas de desenvolvimento

Considere o seguinte ao desenvolver um serviço de dados:

- Se você planeja autenticar usuários ou restringir o acesso para usuários específicos, determine os requisitos de segurança do seu serviço de dados. Para obter mais informações, consulte [securing WCF Data Services](securing-wcf-data-services.md).

- Um programa de inspeção HTTP pode ser útil ao depurar um serviço de dados, permitindo que você inspecione o conteúdo das mensagens de solicitação e resposta. Qualquer analisador de pacote de rede que possa exibir pacotes brutos pode ser usado para inspecionar solicitações HTTP para e respostas do serviço de dados.

- Ao depurar um serviço de dados, talvez você queira obter mais informações sobre um erro do serviço de dados do que durante a operação regular. Você pode obter informações de erro adicionais do serviço de dados definindo a propriedade <xref:System.Data.Services.DataServiceConfiguration.UseVerboseErrors%2A> no <xref:System.Data.Services.DataServiceConfiguration> para `true` e definindo a propriedade <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A> do atributo <xref:System.ServiceModel.Description.ServiceDebugBehavior> na classe de serviço de dados como `true`. Para obter mais informações, consulte o [WCF Data Services](https://docs.microsoft.com/archive/blogs/phaniraj/debugging-wcf-data-services)pós-depuração. Você também pode habilitar o rastreamento no WCF para exibir exceções geradas na camada de mensagens HTTP. Para obter mais informações, consulte [Configurando o rastreamento](../../wcf/diagnostics/tracing/configuring-tracing.md).

- Um serviço de dados normalmente é desenvolvido como um projeto de aplicativo ASP.NET, mas você também pode criar o serviço de dados como um projeto de site ASP.NET no Visual Studio. Para obter informações sobre as diferenças entre os dois tipos de projetos, consulte [projetos de aplicativos Web versus projetos de site no Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd547590(v=vs.110)).

- Quando você cria um serviço de dados usando a caixa de diálogo **Adicionar novo item** no Visual Studio, o serviço de dados é hospedado pelo ASP.net no IIS. Embora ASP.NET e IIS seja o host padrão para um serviço de dados, há suporte para outras opções de hospedagem. Para obter mais informações, consulte [hospedando o serviço de dados](hosting-the-data-service-wcf-data-services.md).

## <a name="deploy-wcf-data-services"></a>Implantar WCF Data Services

O WCF Data Service fornece flexibilidade na escolha do processo que hospeda o serviço de dados. Você pode usar o Visual Studio para implantar um serviço de dados nas seguintes plataformas:

- **Servidor Web hospedado no IIS**

    Quando um serviço de dados é desenvolvido como um projeto ASP.NET, ele pode ser implantado em um servidor Web do IIS usando os processos de implantação padrão do ASP.NET. O Visual Studio fornece as seguintes tecnologias de implantação para ASP.NET, dependendo do tipo de projeto ASP.NET que hospeda o serviço de dados que você está implantando.

  - **Tecnologias de implantação para aplicativos Web ASP.NET**

    - [Como: criar um pacote de implantação da Web no Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465323(v=vs.110))

    - [Como implantar um projeto Web usando a publicação com um clique no Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465337(v=vs.110))

  - **Tecnologias de implantação para sites Web ASP.NET**

    - [Como copiar arquivos de site com a ferramenta Copiar site da Web](https://docs.microsoft.com/previous-versions/aspnet/c95809c0(v=vs.100))

    - [Como publicar sites da Web](https://docs.microsoft.com/previous-versions/aspnet/20yh9f1b(v=vs.100))

    - [Walkthrough: Implantando um aplicativo Web ASP.NET usando XCOPY](https://docs.microsoft.com/previous-versions/aspnet/f735abw9(v=vs.100))

     Para obter mais informações sobre as opções de implantação para um aplicativo ASP.NET, consulte [visão geral da implantação da Web para Visual Studio e ASP.net](https://docs.microsoft.com/previous-versions/aspnet/dd394698(v=vs.110)).

    > [!TIP]
    > Antes de tentar implantar o serviço de dados para o IIS, verifique se você testou a implantação para um servidor Web que esteja executando o IIS. Para obter mais informações, consulte [como: desenvolver um serviço de dados WCF em execução no IIS](how-to-develop-a-wcf-data-service-running-on-iis.md).

- **Azure**

     Você pode implantar um serviço de dados no Azure usando as [Ferramentas do Azure para Visual Studio](../../../azure/vs2015-install.md). Para obter mais informações sobre como implantar um serviço de dados no Azure, consulte [implantando um serviço OData no Azure](https://docs.microsoft.com/archive/blogs/astoriateam/deploying-an-odata-service-in-windows-azure).

### <a name="deployment-considerations"></a>Considerações de implantação

Considere o seguinte ao implantar um serviço de dados:

- Quando você implanta um serviço de dados que usa o provedor de Entity Framework para acessar um banco de dados do SQL Server, você também pode precisar propagar estruturas, dados ou ambos com a implantação do serviço de dados. O Visual Studio pode criar scripts automaticamente (arquivos. Sql) para fazer isso no banco de dados de destino, e esses scripts podem ser incluídos no pacote de implantação da Web de um aplicativo ASP.NET. Para obter mais informações, consulte [como: implantar um banco de dados com um projeto de aplicativo Web](https://docs.microsoft.com/previous-versions/dd465343(v=vs.100)). Para um site da Web do ASP.NET, você pode fazer isso usando o **Assistente de publicação de banco de dados** no Visual Studio. Para obter mais informações, consulte [publicando um banco de dados SQL](https://docs.microsoft.com/previous-versions/aspnet/bb907585(v=vs.100)).

- Como o WCF Data Services inclui uma implementação básica do WCF, você pode usar o Windows Server AppFabric para monitorar um serviço de dados implantado no IIS em execução no Windows Server. Para obter mais informações sobre como usar o Windows Server AppFabric para monitorar um serviço de dados, consulte o WCF Data Services de rastreamento de postagem [com o Windows Server AppFabric](https://docs.microsoft.com/archive/blogs/rjacobs/tracking-wcf-data-services-with-windows-server-appfabric).

## <a name="see-also"></a>Confira também

- [Hospedar o serviço de dados](hosting-the-data-service-wcf-data-services.md)
- [Protegendo o WCF Data Services](securing-wcf-data-services.md)
- [Configurando WCF Data Services](defining-wcf-data-services.md)
