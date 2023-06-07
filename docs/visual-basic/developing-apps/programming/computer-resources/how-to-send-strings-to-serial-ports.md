---
title: 'Como: enviar cadeias de caracteres para portas seriais'
ms.date: 07/20/2015
helpviewer_keywords:
- ports, sending strings to
- strings [Visual Basic], sending to serial ports
- My.Computer.Ports object
- serial ports, sending strings to
ms.assetid: 6ebf46cd-b2d0-4b2c-9a1f-be177b22ad52
ms.openlocfilehash: f78df9cf1bd75432ea645c4dcc06498915ceee49
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84360287"
---
# <a name="how-to-send-strings-to-serial-ports-in-visual-basic"></a>Como enviar cadeias de caracteres para portas seriais no Visual Basic

Este tópico descreve como usar o `My.Computer.Ports` para enviar cadeias de caracteres para portas seriais do computador em Visual Basic.  
  
## <a name="example"></a>Exemplo  

 Este exemplo envia uma cadeia de caracteres para a porta serial COM1. Talvez você precise usar uma porta serial diferente em seu computador.  
  
 Use o método `My.Computer.Ports.OpenSerialPort` para obter uma referência para a porta. Para obter mais informações, consulte <xref:Microsoft.VisualBasic.Devices.Ports.OpenSerialPort%2A>.  
  
 O bloco `Using` permite que o aplicativo feche a porta serial mesmo que ele gere uma exceção. Todo o código que manipula a porta serial deve aparecer dentro deste bloco ou em um bloco `Try...Catch...Finally`.  
  
 O método <xref:System.IO.Ports.SerialPort.WriteLine%2A> envia os dados à porta serial.  
  
 [!code-vb[VbVbalrMyComputer#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#33)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
  
- Este exemplo pressupõe que o computador esteja usando a `COM1`.  
  
## <a name="robust-programming"></a>Programação robusta  

 Este exemplo pressupõe que o computador esteja usando a `COM1`. Para obter mais flexibilidade, o código deve permitir que o usuário selecione a porta serial desejada na lista de portas disponíveis. Para obter mais informações, consulte [Como mostrar portas seriais disponíveis](how-to-show-available-serial-ports.md).  
  
 Este exemplo usa um bloco `Using` para garantir que o aplicativo feche a porta mesmo que ele lance uma exceção. Para obter mais informações, consulte [usando a instrução](../../../language-reference/statements/using-statement.md).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Devices.Ports>
- <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType>
- [Como: discar modems anexados a portas seriais](how-to-dial-modems-attached-to-serial-ports.md)
- [Como: mostrar portas seriais disponíveis](how-to-show-available-serial-ports.md)
