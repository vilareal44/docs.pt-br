---
title: 'Como: criar, inicializar e configurar opções de rastreamento'
description: Crie, inicialize e configure opções de rastreamento usando as classes System. Diagnostics. BooleanSwitch e System. Diagnostics. TraceSwitch no .NET.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- trace switches, configuring
- tracing [.NET Framework], trace switches
- switches, trace
- tracing [.NET Framework], enabling or disabling
- Web.config configuration file, trace switches
ms.assetid: 5a0e41bf-f99c-4692-8799-f89617f5bcf9
ms.openlocfilehash: 6a43e143abba96c841f04b7be9d482c55e78aa8f
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051318"
---
# <a name="how-to-create-initialize-and-configure-trace-switches"></a>Como: criar, inicializar e configurar opções de rastreamento
As opções de rastreamento permitem habilitar, desabilitar e filtrar a saída de rastreamento.  
  
<a name="create"></a>
## <a name="creating-and-initializing-a-trace-switch"></a>Criando e inicializando uma opção de rastreamento  
 Para usar opções de rastreamento, primeiro você deve criá-las e colocá-las no código. Há duas classes predefinidas com base nas quais você pode criar objetos de opção: a classe <xref:System.Diagnostics.BooleanSwitch?displayProperty=nameWithType> e a classe <xref:System.Diagnostics.TraceSwitch?displayProperty=nameWithType>. Use <xref:System.Diagnostics.BooleanSwitch> se você se importar apenas com o fato de uma mensagem de rastreamento ser exibida ou não; use <xref:System.Diagnostics.TraceSwitch> se desejar discriminar entre níveis de rastreamento. Se você usar uma <xref:System.Diagnostics.TraceSwitch>, poderá definir suas próprias mensagens de depuração e associá-las a diferentes níveis de rastreamento. Use ambos os tipos de opções com o rastreamento ou a depuração. Por padrão, uma <xref:System.Diagnostics.BooleanSwitch> está desabilitada e uma <xref:System.Diagnostics.TraceSwitch> está definida como o nível <xref:System.Diagnostics.TraceLevel.Off?displayProperty=nameWithType>. As opções de rastreamento podem ser criadas e colocadas em qualquer parte do código que você pode usá-las.  
  
 Embora seja possível definir níveis de rastreamento e outras opções de configuração no código, recomendamos o uso do arquivo de configuração para gerenciar o estado das opções. Isso ocorre porque o gerenciamento da configuração das opções no sistema de configuração proporciona maior flexibilidade – você pode ativar e desativar várias opções e alterar os níveis sem recompilar o aplicativo.  
  
#### <a name="to-create-and-initialize-a-trace-switch"></a>Para criar e inicializar uma opção de rastreamento  
  
1. Defina uma opção como o tipo <xref:System.Diagnostics.BooleanSwitch?displayProperty=nameWithType> ou o tipo <xref:System.Diagnostics.TraceSwitch?displayProperty=nameWithType> e defina o nome e a descrição da opção.  
  
2. Configure a opção de rastreamento. Para obter mais informações, consulte [Configuring Trace switches](#configure).  
  
     O código a seguir cria duas opções, uma de cada tipo:  
  
    ```vb  
    Dim dataSwitch As New BooleanSwitch("Data", "DataAccess module")  
    Dim generalSwitch As New TraceSwitch("General", "Entire application")  
    ```  
  
    ```csharp  
    System.Diagnostics.BooleanSwitch dataSwitch =
       new System.Diagnostics.BooleanSwitch("Data", "DataAccess module");  
    System.Diagnostics.TraceSwitch generalSwitch =
       new System.Diagnostics.TraceSwitch("General",
       "Entire application");  
    ```  
  
<a name="configure"></a>
## <a name="configuring-trace-switches"></a>Configurando opções de rastreamento  
 Depois que o aplicativo for distribuído, você ainda poderá habilitar ou desabilitar a saída de rastreamento configurando as opções de rastreamento no aplicativo. A configuração de uma opção significa alterar seu valor de uma fonte externa, depois que ela foi inicializada. Você pode alterar os valores dos objetos de opção usando o arquivo de configuração. Configure uma opção de rastreamento para ativá-la e desativá-la ou para definir seu nível, determinando a quantidade e o tipo de mensagens que são passados para os ouvintes.  
  
 As opções são configuradas usando o arquivo .config. Para um aplicativo Web, este é o arquivo Web.config associado ao projeto. Em um aplicativo do Windows, esse arquivo é denominado (nome do aplicativo) .exe.config. Em um aplicativo implantado, esse arquivo deve residir na mesma pasta que o executável.  
  
 Quando o aplicativo executa o código que cria uma instância de uma opção pela primeira vez, ele verifica o arquivo de configuração para obter informações no nível do rastreamento sobre a opção nomeada. O sistema de rastreamento examina o arquivo de configuração apenas uma vez em busca de uma opção específica – na primeira vez que o aplicativo cria o objeto.  
  
 Em um aplicativo implantado, habilite o código de rastreamento reconfigurando objetos de opção quando o aplicativo não estiver em execução. Normalmente, isso envolve a ativação e desativação dos objetos de opção ou a alteração dos níveis de rastreamento e, em seguida, a reinicialização do aplicativo.  
  
 Quando você cria uma instância com base em uma opção, você também inicializa-a com a especificação de dois argumentos: um argumento *displayName* e um argumento *description*. O argumento *displayName* do construtor define a propriedade <xref:System.Diagnostics.Switch.DisplayName%2A?displayProperty=nameWithType> da instância da classe <xref:System.Diagnostics.Switch>. O *displayName* é o nome usado para configurar a opção no arquivo .config e o argumento *description* deve retornar uma breve descrição da opção e quais mensagens são controladas por ela.  
  
 Além de especificar o nome de uma opção a ser configurada, você também deve especificar um valor para a opção. Esse valor é um Inteiro. Para <xref:System.Diagnostics.BooleanSwitch>, um valor igual a 0 corresponde a **Off** e qualquer valor diferente de zero corresponde a **On**. Para <xref:System.Diagnostics.TraceSwitch>, 0, 1, 2, 3 e 4 correspondem a **Off**, **Error**, **Warning**, **Info** e **Verbose**, respectivamente. Qualquer número maior que 4 é tratado como **Verbose** e qualquer número menor que zero é tratado como **Off**.  
  
> [!NOTE]
> No .NET Framework versão 2.0, você pode usar o texto para especificar o valor de uma opção. Por exemplo, `true` para uma <xref:System.Diagnostics.BooleanSwitch> ou o texto que representa um valor de enumeração como `Error` para uma <xref:System.Diagnostics.TraceSwitch>. A linha `<add name="myTraceSwitch" value="Error" />` é equivalente a `<add name="myTraceSwitch" value="1" />`.  
  
 Para que os usuários finais possam definir as opções de rastreamento de um aplicativo, você deve fornecer uma documentação detalhada das opções no aplicativo. É necessário fornecer detalhes sobre quais opções controlam o que e como ativá-las e desativá-las. Você também deve fornecer ao usuário final um arquivo .config que tem a Ajuda apropriada nos comentários.  
  
#### <a name="to-configure-trace-switches"></a>Para configurar opções de rastreamento  
  
1. Para usar opções de rastreamento, primeiro você deve criá-las e colocá-las no código, conforme descrito na seção [Criando e inicializando uma opção de rastreamento](#create).  
  
2. Se o projeto não contiver um arquivo de configuração (app.config ou Web.config), no menu **Projeto**, selecione **Adicionar Novo Item**.  
  
    - **Visual Basic:** na caixa de diálogo **Adicionar Novo Item**, escolha **Arquivo de Configuração de Aplicativo**.  
  
         O arquivo de configuração de aplicativo será criado e aberto. Este é um documento XML cujo elemento raiz é `<configuration>.`  
  
    - **Visual C#:** na caixa de diálogo **Adicionar Novo Item**, escolha **Arquivo XML**. Nomeie este arquivo **app.config**. No editor de XML, após a declaração XML, adicione o seguinte XML:  
  
        ```xml  
        <configuration>  
        </configuration>  
        ```  
  
         Quando o projeto é compilado, o arquivo app.config é copiado para a pasta de saída do projeto e é renomeado *applicationname*.exe.config.  
  
3. Após a marcação `<configuration>`, mas antes da marcação `</configuration>`, adicione o XML apropriado para configurar as opções. Os exemplos a seguir demonstram uma **BooleanSwitch** com uma propriedade **DisplayName**`DataMessageSwitch` e uma **TraceSwitch** com uma propriedade **DisplayName** igual a `TraceLevelSwitch`.  
  
    ```xml  
    <system.diagnostics>  
       <switches>  
          <add name="DataMessagesSwitch" value="0" />  
          <add name="TraceLevelSwitch" value="0" />  
       </switches>  
    </system.diagnostics>  
    ```  
  
     Nessa configuração, ambas as opções estão desativadas.  
  
4. Se precisar ativar uma **BooleanSwitch**, como `DataMessagesSwitch` mostrado no exemplo anterior, altere o **Value** para qualquer inteiro diferente de 0.  
  
5. Se precisar ativar uma **TraceSwitch**, como `TraceLevelSwitch` mostrado no exemplo anterior, altere o **Value** para a configuração de nível apropriada (1 a 4).  
  
6. Adicione comentários ao arquivo .config, de modo que o usuário final tenha uma compreensão clara dos valores que devem ser alterados para configurar as opções de forma adequada.  
  
     O seguinte exemplo mostra a possível aparência do código final, incluindo comentários:  
  
    ```xml  
    <system.diagnostics>  
       <switches>  
          <!-- This switch controls data messages. In order to receive data   
             trace messages, change value="0" to value="1" -->  
          <add name="DataMessagesSwitch" value="0" />  
          <!-- This switch controls general messages. In order to   
             receive general trace messages change the value to the   
             appropriate level. "1" gives error messages, "2" gives errors   
             and warnings, "3" gives more detailed error information, and   
             "4" gives verbose trace information -->  
          <add name="TraceLevelSwitch" value="0" />  
       </switches>  
    </system.diagnostics>  
    ```  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento e instrumentação de aplicativos](tracing-and-instrumenting-applications.md)
- [Como adicionar instruções de rastreamento ao código de um aplicativo](how-to-add-trace-statements-to-application-code.md)
- [Opções de rastreamento](trace-switches.md)
- [Esquema de configurações de rastreamento e depuração](../configure-apps/file-schema/trace-debug/index.md)
