---
title: Instrução Option Explicit
ms.date: 07/20/2015
f1_keywords:
- vb.Explicit
- vb.OptionExplicit
helpviewer_keywords:
- declaring variables [Visual Basic], explicit
- forced variable declaration in Option Explicit statement [Visual Basic]
- Explicit keyword
- explicit variable declaration
- Option Explicit statement [Visual Basic]
ms.assetid: e82ac1ad-2cd3-49b2-b985-8bcf016f3fcc
ms.openlocfilehash: a352df0323cfeca1ea0e206ae45c3f85a2cd7da3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404363"
---
# <a name="option-explicit-statement-visual-basic"></a>Instrução Option Explicit (Visual Basic)
Força a declaração explícita de todas as variáveis em um arquivo ou permite declarações implícitas de variáveis.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Option Explicit { On | Off }  
```  
  
## <a name="parts"></a>Partes  
 `On`  
 Opcional. Habilita a `Option Explicit` verificação. Se `On` ou `Off` não for especificado, o padrão será `On` .  
  
 `Off`  
 Opcional. Desabilita a `Option Explicit` verificação.  
  
## <a name="remarks"></a>Comentários  
 Quando `Option Explicit On` ou `Option Explicit` aparece em um arquivo, você deve declarar explicitamente todas as variáveis usando as `Dim` `ReDim` instruções ou. Se você tentar usar um nome de variável não declarado, ocorrerá um erro no momento da compilação. A `Option Explicit Off` instrução permite a declaração implícita de variáveis.  
  
 Se usado, a instrução `Option Explicit` deve aparecer em um arquivo antes de quaisquer outras instruções de código-fonte.  
  
> [!NOTE]
> `Option Explicit`A configuração como `Off` geralmente não é uma prática recomendada. Você poderia digitar incorretamente um nome de variável em um ou mais locais, o que levaria a resultados inesperados na execução do programa.  
  
## <a name="when-an-option-explicit-statement-is-not-present"></a>Quando uma instrução Option Explicit não está presente  
 Se o código-fonte não contiver uma `Option Explicit` instrução, a configuração **Option Explicit** na [página Compile, Designer de projeto (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic) será usada. Se o compilador de linha de comando for usado, a opção de compilador [-optionexplicit](../../reference/command-line-compiler/optionexplicit.md) será usada.  
  
#### <a name="to-set-option-explicit-in-the-ide"></a>Para definir a opção Explicit no IDE  
  
1. Em **Gerenciador de soluções**, selecione um projeto. No menu **Projeto** , clique em **Propriedades**.  
  
2. Clique na guia **Compilar**.  
  
3. Defina o valor na caixa **Option Explicit** .  
  
 Quando você cria um novo projeto, a **opção configuração explícita** na guia **Compilar** é definida como a **opção configuração explícita** na caixa de diálogo **padrões do VB** . Para acessar a caixa de diálogo **padrões do VB** , no menu **ferramentas** , clique em **Opções**. Na caixa de diálogo **Opções**, expanda **Projetos e Soluções** e, em seguida, clique em **Padrões de VB**. A configuração padrão inicial em **padrões do VB** é `On` .  
  
#### <a name="to-set-option-explicit-on-the-command-line"></a>Para definir a opção Explicit na linha de comando  
  
- Inclua a opção de compilador [-optionexplicit](../../reference/command-line-compiler/optionexplicit.md) no comando **Vbc** .  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a `Option Explicit` instrução para forçar a declaração explícita de todas as variáveis. A tentativa de usar uma variável não declarada causa um erro no tempo de compilação.  
  
 [!code-vb[VbVbalrStatements#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#47)]  
  
 [!code-vb[VbVbalrStatements#48](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#48)]  
  
## <a name="see-also"></a>Confira também

- [Instrução Dim](dim-statement.md)
- [Instrução ReDim](redim-statement.md)
- [Instrução Option Compare](option-compare-statement.md)
- [Instrução Option Strict](option-strict-statement.md)
- [-optioncompare](../../reference/command-line-compiler/optioncompare.md)
- [-optionexplicit](../../reference/command-line-compiler/optionexplicit.md)
- [-optionstrict](../../reference/command-line-compiler/optionstrict.md)
- [Caixa de diálogo Padrões do Visual Basic, Projetos, Opções](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
