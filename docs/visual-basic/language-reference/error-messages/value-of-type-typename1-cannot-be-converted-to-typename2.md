---
title: O valor do tipo '<typename1>' não pode ser convertido em '<typename2>'
ms.date: 07/20/2015
f1_keywords:
- vbc30955
- bc30955
helpviewer_keywords:
- BC30955
ms.assetid: 966b61eb-441e-48b0-bedf-ca95384ecb8b
ms.openlocfilehash: f6b35efbc445887c537b94dd299b317a28e5f689
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406554"
---
# <a name="value-of-type-typename1-cannot-be-converted-to-typename2"></a>O valor do tipo '\<typename1>' não pode ser convertido em '\<typename2>'
O valor do tipo ' \<typename1> ' não pode ser convertido em ' \<typename2> '. A incompatibilidade de tipos pode ser devido à mistura de uma referência de arquivo com uma referência de projeto para o assembly ' \<assemblyname> '. Tente substituir a referência de arquivo para ' \<filepath> ' no projeto ' \<projectname1> ' por uma referência de projeto para ' \<projectname2> '.  
  
 Em uma situação em que um projeto faz uma referência de projeto e uma referência de arquivo, o compilador não pode garantir que um tipo possa ser convertido em outro.  
  
 O pseudocódigo a seguir ilustra uma situação que pode gerar esse erro.  
  
 `' ================ Visual Basic project P1 ================`  
  
 `'        P1 makes a PROJECT REFERENCE to project P2`  
  
 `'        and a FILE REFERENCE to project P3.`  
  
 `Public commonObject As P3.commonClass`  
  
 `commonObject = P2.getCommonClass()`  
  
 `' ================ Visual Basic project P2 ================`  
  
 `'        P2 makes a PROJECT REFERENCE to project P3`  
  
 `Public Function getCommonClass() As P3.commonClass`  
  
 `Return New P3.commonClass`  
  
 `End Function`  
  
 `' ================ Visual Basic project P3 ================`  
  
 `Public Class commonClass`  
  
 `End Class`  
  
 O projeto `P1` faz uma referência de projeto indireta por meio do projeto `P2` ao projeto `P3` e também uma referência direta de arquivo ao `P3` . A declaração de `commonObject` usa a referência de arquivo para `P3` , enquanto a chamada para `P2.getCommonClass` usa a referência de projeto para `P3` .  
  
 O problema nessa situação é que a referência de arquivo especifica um caminho e nome de arquivo para o arquivo de saída de `P3` (normalmente P3. dll), enquanto o projeto faz referência a identificar o projeto de origem ( `P3` ) por nome de projeto. Por isso, o compilador não pode garantir que o tipo `P3.commonClass` venha do mesmo código-fonte por meio de duas referências diferentes.  
  
 Essa situação normalmente ocorre quando referências de projeto e referências de arquivo são misturadas. Na ilustração anterior, o problema não ocorreria se `P1` uma referência de projeto direta fosse feita em `P3` vez de uma referência de arquivo.  
  
 **ID do erro:** BC30955  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Altere a referência de arquivo para uma referência de projeto.  
  
## <a name="see-also"></a>Confira também

- [Conversões de tipo no Visual Basic](../../programming-guide/language-features/data-types/type-conversions.md)
- [Gerenciando referências em um projeto](/visualstudio/ide/managing-references-in-a-project)
