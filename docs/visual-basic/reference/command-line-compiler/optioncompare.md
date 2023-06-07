---
title: -optioncompare
ms.date: 07/20/2015
f1_keywords:
- /optioncompare
- -optioncompare
helpviewer_keywords:
- optioncompare compiler option [Visual Basic]
- -optioncompare compiler option [Visual Basic]
- /optioncompare compiler option [Visual Basic]
ms.assetid: 7237b766-b44d-4cc5-9a3c-885348a7d9e4
ms.openlocfilehash: ed9adc7cddd9eb204937b9819e4eeff176821e95
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400547"
---
# <a name="-optioncompare"></a>-optioncompare

Especifica como são feitas comparações de cadeia de caracteres.

## <a name="syntax"></a>Sintaxe

```console
-optioncompare:{binary | text}
```

## <a name="remarks"></a>Comentários

Você pode especificar `-optioncompare` em uma das duas formas: `-optioncompare:binary` para usar comparações de cadeia de caracteres binárias e para usar comparações de `-optioncompare:text` cadeia de caracteres de texto. Por padrão, o compilador usa o `-optioncompare:binary` .

No Microsoft Windows, a página de código atual determina a ordem de classificação binária. Uma ordem de classificação binária típica é a seguinte:

`A < B < E < Z < a < b < e < z < À < Ê < Ø < à < ê < ø`

As comparações de cadeia de caracteres baseadas em texto são baseadas em uma ordem de classificação de texto que não diferencia maiúsculas de minúsculas determinada pela localidade do seu sistema. Uma ordem de classificação de texto típica é a seguinte:

`(A = a) < (À = à) < (B=b) < (E=e) < (Ê = ê) < (Z=z) < (Ø = ø)`

### <a name="to-set--optioncompare-in-the-visual-studio-ide"></a>Para Set-optioncompare no IDE do Visual Studio

1. Selecione um projeto no **Gerenciador de Soluções**. No menu **Projeto** , clique em **Propriedades**.

2. Clique na guia **Compilar**.

3. Modifique o valor na caixa de **comparação de opções** .

### <a name="to-set--optioncompare-programmatically"></a>Para Set-optioncompare programaticamente

Consulte a [instrução de comparação de opção](../../language-reference/statements/option-compare-statement.md).

## <a name="example"></a>Exemplo

O código a seguir compila `ProjFile.vb` e usa comparações de cadeia de caracteres binárias.

```console
vbc -optioncompare:binary projFile.vb
```

## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [-optionexplicit](optionexplicit.md)
- [-optionstrict](optionstrict.md)
- [-optioninfer](optioninfer.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
- [Instrução Option Compare](../../language-reference/statements/option-compare-statement.md)
- [Caixa de diálogo Padrões do Visual Basic, Projetos, Opções](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
