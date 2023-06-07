---
title: Implantando um Serviço WCF hospedado do Internet Information Services dos Serviços de Informações da Internet
description: Saiba mais sobre as tarefas necessárias para desenvolver e implantar um serviço WCF hospedado no IIS, começando com a verificação da instalação do componente
ms.date: 03/30/2017
ms.assetid: 04ebd329-3fbd-44c3-b3ab-1de3517e27d7
ms.openlocfilehash: 886fd9b8d8cf3059b1fd8679c5dd89ee015f2adf
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245087"
---
# <a name="deploying-an-internet-information-services-hosted-wcf-service"></a>Implantando um Serviço WCF hospedado do Internet Information Services dos Serviços de Informações da Internet

O desenvolvimento e a implantação de um serviço Windows Communication Foundation (WCF) hospedado no Serviços de Informações da Internet (IIS) consiste nas seguintes tarefas:

- Verifique se o IIS, o ASP.NET, o WCF e o componente de ativação do WCF estão corretamente instalados e registrados.

- Crie um novo aplicativo do IIS ou reutilize um aplicativo ASP.NET existente.

- Crie um arquivo. svc para o serviço WCF.

- Implantar a implementação do serviço para o aplicativo do IIS.

- Configure o serviço WCF.

Para obter uma explicação detalhada da criação de um serviço WCF hospedado pelo IIS, consulte [como hospedar um serviço WCF no IIS](how-to-host-a-wcf-service-in-iis.md).

## <a name="ensure-that-iis-aspnet-and-wcf-are-correctly-installed-and-registered"></a>Verificar se o IIS, o ASP.NET, o WCF estão corretamente instalados e registrados

O WCF, o IIS e o ASP.NET devem ser instalados para que os serviços WCF hospedados pelo IIS funcionem corretamente. Os procedimentos para instalar o WCF (como parte do .NET Framework), ASP.NET e IIS variam de acordo com o seu sistema operacional. Para obter mais informações sobre como instalar o WCF e o .NET Framework, consulte [instalar o .NET Framework para desenvolvedores](../../install/guide-for-developers.md). Para instalar o IIS no Windows 10, abra **programas e recursos** no **painel de controle** e selecione **Ativar ou desativar recursos do Windows**. Em **recursos do Windows**, selecione **serviços de informações da Internet** e, em seguida, escolha **OK**.

![Recursos do Windows com IIS realçado](./media/windows-features-iis.png)

As instruções para instalar o IIS em outros sistemas operacionais podem ser encontradas em [instalar o IIS no Windows Vista e no Windows 7](/iis/install/installing-iis-7/installing-iis-on-windows-vista-and-windows-7) e [instalar o IIS 8,5 no Windows Server 2012 R2](/iis/install/installing-iis-85/installing-iis-85-on-windows-server-2012-r2).

O processo de instalação para .NET Framework registrará automaticamente o WCF com o IIS se o IIS já estiver presente no computador. Se o IIS for instalado após a .NET Framework, uma etapa adicional será necessária para registrar o WCF com o IIS e o ASP.NET. Você pode fazer isso da seguinte maneira, dependendo do sistema operacional:

- Windows 7 e Windows Server 2003: Use a ferramenta [ServiceModelReg.exe (ferramenta de registro do ServiceModel)](../servicemodelreg-exe.md) para registrar o WCF com o IIS. Para usar essa ferramenta, digite **ServiceModelReg.exe/i/x** no [prompt de comando do desenvolvedor para Visual Studio](../../tools/developer-command-prompt-for-vs.md).

- Windows 7: por fim, você deve verificar se o ASP.NET está configurado para usar o .NET Framework versão 4 ou posterior. Faça isso executando a ferramenta ASPNET_Regiis com a `–i` opção. Para obter mais informações, consulte [ferramenta de registro do IIS ASP.net](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/k6h9cz8h(v=vs.90)).

## <a name="create-a-new-iis-application-or-reuse-an-existing-aspnet-application"></a>Criar um novo aplicativo do IIS ou reutilizar um existente do ASP.NET

Os serviços WCF hospedados pelo IIS devem residir dentro de um aplicativo IIS. Você pode criar um novo aplicativo do IIS para hospedar exclusivamente os serviços WCF. Como alternativa, você pode implantar um serviço WCF em um aplicativo existente que já hospeda conteúdo ASP.NET 2,0 (como páginas. aspx e ASP.NET Web Services [ASMX]). Para obter mais informações sobre essas opções, consulte as seções "hospedando o WCF lado a lado com ASP.NET" e "hospedando serviços WCF no modo de compatibilidade ASP.NET" nos [Serviços WCF e ASP.net](wcf-services-and-aspnet.md).

Observe que o IIS 6,0 e versões posteriores reiniciam periodicamente um aplicativo de programação orientado a objeto isolado. O valor padrão é 1740 minutos. O valor máximo valor com suporte é 71.582 minutos. Esse reinício pode ser desabilitado. Para obter mais informações sobre essa propriedade, consulte o [PeriodicRestartTime](https://docs.microsoft.com/previous-versions/iis/6.0-sdk/ms525914(v=vs.90)).

## <a name="create-an-svc-file-for-the-wcf-service"></a>Criar um arquivo .svc para o serviço WCF

Os serviços WCF hospedados no IIS são representados como arquivos de conteúdo especiais (arquivos. svc) dentro do aplicativo IIS. Esse modelo é semelhante à maneira como as páginas ASMX são representadas dentro de um aplicativo do IIS como arquivos .asmx. Um arquivo. svc contém uma diretiva de processamento específica do WCF ([ \@ ServiceHost](../../configure-apps/file-schema/wcf-directive/servicehost.md)) que permite que a infraestrutura de hospedagem do WCF ative serviços hospedados em resposta a mensagens de entrada. A sintaxe mais comum para um arquivo .svc está na instrução a seguir.

`<% @ServiceHost Service="MyNamespace.MyServiceImplementationTypeName" %>`

Ele consiste na diretiva [ \@ ServiceHost](../../configure-apps/file-schema/wcf-directive/servicehost.md) e em um único atributo, `Service` . O valor do atributo `Service` é o nome do tipo CLR (Common Language Runtime) da implementação do serviço. Usar essa diretiva é basicamente equivalente a criar um host de serviço usando o código a seguir.

```csharp
new ServiceHost( typeof( MyNamespace.MyServiceImplementationTypeName ) );
```

A configuração de hospedagem adicional, como a criação de uma lista de endereços de base para o serviço também pode ser feita. Você também pode usar um <xref:System.ServiceModel.Activation.ServiceHostFactory> personalizado para estender a política para usar com soluções de hospedagem personalizadas. Os aplicativos IIS que hospedam os serviços WCF não são responsáveis por gerenciar a criação e o tempo de vida de <xref:System.ServiceModel.ServiceHost> instâncias. A infraestrutura de hospedagem do WCF gerenciada cria a <xref:System.ServiceModel.ServiceHost> instância necessária dinamicamente quando a primeira solicitação é recebida para o arquivo. svc. A instância não é liberada até que seja fechada explicitamente pelo código ou quando o aplicativo for reciclado.

Para obter mais informações sobre a sintaxe para arquivos. svc, consulte [ \@ ServiceHost](../../configure-apps/file-schema/wcf-directive/servicehost.md).

## <a name="deploy-the-service-implementation-to-the-iis-application"></a>Implantar a implementação do serviço para o aplicativo do IIS

Os serviços WCF hospedados no IIS usam o mesmo modelo de compilação dinâmica que o ASP.NET 2,0. Assim como acontece com o ASP.NET, você pode implantar o código de implementação para serviços WCF hospedados pelo IIS de várias maneiras em vários locais, da seguinte maneira:

- Como um arquivo .dll pré-compilado localizado no GAC (cache de assembly global) ou no diretório \bin do aplicativo. Os binários pré-compilados não são atualizados até que uma nova versão da biblioteca de classe seja implantada.

- Como arquivos de origem não compilados localizados no diretório \ App_Code do aplicativo. Os arquivos de origem localizados nesse diretório são exigidos dinamicamente ao processar a primeira solicitação do aplicativo. As alterações nos arquivos no diretório \App_Code fazem o aplicativo inteiro ser reciclado e recompilado quando a próxima solicitação é recebida.

- Como código não compilado colocado diretamente no arquivo. svc. O código de implementação também pode ser colocado embutido no arquivo. svc do serviço, após a \@ diretiva ServiceHost. As alterações ao código embutido fazem o aplicativo ser reciclado e recompilado quando a próxima solicitação é recebida.

Para obter mais informações sobre o modelo de compilação do ASP.NET 2,0, consulte [visão geral da compilação do ASP.net](https://docs.microsoft.com/previous-versions/aspnet/ms178466(v=vs.100)).

## <a name="configure-the-wcf-service"></a>Configurar o serviço WCF

Os serviços WCF hospedados pelo IIS armazenam suas configurações no arquivo de Web.config de aplicativos. Os serviços hospedados pelo IIS usam os mesmos elementos de configuração e a mesma sintaxe que os serviços WCF hospedados fora do IIS. No entanto, as seguintes restrições são exclusivas para o ambiente de hospedagem do IIS:

- Endereços de base para serviços hospedados no IIS.

- Os aplicativos que hospedam serviços WCF fora do IIS podem controlar o endereço base dos serviços que eles hospedam passando um conjunto de URIs de endereço base para o <xref:System.ServiceModel.ServiceHost> Construtor ou fornecendo um [\<host>](../../configure-apps/file-schema/wcf/host.md) elemento na configuração do serviço. Os serviços hospedados no IIS não têm a capacidade de controlar seu endereço de base; o endereço de base de um serviço hospedado no IIS é o endereço de seu arquivo .svc.

### <a name="endpoint-addresses-for-iis-hosted-services"></a>Endereços de ponto de extremidade para serviços hospedados no IIS

Quando estão hospedados no IIS, os endereços de ponto de extremidade são sempre considerados relativos para o endereço do arquivo .svc que representa o serviço. Por exemplo, se o endereço base de um serviço WCF for `http://localhost/Application1/MyService.svc` com a seguinte configuração de ponto de extremidade:

```xml
<endpoint address="anotherEndpoint" />
```

Isso fornece um ponto de extremidade que pode ser acessado em `http://localhost/Application1/MyService.svc/anotherEndpoint` .

Da mesma forma, o elemento de configuração do ponto de extremidade que usa uma cadeia de caracteres vazia como o endereço relativo fornece um ponto de extremidade acessível em `http://localhost/Application1/MyService.svc` , que é o endereço base.

```xml
<endpoint address="" />
```

Você sempre deve usar endereços de ponto de extremidade relativos para pontos de extremidade de serviço hospedados no IIS. Fornecer um endereço de ponto de extremidade totalmente qualificado (por exemplo, `http://localhost/MyService.svc` ) pode levar a erros na implantação do serviço se o endereço do ponto de extremidade não apontar para o aplicativo IIS que hospeda o serviço expondo o ponto de extremidade. Usar endereços de ponto de extremidade relativos para serviços hospedados evita esses conflitos em potencial.

### <a name="available-transports"></a>Transportes disponíveis

Os serviços WCF hospedados no IIS 5,1 e no IIS 6,0 são restritos ao uso de comunicação baseada em HTTP. Nessas plataformas do IIS, configurar um serviço hospedado para usar uma associação não HTTP resulta em um erro durante a ativação do serviço. Para o IIS 7,0, os transportes com suporte incluem HTTP, net. TCP, net. pipe, net. MSMQ e MSMQ. FormatName para compatibilidade com versões anteriores com aplicativos MSMQ existentes.

### <a name="http-transport-security"></a>Segurança de transporte de HTTP

Os serviços WCF hospedados pelo IIS podem usar a segurança de transporte HTTP (por exemplo, esquemas de autenticação HTTPS e HTTP, como a autenticação básica, Digest e integrada do Windows), desde que o diretório virtual do IIS que contém o serviço dê suporte a essas configurações. As configurações de segurança de transporte de HTTP em uma associação do ponto de extremidade hospedado devem corresponder às configurações de segurança de transporte no diretório virtual do IIS que as contêm.

Por exemplo, um ponto de extremidade WCF configurado para usar a autenticação digest HTTP deve residir em um diretório virtual do IIS que também esteja configurado para permitir a autenticação digest HTTP. As combinações sem correspondência das configurações do IIS e das configurações do ponto de extremidade do WCF resultam em um erro durante a ativação do serviço.

## <a name="see-also"></a>Veja também

- [Hospedagem no Internet Information Services](hosting-in-internet-information-services.md)
- [Práticas recomendadas de hospedagem de Serviços de Informações da Internet](internet-information-services-hosting-best-practices.md)
- [Recursos de hospedagem do Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
