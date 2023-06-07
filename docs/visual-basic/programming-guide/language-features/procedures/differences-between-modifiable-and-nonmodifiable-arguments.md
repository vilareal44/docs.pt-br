---
title: Diferenças entre argumentos modificáveis e não modificáveis
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedure arguments
- arguments [Visual Basic], nonmodifiable
- Visual Basic code, procedures
- arguments [Visual Basic], modifiable
ms.assetid: 87b2df69-e1f7-4657-9caf-b3f48d693428
ms.openlocfilehash: 733f92cc2cdaa6e923c57649774ceb64de172c18
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403337"
---
# <a name="differences-between-modifiable-and-nonmodifiable-arguments-visual-basic"></a>Diferenças entre argumentos modificáveis e não modificáveis (Visual Basic)
Quando você chama um procedimento, normalmente passa um ou mais argumentos para ele. Cada argumento corresponde a um elemento de programação subjacente. Os elementos subjacentes e os próprios argumentos podem ser modificáveis ou não modificáveis.  
  
## <a name="modifiable-and-nonmodifiable-elements"></a>Elementos modificáveis e não modificáveis  
 Um elemento de programação pode ser um *elemento modificável*, que pode ter seu valor alterado ou um *elemento não modificável*, que tem um valor fixo depois de ter sido criado.  
  
 A tabela a seguir lista elementos de programação modificáveis e não modificáveis.  
  
|Elementos modificáveis|Elementos não modificáveis|  
|-------------------------|----------------------------|  
|Variáveis locais (declaradas dentro de procedimentos), incluindo variáveis de objeto, exceto para somente leitura|Variáveis, campos e propriedades somente leitura|  
|Campos (variáveis de membro de módulos, classes e estruturas), exceto para somente leitura|Constantes e literais|  
|Propriedades, exceto para somente leitura|Membros de enumeração|  
|Elementos da matriz|Expressões (mesmo que seus elementos sejam modificáveis)|  
  
## <a name="modifiable-and-nonmodifiable-arguments"></a>Argumentos modificáveis e não modificáveis  
 Um *argumento modificável* é um com um elemento subjacente modificável. O código de chamada pode armazenar um novo valor a qualquer momento e, se você passar o argumento [ByRef](../../../language-reference/modifiers/byref.md), o código no procedimento também poderá modificar o elemento subjacente no código de chamada.  
  
 Um *argumento não modificável* tem um elemento subjacente não modificável ou é passado como [ByVal](../../../language-reference/modifiers/byval.md). O procedimento não pode modificar o elemento subjacente no código de chamada, mesmo que ele seja um elemento modificável. Se for um elemento não modificável, o código de chamada em si não poderá modificá-lo.  
  
 O procedimento chamado pode modificar sua cópia local de um argumento não modificável, mas essa modificação não afeta o elemento subjacente no código de chamada.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Como passar argumentos para um procedimento](./how-to-pass-arguments-to-a-procedure.md)
- [Passar argumentos por valor e por referência](./passing-arguments-by-value-and-by-reference.md)
- [Diferenças entre passar um argumento por valor e por referência](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [Como alterar o valor de um argumento de procedimento](./how-to-change-the-value-of-a-procedure-argument.md)
- [Como proteger um argumento de procedimento contra alterações de valor](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [Como forçar um argumento a ser passado por Valor](./how-to-force-an-argument-to-be-passed-by-value.md)
- [Passando argumentos por posição e nome](./passing-arguments-by-position-and-by-name.md)
- [Tipos de valor e referência](../data-types/value-types-and-reference-types.md)
