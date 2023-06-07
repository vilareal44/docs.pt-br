---
title: Estático
ms.date: 07/20/2015
f1_keywords:
- vb.Static
helpviewer_keywords:
- static modifier
- Static keyword [Visual Basic]
ms.assetid: 19013910-4658-47b6-a22e-1744b527979e
ms.openlocfilehash: 3b323d5fb1c4f1357b9f476213793c69d29b7208
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402688"
---
# <a name="static-visual-basic"></a>Estático (Visual Basic)
Especifica que uma ou mais variáveis locais declaradas devem continuar a existir e manter seus valores mais recentes após o término do procedimento no qual elas são declaradas.  
  
## <a name="remarks"></a>Comentários  
 Normalmente, uma variável local em um procedimento deixa de existir assim que o procedimento para. Uma variável estática continua existindo e retém seu valor mais recente. Na próxima vez em que o código chamar o procedimento, a variável não será reinicializada e ainda manterá o valor mais recente que você atribuiu a ela. Uma variável estática continua existindo durante o tempo de vida da classe ou do módulo em que ela está definida.  
  
## <a name="rules"></a>Regras  
  
- **Contexto de declaração.** Você pode usar `Static` somente em variáveis locais. Isso significa que o contexto de declaração para uma `Static` variável deve ser um procedimento ou um bloco em um procedimento, e não pode ser um arquivo de origem, namespace, classe, estrutura ou módulo.  
  
     Você não pode usar `Static` dentro de um procedimento de estrutura.  
  
- Os tipos de dados de `Static` variáveis locais não podem ser inferidos. Para obter mais informações, consulte [inferência de tipo local](../../programming-guide/language-features/variables/local-type-inference.md).  
  
- **Modificadores combinados.** Você não pode especificar `Static` juntos com `ReadOnly` , `Shadows` ou `Shared` na mesma declaração.  
  
## <a name="behavior"></a>Comportamento  
 Quando você declara uma variável estática em um `Shared` procedimento, apenas uma cópia da variável estática está disponível para o aplicativo inteiro. Você chama um `Shared` procedimento usando o nome da classe, não uma variável que aponta para uma instância da classe.  
  
 Quando você declara uma variável estática em um procedimento que não é `Shared` , apenas uma cópia da variável está disponível para cada instância da classe. Você chama um procedimento não compartilhado usando uma variável que aponta para uma instância específica da classe.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra o uso de `Static`.  
  
 [!code-vb[VbVbalrKeywords#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#5)]  
  
 A `Static` variável `totalSales` é inicializada como 0 somente uma vez. Cada vez que você inserir `updateSales` , `totalSales` ainda terá o valor mais recente que você calculou para ele.  
  
 O `Static` modificador pode ser usado neste contexto:  
  
 [Instrução Dim](../statements/dim-statement.md)  
  
## <a name="see-also"></a>Confira também

- [Sombras](shadows.md)
- [Compartilhado](shared.md)
- [Tempo de vida no Visual Basic](../../programming-guide/language-features/declared-elements/lifetime.md)
- [Declaração de Variável](../../programming-guide/language-features/variables/variable-declaration.md)
- [Estruturas](../../programming-guide/language-features/data-types/structures.md)
- [Inferência de Tipo de Variável Local](../../programming-guide/language-features/variables/local-type-inference.md)
- [Objetos e classes](../../programming-guide/language-features/objects-and-classes/index.md)
