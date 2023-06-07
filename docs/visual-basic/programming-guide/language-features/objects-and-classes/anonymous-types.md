---
title: Tipos anônimos
ms.date: 07/20/2015
f1_keywords:
- vb.AnonymousType
helpviewer_keywords:
- anonymous types [Visual Basic], about anonymous types
- anonymous types [Visual Basic]
- types [Visual Basic], anonymous
ms.assetid: 7b87532c-4b3e-4398-8503-6ea9d67574a4
ms.openlocfilehash: bbe84ce8a62705027c00bc26db74a3c21fa34fd9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411787"
---
# <a name="anonymous-types-visual-basic"></a>Tipos anônimos (Visual Basic)
O Visual Basic dá suporte a tipos anônimos, que permitem criar objetos sem gravar uma definição de classe para o tipo de dados. Em vez disso, o compilador gera uma classe para você. A classe não tem nome utilizável, herda diretamente de <xref:System.Object> e contém as propriedades que você especifica ao declarar o objeto. Como o nome do tipo de dados não é especificado, ele é conhecido como um *tipo anônimo*.  
  
 O exemplo a seguir declara e cria `product` a variável como uma instância de um tipo anônimo que tem duas propriedades, `Name` e `Price` .  
  
 [!code-vb[VbVbalrAnonymousTypes#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#1)]  
  
 Uma *expressão de consulta* usa tipos anônimos para combinar colunas de dados selecionados por uma consulta. Você não pode definir o tipo de resultado com antecedência, porque não é possível prever as colunas que uma consulta específica pode selecionar. Tipos anônimos permitem que você escreva uma consulta que seleciona qualquer número de colunas, em qualquer ordem. O compilador cria um tipo de dados que corresponde às propriedades especificadas e à ordem especificada.  
  
 Nos exemplos a seguir, `products` é uma lista de objetos Product, cada um dos quais tem muitas propriedades. `namePriceQuery`A variável contém a definição de uma consulta que, quando é executada, retorna uma coleção de instâncias de um tipo anônimo que tem duas propriedades, `Name` e `Price` .  
  
 [!code-vb[VbVbalrAnonymousTypes#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#2)]  
  
 `nameQuantityQuery`A variável contém a definição de uma consulta que, quando é executada, retorna uma coleção de instâncias de um tipo anônimo que tem duas propriedades, `Name` e `OnHand` .  
  
 [!code-vb[VbVbalrAnonymousTypes#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#3)]  
  
 Para obter mais informações sobre o código criado pelo compilador para um tipo anônimo, consulte [definição de tipo anônimo](anonymous-type-definition.md).  
  
> [!CAUTION]
> O nome do tipo anônimo é gerado pelo compilador e pode variar de compilação para compilação. Seu código não deve usar ou depender do nome de um tipo anônimo porque o nome pode ser alterado quando um projeto é recompilado.  
  
## <a name="declaring-an-anonymous-type"></a>Declarando um tipo anônimo  
 A declaração de uma instância de um tipo anônimo usa uma lista de inicializador para especificar as propriedades do tipo. Você pode especificar apenas Propriedades ao declarar um tipo anônimo, não outros elementos de classe, como métodos ou eventos. No exemplo a seguir, `product1` é uma instância de um tipo anônimo que tem duas propriedades: `Name` e `Price` .  
  
 [!code-vb[VbVbalrAnonymousTypes#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#4)]  
  
 Se você designar propriedades como propriedades de chave, poderá usá-las para comparar duas instâncias de tipo anônimo para igualdade. No entanto, os valores das propriedades de chave não podem ser alterados. Consulte a seção Propriedades da chave mais adiante neste tópico para obter mais informações.  
  
 Observe que declarar uma instância de um tipo anônimo é como declarar uma instância de um tipo nomeado usando um inicializador de objeto:  
  
 [!code-vb[VbVbalrAnonymousTypes#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#5)]  
  
 Para obter mais informações sobre outras maneiras de especificar propriedades de tipo anônimo, consulte [como: inferir nomes e tipos de propriedade em declarações de tipo anônimo](how-to-infer-property-names-and-types-in-anonymous-type-declarations.md).  
  
## <a name="key-properties"></a>Propriedades da chave  
 As propriedades de chave diferem das propriedades não chave de várias maneiras fundamentais:  
  
- Somente os valores das propriedades de chave são comparados para determinar se duas instâncias são iguais.  
  
- Os valores das propriedades de chave são somente leitura e não podem ser alterados.  
  
- Somente os valores de propriedade de chave são incluídos no algoritmo de código hash gerado pelo compilador para um tipo anônimo.  
  
### <a name="equality"></a>Igualitário  
 As instâncias de tipos anônimos podem ser iguais somente se forem instâncias do mesmo tipo anônimo. O compilador trata duas instâncias como instâncias do mesmo tipo se elas atenderem às seguintes condições:  
  
- Eles são declarados no mesmo assembly.  
  
- Suas propriedades têm os mesmos nomes, os mesmos tipos inferidos e são declaradas na mesma ordem. Comparações de nome não diferenciam maiúsculas de minúsculas.  
  
- As mesmas propriedades em cada uma são marcadas como propriedades de chave.  
  
- Pelo menos uma propriedade em cada declaração é uma propriedade de chave.  
  
 Uma instância de tipos anônimos que não tem propriedades de chave é igual apenas a si mesma.  
  
 [!code-vb[VbVbalrAnonymousTypes#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#6)]  
  
 Duas instâncias do mesmo tipo anônimo serão iguais se os valores de suas propriedades de chave forem iguais. Os exemplos a seguir ilustram como a igualdade é testada.  
  
 [!code-vb[VbVbalrAnonymousTypes#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#7)]  
  
### <a name="read-only-values"></a>Valores somente leitura  
 Os valores das propriedades de chave não podem ser alterados. Por exemplo, no `prod8` no exemplo anterior, os `Name` campos e `Price` são `read-only` , mas `OnHand` podem ser alterados.  
  
 [!code-vb[VbVbalrAnonymousTypes#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#8)]  
  
## <a name="anonymous-types-from-query-expressions"></a>Tipos anônimos de expressões de consulta  
 As expressões de consulta nem sempre exigem a criação de tipos anônimos. Quando possível, eles usam um tipo existente para manter os dados da coluna. Isso ocorre quando a consulta retorna registros inteiros da fonte de dados ou apenas um campo de cada registro. Nos exemplos de código a seguir, `customers` é uma coleção de objetos de uma `Customer` classe. A classe tem muitas propriedades, e você pode incluir uma ou mais delas no resultado da consulta, em qualquer ordem. Nos dois primeiros exemplos, nenhum tipo anônimo é necessário porque as consultas selecionam elementos de tipos nomeados:  
  
- `custs1`contém uma coleção de cadeias de `cust.Name` caracteres, porque é uma String.  
  
     [!code-vb[VbVbalrAnonymousTypes#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#30)]  
  
- `custs2`contém uma coleção de `Customer` objetos, porque cada elemento de `customers` é um `Customer` objeto e o elemento inteiro é selecionado pela consulta.  
  
     [!code-vb[VbVbalrAnonymousTypes#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#31)]  
  
 No entanto, os tipos nomeados apropriados nem sempre estão disponíveis. Talvez você queira selecionar nomes de clientes e endereços para uma finalidade, números de ID de cliente e locais para outro, e nomes de clientes, endereços e históricos de pedidos para um terceiro. Tipos anônimos permitem que você selecione qualquer combinação de propriedades, em qualquer ordem, sem primeiro declarar um novo tipo nomeado para manter o resultado. Em vez disso, o compilador cria um tipo anônimo para cada compilação de propriedades. A consulta a seguir seleciona apenas o nome do cliente e o número de ID de cada `Customer` objeto no `customers` . Portanto, o compilador cria um tipo anônimo que contém apenas essas duas propriedades.  
  
 [!code-vb[VbVbalrAnonymousTYpes#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#32)]  
  
 Os nomes e os tipos de dados das propriedades no tipo anônimo são extraídos dos argumentos para `Select` `cust.Name` e `cust.ID` . As propriedades em um tipo anônimo que é criado por uma consulta são sempre Propriedades de chave. Quando `custs3` é executado no loop a seguir `For Each` , o resultado é uma coleção de instâncias de um tipo anônimo com duas propriedades de chave `Name` e `ID` .  
  
 [!code-vb[VbVbalrAnonymousTypes#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#33)]  
  
 Os elementos na coleção representados por `custs3` são fortemente tipados, e você pode usar o IntelliSense para navegar pelas propriedades disponíveis e para verificar seus tipos.  
  
 Para obter mais informações, consulte [introdução ao LINQ no Visual Basic](../linq/introduction-to-linq.md).  
  
## <a name="deciding-whether-to-use-anonymous-types"></a>Decidindo se os tipos anônimos devem ser usados  
 Antes de criar um objeto como uma instância de uma classe anônima, considere se essa é a melhor opção. Por exemplo, se você deseja criar um objeto temporário para conter dados relacionados, e não há necessidade de outros campos e métodos que uma classe completa possa conter, um tipo anônimo é uma boa solução. Os tipos anônimos também são convenientes se você quiser uma seleção diferente de propriedades para cada declaração ou se quiser alterar a ordem das propriedades. No entanto, se o seu projeto inclui vários objetos que têm as mesmas propriedades, em uma ordem fixa, você pode declará-los mais facilmente usando um tipo nomeado com um construtor de classe. Por exemplo, com um construtor apropriado, é mais fácil declarar várias instâncias de uma `Product` classe do que declarar várias instâncias de um tipo anônimo.  
  
 [!code-vb[VbVbalrAnonymousTypes#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#9)]  
  
 Outra vantagem dos tipos nomeados é que o compilador pode detectar um erro acidental de digitação de um nome de propriedade. Nos exemplos anteriores, `firstProd2` ,, `secondProd2` e `thirdProd2` devem ser instâncias do mesmo tipo anônimo. No entanto, se você declarasse acidentalmente `thirdProd2` uma das seguintes maneiras, seu tipo seria diferente daquele de `firstProd2` e `secondProd2` .  
  
 [!code-vb[VbVbalrAnonymousTypes#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#10)]  
  
 Mais importante, há limitações no uso de tipos anônimos que não se aplicam a instâncias de tipos nomeados. `firstProd2`, `secondProd2` e `thirdProd2` são instâncias do mesmo tipo anônimo. No entanto, o nome do tipo anônimo compartilhado não está disponível e não pode aparecer onde um nome de tipo é esperado em seu código. Por exemplo, um tipo anônimo não pode ser usado para definir uma assinatura de método, para declarar outra variável ou campo, ou em qualquer declaração de tipo. Como resultado, os tipos anônimos não são apropriados quando você precisa compartilhar informações entre os métodos.  
  
## <a name="an-anonymous-type-definition"></a>Uma definição de tipo anônimo  
 Em resposta à declaração de uma instância de um tipo anônimo, o compilador cria uma nova definição de classe que contém as propriedades especificadas.  
  
 Se o tipo anônimo contiver pelo menos uma propriedade de chave, a definição substituirá três membros herdados de <xref:System.Object> : <xref:System.Object.Equals%2A> , <xref:System.Object.GetHashCode%2A> e <xref:System.Object.ToString%2A> . O código produzido para testar a igualdade e determinar o valor do código hash considera apenas as propriedades de chave. Se o tipo anônimo não contiver nenhuma propriedade de chave, somente <xref:System.Object.ToString%2A> será substituído. As propriedades nomeadas explicitamente de um tipo anônimo não podem entrar em conflito com esses métodos gerados. Ou seja, você não pode usar `.Equals` , `.GetHashCode` ou `.ToString` para nomear uma propriedade.  
  
 Definições de tipo anônimo que têm pelo menos uma propriedade de chave também implementam a <xref:System.IEquatable%601?displayProperty=nameWithType> interface, em que `T` é o tipo do tipo anônimo.  
  
 Para obter mais informações sobre o código criado pelo compilador e a funcionalidade dos métodos substituídos, consulte [definição de tipo anônimo](anonymous-type-definition.md).  
  
## <a name="see-also"></a>Confira também

- [Inicializadores de objeto: tipos nomeados e anônimos](object-initializers-named-and-anonymous-types.md)
- [Inferência de Tipo de Variável Local](../variables/local-type-inference.md)
- [Introdução a LINQ no Visual Basic](../linq/introduction-to-linq.md)
- [Como inferir nomes e tipos de propriedade na declaração de tipo anônimo](how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)
- [Definição do Tipo Anônimo](anonymous-type-definition.md)
- [Chave](../../../language-reference/modifiers/key.md)
