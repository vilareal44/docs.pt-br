---
title: Variáveis e argumentos
description: Este artigo descreve variáveis, que representam o armazenamento de dados e argumentos, que representam o fluxo de dados de/para uma atividade no Workflow Foundation.
ms.date: 03/30/2017
ms.assetid: d03dbe34-5b2e-4f21-8b57-693ee49611b8
ms.openlocfilehash: 5cce9931e9b0a37d5fafbfb84527ffd543a0a50f
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84201951"
---
# <a name="variables-and-arguments"></a>Variáveis e argumentos
No Windows Workflow Foundation (WF), as variáveis representam o armazenamento de dados e os argumentos representam o fluxo de dados para dentro e fora de uma atividade. Uma atividade tem um conjunto de argumentos e compõem a assinatura de atividade. Além disso, uma atividade pode manter uma lista de variáveis para que um desenvolvedor pode adicionar ou remover variáveis durante o design de um fluxo de trabalho. Um argumento é associado usando uma expressão que retorna um valor.  
  
## <a name="variables"></a>Variáveis  
 As variáveis são local de armazenamento de dados. As variáveis são declaradas como parte da definição de um fluxo de trabalho. Variáveis têm em valores em runtime e esses valores são armazenados como parte do estado de uma instância de fluxo de trabalho. Uma definição variável especifica o tipo de variável e opcionalmente, o nome. O código a seguir mostra como declarar uma variável, atribui um valor que usa uma atividade de <xref:System.Activities.Statements.Assign%601> , e o exibe em seu valor para o console usando uma atividade de <xref:System.Activities.Statements.WriteLine> .  
  
```csharp  
// Define a variable named "str" of type string.  
Variable<string> var = new Variable<string>  
{  
    Name = "str"  
};  
  
// Declare the variable within a Sequence, assign  
// a value to it, and then display it.  
Activity wf = new Sequence()  
{  
    Variables = { var },  
    Activities =  
    {  
        new Assign<string>  
        {  
            To = var,  
            Value = "Hello World."  
        },  
        new WriteLine  
        {  
            Text = var  
        }  
    }  
};  
  
WorkflowInvoker.Invoke(wf);  
```  
  
 Uma expressão de valor padrão opcionalmente pode ser especificada como parte de uma declaração de variável. Variáveis também podem ter modificadores. Por exemplo, se um variável é somente leitura no modificador somente leitura de <xref:System.Activities.VariableModifiers> pode ser aplicado. No exemplo a seguir, uma variável somente leitura é criado que tem um valor padrão atribuído.  
  
```csharp  
// Define a read-only variable with a default value.  
Variable<string> var = new Variable<string>  
{  
    Default = "Hello World.",  
    Modifiers = VariableModifiers.ReadOnly  
};  
```  
  
## <a name="variable-scoping"></a>Escopo variável  
 O tempo de vida de uma variável em runtime é igual ao tempo de vida de atividade que a declara. Quando uma atividade concluir, suas variáveis são limpados e não podem mais ser referenciados.  
  
## <a name="arguments"></a>Argumentos  
 Os autores de atividade usam argumentos para definir os fluxos de dados de maneira e fora de uma atividade. Cada argumento possui uma direção especificada: <xref:System.Activities.ArgumentDirection.In>, <xref:System.Activities.ArgumentDirection.Out>, ou <xref:System.Activities.ArgumentDirection.InOut>.  
  
 O runtime de fluxo de trabalho faz as seguintes garantias sobre o controle de tempo de movimentação de dados de e para atividades:  
  
1. Quando uma atividade se inicia, os valores dos argumentos de entrada e de arquivos entrada/saída são calculados. Por exemplo, independentemente de <xref:System.Activities.Argument.Get%2A> quando é chamado, o valor retornado é calculado no runtime antes da chamada de `Execute`.  
  
2. Quando <xref:System.Activities.InOutArgument%601.Set%2A> é chamado, o runtime define o valor imediatamente.  
  
3. Os argumentos podem opcionalmente ter seu <xref:System.Activities.Argument.EvaluationOrder%2A> especificado. <xref:System.Activities.Argument.EvaluationOrder%2A> é um valor com base zero que especifica a ordem em que o argumento é avaliado. Por padrão, a ordem de classificação de argumento é especificado e não é igual ao valor de <xref:System.Activities.Argument.UnspecifiedEvaluationOrder> . Definir <xref:System.Activities.Argument.EvaluationOrder%2A> para um valor maior ou igual a zero para especificar uma ordem de classificação para esse argumento. Windows Workflow Foundation avalia argumentos com uma ordem de avaliação especificada em ordem crescente. Observe que os argumentos com uma ordem não-especificada de avaliação são avaliados antes que esses especificado com uma ordem de classificação.  
  
 Um autor de atividade pode usar um mecanismo fortemente tipado para expor seus argumentos. Isso é feito declarando propriedades do tipo <xref:System.Activities.InArgument%601>, <xref:System.Activities.OutArgument%601>, e <xref:System.Activities.InOutArgument%601>. Isso permite que um autor de atividade estabeleça um contrato específico sobre os dados que vão e fora de uma atividade.  
  
### <a name="defining-the-arguments-on-an-activity"></a>Definindo os argumentos em uma atividade  
 Os argumentos podem ser definidos em uma atividade especificando propriedades do tipo <xref:System.Activities.InArgument%601>, <xref:System.Activities.OutArgument%601>, e <xref:System.Activities.InOutArgument%601>. O código a seguir mostra como definir os argumentos para uma atividade de `Prompt` que reduz uma cadeia de caracteres para exibir ao usuário e retorna uma cadeia de caracteres que contém a resposta do usuário.  
  
```csharp  
public class Prompt : Activity  
{  
    public InArgument<string> Text { get; set; }  
    public OutArgument<string> Response { get; set; }  
    // Rest of activity definition omitted.  
}  
```  
  
> [!NOTE]
> As atividades que retornam um valor único podem derivar de <xref:System.Activities.Activity%601>, de <xref:System.Activities.NativeActivity%601>, ou de <xref:System.Activities.CodeActivity%601>. Essas atividades têm <xref:System.Activities.OutArgument%601> bem definido chamado <xref:System.Activities.Activity%601.Result%2A> que contém o valor de retorno da atividade.  
  
### <a name="using-variables-and-arguments-in-workflows"></a>Usando variáveis e argumentos em fluxos de trabalho  
 O exemplo a seguir mostra como variáveis e os argumentos são usados em um fluxo de trabalho. O fluxo de trabalho é uma sequência que declara três variáveis: `var1`, `var2`, e `var3`. A primeira atividade no fluxo de trabalho é uma atividade de `Assign` que atribui o valor da variável `var1``var2`variável. Isso é seguido por uma atividade de `WriteLine` que imprimir o valor da variável de `var2` . Em seguida é outra atividade de `Assign` que atribui o valor da variável `var2``var3`variável. Finalmente há outra atividade de `WriteLine` que imprimir o valor da variável de `var3` . A primeira atividade de `Assign` usa `InArgument<string>` e `OutArgument<string>` objetos que representa explicitamente as associações para os argumentos de atividade. `InArgument<string>` é usado para <xref:System.Activities.Statements.Assign.Value%2A> porque o valor é fluxo na atividade de <xref:System.Activities.Statements.Assign%601> através de seu argumento de <xref:System.Activities.Statements.Assign.Value%2A> , e `OutArgument<string>` é usado para <xref:System.Activities.Statements.Assign.To%2A> porque o valor de fluxo está fora do argumento de <xref:System.Activities.Statements.Assign.To%2A> na variável. A segunda atividade de `Assign` realiza a mesma coisa com mais sintaxe compacta mas equivalente que usa conversões implícitas. As atividades de `WriteLine` também usam a sintaxe compacta.  
  
```csharp  
// Declare three variables; the first one is given an initial value.  
Variable<string> var1 = new Variable<string>()  
{  
    Default = "one"  
};  
Variable<string> var2 = new Variable<string>();  
Variable<string> var3 = new Variable<string>();  
  
// Define the workflow  
Activity wf = new Sequence  
{  
    Variables = { var1, var2, var3 },  
    Activities =
    {  
        new Assign<string>()  
        {  
            Value = new InArgument<string>(var1),  
            To = new OutArgument<string>(var2)  
        },  
        new WriteLine() { Text = var2 },  
        new Assign<string>()  
        {  
            Value = var2,  
            To = var3  
        },  
        new WriteLine() { Text = var3 }  
    }  
};  
  
WorkflowInvoker.Invoke(wf);  
```  
  
### <a name="using-variables-and-arguments-in-code-based-activities"></a>Usando variáveis e argumentos código com base em atividades  
 Os exemplos anteriores mostram como usar argumentos e variáveis em fluxos de trabalho e em atividades declarativas. Os argumentos e variáveis também são usados em atividades código com base. Conceitualmente o uso é muito semelhante. Variáveis representam o armazenamento de dados dentro da atividade, e os argumentos representam o fluxo de dados ou fora da atividade, e são associados pelo autor de fluxo de trabalho a outras variáveis ou argumentos no fluxo de trabalho que representam de onde os fluxos de dados ou a. Para obter ou definir o valor de uma variável ou um argumento em uma atividade, um contexto de atividade deve ser usado que representa o ambiente de execução atual da atividade. Isso é passado para o método de <xref:System.Activities.CodeActivity%601.Execute%2A> de atividade em runtime de fluxo de trabalho. Nesse exemplo, uma atividade de `Add` personalizado é definida que possui dois argumentos de <xref:System.Activities.ArgumentDirection.In> . Para acessar o valor de argumentos, o método de <xref:System.Activities.Argument.Get%2A> é usado e o contexto que foi passado em runtime de fluxo de trabalho é usado.  
  
```csharp  
public sealed class Add : CodeActivity<int>  
{  
    [RequiredArgument]  
    public InArgument<int> Operand1 { get; set; }  
  
    [RequiredArgument]  
    public InArgument<int> Operand2 { get; set; }  
  
    protected override int Execute(CodeActivityContext context)  
    {  
        return Operand1.Get(context) + Operand2.Get(context);  
    }  
}  
```  
  
 Para obter mais informações sobre como trabalhar com argumentos, variáveis e expressões no código, consulte criação de fluxos de trabalho [, atividades e expressões usando código imperativo](authoring-workflows-activities-and-expressions-using-imperative-code.md) e [argumentos necessários e grupos de sobrecarga](required-arguments-and-overload-groups.md).
