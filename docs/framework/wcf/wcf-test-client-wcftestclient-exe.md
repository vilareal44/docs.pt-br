---
title: Cliente de Teste do WCF (WcfTestClient.exe)
description: Saiba mais sobre o WCF Test Client, que fornece testes de serviço contínuos quando combinados com o host de serviço WCF. Enviar valores de teste de cliente e exibir respostas de serviço.
ms.date: 03/30/2017
ms.assetid: d4302855-677f-4640-aa90-c5d785d72fb7
ms.openlocfilehash: 4f636698c538809f89ee356159839a37b73adb57
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245655"
---
# <a name="wcf-test-client-wcftestclientexe"></a>Cliente de Teste do WCF (WcfTestClient.exe)
Windows Communication Foundation (WCF) Test Client (WcfTestClient.exe) é uma ferramenta de GUI que permite aos usuários inserir parâmetros de teste, enviar essa entrada para o serviço e exibir a resposta que o serviço envia de volta. Ele fornece uma experiência de teste de serviço sem interrupção quando combinado com o host de serviço do WCF.

Normalmente, você pode encontrar o cliente de teste do WCF (WcfTestClient.exe) no seguinte local: `C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE` -a Comunidade pode ser uma "empresa", "Professional" ou "Community", dependendo de qual nível do Visual Studio está instalado.

## <a name="scenarios-for-using-test-client"></a>Cenários para uso do Cliente de Teste

As seções a seguir discutem os cenários mais comuns nos quais você pode usar o cliente de teste do WCF para simplificar o processo de desenvolvimento.

### <a name="inside-visual-studio"></a>No Visual Studio

#### <a name="wcf-service-host-starts-wcf-test-client-with-a-single-service"></a>O Host de Serviço WCF começa o Cliente de Teste do WCF com o um único serviço

Depois de criar um novo projeto de serviço WCF e pressionar F5 para iniciar o depurador, o host de serviço WCF começa a hospedar o serviço em seu projeto. Em seguida, o cliente de teste do WCF abre e exibe uma lista de pontos de extremidade de serviço definidos no arquivo de configuração. Você pode testar os parâmetros e chamar o serviço, e repetir esse processo para testar e validar continuamente o serviço.

#### <a name="wcf-service-host-starts-wcf-test-client-with-multiple-services"></a>O Host de Serviço WCF começa o Cliente de Teste do WCF com vários serviços

Você também pode usar o cliente de teste do WCF para ajudar a depurar um projeto de serviço que contém vários serviços. Quando o cliente de teste do WCF é aberto, ele itera automaticamente a lista de serviços em seu projeto e os abre para teste.

### <a name="outside-visual-studio"></a>Fora do Visual Studio

Você também pode invocar o cliente de teste do WCF (WcfTestClient.exe) fora do Visual Studio para testar um serviço arbitrário na Internet. Para localizar a ferramenta, vá para o seguinte local:

`C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE`(onde a Comunidade pode ser uma de "Enterprise", "Professional" ou "Community", dependendo de qual nível do Visual Studio está instalado no computador)

Para usar a ferramenta, clique duas vezes no nome do arquivo para abri-lo neste local ou inicie-a em uma linha de comando.

O cliente de teste do WCF usa um número arbitrário de URIs como argumentos de linha de comando.  Estes são os URIs de serviços que podem ser testados.

`wcfTestClient.exe URI1 URI2 …`

Depois que a janela do cliente de teste do WCF for aberta, clique em **arquivo** -> **Adicionar serviço**e insira o endereço do ponto de extremidade do serviço que você deseja abrir.

## <a name="wcf-test-client-user-interface"></a>Interface do usuário do Cliente de Teste do WCF

Você pode usar o cliente de teste do WCF com um único serviço ou vários serviços.

### <a name="service-operations"></a>Operações de serviço

O painel esquerdo da janela principal do cliente de teste do WCF lista todos os serviços disponíveis, juntamente com seus respectivos pontos de extremidade e operações.

Clicando duas vezes em uma operação, você pode exibir seu conteúdo no painel direito em um nova guia com o nome da operação.

O painel esquerdo também lista arquivos de configuração de cliente. Clique duas vezes em alguns dos itens para exibir o conteúdo do arquivo em uma nova janela com guias no painel direito.

### <a name="entering-test-parameters"></a>Inserindo parâmetros de teste

Para exibir os parâmetros de teste, clique duas vezes em uma operação para abri-la no painel direito. Os parâmetros são mostrados na exibição **formatada** por padrão e você pode inserir valores arbitrários para os parâmetros para testar o serviço.

Para exibir o XML da mensagem, clique em **XML**. Para enviá-los ao serviço, clique em **invocar**.

Para um parâmetro de conjunto de um, clique em **...** botão ao lado de **Editar...** para editá-lo em uma nova janela mostrando DataGrid. Observe a aparência dos botões **copiar conjunto de DataSet** e **colar conjunto** de notícias. Se o esquema do objeto DataSet for desconhecido na primeira edição, o DataGrid estará vazio. Você precisa colar um objeto DataSet com o mesmo esquema no objeto atual no DataGrid. (Observe que você precisa copiar o esquema de outro lugar antes da operação de colar.) Você também pode copiar um objeto DataSet para uso futuro clicando no botão **Copy DataSet** .

A resposta do serviço aparece abaixo dos parâmetros de teste.

> [!NOTE]
> Se o valor de retorno esperado for uma cadeia de caracteres, o resultado será exibido como uma cadeia de caracteres entre aspas, mesmo que a entrada não tenha sido fornecida entre aspas.

Se você tiver definido uma operação específica como unidirecional quando criou o contrato do serviço, nenhuma resposta do serviço será exibida. Assim que a mensagem for enfileirada para entrega, uma caixa de diálogo pop-up será exibida notificando que a mensagem foi enviada com êxito.

### <a name="session-support"></a>Suporte de sessão

A caixa de seleção **Iniciar um novo proxy** na guia de uma operação de serviço permite alternar o suporte de sessão. Por padrão, essa caixa está desmarcada.

Quando você insere parâmetros de teste para uma operação específica (ou outra operação no mesmo ponto de extremidade de serviço) e clica em **invocar** várias vezes com a caixa de seleção desmarcada, essas operações compartilham um proxy e o status do serviço é persistido em várias operações.

Se a caixa de seleção **Iniciar um novo proxy** estiver marcada, um novo proxy será iniciado para cada **invocação**, o cenário de sessão anterior será encerrado e o status do serviço será redefinido.

### <a name="editing-client-configuration"></a>Editando a configuração do cliente

O painel esquerdo da janela principal do cliente de teste do WCF lista os arquivos de configuração do cliente. Clique duas vezes em alguns dos itens para exibir o conteúdo do arquivo no painel direito.

#### <a name="edit-with-service-configuration-editor"></a>Editar com o Service Configuration Editor

Clique com o botão direito do mouse em **arquivo de configuração** no painel esquerdo e selecione o menu de contexto **Editar com SvcConfigEditor**. O Service Configuration Editor é iniciado com o conteúdo de configuração do cliente. Você pode editar a configuração e salvá-la na ferramenta.

Depois de salvar o arquivo no editor de configuração de serviço, o cliente de teste do WCF exibe uma mensagem de aviso para informar que o arquivo foi modificado fora e pergunta se você deseja recarregá-lo.

Se você selecionar **Sim**, o conteúdo de configuração na guia "Client.dll.config" refletirá as alterações feitas no editor.

Se você selecionar **não**, o conteúdo de configuração na guia "Client.dll.config" permanecerá inalterado e o conteúdo modificado será salvo automaticamente no arquivo de origem.

#### <a name="restore-to-default-configuration"></a>Restaurar para a configuração padrão

Se você quiser cancelar todas as alterações e restaurar para a configuração do cliente padrão, clique com o botão direito do mouse em **arquivo de configuração** no painel esquerdo e selecione o menu de contexto **restaurar para configuração padrão**. O valor de configuração padrão é carregado e o conteúdo na guia "Client.dll.config" é restaurado.

#### <a name="validate-changes"></a>Validar alterações

Quando as alterações salvas estão sendo carregadas no cliente de teste do WCF, a configuração é verificada quanto à validade no esquema do WCF. Se forem encontrados erros, uma caixa de diálogo será exibida para mostrar os detalhes do erro.

Durante a geração de proxy, a compilação binária ou a invocação de serviço, os itens de menu que dão suporte à edição (ou seja, "Editar...", "restaurar..." e assim por diante) são desabilitados. A invocação de serviço também é desabilitada ao carregar a configuração atualizada no cliente de teste do WCF.

#### <a name="persist-client-configuration"></a>Persistir a configuração do cliente

A guia **ferramentas** -> **Opções** -> **configuração do cliente** contém uma opção **sempre regenerar configuração ao iniciar serviços** , que é habilitada por padrão. Essa opção especifica que cada vez que o cliente de teste do WCF carrega um serviço, ele regenera um arquivo de configuração com base no contrato de serviço e nos arquivos de App.config de serviço mais recentes.

Se você editou a configuração do cliente para seu serviço WCF e deseja sempre usar esse arquivo atualizado para depurar seu serviço, você pode desmarcar a opção **regenerar** . Ao fazer isso, mesmo quando você atualiza o serviço e reabre o cliente de teste do WCF, o arquivo de Client.dll.config é aquele que você atualizou anteriormente em vez de um gerado novamente com base no serviço atualizado.

No entanto, talvez você precise editar o arquivo de configuração para torná-lo consistente com o proxy regenerado. Se o proxy e o arquivo de configuração regenerados forem incompatíveis devido a um serviço atualizado, ocorrerão erros quando o serviço for chamado.

> [!CAUTION]
> Se você tiver modificado o arquivo de configuração do cliente e o selecionar para reutilização no futuro, poderá localizar o arquivo no seguinte local:
>
> \Documents and Settings \\ [User Account] \Meus Documents\Test Client Projects.
>
> Todas as informações de credenciais atualizadas armazenadas no arquivo de configuração do cliente são protegidas pela ACL (lista de controle de acesso) dessa pasta.

### <a name="adding-removing-and-refreshing-services"></a>Adicionando, removendo e atualizando serviços

#### <a name="add-service"></a>Adicionar serviço

Clique em **arquivo** -> **Adicionar serviço** para adicionar um serviço ao cliente de teste do WCF. Em seguida, é necessário digitar o URI (endereço do ponto de extremidade) do serviço a ser adicionado. O endereço do serviço pode ser um endereço MEX ou um endereço WSDL.

Você também pode encontrar uma lista de 10 pontos de extremidade dos serviços adicionados recentemente no submenu **Serviços recentes** . Se você selecionar um deles, o serviço especificado será adicionado ao cliente de teste do WCF.

Você também pode clicar com o botão direito do mouse na raiz da árvore de serviço **meus projetos de serviço**e selecionar **Adicionar serviço** para obter o mesmo resultado.

Durante a geração do proxy, da compilação binária ou da chamada do serviço, os itens de menu que dão suporte à edição de um serviço estão desabilitados. A chamada de serviço também está desabilitada.

#### <a name="remove-service"></a>Remover serviço

Clique com o botão direito do mouse na raiz de serviço do serviço a ser removido e selecione **remover serviço** para remover um serviço do cliente de teste do WCF.

Durante a geração do proxy, da compilação binária ou da chamada do serviço, os itens de menu que dão suporte à remoção de um serviço estão desabilitados. A chamada de serviço também está desabilitada.

#### <a name="refresh-service"></a>Atualizar serviço

Se for feita uma alteração no serviço enquanto o cliente de teste do WCF estiver em execução e você quiser garantir que a implementação do cliente de teste do WCF para esse serviço esteja atualizada, clique com o botão direito do mouse na raiz do serviço e selecione **Atualizar serviço**. Observe que depois da atualização, o status do serviço é redefinido.

Durante a geração do proxy, da compilação binária ou da chamada do serviço, os itens de menu que dão suporte à atualização de um serviço estão desabilitados. A chamada de serviço também está desabilitada.

## <a name="location-of-files-generated-by-the-test-client"></a>Local dos arquivos gerados pelo Cliente de Teste

Por padrão, o cliente de teste do WCF armazena os arquivos de configuração e o código do cliente gerados na pasta "projetos de cliente do%appdata%\Local\temp\Test". Essa pasta é excluída após a saída do cliente de teste do WCF. Se um arquivo de configuração for modificado no cliente de teste do WCF e a opção **sempre regenerar configuração ao iniciar serviços** estiver desabilitada, o arquivo modificado será copiado para a pasta "CachedConfig" em "meus projetos de cliente Documents\Test" com um arquivo XML de mapeamento (metadados-endereço-para-nome-do-arquivo) como um índice.

Você também pode iniciar o cliente de teste do WCF em uma linha de comando, usar a `/ProjectPath` opção para especificar um novo caminho desejado para armazenar arquivos gerados ou usar a `/RestoreProjectPath` opção para restaurar o local padrão. A sintaxe é mostrada a seguir:

`wcfTestClient.exe /ProjectPath [desired location]`

A execução desse comando não abre o cliente de teste do WCF. Apenas o local da pasta é alterado. Você pode executar esse comando se o cliente de teste do WCF estiver em execução ou não. O novo local é aplicado quando o cliente de teste do WCF é reiniciado. As informações de local podem ser salvas no registro ou no arquivo WcfTestClient.exe. Option na pasta "%appdata%\Local\temp\Test Client Projects".

## <a name="features-supported-by-wcf-test-client"></a>Recursos suportados pelo Cliente de Teste do WCF

Veja a seguir uma lista de recursos com suporte pelo cliente de teste do WCF:

- Chamada de serviço: solicitação/resposta e mensagem unidirecional.

- Associações: todas as associações suportadas por Svcutil.exe.

- Sessão de controle.

- Contrato de mensagens.

- Serialização XML.

Veja a seguir uma lista de recursos sem suporte pelo cliente de teste do WCF:

- Tipos: <xref:System.IO.Stream>, <xref:System.ServiceModel.Channels.Message>, <xref:System.Xml.XmlElement>, <xref:System.Xml.XmlAttribute>, <xref:System.Xml.XmlNode>, tipos que implementam a interface de <xref:System.Xml.Serialization.IXmlSerializable>, incluindo o atributo <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> relacionado e os tipos <xref:System.Xml.Linq.XDocument> e <xref:System.Xml.Linq.XElement> e o tipo <xref:System.Data.DataTable> do ADO.NET.

- Contrato dúplex.

- Transação.

- Segurança: CardSpace, certificado e nome de usuário/senha.

- Associações: WSFederationbinding, algumas associações de contexto e associação HTTPS, WebHttpbinding (suporte a mensagem de resposta de Json).

## <a name="closing-wcf-test-client"></a>Fechando o Cliente de Teste do WCF

Você pode fechar o cliente de teste do WCF das seguintes maneiras:

- No menu **Arquivo** , clique em **Sair**. Como alternativa, na janela principal do cliente de teste do WCF, clique em **fechar**. Ambas as ações também desligam o host automático do serviço WCF e param o processo de depuração do Visual Studio se o cliente de teste do WCF foi iniciado pelo Visual Studio.

- Clique com o botão direito do mouse no ícone de **host do serviço WCF** na área de notificação e clique em **sair.** Isso desliga o cliente de teste do WCF e o host de início automático do serviço WCF e interrompe o processo de depuração do Visual Studio.

## <a name="see-also"></a>Veja também

- [Host de serviço do WCF (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)
