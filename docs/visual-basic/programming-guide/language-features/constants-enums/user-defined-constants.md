---
title: Constantes definidas pelo usuário
ms.date: 07/20/2015
helpviewer_keywords:
- constants [Visual Basic], circular references
- Const statement [Visual Basic], user-defined constants
- user-defined constants [Visual Basic]
- scope [Visual Basic], constants
- constants [Visual Basic], user-defined
- circular references between constants [Visual Basic]
ms.assetid: a1206d5c-c45e-4ac2-970a-4a0be6a05fdd
ms.openlocfilehash: 14f3de39eb8d8e6820e2b40792a8e8e57217e410
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414370"
---
# <a name="user-defined-constants-visual-basic"></a>Constantes definidas pelo usuário (Visual Basic)
Uma constante é um nome significativo que substitui um número ou uma cadeia de caracteres que não é alterada. Constantes armazenam valores que, como o nome implica, permanecem constantes durante a execução de um aplicativo. Você pode usar constantes que são definidas pelos controles ou componentes com os quais você trabalha, ou você pode criar suas próprias. As constantes que você cria são descritas como *definidas pelo usuário*.  
  
 Você declara uma constante com a `Const` instrução, usando as mesmas diretrizes para criar um nome de variável. Se `Option Strict` for `On` , você deverá declarar explicitamente o tipo de constante.  
  
## <a name="const-statement-usage"></a>Uso da instrução const  
 Uma `Const` instrução pode representar uma quantidade matemática ou de data/hora:  
  
 [!code-vb[VbEnumsTask#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#10)]  
  
 Ele também pode definir `String` constantes:  
  
 [!code-vb[VbEnumsTask#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#13)]  
  
 A expressão no lado direito do sinal de igual ( `=` ) geralmente é um número ou uma cadeia de caracteres literal, mas também pode ser uma expressão que resulte em um número ou cadeia de caracteres (embora essa expressão não possa conter chamadas para funções). Você pode até mesmo definir constantes em termos de constantes definidas anteriormente:  
  
 [!code-vb[VbEnumsTask#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#15)]  
  
## <a name="scope-of-user-defined-constants"></a>Escopo de constantes definidas pelo usuário  
 `Const`O escopo de uma instrução é o mesmo de uma variável declarada no mesmo local. Você pode especificar o escopo de qualquer uma das seguintes maneiras:  
  
- Para criar uma constante que existe somente dentro de um procedimento, declare-a dentro desse procedimento.  
  
- Para criar uma constante disponível para todos os procedimentos em uma classe, mas não para qualquer código fora desse módulo, declare-o na seção declarações da classe.  
  
- Para criar uma constante que está disponível para todos os membros de um assembly, mas não para clientes externos do assembly, declare-o usando a `Friend` palavra-chave na seção declarações da classe.  
  
- Para criar uma constante disponível em todo o aplicativo, declare-a usando a `Public` palavra-chave na seção declarações a classe.  
  
 Para obter mais informações, consulte [como: declarar uma constante](how-to-declare-a-constant.md).  
  
### <a name="avoiding-circular-references"></a>Evitando referências circulares  
 Como constantes podem ser definidas em termos de outras constantes, é possível criar inadvertidamente um *ciclo*, ou referência circular, entre duas ou mais constantes. Um ciclo ocorre quando você tem duas ou mais constantes públicas, cada uma delas definida em termos do outro, como no exemplo a seguir:  
  
 [!code-vb[VbEnumsTask#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#16)]  
[!code-vb[VbEnumsTask#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#17)]  
  
 Se ocorrer um ciclo, Visual Basic gerará um erro do compilador.  
  
## <a name="see-also"></a>Confira também

- [Instrução Const](../../../language-reference/statements/const-statement.md)
- [Tipos de dados constante e literal](constant-and-literal-data-types.md)
- [Constantes e enumerações](index.md)
- [Constantes e enumerações](../../../language-reference/constants-and-enumerations.md)
- [Visão geral de enumerações](enumerations-overview.md)
- [Visão geral de constantes](constants-overview.md)
- [Como declarar uma enumeração](how-to-declare-enumerations.md)
- [Enumerações e qualificação de nome](enumerations-and-name-qualification.md)
- [Instrução Option Strict](../../../language-reference/statements/option-strict-statement.md)
