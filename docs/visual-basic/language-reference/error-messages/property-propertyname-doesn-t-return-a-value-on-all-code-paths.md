---
title: A propriedade '<propertyname>' não retorna um valor em todos os caminhos de código
ms.date: 07/20/2015
f1_keywords:
- bc42107
- vbc42107
helpviewer_keywords:
- BC42107
ms.assetid: 06800966-9c3b-4844-9f13-83ac95607d32
ms.openlocfilehash: 21a1c4dbab6e26cd1cb848e270bbda9a544c2a67
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400416"
---
# <a name="property-propertyname-doesnt-return-a-value-on-all-code-paths"></a>A propriedade '\<propertyname>' não retorna um valor em todos os caminhos de código
A propriedade ' \<propertyname> ' não retorna um valor em todos os caminhos de código. Uma exceção de referência nula pode ocorrer em tempo de execução quando o resultado é usado.  
  
 Um procedimento de propriedade `Get` tem pelo menos um caminho possível por meio de seu código que não retorna um valor.  
  
 Você pode retornar um valor de um procedimento de propriedade de `Get` qualquer uma das seguintes maneiras:  
  
- Atribua o valor ao nome da propriedade e, em seguida, execute uma `Exit Property` instrução.  
  
- Atribua o valor ao nome da propriedade e, em seguida, execute a `End Get` instrução.  
  
- Inclua o valor em uma [instrução return](../statements/return-statement.md).  
  
 Se o controle passar para `Exit Property` ou `End Get` e você não tiver atribuído nenhum valor ao nome da propriedade, o `Get` procedimento retornará o valor padrão do tipo de dados da propriedade. Para obter mais informações, consulte "Behavior" na [instrução function](../statements/function-statement.md).  
  
 Por padrão, esta mensagem é um aviso. Para obter mais informações sobre como ocultar avisos ou tratar avisos como erros, consulte [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **ID do erro:** BC42107  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Verifique sua lógica de fluxo de controle e certifique-se de atribuir um valor antes de cada instrução que causa um retorno.  
  
     É mais fácil garantir que cada retorno do procedimento retorne um valor se você sempre usar a `Return` instrução. Se você fizer isso, a última instrução antes `End Get` deve ser uma `Return` instrução.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos de propriedade](../../programming-guide/language-features/procedures/property-procedures.md)
- [Instrução Property](../statements/property-statement.md)
- [Instrução Get](../statements/get-statement.md)
