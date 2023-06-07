---
title: Genéricos em tempo de execução – Guia de Programação em C#
description: Saiba mais sobre os tipos genéricos no tempo de execução. Consulte exemplos de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], at run time
ms.assetid: 119df7e6-9ceb-49df-af36-24f8f8c0747f
ms.openlocfilehash: 8e072e7aa53177929dda0be931beb85863b6a12e
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87299221"
---
# <a name="generics-in-the-run-time-c-programming-guide"></a>Genéricos em tempo de execução (Guia de Programação em C#)
Um tipo genérico ou método compilado em Microsoft Intermediate Language (MSIL) conterá metadados que o identificarão como possuidor de parâmetros de tipo. O uso de MSIL em um tipo genérico será diferente de acordo com o parâmetro de tipo fornecido, ou seja, se ele é um tipo de valor ou um tipo de referência.  
  
 Quando um tipo genérico é construído pela primeira vez com um tipo de valor como parâmetro, o runtime cria um tipo genérico especializado com o(s) parâmetro(s) fornecido(s) substituído(s) nos locais apropriados do MSIL. Os tipos genéricos especializados são criados uma vez para cada tipo de valor único usado como parâmetro.  
  
 Por exemplo, caso o código do programa declarar uma pilha construída de inteiros:  
  
 [!code-csharp[csProgGuideGenerics#42](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#42)]  
  
 Neste ponto, o runtime gerará uma versão especializada da classe <xref:System.Collections.Generic.Stack%601> com o inteiro substituído corretamente, de acordo com seu parâmetro. Agora, sempre que o código do programa utilizar uma pilha de inteiros, o runtime reutilizará a classe especializada <xref:System.Collections.Generic.Stack%601> gerada. No exemplo a seguir, são criadas duas instâncias de uma pilha de inteiros e eles compartilham uma única instância do código `Stack<int>`:  
  
 [!code-csharp[csProgGuideGenerics#43](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#43)]  
  
 No entanto, suponha que outra classe <xref:System.Collections.Generic.Stack%601> com um tipo de valor diferente – como `long` ou uma estrutura definida pelo usuário como parâmetro – foi criada em outro ponto do código. Como resultado, o runtime gerará outra versão do tipo genérico e substituirá um `long` nos locais apropriados no MSIL. Conversões não são mais necessárias, pois cada classe genérica especializada contém o tipo de valor nativamente.  
  
 Os genéricos funcionam de outro modo nos tipos de referência. Na primeira vez em que um genérico é construído com qualquer tipo de referência, o runtime cria um tipo genérico especializado com referências de objeto substituídas por parâmetros no MSIL. Em seguida, sempre que um tipo construído for instanciado com um tipo de referência como parâmetro, independentemente do tipo, o runtime reutilizará a versão especializada do tipo genérico criada anteriormente. Isso é possível porque todas as referências são do mesmo tamanho.  
  
 Por exemplo, suponha que há dois tipos de referência, uma classe `Customer` e uma classe `Order` e que uma pilha de tipos `Customer` foi criada:  
  
 [!code-csharp[csProgGuideGenerics#47](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#47)]  
  
 [!code-csharp[csProgGuideGenerics#44](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#44)]  
  
 Neste ponto, o runtime gerará uma versão especializada da classe <xref:System.Collections.Generic.Stack%601> que armazenará referências de objeto que serão preenchidas posteriormente, em vez de armazenar dados. Suponha que a próxima linha de código crie uma pilha de outro tipo de referência, com o nome `Order`:  
  
 [!code-csharp[csProgGuideGenerics#45](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#45)]  
  
 Ao contrário dos tipos de valor, outra versão especializada da classe <xref:System.Collections.Generic.Stack%601> não será criada para o tipo `Order`. Em vez disso, uma instância da versão especializada da classe <xref:System.Collections.Generic.Stack%601> será criada e a variável `orders` será definida para referenciá-la. Imagine que uma linha de código foi encontrada para criar uma pilha de um tipo `Customer`:  
  
 [!code-csharp[csProgGuideGenerics#46](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#46)]  
  
 Assim como acontece com o uso anterior da classe <xref:System.Collections.Generic.Stack%601> criada usando o tipo `Order`, outra instância da classe especializada <xref:System.Collections.Generic.Stack%601> é criada. Os ponteiros contidos nela são definidos para referenciar uma área de memória do tamanho de um tipo `Customer`. Como a quantidade de tipos de referência pode variar muito entre os programas, a implementação de genéricos no C# reduz significativamente a quantidade de código ao diminuir para um o número de classes especializadas criadas pelo compilador para classes genéricas ou tipos de referência.  
  
 Além disso, quando uma classe genérica do C# for instanciada usando um tipo de valor ou parâmetro de tipo de referência, a reflexão pode consultá-la em runtime e o seu tipo real e parâmetro de tipo podem ser determinados.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Collections.Generic>
- [Guia de programação C#](../index.md)
- [Introdução aos genéricos](./index.md)
- [Genéricos](../../../standard/generics/index.md)
