---
title: declarar e gerar eventos
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], events
- events [Visual Basic], walkthroughs
- declaring events [Visual Basic], walkthroughs
- events [Visual Basic], declaring
- events [Visual Basic], raising
- raising events [Visual Basic], walkthroughs
ms.assetid: 8ffb3be8-097d-4d3c-b71e-04555ebda2a2
ms.openlocfilehash: 3da60014d7ac95189c5d56c3e339ff1b054a40dc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84405087"
---
# <a name="walkthrough-declaring-and-raising-events-visual-basic"></a>Instruções passo a passo: declarando e acionando eventos (Visual Basic)
Este tutorial demonstra como declarar e gerar eventos para uma classe chamada `Widget` . Depois de concluir as etapas, talvez você queira ler o tópico complementar, [Walkthrough: Manipulando eventos](walkthrough-handling-events.md), que mostra como usar eventos de `Widget` objetos para fornecer informações de status em um aplicativo.  
  
## <a name="the-widget-class"></a>A classe Widget  
 Suponha que, por enquanto, você tenha uma `Widget` classe. Sua `Widget` classe tem um método que pode levar muito tempo para ser executada e você deseja que seu aplicativo seja capaz de colocar algum tipo de indicador de conclusão.  
  
 É claro que você pode fazer `Widget` com que o objeto mostre uma caixa de diálogo de porcentagem concluída, mas, em seguida, você estaria preso a essa caixa de diálogo em todos os projetos nos quais usou a `Widget` classe. Um bom princípio do design de objeto é permitir que o aplicativo que usa um objeto manipule a interface do usuário — a menos que a finalidade do objeto seja gerenciar um formulário ou caixa de diálogo.  
  
 A finalidade do `Widget` é executar outras tarefas, portanto, é melhor adicionar um `PercentDone` evento e permitir que o procedimento que chama os `Widget` métodos manipule esse evento e exiba as atualizações de status. O `PercentDone` evento também pode fornecer um mecanismo para cancelar a tarefa.  
  
#### <a name="to-build-the-code-example-for-this-topic"></a>Para criar o exemplo de código para este tópico  
  
1. Abra um novo projeto de aplicativo do Windows Visual Basic e crie um formulário chamado `Form1` .  
  
2. Adicione dois botões e um rótulo a `Form1` .  
  
3. Nomeie os objetos como mostrado na tabela a seguir.  
  
    |Objeto|Propriedade|Configuração|  
    |------------|--------------|-------------|  
    |`Button1`|`Text`|Tarefa de início|  
    |`Button2`|`Text`|Cancelar|  
    |`Label`|`(Name)`, `Text`|lblPercentDone, 0|  
  
4. No menu **projeto** , escolha **Adicionar classe** para adicionar uma classe chamada `Widget.vb` ao projeto.  
  
#### <a name="to-declare-an-event-for-the-widget-class"></a>Para declarar um evento para a classe Widget  
  
- Use a `Event` palavra-chave para declarar um evento na `Widget` classe. Observe que um evento pode ter `ByVal` `ByRef` argumentos e, como `Widget` é `PercentDone` demonstrado pelo evento:  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Widget.vb#1)]  
  
 Quando o objeto de chamada recebe um `PercentDone` evento, o `Percent` argumento contém a porcentagem da tarefa concluída. O `Cancel` argumento pode ser definido como `True` para cancelar o método que gerou o evento.  
  
> [!NOTE]
> Você pode declarar argumentos de evento da mesma forma como faz argumentos de procedimentos, com as seguintes exceções: os eventos não podem ter `Optional` `ParamArray` argumentos ou, e os eventos não têm valores de retorno.  
  
 O `PercentDone` evento é gerado pelo `LongTask` método da `Widget` classe. `LongTask`usa dois argumentos: o período de tempo que o método pretende a fazer funcionar e o intervalo de tempo mínimo antes de `LongTask` pausar para gerar o `PercentDone` evento.  
  
#### <a name="to-raise-the-percentdone-event"></a>Para gerar o evento PercentDone  
  
1. Para simplificar o acesso à `Timer` Propriedade usada por essa classe, adicione uma `Imports` instrução à parte superior da seção de declarações do seu módulo de classe, acima da `Class Widget` instrução.  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Widget.vb#2)]  
  
2. Adicione o código a seguir à classe `Widget`:  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Widget.vb#3)]  
  
 Quando o aplicativo chama o `LongTask` método, a `Widget` classe gera o `PercentDone` evento a cada `MinimumInterval` segundos. Quando o evento retorna, o `LongTask` verifica se o `Cancel` argumento foi definido como `True` .  
  
 Alguns isenções de responsabilidade são necessários aqui. Para simplificar, o `LongTask` procedimento pressupõe que você saiba com antecedência quanto tempo a tarefa levará. Esse é quase nunca o caso. Dividir tarefas em partes de tamanho par pode ser difícil e, muitas vezes, o que mais importa para os usuários é simplesmente a quantidade de tempo que o passa antes de receber uma indicação de que algo está acontecendo.  
  
 Você pode ter extratado outra falha neste exemplo. A `Timer` propriedade retorna o número de segundos que passaram desde a meia-noite; portanto, o aplicativo ficará preso se for iniciado logo antes da meia-noite. Uma abordagem mais cuidadosa para medir o tempo teria condições de limite como essa em consideração, ou evitá-las completamente, usando propriedades como `Now` .  
  
 Agora que a `Widget` classe pode gerar eventos, você pode ir para o próximo passo a passos. [Walkthrough: manipular eventos](walkthrough-handling-events.md) demonstra como usar `WithEvents` o para associar um manipulador de eventos ao `PercentDone` evento.  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.DateAndTime.Timer%2A>
- <xref:Microsoft.VisualBasic.DateAndTime.Now%2A>
- [Instruções passo a passo: tratando eventos](walkthrough-handling-events.md)
- [Eventos](index.md)
