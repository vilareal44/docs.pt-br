---
title: 'Passo a passo: Programação do Office (C# e Visual Basic)'
description: Saiba mais sobre os recursos que o Visual Studio oferece em C# e Visual Basic que melhoram a programação de Microsoft Office.
ms.date: 07/20/2015
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Office, programming in Visual Basic and C#
- Office programming [C#]
- Office programming [Visual Basic]
ms.assetid: 519cff31-f80b-4f0e-a56b-26358d0f8c51
ms.openlocfilehash: 76f0e2eccb5d1a59d9aaa3eed11b25dd2dd9cac3
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062997"
---
# <a name="walkthrough-office-programming-c-and-visual-basic"></a>Passo a passo: Programação do Office (C# e Visual Basic)

O Visual Studio oferece funcionalidades no C# e no Visual Basic que melhoram a programação do Microsoft Office. As funcionalidades úteis do C# incluem argumentos nomeados e opcionais e valores retornados do tipo `dynamic`. Na programação COM, você pode omitir a palavra-chave `ref` e obter acesso a propriedades indexadas. As funcionalidades do Visual Basic incluem propriedades autoimplementadas, instruções em expressões lambda e inicializadores de coleção.

Ambas as linguagens permitem incorporar as informações de tipo, que permitem a implantação de assemblies que interagem com componentes COM sem implantar assemblies de interoperabilidade primários (PIAs) no computador do usuário. Para obter mais informações, consulte [Instruções passo a passo: Inserindo tipos de assemblies gerenciados](../../../standard/assembly/embed-types-visual-studio.md).

Este passo a passo demonstra essas funcionalidades no contexto de programação do Office, mas muitos deles também são úteis na programação em geral. No passo a passo, você usa um aplicativo Suplemento do Excel para criar uma pasta de trabalho do Excel. Em seguida, você cria um documento do Word que contém um link para a pasta de trabalho. Por fim, você vê como habilitar e desabilitar a dependência de PIA.

## <a name="prerequisites"></a>Pré-requisitos

Você deve ter o Microsoft Office Excel e o Microsoft Office Word instalados no computador para concluir esse passo a passo.

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

### <a name="to-set-up-an-excel-add-in-application"></a>Para configurar um aplicativo Suplemento do Excel

1. Inicie o Visual Studio.

2. No menu **Arquivo** , aponte para **Novo**e clique em **Projeto**.

3. No painel **Modelos Instalados**, expanda **Visual Basic** ou **Visual C#**, expanda **Office** e, em seguida, clique no ano da versão do produto do Office.

4. No painel **modelos** , clique em ** \<version> suplemento do Excel**.

5. Observe a parte superior do painel **Modelos** para se certificar de que **.NET Framework 4** ou uma versão posterior, é exibido na caixa **Estrutura de Destino**.

6. Digite um nome para seu projeto na caixa **Nome**, se desejar.

7. Clique em **OK**.

8. O novo projeto aparece no **Gerenciador de Soluções**.

### <a name="to-add-references"></a>Para adicionar referências

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto e, em seguida, clique em **Adicionar Referência**. A caixa de diálogo **Adicionar Referência** é exibida.

2. Na guia **Assemblies**, selecione **Microsoft.Office.Interop.Excel**, versão `<version>.0.0.0` (para uma chave para os números de versão de produto do Office, consulte [Versões da Microsoft](https://en.wikipedia.org/wiki/Microsoft_Office#Versions)), na lista **Nome do componente** e mantenha a tecla CTRL pressionada e selecione **Microsoft.Office.Interop.Word**, `version <version>.0.0.0`. Se você não vir os assemblies, talvez seja necessário garantir que eles estejam instalados e exibidos (consulte [como instalar assemblies de interoperabilidade primária do Office](/visualstudio/vsto/how-to-install-office-primary-interop-assemblies)).

3. Clique em **OK**.

### <a name="to-add-necessary-imports-statements-or-using-directives"></a>Para adicionar instruções Imports necessárias ou diretivas de uso

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no arquivo **ThisAddIn.vb** ou **ThisAddIn.cs** e clique em **Exibir Código**.

2. Adicione as seguintes instruções `Imports` (Visual Basic) ou diretivas `using` (C#) à parte superior do arquivo de código, se elas ainda não estiverem presentes.

     [!code-csharp[csOfficeWalkthrough#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#1)]

     [!code-vb[csOfficeWalkthrough#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#1)]

### <a name="to-create-a-list-of-bank-accounts"></a>Para criar uma lista de contas bancárias

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto, clique em **Adicionar** e clique em **Classe**. Nomeie a classe Account.vb se você estiver usando Visual Basic ou Account.cs, se você estiver usando C#. Clique em **Adicionar**.

2. Substitua a definição da classe `Account` pelo código a seguir. As definições de classe usam *propriedades autoimplementadas*. Para obter mais informações, consulte [Propriedades autoimplementadas](../../../visual-basic/programming-guide/language-features/procedures/auto-implemented-properties.md).

     [!code-csharp[csOfficeWalkthrough#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/account.cs#2)]

     [!code-vb[csOfficeWalkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/account.vb#2)]

3. Para criar uma `bankAccounts` lista que contém duas contas, adicione o código a seguir ao `ThisAddIn_Startup` método em *ThisAddIn. vb* ou *ThisAddIn.cs*. As declarações de lista usam *inicializadores de coleção*. Para obter mais informações, consulte [Inicializadores de coleção](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md).

     [!code-csharp[csOfficeWalkthrough#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#3)]

     [!code-vb[csOfficeWalkthrough#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#3)]

### <a name="to-export-data-to-excel"></a>Para exportar dados para o Excel

1. No mesmo arquivo, adicione o método a seguir para a classe `ThisAddIn`. O método configura uma planilha do Excel e exporta dados para ela.

     [!code-csharp[csOfficeWalkthrough#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#4)]

     [!code-vb[csOfficeWalkthrough#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#4)]

     Dois novos recursos do C# são usados neste método. Esses dois recursos já existem no Visual Basic.

    - O método [Add](<xref:Microsoft.Office.Interop.Excel.Workbooks.Add%2A>) tem um *parâmetro opcional* para especificar um modelo específico. Os parâmetros opcionais, novos no C# 4, permitem omitir o argumento para esse parâmetro se você quiser usar o valor padrão do parâmetro. Como nenhum argumento é enviado no código anterior, `Add` usa o modelo padrão e cria uma nova pasta de trabalho. A instrução equivalente em versões anteriores do C# requer um argumento de espaço reservado: `excelApp.Workbooks.Add(Type.Missing)`.

         Para obter mais informações, consulte [argumentos nomeados e opcionais](../classes-and-structs/named-and-optional-arguments.md).

    - As propriedades `Range` e `Offset` do objeto [Range](<xref:Microsoft.Office.Interop.Excel.Range>) usam o recurso de *propriedades indexadas*. Este recurso permite consumir essas propriedades de tipos COM usando a sintaxe típica do C# a seguir. Propriedades indexadas também permitem que você use a propriedade `Value` do objeto `Range`, eliminando a necessidade de usar a propriedade `Value2`. A propriedade `Value` é indexada, mas o índice é opcional. Argumentos opcionais e propriedades indexadas trabalham juntos no exemplo a seguir.

         [!code-csharp[csOfficeWalkthrough#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#5)]

         Em versões anteriores da linguagem, a sintaxe especial a seguir é obrigatória.

         [!code-csharp[csOfficeWalkthrough#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#6)]

         Não é possível criar propriedades indexadas de sua preferência. O recurso dá suporte apenas ao consumo de propriedades indexadas existentes.

         Para obter mais informações, consulte [como usar propriedades indexadas em programação de interoperabilidade com](./how-to-use-indexed-properties-in-com-interop-rogramming.md).

2. Adicione o seguinte código no final de `DisplayInExcel` para ajustar as larguras das colunas para adequar o conteúdo.

     [!code-csharp[csOfficeWalkthrough#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#7)]

     [!code-vb[csOfficeWalkthrough#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#7)]

     Essas adições demonstram outro recurso no C#: tratar valores `Object` retornados de hosts COM como o Office, como se eles tivessem o tipo [dinâmico](../../language-reference/builtin-types/reference-types.md). Isso ocorre automaticamente quando os **tipos de interoperabilidade de inserção** são definidos com o valor padrão, ou, de maneira `True` equivalente, quando o assembly é referenciado pela opção de compilador [-link](../../language-reference/compiler-options/link-compiler-option.md) . O tipo `dynamic` permite a vinculação posterior, já disponível no Visual Basic, e evita a conversão explícita necessária no C# 3.0 e em versões anteriores da linguagem.

     Por exemplo, `excelApp.Columns[1]` retorna um `Object` e `AutoFit` é um método [Range](<xref:Microsoft.Office.Interop.Excel.Range>) do Excel. Sem `dynamic`, você deve converter o objeto retornado em `excelApp.Columns[1]` como uma instância de `Range` antes de chamar o método `AutoFit`.

     [!code-csharp[csOfficeWalkthrough#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#8)]

     Para obter mais informações sobre como inserir tipos de interoperabilidade, consulte os procedimentos "Para localizar a referência de PIA" e "Para restaurar a dependência de PIA" posteriormente neste tópico. Para obter mais informações sobre `dynamic`, consulte [dynamic](../../language-reference/builtin-types/reference-types.md) ou [Usando o tipo dynamic](../types/using-type-dynamic.md).

### <a name="to-invoke-displayinexcel"></a>Para invocar DisplayInExcel

1. Adicione o código a seguir no final do método `ThisAddIn_StartUp`. A chamada para `DisplayInExcel` contém dois argumentos. O primeiro argumento é o nome da lista de contas a ser processada. O segundo argumento é uma expressão lambda com várias linhas que define como os dados deverão ser processados. Os valores `ID` e `balance` de cada conta serão exibidos em células adjacentes e a linha será exibida em vermelho se o equilíbrio for menor do que zero. Para obter mais informações, consulte [Expressões Lambda](../../language-reference/operators/lambda-expressions.md).

     [!code-csharp[csOfficeWalkthrough#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#9)]

     [!code-vb[csOfficeWalkthrough#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#9)]

2. Para executar o programa, pressione F5. Uma planilha do Excel é exibida contendo os dados das contas.

### <a name="to-add-a-word-document"></a>Para adicionar um documento do Word

1. Adicione o código a seguir ao final do método `ThisAddIn_StartUp` para criar um documento do Word que contém um link para a pasta de trabalho do Excel.

     [!code-csharp[csOfficeWalkthrough#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#10)]

     [!code-vb[csOfficeWalkthrough#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#10)]

     Esse código demonstra vários dos novos recursos no C#: a capacidade para omitir a palavra-chave `ref` em programação COM, argumentos nomeados e argumentos opcionais. Esses recursos já existem no Visual Basic. O método [PasteSpecial](<xref:Microsoft.Office.Interop.Word.Selection.PasteSpecial%2A>) tem sete parâmetros, todos definidos como parâmetros de referência opcionais. Argumentos opcionais e nomeados permitem designar os parâmetros que você deseja acessar pelo nome e enviar argumentos apenas para esses parâmetros. Neste exemplo, os argumentos são enviados para indicar que deve ser criado um link para a pasta de trabalho na área de transferência (parâmetro `Link`), e que o link será exibido no documento do Word como um ícone (parâmetro `DisplayAsIcon`). O Visual C# permite também que você omita a palavra-chave `ref` nesses argumentos.

### <a name="to-run-the-application"></a>Para executar o aplicativo

1. Pressione F5 para executar o aplicativo. O Excel é iniciado e exibe uma tabela que contém as informações das duas contas em `bankAccounts`. Em seguida, um documento do Word é exibido contendo um link para a tabela do Excel.

### <a name="to-clean-up-the-completed-project"></a>Para limpar o projeto concluído

1. No Visual Studio, clique em **Limpar Solução** no menu **Compilar**. Caso contrário, o suplemento será executado toda vez que você abrir o Excel no seu computador.

### <a name="to-find-the-pia-reference"></a>Para localizar a referência de PIA

1. Execute o aplicativo novamente, mas não clique em **Limpar Solução**.

2. Selecione **Iniciar**. Localize **Microsoft Visual Studio \<version> ** e abra um prompt de comando do desenvolvedor.

3. Digite `ildasm` na janela Prompt de Comando do Desenvolvedor para Visual Studio e pressione Enter. A janela IL DASM é exibida.

4. No menu **Arquivo** na janela IL DASM, selecione **Arquivo** > **Abrir**. Clique duas vezes em **Visual \<version> Studio **e, em seguida, clique duas vezes em **projetos**. Abra a pasta do seu projeto e procure na pasta bin/Debug por *nome do projeto*.dll. Clique duas vezes em *nome do projeto*.dll. Uma nova janela exibe os atributos do projeto, além das referências a outros módulos e assemblies. Observe que os namespaces `Microsoft.Office.Interop.Excel` e `Microsoft.Office.Interop.Word` estão incluídos no assembly. Por padrão, no Visual Studio, o compilador importa os tipos que você precisa de um PIA referenciado para o seu assembly.

     Para obter mais informações, consulte [Como exibir o conteúdo de um assembly](../../../standard/assembly/view-contents.md).

5. Clique duas vezes no ícone **MANIFEST**. Uma janela será exibida contendo uma lista de assemblies que contêm itens referenciados pelo projeto. `Microsoft.Office.Interop.Excel` e `Microsoft.Office.Interop.Word` não estão incluídos na lista. Como os tipos do seu projeto precisam ter sido importados para o assembly, referências a um PIA não são necessárias. Isso facilita a implantação. Os PIAs não precisam estar presentes no computador do usuário e como um aplicativo não requer a implantação de uma versão específica de um PIA, os aplicativos podem ser projetados para trabalhar com várias versões do Office, desde que as APIs necessárias existam em todas as versões.

     Como a implantação de PIAs não é mais necessária, você pode criar um aplicativo em cenários avançados que funcione com várias versões do Office, incluindo versões anteriores. No entanto, isso funcionará apenas se seu código não usar quaisquer APIs que não estejam disponíveis na versão do Office na qual você está trabalhando. Não é sempre claro se uma determinada API estava disponível em uma versão anterior e, por essa razão, não é recomendado trabalhar com versões anteriores do Office.

    > [!NOTE]
    > O Office não publicou PIAs antes do Office 2003. Portanto, a única maneira de gerar um assembly de interoperabilidade para o Office 2002 ou versões anteriores é importando referência COM.

6. Feche a janela do manifesto e a janela do assembly.

### <a name="to-restore-the-pia-dependency"></a>Para restaurar a dependência de PIA

1. Em **Gerenciador de Soluções**, clique no botão **Mostrar Todos os Arquivos**. Expanda a pasta **Referências** e selecione **Microsoft.Office.Interop.Excel**. Pressione F4 para exibir a janela **Propriedades**.

2. Na janela **Propriedades**, altere a propriedade **Inserir Tipos de Interoperabilidade** de **True** para **False**.

3. Repita as etapas 1 e 2 deste procedimento para `Microsoft.Office.Interop.Word`.

4. Em C#, comente as duas chamadas para `Autofit` no final do método `DisplayInExcel`.

5. Pressione F5 para verificar se o projeto ainda é executado corretamente.

6. Repita as etapas de 1 a 3 do procedimento anterior para abrir a janela do assembly. Observe que `Microsoft.Office.Interop.Word` e `Microsoft.Office.Interop.Excel` não estão mais na lista de assemblies inseridos.

7. Clique duas vezes no ícone **MANIFEST** e role a lista de assemblies referenciados. Ambos `Microsoft.Office.Interop.Word` e `Microsoft.Office.Interop.Excel` estão na lista. Como o aplicativo faz referência aos PIAs do Excel e do Word e a propriedade **Inserir Tipos de Interoperabilidade** está definida como **False**, ambos os assemblies devem existir no computador do usuário final.

8. No Visual Studio, clique em **Limpar Solução** no menu **Compilar** para limpar o projeto concluído.

## <a name="see-also"></a>Consulte também

- [Propriedades autoimplementadas (Visual Basic)](../../../visual-basic/programming-guide/language-features/procedures/auto-implemented-properties.md)
- [Propriedades autoimplementadas (C#)](../classes-and-structs/auto-implemented-properties.md)
- [Inicializadores de coleção](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)
- [Inicializadores de objeto e coleção](../classes-and-structs/object-and-collection-initializers.md)
- [Parâmetros Opcionais](../../../visual-basic/programming-guide/language-features/procedures/optional-parameters.md)
- [Passando argumentos por posição e nome](../../../visual-basic/programming-guide/language-features/procedures/passing-arguments-by-position-and-by-name.md)
- [Argumentos nomeados e opcionais](../classes-and-structs/named-and-optional-arguments.md)
- [Associação antecipada e tardia](../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)
- [dinâmico](../../language-reference/builtin-types/reference-types.md)
- [Usando o tipo dynamic](../types/using-type-dynamic.md)
- [Expressões lambda (Visual Basic)](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)
- [Expressões lambda [C#]](../../language-reference/operators/lambda-expressions.md)
- [Como usar propriedades indexadas na programação para interoperabilidade COM](./how-to-use-indexed-properties-in-com-interop-rogramming.md)
- [Passo a passo: inserindo informações de tipo dos Microsoft Office Assemblies no Visual Studio](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/ee317478(v%3dvs.120))
- [Instruções passo a passo: Inserindo tipos de assemblies gerenciados](../../../standard/assembly/embed-types-visual-studio.md)
- [Passo a passo: criando o primeiro suplemento do VSTO para Excel](/visualstudio/vsto/walkthrough-creating-your-first-vsto-add-in-for-excel)
- [Interoperabilidade COM](../../../visual-basic/programming-guide/com-interop/index.md)
- [Interoperabilidade](./index.md)
