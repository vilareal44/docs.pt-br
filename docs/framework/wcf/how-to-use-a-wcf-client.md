---
title: 'Tutorial: usar um cliente Windows Communication Foundation'
description: Saiba como criar uma instância de cliente, compilar o aplicativo e se comunicar com um serviço como parte de uma série de artigos sobre como criar um aplicativo WCF.
ms.date: 03/19/2019
helpviewer_keywords:
- WCF clients [WCF], using
dev_langs:
- CSharp
- VB
ms.assetid: 190349fc-0573-49c7-bb85-8e316df7f31f
ms.openlocfilehash: 5c94d5f8af679580c4194aaaadeda759098953d2
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244329"
---
# <a name="tutorial-use-a-windows-communication-foundation-client"></a>Tutorial: usar um cliente Windows Communication Foundation

Este tutorial descreve a última das cinco tarefas necessárias para criar um aplicativo de Windows Communication Foundation básico (WCF). Para obter uma visão geral dos tutoriais, consulte [tutorial: introdução aos aplicativos Windows Communication Foundation](getting-started-tutorial.md).

Depois de criar e configurar um proxy de Windows Communication Foundation (WCF), você cria uma instância de cliente e compila o aplicativo cliente. Em seguida, você o usa para se comunicar com o serviço WCF.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> - Adicione o código para usar o cliente WCF.
> - Teste o cliente WCF.

## <a name="add-code-to-use-the-wcf-client"></a>Adicionar código para usar o cliente WCF

O código do cliente executa as seguintes etapas:

- Cria uma instância do cliente WCF.
- Chama as operações de serviço do proxy gerado.
- Fecha o cliente após a conclusão da chamada de operação.

Abra o arquivo **Program.cs** ou **Module1. vb** do projeto **GettingStartedClient** e substitua seu código pelo código a seguir:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using GettingStartedClient.ServiceReference1;

namespace GettingStartedClient
{
    class Program
    {
        static void Main(string[] args)
        {
            //Step 1: Create an instance of the WCF proxy.
            CalculatorClient client = new CalculatorClient();

            // Step 2: Call the service operations.
            // Call the Add service operation.
            double value1 = 100.00D;
            double value2 = 15.99D;
            double result = client.Add(value1, value2);
            Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);

            // Call the Subtract service operation.
            value1 = 145.00D;
            value2 = 76.54D;
            result = client.Subtract(value1, value2);
            Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);

            // Call the Multiply service operation.
            value1 = 9.00D;
            value2 = 81.25D;
            result = client.Multiply(value1, value2);
            Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);

            // Call the Divide service operation.
            value1 = 22.00D;
            value2 = 7.00D;
            result = client.Divide(value1, value2);
            Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);

            // Step 3: Close the client to gracefully close the connection and clean up resources.
            Console.WriteLine("\nPress <Enter> to terminate the client.");
            Console.ReadLine();
            client.Close();
        }
    }
}
```

```vb
Imports System.Collections.Generic
Imports System.Text
Imports System.ServiceModel
Imports GettingStartedClient.ServiceReference1

Module Module1

    Sub Main()
        ' Step 1: Create an instance of the WCF proxy.
        Dim Client As New CalculatorClient()

        ' Step 2: Call the service operations.
        ' Call the Add service operation.
        Dim value1 As Double = 100D
        Dim value2 As Double = 15.99D
        Dim result As Double = Client.Add(value1, value2)
        Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result)

        ' Call the Subtract service operation.
        value1 = 145D
        value2 = 76.54D
        result = Client.Subtract(value1, value2)
        Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result)

        ' Call the Multiply service operation.
        value1 = 9D
        value2 = 81.25D
        result = Client.Multiply(value1, value2)
        Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result)

        ' Call the Divide service operation.
        value1 = 22D
        value2 = 7D
        result = Client.Divide(value1, value2)
        Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result)

        ' Step 3: Close the client to gracefully close the connection and clean up resources.
        Console.WriteLine()
        Console.WriteLine("Press <Enter> to terminate the client.")
        Console.ReadLine()
        Client.Close()

    End Sub

End Module
```

Observe a `using` instrução (para Visual C#) ou `Imports` (para Visual Basic) que importa `GettingStartedClient.ServiceReference1` . Essa instrução importa o código gerado pelo Visual Studio com a função **Adicionar referência de serviço** . O código instancia o proxy WCF e chama cada uma das operações de serviço que o serviço de calculadora expõe. Em seguida, ele fecha o proxy e encerra o programa.

## <a name="test-the-wcf-client"></a>Testar o cliente WCF

### <a name="test-the-application-from-visual-studio"></a>Testar o aplicativo do Visual Studio

1. Salve e crie a solução.

2. Selecione a pasta **GettingStartedLib** e, em seguida, selecione **definir como projeto de inicialização** no menu de atalho.

3. Em **projetos de inicialização**, selecione **GettingStartedLib** na lista suspensa e, em seguida, selecione **executar** ou pressione **F5**.

### <a name="test-the-application-from-a-command-prompt"></a>Testar o aplicativo a partir de um prompt de comando

1. Abra um prompt de comando como administrador e, em seguida, navegue até o diretório da solução do Visual Studio.

2. Para iniciar o serviço: insira *GettingStartedHost\bin\Debug\GettingStartedHost.exe*.

3. Para iniciar o cliente: abra outro prompt de comando, navegue até o diretório da sua solução do Visual Studio e, em seguida, insira *GettingStartedClient\bin\Debug\GettingStartedClient.exe*.

   *GettingStartedHost.exe* produz a seguinte saída:

   ```text
   The service is ready.
   Press <Enter> to terminate the service.

   Received Add(100,15.99)
   Return: 115.99
   Received Subtract(145,76.54)
   Return: 68.46
   Received Multiply(9,81.25)
   Return: 731.25
   Received Divide(22,7)
   Return: 3.14285714285714
   ```

   *GettingStartedClient.exe* produz a seguinte saída:

   ```text
   Add(100,15.99) = 115.99
   Subtract(145,76.54) = 68.46
   Multiply(9,81.25) = 731.25
   Divide(22,7) = 3.14285714285714

   Press <Enter> to terminate the client.
   ```

## <a name="next-steps"></a>Próximas etapas

Agora você concluiu todas as tarefas no tutorial introdução ao WCF. Neste tutorial, você aprendeu a:

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> - Adicione o código para usar o cliente WCF.
> - Teste o cliente WCF.

Se você tiver problemas ou erros em qualquer uma das etapas, siga as etapas no artigo de solução de problemas para corrigi-los.

> [!div class="nextstepaction"]
> [Solução de problemas de introdução aos tutoriais do WCF](troubleshooting-the-getting-started-tutorial.md)
