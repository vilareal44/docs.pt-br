---
title: Configurando o Serviço de ativação de processos do Windows (WAS) para utilizar com o Windows Communication Foundation
ms.date: 03/30/2017
ms.assetid: 1d50712e-53cd-4773-b8bc-a1e1aad66b78
ms.openlocfilehash: 06d3a7bd798913b06d342ac09d12e736fc436b3c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597495"
---
# <a name="configuring-the-windows-process-activation-service-for-use-with-windows-communication-foundation"></a>Configurando o Serviço de ativação de processos do Windows (WAS) para utilizar com o Windows Communication Foundation
Este tópico descreve as etapas necessárias para configurar o serviço de ativação de processos do Windows (também conhecido como WAS) no Windows Vista para hospedar serviços de Windows Communication Foundation (WCF) que não se comunicam por protocolos de rede HTTP. As seções a seguir descrevem as etapas para essa configuração:  
  
- Instale (ou confirme a instalação do) os componentes de ativação do WCF necessários.  
  
- Crie um site do WAS com as associações de protocolo de rede que você deseja usar ou adicione uma nova associação de protocolo a um site existente.  
  
- Crie um aplicativo para hospedar seus serviços e habilite esse aplicativo para usar os protocolos de rede necessários.  
  
- Crie um serviço WCF que expõe um ponto de extremidade não HTTP.  
  
## <a name="configuring-a-site-with-non-http-bindings"></a>Configurando um site com associações não HTTP  
 Para usar uma associação não HTTP com o WAS, a associação do site deve ser adicionada à configuração do WAS. O repositório de configuração para WAS é o arquivo applicationHost. config, localizado no diretório%windir%\system32\inetsrv\config Esse repositório de configuração é compartilhado pelo WAS e pelo IIS 7,0.  
  
 applicationHost. config é um arquivo de texto XML que pode ser aberto com qualquer editor de texto padrão (como o bloco de notas). No entanto, a ferramenta de configuração de linha de comando do IIS 7,0 (appcmd. exe) é a maneira preferida de adicionar associações de site não HTTP.  
  
 O comando a seguir adiciona uma associação de site net. TCP ao site padrão usando AppCmd. exe (esse comando é inserido como uma única linha).  
  
```console  
appcmd.exe set site "Default Web Site" -+bindings.[protocol='net.tcp',bindingInformation='808:*']  
```  
  
 Esse comando adiciona a nova associação net. TCP ao site padrão adicionando a linha indicada abaixo ao arquivo applicationHost. config.  
  
```xml  
<sites>  
    <site name="Default Web Site" id="1">  
        <bindings>  
            <binding protocol="HTTP" bindingInformation="*:80:" />  
            //The following line is added by the command.  
            <binding protocol="net.tcp" bindingInformation="808:*" />  
        </bindings>  
    </site>  
</sites>  
```  
  
## <a name="enabling-an-application-to-use-non-http-protocols"></a>Habilitando um aplicativo para usar protocolos não HTTP  
 Você pode habilitar ou desabilitar a rede individual protocolsat o nível do aplicativo. O comando a seguir ilustra como habilitar os protocolos HTTP e net. TCP para um aplicativo que é executado no `Default Web Site` .  
  
```console  
appcmd.exe set app "Default Web Site/appOne" /enabledProtocols:net.tcp  
```  
  
 A lista de protocolos habilitados também pode ser definida no \<applicationDefaults> elemento da configuração XML do site armazenada em ApplicationHost. config.  
  
 O código XML a seguir de applicationHost. config ilustra um site associado a protocolos HTTP e não HTTP. A configuração adicional necessária para dar suporte a protocolos não HTTP é chamada com comentários.  
  
```xml  
<sites>  
    <site name="Default Web Site" id="1">  
      <application path="/">  
        <virtualDirectory path="/" physicalPath="D:\inetpub\wwwroot" />  
      </application>  
      <bindings>  
            <!-- The following two lines are added by the command. -->
            <binding protocol="HTTP" bindingInformation="*:80:" />  
            <binding protocol="net.tcp" bindingInformation="808:*" />  
       </bindings>  
    </site>  
    <siteDefaults>  
        <logFile
        customLogPluginClsid="{FF160663-DE82-11CF-BC0A-00AA006111E0}"  
          directory="D:\inetpub\logs\LogFiles" />  
        <traceFailedRequestsLogging
          directory="D:\inetpub\logs\FailedReqLogFiles" />  
    </siteDefaults>  
    <applicationDefaults
      applicationPool="DefaultAppPool"
      <!-- The following line is inserted by the command. -->
      enabledProtocols="http, net.tcp" />  
    <virtualDirectoryDefaults allowSubDirConfig="true" />  
</sites>  
```  
  
 Se você tentar ativar um serviço usando o WAS para ativação não HTTP e não tiver instalado e configurado, você poderá ver o seguinte erro:  
  
```output  
[InvalidOperationException: The protocol 'net.tcp' does not have an implementation of HostedTransportConfiguration type registered.]   System.ServiceModel.AsyncResult.End(IAsyncResult result) +15778592   System.ServiceModel.Activation.HostedHttpRequestAsyncResult.End(IAsyncResult result) +15698937   System.ServiceModel.Activation.HostedHttpRequestAsyncResult.ExecuteSynchronous(HttpApplication context, Boolean flowContext) +265   System.ServiceModel.Activation.HttpModule.ProcessRequest(Object sender, EventArgs e) +227   System.Web.SyncEventExecutionStep.System.Web.HttpApplication.IExecutionStep.Execute() +80   System.Web.HttpApplication.ExecuteStep(IExecutionStep step, Boolean& completedSynchronously) +171  
```  
  
 Se você vir esse erro, verifique se a ativação não HTTP está instalada e configurada corretamente. Para obter mais informações, consulte [como: instalar e configurar componentes de ativação do WCF](how-to-install-and-configure-wcf-activation-components.md).  
  
## <a name="building-a-wcf-service-that-uses-was-for-non-http-activation"></a>Criar um serviço WCF que usa o WAS para ativação não HTTP  
 Depois de executar as etapas para instalar e configurar o WAS (consulte [como instalar e configurar os componentes de ativação do WCF](how-to-install-and-configure-wcf-activation-components.md)), a configuração de um serviço para usar o was para a ativação é semelhante à configuração de um serviço hospedado no IIS.  
  
 Para obter instruções detalhadas sobre como criar um serviço WCF ativado, consulte [como hospedar um serviço WCF no was](how-to-host-a-wcf-service-in-was.md).  
  
## <a name="see-also"></a>Consulte também

- [Hosting in Windows Process Activation Service](hosting-in-windows-process-activation-service.md) (Hospedagem no Serviço de Ativação de Processos do Windows)
- [Recursos de hospedagem do Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
