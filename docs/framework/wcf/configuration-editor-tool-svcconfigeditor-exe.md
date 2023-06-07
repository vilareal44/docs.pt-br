---
title: Ferramenta Configuration Editor (SvcConfigEditor.exe)
description: Descubra como gerenciar configurações para associações, comportamentos, serviços e diagnósticos do WCF usando o editor de configuração do serviço WCF.
ms.date: 03/30/2017
helpviewer_keywords:
- configuration files, creating
- configuration files
- Configuration file
- configuration file schema
ms.assetid: 2db21a57-5f64-426f-89df-fb0dc2d2def5
ms.openlocfilehash: 258437ff616b969d40feabbfff364ad2cc6b25bc
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247643"
---
# <a name="configuration-editor-tool-svcconfigeditorexe"></a>Ferramenta Configuration Editor (SvcConfigEditor.exe)

O SvcConfigEditor.exe (editor de configuração de serviço) do Windows Communication Foundation (WCF) permite que administradores e desenvolvedores criem e modifiquem definições de configuração para serviços WCF usando uma interface gráfica do usuário. Com essa ferramenta, você pode gerenciar configurações para associações, comportamentos, serviços e diagnósticos do WCF sem precisar editar diretamente os arquivos de configuração XML.

O editor de configuração de serviço pode ser encontrado na pasta C:\Program Files\Microsoft SDKs\Windows\v6.0\Bin

## <a name="the-wcf-configuration-editor"></a>O editor de configuração do WCF

O editor de configuração de serviço vem com um assistente que orienta você por todas as etapas de configuração de um serviço ou cliente WCF. É altamente recomendável usar o assistente em vez do editor diretamente.

Se você já tiver alguns arquivos de configuração que estão em conformidade com o esquema padrão do System.Configuração, poderá gerenciar configurações específicas para associações, comportamento, serviços e diagnósticos com a interface do usuário. O editor de configuração de serviço permite que você gerencie as configurações para arquivos de configuração do WCF existentes, bem como arquivos executáveis, serviços COM+ e serviços hospedados na Web. Ao abrir um serviço hospedado na Web com o editor de configuração de serviço, as seções configuração do próprio serviço e configurações herdadas dos nós de nível superior são mostradas.

Como as definições de configuração do WCF estão localizadas na `<system.serviceModel>` seção do arquivo de configuração, o editor opera exclusivamente no conteúdo desse elemento e não acessa outros elementos no mesmo arquivo. Você pode navegar diretamente para os arquivos de configuração existentes ou pode selecionar um assembly que contenha um serviço, um diretório virtual ou um serviço COM+. O editor carrega o arquivo de configuração para esse serviço específico e permite que o usuário adicione novos elementos ou edite elementos existentes aninhados na `<system.serviceModel>` seção do arquivo de configuração.

O editor dá suporte ao IntelliSense e impõe a conformidade do esquema. A saída resultante é garantida para estar em conformidade com o esquema do arquivo de configuração e para ter valores de dados sintaticamente corretos. No entanto, o editor não garante que o arquivo de configuração seja semanticamente válido. Em outras palavras, o editor não garante que o arquivo de configuração possa funcionar com o serviço que ele configura.

> [!CAUTION]
> O editor não pode limpar um elemento de configuração do arquivo de configuração depois que você modificou o elemento. Por exemplo, se você usar o editor para definir o nome do ponto de extremidade como uma cadeia de caracteres não vazia e salvá-lo, o arquivo de configuração terá o seguinte conteúdo, conforme mostrado no exemplo a seguir.
>
> `<endpoint binding="basicHttpBinding" name="somename" />`
>
> Se você tentar remover o nome definindo-o como uma cadeia de caracteres vazia e salvar o arquivo, o arquivo de configuração ainda conterá o `name` atributo, conforme mostrado no exemplo a seguir.
>
> `<endpoint binding="basicHttpBinding" name="" />`
>
> Para limpar o atributo, você deve editar manualmente o elemento usando outro editor de texto.
>
> Você deve ser especialmente cuidadoso com esse problema ao usar o `issueToken` elemento do comportamento do `clientCredential` ponto de extremidade. Especificamente, o `address` atributo de seu `localIssuer` subelemento não deve ser uma cadeia de caracteres vazia. Se você tiver modificado o `address` atributo usando o editor de configuração e quiser removê-lo completamente, deverá fazer isso usando uma ferramenta diferente do editor. Caso contrário, o atributo contém uma cadeia de caracteres vazia e seu aplicativo gera uma exceção.

## <a name="using-the-configuration-editor"></a>Usando o editor de configuração

O editor de configuração de serviço pode ser encontrado no seguinte local de instalação do SDK do Windows:

C:\Arquivos de Programas\microsoft SDKs\Windows\v6.0\Bin\SvcConfigEditor.exe

Depois de iniciar o editor de configuração de serviço, você pode usar o menu **arquivo/abrir** para procurar o serviço ou o assembly que deseja gerenciar. Você pode abrir os arquivos de configuração diretamente, procurar serviços WCF/COM + e abrir arquivos de configuração para serviços hospedados na Web.

A interface do usuário do editor de configuração de serviço é dividida nas seguintes áreas:

- Painel exibição de árvore, que exibe elementos de configuração em uma estrutura de árvore à esquerda. Você pode executar operações na árvore clicando com o botão direito do mouse nos nós.

- Painel de tarefas, que exibe tarefas comuns para os elementos atuais no canto inferior esquerdo da janela

- Painel de detalhes, que exibe as configurações detalhadas do nó de configuração selecionado no modo de exibição de árvore à direita.

### <a name="opening-a-configuration-file"></a>Abrindo um arquivo de configuração

1. Inicie o editor de configuração de serviço usando uma janela de comando para navegar até o local de instalação do WCF e digite `SvcConfigEditor.exe` .

2. No menu **arquivo** , selecione **abrir** e clique no tipo de arquivo que você deseja gerenciar.

3. Na caixa de diálogo **abrir** , navegue até o arquivo específico que você deseja gerenciar e clique duas vezes nele.

O visualizador segue automaticamente o caminho de mesclagem de configuração e cria uma exibição da configuração mesclada. Por exemplo, a configuração real de um serviço não hospedado é uma combinação de Machine.config e App.config. Todas as alterações são aplicadas ao arquivo ativo no SvcConfigEditor. Se você quiser editar um arquivo específico no caminho de mesclagem de configuração, deverá abri-lo diretamente.

> [!NOTE]
> O editor de configuração recarrega o arquivo de configuração aberto no momento quando o último é modificado fora do editor. Quando isso acontece, todas as alterações que não são permanentemente salvas dentro do editor são perdidas. Se o recarregamento ocorrer consistentemente, a causa mais provável é um serviço que acessa constantemente o arquivo de configuração, por exemplo, um software antivírus em execução em segundo plano. Para resolver isso, verifique se o editor de configuração é o único processo que pode acessar o arquivo quando ele é aberto.

### <a name="services"></a>Serviços

O nó **Serviços** exibe todos os serviços atualmente atribuídos no arquivo de configuração. Cada subnó na árvore corresponde a um subelemento do `services` elemento <> no arquivo de configuração.

Ao clicar no nó **Serviços** , você pode exibir ou executar tarefas na página Resumo do serviço no painel de **detalhes** .

#### <a name="creating-a-new-service-configuration"></a>Criando uma nova configuração de serviço

Você pode criar uma nova configuração de serviço das seguintes maneiras:

- Usando um assistente: clique no link **criar um novo serviço...** no painel de tarefas ou na página Resumo para iniciar o assistente. Você também pode fazer isso no menu **arquivo** -> **Adicionar novo item**.

- Criar manualmente: você pode clicar com o botão direito do mouse no nó **Serviços** e escolher **novo serviço**.

#### <a name="creating-a-new-service-endpoint-configuration"></a>Criando uma nova configuração de ponto de extremidade de serviço

Você pode criar uma nova configuração de ponto de extremidade de serviço das seguintes maneiras:

- Criar usando um assistente: clique no link **criar um novo ponto de extremidade de serviço...** no painel de tarefas ou na página Resumo para iniciar o assistente. Você também pode fazer isso no menu **arquivo** -> **Adicionar novo item**.

- Criar manualmente: depois de criar um serviço, clique com o botão direito do mouse no nó **pontos** de extremidade e escolha "**novo ponto de extremidade de serviço**".

#### <a name="editing-a-service-configuration"></a>Editando uma configuração de serviço

1. Clique em um nó de **serviço** .

2. Edite as configurações nas grades de propriedade.

#### <a name="editing-a-service-endpoint-configuration"></a>Editando uma configuração de ponto de extremidade de serviço

1. Clique em um nó de **ponto de extremidade de serviço** .

2. Edite as configurações nas grades de propriedade.

#### <a name="adding-a-base-address"></a>Adicionando um endereço base

1. Clique no nó **host** .

2. Clique no botão **Novo...** na seção **endereços base** .

3. Digite o URI do endereço base na caixa de diálogo.

4. Clique em **OK**.

> [!NOTE]
> Não é possível editar o valor de [\<baseAddressPrefixFilters>](../configure-apps/file-schema/wcf/baseaddressprefixfilters.md) dentro desta ferramenta. Para adicionar ou modificar esse elemento, você deve usar um editor de texto ou o Visual Studio.

### <a name="client"></a>Cliente

O nó **cliente** exibe todos os pontos de extremidade do cliente no arquivo de configuração. Cada subnó na árvore corresponde a um subelemento do `client` elemento <> no arquivo de configuração.

Ao clicar no nó **cliente** , você pode exibir ou executar tarefas na **página Resumo** do cliente no painel de **detalhes**.

#### <a name="creating-a-new-client-endpoint-configuration"></a>Criando uma nova configuração de ponto de extremidade de cliente

Você pode criar uma nova configuração de ponto de extremidade de cliente das seguintes maneiras:

- Criar por assistente: clique no link **criar um novo cliente...** no **painel de tarefas** na parte inferior esquerda da janela ou **página Resumo** para iniciar o assistente. Você também pode fazer isso no menu **arquivo** -> **Adicionar novo item**. O assistente solicita que você aponte para o local da configuração do serviço, do qual a configuração do cliente é gerada. Você pode escolher o ponto de extremidade de serviço ao qual se conectar.

- Criar manualmente: clique com o botão direito do mouse no nó **pontos de extremidade** em **cliente**e escolha **novo ponto de extremidade do cliente**.

#### <a name="editing-a-client-endpoint-configuration"></a>Editando uma configuração de ponto de extremidade do cliente

1. Clique em um nó de **ponto de extremidade do cliente** .

2. Edite as configurações nas grades de propriedade.

### <a name="standard-endpoint"></a>Ponto de extremidade padrão

Os pontos de extremidade padrão são pontos de extremidade especializados que têm um ou mais aspectos do endereço, contrato e Associação definidos para valores padrão.

Essas definições de configuração são armazenadas no nó do **ponto de extremidade padrão** . O nó do **ponto de extremidade padrão** exibe todas as configurações do ponto de extremidade padrão no arquivo de configuração. Cada subnó na árvore corresponde a um subelemento no `<standardEndpoints>` elemento no arquivo de configuração.

Ao clicar no nó do **ponto de extremidade padrão** , você pode exibir ou executar tarefas na **página Resumo** do ponto de extremidade padrão no painel de **detalhes**.

#### <a name="creating-a-new-standard-endpoint-configuration"></a>Criando uma nova configuração de ponto de extremidade padrão

Você pode criar uma nova configuração de ponto de extremidade padrão das seguintes maneiras:

- Clique com o botão direito do mouse no nó do **ponto de extremidade padrão** e selecione **nova configuração de ponto de extremidade padrão...** Selecione o tipo de associação na caixa de diálogo e clique em **OK**.

- Selecione o nó do **ponto de extremidade padrão** e clique em **nova configuração de ponto de extremidade padrão...** no **painel de tarefas** na parte inferior esquerda da janela.

A caixa de diálogo **criando um novo ponto de extremidade padrão** exibe e lista todos os tipos de ponto de extremidade padrão registrados.

#### <a name="viewing-and-editing-a-standard-endpoint-configuration"></a>Exibindo e editando uma configuração de ponto de extremidade padrão

Você pode abrir uma configuração de ponto de extremidade padrão para exibição e edição das seguintes maneiras:

- Clique para expandir o nó de **ponto de extremidade padrão** e clique no respectivo subnó de ponto de extremidade.

- Clique no nó **ponto de extremidade padrão** e clique no respectivo ponto de extremidade no painel de detalhes.

Os atributos para o ponto de extremidade são mostrados no painel direito para edição.

#### <a name="deleting-a-standard-endpoint-configuration"></a>Excluindo uma configuração de ponto de extremidade padrão

Você pode excluir uma configuração de ponto de extremidade padrão das seguintes maneiras:

- Clique para expandir o nó do **ponto de extremidade padrão** e clique com o botão direito do mouse no respectivo subnó de ponto de extremidade. Use o comando de contexto **Excluir configuração de ponto de extremidade padrão** para excluir o ponto de extremidade.

- Clique no nó do **ponto de extremidade padrão** . No painel de **tarefas** , clique em **Excluir configuração de ponto de extremidade padrão**.

Se o ponto de extremidade padrão estiver em uso, uma mensagem de aviso será exibida quando você tentar excluí-lo: **o ponto de extremidade padrão está em uso. Se você excluí-lo agora, certifique-se de excluir todas as suas referências em outras partes da configuração (por exemplo, no ponto de extremidade de serviço ou ponto de extremidade do cliente). Caso contrário, a configuração será inválida e não poderá ser aberta da próxima vez. Tem certeza de que deseja excluir o ponto de extremidade padrão? "**

### <a name="binding"></a>Associação

As configurações de associação são usadas para configurar associações em pontos de extremidade. Essas definições de configuração são armazenadas no nó de **Associação** . Os pontos de extremidade fazem referência a configurações de associação por nome e vários pontos de extremidade podem fazer referência a uma única configuração de associação.

O nó **associações** exibe todas as configurações de associação no arquivo de configuração. Cada subnó na árvore corresponde a um subelemento no `bindings` elemento <> no arquivo de configuração.

Ao clicar no nó **associações** , você pode exibir ou executar tarefas na **página Resumo** da associação no **painel de detalhes**.

#### <a name="creating-a-new-binding-configuration"></a>Criando uma nova configuração de associação

Você pode criar uma nova configuração de Associação das seguintes maneiras.

- Clique com o botão direito do mouse no nó **associações** e selecione **nova configuração de associação**... Selecione o tipo de associação na caixa de diálogo e clique em **OK**.

- Selecione o nó **associações** e clique em **nova configuração de associação**... no **painel de tarefas** na parte inferior esquerda da janela.

- Na página Resumo do serviço ou do cliente, clique **em clique para criar** no campo **configuração de associação** para criar uma configuração de associação para o ponto de extremidade correspondente.

#### <a name="adding-binding-element-extensions-to-a-custom-binding"></a>Adicionando extensões de elemento de associação a uma associação personalizada

1. Selecione a associação à qual você deseja adicionar um elemento de extensão.

2. Clique em **Adicionar**.

3. Na lista de extensões disponíveis, selecione a extensão de elemento de associação que você deseja adicionar. Para selecionar vários itens, pressione a tecla CTRL simultaneamente.

4. Clique em **Adicionar**.

#### <a name="adjusting-the-extension-position-in-a-custom-binding"></a>Ajustando a posição da extensão em uma associação personalizada

Uma associação personalizada é uma coleção de elementos de associação que formam uma pilha. Cada elemento de ligação na pilha tem suas próprias definições de configuração. A ordem das extensões de elemento de associação em uma associação personalizada indica suas posições na pilha. Os elementos na parte superior da pilha são aplicados primeiro. Para alterar a ordem:

1. Selecione o nó de associação personalizado.

2. Selecione um elemento de extensão de associação na seção **posição da extensão do elemento de associação** .

3. Use o botão para **cima** ou **para baixo** no lado esquerdo da lista para alterar a posição do elemento selecionado.

#### <a name="editing-the-configuration-of-binding-element-extensions-in-a-custom-binding"></a>Editando a configuração de extensões de elemento de associação em uma associação personalizada

1. Selecione o nó de associação na árvore.

2. Selecione a associação personalizada que contém o elemento que você deseja editar.

3. Selecione a extensão de elemento de associação que você deseja editar. As configurações do elemento aparecem no painel direito, onde podem ser editadas.

### <a name="diagnostics"></a>Diagnósticos

O nó **diagnóstico** exibe todas as configurações de diagnóstico no arquivo de configuração. Ele permite ativar ou desativar os contadores de desempenho, habilitar ou desabilitar Instrumentação de Gerenciamento do Windows (WMI), configurar o rastreamento do WCF e configurar o log de mensagens do WCF. As configurações no nó **diagnóstico** correspondem à `system.diagnostics` seção <> e à `<diagnostics>` seção no arquivo de `<system.serviceModel>` configuração.

Ao clicar no nó **diagnóstico** , você pode exibir ou executar tarefas na **página Resumo** do diagnóstico no painel de **detalhes**.

#### <a name="configuring-performance-counters-and-wmi"></a>Configurando contadores de desempenho e WMI

1. Clique no nó **diagnóstico** .

2. Clique em **alternar contadores de desempenho**. O contador de desempenho tem três Estados: desativado (padrão), somente de um e de todos. Clicar no link alterna a configuração entre esses três Estados.

#### <a name="configuring-wmi-provider"></a>Configurando o provedor WMI

1. Clique no nó **diagnóstico** .

2. Para habilitar o provedor WMI, clique no link **habilitar provedor WMI** .

#### <a name="enabling-wcf-tracing"></a>Habilitando o rastreamento do WCF

Você pode criar um arquivo de rastreamento do WCF com propriedades padrão ou configurar um arquivo de rastreamento personalizado.

1. Clique no nó **diagnóstico** .

2. Clique em **Habilitar rastreamento**.

3. Clique no link **nível de rastreamento** para ajustar o nível de rastreamento. Há seis níveis de rastreamento: desativado, crítico, erro, aviso, informações e detalhado. A opção **rastreamento** de atividade e **propagar atividade** permite que você use o recurso de rastreamento de atividade do WCF.

4. Clique no nome do ouvinte de rastreamento para especificar o arquivo de rastreamento e as opções.

#### <a name="enabling-wcf-logging"></a>Habilitando o log do WCF

Você pode criar um arquivo de rastreamento do WCF com propriedades padrão ou configurar um arquivo de rastreamento personalizado.

1. Clique no nó **diagnóstico** .

2. Clique em **habilitar log de mensagens**.

3. Clique no link **nível de log** para ajustar o nível de log. Há três níveis de log: malformados, de serviço e de transporte.

4. Clique no nome do ouvinte para especificar o arquivo de log e as opções.

> [!NOTE]
> Se você quiser que os logs de rastreamento e de mensagem sejam liberados automaticamente quando seu aplicativo for fechado, habilite a opção de **liberação automática** .

A **Diagnostics** **página Resumo** do diagnóstico permite que você realize as tarefas mais comuns na configuração de diagnóstico. No entanto, se você quiser editar manualmente as configurações de ouvintes e fontes, será necessário expandir o nó **diagnóstico** e editar as configurações em **log de mensagens**, **ouvintes** e nó de **fontes** .

#### <a name="enabling-wcf-custom-tracing-or-message-logging"></a>Habilitando o rastreamento personalizado do WCF ou o log de mensagens

1. Clique no nó **diagnóstico** e expanda-o.

2. Clique com o botão direito do mouse no nó **ouvintes** e selecione **novo ouvinte**.

3. Digite o nome do arquivo de rastreamento no campo **initdata** . Você pode clicar no botão "..." botão para navegar até um caminho.

4. Clicar na linha **TypeName** exibe um "..." Button. Clique neste botão para abrir o **navegador do tipo de ouvinte de rastreamento**, que você pode usar para localizar ouvintes de rastreamento pré-configurados que já estão instalados.

5. Observe a seção **origem** . Clique em **Adicionar** nesta seção para abrir uma caixa de diálogo com um menu suspenso, que lista as fontes de rastreamento disponíveis. Selecione uma origem de rastreamento e clique em **OK**.

6. Para editar as configurações de log de mensagens, clique no nó **log de mensagens** . Você pode editar as configurações na grade de propriedades.

### <a name="advanced"></a>Avançado

#### <a name="behaviors"></a>Comportamentos

O nó **comportamentos** exibe os comportamentos atualmente definidos no arquivo de configuração.

As configurações de comportamento são usadas para configurar comportamentos de pontos de extremidade e serviços. Essas definições de configuração são armazenadas no nó **avançado** em comportamentos de **serviço** e comportamentos de **ponto de extremidade**. Os comportamentos de serviço são usados pelos serviços; enquanto os comportamentos de ponto de extremidade por pontos de extremidades.

Os comportamentos são uma coleção de elementos de extensão que para uma pilha. O elemento na parte superior da pilha é aplicado primeiro. Cada elemento de extensão pode ter sua própria configuração.

##### <a name="creating-a-new-behavior-configuration"></a>Criando uma nova configuração de comportamento

Você pode criar uma nova configuração de comportamento de duas maneiras.

- Clique com o botão direito do mouse em um dos nós de comportamento e selecione "**nova configuração de comportamento...**

- Selecione um dos nós de comportamento e clique na **nova configuração de comportamento**... no **painel de tarefas** na parte inferior esquerda da janela.

##### <a name="adding-behavior-element-extensions-to-a-behavior"></a>Adicionando extensões de elemento de comportamento a um comportamento

1. Selecione um dos nós de comportamento.

2. Selecione o comportamento que você deseja editar.

3. Clique em **Adicionar**.

4. Na lista de extensões disponíveis, selecione a extensão de elemento de comportamento que você deseja adicionar.

5. Clique em **Adicionar**.

##### <a name="adjusting-the-extension-position-in-a-behavior"></a>Ajustando a posição da extensão em um comportamento

Os comportamentos são coleções de elementos que formam uma pilha. Cada elemento na pilha tem sua própria configuração. A ordem das extensões de elemento de comportamento em um comportamento indica suas posições na pilha. Os elementos na parte superior da pilha são aplicados primeiro. Para alterar a ordem:

1. Selecione um dos nós de comportamento.

2. Selecione o comportamento que você deseja editar.

3. Selecione um elemento de extensão de comportamento na seção **posição de extensão do elemento de comportamento** .

4. Use o botão para **cima** ou **para baixo** no lado esquerdo da lista para alterar a posição do elemento selecionado.

##### <a name="editing-the-configuration-of-behavior-element-extensions"></a>Editando a configuração das extensões de elemento de comportamento

1. Selecione um dos nós de comportamento na árvore.

2. Selecione o comportamento que contém o elemento que você deseja editar.

3. Selecione a extensão de elemento de comportamento que você deseja editar. As configurações do elemento aparecem no painel direito, onde podem ser editadas.

#### <a name="protocolmapping"></a>ProtocolMapping

Esta seção permite que você defina tipos de associação padrão para protocolos diferentes, como http, TCP, MSMQ ou net. pipe por meio de mapeamento definido entre esquemas de endereço de protocolo e as associações possíveis. Você também pode adicionar novos mapeamentos a outros protocolos.

#### <a name="extensions"></a>Extensões

Novas extensões de associação, extensões de elemento de associação, extensões de ponto de extremidade padrão e extensões de comportamento podem ser registradas para uso na configuração do WCF. As extensões são pares de nome/tipo. O nome define o nome da extensão na configuração, enquanto o tipo implementa a extensão. Há quatro tipos de extensões:

- As extensões de associação definem um tipo de associação inteiro. Exemplo: `basicHttpBinding`.

- As extensões de elemento de associação definem um elemento de uma associação. Exemplo: `textMessageEncoding`.

- As extensões de ponto de extremidade padrão definem um ponto de extremidade padrão inteiro. Exemplo: `discoveryEndpoint`.

- As extensões de elemento de comportamento definem um elemento de um comportamento. Exemplo: `clientVia`.

 As extensões que foram registradas na configuração podem ser usadas como qualquer outro componente WCF do mesmo tipo.

##### <a name="adding-a-new-extension"></a>Adicionando uma nova extensão

Selecione um dos nós de extensão nos nós avançados:

1. Clique em **Novo**.

2. Insira um nome e um tipo.

3. Clique em **OK**.

4. A extensão agora aparece no local apropriado no editor. Por exemplo, se você adicionar uma extensão de elemento de comportamento, ela aparecerá na lista de extensões disponíveis.

#### <a name="hosting-environment"></a>Ambiente de hospedagem

Esta seção permite que você defina as configurações de instanciação para o ambiente de Hospedagem de serviço.

### <a name="creating-a-configuration-file-using-the-wizard"></a>Criando um arquivo de configuração usando o assistente

Uma maneira de criar um novo arquivo de configuração é usar o assistente de novo elemento de serviço. O assistente localiza os tipos de serviço instalados e outros elementos compatíveis com o WCF no computador, incluindo diretórios virtuais do COM+ e hospedados na Web, e os carrega para tornar a criação da configuração muito mais simplificada.

#### <a name="creating-a-configuration-file"></a>Criando um arquivo de configuração

1. Inicie o editor de configuração de serviço usando uma janela de comando para navegar até o local de instalação do WCF e digite `SvcConfigEditor.exe` .

2. No menu **arquivo** , selecione **abrir** e clique em **executável**, **serviço com+** ou **serviço Webhost**, dependendo do tipo de arquivo de configuração que você deseja criar.

3. Na caixa de diálogo **abrir** , navegue até o arquivo específico para o qual você deseja criar um arquivo de configuração e clique duas vezes nele.

4. No menu **arquivo** , aponte para **Adicionar novo item** e clique em **serviço**. O assistente de novo elemento de serviço é aberto.

5. Siga as etapas no Assistente para criar o novo serviço.

> [!NOTE]
> Se você quiser usar o NetPeerTcpBinding do arquivo de configuração gerado pelo assistente, será necessário adicionar manualmente um elemento de configuração de associação e modificar o `mode` atributo de seu `security` elemento para "None".

## <a name="configuring-com"></a>Configurando COM+

O editor de configuração de serviço permite que você crie um novo arquivo de configuração para um aplicativo existente do COM+ ou edite uma configuração existente do COM+. O nó do **contrato com** só é visível quando a `comContract` seção <> existe no arquivo de configuração.

### <a name="creating-a-new-com-configuration"></a>Criando uma nova configuração COM+

Antes de criar uma nova configuração COM+, verifique se seu aplicativo COM+ está instalado nos serviços de componentes e registrado no GAC (cache de assembly global).

1. Selecione o menu **arquivo** -> **integrar**o  ->  **aplicativo com+.** Esta operação fecha o arquivo aberto atual. Se houver dados não salvos no arquivo atual, uma caixa de diálogo Salvar será exibida. O **Assistente de integração com+** é então iniciado.

2. Na primeira página, selecione o aplicativo COM+ na árvore. Se você não encontrar seu aplicativo COM+ na árvore, verifique se ele está instalado nos serviços de componentes e registrado no GAC (cache de assembly global).

3. Na próxima página, selecione quais métodos você deseja expor como serviços WCF. Todos os métodos com suporte no aplicativo COM+ são exibidos e selecionados por padrão.

4. Escolha um método de hospedagem.

5. Defina outras configurações de acordo com os guias do assistente.

6. O editor de configuração de serviço utiliza ComSvcConfig.exe em segundo plano para gerar o arquivo de configuração. Depois que isso for concluído, você poderá exibir um resumo e sair do assistente. O arquivo de configuração gerado é aberto para que você possa editá-lo diretamente.

### <a name="editing-an-existing-com-configuration"></a>Editando uma configuração COM+ existente

1. Selecione o menu **arquivo** -> **abrir**  ->  **serviço com+**...

2. Selecione o serviço COM+ que você deseja editar na lista.

3. Edite as definições de configuração no nó **contratos com** .

    > [!NOTE]
    > Você também pode abrir e editar diretamente um arquivo de configuração que contenha contratos COM.

## <a name="security"></a>Segurança

Não há garantia de que um arquivo de configuração de serviço gerado pelo editor de configuração seja seguro. Consulte a documentação de [segurança](./feature-details/security.md) para saber como proteger seus serviços WCF.

Além disso, o editor de configuração só pode ser usado para ler e gravar elementos válidos de configuração do WCF. A ferramenta ignora elementos definidos pelo usuário em conformidade com o esquema. Ele também não tenta remover esses elementos do arquivo de configuração ou determinar seus efeitos nos elementos conhecidos do WCF. É responsabilidade do usuário determinar se esses elementos representam uma ameaça ao aplicativo ou ao sistema.
