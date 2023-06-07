---
title: Opções de rastreamento
description: Explore as opções de rastreamento, que permitem habilitar, desabilitar e filtrar a saída de rastreamento. O .NET fornece as classes BooleanSwitch, TraceSwitch e SourceSwitch.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tracing [.NET Framework], trace switches
- trace switches, about trace switches
- tracing [.NET Framework], level of detail
- switches, trace
- trace switches
- trace switches, creating custom
ms.assetid: 8ab913aa-f400-4406-9436-f45bc6e54fbe
ms.openlocfilehash: 29de46afa2a96dd7011cec40f4f76e7bfb8ee454
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803530"
---
# <a name="trace-switches"></a>Opções de rastreamento
As opções de rastreamento permitem habilitar, desabilitar e filtrar a saída de rastreamento. Elas são objetos que existem no código e podem ser configuradas externamente por meio do arquivo .config. Há três tipos de opções de rastreamento fornecidas no .NET Framework: a classe <xref:System.Diagnostics.BooleanSwitch>, a classe <xref:System.Diagnostics.TraceSwitch> e a classe <xref:System.Diagnostics.SourceSwitch>. A classe <xref:System.Diagnostics.BooleanSwitch> atua como uma opção de alternância, habilitando ou desabilitando uma variedade de instruções de rastreamento. As classes <xref:System.Diagnostics.TraceSwitch> e <xref:System.Diagnostics.SourceSwitch> permitem habilitar uma opção de rastreamento para um nível de rastreamento específico, de modo que as mensagens <xref:System.Diagnostics.Trace> ou <xref:System.Diagnostics.TraceSource> especificadas para o nível e todos os níveis inferiores a ele sejam exibidas. Se você desabilitar a opção, as mensagens de rastreamento não serão exibidas. Todas essas classes são derivadas da classe **Switch** abstrata (**MustInherit**), assim como todas as opções desenvolvidas pelo usuário.  
  
 As opções de rastreamento podem ser úteis para a filtragem de informações. Por exemplo, talvez você deseje ver todas as mensagens de rastreamento em um módulo de acesso a dados, mas somente as mensagens de erro no restante do aplicativo. Nesse caso, você usará uma opção de rastreamento para o módulo de acesso a dados e uma opção para o restante do aplicativo. Usando o arquivo .config para definir as opções com as configurações apropriadas, você pode controlar quais tipos de mensagem de rastreamento foram recebidas. Para obter mais informações, consulte [Como criar, inicializar e configurar opções de rastreamento](how-to-create-initialize-and-configure-trace-switches.md).  
  
 Normalmente, um aplicativo implantado é executado com suas opções desabilitadas, de modo que os usuários não precisem observar muitas mensagens de rastreamento irrelevantes exibidas em uma tela ou enchendo um arquivo de log conforme o aplicativo é executado. Se surgir um problema durante a execução do aplicativo, pare o aplicativo, habilite as opções e reinicie o aplicativo. Em seguida, as mensagens de rastreamento serão exibidas.  
  
 Para usar uma opção, primeiro você deve criar um objeto de opção com base em uma classe **BooleanSwitch**, uma classe **TraceSwitch** ou uma classe de opção definida pelo desenvolvedor. Para obter mais informações sobre como criar opções definidas pelo desenvolvedor, consulte a classe <xref:System.Diagnostics.Switch> na referência do .NET Framework. Em seguida, defina um valor de configuração que especifica quando o objeto de opção deve ser usado. Depois, teste a configuração do objeto de opção em vários métodos de rastreamento **Trace** (ou **Debug**).  
  
## <a name="trace-levels"></a>Níveis de rastreamento  
 Ao usar **TraceSwitch**, há considerações adicionais. Um objeto **TraceSwitch** tem quatro propriedades que retornam valores **boolianos**, indicando se a opção é definida com, pelo menos, um nível específico:  
  
- <xref:System.Diagnostics.TraceSwitch.TraceError%2A?displayProperty=nameWithType>  
  
- <xref:System.Diagnostics.TraceSwitch.TraceWarning%2A?displayProperty=nameWithType>  
  
- <xref:System.Diagnostics.TraceSwitch.TraceInfo%2A?displayProperty=nameWithType>  
  
- <xref:System.Diagnostics.TraceSwitch.TraceVerbose%2A?displayProperty=nameWithType>  
  
 Os níveis permitem limitar a quantidade de informações de rastreamento recebidas apenas às informações necessárias para solucionar um problema. Especifique o nível de detalhe desejado na saída de rastreamento, definindo e configurando opções de rastreamento para o nível de rastreamento apropriado. Você pode receber mensagens de erro, mensagens de aviso, mensagens informativas, mensagens de rastreamento detalhado ou nenhuma mensagem.  
  
 Cabe inteiramente a você decidir que tipo de mensagem deve ser associada a cada nível. Normalmente, o conteúdo das mensagens de rastreamento depende do que você associa a cada nível, mas você determina as diferenças entre os níveis. Talvez você deseje fornecer descrições detalhadas de um problema no nível 3 (**Informativo**), por exemplo, mas fornecer somente um número de referência de erro no nível 1 (**Erro**). Cabe inteiramente a você decidir qual esquema funciona melhor em seu aplicativo.  
  
 Essas propriedades correspondem aos valores 1 a 4 da enumeração **TraceLevel**. A tabela a seguir lista os níveis da enumeração **TraceLevel** e seus valores.  
  
|Valor enumerado|Valor inteiro|Tipo de mensagem exibido (ou gravado em um destino de saída especificado)|  
|----------------------|-------------------|---------------------------------------------------------------------------|  
|Desativado|0|Nenhum|  
|Erro|1|Somente mensagens de erro|  
|Aviso|2|Mensagens de aviso e mensagens de erro|  
|Informações|3|Mensagens informativas, mensagens de aviso e mensagens de erro|  
|Detalhado|4|Mensagens detalhadas, mensagens informativas, mensagens de aviso e mensagens de erro|  
  
 As propriedades **TraceSwitch** indicam o nível máximo de rastreamento da opção. Ou seja, as informações de rastreamento são gravadas para o nível especificado, bem como para todos os níveis inferiores. Por exemplo, se **TraceInfo** for **true**, **TraceError** e **TraceWarning** também serão **true**, mas **TraceVerbose** poderá ser **false**.  
  
 Essas propriedades são somente leitura. O objeto **TraceSwitch** define-as automaticamente quando a propriedade **TraceLevel** é definida. Por exemplo:  
  
```vb  
Dim myTraceSwitch As New TraceSwitch("SwitchOne", "The first switch")  
myTraceSwitch.Level = TraceLevel.Info  
' This message box displays true, because setting the level to  
' TraceLevel.Info sets all lower levels to true as well.  
MessageBox.Show(myTraceSwitch.TraceWarning.ToString())  
' This messagebox displays false.  
MessageBox.Show(myTraceSwitch.TraceVerbose.ToString())  
```  
  
```csharp  
System.Diagnostics.TraceSwitch myTraceSwitch =
   new System.Diagnostics.TraceSwitch("SwitchOne", "The first switch");  
myTraceSwitch.Level = System.Diagnostics.TraceLevel.Info;  
// This message box displays true, because setting the level to
// TraceLevel.Info sets all lower levels to true as well.  
MessageBox.Show(myTraceSwitch.TraceWarning.ToString());  
// This message box displays false.  
MessageBox.Show(myTraceSwitch.TraceVerbose.ToString());  
```  
  
## <a name="developer-defined-switches"></a>Opções definidas pelo desenvolvedor  
 Além de fornecer **BooleanSwitch** e **TraceSwitch**, você pode definir suas próprias opções herdando da classe **Switch** e substituindo os métodos da classe base por métodos personalizados. Para obter mais informações sobre como criar opções definidas pelo desenvolvedor, consulte a classe <xref:System.Diagnostics.Switch> na referência do .NET Framework.  
  
## <a name="see-also"></a>Consulte também

- [Ouvintes de rastreamento](trace-listeners.md)
- [Como adicionar instruções de rastreamento ao código de um aplicativo](how-to-add-trace-statements-to-application-code.md)
- [Rastreamento e instrumentação de aplicativos](tracing-and-instrumenting-applications.md)
