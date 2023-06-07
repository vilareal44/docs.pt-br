---
title: 'Como: discar modems anexados a portas seriais'
ms.date: 07/20/2015
helpviewer_keywords:
- modems [Visual Basic], dialing
- serial ports [Visual Basic], dialing
- My.Computer.Ports object
ms.assetid: 3834db40-f431-45f1-b671-dc91787164b6
ms.openlocfilehash: e55ce6399dae435fbd5b2f730d4d0848c98d8955
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363261"
---
# <a name="how-to-dial-modems-attached-to-serial-ports-in-visual-basic"></a>Como discar modems conectados a portas seriais no Visual Basic

Este tópico descreve como usar o `My.Computer.Ports` para discar um modem no Visual Basic.  
  
 Normalmente, o modem está conectado a uma das portas seriais no computador. Para que o seu aplicativo se comunique com o modem, é necessário enviar comandos à porta serial apropriada.  
  
### <a name="to-dial-a-modem"></a>Para discar um modem  
  
1. Determine a porta serial à qual o modem está conectado. Este exemplo supõe que o modem esteja na COM1.  
  
2. Use o método `My.Computer.Ports.OpenSerialPort` para obter uma referência para a porta. Para obter mais informações, consulte <xref:Microsoft.VisualBasic.Devices.Ports.OpenSerialPort%2A>.  
  
     O bloco `Using` permite que o aplicativo feche a porta serial mesmo que ele gere uma exceção. Todo o código que manipula a porta serial deve aparecer dentro deste bloco ou em um bloco `Try...Catch...Finally`.  
  
     [!code-vb[VbVbalrMyComputer#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#28)]  
  
3. Defina a propriedade `DtrEnable` para indicar que o computador está pronto para aceitar uma transmissão de entrada do modem.  
  
     [!code-vb[VbVbalrMyComputer#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#29)]  
  
4. Envie o comando de discagem e o número de telefone para o modem através da porta serial por meio do método <xref:System.IO.Ports.SerialPort.Write%2A>.  
  
     [!code-vb[VbVbalrMyComputer#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#30)]  
  
## <a name="example"></a>Exemplo  

 [!code-vb[VbVbalrMyComputer#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#27)]  
  
 Este exemplo de código também está disponível como um snippet de código do IntelliSense. No selecionador de snippet de código, ele está localizado em **Conectividade e Redes**. Para obter mais informações, consulte [Snippets de Código](/visualstudio/ide/code-snippets).  
  
## <a name="compiling-the-code"></a>Compilando o código  

 Este exemplo exige uma referência ao namespace <xref:System?displayProperty=nameWithType>.  
  
## <a name="robust-programming"></a>Programação robusta  

 Este exemplo supõe que o modem esteja conectado à porta COM1. É recomendável que seu código permita que o usuário selecione a porta serial desejada em uma lista de portas disponíveis. Para obter mais informações, consulte [Como mostrar portas seriais disponíveis](how-to-show-available-serial-ports.md).  
  
 Este exemplo usa um bloco `Using` para garantir que o aplicativo feche a porta mesmo que ele lance uma exceção. Para obter mais informações, consulte [usando a instrução](../../../language-reference/statements/using-statement.md).  
  
 Neste exemplo, o aplicativo desconecta a porta serial após discar o modem. Na verdade, você desejará transferir dados de/para o modem. Para obter mais informações, consulte [Como receber cadeias de caracteres de portas seriais](how-to-receive-strings-from-serial-ports.md).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Devices.Ports>
- <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType>
- [Como: enviar cadeias de caracteres para portas seriais](how-to-send-strings-to-serial-ports.md)
- [Como: receber cadeias de caracteres de portas seriais](how-to-receive-strings-from-serial-ports.md)
- [Como: mostrar portas seriais disponíveis](how-to-show-available-serial-ports.md)
