---
title: Padrões de programação assíncrona
description: Saiba mais sobre o padrão assíncrono baseado em tarefa (toque), o padrão assíncrono baseado em evento (EAP), & modelo de programação assíncrona (APM) no .NET.
ms.date: 10/16/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- asynchronous design patterns, .NET
- .NET Framework, asynchronous design patterns
ms.assetid: 4ece5c0b-f8fe-4114-9862-ac02cfe5a5d7
ms.openlocfilehash: bd4d44d8de8a64be82e9ce6af593a86719b59fcf
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84583499"
---
# <a name="asynchronous-programming-patterns"></a>Padrões de programação assíncrona

O .NET fornece três padrões para a execução de operações assíncronas:  

- **Padrão assíncrono baseado em tarefa (TAP)**, que usa um único método para representar a inicialização e a conclusão de uma operação assíncrona. O TAP foi introduzido no .NET Framework 4. **É a abordagem recomendada para a programação assíncrona no .NET.** As palavras-chave [async](../../csharp/language-reference/keywords/async.md) e [await](../../csharp/language-reference/operators/await.md) no C# e os operadores [Async](../../visual-basic/language-reference/modifiers/async.md) e [Await](../../visual-basic/language-reference/operators/await-operator.md) no Visual Basic adicionam suporte à linguagem para TAP. Para saber mais, confira [Padrão assíncrono baseado em tarefa (TAP)](task-based-asynchronous-pattern-tap.md).  

- **Padrão assíncrono baseado em evento (EAP)**, que é o modelo herdado baseado em evento para fornecimento do comportamento assíncrono. Ele requer um método que tem o sufixo `Async` e um ou mais eventos, tipos delegados de manipulador de eventos e tipos derivados de `EventArg`. O EAP foi introduzido no .NET Framework 2.0. Não é mais recomendado para novo desenvolvimento. Para saber mais, confira [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md).  

- Padrão de **Modelo de programação assíncrona (APM)** (também chamado de padrão <xref:System.IAsyncResult>), que é o modelo herdado que usa a interface <xref:System.IAsyncResult> para fornecimento do comportamento assíncrono. Nesse padrão, as operações síncronas exigem os métodos `Begin` e `End` (por exemplo, `BeginWrite` e `EndWrite` para implementar uma operação de gravação assíncrona). Esse padrão não é mais recomendado para implantação nova. Para saber mais, veja [APM (Modelo Assíncrono de Programação)](asynchronous-programming-model-apm.md).  
  
## <a name="comparison-of-patterns"></a>Comparação de padrões

Para obter uma comparação rápida de como os três padrões modelam as operações assíncronas, considere um método `Read` que lê uma quantidade especificada de dados em um buffer fornecido começando em um deslocamento especificado:  
  
```csharp  
public class MyClass  
{  
    public int Read(byte [] buffer, int offset, int count);  
}  
```  

O equivalente do TAP para este método poderia expor o método único `ReadAsync` a seguir:  
  
```csharp
public class MyClass  
{  
    public Task<int> ReadAsync(byte [] buffer, int offset, int count);  
}  
```

O equivalente do EAP poderia expor o seguinte conjunto de tipos e membros:  
  
```csharp  
public class MyClass  
{  
    public void ReadAsync(byte [] buffer, int offset, int count);  
    public event ReadCompletedEventHandler ReadCompleted;  
}  
```  
  
O equivalente do APM poderia expor os métodos `BeginRead` e `EndRead`:  
  
```csharp  
public class MyClass  
{  
    public IAsyncResult BeginRead(  
        byte [] buffer, int offset, int count,
        AsyncCallback callback, object state);  
    public int EndRead(IAsyncResult asyncResult);  
}  
```  

## <a name="see-also"></a>Consulte também

- [Assincronia detalhada](../async-in-depth.md)
- [Programação assíncrona em C#](../../csharp/async.md)
- [Programação assíncrona em F#](../../fsharp/tutorials/asynchronous-and-concurrent-programming/async.md)
- [Programação assíncrona com Async e Await (Visual Basic)](../../visual-basic/programming-guide/concepts/async/index.md)
