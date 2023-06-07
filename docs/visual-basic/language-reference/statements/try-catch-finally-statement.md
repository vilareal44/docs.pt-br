---
title: Tentar... Capturar... Instrução Finally
description: Aprenda a usar a manipulação de exceção com instruções Visual Basic try/catch/finally.
ms.date: 12/07/2018
f1_keywords:
- vb.Try...Catch...Finally
- vb.when
- vb.Finally
- vb.Catch
- vb.Try
helpviewer_keywords:
- Try...Catch...Finally statements
- Try statement [Visual Basic]
- try-catch exception handling, Try...Catch...Finally statements
- error handling, while running code
- Try statement [Visual Basic], Try...Catch...Finally
- Finally keyword [Visual Basic], Try...Catch...Finally
- Catch statement [Visual Basic]
- When keyword [Visual Basic]
- Visual Basic code, handling errors while running
- structured exception handling, Try...Catch...Finally statements
ms.assetid: d6488026-ccb3-42b8-a810-0d97b9d6472b
ms.openlocfilehash: 22f1611786a3da512632b5b547b7ef141c8f65c6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84391762"
---
# <a name="trycatchfinally-statement-visual-basic"></a>Instrução Try...Catch...Finally (Visual Basic)

Fornece uma maneira de lidar com alguns ou todos os erros possíveis que podem ocorrer em um determinado bloco de código, enquanto ainda executa o código.

## <a name="syntax"></a>Sintaxe

```vb
Try
    [ tryStatements ]
    [ Exit Try ]
[ Catch [ exception [ As type ] ] [ When expression ]
    [ catchStatements ]
    [ Exit Try ] ]
[ Catch ... ]
[ Finally
    [ finallyStatements ] ]
End Try
```

## <a name="parts"></a>Partes

|Termo|Definição|
|---|---|
|`tryStatements`|Opcional. Instrução (ões) em que um erro pode ocorrer. Pode ser uma instrução composta.|
|`Catch`|Opcional. Vários `Catch` blocos permitidos. Se ocorrer uma exceção durante o processamento do `Try` bloco, cada `Catch` instrução será examinada em ordem textual para determinar se ele trata a exceção, com `exception` a representação da exceção gerada.|
|`exception`|Opcional. Qualquer nome de variável. O valor inicial de `exception` é o valor do erro gerado. Usado com `Catch` para especificar o erro capturado. Se for omitido, a `Catch` instrução capturará qualquer exceção.|
|`type`|Opcional. Especifica o tipo de filtro de classe. Se o valor de `exception` for do tipo especificado por `type` ou de um tipo derivado, o identificador se tornará associado ao objeto de exceção.|
|`When`|Opcional. Uma `Catch` instrução com uma `When` cláusula captura exceções somente quando `expression` é avaliada como `True` . Uma `When` cláusula é aplicada somente após a verificação do tipo da exceção e `expression` pode se referir ao identificador que representa a exceção.|
|`expression`|Opcional. Deve ser implicitamente conversível em `Boolean` . Qualquer expressão que descreva um filtro genérico. Normalmente usado para filtrar por número de erro. Usado com `When` a palavra-chave para especificar circunstâncias sob as quais o erro é capturado.|
|`catchStatements`|Opcional. Instruções para tratar erros que ocorrem no `Try` bloco associado. Pode ser uma instrução composta.|
|`Exit Try`|Opcional. Palavra-chave que divide a `Try...Catch...Finally` estrutura. A execução é retomada com o código imediatamente após a `End Try` instrução. A `Finally` instrução ainda será executada. Não permitido em `Finally` blocos.|
|`Finally`|Opcional. Um `Finally` bloco é sempre executado quando a execução sai de qualquer parte da `Try...Catch` instrução.|
|`finallyStatements`|Opcional. As instruções que são executadas após o processamento de todo o outro erro ocorreram.|
|`End Try`|Encerra a `Try...Catch...Finally` estrutura.|

## <a name="remarks"></a>Comentários

Se você espera que uma determinada exceção ocorra durante uma determinada seção do código, coloque o código em um `Try` bloco e use um `Catch` bloco para manter o controle e manipular a exceção se ela ocorrer.

Uma `Try…Catch` instrução consiste em um `Try` bloco seguido por uma ou mais `Catch` cláusulas, que especificam manipuladores para várias exceções. Quando uma exceção é lançada em um `Try` bloco, Visual Basic procura a `Catch` instrução que manipula a exceção. Se uma `Catch` instrução correspondente não for encontrada, Visual Basic examinará o método que chamou o método atual e assim por diante na pilha de chamadas. Se nenhum `Catch` bloco for encontrado, Visual Basic exibirá uma mensagem de exceção sem tratamento para o usuário e interromperá a execução do programa.

Você pode usar mais de uma `Catch` instrução em uma `Try…Catch` instrução. Se você fizer isso, a ordem das `Catch` cláusulas será significativa, pois elas serão examinadas em ordem. Capture as exceções mais específicas antes das menos específicas.

As condições de instrução a seguir `Catch` são as menos específicas e capturarão todas as exceções que derivam da <xref:System.Exception> classe. Você normalmente deve usar uma dessas variações como o último `Catch` bloco na `Try...Catch...Finally` estrutura, depois de capturar todas as exceções específicas que você espera. O fluxo de controle nunca pode alcançar um `Catch` bloco que siga qualquer uma dessas variações.

- O `type` é `Exception` , por exemplo:`Catch ex As Exception`

- A instrução não tem nenhuma `exception` variável, por exemplo:`Catch`

Quando uma `Try…Catch…Finally` instrução é aninhada em outro `Try` bloco, Visual Basic primeiro examina cada `Catch` instrução no bloco interno `Try` . Se nenhuma `Catch` instrução correspondente for encontrada, a pesquisa prosseguirá para as `Catch` instruções do bloco externo `Try…Catch…Finally` .

As variáveis locais de um `Try` bloco não estão disponíveis em um `Catch` bloco porque são blocos separados. Se você quiser usar uma variável em mais de um bloco, declare a variável fora da `Try...Catch...Finally` estrutura.

> [!TIP]
> A `Try…Catch…Finally` instrução está disponível como um trecho de código IntelliSense. No Gerenciador de trechos de código, expanda **padrões de código-se, para cada um, Tente catch, propriedade, etc**. e, em seguida, **tratamento de erro (exceções)**. Para obter mais informações, consulte [Snippets de Código](/visualstudio/ide/code-snippets).

## <a name="finally-block"></a>Bloco finally

Se você tiver uma ou mais instruções que devem ser executadas antes de sair da `Try` estrutura, use um `Finally` bloco. O controle passa para o `Finally` bloco logo antes de passar da `Try…Catch` estrutura. Isso é verdadeiro mesmo se ocorrer uma exceção em qualquer lugar dentro da `Try` estrutura.

Um `Finally` bloco é útil para executar qualquer código que deve ser executado mesmo se houver uma exceção. O controle é passado para o `Finally` bloco, independentemente de como o `Try...Catch` bloco é encerrado.

O código em um `Finally` bloco é executado mesmo que seu código encontre uma `Return` instrução em `Try` um `Catch` bloco ou. O controle não passa de um `Try` `Catch` bloco ou para o `Finally` bloco correspondente nos seguintes casos:

- Uma [instrução End](end-statement.md) é encontrada no `Try` bloco ou `Catch` .

- Um <xref:System.StackOverflowException> é gerado no `Try` bloco ou `Catch` .

Não é válido transferir explicitamente a execução para um `Finally` bloco. A transferência da execução para fora de um `Finally` bloco não é válida, exceto por meio de uma exceção.

Se uma `Try` instrução não contiver pelo menos um `Catch` bloco, ela deverá conter um `Finally` bloco.

> [!TIP]
> Se você não precisar capturar exceções específicas, a `Using` instrução se comportará como um `Try…Finally` bloco e garantirá a alienação dos recursos, independentemente de como você sai do bloco. Isso é verdadeiro mesmo com uma exceção sem tratamento. Para obter mais informações, consulte [usando a instrução](using-statement.md).

## <a name="exception-argument"></a>Argumento de exceção

O `Catch` `exception` argumento Block é uma instância da <xref:System.Exception> classe ou uma classe derivada da `Exception` classe. A `Exception` instância de classe corresponde ao erro que ocorreu no `Try` bloco.

As propriedades do `Exception` objeto ajudam a identificar a causa e o local de uma exceção. Por exemplo, a <xref:System.Exception.StackTrace%2A> propriedade lista os métodos chamados que levaram à exceção, ajudando você a descobrir onde o erro ocorreu no código. <xref:System.Exception.Message%2A>Retorna uma mensagem que descreve a exceção. <xref:System.Exception.HelpLink%2A>Retorna um link para um arquivo de ajuda associado. <xref:System.Exception.InnerException%2A>Retorna o `Exception` objeto que causou a exceção atual ou retorna se não `Nothing` houver nenhum original `Exception` .

## <a name="considerations-when-using-a-trycatch-statement"></a>Considerações ao usar uma tentativa... Instrução Catch

Use uma `Try…Catch` instrução somente para sinalizar a ocorrência de eventos de programa incomuns ou inesperados. Os motivos para isso incluem o seguinte:

- A captura de exceções em tempo de execução cria sobrecarga adicional e provavelmente será mais lenta do que a verificação prévia para evitar exceções.

- Se um `Catch` bloco não for manipulado corretamente, a exceção poderá não ser relatada corretamente aos usuários.

- A manipulação de exceções torna um programa mais complexo.

Você nem sempre precisa de uma `Try…Catch` instrução para verificar se há uma condição que provavelmente ocorrerá. O exemplo a seguir verifica se um arquivo existe antes de tentar abri-lo. Isso reduz a necessidade de capturar uma exceção gerada pelo <xref:System.IO.File.OpenText%2A> método.

[!code-vb[VbVbalrStatements#94](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#94)]

Certifique-se de que o código em `Catch` blocos possa relatar corretamente exceções aos usuários, seja por meio de log seguro de threads ou mensagens apropriadas. Caso contrário, as exceções podem permanecer desconhecidas.

## <a name="async-methods"></a>Métodos assíncronos

Se você marcar um método com o modificador [Async](../modifiers/async.md) , poderá usar o operador [Await](../operators/await-operator.md) no método. Uma instrução com o `Await` operador suspende a execução do método até que a tarefa esperada seja concluída. A tarefa representa um trabalho em andamento. Quando a tarefa associada ao `Await` operador for concluída, a execução será retomada no mesmo método. Para obter mais informações, consulte [fluxo de controle em programas assíncronos](../../programming-guide/concepts/async/control-flow-in-async-programs.md).

Uma tarefa retornada por um método assíncrono pode terminar em um estado com falha, indicando que ele foi concluído devido a uma exceção sem tratamento. Uma tarefa também pode terminar em um estado cancelado, o que resulta na `OperationCanceledException` geração da expressão Await. Para capturar qualquer tipo de exceção, coloque a `Await` expressão associada à tarefa em um `Try` bloco e Capture a exceção no `Catch` bloco. Um exemplo é fornecido posteriormente neste tópico.

Uma tarefa pode estar em um estado com falha porque várias exceções foram responsáveis por sua falha. Por exemplo, a tarefa pode ser o resultado de uma chamada para <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>. Quando você aguarda tal tarefa, a exceção capturada é apenas uma das exceções, e você não pode prever qual exceção será detectada. Um exemplo é fornecido posteriormente neste tópico.

Uma `Await` expressão não pode estar dentro de um `Catch` bloco ou `Finally` bloco.

## <a name="iterators"></a>Iterators

Uma função ou `Get` acessador de iterador executa uma iteração personalizada em uma coleção. Um iterador usa uma instrução [yield](yield-statement.md) para retornar cada elemento da coleção um de cada vez. Você chama uma função de iterador usando um [para cada... Próxima instrução](for-each-next-statement.md).

Uma `Yield` instrução pode estar dentro de um `Try` bloco. Um `Try` bloco que contém uma `Yield` instrução pode ter `Catch` blocos e pode ter um `Finally` bloco. Consulte a seção "blocos try em Visual Basic" de [iteradores](../../programming-guide/concepts/iterators.md) para obter um exemplo.

Uma `Yield` instrução não pode estar dentro de um `Catch` bloco ou `Finally` bloco.

Se o `For Each` corpo (fora da função de iterador) gerar uma exceção, um `Catch` bloco na função de iterador não será executado, mas um `Finally` bloco na função de iterador será executado. Um `Catch` bloco dentro de uma função de iterador captura apenas as exceções que ocorrem dentro da função de iterador.

## <a name="partial-trust-situations"></a>Situações de confiança parcial

Em situações de confiança parcial, como um aplicativo hospedado em um compartilhamento de rede, `Try...Catch...Finally` o não captura exceções de segurança que ocorrem antes que o método que contém a chamada seja invocado. O exemplo a seguir, quando você o coloca em um compartilhamento de servidor e executado a partir daí, produz o erro "System. Security. SecurityException: falha na solicitação." Para obter mais informações sobre exceções de segurança, consulte a <xref:System.Security.SecurityException> classe.

[!code-vb[VbVbalrStatements#85](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#85)]

Nessa situação de confiança parcial, você precisa colocar a `Process.Start` instrução em uma separada `Sub` . A chamada inicial para o `Sub` falhará. Isso permite que o `Try...Catch` o acompanhe antes `Sub` que o que contém `Process.Start` seja iniciado e a exceção de segurança produzida.

## <a name="examples"></a>Exemplos

### <a name="the-structure-of-trycatchfinally"></a>A estrutura de try... Capturar... Disso

O exemplo a seguir ilustra a estrutura da `Try...Catch...Finally` instrução.

[!code-vb[VbVbalrStatements#86](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#86)]  

### <a name="exception-in-a-method-called-from-a-try-block"></a>Exceção em um método chamado a partir de um bloco try

No exemplo a seguir, o `CreateException` método gera um `NullReferenceException` . O código que gera a exceção não está em um `Try` bloco. Portanto, o `CreateException` método não trata a exceção. O `RunSample` método manipula a exceção porque a chamada para o `CreateException` método está em um `Try` bloco.

O exemplo inclui `Catch` instruções para vários tipos de exceções, ordenadas da mais específica para a mais geral.

[!code-vb[VbVbalrStatements#91](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#91)]

### <a name="the-catch-when-statement"></a>A instrução Catch When

O exemplo a seguir mostra como usar uma `Catch When` instrução para filtrar em uma expressão condicional. Se a expressão condicional for avaliada como `True` , o código no `Catch` bloco será executado.

[!code-vb[VbVbalrStatements#92](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#92)]

### <a name="nested-try-statements"></a>Instruções Try aninhadas

O exemplo a seguir tem uma `Try…Catch` instrução contida em um `Try` bloco. O `Catch` bloco interno gera uma exceção que tem sua `InnerException` propriedade definida como a exceção original. O `Catch` bloco externo relata sua própria exceção e a exceção interna.

[!code-vb[VbVbalrStatements#93](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#93)]

### <a name="exception-handling-for-async-methods"></a>Tratamento de exceção para métodos assíncronos

O exemplo a seguir ilustra o tratamento de exceção para métodos assíncronos. Para capturar uma exceção que se aplica a uma tarefa assíncrona, a `Await` expressão está em um `Try` bloco do chamador e a exceção é capturada no `Catch` bloco.

Remova a marca de comentário da linha `Throw New Exception` no exemplo para demonstrar o tratamento de exceção. A exceção é capturada no `Catch` bloco, a propriedade da tarefa `IsFaulted` é definida como `True` e a propriedade da tarefa `Exception.InnerException` é definida como a exceção.

Remova a marca de comentário da linha `Throw New OperationCancelledException` para demonstrar o que acontece quando você cancela um processo assíncrono. A exceção é capturada no `Catch` bloco e a propriedade da tarefa `IsCanceled` é definida como `True` . No entanto, em algumas condições que não se aplicam a este exemplo, `IsFaulted` é definido como `True` e `IsCanceled` é definido como `False` .

[!code-vb[csAsyncExceptions#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csasyncexceptions/vb/class1.vb#1)]

### <a name="handling-multiple-exceptions-in-async-methods"></a>Manipulando várias exceções em métodos assíncronos

O exemplo a seguir ilustra a manipulação de exceção em que várias tarefas podem resultar em várias exceções. O `Try` bloco tem a `Await` expressão para a tarefa <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> retornada. A tarefa é concluída quando as três tarefas às quais <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> é aplicada são concluídas.

Cada uma das três tarefas causa uma exceção. O `Catch` bloco itera pelas exceções, que são encontradas na `Exception.InnerExceptions` propriedade da tarefa `Task.WhenAll` retornada.

[!code-vb[csAsyncExceptions#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csasyncexceptions/vb/class1.vb#3)]

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Information.Err%2A>
- <xref:System.Exception>
- [Instrução Exit](exit-statement.md)
- [Instrução On Error](on-error-statement.md)
- [Melhores práticas para usar snippets de código](/visualstudio/ide/best-practices-for-using-code-snippets)
- [Tratamento de exceção](../../../standard/parallel-programming/exception-handling-task-parallel-library.md)
- [Instrução Throw](throw-statement.md)
