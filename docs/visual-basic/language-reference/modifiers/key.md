---
title: Chave
ms.date: 07/20/2015
f1_keywords:
- vb.AnonymousKey
helpviewer_keywords:
- anonymous types [Visual Basic], key
- Key [Visual Basic]
- Key keyword [Visual Basic]
ms.assetid: 7697a928-7d14-4430-a72a-c9e96e8d6c11
ms.openlocfilehash: 5b060f5fa0042dfb8ffa6876f5e172d3bcda67a3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396214"
---
# <a name="key-visual-basic"></a>Chave (Visual Basic)
A `Key` palavra-chave permite que você especifique o comportamento das propriedades de tipos anônimos. Somente as propriedades que você designa como propriedades de chave participam de testes de igualdade entre instâncias de tipo anônimo ou o cálculo de valores de código hash. Os valores das propriedades de chave não podem ser alterados.  
  
 Você designa uma propriedade de um tipo anônimo como uma propriedade de chave colocando a palavra-chave `Key` na frente de sua declaração na lista de inicialização. No exemplo a seguir, `Airline` e `FlightNo` são propriedades de chave, mas `Gate` não são.  
  
 [!code-vb[VbVbalrAnonymousTypes#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#26)]  
  
 Quando um novo tipo anônimo é criado, ele herda diretamente do <xref:System.Object> . O compilador substitui três membros herdados: <xref:System.Object.Equals%2A> , <xref:System.Object.GetHashCode%2A> e <xref:System.Object.ToString%2A> . O código de substituição que é produzido para <xref:System.Object.Equals%2A> e <xref:System.Object.GetHashCode%2A> é baseado em Propriedades de chave. Se não houver propriedades de chave no tipo <xref:System.Object.GetHashCode%2A> e <xref:System.Object.Equals%2A> não forem substituídas.  
  
## <a name="equality"></a>Igualitário  
 Duas instâncias de tipo anônimo são iguais se forem instâncias do mesmo tipo e se os valores de suas propriedades de chave forem iguais. Nos exemplos a seguir, `flight2` é igual ao `flight1` do exemplo anterior porque eles são instâncias do mesmo tipo anônimo e têm valores correspondentes para suas propriedades de chave. No entanto, `flight3` o não é igual a `flight1` porque ele tem um valor diferente para uma propriedade de chave, `FlightNo` . `flight4`A instância não é do mesmo tipo como `flight1` porque designa propriedades diferentes como propriedades de chave.  
  
 [!code-vb[VbVbalrAnonymousTypes#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#27)]  
  
 Se duas instâncias forem declaradas com apenas propriedades não chave, idênticas em nome, tipo, ordem e valor, as duas instâncias não serão iguais. Uma instância sem propriedades de chave é igual apenas a ela mesma.  
  
 Para obter mais informações sobre as condições sob as quais duas instâncias de tipo anônimo são instâncias do mesmo tipo anônimo, consulte [tipos anônimos](../../programming-guide/language-features/objects-and-classes/anonymous-types.md).  
  
## <a name="hash-code-calculation"></a>Cálculo de código hash  
 Assim como <xref:System.Object.Equals%2A> , a função de hash definida em <xref:System.Object.GetHashCode%2A> para um tipo anônimo é baseada nas propriedades de chave do tipo. Os exemplos a seguir mostram a interação entre as propriedades de chave e os valores de código hash.  
  
 As instâncias de um tipo anônimo que têm os mesmos valores para todas as propriedades de chave têm o mesmo valor de código de hash, mesmo se as propriedades não chave não tiverem valores correspondentes. A instrução a seguir retorna `True`.  
  
 [!code-vb[VbVbalrAnonymousTypes#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#37)]  
  
 Instâncias de um tipo anônimo que têm valores diferentes para uma ou mais propriedades de chave têm valores de código hash diferentes. A instrução a seguir retorna `False`.  
  
 [!code-vb[VbVbalrAnonymousTypes#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#38)]  
  
 Instâncias de tipos anônimos que designam propriedades diferentes como propriedades de chave não são instâncias do mesmo tipo. Eles têm valores de código hash diferentes, mesmo quando os nomes e valores de todas as propriedades são os mesmos. A instrução a seguir retorna `False`.  
  
 [!code-vb[VbVbalrAnonymousTypes#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#39)]  
  
## <a name="read-only-values"></a>Valores somente leitura  
 Os valores das propriedades de chave não podem ser alterados. Por exemplo, no `flight1` nos exemplos anteriores, os `Airline` campos e `FlightNo` são somente leitura, mas `Gate` podem ser alterados.  
  
 [!code-vb[VbVbalrAnonymousTypes#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#28)]  
  
## <a name="see-also"></a>Confira também

- [Definição do Tipo Anônimo](../../programming-guide/language-features/objects-and-classes/anonymous-type-definition.md)
- [Como inferir nomes e tipos de propriedade na declaração de tipo anônimo](../../programming-guide/language-features/objects-and-classes/how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)
- [Tipos anônimos](../../programming-guide/language-features/objects-and-classes/anonymous-types.md)
