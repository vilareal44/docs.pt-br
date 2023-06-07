---
title: Ouvintes de rastreamento
description: Explore os ouvintes de rastreamento, que são um mecanismo para coletar e registrar mensagens de rastreamento enviadas no .NET. Um ouvinte coleta, armazena e roteia mensagens.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Listener object types
- listeners
- Trace class, listeners
- trace listeners, about trace listeners
- Listeners collection
- trace listeners
- tracing [.NET Framework], trace listeners
- logs, trace listeners
ms.assetid: 444b0d33-67ea-4c36-9e94-79c50f839025
ms.openlocfilehash: d08f86c782284a296090cf63e4b03c8d446a95fc
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803517"
---
# <a name="trace-listeners"></a>Ouvintes de rastreamento
Ao usar **Trace**, **Debug** e <xref:System.Diagnostics.TraceSource>, você deve ter um mecanismo para coletar e registrar as mensagens que são enviadas. As mensagens de rastreamento são recebidas por *ouvintes*. A finalidade de um ouvinte é coletar, armazenar e rotear mensagens de rastreamento. Os ouvintes direcionam a saída de rastreamento para um destino apropriado, como um log, uma janela ou um arquivo de texto.  
  
 Os ouvintes estão disponíveis para as classes **Debug**, **Trace** e <xref:System.Diagnostics.TraceSource>, cada uma das quais pode enviar sua saída para a uma variedade de objetos de ouvinte. Veja a seguir os ouvintes predefinidos mais usados:  
  
- Um <xref:System.Diagnostics.TextWriterTraceListener> redireciona a saída para uma instância da classe <xref:System.IO.TextWriter> ou para qualquer coisa que seja uma classe <xref:System.IO.Stream>. Ele também pode gravar no console ou em um arquivo, pois eles são classes <xref:System.IO.Stream>.  
  
- Um <xref:System.Diagnostics.EventLogTraceListener> redireciona a saída para um log de eventos.  
  
- Um <xref:System.Diagnostics.DefaultTraceListener> emite mensagens **Write** e **WriteLine** para os métodos **OutputDebugString** e **Debugger.Log**. No Visual Studio, isso faz com que as mensagens de depuração sejam exibidas na janela de Saída. Mensagens **Fail** e **Assert** com falha também são emitidas para a API do Windows **OutputDebugString** e o método **Debugger.Log**, e também fazem com que uma caixa de mensagem seja exibida. Esse comportamento é o comportamento padrão de mensagens **Debug** e **Trace**, pois **DefaultTraceListener** é incluído automaticamente em cada coleção `Listeners` e é o único ouvinte incluído automaticamente.  
  
- Um <xref:System.Diagnostics.ConsoleTraceListener> direciona a saída do rastreamento ou da depuração para a saída padrão ou para o fluxo de erro padrão.  
  
- Um <xref:System.Diagnostics.DelimitedListTraceListener> direciona a saída de rastreamento ou de depuração para um text writer, como um gravador de fluxo ou para um fluxo, como um fluxo de arquivos. A saída de rastreamento está em um formato de texto delimitado que usa o delimitador especificado pela propriedade <xref:System.Diagnostics.DelimitedListTraceListener.Delimiter%2A>.  
  
- Um <xref:System.Diagnostics.XmlWriterTraceListener> direciona a saída de rastreamento ou de depuração como dados codificados em XML para um <xref:System.IO.TextWriter> ou <xref:System.IO.Stream>, como um <xref:System.IO.FileStream>.  
  
 Se você deseja que qualquer ouvinte além do <xref:System.Diagnostics.DefaultTraceListener> receba a saída **Debug**, **Trace** e <xref:System.Diagnostics.TraceSource>, adicione-o à coleção `Listeners`. Para obter mais informações, consulte [Como criar e inicializar ouvintes de rastreamento](how-to-create-and-initialize-trace-listeners.md) e [Como usar TraceSource e filtros com ouvintes de rastreamento](how-to-use-tracesource-and-filters-with-trace-listeners.md). Qualquer ouvinte da coleção **Listeners** obtém as mesmas mensagens dos métodos de saída de rastreamento. Por exemplo, suponha que você configure dois ouvintes: um **TextWriterTraceListener** e um **EventLogTraceListener**. Cada ouvinte recebe a mesma mensagem. O **TextWriterTraceListener** direcionará sua saída para um fluxo e o **EventLogTraceListener** direcionará sua saída para um log de eventos.  
  
 O exemplo a seguir mostra como enviar a saída para a coleção **Listeners**.  
  
```vb  
' Use this example when debugging.  
Debug.WriteLine("Error in Widget 42")  
' Use this example when tracing.  
Trace.WriteLine("Error in Widget 42")  
```  
  
```csharp  
// Use this example when debugging.  
System.Diagnostics.Debug.WriteLine("Error in Widget 42");  
// Use this example when tracing.  
System.Diagnostics.Trace.WriteLine("Error in Widget 42");  
```  
  
 A depuração e o rastreamento compartilham a mesma coleção **Listeners**. Portanto, se você adicionar um objeto de ouvinte a uma coleção **Debug.Listeners** no aplicativo, ele será adicionado à coleção **Trace.Listeners** também.  
  
 O exemplo a seguir mostra como usar um ouvinte para enviar informações de rastreamento para um console:  
  
```vb  
Trace.Listeners.Clear()  
Trace.Listeners.Add(New TextWriterTraceListener(Console.Out))  
```  
  
```csharp  
System.Diagnostics.Trace.Listeners.Clear();  
System.Diagnostics.Trace.Listeners.Add(  
   new System.Diagnostics.TextWriterTraceListener(Console.Out));  
```  
  
## <a name="developer-defined-listeners"></a>Ouvintes definidos pelo desenvolvedor  
 Você pode definir seus próprios ouvintes herdando a classe base **TraceListener** e substituindo seus métodos por métodos personalizados. Para obter mais informações sobre como criar ouvintes definidos pelo desenvolvedor, consulte <xref:System.Diagnostics.TraceListener> na referência do .NET Framework.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Diagnostics.TextWriterTraceListener>
- <xref:System.Diagnostics.EventLogTraceListener>
- <xref:System.Diagnostics.DefaultTraceListener>
- <xref:System.Diagnostics.TraceListener>
- [Rastreamento e instrumentação de aplicativos](tracing-and-instrumenting-applications.md)
- [Opções de rastreamento](trace-switches.md)
