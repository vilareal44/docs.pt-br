---
title: Usando os serviços UDP
description: A classe UdpClient abstrai os detalhes para criar um soquete para solicitar e receber dados usando UDP no .NET Framework.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- protocols, UDP
- network resources, UDP
- requesting data from Internet, UDP
- UDP
- receiving data, UDP
- Internet, UDP
- broadcasting messages to multiple addresses
- data requests, UDP
- UdpClient class, about UdpClient class
- sending data, UDP
- application protocols, UDP
ms.assetid: d5c3477a-e798-454c-a890-738ba14c5707
ms.openlocfilehash: 4c2e88492f737800151d30097719d7d011054a5e
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501944"
---
# <a name="use-udp-services"></a>Usar serviços UDP

A classe <xref:System.Net.Sockets.UdpClient> se comunica com os serviços de rede usando o UDP. As propriedades e os métodos da classe <xref:System.Net.Sockets.UdpClient> abstraem os detalhes da criação de um <xref:System.Net.Sockets.Socket> para solicitar e receber dados usando o UDP.

O protocolo UDP é um protocolo simples que se esforça para fornecer dados para um host remoto. No entanto, como o protocolo UDP é um protocolo sem conexão, os datagramas UDP enviados para o ponto de extremidade remoto não têm a garantia de serem recebidos, nem a garantia de serem recebidos na mesma sequência em que são enviados. Os aplicativos que usam o UDP devem estar preparados para manipular datagramas ausentes, duplicados e fora de sequência.

Para enviar um datagrama usando o UDP, você deve saber o endereço de rede do dispositivo de rede que hospeda o serviço necessário e o número da porta UDP usada pelo serviço para se comunicar. A IANA (Internet Assigned Numbers Authority) define números de porta para serviços comuns (consulte o [nome do serviço e o registro do número da porta do protocolo de transporte](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)). Os serviços que não estão na lista IANA podem ter números de porta no intervalo de 1.024 a 65.535.

Endereços de rede especiais são usados para dar suporte a mensagens de difusão do UDP em redes baseadas em IP. A discussão a seguir usa a família de endereços IP versão 4 usada na Internet como exemplo.

Os endereços IP versão 4 usam 32 bits para especificar um endereço de rede. Para endereços de classe C que usam uma máscara de rede 255.255.255.0, esses bits estão separados em quatro octetos. Quando expressos em decimais, os quatro octetos formam a notação quádrupla pontilhada conhecida, como 192.168.100.2. Os dois primeiros octetos (192.168, neste exemplo) formam o número da rede, o terceiro octeto (100) define a sub-rede e o octeto final (2) é o identificador do host.

A configuração de todos os bits de um endereço IP como um ou 255.255.255.255, forma o endereço de difusão limitada. O envio de um datagrama UDP para esse endereço entrega a mensagem para qualquer host no segmento de rede local. Como os roteadores nunca encaminham mensagens enviadas para esse endereço, somente os hosts no segmento de rede recebem a mensagem de difusão.

As difusões podem ser direcionadas a partes específicas de uma rede, com a configuração de todos os bits do identificador do host. Por exemplo, para enviar uma difusão para todos os hosts na rede identificada por endereços IP que começam com 192.168.1, use o endereço 192.168.1.255.

O exemplo de código a seguir usa um <xref:System.Net.Sockets.UdpClient> para escutar datagramas UDP na porta 11.000. O cliente recebe uma cadeia de caracteres de mensagem e grava a mensagem no console.

```vb
Imports System.Net
Imports System.Net.Sockets
Imports System.Text

Public Class UDPListener
   Private Const listenPort As Integer = 11000

   Private Shared Sub StartListener()
      Dim listener As New UdpClient(listenPort)
      Dim groupEP As New IPEndPoint(IPAddress.Any, listenPort)
      Try
         While True
            Console.WriteLine("Waiting for broadcast")
            Dim bytes As Byte() = listener.Receive(groupEP)
            Console.WriteLine("Received broadcast from {0} :", groupEP)
            Console.WriteLine(Encoding.ASCII.GetString(bytes, 0, bytes.Length))
         End While
      Catch e As SocketException
         Console.WriteLine(e)
      Finally
         listener.Close()
      End Try
   End Sub 'StartListener

   Public Shared Sub Main()
      StartListener()
   End Sub 'Main
End Class 'UDPListener
```

```csharp
using System;
using System.Net;
using System.Net.Sockets;
using System.Text;

public class UDPListener
{
    private const int listenPort = 11000;

    private static void StartListener()
    {
        UdpClient listener = new UdpClient(listenPort);
        IPEndPoint groupEP = new IPEndPoint(IPAddress.Any, listenPort);

        try
        {
            while (true)
            {
                Console.WriteLine("Waiting for broadcast");
                byte[] bytes = listener.Receive(ref groupEP);

                Console.WriteLine($"Received broadcast from {groupEP} :");
                Console.WriteLine($" {Encoding.ASCII.GetString(bytes, 0, bytes.Length)}");
            }
        }
        catch (SocketException e)
        {
            Console.WriteLine(e);
        }
        finally
        {
            listener.Close();
        }
    }

    public static void Main()
    {
        StartListener();
    }
}
```

O exemplo de código a seguir usa um <xref:System.Net.Sockets.Socket> para enviar datagramas UDP para o endereço de difusão direcionado 192.168.1.255, usando a porta 11.000. O cliente envia a cadeia de caracteres de mensagem especificada na linha de comando.

```vb
Imports System.Net
Imports System.Net.Sockets
Imports System.Text

Public Class Program
    Public Shared Sub Main(args() As [String])
      Dim s As New Socket(AddressFamily.InterNetwork, SocketType.Dgram, ProtocolType.Udp)
      Dim broadcast As IPAddress = IPAddress.Parse("192.168.1.255")
      Dim sendbuf As Byte() = Encoding.ASCII.GetBytes(args(0))
      Dim ep As New IPEndPoint(broadcast, 11000)
      s.SendTo(sendbuf, ep)
      Console.WriteLine("Message sent to the broadcast address")
   End Sub 'Main
End Class
```

```csharp
using System;
using System.Net;
using System.Net.Sockets;
using System.Text;

class Program
{
    static void Main(string[] args)
    {
        Socket s = new Socket(AddressFamily.InterNetwork, SocketType.Dgram, ProtocolType.Udp);

        IPAddress broadcast = IPAddress.Parse("192.168.1.255");

        byte[] sendbuf = Encoding.ASCII.GetBytes(args[0]);
        IPEndPoint ep = new IPEndPoint(broadcast, 11000);

        s.SendTo(sendbuf, ep);

        Console.WriteLine("Message sent to the broadcast address");
    }
}
```

## <a name="see-also"></a>Confira também

- <xref:System.Net.Sockets.UdpClient>
- <xref:System.Net.IPAddress>
