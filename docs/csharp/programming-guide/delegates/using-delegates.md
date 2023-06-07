---
title: Como usar delegados – Guia de Programação em C#
description: Saiba como usar delegados. Delegados são um tipo seguro, de tipo seguro e orientado a objeto que encapsula com segurança um método.
ms.date: 07/20/2015
helpviewer_keywords:
- delegates [C#], how to use
ms.assetid: 99a2fc27-a32e-4a34-921c-e65497520eec
ms.openlocfilehash: a9b625b8c0785ed2f446be27c11dc76108bc4bce
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302146"
---
# <a name="using-delegates-c-programming-guide"></a>Usando delegados (Guia de Programação em C#)

Um [delegado](../../language-reference/builtin-types/reference-types.md) é um tipo que encapsula com segurança um método, semelhante a um ponteiro de função em C e C++. No entanto, ao contrário dos ponteiros de função de C, delegados são orientados a objeto, fortemente tipados e seguros. O tipo de um delegado é definido pelo nome do delegado. O exemplo a seguir declara um delegado chamado `Del` que pode encapsular um método que usa uma [cadeia de caracteres](../../language-reference/builtin-types/reference-types.md) como um argumento e retorna [nulo](../../language-reference/builtin-types/void.md):

[!code-csharp[csProgGuideDelegates#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#21)]

Um objeto delegado é normalmente construído fornecendo-se o nome do método que o delegado encapsulará ou com uma [função anônima](../statements-expressions-operators/anonymous-functions.md). Quando um delegado é instanciado, uma chamada de método feita ao delegado será passada pelo delegado para esse método. Os parâmetros passados para o delegado pelo chamador são passados para o método e o valor de retorno, se houver, do método é retornado ao chamador pelo delegado. Isso é conhecido como invocar o delegado. Um delegado instanciado pode ser invocado como se fosse o método encapsulado em si. Por exemplo:

[!code-csharp[csProgGuideDelegates#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#22)]  

[!code-csharp[csProgGuideDelegates#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#23)]

Tipos delegados são derivados da <xref:System.Delegate> classe no .net. Tipos de delegado são [lacrados](../../language-reference/keywords/sealed.md) – não podem ser derivados de – e não é possível derivar classes personalizadas de <xref:System.Delegate>. Como o delegado instanciado é um objeto, ele pode ser passado como um parâmetro ou atribuído a uma propriedade. Isso permite que um método aceite um delegado como um parâmetro e chame o delegado posteriormente. Isso é conhecido como um retorno de chamada assíncrono e é um método comum de notificação de um chamador quando um processo longo for concluído. Quando um delegado é usado dessa maneira, o código que usa o delegado não precisa de conhecimento algum da implementação do método que está sendo usado. A funcionalidade é semelhante ao encapsulamento que as interfaces fornecem.

Outro uso comum de chamadas de retorno é definir um método de comparação personalizada e passar esse delegado para um método de classificação. Ele permite que o código do chamador se torne parte do algoritmo de classificação. O método de exemplo a seguir usa o tipo `Del` como um parâmetro:

[!code-csharp[csProgGuideDelegates#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#24)]

Em seguida, você pode passar o delegado criado acima para esse método:

[!code-csharp[csProgGuideDelegates#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#25)]

e receber a seguinte saída para o console:

```console
The number is: 3
```

Usando o delegado como uma abstração, `MethodWithCallback` não precisa chamar o console diretamente — ele não precisa ser criado com um console em mente. O que `MethodWithCallback` faz é simplesmente preparar uma cadeia de caracteres e passá-la para outro método. Isso é especialmente poderoso, uma vez que um método delegado pode usar qualquer número de parâmetros.

Quando um delegado é construído para encapsular um método de instância, o delegado faz referência à instância e ao método. Um delegado não tem conhecimento do tipo de instância além do método que ele encapsula, de modo que um delegado pode se referir a qualquer tipo de objeto desde que haja um método nesse objeto que corresponda à assinatura do delegado. Quando um delegado é construído para encapsular um método estático, ele só faz referência ao método. Considere as seguintes declarações:

[!code-csharp[csProgGuideDelegates#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#26)]

Além do `DelegateMethod` estático mostrado anteriormente, agora temos três métodos que podem ser encapsulados por uma instância `Del`.

Um delegado pode chamar mais de um método quando invocado. Isso é chamado de multicast. Para adicionar um método extra à lista de métodos do delegado — a lista de invocação — basta adicionar dois delegados usando os operadores de adição ou de atribuição de adição ('+' ou '+ ='). Por exemplo:

[!code-csharp[csProgGuideDelegates#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#27)]

Nesse ponto, `allMethodsDelegate` contém três métodos em sua lista de invocação —`Method1`, `Method2` e `DelegateMethod`. Os três delegados originais, `d1`, `d2` e `d3`, permanecem inalterados. Quando `allMethodsDelegate` é invocado, os três métodos são chamados na ordem. Se o delegado usar parâmetros de referência, a referência será passada em sequência para cada um dos três métodos por vez, e quaisquer alterações em um método serão visíveis no próximo método. Quando algum dos métodos gerar uma exceção que não foi detectada dentro do método, essa exceção será passada ao chamador do delegado e nenhum método subsequente na lista de invocação será chamado. Se o delegado tiver um valor de retorno e/ou parâmetros de saída, ele retornará o valor de retorno e os parâmetros do último método invocado. Para remover um método da lista de invocação, use os operadores de atribuição de subtração [ou subtração](../../language-reference/operators/subtraction-operator.md) ( `-` ou `-=` ). Por exemplo:

[!code-csharp[csProgGuideDelegates#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#28)]

Como os tipos de delegados são derivados de `System.Delegate`, os métodos e as propriedades definidos por essa classe podem ser chamados no delegado. Por exemplo, para localizar o número de métodos na lista de invocação do delegado, é possível escrever:

[!code-csharp[csProgGuideDelegates#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#29)]

Delegados com mais de um método em sua lista de invocação derivam de <xref:System.MulticastDelegate>, que é uma subclasse de `System.Delegate`. O código acima funciona em ambos os casos, pois as classes oferecem suporte à `GetInvocationList`.

Delegados multicast são amplamente usados na manipulação de eventos. Objetos de origem do evento enviam notificações de eventos aos objetos de destinatário que se registraram para receber esse evento. Para se registrar para um evento, o destinatário cria um método projetado para lidar com o evento, em seguida, cria um delegado para esse método e passa o delegado para a origem do evento. A origem chama o delegado quando o evento ocorre. O delegado chama então o método de manipulação de eventos no destinatário, fornecendo os dados do evento. O tipo de delegado de um determinado evento é definido pela origem do evento. Para saber mais, consulte [Eventos](../events/index.md).

A comparação de delegados de dois tipos diferentes atribuídos no tempo de compilação resultará em um erro de compilação. Se as instâncias de delegado forem estaticamente do tipo `System.Delegate`, então a comparação será permitida, mas retornará false no tempo de execução. Por exemplo:

[!code-csharp[csProgGuideDelegates#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#30)]

## <a name="see-also"></a>Confira também

- [Guia de programação C#](../index.md)
- [Representantes](./index.md)
- [Usando variação em delegados](../concepts/covariance-contravariance/using-variance-in-delegates.md)
- [Variação em delegações](../concepts/covariance-contravariance/variance-in-delegates.md)
- [Usando Variação para Delegações Genéricas Func e Action](../concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md)
- [Eventos](../events/index.md)
