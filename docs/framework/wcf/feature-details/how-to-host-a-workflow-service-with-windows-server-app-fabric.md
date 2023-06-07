---
title: Como hospedar um serviço de fluxo de trabalho com o Windows Server App Fabric
ms.date: 03/30/2017
ms.assetid: 83b62cce-5fc2-4c6d-b27c-5742ba3bac73
ms.openlocfilehash: 519c76e3e46e01b5e8c696234e39fefbb9f8ad06
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593302"
---
# <a name="how-to-host-a-workflow-service-with-windows-server-app-fabric"></a>Como hospedar um serviço de fluxo de trabalho com o Windows Server App Fabric

A hospedagem de serviços de fluxo de trabalho no app Fabric é semelhante à hospedagem em IIS/WAS. A única diferença é que as ferramentas do App Fabric fornecem para implantar, monitorar e gerenciar serviços de fluxo de trabalho. Este tópico usa o serviço de fluxo de trabalho criado na [criação de um serviço de fluxo de trabalho de execução longa](creating-a-long-running-workflow-service.md). Esse tópico orientará você na criação de um serviço de fluxo de trabalho. Este tópico explicará como hospedar o serviço de fluxo de trabalho usando o app Fabric. Para obter mais informações sobre a malha de aplicativos do Windows Server, consulte a [documentação do Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ff384253(v=azure.10)). Antes de concluir as etapas abaixo, verifique se você tem o Windows Server app Fabric instalado.  Para fazer isso, abra Serviços de Informações da Internet (inetmgr. exe), clique no nome do servidor na exibição **conexões** , clique em sites e clique em **site padrão**. No lado direito da tela, você verá uma seção chamada **app Fabric**. Se você não vir esta seção (ela estará na parte superior do painel direito), você não tem o app Fabric instalado. Para obter mais informações sobre como instalar o Windows Server app Fabric, consulte [instalando o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee790960(v=azure.10)).  
  
### <a name="creating-a-simple-workflow-service"></a>Criando um serviço de fluxo de trabalho simples  
  
1. Abra o Visual Studio 2012 e carregue a solução OrderProcessing que você criou no tópico [criando um serviço de fluxo de trabalho de execução longa](creating-a-long-running-workflow-service.md) .  
  
2. Clique com o botão direito do mouse no projeto **OrderService** e selecione **Propriedades** e selecione a guia **Web** .  
  
3. Na seção **Iniciar ação** da página de propriedades, selecione **página específica** e digite Service1. xamlx na caixa de edição.  
  
4. Na seção **servidores** da página de propriedades, selecione **usar servidor Web local do IIS** e digite a seguinte URL: `http://localhost/OrderService` .  
  
5. Clique no botão **criar diretório virtual** . Isso criará um novo diretório virtual e configurará o projeto para copiar os arquivos necessários para o diretório virtual quando o projeto for compilado.  Como alternativa, você pode copiar manualmente o. xamlx, o Web. config e todas as DLLs necessárias para o diretório virtual.  
  
### <a name="configuring-a-workflow-service-hosted-in-windows-server-app-fabric"></a>Configurando um serviço de fluxo de trabalho hospedado no Windows Server app Fabric  
  
1. Abra o Gerenciador de Serviços de Informações da Internet (inetmgr. exe).  
  
2. Navegue até o diretório virtual OrderService no painel **conexões** .  
  
3. Clique com o botão direito do mouse em OrderService e selecione **gerenciar serviços WCF e WF**, **Configurar...**. A caixa de diálogo **Configurar WCF e WF para o aplicativo** é exibida.  
  
4. Selecione a guia **geral** para exibir informações gerais sobre o aplicativo, conforme mostrado na captura de tela a seguir.  
  
     ![Guia Geral da caixa de diálogo Configuração da Malha de Aplicativos](media/appfabricconfiguration-general.gif "AppFabricConfiguration-geral")  
  
5. Selecione a guia **monitoramento** . Isso mostra várias configurações de monitoramento, conforme mostrado na captura de tela a seguir.  
  
     ![Guia de monitoramento da configuração da malha de aplicativos](media/appfabricconfiguration-monitoring.gif "AppFabricConfiguration-monitoramento")  
  
     Para obter mais informações sobre como configurar o monitoramento do serviço de fluxo de trabalho no app Fabric, consulte [Configuring Monitoring with app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677384(v=azure.10)).  
  
6. Selecione a guia **persistência do fluxo de trabalho** . Isso permite que você configure seu aplicativo para usar o provedor de persistência padrão do App Fabric, conforme mostrado na captura de tela a seguir.  
  
     ![Persistência de &#45; de configuração do Fabric do aplicativo](media/appfabricconfiguration-persistence.gif "AppFabricConfiguration-persistência")  
  
     Para obter mais informações sobre como configurar a persistência de fluxo de trabalho no Windows Server app Fabric, consulte [Configuring Workflow Persistence in app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677353(v=azure.10)).  
  
7. Selecione a guia **Gerenciamento de host do fluxo de trabalho** . Isso permite que você especifique quando as instâncias de serviço de fluxo de trabalho ocioso devem ser descarregadas e persistidas, conforme mostrado na captura de tela a seguir.  
  
     ![Gerenciamento de host de fluxo de trabalho de configuração do App Fabric](media/appfabricconfiguration-management.gif "AppFabricConfiguration-gerenciamento")  
  
     Para obter mais informações sobre a configuração de gerenciamento de host de fluxo de trabalho, consulte [Configuring Workflow host Management in app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ff383424(v=azure.10)).  
  
8. Selecione a guia **início automático** . Isso permite que você especifique configurações de início automático para os serviços de fluxo de trabalho no aplicativo, conforme mostrado na captura de tela a seguir.  
  
     ![Captura de tela que mostra a configuração de inicialização automática do App Fabric&#45;.](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/app-fabric-auto-start-configuration.gif)  
  
     Para obter mais informações sobre como configurar o início automático, consulte [Configurando o início automático com o app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677261(v=azure.10)).  
  
9. Selecione a guia **limitação** . Isso permite que você defina as configurações de limitação para o serviço de fluxo de trabalho, conforme mostrado na captura de tela a seguir.  
  
     ![Captura de tela que mostra a configuração de limitação do App Fabric.](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/app-fabric-throttling-configuration.gif)  
  
     Para obter mais informações sobre como configurar a limitação, consulte [Configurando a limitação com o app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677261(v=azure.10)).  
  
10. Selecione a guia **segurança** . Isso permite que você defina as configurações de segurança para o aplicativo, conforme mostrado na captura de tela a seguir.  
  
     ![Configuração da segurança da malha de aplicativos](media/appfabricconfiguration-security.gif "AppFabricConfiguration-segurança")  
  
     Para obter mais informações sobre como configurar a segurança com o Windows Server app Fabric, consulte [Configurando a segurança com o app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677278(v=azure.10)).  
  
### <a name="using-windows-server-app-fabric"></a>Usando a malha de aplicativos do Windows Server  
  
1. Compile a solução para copiar os arquivos necessários para o diretório virtual.  
  
2. Clique com o botão direito do mouse no projeto OrderClient e selecione **depurar**, **inicie a nova instância** para iniciar o aplicativo cliente.  
  
3. O cliente será executado e o Visual Studio exibirá uma caixa de diálogo de **aviso de segurança de anexo** , clique no botão **não anexar** . Isso diz ao Visual Studio para não anexar ao processo do IIS para depuração.  
  
4. O aplicativo cliente chamará imediatamente o serviço de fluxo de trabalho e aguardará. O serviço de fluxo de trabalho ficará ocioso e persistirá. Você pode verificar isso iniciando Serviços de Informações da Internet (inetmgr. exe), navegando até OrderService no painel conexões e selecionando-o. Em seguida, clique no ícone do painel do App Fabric no painel à direita. Em instâncias persistentes do WF, você verá que há uma instância de serviço de fluxo de trabalho persistente, conforme mostrado na captura de tela a seguir.  
  
     ![Captura de tela que mostra o painel do App Fabric.](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/app-fabric-dashboard.gif)  
  
     O **histórico de instâncias do WF** lista informações sobre o serviço de fluxo de trabalho, como o número de ativações do serviço de fluxo de trabalho, o número de conclusões da instância do serviço de fluxo de trabalho e o número de instâncias de fluxo de trabalho com falhas. Em instâncias ativas ou ociosas, um link será exibido, clicando no link exibirá mais informações sobre as instâncias de fluxo de trabalho ocioso, conforme mostrado na captura de tela a seguir.  
  
     ![Captura de tela que mostra detalhes da instância de fluxo de trabalho persistente.](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/persisted-workflow-instance-detail.gif)  
  
     Para obter mais informações sobre os recursos do Windows Server app Fabric e como usá-los, consulte [recursos de hospedagem do Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))  
  
## <a name="see-also"></a>Confira também

- [Criando um serviço de fluxo de trabalho de execução longa](creating-a-long-running-workflow-service.md)
- [Recursos de hospedagem do Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
- [Instalando o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee790960(v=azure.10))
- [Documentação do Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ff384253(v=azure.10))
