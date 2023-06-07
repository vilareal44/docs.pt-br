---
title: -define
ms.date: 03/10/2018
helpviewer_keywords:
- -d compiler option [Visual Basic]
- /d compiler option [Visual Basic]
- -define compiler option [Visual Basic]
- d compiler option [Visual Basic]
- /define compiler option [Visual Basic]
- define compiler option [Visual Basic]
ms.assetid: f735c57d-1cf9-4f2f-a26f-0de630fd4077
ms.openlocfilehash: d0d1b03d9ab98f28a0112198f1ecc1e928d6d4a7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408706"
---
# <a name="-define-visual-basic"></a>-definir (Visual Basic)
Define as constantes de compilador condicional.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-define:["]symbol[=value][,symbol[=value]]["]  
```

ou

```console  
-d:["]symbol[=value][,symbol[=value]]["]  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`symbol`|Obrigatórios. O símbolo a ser definido.|  
|`value`|Opcional. O valor para atribuir `symbol`. Se `value` for uma cadeia de caracteres, ela deverá estar entre as sequências de barra invertida/aspas ( \\ ") em vez de aspas. Se nenhum valor for especificado, será considerado como True.|  
  
## <a name="remarks"></a>Comentários  
 A `-define` opção tem um efeito semelhante ao uso de uma `#Const` diretiva de pré-processador em seu arquivo de origem, exceto que as constantes definidas com `-define` são públicas e se aplicam a todos os arquivos no projeto.  
  
 Você pode usar símbolos criados por essa opção com a diretiva `#If`...`Then`...`#Else` para compilar os arquivos de origem condicionalmente.  
  
 `-d` é a forma abreviada de `-define`.  
  
 Você pode definir vários símbolos com `-define` usando uma vírgula para separar as definições de símbolos.  
  
|Para definir-definir no ambiente de desenvolvimento integrado do Visual Studio|  
|---|  
|1. ter um projeto selecionado em **Gerenciador de soluções**. No menu **Projeto** , clique em **Propriedades**. <br />2. Clique na guia **Compilar** .<br />3. clique em **avançado**.<br />4. modifique o valor na caixa **constantes personalizadas** .|  
  
## <a name="example"></a>Exemplo  
 O código a seguir define e usa duas constantes de compilador condicional.  
  
 [!code-vb[VbVbalrCompiler#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/Class1.vb#45)]  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [#If... Diretivas then... #Else](../../language-reference/directives/if-then-else-directives.md)
- [#Const diretiva](../../language-reference/directives/const-directive.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
