---
title: 'Passo a passo: criar e usar objetos dinâmicos (C# e Visual Basic)'
description: Saiba como criar e usar objetos dinâmicos neste passo a passos. Crie um objeto dinâmico personalizado e um projeto que usa uma biblioteca ' IronPython '.
ms.date: 07/20/2015
dev_langs:
- csharp
- vb
helpviewer_keywords:
- dynamic objects [Visual Basic]
- dynamic objects
- dynamic objects [C#]
ms.assetid: 568f1645-1305-4906-8625-5d77af81e04f
ms.openlocfilehash: 144651d3b14f6f6093ab07f1df7be10e6d05ae89
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381249"
---
# <a name="walkthrough-creating-and-using-dynamic-objects-c-and-visual-basic"></a>Passo a passo: criar e usar objetos dinâmicos (C# e Visual Basic)

Os objetos dinâmicos expõem membros como propriedades e métodos em tempo de execução, em vez de em tempo de compilação. Isso permite que você crie objetos para trabalhar com estruturas que não correspondem a um formato ou tipo estático. Por exemplo, você pode usar um objeto dinâmico para fazer referência ao DOM (Modelo de Objeto do Documento) HTML, que pode conter qualquer combinação de atributos e elementos de marcação HTML válidos. Como cada documento HTML é único, os membros de um determinado documento HTML são determinados em tempo de execução. Um método comum para fazer referência a um atributo de um elemento HTML é passar o nome do atributo para o método `GetProperty` do elemento. Para fazer referência ao atributo `id` do elemento HTML `<div id="Div1">`, primeiro você obtém uma referência ao elemento `<div>` e, depois, usa `divElement.GetProperty("id")`. Se usar um objeto dinâmico, você poderá fazer referência ao atributo `id` como `divElement.id`.  
  
 Objetos dinâmicos também fornecem acesso conveniente a linguagens dinâmicas, como IronPython e IronRuby. É possível usar um objeto dinâmico para fazer referência a um script dinâmico que é interpretado em tempo de execução.  
  
 Você faz referência a um objeto dinâmico usando a associação tardia. Em C#, você especifica o tipo de um objeto com associação tardia como `dynamic`. No Visual Basic, você especifica o tipo de um objeto de associação tardia como `Object`. Para obter mais informações, consulte [dynamic](../../language-reference/builtin-types/reference-types.md) e [Associação antecipada e tardia](../../../visual-basic/programming-guide/language-features/early-late-binding/index.md).  
  
 Você pode criar objetos dinâmicos personalizados usando classes no namespace <xref:System.Dynamic?displayProperty=nameWithType>. Por exemplo, é possível criar um <xref:System.Dynamic.ExpandoObject> e especificar os membros desse objeto em tempo de execução. Você também pode criar seu próprio tipo que herda da classe <xref:System.Dynamic.DynamicObject>. Em seguida, você pode substituir os membros da classe <xref:System.Dynamic.DynamicObject> para fornecer funcionalidade dinâmica de tempo de execução.  
  
 Nesta explicação passo a passo, você executará as seguintes tarefas:  
  
- Criar um objeto personalizado que expõe dinamicamente o conteúdo de um arquivo de texto como propriedades de um objeto.  
  
- Criar um projeto que usa uma biblioteca `IronPython`.  
  
## <a name="prerequisites"></a>Pré-requisitos  

É necessário ter o [IronPython](https://ironpython.net/) para .NET para concluir este passo a passo. Navegue até a respectiva [página de download](https://ironpython.net/download/) para obter a versão mais recente.
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="creating-a-custom-dynamic-object"></a>Criando um objeto dinâmico personalizado

O primeiro projeto que você cria neste passo a passo define um objeto dinâmico personalizado que pesquisa o conteúdo de um arquivo de texto. O texto a ser pesquisado é especificado pelo nome de uma propriedade dinâmica. Por exemplo, se o código de chamada especificar `dynamicFile.Sample`, a classe dinâmica retornará uma lista genérica de cadeias de caracteres que contém todas as linhas do arquivo que começam com "Sample". A pesquisa diferencia maiúsculas de minúsculas. A classe dinâmica também dá suporte a dois argumentos opcionais. O primeiro argumento é um valor de enum de opção de pesquisa que especifica que a classe dinâmica deve pesquisar por correspondências no início da linha, no final da linha ou em qualquer lugar da linha. O segundo argumento especifica que a classe dinâmica deve cortar espaços iniciais e finais de cada linha antes de pesquisar. Por exemplo, se o código de chamada especificar `dynamicFile.Sample(StringSearchOption.Contains)`, a classe dinâmica pesquisará por "Sample" em qualquer lugar de uma linha. Se o código de chamada especificar `dynamicFile.Sample(StringSearchOption.StartsWith, false)`, a classe dinâmica pesquisará por "Sample" no início de cada linha e não removerá espaços à direita e à esquerda. O comportamento padrão da classe dinâmica é pesquisar por uma correspondência no início de cada linha e remover espaços à direita e à esquerda.  
  
### <a name="to-create-a-custom-dynamic-class"></a>Para criar uma classe dinâmica personalizada  
  
1. Inicie o Visual Studio.  
  
2. No menu **arquivo** , aponte para **novo** e clique em **projeto**.  
  
3. Na caixa de diálogo **Novo Projeto**, no painel **Tipos de Projetos**, certifique-se de que **Windows** esteja selecionado. Selecione **Aplicativo de Console** no painel **Modelos**. Na caixa **Nome**, digite `DynamicSample` e clique em **OK**. Quando um novo projeto é criado.  
  
4. Clique com botão direito do mouse no projeto DynamicSample e aponte para **Adicionar** e, em seguida, clique em **Classe**. Na caixa **Nome**, digite `ReadOnlyFile` e clique em **OK**. É adicionado um novo arquivo que contém a classe ReadOnlyFile.  
  
5. Na parte superior do arquivo ReadOnlyFile.cs ou ReadOnlyFile.vb, adicione o código a seguir para importar os namespaces <xref:System.IO?displayProperty=nameWithType> e <xref:System.Dynamic?displayProperty=nameWithType>.  

    [!code-csharp[VbDynamicWalkthrough#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#1)]
    [!code-vb[VbDynamicWalkthrough#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#1)]  

6. O objeto dinâmico personalizado usa um enum para determinar os critérios de pesquisa. Antes da instrução de classe, adicione a seguinte definição de enum.  
  
    [!code-csharp[VbDynamicWalkthrough#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#2)]
    [!code-vb[VbDynamicWalkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#2)]
  
7. Atualize a instrução de classe para herdar a classe `DynamicObject`, conforme mostrado no exemplo de código a seguir.  
  
    [!code-csharp[VbDynamicWalkthrough#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#3)]
    [!code-vb[VbDynamicWalkthrough#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#3)]

8. Adicione o código a seguir para a classe `ReadOnlyFile` para definir um campo particular para o caminho do arquivo e um construtor para a classe `ReadOnlyFile`.  
  
    [!code-csharp[VbDynamicWalkthrough#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#4)]
    [!code-vb[VbDynamicWalkthrough#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#4)]
  
9. Adicione o seguinte método `GetPropertyValue` à classe `ReadOnlyFile`. O método `GetPropertyValue` usa, como entrada, critérios de pesquisa e retorna as linhas de um arquivo de texto que correspondem a esse critério de pesquisa. Os métodos dinâmicos fornecidos pela classe `ReadOnlyFile` chamam o método `GetPropertyValue` para recuperar seus respectivos resultados.  
  
    [!code-csharp[VbDynamicWalkthrough#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#5)]
    [!code-vb[VbDynamicWalkthrough#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#5)]  
  
10. Após o método `GetPropertyValue`, adicione o seguinte código para substituir o método <xref:System.Dynamic.DynamicObject.TryGetMember%2A> da classe <xref:System.Dynamic.DynamicObject>. O método <xref:System.Dynamic.DynamicObject.TryGetMember%2A> é chamado quando um membro de uma classe dinâmica é solicitado e nenhum argumento é especificado. O argumento `binder` contém informações sobre o membro referenciado e o argumento `result` faz referência ao resultado retornado para o membro especificado. O método <xref:System.Dynamic.DynamicObject.TryGetMember%2A> retorna um valor booliano que retorna `true` se o membro solicitado existe; caso contrário, ele retorna `false`.  
  
    [!code-csharp[VbDynamicWalkthrough#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#6)]
    [!code-vb[VbDynamicWalkthrough#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#6)]
  
11. Após o método `TryGetMember`, adicione o seguinte código para substituir o método <xref:System.Dynamic.DynamicObject.TryInvokeMember%2A> da classe <xref:System.Dynamic.DynamicObject>. O método <xref:System.Dynamic.DynamicObject.TryInvokeMember%2A> é chamado quando um membro de uma classe dinâmica é solicitado com argumentos. O argumento `binder` contém informações sobre o membro referenciado e o argumento `result` faz referência ao resultado retornado para o membro especificado. O argumento `args` contém uma matriz de argumentos que são passados ao membro. O método <xref:System.Dynamic.DynamicObject.TryInvokeMember%2A> retorna um valor booliano que retorna `true` se o membro solicitado existe; caso contrário, ele retorna `false`.  
  
    A versão personalizada do método `TryInvokeMember` espera que o primeiro argumento seja um valor do enum `StringSearchOption` que você definiu na etapa anterior. O método `TryInvokeMember` espera que o segundo argumento seja um valor booliano. Se um ou os dois argumentos forem valores válidos, eles serão passados para o método `GetPropertyValue` para recuperar os resultados.  
  
    [!code-csharp[VbDynamicWalkthrough#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#7)]
    [!code-vb[VbDynamicWalkthrough#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#7)]
  
12. Salve e feche o arquivo.  
  
#### <a name="to-create-a-sample-text-file"></a>Para criar um arquivo de texto de exemplo  
  
1. Clique com botão direito do mouse no projeto DynamicSample e aponte para **Adicionar** e, em seguida, clique em **Novo Item**. No painel **Modelos Instalados**, selecione **Geral** e, então, selecione o modelo **Arquivo de Texto**. Deixe o nome padrão de TextFile1.txt na caixa **Nome** caixa e, em seguida, clique em **Adicionar**. Um novo arquivo de texto é adicionado ao projeto.  
  
2. Copie o seguinte texto para o arquivo TextFile1.txt.  
  
    ```text  
    List of customers and suppliers  
  
    Supplier: Lucerne Publishing (https://www.lucernepublishing.com/)  
    Customer: Preston, Chris  
    Customer: Hines, Patrick  
    Customer: Cameron, Maria  
    Supplier: Graphic Design Institute (https://www.graphicdesigninstitute.com/)
    Supplier: Fabrikam, Inc. (https://www.fabrikam.com/)
    Customer: Seubert, Roxanne  
    Supplier: Proseware, Inc. (http://www.proseware.com/)
    Customer: Adolphi, Stephan  
    Customer: Koch, Paul  
    ```  
  
3. Salve e feche o arquivo.  
  
#### <a name="to-create-a-sample-application-that-uses-the-custom-dynamic-object"></a>Para criar um aplicativo de exemplo que usa o objeto dinâmico personalizado  
  
1. No **Gerenciador de Soluções**, clique duas vezes no arquivo Module1.vb se você estiver usando Visual Basic ou no arquivo Program.cs se estiver usando o Visual C#.  
  
2. Adicione o código a seguir ao procedimento Main para criar uma instância da classe `ReadOnlyFile` para o arquivo TextFile1.txt. O código usa associação tardia para chamar membros dinâmicos e recuperar linhas de texto que contêm a cadeia de caracteres "Customer".  
  
     [!code-csharp[VbDynamicWalkthrough#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/program.cs#8)]
     [!code-vb[VbDynamicWalkthrough#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/module1.vb#8)]
  
3. Salve o arquivo e pressione CTRL+F5 para compilar e executar o aplicativo.  
  
## <a name="calling-a-dynamic-language-library"></a>Chamando uma biblioteca de linguagem dinâmica  

O próximo projeto que você cria neste passo a passo acessa uma biblioteca escrita na linguagem dinâmica IronPython.
  
### <a name="to-create-a-custom-dynamic-class"></a>Para criar uma classe dinâmica personalizada
  
1. No Visual Studio, no menu **arquivo** , aponte para **novo** e clique em **projeto**.  
  
2. Na caixa de diálogo **Novo Projeto**, no painel **Tipos de Projetos**, certifique-se de que **Windows** esteja selecionado. Selecione **Aplicativo de Console** no painel **Modelos**. Na caixa **Nome**, digite `DynamicIronPythonSample` e clique em **OK**. Quando um novo projeto é criado.  
  
3. Se você estiver usando o Visual Basic, clique com o botão direito do mouse no projeto DynamicIronPythonSample e, em seguida, clique em **Propriedades**. Clique na guia **referências** . Clique no botão **Adicionar** . Se você estiver usando o Visual C#, no **Gerenciador de Soluções**, clique com botão direito do mouse na pasta **Referências** e, em seguida, clique em **Adicionar Referência**.  
  
4. Na guia **Procurar**, navegue até a pasta em que as bibliotecas do IronPython estão instaladas. Por exemplo, C:\Program Files\IronPython 2.6 for .NET 4.0. Selecione as bibliotecas **IronPython.dll**, **IronPython.Modules.dll**, **Microsoft.Scripting.dll** e **Microsoft.Dynamic.dll**. Clique em **OK**.  
  
5. Se você estiver usando o Visual Basic, edite o arquivo Module1.vb. Se estiver usando o Visual C#, edite o arquivo Program.cs.  
  
6. Na parte superior do arquivo, adicione o código a seguir para importar os namespaces `Microsoft.Scripting.Hosting` e `IronPython.Hosting` das bibliotecas do IronPython.  
  
    [!code-csharp[VbDynamicWalkthroughIronPython#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/cs/program.cs#1)]
    [!code-vb[VbDynamicWalkthroughIronPython#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/vb/module1.vb#1)]
  
7. No método Main, adicione o código a seguir para criar um novo objeto `Microsoft.Scripting.Hosting.ScriptRuntime` para hospedar as bibliotecas do IronPython. O objeto `ScriptRuntime` carrega o módulo random.py da biblioteca do IronPython.  
  
     [!code-csharp[VbDynamicWalkthroughIronPython#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/cs/program.cs#2)]
     [!code-vb[VbDynamicWalkthroughIronPython#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/vb/module1.vb#2)]
  
8. Após o código carregar o módulo random.py, adicione o seguinte código para criar uma matriz de inteiros. A matriz é passada para o método `shuffle` do módulo random.py, que classifica aleatoriamente os valores na matriz.  
  
     [!code-csharp[VbDynamicWalkthroughIronPython#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/cs/program.cs#3)]
     [!code-vb[VbDynamicWalkthroughIronPython#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/vb/module1.vb#3)]
  
9. Salve o arquivo e pressione CTRL+F5 para compilar e executar o aplicativo.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Dynamic?displayProperty=nameWithType>
- <xref:System.Dynamic.DynamicObject?displayProperty=nameWithType>
- [Usando o tipo dynamic](./using-type-dynamic.md)
- [Associação antecipada e tardia](../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)
- [dinâmico](../../language-reference/builtin-types/reference-types.md)
- [Implementando interfaces dinâmicas (PDF para download do Microsoft TechNet)](https://download.microsoft.com/download/5/4/B/54B83DFE-D7AA-4155-9687-B0CF58FF65D7/implementing-dynamic-interfaces.pdf)
