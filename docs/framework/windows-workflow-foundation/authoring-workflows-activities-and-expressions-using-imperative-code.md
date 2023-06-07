---
title: Criando fluxos de trabalho, atividades e expressões de design usando o código obrigatório
description: Uma definição de fluxo de trabalho do Workflow Foundation é uma árvore de objetos de atividade configurados. Use o código para criar definições, atividades e expressões de fluxo de trabalho.
ms.date: 03/30/2017
ms.assetid: cefc9cfc-2882-4eb9-8c94-7a6da957f2b2
ms.openlocfilehash: d169049c47c154858a2e653b5f286fa6b66ba44d
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063790"
---
# <a name="authoring-workflows-activities-and-expressions-using-imperative-code"></a>Criando fluxos de trabalho, atividades e expressões de design usando o código obrigatório
Uma definição de fluxo de trabalho é uma árvore de objetos de atividade configurados. Essa árvore de atividades pode ser definida de muitas maneiras, incluindo pela edição manual do XAML ou usando o Designer de Fluxo de Trabalho para gerar XAML. O uso de XAML, porém, não é um requisito. As definições de fluxo de trabalho também podem ser criadas programaticamente. Este tópico fornece uma visão geral de como criar definições, atividades e expressões de fluxo de trabalho usando código. Para obter exemplos de como trabalhar com fluxos de trabalho XAML usando código, consulte [serializando fluxos de trabalho e atividades de e para XAML](serializing-workflows-and-activities-to-and-from-xaml.md).  
  
## <a name="creating-workflow-definitions"></a>Criando definições de fluxo de trabalho  
 Uma definição de fluxo de trabalho pode ser criada criando uma instância de um tipo de atividade e configurando as propriedades do objeto da atividade. Para atividades que não contêm atividades filho, isso pode ser feito usando algumas linhas de código.  
  
 [!code-csharp[CFX_WorkflowApplicationExample#47](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#47)]  
  
> [!NOTE]
> Os exemplos deste tópico usam <xref:System.Activities.WorkflowInvoker> para executar fluxos de trabalho de exemplo. Para obter mais informações sobre como invocar fluxos de trabalho, passar argumentos e as diferentes opções de hospedagem disponíveis, consulte [usando WorkflowInvoker e WorkflowApplication](using-workflowinvoker-and-workflowapplication.md).  
  
 Neste exemplo, um fluxo de trabalho que consiste em uma única atividade <xref:System.Activities.Statements.WriteLine> é criado. O argumento <xref:System.Activities.Statements.WriteLine> da atividade <xref:System.Activities.Statements.WriteLine.Text%2A> está definido e o fluxo de trabalho é invocado. Se uma atividade contiver atividades filhos, o método de criação será semelhante. O exemplo a seguir usa uma atividade <xref:System.Activities.Statements.Sequence> que contém duas atividades <xref:System.Activities.Statements.WriteLine>.  
  
 [!code-csharp[CFX_WorkflowApplicationExample#48](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#48)]  
  
### <a name="using-object-initializers"></a>Usando inicializadores de objeto  
 Os exemplos deste tópico usam sintaxe de inicialização de objeto. A sintaxe de inicialização de objeto pode ser uma maneira útil de criar definições de fluxo de trabalho no código porque fornece uma exibição hierárquica das atividades no fluxo de trabalho e mostra a relação entre as atividades. Não há nenhum requisito para usar a sintaxe de inicialização de objeto quando você cria fluxos de trabalho programaticamente. O exemplo a seguir é funcionalmente equivalente ao exemplo anterior.  
  
 [!code-csharp[CFX_WorkflowApplicationExample#49](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#49)]  
  
 Para obter mais informações sobre inicializadores de objeto, consulte [como: inicializar objetos sem chamar um Construtor (guia de programação C#)](../../csharp/programming-guide/classes-and-structs/how-to-initialize-objects-by-using-an-object-initializer.md) e [como declarar um objeto usando um inicializador de objeto](../../visual-basic/programming-guide/language-features/objects-and-classes/how-to-declare-an-object-by-using-an-object-initializer.md).  
  
### <a name="working-with-variables-literal-values-and-expressions"></a>Trabalhando com variáveis, valores literais e expressões  
 Ao criar uma definição de fluxo de trabalho usando código, saiba qual código é executado como parte da criação da definição de fluxo de trabalho e qual código é executado como parte da execução de uma instância desse fluxo de trabalho. Por exemplo, o seguinte fluxo de trabalho é destinado para gerar um número aleatório e gravá-lo no console.  
  
 [!code-csharp[CFX_WorkflowApplicationExample#50](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#50)]  
  
 Quando esse código da definição de fluxo de trabalho é executado, a chamada para `Random.Next` é feita e o resultado é armazenado na definição do fluxo de trabalho como um valor literal. Muitas instâncias desse fluxo de trabalho podem ser chamadas e todos exibiriam o mesmo número. Para que a geração de números aleatórios ocorra durante a execução do fluxo de trabalho, deve ser usada uma expressão que é avaliada toda vez que o fluxo de trabalho é executado. No exemplo a seguir, uma expressão do Visual Basic é usada com um <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601>.  
  
 [!code-csharp[CFX_WorkflowApplicationExample#51](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#51)]  
  
 A expressão no exemplo anterior também poderia ser implementada usando um <xref:Microsoft.CSharp.Activities.CSharpValue%601> e uma expressão C#.  
  
```csharp  
new Assign<int>  
{  
    To = n,  
    Value = new CSharpValue<int>("new Random().Next(1, 101)")  
}  
```  
  
 As expressões C# devem ser compiladas antes que o fluxo de trabalho que as contém seja invocado. Se as expressões C# não forem compiladas, uma <xref:System.NotSupportedException> será lançada quando o fluxo de trabalho for invocado com uma mensagem semelhante à seguinte: ``Expression Activity type 'CSharpValue`1' requires compilation in order to run.  Please ensure that the workflow has been compiled.`` na maioria dos cenários que envolvem fluxos de trabalho criados no Visual Studio, as expressões c# são compiladas automaticamente, mas em alguns cenários, como fluxos de trabalho de código, as expressões c# devem ser compiladas manualmente. Para obter um exemplo de como compilar expressões C#, consulte a seção [usando expressões c# em fluxos de trabalho de código](csharp-expressions.md#CodeWorkflows) do tópico [expressões C#](csharp-expressions.md) .  
  
 Um <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601> representa uma expressão na sintaxe do Visual Basic que pode ser usada como um valor r em uma expressão, e um <xref:Microsoft.CSharp.Activities.CSharpValue%601> representa uma expressão na sintaxe C# que pode ser usada como um valor r de uma expressão. Essas expressões são avaliadas cada vez que a atividade contentora é executada. O resultado da expressão é atribuído à variável de fluxo de trabalho `n` e esses resultados são usados pela próxima atividade no fluxo de trabalho. Para acessar o valor da `n` do fluxo de trabalho em runtime, o <xref:System.Activities.ActivityContext> é necessário. Isso pode ser acessado usando a seguinte expressão lambda.  
  
> [!NOTE]
> Observe que esses dois códigos são exemplos que usam C# como a linguagem de programação, mas um usa um <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601> e o outro usa um <xref:Microsoft.CSharp.Activities.CSharpValue%601>. <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601> e <xref:Microsoft.CSharp.Activities.CSharpValue%601> podem ser usados em projetos do Visual Basic e C#. Por padrão, as expressões criadas no designer de fluxo de trabalho correspondem à linguagem do projeto de hospedagem. Ao criar fluxos de trabalho no código, a linguagem desejada fica a critério do autor do fluxo de trabalho.  
  
 Nestes exemplos, o resultado da expressão é atribuído à variável `n` do fluxo de trabalho, e esses resultados são usados pela atividade a seguir no fluxo de trabalho. Para acessar o valor da `n` do fluxo de trabalho em runtime, o <xref:System.Activities.ActivityContext> é necessário. Isso pode ser acessado usando a seguinte expressão lambda.  
  
 [!code-csharp[CFX_WorkflowApplicationExample#52](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#52)]  
  
 Para obter mais informações sobre expressões lambda, consulte [expressões lambda (referência C#)](../../csharp/language-reference/operators/lambda-expressions.md) ou [expressões lambda (Visual Basic)](../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md).  
  
 As expressões lambda não são serializáveis para o formato XAML. Se uma tentativa para serializar um fluxo de trabalho com expressões lambda for feita, um <xref:System.Activities.Expressions.LambdaSerializationException> será gerado com a seguinte mensagem: “Esse fluxo de trabalho contém as expressões lambda especificadas no código. Essas expressões não são XAML serializável. Para tornar serializável a XAML de seu fluxo de trabalho, use VisualBasicValue/VisualBasicReference ou ExpressionServices.Convert(lambda). Isso fará as expressões lambda serem convertidas em atividades de expressão." Para tornar essa expressão compatível com XAML, use <xref:System.Activities.Expressions.ExpressionServices> e <xref:System.Activities.Expressions.ExpressionServices.Convert%2A>, conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[CFX_WorkflowApplicationExample#53](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#53)]  
  
 Um <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601> também poderia ser usado. Observe que nenhuma expressão lambda é necessária ao usar uma expressão do Visual Basic.  
  
 [!code-csharp[CFX_WorkflowApplicationExample#54](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#54)]  
  
 Em tempo de execução, as expressões do Visual Basic são compiladas em expressões LINQ. Os dois exemplos anteriores são serializáveis para XAML, mas se o XAML serializado for destinado para ser exibido e editado no designer de fluxo de trabalho, use a <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601> para suas expressões. Os fluxos de trabalho serializados que usam `ExpressionServices.Convert` podem ser abertos no designer, mas o valor da expressão ficará em branco. Para obter mais informações sobre como serializar fluxos de trabalho para XAML, consulte [serializando fluxos de trabalho e atividades de e para XAML](serializing-workflows-and-activities-to-and-from-xaml.md).  
  
#### <a name="literal-expressions-and-reference-types"></a>Expressões literais e tipos de referência  
 As expressões literais são representadas nos fluxos de trabalho pela atividade <xref:System.Activities.Expressions.Literal%601>. As seguintes atividades <xref:System.Activities.Statements.WriteLine> são funcionalmente equivalentes.  
  
```csharp  
new WriteLine  
{  
    Text = "Hello World."  
},  
new WriteLine  
{  
    Text = new Literal<string>("Hello World.")  
}  
```  
  
 Não é válido inicializar uma expressão literal com qualquer tipo de referência exceto <xref:System.String>. No exemplo a seguir, a propriedade <xref:System.Activities.Statements.Assign> de uma atividade <xref:System.Activities.Statements.Assign.Value%2A> é inicializada com uma expressão literal usando `List<string>`.  
  
```csharp  
new Assign  
{  
    To = new OutArgument<List<string>>(items),  
    Value = new InArgument<List<string>>(new List<string>())  
},  
```  
  
 Quando o fluxo de trabalho que contém esta atividade é validado, o seguinte erro de validação será retornado: "O literal somente oferece suporte aos tipos de valor e ao tipo imutável System.String. O tipo System.Collections.Generic.List`1[System.String] não pode ser usado como um literal." Se o fluxo de trabalho for chamado, uma <xref:System.Activities.InvalidWorkflowException> será gerada que contém o texto do erro de validação. Este é um erro de validação porque criar uma expressão literal com um tipo de referência não cria uma nova instância do tipo de referência para cada instância do fluxo de trabalho. Para resolver isso, substitua a expressão literal por uma que cria e retorna uma nova instância do tipo de referência.  
  
```csharp  
new Assign  
{  
    To = new OutArgument<List<string>>(items),  
    Value = new InArgument<List<string>>(new VisualBasicValue<List<string>>("New List(Of String)"))  
},  
```  
  
 Para obter mais informações sobre expressões, consulte [expressões](expressions.md).  
  
#### <a name="invoking-methods-on-objects-using-expressions-and-the-invokemethod-activity"></a>Invocando métodos em objetos usando expressões e a atividade InvokeMethod  
 A atividade <xref:System.Activities.Expressions.InvokeMethod%601> pode ser usada para invocar métodos estáticos e de instância de classes no .NET Framework. Em um exemplo anterior neste tópico, um número aleatório foi gerado usando a classe <xref:System.Random>.  
  
 [!code-csharp[CFX_WorkflowApplicationExample#51](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#51)]  
  
 A atividade <xref:System.Activities.Expressions.InvokeMethod%601> também poderia ter sido usada para chamar o método <xref:System.Random.Next%2A> da classe <xref:System.Random>.  
  
```csharp  
new InvokeMethod<int>  
{  
    TargetObject = new InArgument<Random>(new VisualBasicValue<Random>("New Random()")),  
    MethodName = "Next",  
    Parameters =
    {  
        new InArgument<int>(1),  
        new InArgument<int>(101)  
    },  
    Result = n  
}  
```  
  
 Como <xref:System.Random.Next%2A> não é um método estático, uma instância da classe <xref:System.Random> é fornecida para a propriedade <xref:System.Activities.Expressions.InvokeMethod%601.TargetObject%2A>. Neste exemplo, uma nova instância é criada usando uma expressão do Visual Basic, mas também poderia ter sido criada anteriormente e armazenada em uma variável do fluxo de trabalho. Neste exemplo, seria mais simples usar a atividade <xref:System.Activities.Statements.Assign%601> em vez da atividade <xref:System.Activities.Expressions.InvokeMethod%601>. Se a chamada do método chamado finalmente pelas atividades <xref:System.Activities.Statements.Assign%601> ou <xref:System.Activities.Expressions.InvokeMethod%601> for longa, o <xref:System.Activities.Expressions.InvokeMethod%601> terá uma vantagem porque tem uma propriedade <xref:System.Activities.Expressions.InvokeMethod%601.RunAsynchronously%2A>. Quando esta propriedade for definida como `true`, o método chamado será executado de forma assíncrona em relação ao fluxo de trabalho. Se outras atividades estiverem em paralelo, elas não serão bloqueadas enquanto o método estiver em execução de forma assíncrona. Além disso, se o método a ser chamado não tiver nenhum valor de retorno, então <xref:System.Activities.Expressions.InvokeMethod%601> será o modo apropriado de chamar o método.  
  
## <a name="arguments-and-dynamic-activities"></a>Argumentos e atividades dinâmicas  
 Uma definição de fluxo de trabalho é criada no código montando atividades em uma árvore de atividade e configurando as propriedades e os argumentos. Os argumentos existentes podem ser associados, mas novos argumentos não podem ser adicionados às atividades. Isso inclui os argumentos de fluxo de trabalho passados para a atividade raiz. No código obrigatório, os argumentos de fluxo de trabalho são especificados como propriedades em um novo tipo CLR, e no XAML são declarados usando `x:Class` e `x:Member`. Como não há um novo tipo CLR criado quando uma definição de fluxo de trabalho é criada como uma árvore de objetos de memória, os argumentos não poderão ser adicionados. No entanto, os argumentos podem ser adicionados a um <xref:System.Activities.DynamicActivity>. Neste exemplo, um <xref:System.Activities.DynamicActivity%601> é criado e usa dois argumentos inteiros, adiciona-os juntos e retorna o resultado. Um <xref:System.Activities.DynamicActivityProperty> é criado para cada argumento e o resultado da operação é atribuído ao argumento <xref:System.Activities.Activity%601.Result%2A> do <xref:System.Activities.DynamicActivity%601>.  
  
 [!code-csharp[CFX_WorkflowApplicationExample#55](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#55)]  
  
 Para obter mais informações sobre atividades dinâmicas, consulte [criando uma atividade em tempo de execução](creating-an-activity-at-runtime-with-dynamicactivity.md).  
  
## <a name="compiled-activities"></a>Atividades compiladas  
 As atividades dinâmicas são uma forma de definir uma atividade que contém argumentos usando o código, mas as atividades também podem ser criadas em código e compiladas em tipos. As atividades simples podem ser criadas que derivam de <xref:System.Activities.CodeActivity> e as atividades assíncronas que derivam de <xref:System.Activities.AsyncCodeActivity>. Essas atividades podem ter argumentos, valores de retorno e definir sua lógica usando o código obrigatório. Para obter exemplos de como criar esses tipos de atividades, consulte [classe base CodeActivity](workflow-activity-authoring-using-the-codeactivity-class.md) e [criando atividades assíncronas](creating-asynchronous-activities-in-wf.md).  
  
 As atividades que derivam de <xref:System.Activities.NativeActivity> podem definir sua lógica usando o código obrigatório e elas também podem conter as atividades filho que definem a lógica. Elas também têm acesso completo aos recursos de runtime como criar indicadores. Para obter exemplos de como criar uma <xref:System.Activities.NativeActivity> atividade baseada em uma, consulte [classe base NativeActivity](nativeactivity-base-class.md), [como criar uma atividade](how-to-create-an-activity.md)e a [composição personalizada usando](./samples/custom-composite-using-native-activity.md) a amostra de atividade nativa.  
  
 As atividades que derivam do <xref:System.Activities.Activity> definem sua lógica somente por meio do uso de atividades filho. Essas atividades normalmente são criadas usando o designer de fluxo de trabalho, mas também podem ser definidas usando código. No exemplo a seguir, uma atividade `Square` é definida que deriva de `Activity<int>`. A atividade `Square` tem único <xref:System.Activities.InArgument%601> chamado `Value` e define sua lógica especificando uma atividade <xref:System.Activities.Statements.Sequence> usando a propriedade <xref:System.Activities.Activity.Implementation%2A>. A atividade <xref:System.Activities.Statements.Sequence> contém uma atividade <xref:System.Activities.Statements.WriteLine> e uma atividade <xref:System.Activities.Statements.Assign%601>. Juntas, essas três atividades implementam a lógica da atividade `Square`.  
  
```csharp  
class Square : Activity<int>  
{  
    [RequiredArgument]  
    public InArgument<int> Value { get; set; }  
  
    public Square()  
    {  
        this.Implementation = () => new Sequence  
        {  
            Activities =  
            {  
                new WriteLine  
                {  
                    Text = new InArgument<string>((env) => "Squaring the value: " + this.Value.Get(env))  
                },  
                new Assign<int>  
                {  
                    To = new OutArgument<int>((env) => this.Result.Get(env)),  
                    Value = new InArgument<int>((env) => this.Value.Get(env) * this.Value.Get(env))  
                }  
            }  
        };  
    }  
}  
```  
  
 No exemplo a seguir, uma definição de fluxo de trabalho que consiste em uma única atividade `Square` é invocada usando <xref:System.Activities.WorkflowInvoker>.  
  
```csharp  
Dictionary<string, object> inputs = new Dictionary<string, object> {{ "Value", 5}};  
int result = WorkflowInvoker.Invoke(new Square(), inputs);  
Console.WriteLine("Result: {0}", result);  
```  
  
 Quando o fluxo de trabalho é chamado, a seguinte saída é exibida no console:  
  
 **Quadrado do valor: 5**  
**Resultado: 25**
