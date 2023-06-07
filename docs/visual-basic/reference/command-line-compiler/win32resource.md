---
title: -win32resource
ms.date: 03/13/2018
f1_keywords:
- -win32resource
- win32resource
helpviewer_keywords:
- /win32resource compiler option [Visual Basic]
- -win32resource compiler option [Visual Basic]
- win32resource compiler option [Visual Basic]
ms.assetid: e226946d-19ce-4cc9-91f5-aed24f77aa2b
ms.openlocfilehash: bcbc690690993a094bc5360d0c13bddebf8cd615
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414240"
---
# <a name="-win32resource"></a>-win32resource
Insere um arquivo de recurso do Win32 no arquivo de saída.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-win32resource:filename  
```  
  
## <a name="arguments"></a>Argumentos  
 `filename`  
 O nome do arquivo de recurso a ser adicionado ao arquivo de saída. Coloque o nome de arquivo entre aspas ("") se ele contiver um espaço.  
  
## <a name="remarks"></a>Comentários  
 Você pode criar um arquivo de recurso do Win32 com o compilador de recursos do Microsoft Windows (RC).  
  
 Um recurso do Win32 pode conter informações de versão ou bitmap (ícone) que ajudam a identificar seu aplicativo no **Explorador de arquivos**. Se você não especificar `-win32resource` , o compilador gerará informações de versão com base na versão do assembly. As opções `-win32resource` e `-win32icon` são mutualmente exclusivas.  
  
 Consulte [-linkresource (Visual Basic)](linkresource.md) para fazer referência a um arquivo de recurso .NET Framework ou [-Resource (Visual Basic)](resource.md) para anexar um arquivo de recurso de .NET Framework.  
  
> [!NOTE]
> A `-win32resource` opção não está disponível no ambiente de desenvolvimento do Visual Studio; ela está disponível somente durante a compilação na linha de comando.  
  
## <a name="example"></a>Exemplo  
 O código a seguir compila `In.vb` e anexa um arquivo de recurso do Win32, `Rf.res` :  
  
```console  
vbc -win32resource:rf.res in.vb  
```  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
