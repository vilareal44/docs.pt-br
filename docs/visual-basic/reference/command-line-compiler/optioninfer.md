---
title: -optioninfer
ms.date: 07/20/2015
f1_keywords:
- -optioninfer
helpviewer_keywords:
- -optioninfer compiler option [Visual Basic]
- /optioninfer compiler option [Visual Basic]
- optioninfer compiler option [Visual Basic]
ms.assetid: f6c09db1-0553-464a-abe3-d4510c61d6ed
ms.openlocfilehash: 524660fca7c56fa490cc85169898bf2bf6d1a16e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400572"
---
# <a name="-optioninfer"></a>-optioninfer
Permite o uso de inferência de tipo local nas declarações de variáveis.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-optioninfer[+ | -]  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`+` &#124; `-`|Opcional. Especifique `-optioninfer+` para habilitar a inferência de tipo de local ou `-optioninfer-` para bloqueá-la. A opção `-optioninfer`, sem valor especificado, é a mesma de `-optioninfer+`. O valor padrão quando o comutador `-optioninfer` não estiver presente também é `-optioninfer+`. O valor padrão é definido no arquivo de resposta Vbc.rsp.|  
  
> [!NOTE]
> Você pode usar a opção `-noconfig` para manter os padrões internos do compilador em vez dos especificados no vbc.rsp. O padrão do compilador para essa opção é `-optioninfer-`.  
  
## <a name="remarks"></a>Comentários  
 Se o arquivo de código-fonte contiver uma [instrução Option Infer](../../language-reference/statements/option-infer-statement.md), a instrução substituirá a `-optioninfer` configuração do compilador de linha de comando.  
  
### <a name="to-set--optioninfer-in-the-visual-studio-ide"></a>Para Set-optioninfer no IDE do Visual Studio  
  
1. Selecione um projeto no **Gerenciador de soluções**. No menu **Projeto** , clique em **Propriedades**.  
  
2. Na guia **Compilar** , modifique o valor na caixa **Opção Infer** .  
  
## <a name="example"></a>Exemplo  
 O seguinte código compila `test.vb` com a inferência de tipo local habilitada.  
  
```console
vbc -optioninfer+ test.vb  
```  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [-optioncompare](optioncompare.md)
- [-optionexplicit](optionexplicit.md)
- [-optionstrict](optionstrict.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
- [Instrução Option Infer](../../language-reference/statements/option-infer-statement.md)
- [Inferência de Tipo de Variável Local](../../programming-guide/language-features/variables/local-type-inference.md)
- [Caixa de diálogo Padrões do Visual Basic, Projetos, Opções](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
- [Página de Compilação, Designer de Projeto (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)
- [-noconfig](noconfig.md)
- [Compilando da Linha de Comando](building-from-the-command-line.md)
