---
title: Instrução Option Compare
ms.date: 07/20/2015
f1_keywords:
- vb.Compare
- vb.OptionCompare
helpviewer_keywords:
- case sensitivity, Option Compare statement
- Compare keyword [Visual Basic]
- binary comparison [Visual Basic]
- strings [Visual Basic], returning from functions
- binary comparison [Visual Basic], Option Compare statement
- strings [Visual Basic], comparing
- string comparison [Visual Basic], Option Compare statement
- Text keyword [Visual Basic], Option Compare statement
- Binary keyword [Visual Basic], Option Compare statement
- string comparison [Visual Basic], sorting data
- Option Compare statement [Visual Basic]
- text [Visual Basic], comparing
ms.assetid: 54e8eeeb-3b0d-4fb9-acce-fbfbd5975f6e
ms.openlocfilehash: 1ffe3e45a296d02364f488540d987d85133013bd
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404376"
---
# <a name="option-compare-statement"></a>Instrução Option Compare
Declara o método padrão de comparação a ser usado ao comparar dados da cadeia de caracteres.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Option Compare { Binary | Text }  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`Binary`|Opcional. Resulta em comparações de cadeias de caracteres com base em uma ordem de classificação derivada das representações binárias internas dos caracteres.<br /><br /> Esse tipo de comparação é útil principalmente se as cadeias de caracteres puderem conter caracteres que não serão interpretados como texto. Nesse caso, você não deseja ajustar comparações com equivalentes em ordem alfabética, como maiúsculas e minúsculas.|  
|`Text`|Opcional. Resulta em comparações de cadeias de caracteres com base em uma ordem de classificação de texto com diferenciação de maiúsculas de minúsculas determinada pela localidade do sistema.<br /><br /> Esse tipo de comparação é útil se suas cadeias de caracteres tiverem todos os caracteres de texto e você desejar compará-las levando em conta equivalências alfabéticas como maiúsculas e minúsculas e letras relacionadas. Por exemplo, você pode desejar considerar que `A` e `a` sejam iguais, e que `Ä` e `ä` venham antes de `B` e `b`.|  
  
## <a name="remarks"></a>Comentários  
 Se usado, a instrução `Option Compare` deve aparecer em um arquivo antes de quaisquer outras instruções de código-fonte.  
  
 A instrução `Option Compare` especifica o método de comparação de cadeia de caracteres (`Binary` ou `Text`).  O método de comparação de texto padrão é `Binary`.  
  
 A comparação `Binary` compara o valor de Unicode numérico de cada caractere em cadeia de caracteres. A comparação `Text` compara cada caractere Unicode com base em seu sentido lexical na cultura atual.  
  
 No Microsoft Windows, a ordem de classificação é determinada pela página de código. Para obter mais informações, consulte [Páginas de Código](/cpp/c-runtime-library/code-pages).  
  
 No exemplo a seguir, os caracteres na página de código Inglês/Europeu (ANSI 1252) são classificados usando `Option Compare Binary`, que produz uma ordem de classificação binária típica.  
  
 `A < B < E < Z < a < b < e < z < À < Ê < Ø < à < ê < ø`  
  
 Quando os mesmos caracteres na mesma página de código são classificados usando `Option Compare Text`, a ordem de classificação a seguir é produzida.  
  
 `(A=a) < (À = à) < (B=b) < (E=e) < (Ê = ê) < (Z=z) < (Ø = ø)`  
  
## <a name="when-an-option-compare-statement-is-not-present"></a>Quando uma Instrução Option Compare Não Está Presente  
 Se o código-fonte não contiver uma `Option Compare` instrução, a **opção comparar** configuração na [página compilar, Designer de projeto (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic) será usada. Se você usar o compilador de linha de comando, a configuração especificada pela opção de compilador [-optioncompare](../../reference/command-line-compiler/optioncompare.md) será usada.  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
#### <a name="to-set-option-compare-in-the-ide"></a>Para definir o Option Compare no IDE  
  
1. Em **Gerenciador de soluções**, selecione um projeto. No menu **Projeto** , clique em **Propriedades**.  
  
2. Clique na guia **Compilar**.  
  
3. Defina o valor na caixa de **comparação de opções** .  
  
 Quando você cria um projeto, a **opção comparar** configuração na guia **Compilar** é definida como a **opção comparar** configuração na caixa de diálogo **Opções** . Para alterar essa configuração, no menu **ferramentas** , clique em **Opções**. Na caixa de diálogo **Opções**, expanda **Projetos e Soluções** e, em seguida, clique em **Padrões de VB**. A configuração padrão inicial em **padrões do VB** é **Binary**.  
  
#### <a name="to-set-option-compare-on-the-command-line"></a>Para definir o Option Compare na linha de comando  
  
- Inclua a opção de compilador [-optioncompare](../../reference/command-line-compiler/optioncompare.md) no comando **Vbc** .  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a instrução `Option Compare` para definir a comparação binária como o método padrão de comparação de cadeia de caracteres. Para usar esse código, retire os comentários da instrução `Option Compare Binary` e coloque-os na parte superior do arquivo de origem.  
  
 [!code-vb[VbVbalrStatements#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#45)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a instrução `Option Compare` para definir a ordem de classificação sem diferenciação de maiúsculas de minúsculas como o método padrão de comparação de cadeia de caracteres. Para usar esse código, retire os comentários da instrução `Option Compare Text` e coloque-os na parte superior do arquivo de origem.  
  
 [!code-vb[VbVbalrStatements#46](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#46)]  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Strings.InStr%2A>
- <xref:Microsoft.VisualBasic.Strings.InStrRev%2A>
- <xref:Microsoft.VisualBasic.Strings.Replace%2A>
- <xref:Microsoft.VisualBasic.Strings.Split%2A>
- <xref:Microsoft.VisualBasic.Strings.StrComp%2A>
- [-optioncompare](../../reference/command-line-compiler/optioncompare.md)
- [Operadores de comparação](../operators/comparison-operators.md)
- [Operadores de comparação no Visual Basic](../../programming-guide/language-features/operators-and-expressions/comparison-operators.md)
- [Operador Like](../operators/like-operator.md)
- [Funções de cadeia de caracteres](../functions/string-functions.md)
- [Instrução Option Explicit](option-explicit-statement.md)
- [Instrução Option Strict](option-strict-statement.md)
