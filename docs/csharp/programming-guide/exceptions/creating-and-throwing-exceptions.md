---
title: Como criar e lançar exceções – Guia de Programação em C#
description: Saiba mais sobre como criar e lançar exceções. As exceções são usadas para indicar que ocorreu um erro durante a execução de um programa.
ms.date: 07/20/2015
helpviewer_keywords:
- catching exceptions [C#]
- throwing exceptions [C#]
- exceptions [C#], creating
- exceptions [C#], throwing
ms.assetid: 6bbba495-a115-4c6d-90cc-1f4d7b5f39e2
ms.openlocfilehash: 8ab10dbf686def8d169ef3239492e3b618e9d297
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302042"
---
# <a name="creating-and-throwing-exceptions-c-programming-guide"></a>Criando e lançando exceções (Guia de Programação em C#)
As exceções são usadas para indicar que ocorreu um erro durante a execução do programa. Objetos de exceção que descrevem um erro são criados e, em seguida, *lançados* com a palavra-chave [throw](../../language-reference/keywords/throw.md). Então, o runtime procura o manipulador de exceção mais compatível.  
  
 Os programadores devem lançar exceções quando uma ou mais das seguintes condições forem verdadeiras:  
  
- O método não pode concluir sua funcionalidade definida.  
  
     Por exemplo, se um parâmetro para um método tem um valor inválido:  
  
     [!code-csharp[csProgGuideExceptions#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#12)]  
  
- É feita uma chamada inadequada a um objeto, com base no estado do objeto.  
  
     Um exemplo pode ser a tentativa de gravar em um arquivo somente leitura. Em casos em que um estado do objeto não permite uma operação, lance uma instância de <xref:System.InvalidOperationException> ou um objeto com base em uma derivação dessa classe. Este é um exemplo de um método que gera um objeto <xref:System.InvalidOperationException>:  
  
     [!code-csharp[csProgGuideExceptions#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#13)]  
  
- Quando um argumento para um método causa uma exceção.  
  
     Nesse caso, a exceção original deve ser capturada e uma instância de <xref:System.ArgumentException> deve ser criada. A exceção original deve ser passada para o construtor do <xref:System.ArgumentException> como o parâmetro <xref:System.Exception.InnerException%2A>:  
  
     [!code-csharp[csProgGuideExceptions#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#14)]  
  
 As exceções contêm uma propriedade chamada <xref:System.Exception.StackTrace%2A>. Essa cadeia de caracteres contém o nome dos métodos na pilha de chamadas atual, junto com o nome de arquivo e o número de linha em que a exceção foi lançada para cada método. Um objeto <xref:System.Exception.StackTrace%2A> é criado automaticamente pelo CLR (Common Language Runtime) no ponto da instrução `throw`, de modo que as exceções devem ser lançadas do ponto em que o rastreamento de pilha deve começar.  
  
 Todas as exceções contêm uma propriedade chamada <xref:System.Exception.Message%2A>. Essa cadeia de caracteres deve ser definida para explicar o motivo da exceção. Observe que as informações que são sensíveis à segurança não devem ser colocadas no texto da mensagem. Além <xref:System.Exception.Message%2A>, <xref:System.ArgumentException> contém uma propriedade chamada <xref:System.ArgumentException.ParamName%2A> que deve ser definida como o nome do argumento que causou a exceção a ser lançada. No caso de um setter de propriedade <xref:System.ArgumentException.ParamName%2A> deve ser definido como `value`.  
  
 Os métodos públicos e protegidos devem lançar exceções sempre que não puderem concluir suas funções pretendidas. A classe de exceção que é lançada deve ser a exceção mais específica disponível que se adapta às condições do erro. Essas exceções devem ser documentadas como parte da funcionalidade de classe e as classes derivadas ou as atualizações da classe original devem manter o mesmo comportamento para compatibilidade com versões anteriores.  
  
## <a name="things-to-avoid-when-throwing-exceptions"></a>Coisas a serem evitadas ao lançar exceções  
 A lista a seguir identifica as práticas a serem evitadas ao lançar exceções:  
  
- As exceções não devem ser usadas para alterar o fluxo de um programa, como parte da execução normal. As exceções só devem ser usadas para relatar e tratar condições de erro.  
  
- As exceções não devem ser retornadas como um valor retornado ou um parâmetro em vez de serem lançadas.  
  
- Não lance <xref:System.Exception?displayProperty=nameWithType>, <xref:System.SystemException?displayProperty=nameWithType>, <xref:System.NullReferenceException?displayProperty=nameWithType> ou <xref:System.IndexOutOfRangeException?displayProperty=nameWithType> intencionalmente de seu próprio código-fonte.  
  
- Não crie exceções que podem ser lançadas no modo de depuração, mas não no modo de versão. Em vez disso, use o Debug Assert para identificar erros em tempo de execução durante a fase de desenvolvimento.  
  
## <a name="defining-exception-classes"></a>Definindo classes de exceção  
 Os programas podem lançar uma classe de exceção predefinida no namespace <xref:System> (exceto quando observado anteriormente) ou criar suas próprias classes de exceção, derivando de <xref:System.Exception>. As classes derivadas devem definir pelo menos quatro construtores: um construtor sem parâmetros, um que define a propriedade de mensagem e um que define as propriedades <xref:System.Exception.Message%2A> e <xref:System.Exception.InnerException%2A>. O quarto construtor é usado para serializar a exceção. Novas classes de exceção devem ser serializáveis. Por exemplo:  
  
 [!code-csharp[csProgGuideExceptions#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#15)]  
  
 Novas propriedades só devem ser adicionadas à classe de exceção quando os dados que elas fornecem são úteis para resolver a exceção. Se forem adicionadas novas propriedades à classe de exceção derivada, `ToString()` deverá ser substituído para retornar as informações adicionadas.  
  
## <a name="c-language-specification"></a>Especificação da Linguagem C#  

Para obter mais informações, veja [Exceções](~/_csharplang/spec/exceptions.md) e [A declaração throw](~/_csharplang/spec/statements.md#the-throw-statement) na [Especificação da Linguagem C#](/dotnet/csharp/language-reference/language-specification/introduction). A especificação da linguagem é a fonte definitiva para a sintaxe e o uso de C#.
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Exceções e tratamento de exceções](./index.md)
- [Hierarquia de exceções](../../../standard/exceptions/index.md)
- [Tratamento de exceção](./exception-handling.md)
