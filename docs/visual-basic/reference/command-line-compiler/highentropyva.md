---
title: -highentropyva
ms.date: 03/10/2018
helpviewer_keywords:
- highentropyva compiler option (Visual Basic)
- /highentropyva compiler option (Visual Basic)
ms.assetid: ff25f20a-6ca2-467b-9e52-5cf439f5028e
ms.openlocfilehash: 9501ea46eb13baa171208e20d0c9645d118c4301
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408615"
---
# <a name="-highentropyva-visual-basic"></a>-highentropyva (Visual Basic)
Indica se um executável de 64 bits ou um executável que é marcado pela opção de compilador [-Platform: anycpu](platform.md) dá suporte à alta possibilidade de seleção de layout de espaço de endereço de entropia (ASLR).  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-highentropyva[+ | -]  
```  
  
## <a name="arguments"></a>Argumentos  
 `+` &#124; `-`  
 Opcional. A opção está desativada por padrão ou, se você especificar `-highentropyva-` . A opção estará ativada se você especificar `-highentropyva` ou `-highentropyva+` .  
  
## <a name="remarks"></a>Comentários  
 Se você especificar essa opção, as versões compatíveis do kernel do Windows poderão usar graus mais altos de entropia quando o kernel distribui o layout de espaço de endereço de um processo como parte de ASLR. Se o kernel usar graus maiores de entropia, um número maior de endereços poderá ser alocado para regiões de memória, como pilhas e heaps. Como resultado, é mais difícil adivinhar a localização de uma região específica da memória.  
  
 Quando a opção está ativada, o executável de destino e todos os módulos dos quais depende devem ser capazes de lidar com valores de ponteiro maiores que 4 gigabytes (GB) quando esses módulos estiverem sendo executados como processos de 64 bits.  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
