---
title: '#Diretiva Const'
ms.date: 07/20/2015
f1_keywords:
- vb.#Const
- '#vb.Const'
- '#Const'
helpviewer_keywords:
- '#Const directive'
- conditional compilation [Visual Basic], directives
- Const directive (#Const)
- Visual Basic compiler, compiler directives
- constants [Visual Basic], Const directive
- constants [Visual Basic], declaring
- Const statement [Visual Basic], directive (#Const)
- 'declaring constants [Visual Basic], #const directive'
ms.assetid: 707669e5-23f9-4f17-8622-a0d534429386
ms.openlocfilehash: 91152771a4ef5ec74a7408511ccc2afe28dd442e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415460"
---
# <a name="const-directive"></a>Diretiva #Const

Define constantes de compilador condicional para Visual Basic.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
#Const constname = expression  
```  
  
## <a name="parts"></a>Partes  

 `constname`  
 Obrigatórios. Nome da constante que está sendo definida.  
  
 `expression`  
 Obrigatórios. Literal, outra constante de compilador condicional ou qualquer combinação que inclua qualquer ou todos os operadores aritméticos ou lógicos, exceto `Is` .  
  
## <a name="remarks"></a>Comentários  

 Constantes de compilador condicional são sempre particulares para o arquivo no qual aparecem. Você não pode criar constantes de compilador público usando a `#Const` diretiva; você pode criá-las somente na interface do usuário ou com a `/define` opção do compilador.  
  
 Você pode usar apenas constantes de compilador condicionais e literais no `expression` . Usar uma constante padrão definida com `Const` causa um erro. Por outro lado, você pode usar constantes definidas com a `#Const` palavra-chave somente para compilação condicional. As constantes também podem ser indefinidas; nesse caso, elas têm um valor de `Nothing` .  
  
## <a name="example"></a>Exemplo  

 Este exemplo usa a `#Const` diretiva.  
  
 [!code-vb[VbVbalrConditionalComp#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConditionalComp/VB/Class1.vb#3)]  
  
## <a name="see-also"></a>Confira também

- [-definir (Visual Basic)](../../reference/command-line-compiler/define.md)
- [#If... Diretivas then... #Else](if-then-else-directives.md)
- [Instrução Const](../statements/const-statement.md)
- [Compilação condicional](../../programming-guide/program-structure/conditional-compilation.md)
- [Instrução If...Then...Else](../statements/if-then-else-statement.md)
