---
title: Rastreamento circular
ms.date: 03/30/2017
ms.assetid: 5ff139f9-8806-47bc-8f33-47fe6c436b92
ms.openlocfilehash: 1759db28cb024afc04d02c4b128f96d73aefdd87
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585435"
---
# <a name="circular-tracing"></a>Rastreamento circular

Este exemplo demonstra a implementação de um ouvinte de rastreamento de buffer circular. Um cenário comum para os serviços de produção é ter serviços disponíveis por longos períodos de tempo e ter o log de rastreamento habilitado em um nível baixo. Esses serviços consomem muito espaço em disco. Ao solucionar problemas de um serviço, os dados mais recentes no log de rastreamento são relevantes para resolver um problema. Este exemplo demonstra uma implementação de um ouvinte de rastreamento de buffer circular no qual apenas os rastreamentos mais recentes são mantidos em disco até uma quantidade configurável de dados. Este exemplo é baseado na [introdução](getting-started-sample.md) e inclui um ouvinte de rastreamento personalizado.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

Este exemplo pressupõe que você esteja familiarizado com o [rastreamento e](tracing-and-message-logging.md) a amostra de log de mensagens e leu a documentação fornecida para o exemplo de [rastreamento e log de mensagens](tracing-and-message-logging.md) .

## <a name="circular-buffer-trace-listener"></a>Ouvinte de rastreamento de buffer circular

O conceito por trás da implementação do ouvinte de rastreamento de buffer circular é ter dois arquivos que podem armazenar até metade do total de dados de log de rastreamento desejado. O ouvinte cria um arquivo e grava nesse arquivo até atingir o limite de metade do tamanho dos dados, em que ponto ele alterna para um segundo arquivo. Quando o ouvinte atinge o limite do segundo arquivo, ele substitui o primeiro arquivo por novos rastreamentos.

Esse ouvinte deriva de `XmlWriteTraceListener` e permite que os logs sejam exibidos com a [ferramenta do Visualizador de rastreamento de serviço (SvcTraceViewer. exe)](../service-trace-viewer-tool-svctraceviewer-exe.md). Ao tentar exibir os logs, os dois arquivos de log podem ser facilmente recombinados abrindo-se ambos os arquivos de log ao mesmo tempo na ferramenta do Visualizador de rastreamento de serviço. A ferramenta Visualizador de rastreamento de serviço cuida automaticamente da classificação dos rastreamentos para que eles apareçam na ordem correta.

## <a name="configuration"></a>Configuração

Um serviço pode ser configurado para usar o ouvinte de rastreamento de buffer circular adicionando o código a seguir para um ouvinte e elementos de origem. O tamanho máximo do arquivo é especificado definindo o `maxFileSizeKB` atributo na configuração do ouvinte de rastreamento circular. Isso é demonstrado no código a seguir.

```xml
<system.diagnostics>
  <sources>
    <source name="System.ServiceModel" switchValue="Information,ActivityTracing" propagateActivity="true">
      <listeners>
        <add name="CircularTraceListener" />
      </listeners>
    </source>
  </sources>
  <sharedListeners>
    <add name="CircularTraceListener" type="Microsoft. Samples.ServiceModel.CircularTraceListener,CircularTraceListener"
         initializeData="c:\logs\CircularTracing-service.svclog" maxFileSizeKB="100" />
  </sharedListeners>
  <trace autoflush="true" />
</system.diagnostics>
```

#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Certifique-se de ter executado o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\CircularTracing`

## <a name="see-also"></a>Consulte também

- [AppFabric que monitora Exemplos](https://docs.microsoft.com/previous-versions/appfabric/ff383407(v=azure.10))
