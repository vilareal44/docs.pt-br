---
title: Diferenças entre sombreamento e sobreposição
ms.date: 07/20/2015
helpviewer_keywords:
- shadowing, vs. overriding
- overriding, vs. shadowing
ms.assetid: 2d014a0b-7630-407d-8f4e-24bd87987923
ms.openlocfilehash: a6ea83fadf18ef3be778e6de31c0eb4e65e74824
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392864"
---
# <a name="differences-between-shadowing-and-overriding-visual-basic"></a>Diferenças entre sombreamento e sobreposição (Visual Basic)
Quando você define uma classe que herda de uma classe base, às vezes você deseja redefinir um ou mais dos elementos da classe base na classe derivada. Sombreamento e substituição estão disponíveis para essa finalidade.  
  
## <a name="comparison"></a>Comparação  
 Sombreamento e substituição são usados quando uma classe derivada herda de uma classe base e ambas redefinem um elemento declarado com outro. Mas há diferenças significativas entre os dois.  
  
 A tabela a seguir compara a sombra com a substituição.  
  
||||  
|---|---|---|  
|Ponto de comparação|Sombreamento|Substituição|  
|Finalidade|Protege contra uma modificação subsequente de classe base que apresenta um membro que você já definiu em sua classe derivada|Obtém polimorfismo definindo uma implementação diferente de um procedimento ou propriedade com a mesma sequência de chamada<sup>1</sup>|  
|Elemento redefinido|Qualquer tipo de elemento declarado|Somente um procedimento ( `Function` , `Sub` ou `Operator` ) ou propriedade|  
|Redefinindo elemento|Qualquer tipo de elemento declarado|Somente um procedimento ou propriedade com a sequência de chamada idêntica<sup>1</sup>|  
|Nível de acesso do elemento redefinido|Qualquer nível de acesso|Não é possível alterar o nível de acesso do elemento substituído|  
|Legibilidade e gravação do elemento redefinindo|Qualquer combinação|Não é possível alterar a legibilidade ou gravação da propriedade substituída|  
|Controle sobre a redefinição|O elemento da classe base não pode impor ou proibir sombreamento|O elemento da classe base pode especificar `MustOverride` , `NotOverridable` ou`Overridable`|  
|Uso de palavra-chave|`Shadows`recomendado na classe derivada; `Shadows`presumido se nem `Shadows` nem `Overrides` especificado<sup>2</sup>|`Overridable`ou `MustOverride` obrigatório na classe base; `Overrides` necessário na classe derivada|  
|Herança da redefinição de elemento por classes derivadas de sua classe derivada|Elemento de sombreamento herdado por mais classes derivadas; elemento sombreado ainda oculto<sup>3</sup>|Elemento de substituição herdado por classes derivadas posteriores; elemento substituído ainda substituído|  
  
 <sup>1</sup> a *sequência de chamada* consiste no tipo de elemento ( `Function` ,, `Sub` `Operator` ou `Property` ), nome, lista de parâmetros e tipo de retorno. Você não pode substituir um procedimento por uma propriedade, ou o contrário. Não é possível substituir um tipo de procedimento ( `Function` , `Sub` ou `Operator` ) por outro tipo.  
  
 <sup>2</sup> se você não especificar `Shadows` ou `Overrides` , o compilador emitirá uma mensagem de aviso para ajudá-lo a ter certeza de qual tipo de redefinição você deseja usar. Se você ignorar o aviso, o mecanismo de sombreamento será usado.  
  
 <sup>3</sup> se o elemento de sombreamento estiver inacessível em uma classe derivada adicional, o sombreamento não será herdado. Por exemplo, se você declarar o elemento de sombreamento como `Private` , uma classe que deriva de sua classe derivada herdará o elemento original em vez do elemento de sombreamento.  
  
## <a name="guidelines"></a>Diretrizes  
 Normalmente, você usa a substituição nos seguintes casos:  
  
- Você está definindo classes derivadas polimórficas.  
  
- Você quer que a segurança de fazer com que o compilador Force o tipo de elemento e a sequência de chamada idênticos.  
  
 Normalmente, você usa sombreamento nos seguintes casos:  
  
- Você prevê que sua classe base pode ser modificada e definir um elemento usando o mesmo nome que a sua.  
  
- Você quer a liberdade de alterar o tipo de elemento ou a sequência de chamada.  
  
## <a name="see-also"></a>Confira também

- [Referências a elementos declarados](references-to-declared-elements.md)
- [Sombreamento no Visual Basic](shadowing.md)
- [Como ocultar uma variável com o mesmo nome que a variável](how-to-hide-a-variable-with-the-same-name-as-your-variable.md)
- [Como ocultar uma variável herdada](how-to-hide-an-inherited-variable.md)
- [Como acessar uma variável oculta por uma classe derivada](how-to-access-a-variable-hidden-by-a-derived-class.md)
- [Sombras](../../../language-reference/modifiers/shadows.md)
- [Substituições](../../../language-reference/modifiers/overrides.md)
