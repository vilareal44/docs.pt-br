---
title: Host de serviço do WCF (WcfSvcHost.exe)
description: Use o host de serviço WCF para hospedar e testar um serviço que você implementou. Você pode testar o serviço usando o cliente de teste do WCF ou seu próprio cliente.
ms.date: 03/30/2017
ms.assetid: 8643a63d-a357-4c39-bd6c-cdfdf71e370e
ms.openlocfilehash: efc9512766d2a9cc814083ab632226d98917bf4e
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245720"
---
# <a name="wcf-service-host-wcfsvchostexe"></a>Host de serviço do WCF (WcfSvcHost.exe)

O host de serviço do Windows Communication Foundation (WCF) (WcfSvcHost.exe) permite que você inicie o depurador do Visual Studio (F5) para hospedar e testar automaticamente um serviço que você implementou. Em seguida, você pode testar o serviço usando o cliente de teste do WCF (WcfTestClient.exe) ou seu próprio cliente, para localizar e corrigir possíveis erros.

## <a name="wcf-service-host"></a>Host de serviço WCF

O host de serviço WCF enumera os serviços em um projeto de serviço WCF, carrega a configuração do projeto e cria uma instância de um host para cada serviço que ele encontra. A ferramenta é integrada ao Visual Studio por meio do modelo de serviço do WCF e é invocada quando você começa a depurar seu projeto.

Usando o host de serviço do WCF, você pode hospedar um serviço WCF (em um projeto de biblioteca de serviço WCF) sem gravar código extra ou confirmar um host específico durante o desenvolvimento.

> [!NOTE]
> O host de serviço WCF não oferece suporte a confiança parcial. Se você quiser usar um serviço WCF em confiança parcial, não use o modelo de projeto da biblioteca de serviço WCF no Visual Studio para compilar seu serviço. Em vez disso, crie um novo site no Visual Studio escolhendo o modelo de site do serviço WCF, que pode hospedar o serviço em um servidor Web no qual há suporte para confiança parcial do WCF.

## <a name="project-types-hosted-by-wcf-service-host"></a>Tipos de projeto hospedados pelo host de serviço WCF

O host de serviço WCF pode hospedar os seguintes tipos de projeto de biblioteca de serviço WCF: biblioteca de serviços WCF, biblioteca de serviço de fluxo de trabalho Sequencial, biblioteca de serviço de fluxo de trabalho de máquina de estado e biblioteca O host de serviço WCF também pode hospedar esses serviços que podem ser adicionados a um projeto de biblioteca de serviço usando a funcionalidade **Adicionar item** . Isso inclui serviço WCF, serviço de máquina de estado do WF, serviço sequencial do WF, serviço de máquina de estado XAML do WF e serviço sequencial do WF do XAML.

No entanto, você deve observar que a ferramenta não ajudará a configurar um host. Para essa tarefa, você deve editar manualmente o arquivo de App.config. A ferramenta também não valida arquivos de configuração definidos pelo usuário.

> [!CAUTION]
> Você não deve usar o host de serviço do WCF para hospedar serviços em um ambiente de produção, pois ele não foi projetado para essa finalidade.  O host de serviço WCF não oferece suporte aos requisitos de confiabilidade, segurança e gerenciamento desse ambiente. Em vez disso, use o IIS, pois ele fornece recursos de confiabilidade e monitoramento superiores e é a solução preferida para hospedar serviços. Após a conclusão do desenvolvimento de seus serviços, você deve migrar os serviços do host de serviço do WCF para o IIS.

## <a name="scenarios-for-using-wcf-service-host-inside-visual-studio"></a>Cenários para usar o host de serviço WCF dentro do Visual Studio

A tabela a seguir lista todos os parâmetros na caixa de diálogo **argumentos de linha de comando** , que podem ser encontrados clicando com o botão direito do mouse em seu projeto no Gerenciador de **soluções** no Visual Studio, selecionando **Propriedades**, selecionando a guia **depurar** e clicando em **Iniciar projeto**. Esses parâmetros são úteis na configuração do host de serviço WCF.

|Parâmetro|Significado|
|---------------|-------------|
|`/client`|Um parâmetro opcional que especifica o caminho para um executável a ser executado depois que os serviços são hospedados. Isso inicia o cliente de teste do WCF seguindo a hospedagem.|
|`/clientArg`|Especifique uma cadeia de caracteres como um argumento que é passado para o aplicativo cliente personalizado.|
|`/?`|Exibe o texto de ajuda.|

#### <a name="using-wcf-test-client"></a>Usando o cliente de teste do WCF

Depois de criar um novo projeto de serviço do WCF e pressionar F5 para iniciar o depurador, o host de serviço do WCF inicia a hospedagem de todos os serviços encontrados em seu projeto. O cliente de teste do WCF abre automaticamente e exibe uma lista de pontos de extremidade de serviço definidos no arquivo de configuração. Na janela principal, você pode testar os parâmetros e invocar o serviço.

Para verificar se o cliente de teste do WCF é usado, clique com o botão direito do mouse em seu projeto no **Gerenciador de soluções** no Visual Studio, selecione **Propriedades**e, em seguida, selecione a guia **depurar** . clique em **Iniciar projeto** e verifique se o seguinte aparece na caixa de diálogo **argumentos de linha de comando** .

`/client:WcfTestClient.exe`

#### <a name="using-a-custom-client"></a>Usando um cliente personalizado

Para usar um cliente personalizado, clique com o botão direito do mouse em seu projeto no **Gerenciador de soluções** no Visual Studio, selecione **Propriedades**e, em seguida, selecione a guia **depurar** . clique em **Iniciar projeto** e edite o `/client` parâmetro na caixa de diálogo **argumentos de linha de comando** para apontar para o cliente personalizado, conforme indicado no exemplo a seguir.

`/client:"path/CustomClient.exe"`

Quando você pressiona F5 para iniciar o serviço novamente, o host de serviço do WCF inicia automaticamente o cliente personalizado quando você inicia o depurador.

Você também pode usar o `/clientArg:` parâmetro para especificar uma cadeia de caracteres como um argumento que é passado para o aplicativo cliente personalizado, conforme indicado no exemplo a seguir.

`/client:"path/CustomClient.exe" /clientArg:"arguments that are passed to Client"`

Por exemplo, se você estiver usando o modelo de biblioteca de serviço de distribuição, poderá usar os seguintes argumentos de linha de comando,

`/client:iexplore.exe /clientArgs:http://localhost:8731/Design_Time_Addresses/Feed1/`

#### <a name="specifying-no-client"></a>Especificando nenhum cliente

Para especificar que nenhum cliente será usado após a hospedagem do serviço WCF, clique com o botão direito do mouse em seu projeto no **Gerenciador de soluções** no Visual Studio, selecione **Propriedades**e, em seguida, selecione a guia **depurar** . clique em **Iniciar projeto** e deixe a caixa de diálogo **argumentos de linha de comando** em branco.

#### <a name="using-a-custom-host"></a>Usando um host personalizado

Para usar um host personalizado, clique com o botão direito do mouse em seu projeto no **Gerenciador de soluções** no Visual Studio, selecione **Propriedades**e, em seguida, selecione a guia **depurar** . clique em **Iniciar programa externo** e insira o caminho completo para o host personalizado. Você também pode usar a caixa de diálogo **argumentos de linha de comando** para especificar os argumentos a serem passados para o host.

## <a name="wcf-service-host-user-interface"></a>Interface do usuário do host de serviço WCF

Quando você chama inicialmente o host de serviço do WCF (pressionando F5 dentro do Visual Studio), a janela **host do serviço WCF** é aberta automaticamente. Quando o host de serviço do WCF estiver em execução, o ícone do programa aparecerá na área de notificação. Clique duas vezes no ícone para abrir a janela **host de serviço do WCF**

Quando ocorrerem erros durante a hospedagem de serviço, a caixa de diálogo host de serviço do WCF será aberta para exibir informações relevantes.

A janela principal do **host de serviço do WCF** contém dois menus:

- **Arquivo**: contém os comandos **fechar** e **sair** . Quando você clica em **fechar**, a caixa de diálogo **host de serviço do WCF** é fechada, mas os serviços continuam sendo hospedados. Quando você clica em **sair**, o host de serviço WCF também é desligado. Isso também interrompe todos os serviços hospedados.

- **Ajuda**: contém o comando **about** que contém informações de versão. Ele também contém o comando de **ajuda** que pode abrir um arquivo de ajuda.

A janela principal do **host de serviço WCF** contém duas áreas:

- A primeira área é **serviço**. Ele contém uma lista que exibe informações básicas de todos os serviços. As informações incluem:

  - **Serviço**: lista todos os serviços.

  - **Status**: lista o status do serviço. Os valores válidos são "iniciado", "parado" e "erro".

  - **Endereço de metadados**: exibe o endereço de metadados dos serviços.

- A segunda área é de **informações adicionais**. Ele exibe uma explicação detalhada do status do serviço quando a linha de serviço específica é selecionada na área de **serviço** . Se o status for erro, você poderá exibir a mensagem de erro completa na tela.

## <a name="stopping-wcf-service-host"></a>Parando o host de serviço WCF

Você pode desligar o host de serviço do WCF das quatro maneiras a seguir:

- Pare a sessão de depuração no Visual Studio.

- Selecione **sair** no menu **arquivo** na janela **host de serviço do WCF** .

- Selecione **sair** no menu de contexto do ícone da bandeja de host do serviço WCF na área de notificação do sistema.

- Saia do cliente de teste do WCF se ele estiver sendo usado.

## <a name="using-service-host-without-administrator-privilege"></a>Usando o host de serviço sem privilégio de administrador

Para permitir que os usuários sem privilégios de administrador desenvolvam serviços WCF, uma ACL (lista de controle de acesso) é criada para o namespace " http://+:8731/Design_Time_Addresses " durante a instalação do Visual Studio. A ACL é definida como (UI), que inclui todos os usuários interativos conectados à máquina. Os administradores podem adicionar ou remover usuários dessa ACL ou abrir portas adicionais. Essa ACL permite que os usuários usem o host automático do serviço WCF (wcfSvcHost.exe) sem conceder a eles privilégios de administrador.

Você pode modificar o acesso usando a ferramenta de netsh.exe no Windows Vista na conta de administrador elevado. Veja a seguir um exemplo de como usar netsh.exe.

```console
netsh http add urlacl url=http://+:8001/MyService user=<domain>\<user>
```

Para obter mais informações sobre netsh.exe, consulte "[como usar a ferramenta de Netsh.exe e as opções de linha de comando](https://docs.microsoft.com/previous-versions/tn-archive/bb490939(v=technet.10))".

## <a name="see-also"></a>Veja também

- [Cliente de Teste do WCF (WcfTestClient.exe)](wcf-test-client-wcftestclient-exe.md)
