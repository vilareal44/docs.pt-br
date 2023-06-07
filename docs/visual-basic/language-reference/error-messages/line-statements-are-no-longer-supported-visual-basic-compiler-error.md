---
title: Instruções 'Line' não são mais compatíveis (erro de compilador do Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- bc30830
- vbc30830
helpviewer_keywords:
- BC30830
ms.assetid: 4734bc1d-882e-4555-b498-1f1ec0399d16
ms.openlocfilehash: a3d243f39f3fc45ca6b1ba0d26892d4c3db56f59
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397292"
---
# <a name="line-statements-are-no-longer-supported-visual-basic-compiler-error"></a>Instruções 'Line' não são mais compatíveis (erro de compilador do Visual Basic)
Não há mais suporte para instruções de linha. A funcionalidade de e/s de arquivo está disponível `Microsoft.VisualBasic.FileSystem.LineInput` e a funcionalidade de gráficos está disponível como `System.Drawing.Graphics.DrawLine` .  
  
 **ID do erro:** BC30830  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Se estiver executando o acesso ao arquivo, use `Microsoft.VisualBasic.FileSystem.LineInput` .  
  
2. Se estiver executando gráficos, use `System.Drawing.Graphics.Drawline` .  
  
## <a name="see-also"></a>Confira também

- <xref:System.IO>
- <xref:System.Drawing>
- [Access de arquivo com o Visual Basic](../../developing-apps/programming/drives-directories-files/file-access.md)
