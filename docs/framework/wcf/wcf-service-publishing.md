---
title: Publicação de serviço do WCF
description: A publicação do serviço WCF ajuda a implantar seu aplicativo em um ambiente de produção para fins de teste.
ms.date: 03/30/2017
ms.assetid: c806b253-cd47-4b96-b831-e73cbf08808f
ms.openlocfilehash: 99798b75e1dc01c8db361f4d8d1f162c7f7617b1
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245668"
---
# <a name="wcf-service-publishing"></a>Publicação de serviço do WCF

A publicação de serviço do Windows Communication Foundation (WCF) ajuda você a progredir do ambiente de desenvolvimento antecipado fornecido pelo host de serviço WCF e pelo cliente de teste do WCF para realmente implantar o aplicativo em um ambiente de produção para fins de teste. Antes de se comprometer com um plano de implantação final, você pode usar a publicação do serviço Windows Communication Foundation (WCF) para verificar se o serviço WCF está funcionando corretamente e se está pronto para ser publicado. Você também pode optar por implantar suas bibliotecas de serviço WCF em vários locais de destino para teste.

## <a name="supported-services-and-target-locations"></a>Locais de destino e serviços com suporte

A publicação do serviço WCF dá suporte à publicação de serviços WCF criados a partir do conjunto de modelos de biblioteca de serviços WCF e seus modelos de item correspondentes, que incluem o seguinte:

- Modelo de biblioteca de serviço do WCF com modelo de item.

- Biblioteca de serviço de distribuição.

Você pode encontrar esses modelos de serviço escolhendo **arquivo**  >  **novo projeto** > [**Visual Basic** ou **Visual C#**] > **WCF**. Para outros modelos do WCF neste local (incluindo o aplicativo de serviço de fluxo de trabalho do WCF e o aplicativo de serviço WCF), você pode publicar usando [a publicação de um clique para aplicativos Web](https://docs.microsoft.com/previous-versions/aspnet/dd465337(v=vs.110)).

O serviço pode ser publicado nos seguintes locais de destino.

- IIS local.

- Sistema de arquivos.

- Site FTP.

## <a name="using-wcf-service-publishing"></a>Usando a publicação do serviço WCF

Execute as seguintes etapas para implantar uma implementação de serviço:

1. Abra o Visual Studio com privilégios elevados (clique com o botão direito do mouse no executável e escolha **Executar como administrador** para abri-lo).  Se você estiver usando o IIS 7,0 ou posterior, verifique se você instalou o componente "metabase do IIS e compatibilidade de configuração do IIS6" usando "ativar ou desativar recursos do Windows" no painel de controle.

2. Abra um projeto de serviço, selecione **Compilar**  >  ** \<Project Name> publicar** no menu principal ou clique com o botão direito do mouse no projeto no **Gerenciador de soluções** e clique em **publicar**.

3. A janela **publicar** é exibida. Clique em **..**.. botão para especificar o local de destino no qual o serviço deve ser implantado. Você pode optar por implantar o aplicativo no IIS local, no sistema de arquivos ou no site FTP. Se estiver implantando o aplicativo no IIS local, você poderá selecionar seu site e criar seu aplicativo Web sob ele, clicando no ícone **criar novo aplicativo Web** no canto superior direito.

4. Depois de clicar em **publicar** na janela principal, o Visual Studio implanta o aplicativo no local de destino especificado e copia os arquivos de Web.config,. svc e assembly para o diretório de destino. . O nome de. svc será "ProjectName. ServiceName. svc". Depois que o serviço for publicado com êxito, você poderá encontrar um hotlink na janela de saída do Visual Studio, que é semelhante a "conectando-se a `http://localhost/WebApplicationFolderName...` ". Você pode pressionar CTRL e clicar no link para abrir uma página do navegador dentro do Visual Studio para exibir a estrutura do diretório de serviço.

     Se você não puder navegar até o site, talvez o navegador de diretórios não esteja habilitado no IIS. Siga as dicas na seção "coisas que você pode tentar" para habilitá-la. Como alternativa, você pode digitar diretamente `http://localhost/WebApplicationFolderName/ProjectName.ServiceName.svc` para exibir sua página de serviço.

Você pode usar **publicar** para especificar se deseja copiar o assembly, a configuração e o arquivo. svc para todos os serviços definidos no projeto para o local de destino e substituir os arquivos existentes no destino.

Se você optar por implantar seu aplicativo no IIS local, poderá encontrar erros relacionados à instalação do IIS. Verifique se o IIS está instalado corretamente. Você pode inserir `http://localhost` na barra de endereços do navegador e verificar se a página padrão do IIS é exibida. Em alguns casos, os problemas também podem ser causados pelo registro impróprio de ASP.NET ou WCF no IIS. Você pode abrir o Prompt de Comando do Desenvolvedor para o Visual Studio e executar o comando `aspnet_regiis.exe -ir` para corrigir problemas de registro do ASP.net ou executar o comando `ServiceModelReg.exe –ia` para corrigir problemas de registro do WCF.

## <a name="files-generated-for-publishing"></a>Arquivos gerados para publicação
 Antes que uma biblioteca de serviços WCF possa ser hospedada pela Web, os seguintes arquivos são gerados pela ferramenta: Arquivos de assembly, arquivo de Web.config e arquivo. svc. Todos os arquivos são copiados para o local de destino. Em seguida, o serviço é publicado.

### <a name="assembly-files"></a>Arquivos de assembly
 Quando você publica um serviço WCF usando essa ferramenta, o serviço é automaticamente compilado primeiro e os arquivos de assembly são gerados no projeto de serviço após a compilação.

### <a name="svc-file"></a>. Arquivo SVC
 A operação de publicação gera um arquivo *. svc para cada serviço WCF, se o arquivo existe ou não, para garantir a validade da versão. Há dois tipos diferentes de arquivos svc: um para a biblioteca de serviços do WCF e a biblioteca de serviços de distribuição e outro para a biblioteca de serviço de fluxo de trabalho de máquina de estado e sequencial. O \* arquivo. svc gerado é copiado para a pasta raiz no local de destino.

### <a name="webconfig-file"></a>Arquivo de Web.config
 Cada vez que um projeto de serviço é publicado em um local de destino específico, um arquivo de Web.config é criado.

 O arquivo Web.config gerado inclui seções da Web que são úteis para hospedagem na Web e o conteúdo de App.config para a biblioteca de serviços WCF com as seguintes alterações:

- O endereço base é excluído.

- As configurações no `<diagnostics>` elemento são excluídas para preservar as configurações de rastreamento da plataforma de destino.

## <a name="publishing-wcf-services-with-non-http-bindings-to-iis"></a>Publicando serviços WCF com associações não HTTP para o IIS
 Se você estiver usando o IIS 7.0 ou posterior, poderá publicar serviços WCF com associações não HTTP para o IIS. Você precisa fazer algumas configurações prévias. Para obter mais informações, consulte os tópicos em [hospedagem no serviço de ativação de processos do Windows](./feature-details/hosting-in-windows-process-activation-service.md).

## <a name="security"></a>Segurança
 A publicação no IIS local requer privilégio de administrador, pois o IIS requer a execução na conta de administrador. Se um usuário sem privilégio de administrador abrir a publicação do serviço WCF, o IIS não estará disponível como um local de destino. A publicação no sistema de arquivos ou no site FTP funciona sem privilégios de administrador.

## <a name="see-also"></a>Veja também

- [Modelos do Visual Studio do WCF](wcf-vs-templates.md)
- [Host de serviço do WCF (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)
- [Cliente de Teste do WCF (WcfTestClient.exe)](wcf-test-client-wcftestclient-exe.md)
