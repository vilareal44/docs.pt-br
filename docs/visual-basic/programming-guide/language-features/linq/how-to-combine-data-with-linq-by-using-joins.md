---
title: Como combinar dados a LINQ com junções
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], joins
- joins [LINQ in Visual Basic]
- Join clause [LINQ in Visual Basic]
- Group Join clause [Visual Basic]
- joining [LINQ in Visual Basic]
- queries [LINQ in Visual Basic], how-to topics
ms.assetid: 5b00a478-035b-41c6-8918-be1a97728396
ms.openlocfilehash: de8c4ec3ab8a0f2335c034231c661380420fd31b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404998"
---
# <a name="how-to-combine-data-with-linq-by-using-joins-visual-basic"></a>Como combinar dados a LINQ com junções (Visual Basic)
Visual Basic fornece as `Join` `Group Join` cláusulas de consulta e para permitir que você combine o conteúdo de várias coleções com base em valores comuns entre as coleções. Esses valores são conhecidos como valores de *chave* . Os desenvolvedores familiarizados com conceitos de banco de dados relacional reconhecerão a `Join` cláusula como uma junção interna e a `Group Join` cláusula como, efetivamente, uma junção externa esquerda.  
  
 Os exemplos neste tópico demonstram algumas maneiras de combinar dados usando as `Join` `Group Join` cláusulas de consulta e.  
  
## <a name="create-a-project-and-add-sample-data"></a>Criar um projeto e adicionar dados de exemplo  
  
#### <a name="to-create-a-project-that-contains-sample-data-and-types"></a>Para criar um projeto que contém dados de exemplo e tipos  
  
1. Para executar os exemplos neste tópico, abra o Visual Studio e adicione um novo projeto de aplicativo de console Visual Basic. Clique duas vezes no arquivo Module1. vb criado por Visual Basic.  
  
2. Os exemplos neste tópico usam os `Person` tipos e `Pet` dados do exemplo de código a seguir. Copie esse código no módulo padrão `Module1` criado por Visual Basic.  
  
     [!code-vb[VbLINQHowTos#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#1)]  
    [!code-vb[VbLINQHowTos#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#2)]  
  
## <a name="perform-an-inner-join-by-using-the-join-clause"></a>Executar uma junção interna usando a cláusula JOIN  
 Uma junção interna combina dados de duas coleções. Os itens para os quais os valores de chave especificados correspondem são incluídos. Todos os itens de uma das coleções que não têm um item correspondente na outra coleção são excluídos.  
  
 No Visual Basic, o LINQ fornece duas opções para executar uma junção interna: uma junção implícita e uma junção explícita.  
  
 Uma junção implícita especifica as coleções a serem unidas em uma `From` cláusula e identifica os campos de chave correspondentes em uma `Where` cláusula. Visual Basic une implicitamente as duas coleções com base nos campos de chave especificados.  
  
 Você pode especificar uma junção explícita usando a `Join` cláusula quando quiser ser específico sobre quais campos de chave usar na junção. Nesse caso, uma `Where` cláusula ainda pode ser usada para filtrar os resultados da consulta.  
  
#### <a name="to-perform-an-inner-join-by-using-the-join-clause"></a>Para executar uma junção interna usando a cláusula JOIN  
  
1. Adicione o seguinte código ao `Module1` módulo em seu projeto para ver exemplos de uma junção interna implícita e explícita.  
  
     [!code-vb[VbLINQHowTos#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#4)]  
  
## <a name="perform-a-left-outer-join-by-using-the-group-join-clause"></a>Executar uma junção externa esquerda usando a cláusula Group Join  
 Uma junção externa esquerda inclui todos os itens da coleção do lado esquerdo da junção e apenas valores correspondentes da coleção do lado direito da junção. Todos os itens da coleção do lado direito da junção que não têm um item correspondente na coleção do lado esquerdo são excluídos do resultado da consulta.  
  
 A `Group Join` cláusula executa, na verdade, uma junção externa esquerda. A diferença entre o que normalmente é conhecido como uma junção externa esquerda e o que a `Group Join` cláusula retorna é que a `Group Join` cláusula agrupa os resultados da coleção do lado direito da junção para cada item na coleção do lado esquerdo. Em um banco de dados relacional, uma junção externa esquerda retorna um resultado não agrupado no qual cada item no resultado da consulta contém itens correspondentes de ambas as coleções na junção. Nesse caso, os itens da coleção do lado esquerdo da junção são repetidos para cada item correspondente da coleção do lado direito. Você verá como isso se parece quando você conclui o próximo procedimento.  
  
 Você pode recuperar os resultados de uma `Group Join` consulta como um resultado não agrupado estendendo sua consulta para retornar um item para cada resultado de consulta agrupado. Para fazer isso, você precisa garantir que você consulte o `DefaultIfEmpty` método da coleção agrupada. Isso garante que os itens da coleção do lado esquerdo da junção ainda estejam incluídos no resultado da consulta, mesmo que não tenham resultados correspondentes da coleção do lado direito. Você pode adicionar código à sua consulta para fornecer um valor de resultado padrão quando não houver nenhum valor correspondente da coleção do lado direito da junção.  
  
#### <a name="to-perform-a-left-outer-join-by-using-the-group-join-clause"></a>Para executar uma junção externa esquerda usando a cláusula Group Join  
  
1. Adicione o código a seguir ao `Module1` módulo em seu projeto para ver exemplos de uma junção externa agrupada à esquerda e uma junção externa esquerda desagrupada.  
  
     [!code-vb[VbLINQHowTos#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#3)]  
  
## <a name="perform-a-join-by-using-a-composite-key"></a>Executar uma junção usando uma chave composta  
 Você pode usar a `And` palavra-chave em uma `Join` `Group Join` cláusula or para identificar vários campos de chave a serem usados ao corresponder valores das coleções que estão sendo Unidas. A `And` palavra-chave especifica que todos os campos de chave especificados devem corresponder aos itens a serem associados.  
  
#### <a name="to-perform-a-join-by-using-a-composite-key"></a>Para executar uma junção usando uma chave composta  
  
1. Adicione o seguinte código ao `Module1` módulo em seu projeto para ver exemplos de uma junção que usa uma chave composta.  
  
     [!code-vb[VbLINQHowTos#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#5)]  
  
## <a name="run-the-code"></a>executar o código  
  
#### <a name="to-add-code-to-run-the-examples"></a>Para adicionar código para executar os exemplos  
  
1. Substitua o `Sub Main` no `Module1` módulo em seu projeto pelo código a seguir para executar os exemplos neste tópico.  
  
     [!code-vb[VbLINQHowTos#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#6)]  
  
2. Pressione F5 para executar os exemplos.  
  
## <a name="see-also"></a>Confira também

- [LINQ](index.md)
- [Introdução a LINQ no Visual Basic](introduction-to-linq.md)
- [Cláusula JOIN](../../../language-reference/queries/join-clause.md)
- [Cláusula Group Join](../../../language-reference/queries/group-join-clause.md)
- [Cláusula from](../../../language-reference/queries/from-clause.md)
- [Cláusula WHERE](../../../language-reference/queries/where-clause.md)
- [Consultas](../../../language-reference/queries/index.md)
- [Transformações de dados com LINQ (C#)](../../../../csharp/programming-guide/concepts/linq/data-transformations-with-linq.md)
