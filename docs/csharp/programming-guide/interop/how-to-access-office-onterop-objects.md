---
title: Como acessar objetos de interoperabilidade do Office – Guia de programação C#
description: Saiba mais sobre os recursos do C# que simplificam o acesso aos objetos da API do Office. Use os novos recursos para escrever código que cria e exibe uma planilha do Excel.
ms.date: 07/20/2015
helpviewer_keywords:
- optional parameters [C#], Office programming
- named and optional arguments [C#], Office programming
- dynamic [C#], Office programming
- optional arguments [C#], Office programming
- named arguments [C#], Office programming
- Office programming [C#]
ms.assetid: 041b25c2-3512-4e0f-a4ea-ceb2999e4d5e
ms.openlocfilehash: bc4b5755bf56a013a0deb4efdb821df18db5a18e
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303017"
---
# <a name="how-to-access-office-interop-objects-c-programming-guide"></a>Como acessar objetos de interoperabilidade do Office (guia de programação C#)

O C# tem recursos que simplificam o acesso aos objetos da API do Office. Os novos recursos incluem argumentos nomeados e opcionais, um novo tipo chamado `dynamic` e a capacidade de passar argumentos para parâmetros de referência em métodos COM como se fossem parâmetros de valor.

Neste tópico, você usará os novos recursos para escrever código que cria e exibe uma planilha do Microsoft Office Excel. Em seguida, você irá escrever código para adicionar um documento do Office Word que contenha um ícone que esteja vinculado à planilha do Excel.

Para concluir este passo a passo, você deve ter o Microsoft Office Excel 2007 e o Microsoft Office Word 2007 ou versões posteriores, instaladas no computador.

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="to-create-a-new-console-application"></a>Para criar um novo aplicativo de console

1. Inicie o Visual Studio.

2. No menu **Arquivo** , aponte para **Novo**e clique em **Projeto**. A caixa de diálogo **Novo Projeto** aparecerá.

3. No painel **Modelos Instalados**, expanda **Visual C#** e, em seguida, selecione **Windows**.

4. Observe a parte superior da caixa de diálogo **Novo Projeto** para verificar se **.NET Framework 4** (ou versão posterior) está selecionado como uma estrutura de destino.

5. No painel **Modelos**, clique em **Aplicativo de Console**.

6. Digite um nome para o projeto no campo **Nome**.

7. Clique em **OK**.

     O novo projeto aparece no **Gerenciador de Soluções**.

## <a name="to-add-references"></a>Para adicionar referências

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto e, em seguida, clique em **Adicionar Referência**. A caixa de diálogo **Adicionar Referência** é exibida.

2. Na página **Assemblies**, selecione **Microsoft.Office.Interop.Word** na lista **Nome do Componente** e, mantendo a tecla CTRL pressionada, selecione **Microsoft.Office.Interop.Excel**.  Se você não vir os assemblies, talvez seja necessário garantir que eles sejam instalados e exibidos. Consulte [como instalar assemblies de interoperabilidade primária do Office](/visualstudio/vsto/how-to-install-office-primary-interop-assemblies).

3. Clique em **OK**.

## <a name="to-add-necessary-using-directives"></a>Para adicionar as diretivas using necessárias

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no arquivo *Program.cs* e, em seguida, clique em **Exibir Código**.

2. Adicione as seguintes `using` diretivas à parte superior do arquivo de código:

     [!code-csharp[csProgGuideOfficeHowTo#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#1)]

## <a name="to-create-a-list-of-bank-accounts"></a>Para criar uma lista de contas bancárias

1. Cole a seguinte definição de classe em **Program.cs**, na classe `Program`.

     [!code-csharp[csProgGuideOfficeHowTo#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#2)]

2. Adicione o seguinte código ao método `Main` para criar uma lista `bankAccounts` que contenha duas contas.

     [!code-csharp[csProgGuideOfficeHowTo#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#3)]

## <a name="to-declare-a-method-that-exports-account-information-to-excel"></a>Para declarar um método que exporta as informações de conta para o Excel

1. Adicione o método a seguir à classe `Program` para configurar uma planilha do Excel.

     O método <xref:Microsoft.Office.Interop.Excel.Workbooks.Add%2A> tem um parâmetro opcional para especificar um modelo específico. Os parâmetros opcionais, novos no C# 4, permitem omitir o argumento para esse parâmetro se você quiser usar o valor padrão do parâmetro. Como nenhum argumento é enviado no código a seguir, `Add` usa o modelo padrão e cria uma nova pasta de trabalho. A instrução equivalente em versões anteriores do C# requer um argumento de espaço reservado: `ExcelApp.Workbooks.Add(Type.Missing)`.

     [!code-csharp[csProgGuideOfficeHowTo#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#4)]

2. Adicione o código a seguir no final de `DisplayInExcel`. O código insere valores nas duas primeiras colunas da primeira linha da planilha.

     [!code-csharp[csProgGuideOfficeHowTo#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#5)]

3. Adicione o código a seguir no final de `DisplayInExcel`. O loop `foreach` coloca as informações da lista de contas nas duas primeiras colunas de sucessivas linhas da planilha.

     [!code-csharp[csProgGuideOfficeHowTo#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#7)]

4. Adicione o seguinte código no final de `DisplayInExcel` para ajustar as larguras das colunas para adequar o conteúdo.

     [!code-csharp[csProgGuideOfficeHowTo#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#13)]

     As versões anteriores do C# exigem a conversão explícita para essas operações, porque `ExcelApp.Columns[1]` retorna um `Object`, e `AutoFit` é um método <xref:Microsoft.Office.Interop.Excel.Range> do Excel. As linhas a seguir mostram a conversão.

     [!code-csharp[csProgGuideOfficeHowTo#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#14)]

     O C# 4 e versões posteriores convertem o retornado `Object` para `dynamic` automaticamente se o assembly for referenciado pela opção de compilador [-link](../../language-reference/compiler-options/link-compiler-option.md) ou, de maneira equivalente, se a propriedade **inserir tipos de interoperabilidade** do Excel estiver definida como true. True é o valor padrão para essa propriedade.

## <a name="to-run-the-project"></a>Para executar o projeto

1. Adicione a seguinte linha no final de `Main`.

     [!code-csharp[csProgGuideOfficeHowTo#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#8)]

2. Pressione CTRL+F5.

     Uma planilha do Excel é exibida contendo os dados das duas contas.

## <a name="to-add-a-word-document"></a>Para adicionar um documento do Word

1. Para ilustrar maneiras adicionais pelas quais o C# 4 e versões posteriores aprimoram a programação do Office, o código a seguir abre um aplicativo Word e cria um ícone vinculado à planilha do Excel.

     Cole o método `CreateIconInWordDoc`, fornecido posteriormente nesta etapa, na classe `Program`. O `CreateIconInWordDoc` usa argumentos nomeados e opcionais para reduzir a complexidade das chamadas de método a <xref:Microsoft.Office.Interop.Word.Documents.Add%2A> e <xref:Microsoft.Office.Interop.Word.Selection.PasteSpecial%2A>. Essas chamadas incorporam dois novos recursos apresentados no C# 4 que simplificam as chamadas aos métodos COM que possuem parâmetros de referência. Primeiro, você pode enviar argumentos para os parâmetros de referência como se fossem parâmetros de valor. Ou seja, você pode enviar valores diretamente, sem criar uma variável para cada parâmetro de referência. O compilador gera variáveis temporárias para conter os valores de argumento e descarta as variáveis quando você retornar da chamada. Em segundo lugar, você pode omitir a palavra-chave `ref` na lista de argumentos.

     O método `Add` tem quatro parâmetros de referência que são opcionais. No C# 4.0 e versões posteriores, você poderá omitir argumentos para alguns ou todos os parâmetros se desejar usar os valores padrão. No C# 3.0 e versões anteriores, um argumento deve ser fornecido para cada parâmetro e o argumento deve ser uma variável, pois os parâmetros são parâmetros de referência.

     O método `PasteSpecial` insere o conteúdo da área de transferência. O método tem sete parâmetros de referência que são opcionais. O código a seguir especifica argumentos para dois deles: `Link`, para criar um link para a origem do conteúdo da área de transferência, e `DisplayAsIcon`, para exibir o link como um ícone. No C# 4.0 e versões posteriores, você pode usar argumentos nomeados para esses dois e omitir os outros. Embora esses sejam parâmetros de referência, você não precisa usar a palavra-chave `ref` ou criar variáveis para os enviar como argumentos. Você pode enviar os valores diretamente. No C# 3.0 e versões anteriores, você deve fornecer um argumento variável para cada parâmetro de referência.

     [!code-csharp[csProgGuideOfficeHowTo#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#9)]

     No C# 3.0 e versões anteriores da linguagem, o código a seguir mais complexo é necessário.

     [!code-csharp[csProgGuideOfficeHowTo#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#10)]

2. Adicione a instrução a seguir no final de `Main`.

     [!code-csharp[csProgGuideOfficeHowTo#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#11)]

3. Adicione a instrução a seguir no final de `DisplayInExcel`. O método `Copy` adiciona a planilha na área de transferência.

     [!code-csharp[csProgGuideOfficeHowTo#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#12)]

4. Pressione CTRL+F5.

     Um documento do Word é exibido contendo um ícone. Clique duas vezes no ícone para colocar a planilha no primeiro plano.

## <a name="to-set-the-embed-interop-types-property"></a>Para definir a propriedade Inserir Tipos Interop

1. Melhorias adicionais são possíveis quando você chama um tipo COM que não requer um assembly de interoperabilidade primário (PIA) no tempo de execução. A remoção da dependência nos PIAs resulta na independência de versão e em uma implantação mais fácil. Para obter mais informações sobre as vantagens da programação sem PIAs, consulte [Passo a passo: inserindo tipos de assemblies gerenciados](../../../standard/assembly/embed-types-visual-studio.md).

     Além disso, a programação é mais fácil porque os tipos necessários e retornados por métodos COM podem ser representados usando o tipo `dynamic`, em vez de `Object`. Variáveis com o tipo `dynamic` não são avaliadas até o tempo de execução, o que elimina a necessidade de conversão explícita. Para obter mais informações, veja [Usando o tipo dynamic](../types/using-type-dynamic.md).

     No C# 4, a inserção de informações de tipo, em vez do uso de PIAs, é o comportamento padrão. Devido a esse padrão, vários dos exemplos anteriores são simplificados pois a conversão explícita não é necessária. Por exemplo, a declaração de `worksheet` em `DisplayInExcel` é escrita como `Excel._Worksheet workSheet = excelApp.ActiveSheet`, em vez de `Excel._Worksheet workSheet = (Excel.Worksheet)excelApp.ActiveSheet`. As chamadas para `AutoFit` no mesmo método também exigem conversão explícita sem o padrão, pois `ExcelApp.Columns[1]` retorna um `Object`, e `AutoFit` é um método do Excel. O código a seguir mostra a conversão.

     [!code-csharp[csProgGuideOfficeHowTo#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#14)]

2. Para alterar o padrão e usar PIAs em vez de inserir informações de tipo, expanda o nó **Referências** no **Gerenciador de Soluções** e, em seguida, selecione **Microsoft.Office.Interop.Excel** ou **Microsoft.Office.Interop.Word**.

3. Se você não conseguir ver a janela **Propriedades**, pressione **F4**.

4. Localize **Inserir Tipos Interop** na lista de propriedades e altere seu valor para **False**. De maneira equivalente, você pode compilar usando a opção [-Reference](../../language-reference/compiler-options/reference-compiler-option.md) do compilador em vez de [-link](../../language-reference/compiler-options/link-compiler-option.md) em um prompt de comando.

## <a name="to-add-additional-formatting-to-the-table"></a>Para adicionar formatação adicional à tabela

1. Substitua as duas chamadas para `AutoFit` em `DisplayInExcel` pela instrução a seguir.

     [!code-csharp[csProgGuideOfficeHowTo#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#15)]

     O método <xref:Microsoft.Office.Interop.Excel.Range.AutoFormat%2A> tem sete parâmetros de valor, que são opcionais. Argumentos nomeados e opcionais permitem que você forneça argumentos para nenhum, alguns ou todos eles. Na instrução anterior, um argumento é fornecido para apenas um dos parâmetros, `Format`. Como `Format` é o primeiro parâmetro na lista de parâmetros, você não precisará fornecer o nome do parâmetro. No entanto, poderá ser mais fácil entender a instrução se o nome do parâmetro estiver incluído, conforme mostrado no código a seguir.

     [!code-csharp[csProgGuideOfficeHowTo#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#16)]

2. Pressione CTRL + F5 para ver o resultado. Outros formatos estão listados na enumeração <xref:Microsoft.Office.Interop.Excel.XlRangeAutoFormat>.

3. Compare a instrução na etapa 1 com o código a seguir, que mostra os argumentos que são necessários no C# 3.0 ou versões anteriores.

     [!code-csharp[csProgGuideOfficeHowTo#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#17)]

## <a name="example"></a>Exemplo

O código a seguir mostra um exemplo completo.

[!code-csharp[csProgGuideOfficeHowTo#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/walkthrough.cs#18)]

## <a name="see-also"></a>Confira também

- <xref:System.Type.Missing?displayProperty=nameWithType>
- [dinâmico](../../language-reference/builtin-types/reference-types.md)
- [Usando o tipo dynamic](../types/using-type-dynamic.md)
- [Argumentos nomeados e opcionais](../classes-and-structs/named-and-optional-arguments.md)
- [Como usar argumentos nomeados e opcionais na programação do Office](../classes-and-structs/how-to-use-named-and-optional-arguments-in-office-programming.md)
