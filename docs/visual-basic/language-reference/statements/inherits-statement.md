---
title: Instrução Inherits
ms.date: 07/20/2015
f1_keywords:
- vb.Inherits
- Inherits
helpviewer_keywords:
- Inherits statement [Visual Basic]
- Inherits statement [Visual Basic], syntax
ms.assetid: 9e6fe042-9af3-4341-8093-fc3537770cf2
ms.openlocfilehash: 5d88a01f90bc91a88229d19aa2368f8c71075b2f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404493"
---
# <a name="inherits-statement"></a>Instrução Inherits
Faz com que a classe ou a interface atual herde os atributos, variáveis, propriedades, procedimentos e eventos de outra classe ou conjunto de interfaces.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Inherits basetypenames  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`basetypenames`|Obrigatórios. O nome da classe da qual essa classe deriva.<br /><br /> -ou-<br /><br /> Os nomes das interfaces das quais essa interface deriva. Use vírgulas para separar vários nomes.|  
  
## <a name="remarks"></a>Comentários  
 Se usado, a `Inherits` instrução deve ser a primeira linha não em branco, que não seja de comentário em uma definição de classe ou de interface. Ele deve seguir imediatamente a `Class` `Interface` instrução ou.  
  
 Você pode usar `Inherits` somente em uma classe ou interface. Isso significa que o contexto de declaração para uma herança não pode ser um arquivo de origem, namespace, estrutura, módulo, procedimento ou bloco.  
  
## <a name="rules"></a>Regras  
  
- **Herança de classe.** Se uma classe usar a `Inherits` instrução, você poderá especificar apenas uma classe base.  
  
     Uma classe não pode herdar de uma classe aninhada dentro dela.  
  
- **Herança de interface.** Se uma interface usar a `Inherits` instrução, você poderá especificar uma ou mais interfaces base. Você pode herdar de duas interfaces, mesmo se cada uma delas definir um membro com o mesmo nome. Se você fizer isso, o código de implementação deverá usar a qualificação de nome para especificar qual membro está sendo implementado.  
  
     Uma interface não pode herdar de outra interface com um nível de acesso mais restritivo. Por exemplo, uma `Public` interface não pode herdar de uma `Friend` interface.  
  
     Uma interface não pode herdar de uma interface aninhada dentro dela.  
  
 Um exemplo de herança de classe no .NET Framework é a <xref:System.ArgumentException> classe, que é herdada da <xref:System.SystemException> classe. Isso fornece a <xref:System.ArgumentException> todas as propriedades predefinidas e os procedimentos exigidos pelas exceções do sistema, como a <xref:System.Exception.Message%2A> propriedade e o <xref:System.Exception.ToString%2A> método.  
  
 Um exemplo de herança de interface no .NET Framework é a <xref:System.Collections.ICollection> interface, que é herdada da <xref:System.Collections.IEnumerable> interface. Isso faz com que <xref:System.Collections.ICollection> o herde a definição do enumerador necessário para percorrer uma coleção.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a `Inherits` instrução para mostrar como uma classe denominada `thisClass` pode herdar todos os membros de uma classe base denominada `anotherClass` .  
  
 [!code-vb[VbVbalrStatements#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#37)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra a herança de várias interfaces.  
  
 [!code-vb[VbVbalrStatements#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#38)]  
  
 A interface chamada `thisInterface` agora inclui todas as definições no <xref:System.IComparable> , <xref:System.IDisposable> e <xref:System.IFormattable> as interfaces que os membros herdados fornecem, respectivamente, para comparação específica de tipo de dois objetos, liberando recursos alocados e expressando o valor de um objeto como um `String` . Uma classe que implementa o `thisInterface` deve implementar todos os membros de cada interface base.  
  
## <a name="see-also"></a>Confira também

- [MustInherit](../modifiers/mustinherit.md)
- [NotInheritable](../modifiers/notinheritable.md)
- [Objetos e classes](../../programming-guide/language-features/objects-and-classes/index.md)
- [Noções básicas de herança](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
- [Interfaces](../../programming-guide/language-features/interfaces/index.md)
