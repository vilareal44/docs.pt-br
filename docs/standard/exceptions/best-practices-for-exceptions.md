---
title: Melhores práticas para exceções – .NET
description: Conheça as práticas recomendadas para exceções, como usar try/catch/finally, manipular condições comuns sem exceções e usar tipos de exceção .NET predefinidos.
ms.date: 12/05/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- exceptions, best practices
ms.assetid: f06da765-235b-427a-bfb6-47cd219af539
ms.openlocfilehash: 90dda00acd32852b032fc383580c5f34022ec9b4
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84447089"
---
# <a name="best-practices-for-exceptions"></a>Práticas recomendadas para exceções

Um aplicativo bem projetado sabe tratar erros e exceções para evitar falhas. Esta seção descreve as práticas recomendadas para tratar e criar exceções.

## <a name="use-trycatchfinally-blocks-to-recover-from-errors-or-release-resources"></a>Usar blocos try/catch/finally para se recuperar de erros ou liberar recursos

Use blocos `try`/`catch` ao redor de código que pode potencialmente gerar uma exceção ***e*** seu código pode se recuperar dessa exceção. Em blocos `catch`, sempre ordene as exceções da mais derivada para a menos derivada. Todas as exceções derivam de <xref:System.Exception>. Exceções mais derivadas não são manipuladas por uma cláusula catch é precedido por uma cláusula catch de uma classe de exceção base. Quando seu código não pode se recuperar de uma exceção, não use cláusula catch nessa exceção. Habilite métodos adicionais na pilha de chamadas para se recuperar se possível.

Limpe os recursos alocados com instruções `using` ou blocos `finally`. Prefira instruções `using` para limpar recursos automaticamente quando exceções forem lançadas. Use blocos `finally` para limpar os recursos que não implementam <xref:System.IDisposable>. O código em uma cláusula `finally` quase sempre é executado, mesmo quando exceções são geradas.

## <a name="handle-common-conditions-without-throwing-exceptions"></a>Tratar de condições comuns sem gerar exceções

Para condições que têm boa probabilidade de ocorrer mas que podem disparar uma exceção, considere tratá-las de uma maneira que evite essa exceção. Por exemplo, se você tentar fechar uma conexão que já está fechada, você obterá um `InvalidOperationException`. Você pode evitar isso usando uma instrução `if` para verificar o estado da conexão antes de tentar fechá-la.

[!code-cpp[Conceptual.Exception.Handling#2](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#2)]
[!code-csharp[Conceptual.Exception.Handling#2](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#2)]
[!code-vb[Conceptual.Exception.Handling#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#2)]

Se você não verificar estado da conexão antes de fechar, você poderá capturar a exceção `InvalidOperationException`.

[!code-cpp[Conceptual.Exception.Handling#3](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#3)]
[!code-csharp[Conceptual.Exception.Handling#3](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#3)]
[!code-vb[Conceptual.Exception.Handling#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#3)]

O método a escolher depende da frequência com que você espera que o evento ocorra.

- Use o tratamento de exceções se o evento não ocorrer muito frequentemente, ou seja, se o evento é realmente excepcional e indica um erro (como um fim de arquivo inesperado). Quando você usa o tratamento de exceções, menos código é executado em condições normais.

- Verifique a existência de condições de erro no código se o evento ocorrer rotineiramente e puder ser considerado parte da execução normal. Quando você verifica se há condições de erro comuns, menos código é executado porque você evita exceções.

## <a name="design-classes-so-that-exceptions-can-be-avoided"></a>Projetar classes de modo que as exceções possam ser evitadas

Uma classe pode fornecer métodos ou propriedades que permitem que você evite fazer uma chamada que dispararia uma exceção. Por exemplo, uma classe <xref:System.IO.FileStream> fornece métodos que ajudam a determinar se ao final do arquivo foi atingido. Elas podem ser usadas para evitar exceções geradas quando você faz a leitura após o fim do arquivo. O exemplo a seguir mostra como ler até o final de um arquivo sem disparar uma exceção.

[!code-cpp[Conceptual.Exception.Handling#5](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#5)]
[!code-csharp[Conceptual.Exception.Handling#5](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#5)]
[!code-vb[Conceptual.Exception.Handling#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#5)]

Outra maneira de evitar exceções é retornar nulo (ou padrão) para casos muito comuns de erro, em vez de gerar uma exceção. Um caso extremamente comum de erro pode ser considerado um fluxo normal de controle. Ao retornar nulo (ou padrão) nesses casos, você minimiza o impacto no desempenho de um aplicativo.

Para tipos de valor, convém considerar o uso de `Nullable<T>` ou padrão como indicador de erro para seu aplicativo. Ao usar `Nullable<Guid>`, `default` se torna `null` em vez de `Guid.Empty`. Algumas vezes, adicionar `Nullable<T>` pode deixar mais claro quando um valor está presente ou ausente. Outras vezes, adicionar `Nullable<T>` pode criar casos extras que não precisam ser verificados e só servem para criar possíveis fontes de erros.

## <a name="throw-exceptions-instead-of-returning-an-error-code"></a>Gerar exceções em vez de retornar um código de erro

Exceções asseguram que falhas não passem despercebidas porque o código de chamada não verificou um código de retorno.

## <a name="use-the-predefined-net-exception-types"></a>Usar os tipos de exceção do .NET predefinidos

Apresente uma nova classe de exceção apenas quando a predefinida não se aplicar. Por exemplo:

- Gere uma exceção <xref:System.InvalidOperationException> se uma propriedade de definição ou chamada de método não for adequada para o estado atual do objeto.

- Gere uma exceção <xref:System.ArgumentException> ou uma das classes predefinidas que derivam de <xref:System.ArgumentException> se parâmetros inválidos forem passados.

## <a name="end-exception-class-names-with-the-word-exception"></a>Terminar os nomes das classes de exceção com a palavra `Exception`

Quando uma exceção personalizada for necessária, nomeie-a adequadamente e derive-a da classe <xref:System.Exception>. Por exemplo:

[!code-cpp[Conceptual.Exception.Handling#4](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#4)]
[!code-csharp[Conceptual.Exception.Handling#4](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#4)]
[!code-vb[Conceptual.Exception.Handling#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#4)]

## <a name="include-three-constructors-in-custom-exception-classes"></a>Incluir três construtores em classes de exceção personalizada

Use pelo menos três os construtores comuns ao criar suas próprias classes de exceção: o construtor sem parâmetros, um construtor que recebe uma mensagem de cadeia de caracteres e uma exceção interna.

- <xref:System.Exception.%23ctor>, que usa valores padrão.

- <xref:System.Exception.%23ctor%28System.String%29>, que aceita uma mensagem de cadeia de caracteres.

- <xref:System.Exception.%23ctor%28System.String%2CSystem.Exception%29>, que aceita uma mensagem de cadeia de caracteres e a exceção interna.

Para ver um exemplo, veja [Como criar exceções definidas pelo usuário](how-to-create-user-defined-exceptions.md).

## <a name="ensure-that-exception-data-is-available-when-code-executes-remotely"></a>Certifique-se de que os dados de exceção estão disponíveis quando o código é executado remotamente

Ao criar exceções definidas pelo usuário, assegure que os metadados para as exceções estejam disponíveis para códigos executando remotamente.

Por exemplo, em implementações do .NET que dão suporte a Domínios de Aplicativo, podem ocorrer exceções entre Domínios de Aplicativo. Suponha que o Domínio de Aplicativo A crie o Domínio de aplicativo B, o qual executa o código que gera uma exceção. Para que o Domínio de Aplicativo A capture e trate corretamente a exceção, ele deverá ser capaz de localizar o assembly que contém a exceção gerada pelo Domínio de Aplicativo B. Se o Domínio de Aplicativo B gerar uma exceção contida em um assembly em sua base de aplicativo, mas não na base de aplicativo do Domínio de Aplicativo A, o Domínio de Aplicativo A não será capaz de localizar a exceção e o Common Language Runtime gerará uma exceção <xref:System.IO.FileNotFoundException>. Para evitar essa situação, você pode implantar o assembly que contém as informações de exceção de duas maneiras:

- Coloque o assembly em uma base de aplicativos comum compartilhada por ambos os domínios de aplicativos.

    \- ou –

- Se os domínios não compartilham uma base de aplicativos comum, assine o assembly que contém as informações de exceção com um nome forte e implante o assembly no cache de assembly global.

## <a name="use-grammatically-correct-error-messages"></a>Usar mensagens de erro gramaticalmente corretas

Escreva frases claras e inclua pontuação final. Cada sentença na cadeia de caracteres atribuída à propriedade <xref:System.Exception.Message?displayProperty=nameWithType> deve terminar com um ponto. Por exemplo, "A tabela de log estourou". seria uma cadeia de caracteres de mensagem adequada.

## <a name="include-a-localized-string-message-in-every-exception"></a>Incluir uma mensagem de cadeia de caracteres localizada em cada exceção

A mensagem de erro que o usuário recebe é derivada da propriedade <xref:System.Exception.Message?displayProperty=nameWithType> da exceção que foi gerada, e não do nome da classe de exceção. Normalmente, você atribui um valor à propriedade <xref:System.Exception.Message?displayProperty=nameWithType> passando a cadeia de caracteres de mensagem para o argumento `message` de um [Construtor de exceção](xref:System.Exception.%23ctor%2A).

Para aplicativos localizados, você deverá fornecer uma cadeia de caracteres de mensagem localizada para toda exceção que seu aplicativo puder gerar. Use arquivos de recurso para fornecer mensagens de erro localizadas. Para obter informações sobre a localização de aplicativos e a recuperação de cadeias de caracteres localizadas, consulte os seguintes artigos:

- [Como criar exceções definidas pelo usuário com mensagens de exceção localizadas](how-to-create-localized-exception-messages.md)
- [Recursos em aplicativos da área de trabalho](../../framework/resources/index.md)
- <xref:System.Resources.ResourceManager?displayProperty=nameWithType>

## <a name="in-custom-exceptions-provide-additional-properties-as-needed"></a>Em exceções personalizadas, forneça propriedades adicionais conforme necessário

Forneça propriedades adicionais para uma exceção (além da cadeia de caracteres de mensagem personalizada) somente quando houver um cenário programático no qual as informações adicionais serão úteis. Por exemplo, o <xref:System.IO.FileNotFoundException> fornece a propriedade <xref:System.IO.FileNotFoundException.FileName>.

## <a name="place-throw-statements-so-that-the-stack-trace-will-be-helpful"></a>Posicionar instruções throw de modo que o rastreamento de pilha seja útil

O rastreamento de pilha começa na instrução na qual a exceção é lançada e termina na instrução `catch` que captura a exceção.

## <a name="use-exception-builder-methods"></a>Usar métodos de construtor de exceção

É comum uma classe lançar a mesma exceção em locais diferentes em sua implementação. Para evitar excesso de código, use métodos auxiliares que criam a exceção e a retornam. Por exemplo:

[!code-cpp[Conceptual.Exception.Handling#6](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#6)]
[!code-csharp[Conceptual.Exception.Handling#6](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#6)]
[!code-vb[Conceptual.Exception.Handling#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#6)]

Em alguns casos, é mais adequado usar o construtor da exceção para compilá-la. Um exemplo é uma classe de exceção global como <xref:System.ArgumentException>.

## <a name="restore-state-when-methods-dont-complete-due-to-exceptions"></a>Restaurar o estado quando os métodos não são concluídos devido a exceções

Os chamadores devem ser capazes de pressupor que não haverá efeitos colaterais quando uma exceção for gerada de um método. Por exemplo, se você tiver um código que transfere dinheiro retirando-o de uma conta e depositando em outra e uma exceção for gerada ao executar o depósito, você não desejará que a retirada permaneça em vigor.

```csharp
public void TransferFunds(Account from, Account to, decimal amount)
{
    from.Withdrawal(amount);
    // If the deposit fails, the withdrawal shouldn't remain in effect.
    to.Deposit(amount);
}
```

```vb
Public Sub TransferFunds(from As Account, [to] As Account, amount As Decimal)
    from.Withdrawal(amount)
    ' If the deposit fails, the withdrawal shouldn't remain in effect.
    [to].Deposit(amount)
End Sub
```

O método acima não gera nenhuma exceção diretamente, mas deverá ser escrito de forma defensiva para que, se a operação de depósito falhar, a retirada seja invertida.

Uma maneira de lidar com essa situação é capturar todas as exceções geradas pela transação do depósito e reverter a retirada.

```csharp
private static void TransferFunds(Account from, Account to, decimal amount)
{
    string withdrawalTrxID = from.Withdrawal(amount);
    try
    {
        to.Deposit(amount);
    }
    catch
    {
        from.RollbackTransaction(withdrawalTrxID);
        throw;
    }
}
```

```vb
Private Shared Sub TransferFunds(from As Account, [to] As Account, amount As Decimal)
    Dim withdrawalTrxID As String = from.Withdrawal(amount)
    Try
        [to].Deposit(amount)
    Catch
        from.RollbackTransaction(withdrawalTrxID)
        Throw
    End Try
End Sub
```

Este exemplo ilustra o uso de `throw` para gerar novamente a exceção original, que pode tornar mais fácil para os chamadores ver a causa real do problema sem a necessidade de examinar a propriedade <xref:System.Exception.InnerException>. Uma alternativa é gerar uma nova exceção e incluir a exceção original como a exceção interna:

```csharp
catch (Exception ex)
{
    from.RollbackTransaction(withdrawalTrxID);
    throw new TransferFundsException("Withdrawal failed.", innerException: ex)
    {
        From = from,
        To = to,
        Amount = amount
    };
}
```

```vb
Catch ex As Exception
    from.RollbackTransaction(withdrawalTrxID)
    Throw New TransferFundsException("Withdrawal failed.", innerException:=ex) With
    {
        .From = from,
        .[To] = [to],
        .Amount = amount
    }
End Try
```

## <a name="see-also"></a>Confira também

- [Exceções](index.md)
