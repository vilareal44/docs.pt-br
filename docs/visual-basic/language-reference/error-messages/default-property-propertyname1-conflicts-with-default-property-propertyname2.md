---
title: A propriedade padrão '<propertyname1>' está em conflito com a propriedade padrão '<propertyname2>' em '<classname>' e por isso deve ser declarada como 'Shadows'
ms.date: 07/20/2015
f1_keywords:
- vbc40007
- bc40007
helpviewer_keywords:
- BC40007
ms.assetid: 692ccf76-5715-4f11-a972-84cf9de30bc1
ms.openlocfilehash: 37f98ce8120d5861552819690f9d5f22c9959a0e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409719"
---
# <a name="default-property-propertyname1-conflicts-with-default-property-propertyname2-in-classname-and-so-should-be-declared-shadows"></a>A propriedade padrão '\<propertyname1>' está em conflito com a propriedade padrão '\<propertyname2>' em '\<classname>' e por isso deve ser declarada como 'Shadows'
Uma propriedade é declarada com o mesmo nome de uma propriedade definida na classe base. Nessa situação, a propriedade nessa classe deve sombrear a propriedade da classe base.  
  
 Esta mensagem é um aviso. `Shadows`é assumido por padrão. Para obter mais informações sobre como ocultar avisos ou tratar avisos como erros, consulte [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **ID do erro:** BC40007  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Adicione a `Shadows` palavra-chave à declaração ou altere o nome da propriedade que está sendo declarada.  
  
## <a name="see-also"></a>Confira também

- [Sombras](../modifiers/shadows.md)
- [Sombreamento no Visual Basic](../../programming-guide/language-features/declared-elements/shadowing.md)
