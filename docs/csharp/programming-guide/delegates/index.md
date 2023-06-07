---
title: Delegados – Guia de Programação em C#
description: Um delegado em C# é um tipo que se refere a métodos com uma lista de parâmetros e um tipo de retorno. Delegados são usados para passar métodos como argumentos a outros métodos.
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, delegates
- delegates [C#]
ms.assetid: 97de039b-c76b-4b9c-a27d-8c1e1c8d93da
ms.openlocfilehash: 2c1be56b67528c17a6cf1d8d8517817ff93b2aa5
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063632"
---
# <a name="delegates-c-programming-guide"></a>Delegados (Guia de Programação em C#)
Um [delegado](../../language-reference/builtin-types/reference-types.md) é um tipo que representa referências aos métodos com lista de parâmetros e tipo de retorno específicos. Ao instanciar um delegado, você pode associar sua instância a qualquer método com assinatura e tipo de retorno compatíveis. Você pode invocar (ou chamar) o método através da instância de delegado.  
  
 Delegados são usados para passar métodos como argumentos a outros métodos. Os manipuladores de eventos nada mais são do que métodos chamados por meio de delegados. Ao criar um método personalizado, uma classe como um controle do Windows poderá chamá-lo quando um determinado evento ocorrer. O seguinte exemplo mostra uma declaração de delegado:  
  
 [!code-csharp[csProgGuideDelegates#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#20)]  
  
 Qualquer método de qualquer classe ou struct acessível que corresponda ao tipo delegado pode ser atribuído ao delegado. O método pode ser estático ou de instância. Isso possibilita alterar via programação chamadas de método e também conectar novo código a classes existentes.  
  
> [!NOTE]
> No contexto da sobrecarga de método, a assinatura de um método não inclui o valor retornado. No entanto, no contexto de delegados, a assinatura inclui o valor retornado. Em outras palavras, um método deve ter o mesmo tipo de retorno que o delegado.  
  
 Essa capacidade de se referir a um método como um parâmetro torna delegados ideais para definir métodos de retorno de chamada. Por exemplo, uma referência a um método que compara dois objetos poderia ser passada como um argumento para um algoritmo de classificação. Como o código de comparação está em um procedimento separado, o algoritmo de classificação pode ser escrito de forma mais geral.  
  
## <a name="delegates-overview"></a>Visão geral de delegados  
 Os delegados possuem as seguintes propriedades:  
  
- Representantes são semelhantes a ponteiros de função do C++, mas delegados são totalmente orientados a objeto e, ao contrário dos ponteiros de C++ para funções de membro, os delegados encapsulam uma instância do objeto e um método.
  
- Os delegados permitem que métodos sejam passados como parâmetros.  
  
- Os delegados podem ser usados para definir métodos de retorno de chamada.  
  
- Os delegados podem ser encadeados juntos; por exemplo, vários métodos podem ser chamados um único evento.  
  
- Os métodos não precisam corresponder ao tipo delegado exatamente. Para obter mais informações, confira [Usando a variação delegados](../concepts/covariance-contravariance/using-variance-in-delegates.md).  
  
- O C# versão 2.0 introduziu o conceito de [métodos anônimos](../../language-reference/operators/delegate-operator.md) que permitem que blocos de código sejam passados como parâmetros em vez de um método definido separadamente. O C# 3.0 introduziu expressões lambda como uma maneira mais concisa de escrever blocos de código embutidos. Os métodos anônimos e as expressões lambda (em determinados contextos) são compilados para tipos delegados. Juntos, esses recursos são agora conhecidos como funções anônimas. Para saber mais sobre expressões lambda, confira o artigo sobre [expressões lambda](../../language-reference/operators/lambda-expressions.md).
  
## <a name="in-this-section"></a>Nesta seção  
  
- [Usando delegados](./using-delegates.md)  
  
- [Quando usar delegados em vez de interfaces (Guia de Programação em C#)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/ms173173(v=vs.100))  
  
- [Delegados com Métodos Nomeados vs. Métodos anônimos](./delegates-with-named-vs-anonymous-methods.md)  
  
- [Usando variação em delegados](../concepts/covariance-contravariance/using-variance-in-delegates.md)  
  
- [Como combinar delegados (delegados de multicast)](./how-to-combine-delegates-multicast-delegates.md)  
  
- [Como declarar e usar um delegado e criar uma instância dele](./how-to-declare-instantiate-and-use-a-delegate.md)

## <a name="c-language-specification"></a>Especificação da Linguagem C#  

Para obter mais informações, veja [Delegados](~/_csharplang/spec/delegates.md) na [Especificação da linguagem C#](/dotnet/csharp/language-reference/language-specification/introduction). A especificação da linguagem é a fonte definitiva para a sintaxe e o uso de C#.
  
## <a name="featured-book-chapters"></a>Capítulos do Livro em Destaque  
 [Expressões lambda, eventos e delegados](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff518994%28v=orm.10%29) em [C# 3.0 Cookbook, Third Edition: More than 250 solutions for C# 3.0 programmers](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff518995%28v=orm.10%29)  
  
 [Delegados e eventos](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff652490%28v=orm.10%29) em [Learning C# 3.0: Master the fundamentals of C# 3.0](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff652493%28v=orm.10%29)  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Delegate>
- [Guia de programação C#](../index.md)
- [Eventos](../events/index.md)
