---
title: Práticas recomendadas de hospedagem dos Serviços de Informações da Internet
ms.date: 03/30/2017
ms.assetid: 0834768e-9665-46bf-86eb-d4b09ab91af5
ms.openlocfilehash: e62fed4f6a711ecc317b8f758d4948a477d136e1
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595265"
---
# <a name="internet-information-services-hosting-best-practices"></a>Práticas recomendadas de hospedagem dos Serviços de Informações da Internet
Este tópico descreve algumas práticas recomendadas para hospedar serviços de Windows Communication Foundation (WCF).  
  
## <a name="implementing-wcf-services-as-dlls"></a>Implementando serviços WCF como DLLs  
 A implementação de um serviço WCF como uma DLL que é implantada no diretório \bin de um aplicativo Web permite que você reutilize o serviço fora do modelo de aplicativo Web, por exemplo, em um ambiente de teste que pode não ter Serviços de Informações da Internet (IIS) implantado.  
  
## <a name="service-hosts-in-iis-hosted-applications"></a>Hosts de serviço em aplicativos hospedados no IIS  
 Não use as APIs de auto-host imperativas para criar novos hosts de serviço que escutam em transportes de rede não suportados nativamente pelo ambiente de hospedagem do IIS (por exemplo, o IIS 6,0 para hospedar serviços TCP, pois a comunicação TCP não tem suporte nativo no IIS 6,0). Essa abordagem não é recomendada. Os hosts de serviço criados imperativamente não são conhecidos no ambiente de hospedagem do IIS. O ponto crítico é que o processamento feito por serviços criados imperativamente não é contabilizado pelo IIS quando determina se o pool de aplicativos de hospedagem está ocioso. O resultado é que os aplicativos que têm hosts de serviço criados de forma imperativa têm um ambiente de hospedagem do IIS que descarte agressivamente os processos de host do IIS.  
  
## <a name="uris-and-iis-hosted-endpoints"></a>URIs e pontos de extremidade hospedados no IIS  
 Os pontos de extremidade para um serviço hospedado pelo IIS devem ser configurados usando URIs (identificadores de recursos uniformes) relativos, não endereços absolutos. Isso garante que o endereço do ponto de extremidade esteja dentro do conjunto de endereços URI que pertencem ao aplicativo de hospedagem e garante que a ativação baseada em mensagem ocorra conforme o esperado.  
  
## <a name="state-management-and-process-recycling"></a>Gerenciamento de estado e reciclagem de processo  
 O ambiente de hospedagem do IIS é otimizado para serviços que não mantêm o estado local na memória. O IIS recicla o processo de host em resposta a uma variedade de eventos externos e internos, fazendo com que qualquer estado volátil armazenado exclusivamente na memória seja perdido. Os serviços hospedados no IIS devem armazenar seu estado externo ao processo (por exemplo, em um banco de dados) ou em um cache na memória que pode ser facilmente recriado se ocorrer um evento de reciclagem de aplicativo.  
  
> [!NOTE]
> Os protocolos que o WCF usa para confiabilidade e segurança da camada de mensagem usam o estado volátil na memória. As sessões confiáveis do WCF e as sessões de segurança podem ser encerradas inesperadamente devido a reciclagens do aplicativo. Os aplicativos hospedados no IIS que usam esses protocolos devem depender de algo diferente da chave de sessão fornecida pelo WCF para correlacionar o estado da camada de aplicativo (por exemplo, um constructo de camada de aplicativo ou cabeçalho de correlação personalizado) ou desabilitar a reciclagem de processo do IIS para o aplicativo hospedado.  
  
## <a name="optimizing-performance-in-middle-tier-scenarios"></a>Otimizando o desempenho em cenários de camada intermediária  
 Para um desempenho ideal em um *cenário de camada intermediária*— um serviço que chama outros serviços em resposta a mensagens de entrada — instancie o cliente do serviço WCF para o serviço remoto uma vez e reutilize-o em várias solicitações de entrada. A instanciação de clientes de serviço WCF é uma operação cara em relação à criação de uma chamada de serviço em uma instância de cliente pré-existente, e os cenários de camada intermediária produzem ganhos de desempenho distintos ao armazenar em cache clientes remotos entre solicitações. Os clientes de serviço WCF são thread-safe, portanto não é necessário sincronizar o acesso a um cliente em vários threads.  
  
 Cenários de camada intermediária também produzem ganhos de desempenho usando as APIs assíncronas geradas pela `svcutil /a` opção. A `/a` opção faz com que a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) gere `BeginXXX/EndXXX` métodos para cada operação de serviço, o que permite que as chamadas de longa duração para serviços remotos sejam feitas em threads em segundo plano.  
  
## <a name="wcf-in-multi-homed-or-multi-named-scenarios"></a>WCF em cenários de hospedagem múltipla ou vários nomes  
 Você pode implantar serviços WCF dentro de um Web farm do IIS, em que um conjunto de computadores compartilha um nome externo comum (como `http://www.contoso.com` ), mas que é endereçado individualmente por diferentes nomes de host (por exemplo, `http://www.contoso.com` pode direcionar o tráfego para dois computadores diferentes denominados `http://machine1.internal.contoso.com` e `http://machine2.internal.contoso.com` ). Esse cenário de implantação tem suporte total do WCF, mas requer configuração especial do site do IIS que hospeda serviços WCF para exibir o nome de host correto (externo) nos metadados do serviço (linguagem de descrição dos serviços Web).  
  
 Para garantir que o nome de host correto apareça no serviço de metadados do WCF gerado, configure a identidade padrão para o site do IIS que hospeda os serviços WCF para usar um nome de host explícito. Por exemplo, os computadores que residem dentro do `www.contoso.com` farm devem usar uma associação de site do IIS de *: 80: www. contoso. com para http e \* : 443: www. contoso. com para https.  
  
 Você pode configurar as associações de site do IIS usando o snap-in MMC (console de gerenciamento Microsoft) do IIS.  
  
## <a name="application-pools-running-in-different-user-contexts-overwrite-assemblies-from-other-accounts-in-the-temporary-folder"></a>Pools de aplicativos em execução em diferentes contextos de usuário substituem assemblies de outras contas na pasta temporária  
 Para garantir que os pools de aplicativos executados em diferentes contextos de usuário não possam substituir assemblies de outras contas na pasta Temporary ASP.NET Files, use diferentes identidades e pastas temporárias para aplicativos diferentes. Por exemplo, se você tiver dois aplicativos virtuais/Application1 e/Application2, poderá criar dois pools de aplicativos, A e B, com duas identidades diferentes. O pool de aplicativos A pode ser executado em uma identidade de usuário (Usuário1), enquanto o pool de aplicativos B pode ser executado em outra identidade de usuário (Usuário2) e configurar o/Application1 para usar um e/Application2 para usar B.  
  
 Em Web. config, você pode configurar a pasta temporária usando \<system.web/compilation/@tempFolder> . Para/Application1, ele pode ser "c:\tempForUser1" e, para Application2, pode ser "c:\tempForUser2". Conceda permissão de gravação correspondente a essas pastas para as duas identidades.  
  
 Em seguida, o User2 não pode alterar a pasta de geração de código para/Application2 (em c:\tempForUser1).  
  
## <a name="enabling-asynchronous-processing"></a>Habilitando o processamento assíncrono  
 Por padrão, as mensagens enviadas para um serviço WCF hospedados no IIS 6,0 e versões anteriores são processadas de maneira síncrona. ASP.NET chama o WCF em seu próprio thread (o thread de trabalho do ASP.NET) e o WCF usa outro thread para processar a solicitação. O WCF mantém-se no thread de trabalho do ASP.NET até concluir seu processamento. Isso leva ao processamento síncrono de solicitações. O processamento de solicitações assincronamente permite maior escalabilidade porque reduz o número de threads necessários para processar uma solicitação – o WCF não mantém o thread ASP.NET durante o processamento da solicitação. O uso do comportamento assíncrono não é recomendado para computadores que executam o IIS 6,0 porque não há nenhuma maneira de limitar as solicitações de entrada que abrem o servidor para ataques de dos ( *negação de serviço* ). A partir do IIS 7,0, foi introduzido um limite de solicitação simultâneo: `[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\2.0.50727.0]"MaxConcurrentRequestsPerCpu` . Com essa nova limitação, é seguro usar o processamento assíncrono.  Por padrão, no IIS 7,0, o manipulador assíncrono e o módulo são registrados. Se isso tiver sido desativado, você poderá habilitar manualmente o processamento assíncrono de solicitações no arquivo Web. config do seu aplicativo. As configurações usadas dependem de sua `aspNetCompatibilityEnabled` configuração. Se você tiver `aspNetCompatibilityEnabled` definido como `false` , configure o `System.ServiceModel.Activation.ServiceHttpModule` conforme mostrado no trecho de configuração a seguir.  
  
```xml  
<system.serviceModel>  
    <serviceHostingEnvironment aspNetCompatibilityEnabled="false" />
  </system.serviceModel>  
  <system.webServer>  
    <modules>  
      <remove name="ServiceModel"/>  
      <add name="ServiceModel"
           preCondition="integratedMode,runtimeVersionv2.0"
           type="System.ServiceModel.Activation.ServiceHttpModule, System.ServiceModel,Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>  
    </modules>  
    </system.webServer>  
```  
  
 Se você tiver `aspNetCompatibilityEnabled` definido como `true` , configure o `System.ServiceModel.Activation.ServiceHttpHandlerFactory` conforme mostrado no trecho de configuração a seguir.  
  
```xml  
<system.serviceModel>  
    <serviceHostingEnvironment aspNetCompatibilityEnabled="true" />
  </system.serviceModel>  
  <system.webServer>  
    <handlers>  
          <clear/>  
          <add name="TestAsyncHttpHandler"
               path="*.svc"
               verb="*"
               type="System.ServiceModel.Activation.ServiceHttpHandlerFactory, System.ServiceModel, Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
               />  
    </handlers>
  </system.webServer>  
```  
  
## <a name="see-also"></a>Confira também

- [Exemplos de Hospedagem de serviço](../samples/hosting.md)
- [Recursos de hospedagem do Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
