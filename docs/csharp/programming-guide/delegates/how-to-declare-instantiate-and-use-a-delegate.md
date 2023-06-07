---
title: Como declarar, instanciar e usar um guia de programação do delegado C#
description: Saiba como declarar, criar uma instância e usar um delegado. Veja exemplos que abordam o C# 1,0, 2,0 e 3,0 e posterior.
ms.date: 07/20/2015
helpviewer_keywords:
- delegates [C#], declaring and instantiating
ms.assetid: 61c4895f-f785-48f8-8bfe-db73b411c4ae
ms.openlocfilehash: b741b3bc9c03faaa5fa2c01bd8f70d4be9b099c2
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063660"
---
# <a name="how-to-declare-instantiate-and-use-a-delegate-c-programming-guide"></a>Como declarar, instanciar e usar um delegado (guia de programação C#)
No C# 1.0 e versões posteriores, é possível declarar delegados conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[csProgGuideDelegates#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#13)]  
  
 [!code-csharp[csProgGuideDelegates#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#14)]  
  
 O C# 2.0 oferece uma maneira mais simples de gravar a declaração anterior, conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[csProgGuideDelegates#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#32)]  
  
 No C# 2.0 e versões posteriores, também é possível usar um método anônimo para declarar e inicializar um [delegado](../../language-reference/builtin-types/reference-types.md), conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[csProgGuideDelegates#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#15)]  
  
 No C# 3.0 e versões posteriores, os delegados também podem ser declarados e instanciados usando uma expressão lambda, conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[csProgGuideDelegates#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#31)]  
  
 Para obter mais informações, consulte [Expressões Lambda](../../language-reference/operators/lambda-expressions.md).  
  
 O exemplo a seguir ilustra a declaração, instanciação e o uso de um delegado. A classe `BookDB` encapsula um banco de dados de uma livraria que mantém um banco de dados de livros. Ela expõe um método, `ProcessPaperbackBooks`, que localiza todos os livros de bolso no banco de dados e chama um delegado para cada um. O tipo `delegate` usado tem o nome `ProcessBookDelegate`. A classe `Test` usa essa classe para imprimir os títulos e o preço médio dos livros de bolso.  
  
 O uso de delegados promove uma boa separação de funcionalidade entre o banco de dados da livraria e o código de cliente. O código de cliente não tem conhecimento de como os livros são armazenados ou como o código da livraria localiza os livros de bolso. O código da livraria não tem conhecimento do processamento executado nos livros de bolso após a localização.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[csProgGuideDelegates#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#12)]  
  
## <a name="robust-programming"></a>Programação robusta  
  
- Declarando um delegado.  
  
     A instrução a seguir declara um novo tipo de delegado.  
  
     [!code-csharp[csProgGuideDelegates#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#16)]  
  
     Cada tipo de delegado descreve o número e os tipos dos argumentos e o tipo do valor retornado dos métodos que pode encapsular. Sempre que um novo conjunto de tipos de argumento ou tipo de valor retornado for necessário, um novo tipo de delegado deverá ser declarado.  
  
- Instanciando um delegado.  
  
     Após a declaração do tipo de delegado, um objeto delegado deve ser criado e associado a um método específico. No exemplo anterior, faça isso passando o método `PrintTitle` para o método `ProcessPaperbackBooks`, como no exemplo a seguir:  
  
     [!code-csharp[csProgGuideDelegates#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#17)]  
  
     Isso cria um novo objeto delegado associado ao método [estático](../../language-reference/keywords/static.md)`Test.PrintTitle`. Da mesma forma, o método não estático `AddBookToTotal` no objeto `totaller` é passado como no exemplo a seguir:  
  
     [!code-csharp[csProgGuideDelegates#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#18)]  
  
     Em ambos os casos, um novo objeto delegado é passado para o método `ProcessPaperbackBooks`.  
  
     Após a criação de um delegado, o método ao qual ele está associado nunca se altera; objetos delegados são imutáveis.  
  
- Chamando um delegado.  
  
     Normalmente, o objeto delegado, após sua criação, é passado para outro código que chamará o delegado. Um objeto delegado é chamado usando seu nome seguido dos argumentos entre parênteses a serem passados para o delegado. A seguir, veja um exemplo de uma chamada de delegado:  
  
     [!code-csharp[csProgGuideDelegates#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#19)]  
  
     Um delegado pode ser chamado de forma síncrona, como neste exemplo ou de forma assíncrona, usando os métodos `BeginInvoke` e `EndInvoke`.  
  
## <a name="see-also"></a>Consulte também

- [Guia de programação C#](../index.md)
- [Eventos](../events/index.md)
- [Representantes](./index.md)
