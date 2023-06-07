---
title: Esta matriz é fixa ou está temporariamente bloqueada
ms.date: 07/20/2015
f1_keywords:
- vbrID10
ms.assetid: de6713a6-51d7-4edb-8515-d5fb544e2091
ms.openlocfilehash: 4a86460104b6c4d9d6791e60f6f377cec0030425
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363027"
---
# <a name="this-array-is-fixed-or-temporarily-locked-visual-basic"></a>Esta matriz é fixa ou está temporariamente bloqueada (Visual Basic)
Esse erro tem as seguintes causas possíveis:  
  
- Usando `ReDim` para alterar o número de elementos de uma matriz de tamanho fixo.  
  
- Redimensionamento de uma matriz dinâmica em nível de módulo, na qual um elemento foi passado como um argumento para um procedimento. Se um elemento for passado, a matriz será bloqueada para evitar a desalocação de memória para o parâmetro de referência no procedimento.  
  
- Tentativa de atribuir um valor a uma `Variant` variável que contém uma matriz, mas o `Variant` está bloqueado no momento.  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Torne a matriz original dinâmica, em vez de corrigida, declarando-a com `ReDim` (se a matriz for declarada dentro de um procedimento) ou declarando-a sem especificar o número de elementos (se a matriz for declarada no nível do módulo.  
  
2. Determine se você realmente precisa passar o elemento, pois ele está visível em todos os procedimentos no módulo.  
  
3. Determine o que está bloqueando o `Variant` e corrigi-lo.  
  
## <a name="see-also"></a>Confira também

- [Matrizes](../../programming-guide/language-features/arrays/index.md)
