---
title: Objetos e classes
ms.date: 05/26/2020
helpviewer_keywords:
- classes [Visual Basic]
- objects [Visual Basic]
ms.assetid: c68c5752-1006-46e1-975a-6717b62a42fc
ms.openlocfilehash: 9e3cf262ef617a1ae5ee92bcc3d6fd5c691602f9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600407"
---
# <a name="objects-and-classes-in-visual-basic"></a>Objetos e classes no Visual Basic

Um *objeto* é uma combinação de código e dados que podem ser tratados como uma unidade. Um objeto pode ser uma parte de um aplicativo, como um controle ou um formulário. Todo o aplicativo também pode ser um objeto.

Ao criar um aplicativo no Visual Basic, você trabalha constantemente com objetos. Você pode usar objetos fornecidos por Visual Basic, como controles, formulários e objetos de acesso a dados. Você também pode usar objetos de outros aplicativos dentro de seu aplicativo Visual Basic. Você pode até mesmo criar seus próprios objetos e definir propriedades e métodos adicionais para eles. Os objetos atuam como blocos de construção pré-fabricados para programas. Eles permitem que você escreva um trecho de código uma vez e reutilize repetidamente.

Este tópico discute os objetos em detalhes.

## <a name="objects-and-classes"></a>Objetos e classes

Cada objeto no Visual Basic é definido por uma *classe*. Uma classe descreve as variáveis, as propriedades, os procedimentos e os eventos de um objeto. Os objetos são instâncias de classes. Você pode criar a quantidade de objetos que precisar após ter definido uma classe.

Para entender a relação entre um objeto e sua classe, pense em cookies e cortadores de cookie. O cortador de cookie é a classe. Ele define as características de cada cookie, por exemplo, tamanho e forma. A classe é usada para criar objetos. Os objetos são os cookies.

Você deve criar um objeto antes de poder acessar seus membros, exceto para [`Shared`](../../../language-reference/modifiers/shared.md) Membros que podem ser acessados sem um objeto da classe.

### <a name="create-an-object-from-a-class"></a>Criar um objeto a partir de uma classe

1. Determine de qual classe você deseja criar um objeto ou defina sua própria classe. Por exemplo:

   ```vb
   Public Class Customer
       Public Property AccountNumber As Integer
   End Class
   ```

2. Escreva uma [instrução Dim](../../../language-reference/statements/dim-statement.md) para criar uma variável a qual você pode atribuir uma instância da classe. A variável deve ser do tipo da classe desejada.

   ```vb
   Dim nextCustomer As Customer
   ```

3. Adicione a palavra-chave [novo operador](../../../language-reference/operators/new-operator.md) para inicializar a variável a uma nova instância da classe.

   ```vb
   Dim nextCustomer As New Customer
   ```

4. Agora você pode acessar os membros da classe pela variável de objeto.

   ```vb
   nextCustomer.AccountNumber = lastAccountNumber + 1
   ```

> [!NOTE]
> Sempre que possível, você deve declarar a variável para ser do tipo de classe que você pretende atribuir a ela. Isso é chamado de *associação inicial*. Se não souber o tipo de classe no tempo de compilação, você poderá invocar a *associação tardia*, declarando a variável como o [Tipo de Dados do Objeto](../../../language-reference/data-types/object-data-type.md). Entretanto, a associação tardia pode tornar o desempenho mais lento e limitar o acesso a membros do objeto do tempo de execução. Para obter mais informações, consulte [Declaração de variável de objeto](../variables/object-variable-declaration.md).

### <a name="multiple-instances"></a>Várias instâncias

Objetos recentemente criados de uma classe geralmente são idênticos entre si. Assim que existem como objetos individuais, no entanto, suas propriedades e variáveis podem ser alteradas independentemente de outras instâncias. Por exemplo, se você adicionar três caixas de seleção a um formulário, cada objeto da caixa de seleção será uma instância da classe <xref:System.Windows.Forms.CheckBox>. Os objetos <xref:System.Windows.Forms.CheckBox> individuais compartilham um conjunto comum de características e recursos (propriedades, variáveis, procedimentos e eventos) definidos pela classe. No entanto, cada um tem seu próprio nome, pode ser separadamente habilitado e desabilitado e pode ser colocado em um local diferente no formulário.

## <a name="object-members"></a>Membros do objeto

Um objeto é um elemento de um aplicativo, que representa uma *instância* de uma classe. Campos, propriedades, métodos e eventos são os blocos de construção de objetos e constituem seus *membros*.

### <a name="member-access"></a>Acesso de membro

É possível acessar um membro de um objeto especificando, em ordem, o nome da variável do objeto, um período (`.`) e o nome do membro. O exemplo a seguir define a propriedade <xref:System.Windows.Forms.Control.Text%2A> de um objeto <xref:System.Windows.Forms.Label>.

```vb
warningLabel.Text = "Data not saved"
```

#### <a name="intellisense-listing-of-members"></a>Lista do IntelliSense de membros

O IntelliSense lista os membros de uma classe quando você invoca sua opção de membros da lista, por exemplo, ao digitar um ponto final (`.`) como um operador de acesso de membro. Se você digitar o período após o nome de uma variável declarada como uma instância da classe, o IntelliSense listará todos os membros de instância e nenhum dos membros compartilhados. Se você digitar o ponto final após o nome da classe, o IntelliSense listará todos os membros compartilhados e nenhum dos membros de instância. Para obter mais informações, veja [Usando o IntelliSense](/visualstudio/ide/using-intellisense).

### <a name="fields-and-properties"></a>Campos e propriedades

Os *campos* e as *propriedades* representam as informações armazenadas em um objeto. Você recupera e define seus valores com instruções de atribuição da mesma maneira que você recupera e define variáveis locais em um procedimento. O exemplo a seguir recupera a propriedade <xref:System.Windows.Forms.Control.Width%2A> e define a propriedade <xref:System.Windows.Forms.Control.ForeColor%2A> de um objeto <xref:System.Windows.Forms.Label>.

```vb
Dim warningWidth As Integer = warningLabel.Width
warningLabel.ForeColor = System.Drawing.Color.Red
```

Observe que um campo também é chamado de uma *variável membro*.

Use os procedimentos de propriedade quando:

- Precisar controlar quando e como um valor é definido ou recuperado.

- A propriedade tiver um conjunto bem definido de valores que precisam ser validados.

- Definir o valor causa algumas alterações perceptíveis no estado do objeto, como um propriedade `IsVisible`.

- Definir a propriedade faz alterações em outras variáveis internas ou nos valores de outras propriedades.

- Um conjunto de etapas deve ser executado antes de a propriedade pode ser definida ou recuperada.

Use os campos quando:

- O valor for de um tipo de validação automática. Por exemplo, um erro ou a conversão automática de dados ocorrerá se um valor diferente de `True` ou `False` for atribuído a uma variável `Boolean`.

- Qualquer valor no intervalo com suporte do tipo de dados é válido. Isso é verdadeiro para muitas propriedades de tipo `Single` ou `Double`.

- A propriedade é um tipo de dados `String` e não há restrição sobre o tamanho ou o valor da cadeia de caracteres.

- Para obter mais informações, consulte [Procedimentos de propriedade](../procedures/property-procedures.md).

> [!TIP]
> Sempre mantenha os campos não constantes particulares. Quando você quiser torná-lo público, use uma propriedade em vez disso.

### <a name="methods"></a>Métodos

Um *método* é uma ação que um objeto pode executar. Por exemplo, <xref:System.Windows.Forms.ComboBox.ObjectCollection.Add%2A> é um método do objeto <xref:System.Windows.Forms.ComboBox> que adiciona uma nova entrada para uma caixa de combinação.

O exemplo a seguir demonstra o método <xref:System.Windows.Forms.Timer.Start%2A> de um objeto <xref:System.Windows.Forms.Timer>.

```vb
Dim safetyTimer As New System.Windows.Forms.Timer
safetyTimer.Start()
```

Observe que um método é simplesmente um *procedimento* que é exposto por um objeto.

Para obter mais informações, consulte [Procedimentos](../procedures/index.md).

### <a name="events"></a>Eventos

Um evento é uma ação reconhecida por um objeto, como clicar com o mouse ou pressionar uma tecla e para o qual você pode escrever código para responder. Os eventos podem ocorrer como resultado de uma ação do usuário ou código do programa, ou eles podem ser causados pelo sistema. O código que sinaliza um evento *aciona* o evento e o código que responde a ele *lida* com ele.

Você também pode desenvolver seus próprios eventos personalizados para serem acionados por seus objetos e manipulados por outros objetos. Para obter mais informações, consulte [Eventos](../events/index.md).

### <a name="instance-members-and-shared-members"></a>Membros de instância e membros compartilhados

Quando você cria um objeto de uma classe, o resultado é uma instância dessa classe. Membros que não são declarados com a palavra-chave [compartilhado](../../../language-reference/modifiers/shared.md) são *membros de instância*, que pertencem estritamente a essa instância em particular. Um membro de instância em uma instância é independente do mesmo membro em outra instância da mesma classe. Por exemplo, uma variável de membro de instância pode ter valores diferentes em instâncias diferentes.

Membros declarados com a palavra-chave `Shared` são *membros compartilhados*, que pertencem à classe como um todo e não a qualquer instância específica. Um membro compartilhado existe somente uma vez, independentemente de quantas instâncias de sua classe você cria ou mesmo se você não criar nenhuma instância. Uma variável de membro compartilhado, por exemplo, tem apenas um valor, que está disponível para todo o código que pode acessar a classe.

#### <a name="accessing-non-shared-members"></a>Acessando membros não compartilhados

1. Verifique se o objeto foi criado com base na sua classe e atribuído a uma variável de objeto.

   ```vb
   Dim secondForm As New System.Windows.Forms.Form
   ```

2. Na instrução que acessa o membro, siga o nome da variável de objeto com o *operador de acesso para membro* ( `.` ) e, em seguida, o nome do membro.

   ```vb
   secondForm.Show()
   ```

#### <a name="accessing-shared-members"></a>Acessando membros compartilhados

- Siga o nome da classe com o *operador de acesso para membro* ( `.` ) e, em seguida, o nome do membro. Você sempre deve acessar um membro do objeto `Shared` diretamente pelo nome da classe.

   ```vb
   Console.WriteLine("This computer is called " & Environment.MachineName)
   ```

- Se já tiver criado um objeto da classe, você também poderá acessar um membro `Shared` pela variável do objeto.

### <a name="differences-between-classes-and-modules"></a>Diferenças entre classes e módulos

A principal diferença entre classes e módulos é que as classes podem ser instanciadas como objetos enquanto os módulos padrão não podem. Como há apenas uma cópia dos dados de um módulo padrão, quando uma parte do seu programa altera uma variável pública em um módulo padrão, qualquer outra parte do programa obtém o mesmo valor se ler essa variável. Em contraste, os dados do objeto existem separadamente para cada objeto instanciado. Outra diferença é que, diferentemente dos módulos padrão, as classes podem implementar interfaces. Se uma classe for marcada com o modificador [MustInherit](../../../language-reference/modifiers/mustinherit.md) , ela não poderá ser instanciada diretamente. No entanto, ele ainda é diferente de um módulo, pois pode ser herdado enquanto os módulos não podem ser herdados.

> [!NOTE]
> Quando o modificador `Shared` é aplicado a um membro de classe, ele é associado à classe em si em vez de uma determinada instância da classe. O membro é acessado diretamente usando o nome de classe, do mesmo que os membros do módulo são acessados.

Classes e módulos também usam escopos diferentes para seus membros. Membros definidos em uma classe têm escopo em uma instância específica da classe e existem somente para o tempo de vida do objeto. Para acessar membros de classe de fora de uma classe, você deve usar nomes totalmente qualificados no formato de *Objeto*.*Membro*.

Por outro lado, os membros declarados dentro de um módulo são acessíveis publicamente por padrão e podem ser acessados por qualquer código que pode acessar o módulo. Isso significa que variáveis em um módulo padrão são variáveis globais efetivamente porque elas são visíveis de qualquer lugar em seu projeto e existem durante a vida útil do programa.

## <a name="reusing-classes-and-objects"></a>Reutilização de classes e objetos

Os objetos permitem que você declare variáveis e procedimentos uma vez e reutilize-os quando necessário. Por exemplo, se desejar adicionar um verificador ortográfico a um aplicativo, defina todas as variáveis e funções de suporte para fornecer a funcionalidade de verificação ortográfica. Se criar o verificador de ortografia como uma classe, você poderá reutilizá-lo em outros aplicativos adicionando uma referência ao assembly compilado. Melhor ainda, você poderá diminuir seu trabalho usando uma classe de verificador ortográfico que alguém já desenvolveu.

O .NET fornece muitos exemplos de componentes que estão disponíveis para uso. O exemplo a seguir usa a classe <xref:System.TimeZone> no namespace <xref:System>. <xref:System.TimeZone> fornece membros que permitem que você recupere informações sobre o fuso horário do sistema atual do computador.

```vb
Public Sub ExamineTimeZone()
    Dim tz As System.TimeZone = System.TimeZone.CurrentTimeZone
    Dim s As String = "Current time zone is "
    s &= CStr(tz.GetUtcOffset(Now).Hours) & " hours and "
    s &= CStr(tz.GetUtcOffset(Now).Minutes) & " minutes "
    s &= "different from UTC (coordinated universal time)"
    s &= vbCrLf & "and is currently "
    If tz.IsDaylightSavingTime(Now) = False Then s &= "not "
    s &= "on ""summer time""."
    Console.WriteLine(s)
End Sub
```

No exemplo anterior, o primeiro [Demonstrativo Esmaecido](../../../language-reference/statements/dim-statement.md) declara uma variável de objeto do tipo <xref:System.TimeZone> e atribui a ele um objeto <xref:System.TimeZone> retornado pela propriedade <xref:System.TimeZone.CurrentTimeZone%2A>.

## <a name="relationships-among-objects"></a>Relacionamentos entre objetos

Os objetos podem estar relacionados entre si de várias maneiras. Os principais tipos de relação são *hierárquico* e *confinamento*.

### <a name="hierarchical-relationship"></a>Relação hierárquica

Quando classes são derivadas de classes mais fundamentais, elas devem ter um *relacionamento hierárquico*. As hierarquias de classe são úteis para descrever itens que são um subtipo de uma classe mais geral.

No exemplo a seguir, suponha que você deseja definir um tipo especial de <xref:System.Windows.Forms.Button> que atua como um <xref:System.Windows.Forms.Button> normal, mas também expõe um método que reverte as cores de primeiro plano e da tela de fundo.

#### <a name="define-a-class-that-is-derived-from-an-already-existing-class"></a>Definir uma classe que é derivada de uma classe já existente

1. Use uma [instrução Class](../../../language-reference/statements/class-statement.md) para definir uma classe da qual criar o objeto que você precisa.

   ```vb
   Public Class ReversibleButton
   ```

   Certifique-se de que uma instrução `End Class` segue a última linha de código em sua classe. Por padrão, o IDE (ambiente de desenvolvimento integrado) gera automaticamente um `End Class` quando você insere uma instrução `Class`.

2. Siga a instrução `Class` imediatamente com uma [instrução Inherits](../../../language-reference/statements/inherits-statement.md). Especifique a classe da qual a nova classe deriva.

   ```vb
   Inherits System.Windows.Forms.Button
   ```

   A nova classe herda todos os membros definidos pela classe base.

3. Adicione o código para os membros adicionais que sua classe derivada expõe. Por exemplo, você pode adicionar um método `ReverseColors` e sua classe derivada será semelhante ao seguinte:

   ```vb
   Public Class ReversibleButton
       Inherits System.Windows.Forms.Button
           Public Sub ReverseColors()
               Dim saveColor As System.Drawing.Color = Me.BackColor
               Me.BackColor = Me.ForeColor
               Me.ForeColor = saveColor
          End Sub
   End Class
   ```

   Se você criar um objeto da `ReversibleButton` classe, ele poderá acessar todos os membros da <xref:System.Windows.Forms.Button> classe, bem como o `ReverseColors` método e quaisquer outros novos membros que você definir no `ReversibleButton` .

As classes derivadas herdam membros da classe que eles se baseiam, permitindo que você adicione complexidade enquanto progride em uma hierarquia de classe. Para obter mais informações, consulte [Noções básicas de herança](inheritance-basics.md).

### <a name="compile-the-code"></a>Compilar o código

Certifique-se de que o compilador pode acessar a classe da qual você pretende derivar sua nova classe. Isso pode significar qualificar totalmente seu nome, como no exemplo anterior, ou identificar seu namespace em uma [Instrução Imports (Tipo e Namespace .NET)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md). Se a classe estiver em um projeto diferente, você precisará adicionar uma referência ao projeto. Para obter mais informações, consulte [Gerenciando referências em um projeto](/visualstudio/ide/managing-references-in-a-project).

### <a name="containment-relationship"></a>Relação de confinamento

Outro modo de relação dos objetos é a *relação de confinamento*. Os objetos de contêiner encapsulam logicamente outros objetos. Por exemplo, o objeto <xref:System.OperatingSystem> logicamente contém um objeto <xref:System.Version>, que ele retorna por intermédio de sua propriedade <xref:System.OperatingSystem.Version%2A>. Observe que o objeto de contêiner não contém fisicamente qualquer outro objeto.

#### <a name="collections"></a>Coleções

Um tipo específico de confinamento de objeto é representado pelas *coleções*. As coleções são grupos de objetos semelhantes que podem ser enumerados. Visual Basic dá suporte a uma sintaxe específica no [para cada... Próxima instrução](../../../language-reference/statements/for-each-next-statement.md) que permite que você itere pelos itens de uma coleção. Além disso, as coleções permitem, com frequência, que você use um <xref:Microsoft.VisualBasic.Collection.Item%2A> para recuperar elementos pelo índice ou associando-os com uma cadeia de caracteres exclusiva. As coleções podem ser mais fáceis de usar que matrizes porque elas permitem que você adicione ou remova itens sem usar índices. Devido à facilidade de uso, as coleções geralmente são usadas para armazenar formulários e controles.

## <a name="related-topics"></a>Tópicos relacionados

[Walkthrough: definindo classes](walkthrough-defining-classes.md)\
Fornece uma descrição passo a passo de como criar uma classe.

[Propriedades e métodos sobrecarregados](overloaded-properties-and-methods.md)\
Propriedades e métodos sobrecarregados

[Noções básicas de herança](inheritance-basics.md)\
Aborda modificadores de herança, substituindo métodos e propriedades, MyClass e MyBase.

[Tempo de vida do objeto: como os objetos são criados e destruídos](object-lifetime-how-objects-are-created-and-destroyed.md)\
Aborda a criação e descarte de instâncias de classe.

[Tipos anônimos](anonymous-types.md)\
Descreve como criar e usar tipos anônimos, que permitem que você crie objetos sem escrever uma definição de classe para o tipo de dados.

[Inicializadores de objeto: tipos nomeados e anônimos](object-initializers-named-and-anonymous-types.md)\
Discute inicializadores de objeto, que são usados para criar instâncias de tipos nomeados e anônimos usando uma única expressão.

[Como: inferir nomes e tipos de propriedade em declarações de tipo anônimo](how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)\
Explica como inferir nomes e tipos de propriedade em declarações de tipo anônimo. Fornece exemplos de inferência de tipos com e sem êxito.
