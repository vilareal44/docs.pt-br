---
title: Como inicializar um dicionário com um inicializador de coleção – Guia de Programação em C#
description: Saiba como inicializar um dicionário em C#, usando o método Add ou um inicializador de índice. Este exemplo mostra as duas opções.
ms.date: 12/20/2018
helpviewer_keywords:
- collection initializers [C#], with Dictionary
ms.assetid: 25283922-f8ee-40dc-a639-fac30804ec71
ms.openlocfilehash: 2f33240b02785c5c886a1ebebb8984d29c9f7795
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865041"
---
# <a name="how-to-initialize-a-dictionary-with-a-collection-initializer-c-programming-guide"></a>Como inicializar um dicionário com um inicializador de coleção (Guia de Programação em C#)

Um <xref:System.Collections.Generic.Dictionary%602> contém uma coleção de pares de chave-valor. Seu método <xref:System.Collections.Generic.Dictionary%602.Add%2A> recebe dois parâmetros, um para a chave e outro para o valor. Uma maneira de inicializar um <xref:System.Collections.Generic.Dictionary%602> ou qualquer coleção cujo método `Add` use vários parâmetros, é colocar cada conjunto de parâmetros entre chaves, conforme mostrado no exemplo a seguir. Outra opção é usar um inicializador de índice, também mostrado no exemplo a seguir.

## <a name="example"></a>Exemplo

No exemplo de código a seguir, um <xref:System.Collections.Generic.Dictionary%602> é inicializado com instâncias do tipo `StudentName`.  A primeira inicialização usa o método `Add` com dois argumentos. O compilador gera uma chamada para `Add` para cada um dos pares de chaves `int` e valores `StudentName`. A segunda usa um método de indexador público de leitura/gravação da classe `Dictionary`:

[!code-csharp[InitializerExample](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/HowToDictionaryInitializer.cs#HowToDictionaryInitializer)]  

Observe os dois pares de chaves em cada elemento da coleção na primeira declaração. As chaves mais internas incluem o inicializador de objeto para o `StudentName` e as chaves mais externas delimitam o inicializador para o par de chave/valor que será adicionado ao `students` <xref:System.Collections.Generic.Dictionary%602> . Por fim, todo o inicializador de coleção do dicionário é colocado entre chaves. Na segunda inicialização, o lado esquerdo da atribuição é a chave e o lado direito é o valor, usando um inicializador de objeto para `StudentName`.

## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Inicializadores de objeto e coleção](./object-and-collection-initializers.md)
