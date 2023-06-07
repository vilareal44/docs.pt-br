---
title: Relacionamentos de tipo em operações de consulta LINQ (C#)
description: Saiba como os tipos de variáveis em uma consulta LINQ se relacionam entre si. As operações de consulta LINQ são fortemente tipadas na fonte de dados, na consulta e na execução.
ms.date: 07/20/2015
helpviewer_keywords:
- inferring type information [LINQ in C#]
- data sources [LINQ in C#], type relationships
- queries [LINQ in C#], type relationships
- relationships [LINQ in C#]
- type relationships [LINQ in C#]
- variable relationships [LINQ in C#]
- type information inferred [LINQ in C#]
- data transformations [LINQ in C#]
- LINQ [C#], type relationships
ms.assetid: 99118938-d47c-4d7e-bb22-2657a9f95268
ms.openlocfilehash: 20f0b37a156e3b3f9c63f14cb83d678d26f685ee
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302276"
---
# <a name="type-relationships-in-linq-query-operations-c"></a>Relacionamentos de tipo em operações de consulta LINQ (C#)
Para escrever consultas com eficiência, você precisa entender como os tipos de variáveis em uma operação de consulta completa se relacionam entre si. Se você entender essas relações, compreenderá mais facilmente os exemplos de código e exemplos de LINQ na documentação. Além disso, você compreenderá o que ocorre nos bastidores quando variáveis são tipadas de forma implícita usando `var`.  
  
 As operações de consulta LINQ são fortemente tipadas na fonte de dados, na própria consulta e na execução da consulta. O tipo das variáveis na consulta deve ser compatível com o tipo dos elementos na fonte de dados e com o tipo da variável de iteração na instrução `foreach`. Essa tipagem forte garante que erros de tipo sejam capturados em tempo de compilação, quando podem ser corrigidos antes que os usuários os encontrem.  
  
 Para demonstrar essas relações de tipo, a maioria dos exemplos a seguir usam tipagem explícita para todas as variáveis. O último exemplo mostra como os mesmos princípios se aplicam mesmo quando você usa tipagem implícita usando [var](../../../language-reference/keywords/var.md).  
  
## <a name="queries-that-do-not-transform-the-source-data"></a>Consultas que não transformam os dados de origem  
 A ilustração a seguir mostra uma operação de consulta LINQ to Objects que não executa transformações nos dados. A fonte contém uma sequência de cadeias de caracteres e a saída da consulta também é uma sequência de cadeias de caracteres.  
  
 ![Diagrama que mostra a relação dos tipos de dados em uma consulta LINQ.](./media/type-relationships-in-linq-query-operations/linq-query-data-type-relation.png)  
  
1. O argumento de tipo da fonte de dados determina o tipo da variável de intervalo.  
  
2. O tipo do objeto selecionado determina o tipo da variável de consulta. Esta é uma cadeia de caracteres `name`. Portanto, a variável de consulta é um `IEnumerable<string>`.  
  
3. A variável de consulta é iterada na instrução `foreach`. Como a variável de consulta é uma sequência de cadeias de caracteres, a variável de iteração também é uma cadeia de caracteres.  
  
## <a name="queries-that-transform-the-source-data"></a>Consultas que transformam os dados de origem  
 A ilustração a seguir mostra uma operação de consulta de [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] que executa transformações simples nos dados. A consulta usa uma sequência de objetos `Customer` como entrada e seleciona somente a propriedade `Name` no resultado. Como `Name` é uma cadeia de caracteres, a consulta produz uma sequência de cadeias de caracteres como saída.  
  
 ![Diagrama que mostra uma consulta que transforma o tipo de dados.](./media/type-relationships-in-linq-query-operations/linq-query-transform-data-type.png)  
  
1. O argumento de tipo da fonte de dados determina o tipo da variável de intervalo.  
  
2. A instrução `select` retorna a propriedade `Name` em vez do objeto `Customer` completo. Como `Name` é uma cadeia de caracteres, o argumento de tipo de `custNameQuery` é `string` e não `Customer`.  
  
3. Como `custNameQuery` é uma sequência de cadeias de caracteres, a variável de iteração do loop `foreach` também deve ser um `string`.  
  
 A ilustração a seguir mostra uma transformação um pouco mais complexa. A instrução `select` retorna um tipo anônimo que captura apenas dois membros do objeto `Customer` original.  
  
 ![Diagrama que mostra uma consulta mais complexa que transforma o tipo de dados.](./media/type-relationships-in-linq-query-operations/linq-complex-query-transform-data-type.png)  
  
1. O argumento de tipo da fonte de dados sempre é o tipo da variável de intervalo na consulta.  
  
2. Como a instrução `select` produz um tipo anônimo, a variável de consulta deve ser tipada implicitamente usando `var`.  
  
3. Como o tipo da variável de consulta é implícito, a variável de iteração no loop `foreach` também deve ser implícito.  
  
## <a name="letting-the-compiler-infer-type-information"></a>Deixando o compilador inferir informações de tipo  
 Embora você precise entender as relações de tipo em uma operação de consulta, você tem a opção de permitir que o compilador fazer todo o trabalho. A palavra-chave [var](../../../language-reference/keywords/var.md) pode ser usada para qualquer variável local em uma operação de consulta. A ilustração a seguir é semelhante ao exemplo número 2 que foi discutido anteriormente. No entanto, o compilador fornece o tipo forte para cada variável na operação de consulta.  
  
 ![Diagrama que mostra o fluxo de tipo com tipagem implícita.](./media/type-relationships-in-linq-query-operations/linq-type-flow-implicit-typing.png)  
  
 Para obter mais informações sobre `var`, consulte [Variáveis de local digitadas implicitamente](../../classes-and-structs/implicitly-typed-local-variables.md).  
