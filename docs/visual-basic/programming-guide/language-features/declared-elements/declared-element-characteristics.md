---
title: Características do elemento declarado
ms.date: 07/20/2015
helpviewer_keywords:
- declared elements [Visual Basic], lifetime
- access levels, declared elements
- declared elements [Visual Basic], scope
- visibility [Visual Basic], declared elements
- elements [Visual Basic], programming
- scope [Visual Basic], declared elements
- lifetime [Visual Basic], declared elements
- declared elements [Visual Basic], access level
- data types [Visual Basic], declared elements
- declared elements [Visual Basic], visibility
ms.assetid: 1bc40fb8-b67c-4428-90a4-76b630ae2583
ms.openlocfilehash: 9d1e327c25689bed1405ea9a627da6232abb707b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392942"
---
# <a name="declared-element-characteristics-visual-basic"></a>Características do elemento declarado (Visual Basic)
Uma *característica* de um elemento declarado é um aspecto desse elemento que afeta como o código pode interagir com ele. Cada elemento declarado tem uma ou mais das seguintes características associadas a ele:  
  
- *Tipo de dados* — os valores que o elemento pode conter e como ele armazena esses valores. Para obter mais informações, veja [Tipos de Dados](../../../language-reference/data-types/index.md).  
  
- *Vida útil* — o período de tempo de execução durante o qual o elemento está disponível para uso. Para obter mais informações, consulte [tempo de vida em Visual Basic](lifetime.md).  
  
- *Escopo* — o conjunto de todos os códigos que podem se referir ao elemento sem qualificar seu nome. Para obter mais informações, consulte [como controlar o escopo de uma variável](how-to-control-the-scope-of-a-variable.md).  
  
- *Nível de acesso* — a permissão para que o código faça uso do elemento. Para obter mais informações, consulte [como controlar a disponibilidade de uma variável](how-to-control-the-availability-of-a-variable.md).  
  
## <a name="characteristics-of-the-elements"></a>Características dos elementos  
 A tabela a seguir mostra os elementos declarados e as características que se aplicam a cada um.  
  
|Elemento|Tipo de Dados|Tempo de vida|Escopo <sup>1</sup>|Nível de acesso|  
|-------------|---------------|--------------|------------------------|------------------|  
|Variável|Sim|Sim|Sim|Sim|  
|Constante|Sim|Não|Sim|Sim|  
|Enumeração|Sim|Não|Sim|Sim|  
|Estrutura|Não|Não|Sim|Sim|  
|Propriedade|Sim|Sim|Sim|Sim|  
|Método|Não|Sim|Sim|Sim|  
|Procedimento ( `Sub` ou `Function` )|Não|Sim|Sim|Sim|  
|Parâmetro de procedimento|Sim|Sim|Sim|Não|  
|Retorno de função|Sim|Sim|Sim|Não|  
|Operador|Sim|Não|Sim|Sim|  
|Interface|Não|Não|Sim|Sim|  
|Classe|Não|Não|Sim|Sim|  
|Evento|Não|Não|Sim|Sim|  
|Delegar|Não|Não|Sim|Sim|  
  
 <sup>1</sup> o escopo é às vezes chamado de *visibilidade*.  
  
## <a name="see-also"></a>Confira também

- [Elementos declarados](index.md)
- [Nomes de elementos declarados](declared-element-names.md)
- [Referências a elementos declarados](references-to-declared-elements.md)
- [Tempo de vida no Visual Basic](lifetime.md)
- [Escopo no Visual Basic](scope.md)
- [Níveis de acesso no Visual Basic](access-levels.md)
- [Tipos de dados](../data-types/index.md)
- [Declaração de Variável](../variables/variable-declaration.md)
