---
title: Serviços WCF e ASP.NET
description: Saiba como hospedar serviços WCF lado a lado com o ASP.NET e hospedá-los no modo de compatibilidade ASP.NET.
ms.date: 03/30/2017
ms.assetid: b980496a-f0b0-4319-8e55-a0f0fa32da70
ms.openlocfilehash: 1d7401f6a326bc50923123acf803e26ce8238415
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246409"
---
# <a name="wcf-services-and-aspnet"></a>Serviços WCF e ASP.NET

Este tópico discute a hospedagem de serviços do Windows Communication Foundation (WCF) lado a lado com o ASP.NET e sua hospedagem no modo de compatibilidade do ASP.NET.

## <a name="hosting-wcf-side-by-side-with-aspnet"></a>Hospedando o WCF lado a lado com o ASP.NET

Os serviços WCF hospedados no Serviços de Informações da Internet (IIS) podem ser localizados com. Páginas ASPX e serviços Web ASMX dentro de um domínio de aplicativo único e comum. O ASP.NET fornece serviços comuns de infraestrutura, como o gerenciamento de AppDomain e a compilação dinâmica para o WCF e o tempo de execução HTTP do ASP.NET. A configuração padrão do WCF é lado a lado com ASP.NET.

![Captura de tela mostrando serviços WCF e ASP .NET: estado de compartilhamento.](./media/wcf-services-and-aspnet/windows-communication-foundation-services-asp-dotnet-configuration.gif)

O tempo de execução HTTP ASP.NET manipula solicitações ASP.NET, mas não participa do processamento de solicitações destinadas a serviços WCF, embora esses serviços estejam hospedados no mesmo AppDomain que o conteúdo ASP.NET. Em vez disso, o modelo de serviço do WCF intercepta mensagens endereçadas aos serviços WCF e as roteia por meio da pilha de transporte/canal do WCF.

Os resultados do modelo lado a lado são os seguintes:

- Os serviços ASP.NET e WCF podem compartilhar o estado do AppDomain. Como as duas estruturas podem coexistir no mesmo AppDomain, o WCF também pode compartilhar o estado do AppDomain com ASP.NET (incluindo variáveis estáticas, eventos e assim por diante).

- Os serviços WCF se comportam de forma consistente, independentemente do ambiente de hospedagem e do transporte. O tempo de execução de HTTP ASP.NET é intencionalmente acoplado ao ambiente de hospedagem do IIS/ASP. NET e à comunicação HTTP. Por outro lado, o WCF foi projetado para se comportar consistentemente em ambientes de hospedagem (o WCF se comporta de forma consistente tanto dentro quanto fora do IIS) e no transporte (um serviço hospedado no IIS 7,0 e posterior tem comportamento consistente em todos os pontos de extremidade que ele expõe, mesmo que alguns desses pontos de extremidade usem protocolos diferentes de HTTP).

- Dentro de um AppDomain, os recursos implementados pelo tempo de execução HTTP se aplicam ao conteúdo ASP.NET, mas não ao WCF. Muitos recursos específicos de HTTP da plataforma de aplicativo ASP.NET não se aplicam aos serviços WCF hospedados dentro de um AppDomain que contém conteúdo ASP.NET. Exemplos desses recursos incluem o seguinte:

  - HttpContext: <xref:System.Web.HttpContext.Current%2A> é sempre `null` quando acessado de dentro de um serviço WCF. Use <xref:System.ServiceModel.Channels.RequestContext> em vez disso.

  - Autorização baseada em arquivo: o modelo de segurança do WCF não permite a ACL (lista de controle de acesso) aplicada ao arquivo. svc do serviço ao decidir se uma solicitação de serviço está autorizada.

  - Autorização de URL baseada em configuração: da mesma forma, o modelo de segurança do WCF não adere a nenhuma regra de autorização baseada em URL especificada no elemento de configuração System. Web \<authorization> . Essas configurações serão ignoradas para solicitações do WCF se um serviço residir em um espaço de URL protegido pelo ASP. Regras de autorização de URL do NET.

  - Extensibilidade de HttpModule: a infraestrutura de hospedagem do WCF intercepta as solicitações do WCF quando o <xref:System.Web.HttpApplication.PostAuthenticateRequest> evento é gerado e não retorna o processamento para o pipeline HTTP ASP.net. Os módulos que são codificados para interceptar solicitações em estágios posteriores do pipeline não interceptam solicitações do WCF.

  - Representação ASP.NET: por padrão, as solicitações do WCF sempre são executadas como a identidade do processo do IIS, mesmo se ASP.NET estiver definido para habilitar a representação usando a opção de configuração System. Web \<identity impersonate="true" /> .

Essas restrições se aplicam somente aos serviços WCF hospedados no aplicativo IIS. O comportamento do conteúdo do ASP.NET não é afetado pela presença do WCF.

Os aplicativos WCF que exigem funcionalidade tradicionalmente fornecidas pelo pipeline HTTP devem considerar o uso de equivalentes do WCF, que são independentes de host e transporte:

- <xref:System.ServiceModel.OperationContext>em vez de <xref:System.Web.HttpContext> .

- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>em vez de ASP. Autorização de arquivo/URL da rede.

- <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector>ou canais em camadas personalizados em vez de módulos HTTP.

- Representação para cada operação usando o WCF em vez da representação de System. Web.

Como alternativa, você pode considerar a execução de seus serviços no modo de compatibilidade ASP.NET do WCF.

## <a name="hosting-wcf-services-in-aspnet-compatibility-mode"></a>Hospedando serviços WCF no modo de compatibilidade ASP.NET

Embora o modelo do WCF seja projetado para se comportar consistentemente em ambientes de hospedagem e transportes, geralmente há cenários em que um aplicativo não requer esse grau de flexibilidade. O modo de compatibilidade do ASP.NET do WCF é adequado para cenários que não exigem a capacidade de hospedar fora do IIS ou para se comunicar por protocolos diferentes de HTTP, mas que usam todos os recursos da plataforma de aplicativo Web do ASP.NET.

Diferentemente da configuração lado a lado padrão, em que a infraestrutura de hospedagem do WCF intercepta mensagens do WCF e as roteia do pipeline HTTP, os serviços WCF em execução no modo de compatibilidade ASP.NET participam totalmente do ciclo de vida da solicitação HTTP ASP.NET. No modo de compatibilidade, os serviços WCF usam o pipeline HTTP por meio de uma <xref:System.Web.IHttpHandler> implementação, semelhante à forma como as solicitações para páginas aspx e serviços Web ASMX são manipuladas. Como resultado, o WCF se comporta de forma idêntica a ASMX em relação aos seguintes recursos de ASP.NET:

- <xref:System.Web.HttpContext>: Os serviços WCF em execução no modo de compatibilidade ASP.NET podem acessar <xref:System.Web.HttpContext.Current%2A> e seu estado associado.

- Autorização baseada em arquivo: os serviços WCF em execução no modo de compatibilidade ASP.NET podem ser protegidos ao anexar ACLs (listas de controle de acesso) do sistema de arquivos ao arquivo. svc do serviço.

- Autorização de URL configurável: ASP. As regras de autorização de URL da rede são impostas para solicitações do WCF quando o serviço WCF está sendo executado no modo de compatibilidade do ASP.NET.

- <xref:System.Web.HttpModuleCollection>extensibilidade: como os serviços WCF em execução no modo de compatibilidade ASP.NET participam totalmente do ciclo de vida da solicitação HTTP ASP.NET, qualquer módulo HTTP configurado no pipeline HTTP é capaz de operar em solicitações do WCF antes e depois da invocação de serviço.

- Representação ASP.NET: os serviços WCF são executados usando a identidade atual do thread ASP.NET representado, que pode ser diferente da identidade do processo do IIS se a representação do ASP.NET tiver sido habilitada para o aplicativo. Se a representação de ASP.NET e a representação do WCF estiverem ambas habilitadas para uma operação de serviço específica, a implementação do serviço será executada por fim usando a identidade obtida do WCF.

O modo de compatibilidade de ASP.NET do WCF é habilitado no nível do aplicativo por meio da seguinte configuração (localizada no arquivo de Web.config do aplicativo):

```xml
<system.serviceModel>
    <serviceHostingEnvironment aspNetCompatibilityEnabled="true" />
</system.serviceModel>
```

Esse valor padrão será `false` se não for especificado. O valor de `false` indica que todos os serviços WCF em execução no aplicativo não serão executados no modo de compatibilidade ASP.net.

Como o modo de compatibilidade ASP.NET implica semântica de processamento de solicitação que são fundamentalmente diferentes do padrão do WCF, as implementações de serviço individuais têm a capacidade de controlar se são executadas dentro de um aplicativo para o qual o modo de compatibilidade do ASP.NET foi habilitado. Os serviços podem usar o <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> para indicar se eles dão suporte ao modo de compatibilidade ASP.net. O valor padrão para esse atributo é <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed> .

```csharp
[AspNetCompatibilityRequirements(RequirementsMode = AspNetCompatibilityRequirementsMode.Allowed)]
public class CalculatorService : ICalculatorSession
{//Implement calculator service methods.}
```

A tabela a seguir ilustra como a configuração do modo de compatibilidade em todo o aplicativo interage com o nível de suporte indicado do serviço individual:

|Configuração do modo de compatibilidade em todo o aplicativo|[AspNetCompatibilityRequirementsMode]<br /><br /> Configuração|Resultado observado|
|--------------------------------------------------|---------------------------------------------------------|---------------------|
|aspNetCompatibilityEnabled = " `true` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Required>|O serviço é ativado com êxito.|
|aspNetCompatibilityEnabled = " `true` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed>|O serviço é ativado com êxito.|
|aspNetCompatibilityEnabled = " `true` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.NotAllowed>|Um erro de ativação ocorre quando o serviço recebe uma mensagem.|
|aspNetCompatibilityEnabled = " `false` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Required>|Um erro de ativação ocorre quando o serviço recebe uma mensagem.|
|aspNetCompatibilityEnabled = " `false` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed>|O serviço é ativado com êxito.|
|aspNetCompatibilityEnabled = " `false` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.NotAllowed>|O serviço é ativado com êxito.|

> [!NOTE]
> O IIS 7,0 e o WAS permite que os serviços WCF se comuniquem por protocolos diferentes de HTTP. No entanto, os serviços WCF em execução em aplicativos que habilitaram o modo de compatibilidade ASP.NET não têm permissão para expor pontos de extremidade não HTTP. Essa configuração gera uma exceção de ativação quando o serviço recebe sua primeira mensagem.

Para obter mais informações sobre como habilitar o modo de compatibilidade ASP.NET para serviços WCF, consulte <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode> e o exemplo de [compatibilidade ASP.net](../samples/aspnet-compatibility.md) .

## <a name="see-also"></a>Veja também

- <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute>
- [Recursos de hospedagem do Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
