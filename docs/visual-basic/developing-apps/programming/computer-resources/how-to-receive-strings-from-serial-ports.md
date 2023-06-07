---
title: 'Como: receber cadeias de caracteres de portas seriais'
ms.date: 07/20/2015
helpviewer_keywords:
- serial ports, retrieving strings
- strings [Visual Basic], retrieving from serial ports
- My.Resources object
ms.assetid: 8371ce2c-e1c7-476b-a86d-9afc2614b6b7
ms.openlocfilehash: 93b6b47d89d05331c85a6459bba7d6fd5e2e3377
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401830"
---
# <a name="how-to-receive-strings-from-serial-ports-in-visual-basic"></a>Como receber cadeias de caracteres de portas seriais no Visual Basic

Este tópico descreve como usar o `My.Computer.Ports` para receber cadeias de caracteres de portas seriais do computador em Visual Basic.  
  
### <a name="to-receive-strings-from-the-serial-port"></a>Para receber cadeias de caracteres da porta serial  
  
1. Inicialize a cadeia de caracteres de retorno.  
  
     [!code-vb[VbVbalrMyComputer#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#38)]  
  
2. Determine qual porta serial deve fornecer as cadeias de caracteres. Este exemplo assume que é `COM1`.  
  
3. Use o método `My.Computer.Ports.OpenSerialPort` para obter uma referência para a porta. Para obter mais informações, consulte <xref:Microsoft.VisualBasic.Devices.Ports.OpenSerialPort%2A>.  
  
     O bloco `Try...Catch...Finally` permite que o aplicativo feche a porta serial mesmo que ele gere uma exceção. Todo o código que manipula a porta serial deve aparecer dentro deste bloco.  
  
     [!code-vb[VbVbalrMyComputer#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#39)]  
  
4. Crie um loop de `Do` para ler linhas do texto até que não haja mais linhas disponíveis.  
  
     [!code-vb[VbVbalrMyComputer#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#40)]  
  
5. Use o método <xref:System.IO.Ports.SerialPort.ReadLine> para ler a próxima linha de texto disponível da porta serial.  
  
     [!code-vb[VbVbalrMyComputer#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#41)]  
  
6. Use uma instrução `If` para determinar se o método <xref:System.IO.Ports.SerialPort.ReadLine> retorna `Nothing` (o que significa que não há mais texto disponível). Se ele retornar `Nothing`, saia do loop de `Do`.  
  
     [!code-vb[VbVbalrMyComputer#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#42)]  
  
7. Adicione um bloco `Else` à instrução `If` para lidar com o caso se a cadeia de caracteres for realmente lida. O bloco acrescenta a cadeia de caracteres da porta serial à cadeia de caracteres de retorno.  
  
     [!code-vb[VbVbalrMyComputer#43](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#43)]  
  
8. Retorne a cadeia de caracteres.  
  
     [!code-vb[VbVbalrMyComputer#44](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#44)]  
  
## <a name="example"></a>Exemplo  

 [!code-vb[VbVbalrMyComputer#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#37)]  
  
 Este exemplo de código também está disponível como um snippet de código do IntelliSense. No selecionador de snippet de código, ele está localizado em **Conectividade e Redes**. Para obter mais informações, consulte [Snippets de Código](/visualstudio/ide/code-snippets).  
  
## <a name="compiling-the-code"></a>Compilando o código  

 Este exemplo pressupõe que o computador esteja usando a `COM1`.  
  
## <a name="robust-programming"></a>Programação robusta  

 Este exemplo pressupõe que o computador esteja usando a `COM1`. Para obter mais flexibilidade, o código deve permitir que o usuário selecione a porta serial desejada na lista de portas disponíveis. Para obter mais informações, consulte [Como mostrar portas seriais disponíveis](how-to-show-available-serial-ports.md).  
  
 Este exemplo usa um bloco `Try...Catch...Finally` para garantir que o aplicativo feche a porta e capture quaisquer exceções de tempo limite. Para obter mais informações, consulte [Instrução Try...Catch...Finally](../../../language-reference/statements/try-catch-finally-statement.md).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Devices.Ports>
- <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType>
- [Como: discar modems anexados a portas seriais](how-to-dial-modems-attached-to-serial-ports.md)
- [Como: enviar cadeias de caracteres para portas seriais](how-to-send-strings-to-serial-ports.md)
- [Como: mostrar portas seriais disponíveis](how-to-show-available-serial-ports.md)
