---
title: Parcial
ms.date: 07/20/2015
f1_keywords:
- vb.Partial
- partial
helpviewer_keywords:
- structures [Visual Basic], partial
- classes [Visual Basic]
- partial, types [Visual Basic]
- partial, structures
- partial, classes [Visual Basic]
- classes [Visual Basic], partial
- Partial keyword [Visual Basic]
- type promotion
ms.assetid: 7adaef80-f435-46e1-970a-269fff63b448
ms.openlocfilehash: 650ead2f0deb9813b26241a6a4676907de3f263d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84362236"
---
# <a name="partial-visual-basic"></a>Parcial (Visual Basic)
Indica que uma declaração de tipo é uma definição parcial do tipo.  
  
 Você pode dividir a definição de um tipo entre várias declarações usando a `Partial` palavra-chave. Você pode usar quantas declarações parciais desejar, em quantos arquivos de origem diferentes desejar. No entanto, todas as declarações devem estar no mesmo assembly e no mesmo namespace.  
  
> [!NOTE]
> O Visual Basic dá suporte a *métodos parciais*, que normalmente são implementados em classes parciais. Para obter mais informações, consulte [partial Methods](../../programming-guide/language-features/procedures/partial-methods.md) e [sub Statement](../statements/sub-statement.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
[ <attrlist> ] [ accessmodifier ] [ Shadows ] [ MustInherit | NotInheritable ] _  
Partial { Class | Structure | Interface | Module } name [ (Of typelist) ]  
    [ Inherits classname ]  
    [ Implements interfacenames ]  
    [ variabledeclarations ]  
    [ proceduredeclarations ]  
{ End Class | End Structure }  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`attrlist`|Opcional. Lista de atributos que se aplicam a este tipo. Você deve colocar a [lista de atributos](../statements/attribute-list.md) entre colchetes angulares ( `< >` ).|  
|`accessmodifier`|Opcional. Especifica qual código pode acessar este tipo. Consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).|  
|`Shadows`|Opcional. Consulte [Shadows](shadows.md).|  
|`MustInherit`|Opcional. Consulte [MustInherit](mustinherit.md).|  
|`NotInheritable`|Opcional. Consulte [NotInheritable](notinheritable.md).|  
|`name`|Obrigatórios. Nome deste tipo. Deve corresponder ao nome definido em todas as outras declarações parciais do mesmo tipo.|  
|`Of`|Opcional. Especifica que este é um tipo genérico. Consulte [tipos genéricos em Visual Basic](../../programming-guide/language-features/data-types/generic-types.md).|  
|`typelist`|Necessário se você usar [do](../statements/of-clause.md). Consulte [lista de tipos](../statements/type-list.md).|  
|`Inherits`|Opcional. Consulte a [instrução Inherits](../statements/inherits-statement.md).|  
|`classname`|Necessário se você usar `Inherits` . O nome da classe ou interface da qual essa classe deriva.|  
|`Implements`|Opcional. Consulte a [instrução Implements](../statements/implements-statement.md).|  
|`interfacenames`|Necessário se você usar `Implements` . Os nomes das interfaces que esse tipo implementa.|  
|`variabledeclarations`|Opcional. Instruções que declaram variáveis e eventos adicionais para o tipo.|  
|`proceduredeclarations`|Opcional. Instruções que declaram e definem procedimentos adicionais para o tipo.|  
|`End Class` ou `End Structure`|Finaliza essa `Class` definição ou parcial `Structure` .|  
  
## <a name="remarks"></a>Comentários  
 Visual Basic usa definições de classe parcial para separar o código gerado do código criado pelo usuário em arquivos de origem separados. Por exemplo, o **Windows Form Designer** define classes parciais para controles, como <xref:System.Windows.Forms.Form>. Você não deve modificar o código gerado nesses controles.  
  
 Todas as regras para classe, estrutura, interface e criação de módulo, como aquelas para uso e herança de modificador, se aplicam ao criar um tipo parcial.  
  
## <a name="best-practices"></a>Práticas Recomendadas  
  
- Em circunstâncias normais, você não deve dividir o desenvolvimento de um único tipo entre duas ou mais declarações. Portanto, na maioria dos casos, você não precisa da `Partial` palavra-chave.  
  
- Para facilitar a leitura, todas as declarações parciais de um tipo devem incluir a `Partial` palavra-chave. O compilador permite no máximo uma declaração parcial para omitir a palavra-chave; se dois ou mais omitirem, o compilador sinalizará um erro.  
  
## <a name="behavior"></a>Comportamento  
  
- **União de declarações.** O compilador trata o tipo como a União de todas as suas declarações parciais. Cada modificador de cada definição parcial se aplica a todo o tipo e todos os membros de todas as definições parciais estão disponíveis para todo o tipo.  
  
- **Promoção de tipos não permitida para tipos parciais em módulos.** Se uma definição parcial estiver dentro de um módulo, a promoção de tipos desse tipo será automaticamente derrotada. Nesse caso, um conjunto de definições parciais pode causar resultados inesperados e até mesmo erros de compilador. Para obter mais informações, consulte [promoção de tipos](../../programming-guide/language-features/declared-elements/type-promotion.md).  
  
     O compilador mescla definições parciais somente quando seus caminhos totalmente qualificados são idênticos.  
  
 A `Partial` palavra-chave pode ser usada nesses contextos:  
  
 [Instrução Class](../statements/class-statement.md)  
  
 [Instrução Structure](../statements/structure-statement.md)  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir divide a definição da classe `sampleClass` em duas declarações, cada uma delas define um `Sub` procedimento diferente.  
  
 [!code-vb[VbVbalrKeywords#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#3)]  
  
 As duas definições parciais no exemplo anterior podem estar no mesmo arquivo de origem ou em dois arquivos de origem diferentes.  
  
## <a name="see-also"></a>Confira também

- [Instrução Class](../statements/class-statement.md)
- [Instrução Structure](../statements/structure-statement.md)
- [Tipo de promoção](../../programming-guide/language-features/declared-elements/type-promotion.md)
- [Sombras](shadows.md)
- [Tipos genéricos no Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [Métodos Parciais](../../programming-guide/language-features/procedures/partial-methods.md)
