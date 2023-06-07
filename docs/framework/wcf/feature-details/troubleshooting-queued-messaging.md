---
title: Mensagens em fila da solução de problemas
ms.date: 03/30/2017
ms.assetid: a5f2836f-018d-42f5-a571-1e97e64ea5b0
ms.openlocfilehash: f695af3d2ad498e1f5975e1a396f1e7b05bf63bc
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595122"
---
# <a name="troubleshooting-queued-messaging"></a>Mensagens em fila da solução de problemas

Esta seção contém perguntas comuns e ajuda de solução de problemas para usar filas no Windows Communication Foundation (WCF).

## <a name="common-questions"></a>Perguntas comuns

**P:** Usei o WCF beta 1 e instalei o hotfix MSMQ. É necessário remover o hotfix?

**R:** Sim. Não há mais suporte para este hotfix. O WCF agora funciona no MSMQ sem um requisito de hotfix.

**P:** Há duas associações para o MSMQ: <xref:System.ServiceModel.NetMsmqBinding> e <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> . O que devo usar e quando?

**R:** Use o <xref:System.ServiceModel.NetMsmqBinding> quando quiser usar o MSMQ como um transporte para a comunicação em fila entre dois aplicativos WCF. Use o <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> quando desejar usar aplicativos MSMQ existentes para se comunicar com novos aplicativos WCF.

**P:** É necessário atualizar o MSMQ para usar as <xref:System.ServiceModel.NetMsmqBinding> associações e `MsmqIntegration` ?

**R:** Não. Ambas as associações funcionam com o MSMQ 3,0 no Windows XP e no Windows Server 2003. Determinados recursos das associações ficam disponíveis quando você atualiza para o MSMQ 4,0 no Windows Vista.

**P:** Quais recursos das <xref:System.ServiceModel.NetMsmqBinding> associações e <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> estão disponíveis no MSMQ 4,0, mas não no MSMQ 3,0?

**R:** Os recursos a seguir estão disponíveis no MSMQ 4,0, mas não no MSMQ 3,0:

- A fila de mensagens mortas personalizada tem suporte apenas no MSMQ 4,0.

- O MSMQ 3,0 e 4,0 lidam com mensagens suspeitas de forma diferente.

- Somente o MSMQ 4,0 dá suporte à leitura transacionada remota.

Para obter mais informações, consulte [diferenças em recursos de enfileiramento no Windows Vista, no Windows Server 2003 e no Windows XP](diff-in-queue-in-vista-server-2003-windows-xp.md).

**P:** Posso usar o MSMQ 3,0 em um lado de uma comunicação em fila e o MSMQ 4,0 no outro lado?

**R:** Sim.

**P:** Quero integrar os aplicativos MSMQ existentes a novos clientes ou servidores WCF. É necessário atualizar ambos os lados da minha infraestrutura do MSMQ?

**R:** Não. Não é necessário atualizar para o MSMQ 4,0 em ambos os lados.

## <a name="troubleshooting"></a>Solução de problemas

Esta seção contém respostas para os problemas mais comuns de solução de problemas. Alguns problemas que são limitações conhecidas também são descritos nas notas de versão.

**P:** Estou tentando usar uma fila particular e obtenho a seguinte exceção: `System.InvalidOperationException` : a URL é inválida. A URL da fila não pode conter o caractere ' $ '. Use a sintaxe em net. MSMQ://machine/private/queueName para endereçar uma fila privada.

**R:** Verifique a Uniform Resource Identifier da fila (URI) em sua configuração e código. Não use o caractere "$" no URI. Por exemplo, para endereçar uma fila particular chamada OrdersQueue, especifique o URI como `net.msmq://localhost/private/ordersQueue` .

**P:** Chamar `ServiceHost.Open()` em meu aplicativo em fila gera a seguinte exceção: `System.ArgumentException` : um endereço base não pode conter uma cadeia de caracteres de consulta de URI. Por que?

**R:** Verifique o URI da fila no arquivo de configuração e no seu código. Enquanto as filas do MSMQ dão suporte ao uso do caractere '? ', os URIs interpretam esse caractere como o início de uma consulta de cadeia de caracteres. Para evitar esse problema, use nomes de fila que não contenham caracteres '? '.

**P:** Meu envio foi bem-sucedido, mas nenhuma operação de serviço é invocada no receptor. Por que?

**R:** Para determinar a resposta, trabalhe na seguinte lista de verificação:

- Verifique se os requisitos de fila transacional são compatíveis com as garantias especificadas. Observe os seguintes princípios:

  - Você pode enviar mensagens duráveis (datagrams e sessões) com garantias "exatamente uma vez" ( <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>  =  `true` ) somente para uma fila transacional.

  - Você pode enviar sessões somente com garantias "exatamente uma vez".

  - Uma transação é necessária para receber mensagens em uma sessão de uma fila transacional.

  - Você pode enviar ou receber mensagens voláteis ou duráveis (somente datagrams) sem garantias ( <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>  =  `false` ) somente para uma fila não transacional.

- Verifique a fila de mensagens mortas. Se você encontrar as mensagens lá, determine por que elas não foram entregues.

- Verifique as filas de saída para obter conectividade ou resolver problemas.

**P:** Especifiquei uma fila de mensagens mortas personalizada, mas quando eu iniciar o aplicativo do remetente, recebo uma exceção de que a fila de mensagens mortas não foi encontrada ou o aplicativo de envio não tem permissão para a fila de mensagens mortas. Por que isso está acontecendo?

**R:** O URI de fila de mensagens mortas personalizado deve incluir um "localhost" ou o nome do computador no primeiro segmento, por exemplo, net. MSMQ://localhost/private/myAppdead-letter Queue.

**P:** É sempre necessário definir uma fila de mensagens mortas personalizada ou existe uma fila de mensagens mortas padrão?

**R:** Se as garantias forem "exatamente uma vez" ( <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>  =  `true` ) e se você não especificar uma fila de mensagens mortas personalizada, o padrão será uma fila de mensagens mortas transacional em todo o sistema.

Se as garantias forem nenhuma ( <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>  =  `false` ), o padrão será nenhuma funcionalidade de fila de mensagens mortas.

**P:** Meu serviço é acionado em SvcHost. Open com uma mensagem "os requisitos de EndpointListener não podem ser atendidos pelo ListenerFactory". Por que?

A. Verifique seu contrato de serviço. Você pode ter esquecido de colocar "IsOneWay = `true` " em todas as operações de serviço. As filas dão suporte apenas a operações de serviço unidirecionais.

**P:** Há mensagens na fila, mas nenhuma operação de serviço é invocada. Qual é o problema?

**R:** Determine se o host de serviço está com falha. Você pode verificar examinando o rastreamento ou implementando `IErrorHandler` . Falhas de host de serviço, por padrão, se uma mensagem suspeita for detectada.

**P:** Há mensagens na fila, mas meu serviço enfileirado hospedado na Web não está sendo ativado. Por que?

**R:** O motivo mais comum é as permissões.

1. Verifique se o `NetMsmqActivator` processo está em execução e se a identidade do `NetMsmqActivator` processo recebeu a permissão de leitura e busca na fila.

2. Se o `NetMsmqActivator` estiver monitorando filas em um computador remoto, certifique-se de que o `NetMsmqActivator` não seja executado sob um token restrito. Para executar o `NetMsmqActivator` com um token irrestrito:

    ```console
    sc sidtype NetMsmqActivator unrestricted
    ```

Para problemas de host da Web relacionados à não segurança, consulte: [hospedagem Web de um aplicativo enfileirado](web-hosting-a-queued-application.md).

**P:** Qual é a maneira mais fácil de acessar sessões?

**R:** Defina preenchimento automático = `true` na operação que corresponde à última mensagem na sessão e defina AutoCompletar = `false` em todas as operações de serviço restantes.

**P:** Por que meu serviço lança um `ProtocolException` ao ler de uma fila que contém mensagens de sessão enfileiradas e mensagens de datagrama em fila?

**R:** Há uma diferença fundamental no modo como as mensagens de sessão enfileiradas e as mensagens de datagrama em fila são compostas. Por isso, um serviço que está esperando ler uma mensagem de sessão em fila não pode receber uma mensagem de datagrama enfileirada e um serviço esperando ler uma mensagem de datagrama em fila não pode receber uma mensagem de sessão. A tentativa de ler os dois tipos de mensagens da mesma fila gera a seguinte exceção:

```console
System.ServiceModel.MsmqPoisonMessageException: The transport channel detected a poison message. This occurred because the message exceeded the maximum number of delivery attempts or because the channel detected a fundamental problem with the message. The inner exception may contain additional information.
---> System.ServiceModel.ProtocolException: An incoming MSMQ message contained invalid or unexpected .NET Message Framing information in its body. The message cannot be received. Ensure that the sender is using a compatible service contract with a matching SessionMode.
```

A fila de mensagens mortas do sistema, bem como qualquer fila de mensagens mortas personalizada, é particularmente suscetível a esse problema se um aplicativo envia mensagens de sessão em fila e mensagens de datagrama em fila do mesmo computador. Se uma mensagem não puder ser enviada com êxito, ela será movida para a fila de mensagens mortas. Sob essas circunstâncias, é possível ter mensagens de mensagem e de datagrama na fila de mensagens mortas. Não há como separar os dois tipos de mensagens no tempo de execução ao ler de uma fila, portanto, os aplicativos não devem enviar mensagens de sessão em fila e mensagens de datagrama em fila do mesmo computador.

### <a name="msmq-integration-specific-troubleshooting"></a>Integração do MSMQ: solução de problemas específica

**P:** Quando envio uma mensagem ou quando abro o host de serviço, recebo um erro indicando que o esquema está errado. Por que?

**R:** Ao usar a associação de integração do MSMQ, você deve usar o esquema MSMQ. FormatName. Por exemplo, MSMQ. FormatName: DIRECT = OS: .\Private $ \OrdersQueue. Mas ao especificar a fila de mensagens mortas personalizada, você deve usar o esquema net. MSMQ.

**P:** Quando uso um nome de formato público ou privado e abro o host de serviço no Windows Vista, recebo um erro. Por que?

**R:** O canal de integração do WCF no Windows Vista verifica se uma subfila pode ser aberta para a fila de aplicativo principal para lidar com mensagens suspeitas. O nome da subfila é derivado de um URI MSMQ. FormatName passado para o ouvinte. O nome da subfila no MSMQ só pode ser um nome de formato direto. Você verá o erro. Altere o URI da fila para um nome de formato direto.

**P:** Ao receber uma mensagem de um aplicativo MSMQ, a mensagem fica na fila e não é lida pelo aplicativo WCF receptor. Por que?

**R:** Verifique se a mensagem tem um corpo. Se a mensagem não tiver corpo, o canal de integração do MSMQ ignorará a mensagem. Implemente `IErrorHandler` para ser notificado de exceções e verifique os rastreamentos.

### <a name="security-related-troubleshooting"></a>Solução de problemas relacionada à segurança

**P:** Quando executo o exemplo que usa uma associação padrão no modo de grupo de trabalho, as mensagens parecem ser enviadas, mas nunca são recebidas pelo destinatário.

**R:** Por padrão, as mensagens são assinadas usando um certificado interno do MSMQ que requer o serviço de diretório Active Directory. No modo de grupo de trabalho, como Active Directory não está disponível, a assinatura da mensagem falha. Assim, a mensagem chega à fila de mensagens mortas e a causa de falha, como "assinatura inadequada", é indicada.

A solução alternativa é desativar a segurança. Isso é feito definindo <xref:System.ServiceModel.NetMsmqSecurity.Mode%2A>  =  <xref:System.ServiceModel.NetMsmqSecurityMode.None> para fazê-lo funcionar no modo de grupo de trabalho.

Outra solução alternativa é obter o <xref:System.ServiceModel.MsmqTransportSecurity> da <xref:System.ServiceModel.NetMsmqSecurity.Transport%2A> propriedade e defini-lo como e <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate> definir o certificado do cliente.

Outra alternativa é instalar o MSMQ com a integração do Active Directory.

**P:** Quando envio uma mensagem com associação padrão (segurança de transporte ativada) em Active Directory a uma fila, recebo uma mensagem "certificado interno não encontrado". Como fazer corrigir isso?

**R:** Isso significa que o certificado em Active Directory para o remetente deve ser renovado. Para fazer isso, abra **painel de controle**, **Ferramentas administrativas**, **Gerenciamento do computador**, clique com o botão direito do mouse em **MSMQ**e selecione **Propriedades**. Selecione a guia **certificado de usuário** e clique no botão **renovar** .

**P:** Quando eu envio uma mensagem usando <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate> e especificamos o certificado a ser usado, recebo uma mensagem de "certificado inválido". Como fazer corrigir isso?

**R:** Você não pode usar um repositório de certificados do computador local com o modo de certificado. Você precisa copiar o certificado do repositório de certificados do computador para o repositório de usuários atual usando o snap-in de certificado. Para obter o snap-in de certificado:

1. Clique em **Iniciar**, selecione **executar**, digite `mmc` e clique em **OK**.

2. No **console de gerenciamento Microsoft**, abra o menu **arquivo** e selecione **Adicionar/remover snap-in**.

3. Na caixa de diálogo **Adicionar/remover snap-in** , clique no botão **Adicionar** .

4. Na caixa de diálogo **Adicionar Snap-in autônomo** , selecione certificados e clique em **Adicionar**.

5. Na caixa de diálogo snap-in de **certificados** , selecione **minha conta de usuário** e clique em **concluir**.

6. Em seguida, adicione um segundo snap-in de certificados usando as etapas anteriores, mas desta vez selecione **conta de computador** e clique em **Avançar**.

7. Selecione **Computador Local** e clique em **Concluir**. Agora você pode arrastar e soltar certificados do repositório de certificados do computador para o repositório de usuários atual.

**P:** Quando meu serviço lê de uma fila em outro computador no modo de grupo de trabalho, obtenho uma exceção de "acesso negado".

**R:** No modo de grupo de trabalho, para que um aplicativo remoto tenha acesso à fila, o aplicativo deve ter permissão para acessar a fila. Adicione "logon anônimo" à ACL (lista de controle de acesso) da fila e conceda a ela permissão de leitura.

**P:** Quando um cliente de serviço de rede (ou qualquer cliente que não tem uma conta de domínio) envia uma mensagem em fila, o envio falha com um certificado inválido. Como fazer corrigir isso?

**R:** Verifique a configuração de associação. A associação padrão tem a segurança de transporte MSMQ ativada para assinar a mensagem. Desative-o.

### <a name="remote-transacted-receives"></a>Recebimentos transacionados remotamente

**P:** Quando tenho uma fila no computador A e um serviço WCF que lê mensagens de uma fila no computador B (o cenário de recebimento remoto transacionado), as mensagens não são lidas da fila. Informações de rastreamento indicam que o recebimento falhou com a mensagem "a transação não pode ser importada". O que fazer para corrigir isso?

**R:** Há três motivos possíveis para isso:

- Se você estiver no modo de domínio, o recebimento remoto transacionado exigirá acesso à rede do Microsoft Coordenador de Transações Distribuídas (MSDTC). Você pode habilitar isso usando **Adicionar/remover componentes**.

  ![Captura de tela que mostra a habilitação do acesso DTC de rede.](./media/troubleshooting-queued-messaging/enable-distributed-transaction-coordinator-access.jpg)

- Verifique o modo de autenticação para se comunicar com o Gerenciador de transações. Se você estiver no modo de grupo de trabalho, "nenhuma autenticação necessária" deverá ser selecionada. Se você estiver no modo de domínio, a "autenticação mútua necessária" deverá ser selecionada.

  ![Habilitando transações XA](media/4f3695e0-fb0b-4c5b-afac-75f8860d2bb0.jpg "4f3695e0-fb0b-4c5b-afac-75f8860d2bb0")

- Verifique se o MSDTC está na lista de exceções nas configurações de **Firewall de conexão com a Internet** .

- Verifique se você está usando o Windows Vista. O MSMQ no Windows Vista dá suporte à leitura transacionada remota. O MSMQ em versões anteriores do Windows não dá suporte à leitura transacionada remota.

**P:** Quando o serviço de leitura da fila é um serviço de rede, por exemplo, em um host da Web, por que obtenho uma exceção de acesso negado ao ler da fila?

**R:** O acesso de leitura do serviço de rede deve ser adicionado à ACL da fila para garantir que um serviço de rede possa ler a partir da fila.

**P:** Posso usar o serviço de ativação MSMQ para ativar aplicativos com base em mensagens em uma fila em um computador remoto?

**R:** Sim. Para fazer isso, você deve configurar o serviço de ativação MSMQ para ser executado como um serviço de rede e adicionar acesso ao serviço de rede à fila no computador remoto.

## <a name="using-custom-msmq-bindings-with-receivecontext-enabled"></a>Usando associações MSMQ personalizadas com ReceiveContext habilitado

Ao usar uma associação do MSMQ personalizada com <xref:System.ServiceModel.Channels.ReceiveContext> habilitado, o processamento de uma mensagem de entrada usa um thread do pool de threads porque o MSMQ nativo não dá suporte à conclusão de e/s para <xref:System.ServiceModel.Channels.ReceiveContext> recebimentos assíncronos. Isso ocorre porque o processamento dessa mensagem usa transações internas para <xref:System.ServiceModel.Channels.ReceiveContext> e o MSMQ não dá suporte ao processamento assíncrono. Para contornar esse problema, você pode adicionar um <xref:System.ServiceModel.Description.SynchronousReceiveBehavior> ao ponto de extremidade para forçar o processamento síncrono ou definir <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior.MaxPendingReceives%2A> como 1.
