---
title: -optionexplicit
ms.date: 07/20/2015
f1_keywords:
- /optionexplicit
- -optionexplicit
helpviewer_keywords:
- /optionexplicit compiler option [Visual Basic]
- optionexplicit compiler option [Visual Basic]
- -optionexplicit compiler option [Visual Basic]
ms.assetid: 5d296ab3-bafe-4c4d-9887-78f162ed86c7
ms.openlocfilehash: b004acb0c1c7d145c59a1e3a88ef7f1d405a91c6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400546"
---
# <a name="-optionexplicit"></a>-optionexplicit
Faz com que o compilador Relate erros se as variáveis não forem declaradas antes de serem usadas.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-optionexplicit[+ | -]  
```  
  
## <a name="arguments"></a>Argumentos  
 `+` &#124; `-`  
 Opcional. Especifique `-optionexplicit+` para exigir declaração explícita de variáveis. A `-optionexplicit+` opção é o padrão e é a mesma que `-optionexplicit` . A `-optionexplicit-` opção habilita a declaração implícita de variáveis.  
  
## <a name="remarks"></a>Comentários  
 Se o arquivo de código-fonte contiver uma [instrução Option Explicit](../../language-reference/statements/option-explicit-statement.md), a instrução substituirá a `-optionexplicit` configuração do compilador de linha de comando.  
  
### <a name="to-set--optionexplicit-in-the-visual-studio-ide"></a>Para Set-optionexplicit no IDE do Visual Studio  
  
1. Selecione um projeto no **Gerenciador de Soluções**. No menu **Projeto** , clique em **Propriedades**.
  
2. Clique na guia **Compilar**.  
  
3. Modifique o valor na caixa **Option Explicit** .  
  
## <a name="example"></a>Exemplo  
 O código a seguir é compilado quando `-optionexplicit-` é usado.  
  
 [!code-vb[VbVbalrCompiler#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionExplicitOff.vb#5)]  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [-optioncompare](optioncompare.md)
- [-optionstrict](optionstrict.md)
- [-optioninfer](optioninfer.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
- [Instrução Option Explicit](../../language-reference/statements/option-explicit-statement.md)
- [Caixa de diálogo Padrões do Visual Basic, Projetos, Opções](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
