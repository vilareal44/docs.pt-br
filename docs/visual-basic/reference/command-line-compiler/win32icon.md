---
title: -win32icon
ms.date: 03/13/2018
helpviewer_keywords:
- win32icon compiler option [Visual Basic]
- -win32icon compiler option [Visual Basic]
- /win32icon compiler option [Visual Basic]
ms.assetid: aecaab01-9353-46c5-941c-6edabd4eff92
ms.openlocfilehash: 52ef0b991554c800dba4320b0c64c81ddd1b0ff4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414279"
---
# <a name="-win32icon"></a>-win32icon
Insere um arquivo. ico no arquivo de saída. Esse arquivo. ico representa o arquivo de saída no **Explorador de arquivos**.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-win32icon:filename  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`filename`|O arquivo. ico a ser adicionado ao arquivo de saída. Coloque o nome de arquivo entre aspas ("") se ele contiver um espaço.|  
  
## <a name="remarks"></a>Comentários  
 Você pode criar um arquivo. ico com o compilador de recursos do Microsoft Windows (RC). O compilador de recursos é invocado quando você compila um programa de Visual C++; um arquivo. ico é criado a partir do arquivo. rc. As opções `-win32icon` e `-win32resource` são mutualmente exclusivas.  
  
 Consulte [-linkresource (Visual Basic)](linkresource.md) para fazer referência a um arquivo de recurso .NET Framework ou [-Resource (Visual Basic)](resource.md) para anexar um arquivo de recurso de .NET Framework. Consulte [-Win32Resource](win32resource.md) para importar um arquivo. res.  
  
|Para definir-win32icon no IDE do Visual Studio|  
|---|  
|1. ter um projeto selecionado em **Gerenciador de soluções**. No menu **Projeto** , clique em **Propriedades**. <br />2. Clique na guia **aplicativo** .<br />3. modifique o valor na caixa **ícone** .|  
  
## <a name="example"></a>Exemplo  
 O código a seguir compila `In.vb` e anexa um arquivo. ico, `Rf.ico` .  
  
```console
vbc -win32icon:rf.ico in.vb  
```  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
