---
title: "'<elementname>' está obsoleto (aviso do Visual Basic)"
ms.date: 07/20/2015
f1_keywords:
- vbc40008
- bc40008
helpviewer_keywords:
- BC40008
ms.assetid: 729e3eb5-76ac-4c55-9fdd-78350e0de55e
ms.openlocfilehash: 7914bc859966e17f3da41c9a13a01573b31baf91
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409667"
---
# <a name="elementname-is-obsolete-visual-basic-warning"></a>'\<elementname>' está obsoleto (aviso do Visual Basic)
Uma instrução tenta acessar um elemento de programação que foi marcado com o <xref:System.ObsoleteAttribute> atributo e a diretiva para tratá-lo como um aviso.  
  
 Você pode marcar qualquer elemento de programação como não sendo mais usado aplicando <xref:System.ObsoleteAttribute> -se a ele. Se você fizer isso, poderá definir a propriedade do atributo <xref:System.ObsoleteAttribute.IsError%2A> como `True` ou `False` . Se você defini-lo como `True` , o compilador tratará uma tentativa de usar o elemento como um erro. Se você defini-lo como `False` , ou deixá-lo como padrão `False` , o compilador emitirá um aviso se houver uma tentativa de usar o elemento.  
  
 Por padrão, essa mensagem é um aviso, pois a <xref:System.ObsoleteAttribute.IsError%2A> propriedade de <xref:System.ObsoleteAttribute> é `False` . Para obter mais informações sobre como ocultar avisos ou tratar avisos como erros, consulte [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **ID do erro:** BC40008  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Certifique-se de que a referência de código-fonte esteja soletrando o nome do elemento corretamente.  
  
## <a name="see-also"></a>Confira também

- [Visão geral de atributos](../../programming-guide/concepts/attributes/index.md)
