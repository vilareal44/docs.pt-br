---
title: -addmodule
ms.date: 03/09/2018
helpviewer_keywords:
- /addmodule compiler option [Visual Basic]
- addmodule compiler option [Visual Basic]
- -addmodule compiler option [Visual Basic]
ms.assetid: fb4b89d4-4926-4f20-868d-427fa28497b2
ms.openlocfilehash: 9e8146497d63d949f138d6cd08c9ea8c7b03c651
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414305"
---
# <a name="-addmodule"></a>-addmodule
Faz com que o compilador verifique todos os tipos de informações de arquivos especificados disponíveis para o projeto que você está compilando.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-addmodule:fileList  
```  
  
## <a name="arguments"></a>Argumentos  
 `fileList`  
 Obrigatórios. Lista delimitada por vírgulas de arquivos que contêm metadados, mas não contêm manifestos de assembly. Os nomes de arquivo que contêm espaços devem estar entre aspas ("").  
  
## <a name="remarks"></a>Comentários  
 Os arquivos listados pelo `fileList` parâmetro devem ser criados com a `-target:module` opção ou com o equivalente de outro compilador `-target:module` .  
  
 Todos os módulos adicionados com `-addmodule` devem estar no mesmo diretório que o arquivo de saída em tempo de execução. Ou seja, você pode especificar um módulo em qualquer diretório em tempo de compilação, mas o módulo deve estar no diretório do aplicativo em tempo de execução. Se não for, você receberá um <xref:System.TypeLoadException> erro.  
  
 Se você especificar (implícita ou explicitamente) qualquer opção[-Target (Visual Basic)](target.md) diferente de `-target:module` with `-addmodule` , os arquivos passados para `-addmodule` se tornarão parte do assembly do projeto. Um assembly é necessário para executar um arquivo de saída que tenha um ou mais arquivos adicionados com o `-addmodule` .  
  
 Use [-Reference (Visual Basic)](reference.md) para importar metadados de um arquivo que contém um assembly.  
  
> [!NOTE]
> A `-addmodule` opção não está disponível no ambiente de desenvolvimento do Visual Studio; ela está disponível somente durante a compilação na linha de comando.  
  
## <a name="example"></a>Exemplo  
 O código a seguir cria um módulo.  
  
 [!code-vb[VbVbalrCompiler#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#47)]  
  
 O código a seguir importa os tipos do módulo.  
  
 [!code-vb[VbVbalrCompiler#48](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#48)]  
  
 Quando você executa `t1` , ele gera `802` .  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [-Target (Visual Basic)](target.md)
- [-referência (Visual Basic)](reference.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
