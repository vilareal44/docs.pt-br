---
title: Provedores ETW no CLR
description: 'Examine os detalhes sobre os dois provedores de ETW (rastreamento de eventos) Common Language Runtime (CLR): o provedor runtimne e o provedor de encerramento.'
ms.date: 03/30/2017
helpviewer_keywords:
- ETW, CLR providers
- CLR ETW providers
ms.assetid: 0beafad4-b2c8-47f4-b342-83411d57a51f
ms.openlocfilehash: 9f86e8334482880c4f7cb23ec93a3c826c083389
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309645"
---
# <a name="clr-etw-providers"></a>Provedores ETW no CLR
O CLR (Common Language Runtime) tem dois provedores: o provedor de tempo de execução e o provedor de encerramento.  
  
 O provedor de runtime aciona eventos, dependendo de quais palavras-chave (categorias de eventos) são habilitadas. Por exemplo, é possível coletar eventos de carregador habilitando a palavra-chave `LoaderKeyword`.  
  
 Os eventos do ETW (rastreamento de eventos para Windows) são registrados em um arquivo que tem uma extensão. ETL, que posteriormente pode ser processada novamente em arquivos de valores separados por vírgulas (. csv), conforme necessário. Para obter informações sobre como converter o arquivo .etl em um arquivo .csv, consulte [Controlando o log do .NET Framework](controlling-logging.md).  
  
## <a name="the-runtime-provider"></a>O provedor de runtime  
 O provedor de runtime é o principal provedor CLR ETW.  
  
 O GUID do provedor de runtime CLR é e13c0d23-ccbc-4e12-931b-d9cc2eee27e4.  
  
 Para obter exemplos de como registrar e exibir eventos CLR ETW usando as ferramentas geralmente disponíveis, consulte [Controlando o log do .NET Framework](controlling-logging.md).  
  
 Além de usar palavras-chave como `LoaderKeyword`, talvez você precise habilitar palavras-chave para registrar eventos que podem ser acionados com muita frequência. As palavras-chave `StartEnumerationKeyword` e `EndEnumerationKeyword` habilitam esses eventos e são resumidas em [Palavras-chave e níveis CLR ETW](clr-etw-keywords-and-levels.md).  
  
## <a name="the-rundown-provider"></a>O provedor de encerramento  
 O provedor de encerramento deve ser ativado para determinados usos de finalidade especial. No entanto, para a maioria dos usuários, o provedor de runtime deve ser suficiente.  
  
 O GUID do provedor de encerramento CLR é A669021C-C450-4609-A035-5AF59AF4DF18.  
  
 Normalmente, o log ETW é habilitado antes do início de um processo e é desativado depois que o processo é fechado. No entanto, se o log ETW for ativado durante a execução do processo, serão necessárias informações adicionais sobre o processo. Por exemplo, para a resolução de símbolo, você precisa registrar eventos de método para métodos que já foram carregados antes da ativação do log.  
  
 Os eventos `DCStart` e `DCEnd` capturam o estado do processo quando a coleta de dados foi iniciada e interrompida. (State refere-se a informações em um nível alto, incluindo os métodos que já foram compilados JIT (just-in-time) e assemblies que foram carregados.) Esses dois eventos podem fornecer informações sobre o que já aconteceu no processo; por exemplo, quais métodos foram compilados em JIT e assim por diante.  
  
 Somente os eventos com `DC`, `DCStart`, `DCEnd` ou `DCInit` em seus nomes são acionados no provedor de encerramento. Além disso, esses eventos são acionados apenas no provedor de encerramento.  
  
 Além dos filtros de palavra-chave do evento, o provedor de encerramento também dá suporte às palavras-chave `StartRundownKeyword` e `EndRundownKeyword` para fornecer filtragem direcionada.  
  
### <a name="start-rundown"></a>Encerramento inicial  
 Um encerramento inicial é disparado quando o log no provedor de encerramento é habilitado com a palavra-chave `StartRundownKeyword`. Isso faz com que o evento `DCStart` seja acionado e captura o estado do sistema. Antes do início da enumeração, o evento `DCStartInit` é acionado. Ao final da enumeração, o evento `DCStartComplete` é acionado para notificar o controlador de que a coleta de dados terminou normalmente.  
  
### <a name="end-rundown"></a>Encerramento final  
 Um encerramento final é disparado quando o log no provedor de encerramento é habilitado com a palavra-chave `EndRundownKeyword`. O encerramento final interrompe a criação de perfil em um processo que continua sendo executado. Os eventos `DCEnd` capturam o estado do sistema quando a criação de perfil é interrompida.  
  
 Antes do início da enumeração, o evento `DCEndInit` é acionado. Ao final da enumeração, o evento `DCEndComplete` é acionado para notificar o consumidor de que a coleta de dados terminou normalmente. O encerramento inicial e final são usados principalmente para a resolução de símbolo gerenciado. O encerramento inicial pode fornecer informações de intervalo de endereços para métodos que já foram compilados pelo JIT antes do início da sessão de criação de perfil. O encerramento final pode fornecer informações de intervalo de endereços para todos os métodos que foram compilados pelo JIT quando a criação de perfil está prestes a ser desativada.  
  
 O encerramento final não ocorre automaticamente quando uma sessão de criação de perfil é interrompida. Em vez disso, uma ferramenta que está tentando executar a resolução de símbolo gerenciado deve invocar explicitamente uma sessão do provedor de encerramento CLR com a palavra-chave `EndRundownKeyword` habilitada, logo antes que a criação de perfil seja interrompida.  
  
 Embora o encerramento inicial ou final possa fornecer informações de intervalo de endereços do método para a resolução de símbolo gerenciado, recomendamos o uso da palavra-chave `EndRundownKeyword` (que fornece eventos `DCEnd`), em vez da palavra-chave `StartRundownKeyword` (que fornece eventos `DCStart`). O uso de `StartRundownKeyword` faz com que o encerramento ocorra durante a sessão de criação de perfil, o que pode interferir no cenário com perfil criado.  
  
## <a name="etw-data-collection-using-runtime-and-rundown-providers"></a>Coleta de dados ETW usando o runtime e provedores de encerramento  
 O exemplo a seguir demonstra como usar o provedor de encerramento CLR de uma maneira que permite a resolução de símbolo de processos gerenciados com impacto mínimo, independentemente do início ou término dos processos dentro ou fora da janela com perfil criado.  
  
1. Ative o log ETW usando o provedor de runtime do CLR:  
  
    ```console
    xperf -start clr -on e13c0d23-ccbc-4e12-931b-d9cc2eee27e4:0x1CCBD:0x5 -f clr1.etl
    ```  
  
     O log será salvo no arquivo clr1.etl.  
  
2. Para interromper a criação de perfil durante a execução do processo, inicie o provedor de encerramento para capturar os eventos `DCEnd`:  
  
    ```console
    xperf -start clrRundown -on A669021C-C450-4609-A035-5AF59AF4DF18:0xB8:0x5 -f clr2.etl
    ```  
  
     Isso permite que a coleção de eventos `DCEnd` inicie uma sessão de encerramento. Talvez seja necessário aguardar de 30 a 60 segundos para que todos os eventos sejam coletados. O log será salvo no arquivo clr1.et2.  
  
3. Desligue toda a criação de perfil ETW:  
  
    ```console
    xperf -stop clrRundown
    xperf -stop clr  
    ```  
  
4. Mescle os perfis para criar um arquivo de log:  
  
    ```console
    xperf -merge clr1.etl clr2.etl merged.etl  
    ```  
  
     O arquivo merged.etl conterá os eventos do runtime e as sessões do provedor de encerramento.  
  
 Uma ferramenta pode executar as etapas 2 e 3 (iniciar uma sessão de encerramento e, em seguida, terminar a criação de perfil), em vez de desativar a criação de perfil imediatamente quando um usuário solicitar que a criação de perfil seja interrompida. Uma ferramenta também pode executar a etapa 4.  
  
## <a name="see-also"></a>Confira também

- [Eventos ETW no Common Language Runtime](etw-events-in-the-common-language-runtime.md)
