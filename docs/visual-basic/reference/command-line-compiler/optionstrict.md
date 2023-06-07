---
title: -optionstrict
ms.date: 07/20/2015
f1_keywords:
- -optionstrict
helpviewer_keywords:
- -optionstrict compiler option [Visual Basic]
- optionstrict compiler option [Visual Basic]
- /optionstrict compiler option [Visual Basic]
ms.assetid: c7b10086-0fa4-49db-b3c8-4ae0db5957da
ms.openlocfilehash: 3dd12971a082869c32b6292ed45e2014b8b0e2c0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400533"
---
# <a name="-optionstrict"></a>-optionstrict

Impõe uma semântica de tipo estrito para restringir conversões implícitas de tipo.

## <a name="syntax"></a>Sintaxe

```console
-optionstrict[+ | -]
-optionstrict[:custom]
```

## <a name="arguments"></a>Argumentos

`+` &#124; `-`  
Opcional. A `-optionstrict+` opção restringe a conversão implícita de tipo. O padrão para essa opção é `-optionstrict-` . A `-optionstrict+` opção é a mesma que `-optionstrict` . Você pode usar ambas as semânticas de tipo permissivas.

`custom`  
Obrigatórios. Avisar quando a semântica de linguagem estrita não for respeitada.

## <a name="remarks"></a>Comentários

Quando `-optionstrict+` está em vigor, apenas as conversões de tipo de ampliação podem ser feitas implicitamente. As conversões de tipo de estreitamento implícita, como a atribuição de um `Decimal` objeto de tipo a um objeto de tipo inteiro, são relatadas como erros.

Para gerar avisos para conversões de tipo de estreitamento implícita, use `-optionstrict:custom` . Use `-nowarn:numberlist` para ignorar avisos específicos e `-warnaserror:numberlist` tratar avisos específicos como erros.

### <a name="to-set--optionstrict-in-the-visual-studio-ide"></a>Para Set-optionstrict no IDE do Visual Studio

1. Selecione um projeto no **Gerenciador de Soluções**. No menu **projeto** , clique em **Propriedades.**

2. Clique na guia **Compilar**.

3. Modifique o valor na caixa **Option Strict** .

### <a name="to-set--optionstrict-programmatically"></a>Para Set-optionstrict programaticamente

Consulte a [instrução Option Strict](../../language-reference/statements/option-strict-statement.md).

## <a name="example"></a>Exemplo

O código a seguir compila `Test.vb` usando semântica de tipo estrito.

```console
vbc -optionstrict+ test.vb
```

## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [-optioncompare](optioncompare.md)
- [-optionexplicit](optionexplicit.md)
- [-optioninfer](optioninfer.md)
- [-nowarn](nowarn.md)
- [-warnaserror (Visual Basic)](warnaserror.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
- [Instrução Option Strict](../../language-reference/statements/option-strict-statement.md)
- [Caixa de diálogo Padrões do Visual Basic, Projetos, Opções](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
