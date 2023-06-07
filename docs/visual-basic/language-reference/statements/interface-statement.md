---
title: Instrução Interface
ms.date: 05/12/2018
f1_keywords:
- vb.Interface
helpviewer_keywords:
- interface statement [Visual Basic]
- interfaces [Visual Basic], interface definition
ms.assetid: 8997af73-bda3-4f79-bd41-ca396b610260
ms.openlocfilehash: 02d258084aaaa53dcc559cfaa0dec27556351037
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404480"
---
# <a name="interface-statement-visual-basic"></a>Instrução Interface (Visual Basic)
Declara o nome de uma interface e apresenta as definições dos membros que a interface compreende.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
[ <attributelist> ] [ accessmodifier ] [ Shadows ] _  
Interface name [ ( Of typelist ) ]  
    [ Inherits interfacenames ]  
    [ [ modifiers ] Property membername ]  
    [ [ modifiers ] Function membername ]  
    [ [ modifiers ] Sub membername ]  
    [ [ modifiers ] Event membername ]  
    [ [ modifiers ] Interface membername ]  
    [ [ modifiers ] Class membername ]  
    [ [ modifiers ] Structure membername ]  
End Interface  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`attributelist`|Opcional. Consulte a [lista de atributos](attribute-list.md).|  
|`accessmodifier`|Opcional. Pode ser um dos seguintes:<br /><br /> -   [Publicada](../modifiers/public.md)<br />-   [Protected](../modifiers/protected.md)<br />-   [Público](../modifiers/friend.md)<br />-   [Pessoal](../modifiers/private.md)<br />-  [Amigo protegido](../modifiers/protected-friend.md)<br/>- [Particular protegido](../modifiers/private-protected.md)<br /><br /> Consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).|  
|`Shadows`|Opcional. Consulte [Shadows](../modifiers/shadows.md).|  
|`name`|Obrigatórios. Nome desta interface. Consulte [nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md).|  
|`Of`|Opcional. Especifica que esta é uma interface genérica.|  
|`typelist`|Necessário se você usar a palavra-chave [of](of-clause.md) . Lista de parâmetros de tipo para esta interface. Opcionalmente, cada parâmetro de tipo pode ser declarado como Variant usando os `In` `Out` modificadores genérico. Consulte [lista de tipos](type-list.md).|  
|`Inherits`|Opcional. Indica que essa interface herda os atributos e membros de outra interface ou interfaces. Consulte a [instrução Inherits](inherits-statement.md).|  
|`interfacenames`|Necessário se você usar a `Inherits` instrução. Os nomes das interfaces das quais essa interface deriva.|  
|`modifiers`|Opcional. Modificadores apropriados para o membro da interface que está sendo definido.|  
|`Property`|Opcional. Define uma propriedade que é um membro da interface.|  
|`Function`|Opcional. Define um `Function` procedimento que é um membro da interface.|  
|`Sub`|Opcional. Define um `Sub` procedimento que é um membro da interface.|  
|`Event`|Opcional. Define um evento que é um membro da interface.|  
|`Interface`|Opcional. Define uma interface que é aninhada dentro desta interface. A definição de interface aninhada deve terminar com uma `End Interface` instrução.|  
|`Class`|Opcional. Define uma classe que é membro da interface. A definição de classe de membro deve terminar com uma `End Class` instrução.|  
|`Structure`|Opcional. Define uma estrutura que é membro da interface. A definição da estrutura do membro deve terminar com uma `End Structure` instrução.|  
|`membername`|Necessário para cada propriedade, procedimento, evento, interface, classe ou estrutura definida como um membro da interface. O nome do membro.|  
|`End Interface`|Encerra a `Interface` definição.|  
  
## <a name="remarks"></a>Comentários  
 Uma *interface* define um conjunto de membros, como propriedades e procedimentos, que as classes e estruturas podem implementar. A interface define apenas as assinaturas dos membros e não seus trabalhos internos.  
  
 Uma classe ou estrutura implementa a interface fornecendo o código para cada membro definido pela interface. Por fim, quando o aplicativo cria uma instância dessa classe ou estrutura, um objeto existe e é executado na memória. Para obter mais informações, consulte [objetos e classes](../../programming-guide/language-features/objects-and-classes/index.md) e [interfaces](../../programming-guide/language-features/interfaces/index.md).  
  
 Você pode usar `Interface` somente no nível de namespace ou de módulo. Isso significa que o *contexto de declaração* para uma interface deve ser um arquivo de origem, namespace, classe, estrutura, módulo ou interface e não pode ser um procedimento ou bloco. Para obter mais informações, consulte [Contextos de declaração e níveis de acesso padrão](declaration-contexts-and-default-access-levels.md).  
  
 Interfaces padrão para acesso [Friend](../modifiers/friend.md) . Você pode ajustar seus níveis de acesso com os modificadores de acesso. Para obter mais informações, consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).  
  
## <a name="rules"></a>Regras  
  
- **Aninhando interfaces.** Você pode definir uma interface dentro de outra. A interface externa é chamada de *interface que a contém*e a interface interna é chamada de *interface aninhada*.  
  
- **Declaração de membro.** Quando você declara uma propriedade ou um procedimento como membro de uma interface, você está definindo apenas a *assinatura* dessa propriedade ou procedimento. Isso inclui o tipo de elemento (Propriedade ou procedimento), seus parâmetros e tipos de parâmetro e seu tipo de retorno. Por isso, a definição de membro usa apenas uma linha de código e encerrando instruções como `End Function` ou `End Property` não são válidas em uma interface.  
  
     Por outro lado, quando você define uma enumeração ou uma estrutura, ou uma classe ou interface aninhada, é necessário incluir seus membros de dados.  
  
- **Modificadores de membro.** Você não pode usar nenhum modificador de acesso ao definir membros de módulo, nem pode especificar qualquer modificador [compartilhado](../modifiers/shared.md) ou de procedimento, exceto [sobrecargas](../modifiers/overloads.md). Você pode declarar qualquer membro com [Shadows](../modifiers/shadows.md)e pode usar [Default](../modifiers/default.md) ao definir uma propriedade, bem como [ReadOnly](../modifiers/readonly.md) ou [WriteOnly](../modifiers/writeonly.md).  
  
- **Herda.** Se a interface usar a [instrução Inherits](inherits-statement.md), você poderá especificar uma ou mais interfaces base. Você pode herdar de duas interfaces, mesmo se cada uma delas definir um membro com o mesmo nome. Se você fizer isso, o código de implementação deverá usar a qualificação de nome para especificar qual membro está sendo implementado.  
  
     Uma interface não pode herdar de outra interface com um nível de acesso mais restritivo. Por exemplo, uma `Public` interface não pode herdar de uma `Friend` interface.  
  
     Uma interface não pode herdar de uma interface aninhada dentro dela.  
  
- **Implementação.** Quando uma classe usa a instrução [Implements](implements-clause.md) para implementar essa interface, ela deve implementar todos os membros definidos na interface. Além disso, cada assinatura no código de implementação deve corresponder exatamente à assinatura correspondente definida nesta interface. No entanto, o nome do membro no código de implementação não precisa corresponder ao nome do membro conforme definido na interface.  
  
     Quando uma classe está implementando um procedimento, ela não pode designar o procedimento como `Shared` .  
  
- **Propriedade padrão.** Uma interface pode especificar, no máximo, uma propriedade como sua *propriedade padrão*, que pode ser referenciada sem usar o nome da propriedade. Você especifica tal propriedade declarando-a com o modificador [padrão](../modifiers/default.md) .  
  
     Observe que isso significa que uma interface pode definir uma propriedade padrão somente se ela não herda nenhuma.  
  
## <a name="behavior"></a>Comportamento  
  
- **Nível de acesso.** Todos os membros de interface implicitamente têm acesso [público](../modifiers/public.md) . Você não pode usar nenhum modificador de acesso ao definir um membro. No entanto, uma classe que implementa a interface pode declarar um nível de acesso para cada membro implementado.  
  
     Se você atribuir uma instância de classe a uma variável, o nível de acesso de seus membros poderá depender se o tipo de dados da variável é a interface subjacente ou a classe de implementação. O exemplo a seguir ilustra isto.  
  
     [!code-vb[VbVbalrStatements#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#39)]  
  
     Se você acessar membros da classe por meio do `varAsInterface` , todos eles terão acesso público. No entanto, se você acessar membros por meio `varAsClass` do, o `Sub` procedimento `doSomething` terá acesso privado.  
  
- **Com.** Uma interface está no escopo em todo o namespace, classe, estrutura ou módulo.  
  
     O escopo de cada membro de interface é a interface inteira.  
  
- **Existência.** Uma interface não tem, ela própria, um tempo de vida, nem seus membros. Quando uma classe implementa uma interface e um objeto é criado como uma instância dessa classe, o objeto tem um tempo de vida dentro do aplicativo no qual está sendo executado. Para obter mais informações, consulte "Lifetime" na [declaração de classe](class-statement.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a `Interface` instrução para definir uma interface chamada `thisInterface` , que deve ser implementada com uma `Property` instrução e uma `Function` instrução.  
  
 [!code-vb[VbVbalrStatements#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#40)]  
  
 Observe que as `Property` instruções e não `Function` introduzem blocos que terminam com `End Property` e `End Function` dentro da interface. A interface define somente as assinaturas de seus membros. Os `Property` blocos Full e The `Function` são exibidos em uma classe que implementa `thisInterface` .  
  
## <a name="see-also"></a>Confira também

- [Interfaces](../../programming-guide/language-features/interfaces/index.md)
- [Instrução Class](class-statement.md)
- [Instrução Module](module-statement.md)
- [Instrução Structure](structure-statement.md)
- [Instrução Property](property-statement.md)
- [Instrução Function](function-statement.md)
- [Instrução Sub](sub-statement.md)
- [Tipos genéricos no Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [Variação em interfaces genéricas](../../programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)
- [Em](../modifiers/in-generic-modifier.md)
- [Fora](../modifiers/out-generic-modifier.md)
