---
title: Instrução Delegate
ms.date: 07/20/2015
f1_keywords:
- vb.Delegate
helpviewer_keywords:
- delegate keyword [Visual Basic]
- Delegate statement [Visual Basic]
ms.assetid: f799c518-0817-40cc-ad0b-4da846fdba57
ms.openlocfilehash: 8dec28620b0409f05007b2c0b1c1fd4494c2d7c8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404752"
---
# <a name="delegate-statement"></a>Instrução Delegate
Usado para declarar um delegado. Um delegado é um tipo de referência que se refere a um `Shared` método de um tipo ou a um método de instância de um objeto. Qualquer procedimento com parâmetros correspondentes e tipos de retorno pode ser usado para criar uma instância dessa classe delegate. O procedimento pode ser posteriormente invocado por meio da instância de delegado.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
[ <attrlist> ] [ accessmodifier ] _  
[ Shadows ] Delegate [ Sub | Function ] name [( Of typeparamlist )] [([ parameterlist ])] [ As type ]  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`attrlist`|Opcional. Lista de atributos que se aplicam a este delegado. Vários atributos são separados por vírgulas. Você deve colocar a [lista de atributos](attribute-list.md) entre colchetes angulares (" `<` " e " `>` ").|  
|`accessmodifier`|Opcional. Especifica qual código pode acessar o delegado. Pode ser um dos seguintes:<br /><br /> - [Público](../modifiers/public.md). Qualquer código que possa acessar o elemento que declara o delegado pode acessá-lo.<br />-   [Protegido](../modifiers/protected.md). Somente o código dentro da classe do delegado ou de uma classe derivada pode acessá-lo.<br />-   [Amigo](../modifiers/friend.md). Somente o código dentro do mesmo assembly pode acessar o delegado.<br />- [Particular](../modifiers/private.md). Somente o código dentro do elemento que declara o delegado pode acessá-lo.<br /><br /> - [Amigo protegido](../modifiers/protected-friend.md) Somente o código dentro da classe do delegado, uma classe derivada ou o mesmo assembly pode acessar o delegado. <br />- [Particular protegido](../modifiers/private-protected.md) Somente o código dentro da classe do delegado ou em uma classe derivada no mesmo assembly pode acessar o delegado. |  
|`Shadows`|Opcional. Indica que esse delegado redeclara e oculta um elemento de programação de nome idêntico, ou conjunto de elementos sobrecarregados, em uma classe base. Você pode sombrear qualquer tipo de elemento declarado com qualquer outro tipo.<br /><br /> Um elemento sombreado não está disponível de dentro da classe derivada que o sombreia, exceto de onde o elemento de sombreamento está inacessível. Por exemplo, se um `Private` elemento sombreia um elemento de classe base, o código que não tem permissão para acessar `Private` o elemento acessa o elemento de classe base em vez disso.|  
|`Sub`|Opcional, mas `Sub` ou `Function` deve aparecer. Declara esse procedimento como um procedimento delegado `Sub` que não retorna um valor.|  
|`Function`|Opcional, mas `Sub` ou `Function` deve aparecer. Declara esse procedimento como um procedimento delegado `Function` que retorna um valor.|  
|`name`|Obrigatórios. Nome do tipo delegado; segue as convenções padrão de nomenclatura de variável.|  
|`typeparamlist`|Opcional. Lista de parâmetros de tipo para este delegado. Vários parâmetros de tipo são separados por vírgulas. Opcionalmente, cada parâmetro de tipo pode ser declarado como Variant usando os `In` `Out` modificadores genérico. Você deve colocar a [lista de tipos](type-list.md) entre parênteses e apresentá-la com a `Of` palavra-chave.|  
|`parameterlist`|Opcional. Lista de parâmetros que são passados para o procedimento quando ele é chamado. Você deve colocar a [lista de parâmetros](parameter-list.md) entre parênteses.|  
|`type`|Necessário se você especificar um `Function` procedimento. Tipo de dados do valor de retorno.|  
  
## <a name="remarks"></a>Comentários  
 A `Delegate` instrução define o parâmetro e os tipos de retorno de uma classe delegate. Qualquer procedimento com parâmetros e tipos de retorno correspondentes pode ser usado para criar uma instância dessa classe delegate. O procedimento pode ser posteriormente invocado por meio da instância delegar, chamando o método do delegado `Invoke` .  
  
 Os delegados podem ser declarados no nível de namespace, módulo, classe ou estrutura, mas não dentro de um procedimento.  
  
 Cada classe de delegado define um construtor que é passado para a especificação de um método do objeto. Um argumento para o construtor delegado deve ser uma referência a um método ou uma expressão lambda.  
  
 Para especificar uma referência a um método, use a seguinte sintaxe:  
  
 `AddressOf` [`expression`.]`methodname`  
  
 O tipo de tempo de compilação do `expression` deve ser o nome de uma classe ou uma interface que contém um método do nome especificado cuja assinatura coincide com a assinatura da classe delegada. O `methodname` pode ser um método compartilhado ou um método de instância. O `methodname` não é opcional, mesmo se você criar um delegado para o método padrão da classe.  
  
 Para especificar uma expressão lambda, use a seguinte sintaxe:  
  
 `Function` ([`parm` As `type`, `parm2` As `type2`, ...]) `expression`  
  
 A assinatura da função deve corresponder a do tipo delegado. Para obter mais informações sobre expressões lambda, consulte [expressões lambda](../../programming-guide/language-features/procedures/lambda-expressions.md).  
  
 Para obter mais informações sobre delegados, confira [Delegados](../../programming-guide/language-features/delegates/index.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a `Delegate` instrução para declarar um delegado para operar em dois números e retornar um número. O `DelegateTest` método usa uma instância de um delegado desse tipo e a usa para operar em pares de números.  
  
 [!code-vb[VbVbalrDelegates#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#14)]  
  
## <a name="see-also"></a>Confira também

- [Operador AddressOf](../operators/addressof-operator.md)
- [Desse](of-clause.md)
- [Delegados](../../programming-guide/language-features/delegates/index.md)
- [Como: Usar uma classe genérica](../../programming-guide/language-features/data-types/how-to-use-a-generic-class.md)
- [Tipos genéricos no Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [Covariância e contravariância](../../programming-guide/concepts/covariance-contravariance/index.md)
- [Em](../modifiers/in-generic-modifier.md)
- [Fora](../modifiers/out-generic-modifier.md)
