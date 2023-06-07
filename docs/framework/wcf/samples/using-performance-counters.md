---
title: Utilizando contadores de desempenho
ms.date: 03/30/2017
ms.assetid: 00a787af-1876-473c-a48d-f52b51e28a3f
ms.openlocfilehash: 0b63cdc145ff8806c26b255500bcb2a132e9ef9f
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596494"
---
# <a name="using-performance-counters"></a>Utilizando contadores de desempenho
Este exemplo demonstra como acessar os contadores de desempenho do Windows Communication Foundation (WCF) e como criar contadores de desempenho definidos pelo usuário. Este exemplo é baseado na [introdução](getting-started-sample.md).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Neste exemplo, o cliente chama os quatro métodos do `ICalculator` serviço. O cliente continua a fazer isso até que seja interrompido pelo usuário. O serviço permanece inalterado.  
  
 Os contadores de desempenho são habilitados na seção diagnóstico do arquivo Web. config para o serviço, conforme mostrado na seguinte configuração de exemplo.  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <diagnostics performanceCounters="All" />
  </system.serviceModel>  
</configuration>  
```  
  
 Essa tarefa também pode ser feita usando a [ferramenta Editor de configuração (SvcConfigEditor. exe)](../configuration-editor-tool-svcconfigeditor-exe.md).  
  
 Quando os contadores de desempenho estão habilitados, o conjunto inteiro de contadores de desempenho do WCF é habilitado para o serviço. O .NET Framework mantém automaticamente os dados de desempenho em três níveis: `ServiceModelService` `ServiceModelEndpoint` e `ServiceModelOperation` . Cada um desses níveis tem contadores de desempenho como "chamadas", "chamadas por segundo" e "chamadas de segurança não autorizadas".  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
### <a name="to-view-performance-data"></a>Para exibir dados de desempenho  
  
1. Inicie a ferramenta Monitor de desempenho clicando em **Iniciar**, **executar...**, insira `perfmon` e clique em **OK,** ou, no painel de controle, selecione **Ferramentas administrativas** e clique duas vezes em **desempenho**.  
  
    > [!NOTE]
    > Você não pode adicionar contadores até que o código de exemplo esteja em execução.  
  
2. Remova os contadores de desempenho listados selecionando-os e pressionando a tecla Delete.  
  
3. Adicione os contadores do WCF clicando com o botão direito do mouse no painel gráfico e selecionando **Adicionar contadores**. Na caixa de diálogo **Adicionar contadores** , selecione **ServiceModelOperation 3.0.0.0, ServiceModelEndpoint 3.0.0.0 ou ServiceModelService 3.0.0.0** na caixa de listagem suspensa objeto de desempenho. Selecione os contadores que você deseja exibir na lista.  
  
    > [!NOTE]
    > Não haverá nenhum contador de desempenho do WCF para um serviço se não houver nenhum serviço WCF em execução no computador.  
  
### <a name="to-use-the-configuration-editor-to-enable-counters"></a>Para usar o editor de configuração para habilitar contadores  
  
1. Abra uma instância do SvcConfigEditor. exe.  
  
2. No menu arquivo, clique em **abrir** e em **arquivo de configuração...**.  
  
3. Navegue até a pasta de serviço do aplicativo de exemplo e abra o arquivo Web. config.  
  
4. Clique em **diagnóstico** na árvore de configuração.  
  
5. Alterne o **contador de desempenho** na janela de **diagnóstico** para mostrar ' todos '.  
  
6. Salve o arquivo de configuração e saia do editor.  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\PerfCounters`  
  
## <a name="see-also"></a>Consulte também

- [AppFabric que monitora Exemplos](https://docs.microsoft.com/previous-versions/appfabric/ff383407(v=azure.10))
