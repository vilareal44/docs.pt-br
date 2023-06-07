---
title: Configurando suporte de transações de WS-Atomic
ms.date: 03/30/2017
helpviewer_keywords:
- WS-AT protocol [WCF], configuring WS-Atomic Transaction
ms.assetid: cb9f1c9c-1439-4172-b9bc-b01c3e09ac48
ms.openlocfilehash: d396ccdaca81eab74de5e20d7ba7a9a00acbf7a6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597469"
---
# <a name="configure-ws-atomic-transaction-support"></a>Configurar o suporte à transação WS-Atomic

Este tópico descreve como você pode configurar o suporte a WS-AtomicTransaction (WS-AT) usando o utilitário de configuração WS-AT.

## <a name="use-the-ws-at-configuration-utility"></a>Usar o utilitário de configuração WS-AT

O utilitário de configuração WS-AT (wsatConfig. exe) é usado para definir configurações WS-AT. Para habilitar o serviço de protocolo WS-AT, você deve usar o utilitário de configuração do para configurar a porta HTTPS para WS-AT, associar um certificado X. 509 à porta HTTPS e configurar certificados de parceiros autorizados especificando nomes de entidades de certificado ou impressões digitais. O utilitário de configuração também permite que você selecione o modo de rastreamento e defina o tempo limite de saída padrão e máximo de transações de entrada.

Você pode acessar a funcionalidade dessa ferramenta usando um snap-in de página de propriedades do console de gerenciamento Microsoft (MMC) no console de gerenciamento dos serviços de componentes ou em uma janela de linha de comando. Configure o suporte do WS-AT no computador local por meio da janela de linha de comando; Defina as configurações em máquinas locais e remotas usando o snap-in do MMC.

A janela de linha de comando pode ser acessada no local de instalação do SDK do Windows "%WINDIR%\Microsoft.NET\Framework\v3.0\Windows Communication Foundation".

Para obter mais informações sobre a ferramenta de linha de comando, consulte [Utilitário de configuração do WS-AtomicTransaction (wsatConfig. exe)](../ws-atomictransaction-configuration-utility-wsatconfig-exe.md).

Se você estiver executando o Windows XP ou o Windows Server 2003, poderá acessar o snap-in do MMC navegando até **painel de controle/ferramentas administrativas/serviços de componentes**, clicando com o botão direito do mouse em **meu computador**e selecionando **Propriedades**. Esse é o mesmo local em que você pode configurar o Microsoft Coordenador de Transações Distribuídas (MSDTC). As opções disponíveis para configuração são agrupadas na guia **WS-AT** . Se você estiver executando o Windows Vista ou o Windows Server 2008, o snap-in do MMC pode ser encontrado clicando no botão **Iniciar** e digitando `dcomcnfg.exe` na caixa de **pesquisa** . Quando o MMC for aberto, navegue até o nó **minhas Computer\Distributed Transaction COORDINATOR\LOCAL DTC** , clique com o botão direito e selecione **Propriedades**. As opções disponíveis para configuração são agrupadas na guia **WS-AT** .

Para obter mais informações sobre o snap-in, consulte o [snap-in MMC de configuração do WS-AtomicTransaction](../ws-atomictransaction-configuration-mmc-snap-in.md).

Para habilitar a interface do usuário da ferramenta, você deve primeiro registrar o arquivo WsatUI. dll, localizado no seguinte caminho

%PROGRAMFILES%\Microsoft SDKs\Windows\v6.0\Bin

Para registrar o produto, execute o seguinte comando em uma janela de prompt de comando:

`regasm.exe /codebase WsatUI.dll`

## <a name="enable-ws-at"></a>Habilitar WS-AT

Para habilitar o serviço de protocolo WS-AT dentro do MSDTC usando a porta 443 e um certificado X. 509 com uma chave privada que foi instalada no repositório do computador local, use a ferramenta wsatConfig. exe com o comando a seguir.

`WsatConfig.exe –network:enable –port:8443 –endpointCert:<machine|"Issuer\SubjectName"> -accountsCerts:<thumbprint|"Issuer\SubjectName"> -restart`

Substitua os respectivos parâmetros pelos valores relevantes para o seu ambiente.

Para desabilitar o serviço de protocolo WS-AT dentro do MSDTC, use a ferramenta wsatConfig. exe com o comando a seguir.

`WsatConfig.exe –network:disable -restart`

## <a name="configure-trust-between-two-machines"></a>Configurar a confiança entre dois computadores

O serviço de protocolo WS-AT exige que o administrador autorize explicitamente contas individuais para participar de transações distribuídas. Se você for um administrador de dois computadores, poderá configurar ambos os computadores para estabelecer uma relação de confiança mútua, trocando o conjunto correto de certificados entre os computadores, instalando-os nos repositórios de certificados apropriados e usando a ferramenta wsatConfig. exe para adicionar o certificado de cada computador à lista de certificados de participante autorizados. Essa etapa é necessária para executar transações distribuídas entre dois computadores usando WS-AT.

No exemplo a seguir, descreve as etapas para estabelecer a confiança entre duas máquinas, A e B.

### <a name="create-and-export-certificates"></a>Criar e exportar certificados

Esse procedimento requer o snap-in de certificados do MMC. O snap-in pode ser acessado abrindo o menu Iniciar/Executar, digitando "MMC" na caixa de entrada e pressionando OK. Em seguida, na janela **Console1** , navegue até o snap-in **arquivo/adicionar-remover** , clique em Adicionar e escolha **certificados** na lista **snap-ins Autônomos disponíveis** . Por fim, selecione a **conta de computador** a ser gerenciada e clique em **OK**. O nó **certificados** é exibido no console do snap-in do.

Você já deve ter os certificados necessários para estabelecer confiança. Para saber como criar e instalar novos certificados antes das etapas a seguir, consulte [como: criar e instalar certificados de cliente temporários no WCF durante o desenvolvimento](https://docs.microsoft.com/previous-versions/msp-n-p/ff650751(v=pandp.10)).

1. No computador A, usando o snap-in de certificados do MMC, importe o certificado (certa) existente para o LocalMachine\MY (nó pessoal) e o repositório LocalMachine\ROOT (nó de autoridade de certificação raiz confiável). Para importar um certificado para um nó específico, clique com o botão direito do mouse no nó e escolha **todas as tarefas/importar**.

2. No computador B, usando o snap-in de certificados do MMC, crie ou obtenha um certB de certificado com uma chave privada e importe-o para o LocalMachine\MY (nó pessoal) e o repositório LocalMachine\ROOT (nó de autoridade de certificação raiz confiável).

3. Exporte a chave pública do certa para um arquivo se isso ainda não tiver sido feito.

4. Exporte a chave pública do certB para um arquivo se isso ainda não tiver sido feito.

### <a name="establish-mutual-trust-between-machines"></a>Estabelecer confiança mútua entre computadores

1. No computador A, importe a representação de arquivo do certB para os armazenamentos LocalMachine\MY e LocalMachine\ROOT. Isso declara que o computador A confia em certB para se comunicar com ele.

2. No computador B, importe o arquivo de certa para as lojas LocalMachine\MY e LocalMachine\ROOT. Isso significa que o computador B confia no certa para se comunicar com ele.

Depois de concluir essas etapas, a confiança é estabelecida entre as duas máquinas, e elas podem ser configuradas para se comunicarem entre si usando WS-AT.

### <a name="configure-msdtc-to-use-certificates"></a>Configurar o MSDTC para usar certificados

Como o serviço de protocolo WS-AT atua como um cliente e um servidor, ele deve escutar conexões de entrada e iniciar conexões de saída. Portanto, você precisa configurar o MSDTC para que ele saiba qual certificado usar ao se comunicar com partes externas e quais certificados autorizar ao aceitar a comunicação de entrada.

Você pode configurar isso usando o snap-in do WS-AT do MMC. Para obter mais informações sobre essa ferramenta, consulte o tópico [snap-in MMC de configuração do WS-AtomicTransaction](../ws-atomictransaction-configuration-mmc-snap-in.md) . As etapas a seguir descrevem como estabelecer a confiança entre dois computadores que executam o MSDTC.

1. Defina as configurações do computador A. Para "certificado de ponto de extremidade", selecione certa. Para "certificados autorizados", selecione o certB.

2. Defina as configurações do computador B. Para "certificado de ponto de extremidade", selecione certB. Para "certificados autorizados", selecione o certificadoa.

> [!NOTE]
> Quando um computador envia uma mensagem para o outro computador, o remetente tenta verificar se o nome da entidade do certificado do destinatário e o nome da máquina do destinatário correspondem. Se eles não corresponderem, a verificação de certificado falhará e os dois computadores não poderão se comunicar.
>
> Para um computador ingressado em um domínio, o nome é o nome de domínio totalmente qualificado. Por padrão, o nome de um computador em um grupo de trabalho é o nome NetBIOS do computador. No entanto, o nome também pode incluir um sufixo DNS (sistema de nomes de domínio) se um estiver presente para a conexão que está sendo usada entre os dois computadores.
>
> Se o nome da máquina for alterado, por exemplo, quando um computador de grupo de trabalho ingressar em um domínio, você deverá reemitir certificados ou configurar manualmente sufixos DNS.

## <a name="security"></a>Segurança

Como algumas das configurações relacionadas ao MSDTC e ao WS-AT são armazenadas no registro em HKLM\Software\Microsoft\MSDTC e em HKLM\Software\Microsoft\WSAT, respectivamente, verifique se essas chaves do registro estão protegidas para que somente os administradores possam gravar nelas. Na ferramenta Editor do registro, clique com o botão direito do mouse na chave que você deseja proteger e selecione **permissão** para definir o controle de acesso apropriado. É crucial para a segurança e a integridade do sistema que chaves importantes são somente leitura para usuários com poucos privilégios.

Ao implantar o MSDTC, o administrador deve garantir que qualquer intercâmbio de dados do MSDTC seja seguro. Em uma implantação de grupo de trabalho, isole a infraestrutura transacional de usuários mal-intencionados; em uma implantação de cluster, proteja o registro de cluster.

## <a name="tracing"></a>Rastreamento

O serviço de protocolo WS-AT dá suporte ao rastreamento integrado, específico da transação, que pode ser habilitado e gerenciado por meio do uso da ferramenta de [snap-in MMC de configuração do WS-AtomicTransaction](../ws-atomictransaction-configuration-mmc-snap-in.md) . Os rastreamentos podem incluir dados indicando a hora em que uma inscrição é feita para uma transação específica, a hora em que uma transação atinge seu estado de terminal, o resultado que cada inscrição de transação recebeu. Todos os rastreamentos podem ser exibidos usando a ferramenta do [Visualizador de rastreamento de serviço (SvcTraceViewer. exe)](../service-trace-viewer-tool-svctraceviewer-exe.md) .

O serviço de protocolo WS-AT também dá suporte ao rastreamento de ServiceModel integrado por meio da sessão de rastreamento ETW. Isso fornece rastreamentos mais detalhados e específicos de comunicação, além dos rastreamentos de transação existentes.  Para habilitar esses rastreamentos adicionais, siga estas etapas

1. Abra o menu **Iniciar/Executar** , digite "regedit" na caixa de entrada e selecione **OK**.

2. No **Editor do registro**, navegue até a seguinte pasta no painel esquerdo, HKEY_LOCAL_MACHINE \software\microsoft\wsat\3.0\

3. Clique com o botão direito do mouse no `ServiceModelDiagnosticTracing` valor no painel direito e selecione **Modificar**.

4. Na caixa entrada de **dados de valor** , insira um dos valores válidos a seguir para especificar o nível de rastreamento que você deseja habilitar.

- 0: desativado

- 1: crítico

- 3: erro. Esse é o valor padrão.

- 7: aviso

- 15: informações

- 31: detalhado

## <a name="see-also"></a>Consulte também

- [Utilitário de configuração de WS-AtomicTransaction (wsatConfig.exe)](../ws-atomictransaction-configuration-utility-wsatconfig-exe.md)
- [Snap-in do MMC de configuração de WS-AtomicTransaction](../ws-atomictransaction-configuration-mmc-snap-in.md)
