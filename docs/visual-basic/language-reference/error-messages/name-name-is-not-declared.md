---
title: O nome '<name>' não é declarado
ms.date: 10/10/2018
f1_keywords:
- bc30451
- vbc30451
helpviewer_keywords:
- BC30451
ms.assetid: 765f099b-e21e-47c6-a906-a065444e56b3
ms.openlocfilehash: 6fa4639b97e4314d8752ae520e94a58a189b7cbb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397162"
---
# <a name="name-name-is-not-declared"></a>O nome '\<name>' não é declarado
Uma instrução refere-se a um elemento de programação, mas o compilador não pode localizar um elemento com esse nome exato.  
  
 **ID do erro:** BC30451  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Verifique a ortografia do nome na instrução de referência. Visual Basic não diferencia maiúsculas de minúsculas, mas qualquer outra variação na ortografia é considerada como um nome completamente diferente. Observe que o sublinhado ( `_` ) faz parte do nome e, portanto, parte da grafia.  
  
2. Verifique se você tem o operador de acesso de membro ( `.` ) entre um objeto e seu membro. Por exemplo, se você tiver um <xref:System.Windows.Forms.TextBox> controle chamado `TextBox1` , para acessar sua <xref:System.Windows.Forms.TextBoxBase.Text%2A> propriedade, você deve digitar `TextBox1.Text` . Se, em vez disso, você digitar `TextBox1Text` , terá criado um nome diferente.  
  
3. Se a ortografia estiver correta e a sintaxe de qualquer acesso de membro de objeto estiver correta, verifique se o elemento foi declarado. Para obter mais informações, consulte [elementos declarados](../../programming-guide/language-features/declared-elements/index.md).  
  
4. Se o elemento de programação tiver sido declarado, verifique se ele está no escopo. Se a instrução referenciada estiver fora da região declarando o elemento de programação, talvez seja necessário qualificar o nome do elemento. Para obter mais informações, consulte [escopo em Visual Basic](../../programming-guide/language-features/declared-elements/scope.md).  

5. Se você não estiver usando um tipo totalmente qualificado ou um nome de membro (por exemplo, seu código se refere a uma propriedade como `MethodInfo.Name` em vez de `System.Reflection.MethodInfo.Name` ), adicione uma [instrução Imports](../statements/imports-statement-net-namespace-and-type.md).

6. Se você estiver tentando compilar um projeto no estilo SDK (um projeto com um \* arquivo. vbproj que começa com a linha `<Project Sdk="Microsoft.NET.Sdk">` ) e a mensagem de erro se referir a um tipo ou membro no assembly Microsoft. VisualBasic. dll, configure seu aplicativo para compilar com uma referência à biblioteca de tempo de execução Visual Basic. Por padrão, um subconjunto da biblioteca é inserido em seu assembly em um projeto no estilo SDK.

   Por exemplo, o exemplo a seguir falha na compilação porque o <xref:Microsoft.VisualBasic.CompilerServices.Conversions.ChangeType%2A?displayProperty=fullName> método não pode ser encontrado. Ele não é inserido no subconjunto do Visual Basic Runtime incluído com seu aplicativo.  

   [!code-vb[BC30451](~/samples/snippets/visualbasic/language-reference/error-messages/bc30451/program1.vb?highlight=7)]

   Para resolver esse erro, adicione o `<VBRuntime>Default</VBRuntime>` elemento à seção projetos `<PropertyGroup>` , como mostra o arquivo de projeto Visual Basic a seguir.

   [!code-xml[BC30451](~/samples/snippets/visualbasic/language-reference/error-messages/bc30451/vbruntime.vbproj?highlight=6)]

## <a name="see-also"></a>Confira também

- [Resumo de Declarações e Constantes](../keywords/declarations-and-constants-summary.md)
- [Convenções de nomenclatura do Visual Basic](../../programming-guide/program-structure/naming-conventions.md)
- [Nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md)
- [Referências a elementos declarados](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
