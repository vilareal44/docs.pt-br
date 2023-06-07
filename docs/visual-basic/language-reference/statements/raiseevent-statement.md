---
title: Instrução RaiseEvent
ms.date: 07/20/2015
f1_keywords:
- vb.RaiseEventMethod
- vb.RaiseEvent
- RaiseEvent
helpviewer_keywords:
- raising events [Visual Basic], RaiseEvent statement
- RaiseEvent statement [Visual Basic]
- event handlers, connecting events to
ms.assetid: f82e380a-1e6b-4047-bea8-c853f4d2c742
ms.openlocfilehash: 46b93c060a12d82b34dafdf3aa4ea677df6f54cd
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404285"
---
# <a name="raiseevent-statement"></a>Instrução RaiseEvent
Dispara um evento declarado no nível de módulo dentro de uma classe, formulário ou documento.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
RaiseEvent eventname[( argumentlist )]  
```  
  
## <a name="parts"></a>Partes  
 `eventname`  
 Obrigatórios. Nome do evento a ser disparado.  
  
 `argumentlist`  
 Opcional. Lista delimitada por vírgula de variáveis, matrizes ou expressões. O `argumentlist` argumento deve ser colocado entre parênteses. Se não houver argumentos, os parênteses deverão ser omitidos.  
  
## <a name="remarks"></a>Comentários  
 O necessário `eventname` é o nome de um evento declarado dentro do módulo. Ele segue Visual Basic convenções de nomenclatura de variáveis.  
  
 Se o evento não tiver sido declarado dentro do módulo no qual ele é gerado, ocorrerá um erro. O fragmento de código a seguir ilustra uma declaração de evento e um procedimento no qual o evento é gerado.  
  
 [!code-vb[VbVbalrEvents#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#37)]  
  
 Você não pode usar o `RaiseEvent` para gerar eventos que não são declarados explicitamente no módulo. Por exemplo, todos os formulários herdam um <xref:System.Windows.Forms.Control.Click> evento de <xref:System.Windows.Forms.Form?displayProperty=nameWithType> , não podem ser gerados usando `RaiseEvent` em um formulário derivado. Se você declarar um `Click` evento no módulo de formulário, ele sombreia o próprio evento do formulário <xref:System.Windows.Forms.Control.Click> . Você ainda pode invocar o evento do formulário <xref:System.Windows.Forms.Control.Click> chamando o <xref:System.Windows.Forms.Control.OnClick%2A> método.  
  
 Por padrão, um evento definido em Visual Basic gera seus manipuladores de eventos na ordem em que as conexões são estabelecidas. Como os eventos podem ter `ByRef` parâmetros, um processo que se conecta tardiamente pode receber parâmetros que foram alterados por um manipulador de eventos anterior. Depois que os manipuladores de eventos são executados, o controle é retornado para a sub-rotina que gerou o evento.  
  
> [!NOTE]
> Eventos não compartilhados não devem ser gerados dentro do construtor da classe na qual eles são declarados. Embora tais eventos não causem erros em tempo de execução, eles podem não ser detectados por manipuladores de eventos associados. Use o `Shared` modificador para criar um evento compartilhado se você precisar gerar um evento a partir de um construtor.  
  
> [!NOTE]
> Você pode alterar o comportamento padrão de eventos definindo um evento personalizado. Para eventos personalizados, a `RaiseEvent` instrução invoca o `RaiseEvent` acessador do evento. Para obter mais informações sobre eventos personalizados, consulte [Event Statement](event-statement.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa eventos para contar os segundos de 10 a 0. O código ilustra vários dos métodos, propriedades e instruções relacionados ao evento, incluindo a `RaiseEvent` instrução.  
  
 A classe que gera um evento é a origem do evento, e os métodos que processam o evento são os manipuladores de eventos. Uma origem de evento pode ter vários manipuladores para os eventos que ele gera. Quando a classe gera o evento, esse evento é gerado em todas as classes que escolheram manipular eventos para essa instância do objeto.  
  
 O exemplo também usa um formulário ( `Form1` ) com um botão () `Button1` e uma caixa de texto ( `TextBox1` ). Quando você clica no botão, a primeira caixa de texto exibe uma contagem regressiva de 10 a 0 segundos. Quando o tempo total (10 segundos) tiver decorrido, a primeira caixa de texto exibirá "concluído".  
  
 O código para `Form1` especifica os Estados inicial e de terminal do formulário. Ele também contém o código executado quando eventos são gerados.  
  
 Para usar este exemplo, abra um novo projeto de aplicativo do Windows, adicione um botão chamado `Button1` e uma caixa de texto nomeada `TextBox1` ao formulário principal, denominada `Form1` . Em seguida, clique com o botão direito do mouse no formulário e clique em **Exibir código** para abrir o editor de código.  
  
 Adicione uma `WithEvents` variável à seção de declarações da `Form1` classe.  
  
 [!code-vb[VbVbalrEvents#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#14)]  
  
## <a name="example"></a>Exemplo  
 Adicione o código a seguir ao código para `Form1` . Substitua quaisquer procedimentos duplicados que possam existir, como `Form_Load` ou `Button_Click` .  
  
 [!code-vb[VbVbalrEvents#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#15)]  
  
 Pressione F5 para executar o exemplo anterior e clique no botão chamado **Iniciar**. A primeira caixa de texto começa a contar os segundos. Quando o tempo total (10 segundos) tiver decorrido, a primeira caixa de texto exibirá "concluído".  
  
> [!NOTE]
> O `My.Application.DoEvents` método não processa eventos exatamente da mesma forma que o formulário. Para permitir que o formulário manipule os eventos diretamente, você pode usar multithreading. Para obter mais informações, consulte [Threading gerenciado](../../../standard/threading/index.md).  
  
## <a name="see-also"></a>Confira também

- [Eventos](../../programming-guide/language-features/events/index.md)
- [Instrução Event](event-statement.md)
- [Instrução AddHandler](addhandler-statement.md)
- [Instrução RemoveHandler](removehandler-statement.md)
- [Alças](handles-clause.md)
