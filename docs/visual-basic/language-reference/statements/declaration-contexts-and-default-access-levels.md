---
title: Contextos de Declaração e Níveis de Acesso Padrão
ms.date: 07/20/2015
helpviewer_keywords:
- module level, defined
- declaration contexts, Visual Basic
- procedure level, defined
- namespace level, defined
- access levels, Visual Basic
- access levels, default levels
ms.assetid: bf63b96e-e825-4745-88c8-5dae222728db
ms.openlocfilehash: b5bb943a062ac648f88645fb6de1acb42213071c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404791"
---
# <a name="declaration-contexts-and-default-access-levels-visual-basic"></a>Contextos de declaração e níveis de acesso padrão (Visual Basic)
Este tópico descreve quais tipos de Visual Basic podem ser declarados dentro dos outros tipos e quais seus níveis de acesso padrão se não forem especificados.  
  
## <a name="declaration-context-levels"></a>Níveis de contexto de declaração  
 O *contexto de declaração* de um elemento de programação é a região do código na qual ele é declarado. Geralmente, esse é outro elemento de programação, que é chamado de *elemento que o contém*.  
  
 Os níveis para contextos de declaração são os seguintes:  
  
- *Nível de namespace* — dentro de um arquivo de origem ou namespace, mas não dentro de uma classe, estrutura, módulo ou interface  
  
- *Nível de módulo* — dentro de uma classe, estrutura, módulo ou interface, mas não dentro de um procedimento ou bloco  
  
- *Nível de procedimento* — dentro de um procedimento ou bloco (como `If` ou `For` )  
  
 A tabela a seguir mostra os níveis de acesso padrão para vários elementos de programação declarados, dependendo de seus contextos de declaração.  
  
|Elemento declarado|Nível de namespace|Nível de módulo|Nível de procedimento|  
|----------------------|---------------------|------------------|---------------------|  
|Variável ([instrução Dim](dim-statement.md))|Não permitido|`Private`( `Public` em `Structure` , não permitido em `Interface` )|`Public`|  
|Constante ([instrução const](const-statement.md))|Não permitido|`Private`( `Public` em `Structure` , não permitido em `Interface` )|`Public`|  
|Enumeration ([instrução enum](enum-statement.md))|`Friend`|`Public`|Não permitido|  
|Classe ([declaração de classe](class-statement.md))|`Friend`|`Public`|Não permitido|  
|Estrutura ([instrução de estrutura](structure-statement.md))|`Friend`|`Public`|Não permitido|  
|Módulo ([instrução Module](module-statement.md))|`Friend`|Não permitido|Não permitido|  
|Interface ([instrução de interface](interface-statement.md))|`Friend`|`Public`|Não permitido|  
|Procedimento ([instrução function](function-statement.md), [sub Statement](sub-statement.md))|Não permitido|`Public`|Não permitido|  
|Referência externa ([instrução Declare](declare-statement.md))|Não permitido|`Public`(não permitido em `Interface` )|Não permitido|  
|Operador ([instrução Operator](operator-statement.md))|Não permitido|`Public`(não permitido no `Interface` ou `Module` )|Não permitido|  
|Propriedade ([instrução de propriedade](property-statement.md))|Não permitido|`Public`|Não permitido|  
|Propriedade Default ([padrão](../modifiers/default.md))|Não permitido|`Public`(não permitido em `Module` )|Não permitido|  
|Evento ([instrução de evento](event-statement.md))|Não permitido|`Public`|Não permitido|  
|Delegate ([instrução delegate](delegate-statement.md))|`Friend`|`Public`|Não permitido|  
  
 Para obter mais informações, consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).  
  
## <a name="see-also"></a>Confira também

- [Público](../modifiers/friend.md)
- [Privada](../modifiers/private.md)
- [Pública](../modifiers/public.md)
