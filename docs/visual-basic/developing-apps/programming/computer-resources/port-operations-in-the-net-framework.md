---
title: Operações de porta no .NET Framework
ms.date: 07/20/2015
helpviewer_keywords:
- ports, Visual Basic
ms.assetid: 1eba223b-7bd3-401a-b097-982bce96df1b
ms.openlocfilehash: 0ef0b8a7aec40603185d227d972cea655fd238c1
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84360131"
---
# <a name="port-operations-in-the-net-framework-with-visual-basic"></a>Operações de porta no .NET Framework com Visual Basic

Você pode acessar as portas seriais do seu computador por meio de classes do .NET Framework no namespace <xref:System.IO.Ports?displayProperty=nameWithType>. A classe mais importante, <xref:System.IO.Ports.SerialPort>, fornece uma estrutura para a E/S síncrona e orientada a eventos, acesso aos estados de fixação e interrupção e acesso às propriedades do driver serial. Ela pode ser encapsulada em um objeto <xref:System.IO.Stream>, acessível por meio da propriedade <xref:System.IO.Ports.SerialPort.BaseStream>. O encapsulamento de <xref:System.IO.Ports.SerialPort> em um objeto <xref:System.IO.Stream> permite que a porta serial seja acessada por classes que usam fluxos. O namespace inclui enumerações que simplificam o controle de portas seriais.

A maneira mais simples para criar um objeto <xref:System.IO.Ports.SerialPort> é por meio do método <xref:Microsoft.VisualBasic.Devices.Ports.OpenSerialPort%2A>.

> [!NOTE]
> Não é possível usar classes do .NET Framework para acessar diretamente outros tipos de portas, como portas paralelas, portas USB e assim por diante.

## <a name="enumerations"></a>Enumerações

Esta tabela lista e descreve as principais enumerações usadas para acessar uma porta serial:

|Enumeração|Descrição|
|---|---|
|<xref:System.IO.Ports.Handshake>|Especifica o protocolo de controle utilizado para estabelecer uma comunicação de porta serial para um objeto <xref:System.IO.Ports.SerialPort>.|
|<xref:System.IO.Ports.Parity>|Especifica o bit de paridade para um objeto <xref:System.IO.Ports.SerialPort>.|
|<xref:System.IO.Ports.SerialData>|Especifica o tipo de caractere que foi recebido na porta serial do objeto <xref:System.IO.Ports.SerialPort>.|
|<xref:System.IO.Ports.SerialError>|Especifica os erros que ocorrem no objeto <xref:System.IO.Ports.SerialPort>|
|<xref:System.IO.Ports.SerialPinChange>|Especifica o tipo de alteração ocorrida no objeto <xref:System.IO.Ports.SerialPort>.|
|<xref:System.IO.Ports.StopBits>|Especifica o número de bits de parada usado no objeto <xref:System.IO.Ports.SerialPort>.|

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Devices.Ports>
- [Acessar as portas do computador](accessing-the-computer-s-ports.md)
