---
title: Propriedades – Guia de Programação em C#
description: Uma propriedade em C# é um membro que usa métodos de acessador para ler, gravar ou calcular o valor de um campo particular como se fosse um membro de dados públicos.
ms.date: 03/10/2017
f1_keywords:
- cs.properties
helpviewer_keywords:
- properties [C#]
- C# language, properties
ms.assetid: e295a8a2-b357-4ee7-a12e-385a44146fa8
ms.openlocfilehash: 231e8e6a11f2655ccdea5489f054910a1ecf2586
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86863936"
---
# <a name="properties-c-programming-guide"></a>Propriedades (Guia de Programação em C#)

Uma propriedade é um membro que oferece um mecanismo flexível para ler, gravar ou calcular o valor de um campo particular. As propriedades podem ser usadas como se fossem membros de dados públicos, mas na verdade elas são métodos realmente especiais chamados *acessadores*. Isso permite que os dados sejam acessados facilmente e ainda ajuda a promover a segurança e a flexibilidade dos métodos.  

## <a name="properties-overview"></a>Visão geral das propriedades  
  
- As propriedades permitem que uma classe exponha uma forma pública de obter e definir valores, enquanto oculta o código de implementação ou de verificação.  
  
- Um acessador de propriedade [get](../../language-reference/keywords/get.md) é usado para retornar o valor da propriedade e um acessador de propriedade [set](../../language-reference/keywords/set.md) é usado para atribuir um novo valor. Esses acessadores podem ter diferentes níveis de acesso. Para obter mais informações, consulte [Restringindo a acessibilidade aos acessadores](./restricting-accessor-accessibility.md).  
  
- A palavra-chave [value](../../language-reference/keywords/value.md) é usada para definir o valor que está sendo atribuído pelo acessador `set`.  
- As propriedades podem ser de *leitura/gravação* (elas têm um acessador `get` e `set`), *somente leitura* (elas têm um acessador `get`, mas nenhum `set`) ou *somente gravação* (elas têm um acessador `set`, mas nenhum `get`). As propriedades somente gravação são raras e são mais comumente usadas para restringir o acesso a dados confidenciais.

- As propriedades simples que não exigem nenhum código de acessador personalizado podem ser implementadas como definições de corpo da expressão ou como [propriedades autoimplementadas](./auto-implemented-properties.md).

## <a name="properties-with-backing-fields"></a>Propriedades com campos de suporte

Um padrão básico para implementar uma propriedade envolve o uso de um campo de suporte particular da propriedade para definir e recuperar o valor da propriedade. O acessador `get` retorna o valor do campo particular e o acessador `set` pode realizar alguma validação de dados antes de atribuir um valor ao campo particular. Os dois acessadores também podem realizar alguma conversão ou cálculo nos dados antes de eles serem armazenados ou retornados.

O exemplo a seguir ilustra esse padrão. Neste exemplo, a classe `TimePeriod` representa um intervalo de tempo. Internamente, a classe armazena o intervalo de tempo em segundos em um campo particular chamado `_seconds`. Uma propriedade de leitura/gravação chamada `Hours` permite que o cliente especifique o intervalo de tempo em horas. Tanto o acessador `get` quanto o `set` executam a conversão necessária entre horas e segundos. Além disso, o acessador `set` valida os dados e gera um <xref:System.ArgumentOutOfRangeException> se o número de horas é inválido.

 [!code-csharp[Properties#1](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/properties-1.cs)]  
  
## <a name="expression-body-definitions"></a>Definições de corpo de expressão  

 Os acessadores de propriedade geralmente consistem em instruções de linha única que simplesmente atribuem ou retornam o resultado de uma expressão. Você pode implementar essas propriedades como membros aptos para expressão. As definições de corpo da expressão consistem no símbolo `=>` seguido pela expressão à qual atribuir ou recuperar da propriedade.

 A partir do C#6, as propriedades somente leitura podem implementar o acessador `get` como um membro apto para expressão. Nesse caso, nem a palavra-chave do acessador `get` nem a palavra-chave `return` é usada. O exemplo a seguir implementa a propriedade `Name` somente leitura como um membro apto para expressão.

 [!code-csharp[Properties#2](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/properties-2.cs)]  

 Começando com o C# 7.0, os acessadores `get` e `set` podem ser implementados como membros aptos para expressão. Nesse caso, as palavras-chave `get` e `set` devem estar presentes. O exemplo a seguir ilustra o uso de definições de corpo de expressão para ambos os acessadores. Observe que a palavra-chave `return` não é usada com o acessador `get`.

  [!code-csharp[Properties#3](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/properties-3.cs)]  

## <a name="auto-implemented-properties"></a>Propriedades autoimplementadas

Em alguns casos, os acessadores `get` e `set` da propriedade apenas atribuem um valor ou recuperam um valor de um campo de suporte sem incluir nenhuma lógica adicional. Usando propriedades autoimplementadas, você pode simplificar o código enquanto o compilador C# fornece de forma transparente o campo de suporte para você.

Se uma propriedade tiver tanto um acessador `get` quanto um `set`, ambos deverão ser autoimplementados. Você define uma propriedade autoimplementada usando as palavras-chave `get` e `set` sem fornecer qualquer implementação. O exemplo a seguir repete o anterior, exceto que `Name` e `Price` são propriedades autoimplementadas. Observe que o exemplo também remove o construtor com parâmetros, de modo que objetos `SaleItem` agora são inicializados com uma chamada para o construtor sem parâmetros e um [inicializador de objeto](object-and-collection-initializers.md).

  [!code-csharp[Properties#4](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/properties-4.cs)]  

## <a name="related-sections"></a>Seções relacionadas  
  
- [Usando propriedades](./using-properties.md)  
  
- [Propriedades de interface](./interface-properties.md)  
  
- [Comparação entre propriedades e indexadores](../indexers/comparison-between-properties-and-indexers.md)  
  
- [Restringindo a acessibilidade ao acessador](./restricting-accessor-accessibility.md)  
  
- [Propriedades autoimplementadas](./auto-implemented-properties.md)  
  
## <a name="c-language-specification"></a>Especificação da Linguagem C#  

Para obter mais informações, veja [Propriedades](~/_csharplang/spec/classes.md#properties) na [Especificação da Linguagem C#](/dotnet/csharp/language-reference/language-specification/introduction). A especificação da linguagem é a fonte definitiva para a sintaxe e o uso de C#.
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Usando propriedades](./using-properties.md)
- [Indexadores](../indexers/index.md)
- [Palavra-chave get](../../language-reference/keywords/get.md)
- [Palavra-chave set](../../language-reference/keywords/set.md)
