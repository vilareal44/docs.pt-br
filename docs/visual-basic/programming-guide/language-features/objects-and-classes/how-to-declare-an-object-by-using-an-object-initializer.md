---
title: Como declarar um objeto usando um inicializador de objeto
ms.date: 07/20/2015
helpviewer_keywords:
- declaring objects using object initializer
- object initializers [Visual Basic]
- initializers [Visual Basic]
- Video How tos, Visual Basic
ms.assetid: 0f53a553-efd6-466d-80bf-6b679e5cd174
ms.openlocfilehash: cf4954059a4b0bf015bed82a74357ecfd5f5987e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404869"
---
# <a name="how-to-declare-an-object-by-using-an-object-initializer-visual-basic"></a>Como declarar um objeto usando um inicializador de objeto (Visual Basic)
Inicializadores de objeto permitem declarar e instanciar uma instância de uma classe em uma única instrução. Além disso, você pode inicializar um ou mais membros da instância ao mesmo tempo, sem invocar um construtor com parâmetros.  
  
 Quando você usa um inicializador de objeto para criar uma instância de um tipo nomeado, o construtor sem parâmetros para a classe é chamado, seguido pela inicialização de membros designados na ordem especificada.  
  
 O procedimento a seguir mostra como criar uma instância de uma `Student` classe de três maneiras diferentes. A classe tem primeiro nome, sobrenome e propriedades de ano de classe, entre outras. Cada uma das três declarações cria uma nova instância de `Student` , com `First` a propriedade definida como "Michael", propriedade `Last` definida como "Tucker" e todos os outros membros definidos com seus valores padrão. O resultado de cada declaração no procedimento é equivalente ao exemplo a seguir, que não usa um inicializador de objeto.  
  
 [!code-vb[VbVbalrObjectInit#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class2.vb#20)]  
  
 Para uma implementação da `Student` classe, consulte [como: criar uma lista de itens](../../concepts/linq/how-to-create-a-list-of-items.md). Você pode copiar o código desse tópico para configurar a classe e criar uma lista de `Student` objetos com as quais trabalhar.  
  
### <a name="to-create-an-object-of-a-named-class-by-using-an-object-initializer"></a>Para criar um objeto de uma classe nomeada usando um inicializador de objeto  
  
1. Inicie a declaração como se você planejasse usar um construtor.  
  
     `Dim student1 As New Student`  
  
2. Digite a palavra-chave `With` , seguida de uma lista de inicialização entre chaves.  
  
     `Dim student1 As New Student With { <initialization list> }`  
  
3. Na lista inicialização, inclua cada propriedade que você deseja inicializar e atribua um valor inicial a ela. O nome da propriedade é precedido por um ponto.  
  
     [!code-vb[VbVbalrObjectInit#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class2.vb#21)]  
  
     Você pode inicializar um ou mais membros da classe.  
  
4. Como alternativa, você pode declarar uma nova instância da classe e, em seguida, atribuir um valor a ela. Primeiro, declare uma instância de `Student` :  
  
     `Dim student2 As Student`  
  
5. Comece a criação de uma instância do da `Student` maneira normal.  
  
     `Dim student2 As Student = New Student`  
  
6. Digite `With` e, em seguida, um inicializador de objeto para inicializar um ou mais membros da nova instância.  
  
     [!code-vb[VbVbalrObjectInit#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class2.vb#22)]  
  
7. Você pode simplificar a definição na etapa anterior omitindo `As Student` . Se você fizer isso, o compilador determinará que `student3` é uma instância do usando a `Student` inferência de tipos local.  
  
     [!code-vb[VbVbalrObjectInit#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class2.vb#23)]  
  
     Para obter mais informações, consulte [inferência de tipo local](../variables/local-type-inference.md).  
  
## <a name="see-also"></a>Confira também

- [Inferência de Tipo de Variável Local](../variables/local-type-inference.md)
- [Como: criar uma lista de itens](../../concepts/linq/how-to-create-a-list-of-items.md)
- [Inicializadores de objeto: tipos nomeados e anônimos](object-initializers-named-and-anonymous-types.md)
- [Tipos anônimos](anonymous-types.md)
