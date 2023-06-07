---
title: -libpath
ms.date: 03/10/2018
helpviewer_keywords:
- libpath compiler option [Visual Basic]
- /libpath compiler option [Visual Basic]
- -libpath compiler option [Visual Basic]
ms.assetid: 5f1c26c9-3455-4e89-bdf3-b12d6c2e655b
ms.openlocfilehash: dff7e0c3eb696b9b18f4c4e59240a26c1cb9782c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408530"
---
# <a name="-libpath"></a>-libpath
Especifica o local dos assemblies referenciados.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-libpath:dirList  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`dirList`|Obrigatórios. Lista delimitada por ponto-e-vírgula de diretórios para o compilador examinar se um assembly referenciado não for encontrado no diretório de trabalho atual (o diretório do qual você está invocando o compilador) ou o diretório do sistema do Common Language Runtime. Se o nome do diretório contiver um espaço, coloque o nome entre aspas ("").|  
  
## <a name="remarks"></a>Comentários  
 A `-libpath` opção especifica o local dos assemblies referenciados pela opção [-Reference](reference.md) .  
  
 O compilador pesquisa referências de assembly que não são totalmente qualificadas na seguinte ordem:  
  
1. Diretório de trabalho atual. Esse é o diretório do qual o compilador é invocado.  
  
2. O diretório de sistema do Common Language Runtime.  
  
3. Diretórios especificados por `-libpath` .  
  
4. Diretórios especificados pela variável de ambiente LIB.  
  
 A `-libpath` opção é aditiva; especificá-la mais de uma vez para todos os valores anteriores.  
  
 Use `-reference` para especificar uma referência de assembly.  
  
|Para Set-LIBPATH no ambiente de desenvolvimento integrado do Visual Studio|  
|---|  
|1. ter um projeto selecionado em **Gerenciador de soluções**. No menu **Projeto** , clique em **Propriedades**. <br />2. Clique na guia **referências** .<br />3. Clique no botão **Reference Paths...** .<br />4. na caixa de diálogo **caminhos de referência** , digite o nome do diretório na caixa **pasta:** .<br />5. clique em **Adicionar pasta**.|  
  
## <a name="example"></a>Exemplo  
 O código a seguir é compilado `T2.vb` para criar um arquivo. exe. O compilador procura no diretório de trabalho, no diretório raiz da unidade C:, e no novo diretório assemblies da unidade C: para referências de assembly.  
  
```console  
vbc -libpath:c:\;"c:\New Assemblies" -reference:t2.dll t2.vb  
```  
  
## <a name="see-also"></a>Confira também

- [Assemblies no .NET](../../../standard/assembly/index.md)
- [Compilador de linha de comando do Visual Basic](index.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
