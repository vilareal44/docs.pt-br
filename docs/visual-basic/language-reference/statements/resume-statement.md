---
title: Instrução Resume
ms.date: 07/20/2015
f1_keywords:
- vb.Resume
- vb.ResumeNext
helpviewer_keywords:
- Next statement [Visual Basic], Resume
- Resume Next statement [Visual Basic]
- execution [Visual Basic], resuming
- run-time errors [Visual Basic], resuming after
- Resume statement [Visual Basic], syntax
- errors [Visual Basic], resuming after
- Error statement [Visual Basic], and Resume statement
- execution
- Resume statement [Visual Basic]
ms.assetid: e24d058b-1a5c-4274-acb9-7d295d3ea537
ms.openlocfilehash: 3f49f05f1deb2027b03bbf3443ca44f30c44344e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404207"
---
# <a name="resume-statement"></a>Instrução Resume
Retoma a execução após a conclusão de uma rotina de tratamento de erros.  
  
 Sugerimos que você use manipulação de exceção estruturada em seu código sempre que possível, em vez de usar a manipulação de exceção não estruturada e as `On Error` `Resume` instruções e. Para obter mais informações, consulte [Instrução Try...Catch...Finally](try-catch-finally-statement.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Resume [ Next | line ]  
```  
  
## <a name="parts"></a>Partes  
 `Resume`  
 Obrigatórios. Se o erro ocorreu no mesmo procedimento que o manipulador de erros, a execução é retomada com a instrução que causou o erro. Se o erro ocorreu em um procedimento chamado, a execução será retomada na instrução última chamada para fora do procedimento que contém a rotina de tratamento de erros.  
  
 `Next`  
 Opcional. Se o erro ocorreu no mesmo procedimento que o manipulador de erros, a execução é retomada com a instrução imediatamente após a instrução que causou o erro. Se o erro ocorreu em um procedimento chamado, a execução será retomada com a instrução imediatamente após a instrução que a última chamada para fora do procedimento que contém a rotina de tratamento de erros (ou a `On Error Resume Next` instrução).  
  
 `line`  
 Opcional. A execução é retomada na linha especificada no `line` argumento necessário. O `line` argumento é um rótulo de linha ou número de linha e deve estar no mesmo procedimento que o manipulador de erro.  
  
## <a name="remarks"></a>Comentários  
  
> [!NOTE]
> É recomendável que você use manipulação de exceção estruturada em seu código sempre que possível, em vez de usar a manipulação de exceção não estruturada e as `On Error` `Resume` instruções e. Para obter mais informações, consulte [Instrução Try...Catch...Finally](try-catch-finally-statement.md).  
  
 Se você usar uma `Resume` instrução em qualquer lugar que não seja em uma rotina de tratamento de erros, ocorrerá um erro.  
  
 A `Resume` instrução não pode ser usada em nenhum procedimento que contenha uma `Try...Catch...Finally` instrução.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa a `Resume` instrução para encerrar o tratamento de erros em um procedimento e, em seguida, retomar a execução com a instrução que causou o erro. O número de erro 55 é gerado para ilustrar o uso da `Resume` instrução.  
  
 [!code-vb[VbVbalrErrorHandling#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrErrorHandling/VB/Class1.vb#16)]  
  
## <a name="requirements"></a>Requisitos  
 **Namespace:** [Microsoft. VisualBasic](../runtime-library-members.md)  
  
 **Assembly:** Visual Basic a biblioteca de tempo de execução (em Microsoft. VisualBasic. dll)  
  
## <a name="see-also"></a>Confira também

- [Instrução Try...Catch...Finally](try-catch-finally-statement.md)
- [Instrução Error](error-statement.md)
- [Instrução On Error](on-error-statement.md)
