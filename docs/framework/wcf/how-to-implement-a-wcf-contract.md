---
title: 'Tutorial: implementar um contrato de serviço de Windows Communication Foundation'
description: Saiba como adicionar código para implementar uma interface de serviço WCF como parte de uma série de artigos que ajudam você a começar a criar um aplicativo WCF.
ms.date: 03/19/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service contracts [WCF], implementing
ms.assetid: d5ab51ba-61ae-403e-b3c8-e2669e326806
ms.openlocfilehash: 89f97610cccd42c2a5d298baa667327d077fd472
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244641"
---
# <a name="tutorial-implement-a-windows-communication-foundation-service-contract"></a>Tutorial: implementar um contrato de serviço de Windows Communication Foundation

Este tutorial descreve a segunda das cinco tarefas necessárias para criar um aplicativo de Windows Communication Foundation básico (WCF). Para obter uma visão geral dos tutoriais, consulte [tutorial: introdução aos aplicativos Windows Communication Foundation](getting-started-tutorial.md).

A próxima etapa para criar um aplicativo WCF é adicionar código para implementar a interface de serviço do WCF que você criou na etapa anterior. Nesta etapa, você cria uma classe chamada `CalculatorService` que implementa a interface definida pelo usuário `ICalculator` . Cada método no código a seguir chama uma operação de calculadora e grava o texto no console do para testá-lo.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> - Adicione o código para implementar o contrato de serviço do WCF.
> - Compile a solução.

## <a name="add-code-to-implement-the-wcf-service-contract"></a>Adicionar código para implementar o contrato de serviço do WCF

No **GettingStartedLib**, abra o arquivo **Service1.cs** ou **Service1. vb** e substitua seu código pelo código a seguir:

```csharp
using System;
using System.ServiceModel;

namespace GettingStartedLib
{
    public class CalculatorService : ICalculator
    {
        public double Add(double n1, double n2)
        {
            double result = n1 + n2;
            Console.WriteLine("Received Add({0},{1})", n1, n2);
            // Code added to write output to the console window.
            Console.WriteLine("Return: {0}", result);
            return result;
        }

        public double Subtract(double n1, double n2)
        {
            double result = n1 - n2;
            Console.WriteLine("Received Subtract({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }

        public double Multiply(double n1, double n2)
        {
            double result = n1 * n2;
            Console.WriteLine("Received Multiply({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }

        public double Divide(double n1, double n2)
        {
            double result = n1 / n2;
            Console.WriteLine("Received Divide({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }
    }
}
```

```vb
Imports System.ServiceModel

Namespace GettingStartedLib

    Public Class CalculatorService
        Implements ICalculator

        Public Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Add
            Dim result As Double = n1 + n2
            ' Code added to write output to the console window.
            Console.WriteLine("Received Add({0},{1})", n1, n2)
            Console.WriteLine("Return: {0}", result)
            Return result
        End Function

        Public Function Subtract(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Subtract
            Dim result As Double = n1 - n2
            Console.WriteLine("Received Subtract({0},{1})", n1, n2)
            Console.WriteLine("Return: {0}", result)
            Return result

        End Function

        Public Function Multiply(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Multiply
            Dim result As Double = n1 * n2
            Console.WriteLine("Received Multiply({0},{1})", n1, n2)
            Console.WriteLine("Return: {0}", result)
            Return result

        End Function

        Public Function Divide(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Divide
            Dim result As Double = n1 / n2
            Console.WriteLine("Received Divide({0},{1})", n1, n2)
            Console.WriteLine("Return: {0}", result)
            Return result

        End Function
    End Class
End Namespace
```

## <a name="edit-appconfig"></a>Editar App.config

Edite **App.config** no **GettingStartedLib** para refletir as alterações feitas no código.

- Para projetos do Visual C#:
  - Altere a linha 14 para`<service name="GettingStartedLib.CalculatorService">`
  - Altere a linha 17 para`<add baseAddress = "http://localhost:8000/GettingStarted/CalculatorService" />`
  - Altere a linha 22 para`<endpoint address="" binding="wsHttpBinding" contract="GettingStartedLib.ICalculator">`

- Para projetos do Visual Basic:
  - Altere a linha 14 para`<service name="GettingStartedLib.GettingStartedLib.CalculatorService">`
  - Altere a linha 17 para`<add baseAddress = "http://localhost:8000/GettingStarted/CalculatorService" />`
  - Altere a linha 22 para`<endpoint address="" binding="wsHttpBinding" contract="GettingStartedLib.GettingStartedLib.ICalculator">`

## <a name="compile-the-code"></a>Compilar o código

Compile a solução para verificar se não há erros de compilação. Se você estiver usando o Visual Studio, no menu **Compilar** , selecione **Compilar solução** (ou pressione **Ctrl** + **Shift** + **B**).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:
> [!div class="checklist"]
>
> - Adicione o código para implementar o contrato de serviço do WCF.
> - Compile a solução.

Avance para o próximo tutorial para aprender a executar o serviço WCF.

> [!div class="nextstepaction"]
> [Tutorial: hospedar e executar um serviço WCF básico](how-to-host-and-run-a-basic-wcf-service.md)
