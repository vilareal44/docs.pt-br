---
title: A primeira instrução deste ' Sub New ' deve ser uma chamada para 'MyBase.New' ou 'MyClass.New' (nenhum construtor acessível sem parâmetros)
ms.date: 07/20/2015
f1_keywords:
- bc30148
- vbc30148
helpviewer_keywords:
- BC30148
ms.assetid: 4426e8fc-cb39-4eb8-ba95-503cd32fcc89
ms.openlocfilehash: 2b9d2568fb64e4af72733ad1f3dee58aaee650e5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402973"
---
# <a name="first-statement-of-this-sub-new-must-be-a-call-to-mybasenew-or-myclassnew-no-accessible-constructor-without-parameters"></a>A primeira instrução deste ' Sub New ' deve ser uma chamada para 'MyBase.New' ou 'MyClass.New' (nenhum construtor acessível sem parâmetros)
A primeira instrução deste "Sub New" deve ser uma chamada para "MyBase. New" ou "MyClass. New" porque a classe base " \<basename> " de " \<derivedname> " não tem um "Sub New" acessível que pode ser chamado sem argumentos.  
  
 Em uma classe derivada, cada Construtor deve chamar um construtor de classe base ( `MyBase.New` ). Se a classe base tiver um construtor sem parâmetros que seja acessível a classes derivadas, o `MyBase.New` poderá ser chamado automaticamente. Caso contrário, um construtor de classe base deve ser chamado com parâmetros e isso não pode ser feito automaticamente. Nesse caso, a primeira instrução de cada Construtor de classe derivada deve chamar um construtor com parâmetros na classe base ou chamar outro construtor na classe derivada que faz uma chamada de construtor de classe base.  
  
 **ID do erro:** BC30148  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Chame `MyBase.New` o fornecimento dos parâmetros necessários ou chame um construtor de pares que faça tal chamada.  
  
     Por exemplo, se a classe base tiver um construtor declarado como `Public Sub New(ByVal index as Integer)` , a primeira instrução no construtor de classe derivada poderá ser `MyBase.New(100)` .  
  
## <a name="see-also"></a>Confira também

- [Noções básicas de herança](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
