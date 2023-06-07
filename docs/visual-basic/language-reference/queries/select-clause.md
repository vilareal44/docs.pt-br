---
title: Cláusula Select
ms.date: 07/20/2015
f1_keywords:
- vb.QuerySelect
helpviewer_keywords:
- Select statement [Visual Basic]
- Select clause [Visual Basic]
- queries [Visual Basic], Select
ms.assetid: 27a3f61c-5960-4692-9b91-4d0c4b6178fe
ms.openlocfilehash: a909b1d79b10f82ece03bab788ae889c64b27124
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359688"
---
# <a name="select-clause-visual-basic"></a>Cláusula Select (Visual Basic)
Define o resultado de uma consulta.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Select [ var1 = ] fieldName1 [, [ var2 = ] fieldName2 [...] ]  
```  
  
## <a name="parts"></a>Partes  
 `var1`  
 Opcional. Um alias que pode ser usado para referenciar os resultados da expressão de coluna.  
  
 `fieldName1`  
 Obrigatórios. O nome do campo a ser retornado no resultado da consulta.  
  
## <a name="remarks"></a>Comentários  
 Você pode usar a `Select` cláusula para definir os resultados a serem retornados de uma consulta. Isso permite que você defina os membros de um novo tipo anônimo que é criado por uma consulta ou para direcionar os membros de um tipo nomeado que é retornado por uma consulta. A `Select` cláusula não é necessária para uma consulta. Se nenhuma `Select` cláusula for especificada, a consulta retornará um tipo com base em todos os membros das variáveis de intervalo identificadas para o escopo atual. Para obter mais informações, consulte [Tipos Anônimos](../../programming-guide/language-features/objects-and-classes/anonymous-types.md). Quando uma consulta cria um tipo nomeado, ela retornará um resultado do tipo <xref:System.Collections.Generic.IEnumerable%601> onde `T` é o tipo criado.  
  
 A `Select` cláusula pode fazer referência a qualquer variável no escopo atual. Isso inclui variáveis de intervalo identificadas na `From` cláusula (ou `From` cláusulas). Ele também inclui todas as novas variáveis criadas com um alias pelas `Aggregate` `Let` `Group By` cláusulas,,, ou `Group Join` variáveis de uma `Select` cláusula Previous na expressão de consulta. A `Select` cláusula também pode incluir valores estáticos. Por exemplo, o exemplo de código a seguir mostra uma expressão de consulta na qual a `Select` cláusula define o resultado da consulta como um novo tipo anônimo com quatro membros: `ProductName` ,, `Price` `Discount` e `DiscountedPrice` . Os `ProductName` `Price` valores de membro e são obtidos da variável de intervalo de produtos que é definida na `From` cláusula. O `DiscountedPrice` valor do membro é calculado na `Let` cláusula. O `Discount` membro é um valor estático.  
  
 [!code-vb[VbSimpleQuerySamples#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#27)]  
  
 A `Select` cláusula apresenta um novo conjunto de variáveis de intervalo para cláusulas de consulta subsequentes, e as variáveis de intervalo anteriores não estão mais no escopo. A última `Select` cláusula em uma expressão de consulta determina o valor de retorno da consulta. Por exemplo, a consulta a seguir retorna o nome da empresa e a ID do pedido para cada pedido de cliente para o qual o total excede 500. A primeira `Select` cláusula identifica as variáveis de intervalo para a `Where` cláusula e a segunda `Select` cláusula. A segunda `Select` cláusula identifica os valores retornados pela consulta como um novo tipo anônimo.  
  
 [!code-vb[VbSimpleQuerySamples#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#28)]  
  
 Se a `Select` cláusula identificar um único item a ser retornado, a expressão de consulta retornará uma coleção do tipo desse item único. Se a `Select` cláusula identificar vários itens a serem retornados, a expressão de consulta retornará uma coleção de um novo tipo anônimo, com base nos itens selecionados. Por exemplo, as duas consultas a seguir retornam coleções de dois tipos diferentes com base na `Select` cláusula. A primeira consulta retorna uma coleção de nomes de empresa como cadeias de caracteres. A segunda consulta retorna uma coleção de `Customer` objetos preenchida com os nomes de empresa e informações de endereço.  
  
 [!code-vb[VbSimpleQuerySamples#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#29)]  
  
## <a name="example"></a>Exemplo  
 A expressão de consulta a seguir usa uma `From` cláusula para declarar uma variável `cust` de intervalo para a `customers` coleção. A `Select` cláusula seleciona o nome do cliente e o valor da ID e popula as `CompanyName` `CustomerID` colunas e da nova variável de intervalo. A `For Each` instrução faz um loop sobre cada objeto retornado e exibe `CompanyName` as `CustomerID` colunas e para cada registro.  
  
 [!code-vb[VbSimpleQuerySamples#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#30)]  
  
## <a name="see-also"></a>Confira também

- [Introdução a LINQ no Visual Basic](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [Consultas](index.md)
- [Cláusula from](from-clause.md)
- [Cláusula WHERE](where-clause.md)
- [Cláusula Order By](order-by-clause.md)
- [Tipos anônimos](../../programming-guide/language-features/objects-and-classes/anonymous-types.md)
