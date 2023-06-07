---
title: Implementando o padrão assíncrono baseado em evento
description: Saiba como implementar o padrão assíncrono baseado em evento (EAP) no .NET. O EAP é uma maneira padrão de empacotar uma classe que tem recursos assíncronos.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Event-based Asynchronous Pattern
- ProgressChangedEventArgs class
- BackgroundWorker component
- events [.NET Framework], asynchronous
- Asynchronous Pattern
- AsyncOperationManager class
- threading [.NET Framework], asynchronous features
- components [.NET Framework], asynchronous
- AsyncOperation class
- AsyncCompletedEventArgs class
ms.assetid: 43402d19-8d30-426d-8785-1a4478233bfa
ms.openlocfilehash: e36ae21e1e03c8c5c688b7446f660ab1bb666a94
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904371"
---
# <a name="implementing-the-event-based-asynchronous-pattern"></a>Implementando o padrão assíncrono baseado em evento

Se você estiver escrevendo uma classe com algumas operações que podem incorrer em atrasos perceptíveis, considere a possibilidade de fornecer funcionalidade assíncrona implementando o [padrão assíncrono baseado em evento](event-based-asynchronous-pattern-overview.md).

O Padrão assíncrono baseado em evento fornece uma maneira padronizada de empacotar uma classe que tem recursos assíncronos. Se for implementada com classes auxiliares como <xref:System.ComponentModel.AsyncOperationManager>, sua classe funcionará corretamente em qualquer modelo de aplicativo, incluindo ASP.NET, aplicativos de Console e aplicativos do Windows Forms.

Para obter um exemplo que implementa o Padrão assíncrono baseado em evento, consulte [Como implementar um componente compatível com o padrão assíncrono baseado em evento](component-that-supports-the-event-based-asynchronous-pattern.md).

Para operações assíncronas simples, você pode obter o componente <xref:System.ComponentModel.BackgroundWorker> adequado. Para saber mais sobre <xref:System.ComponentModel.BackgroundWorker>, consulte [Como executar uma operação em segundo plano](../../framework/winforms/controls/how-to-run-an-operation-in-the-background.md).

A lista a seguir descreve os recursos do padrão assíncrono baseado em evento discutidos neste tópico.

- Oportunidades para a implementação do Padrão assíncrono baseado em evento

- Nomenclatura de métodos assíncronos

- Suporte opcional de cancelamento

- Suporte opcional da propriedade IsBusy

- Fornecimento opcional de suporte para relatórios de progresso

- Fornecimento opcional de suporte para retorno de resultados incrementais

- Tratamento de parâmetros Out e Ref em métodos

## <a name="opportunities-for-implementing-the-event-based-asynchronous-pattern"></a>Oportunidades para a implementação do Padrão assíncrono baseado em evento

Considere a implementação do Padrão assíncrono baseado em evento quando:

- Os clientes de sua classe não precisam de objetos <xref:System.Threading.WaitHandle> e <xref:System.IAsyncResult> disponíveis para operações assíncronas, ou seja, a sondagem e <xref:System.Threading.WaitHandle.WaitAll%2A> ou <xref:System.Threading.WaitHandle.WaitAny%2A> deverão ser compilados pelo cliente.

- Você quer que as operações assíncronas sejam gerenciadas pelo cliente com o modelo de evento/delegado conhecido.

Qualquer operação é candidata para uma implementação assíncrona, mas aquelas com expectativa de latência longas devem ser consideradas. Operações nas quais os clientes chamam um método e são notificados após a conclusão, sem necessidade de intervenção adicional, são especialmente apropriadas. Também são apropriadas as operações executadas continuamente, notificando periodicamente os clientes sobre o andamento, resultados incrementais ou alterações de estado.

Para saber mais sobre como decidir pelo suporte ao Padrão assíncrono baseado em evento, confira [Decidir quando implementar o Padrão assíncrono baseado em evento](deciding-when-to-implement-the-event-based-asynchronous-pattern.md).

## <a name="naming-asynchronous-methods"></a>Nomenclatura de métodos assíncronos

Para cada método síncrono *MethodName* para o qual você deseja fornecer uma contraparte assíncrona:

Defina um método _MethodName_**Async** que:

- Retorna `void`.

- Usa os mesmos parâmetros que o método *MethodName*.

- Aceita várias invocações.

Opcionalmente, defina uma sobrecarga _MethodName_**Async**, idêntica ao _MethodName_**Async**, mas com um parâmetro adicional com valor de objeto chamado `userState`. Faça isso se você estiver preparado para gerenciar várias chamadas simultâneas de seu método. Nesse caso, o valor `userState` será entregue novamente para todos os manipuladores de eventos a fim de distinguir invocações do método. Você também pode optar por fazer isso simplesmente como um local para armazenar o estado do usuário para recuperação posterior.

Para cada assinatura de método _MethodName_**Async** separada:

1. Defina o evento a seguir na mesma classe que o método:

    ```vb
    Public Event MethodNameCompleted As MethodNameCompletedEventHandler
    ```

    ```csharp
    public event MethodNameCompletedEventHandler MethodNameCompleted;
    ```

2. Defina o representante a seguir e <xref:System.ComponentModel.AsyncCompletedEventArgs>. Provavelmente, eles serão definidos fora da própria classe, mas no mesmo namespace.

    ```vb
    Public Delegate Sub MethodNameCompletedEventHandler( _
        ByVal sender As Object, _
        ByVal e As MethodNameCompletedEventArgs)

    Public Class MethodNameCompletedEventArgs
        Inherits System.ComponentModel.AsyncCompletedEventArgs
    Public ReadOnly Property Result() As MyReturnType
    End Property
    ```

    ```csharp
    public delegate void MethodNameCompletedEventHandler(object sender,
        MethodNameCompletedEventArgs e);

    public class MethodNameCompletedEventArgs : System.ComponentModel.AsyncCompletedEventArgs
    {
        public MyReturnType Result { get; }
    }
    ```

    - Certifique-se de que a classe _MethodName_**CompletedEventArgs** expõe seus membros como propriedades somente leitura, e não como campos, pois os campos impedem a associação de dados.

    - Não defina qualquer classe derivada de <xref:System.ComponentModel.AsyncCompletedEventArgs> para métodos que não produzem resultados. Simplesmente use uma instância do próprio <xref:System.ComponentModel.AsyncCompletedEventArgs>.

      > [!NOTE]
      > É perfeitamente aceitável, quando apropriada e plausível, a reutilização de delegados e tipos <xref:System.ComponentModel.AsyncCompletedEventArgs>. Nesse caso, a nomenclatura não será consistente com o nome do método, pois um delegado e <xref:System.ComponentModel.AsyncCompletedEventArgs> específicos não ficarão vinculados a um único método.

## <a name="optionally-support-cancellation"></a>Suporte opcional de cancelamento

Se sua classe oferecer suporte ao cancelamento de operações assíncronas, o cancelamento deverá ser exposto ao cliente, conforme descrito abaixo. Perceba que há dois pontos de decisão que precisam ser alcançados antes de definir o suporte ao cancelamento:

- Sua classe, incluindo acréscimos futuros antecipados, tem apenas uma operação assíncrona que oferece suporte ao cancelamento?

- As operações assíncronas que oferecem suporte ao cancelamento podem dar suporte a várias operações pendentes? Ou seja, o método _MethodName_**Async** usa um parâmetro `userState` e ele permite várias invocações antes da conclusão de alguma?

Use as respostas para essas duas perguntas na tabela abaixo para determinar qual deve ser a assinatura para o método de cancelamento.

### <a name="visual-basic"></a>Visual Basic

||Várias operações simultâneas com suporte|Apenas uma operação por vez|
|------|------------------------------------------------|----------------------------------|
|Uma operação assíncrona em toda a classe|`Sub MethodNameAsyncCancel(ByVal userState As Object)`|`Sub MethodNameAsyncCancel()`|
|Várias operações assíncronas em classe|`Sub CancelAsync(ByVal userState As Object)`|`Sub CancelAsync()`|

### <a name="c"></a>C\#

||Várias operações simultâneas com suporte|Apenas uma operação por vez|
|------|------------------------------------------------|----------------------------------|
|Uma operação assíncrona em toda a classe|`void MethodNameAsyncCancel(object userState);`|`void MethodNameAsyncCancel();`|
|Várias operações assíncronas em classe|`void CancelAsync(object userState);`|`void CancelAsync();`|

Se você definir o método `CancelAsync(object userState)`, os clientes deverão ter cuidado ao escolher seus valores de estado a fim de torná-los capazes de distinguir entre todos os métodos assíncronos invocados no objeto, e não apenas entre todas as invocações de um único método assíncrono.

A decisão de nomear a versão de operação assíncrona única _MethodName_**AsyncCancel** tem base em ser capaz de descobrir mais facilmente o método em um ambiente de design como IntelliSense do Visual Studio. Isso agrupa os membros relacionados e os distingue de outros membros que não têm qualquer relação com funcionalidade assíncrona. Se você espera que outras operações assíncronas sejam adicionadas em versões subsequentes, é melhor definir `CancelAsync`.

Não defina vários métodos da tabela acima na mesma classe. Isso não fará sentido, ou sobrecarregará a interface de classe com a proliferação de métodos.

Normalmente, esses métodos retornarão imediatamente, e a operação poderá ou não ser realmente cancelada. No manipulador de eventos do evento _MethodName_**Completed**, o objeto _MethodName_**CompletedEventArgs** contém um campo `Cancelled`, o qual os clientes podem usar para determinar se o cancelamento ocorreu.

Obedeça à semântica de cancelamento descrita em [Melhores práticas para implementar o Padrão assíncrono baseado em evento](best-practices-for-implementing-the-event-based-asynchronous-pattern.md).

## <a name="optionally-support-the-isbusy-property"></a>Suporte opcional da propriedade IsBusy

Se sua classe não oferecer suporte a várias invocações simultâneas, considere a exposição de uma propriedade `IsBusy`. Isso permite que os desenvolvedores determinem se um método _MethodName_**Async** está sendo executado sem capturar uma exceção do método _MethodName_**Async**.

Obedeça à semântica `IsBusy` descrita em [Melhores práticas para implementar o Padrão assíncrono baseado em evento](best-practices-for-implementing-the-event-based-asynchronous-pattern.md).

## <a name="optionally-provide-support-for-progress-reporting"></a>Fornecimento opcional de suporte para relatórios de progresso

Costuma ser bom relatar o progresso durante uma operação assíncrona. O padrão assíncrono baseado em evento fornece uma orientação para fazer isso.

- Opcionalmente, defina um evento que será gerado pela operação assíncrona e invocado no thread apropriado. O objeto <xref:System.ComponentModel.ProgressChangedEventArgs> transporta um indicador de progresso com valor de inteiro que deve estar entre 0 e 100.

- Nomeie esse evento da seguinte maneira:

  - `ProgressChanged` se a classe tiver várias operações assíncronas (ou exista a expectativa de crescimento a fim de incluir várias operações assíncronas em versões futuras);

  - _MethodName_**ProgressChanged** se a classe tiver uma única operação assíncrona.

  Essa opção de nomenclatura é comparável à feita para o método de cancelamento, conforme descrito na seção Suporte opcional de cancelamento.

Esse evento deve usar a assinatura do delegado <xref:System.ComponentModel.ProgressChangedEventHandler> e a classe <xref:System.ComponentModel.ProgressChangedEventArgs>. Como alternativa, se um indicador de progresso mais específico ao domínio puder ser fornecido (por exemplo, bytes lidos e total de bytes de uma operação de download), você deverá definir uma classe derivada de <xref:System.ComponentModel.ProgressChangedEventArgs>.

Observe que há apenas um `ProgressChanged` ou _MethodName_evento**ProgressChanged** para a classe, independentemente do número de métodos assíncronos com suporte. Os clientes devem usar o objeto `userState` que é passado para os métodos _MethodName_**Async** a fim de distinguir entre as atualizações de andamento em várias operações simultâneas.

Pode haver situações em que várias operações dão suporte ao progresso e cada uma retorna um indicador diferente para o progresso. Nesse caso, um único `ProgressChanged` evento não é apropriado, e você pode considerar o suporte a vários eventos `ProgressChanged`. Nesse caso, use um padrão de nomenclatura de _MethodName_**ProgressChanged** para cada método _MethodName_**Async**.

Obedeça à semântica de relatório de progresso descrita em [Melhores práticas para implementar o Padrão assíncrono baseado em evento](best-practices-for-implementing-the-event-based-asynchronous-pattern.md).

## <a name="optionally-provide-support-for-returning-incremental-results"></a>Fornecimento opcional de suporte para retorno de resultados incrementais

Às vezes, uma operação assíncrona pode retornar resultados incrementais antes da conclusão. Há várias opções que podem ser usadas para oferecer suporte a esse cenário. Veja a seguir alguns exemplos.

### <a name="single-operation-class"></a>Classe de operação única

Se sua classe der suporte apenas a uma única operação assíncrona, e essa operação for capaz de retornar resultados incrementais, então:

- Estenda o tipo <xref:System.ComponentModel.ProgressChangedEventArgs> para carregar os dados de resultado incrementais e definir um evento _MethodName_**ProgressChanged** com esses dados estendidos.

- Gere esse evento _MethodName_**ProgressChanged** quando houver um resultado incremental para o relatório.

Essa solução aplica-se especificamente a uma classe de operação assíncrona única, pois não há nenhum problema com o mesmo evento ocorrendo para retornar resultados incrementais em "todas as operações", como faz o evento _MethodName_**ProgressChanged**.

### <a name="multiple-operation-class-with-homogeneous-incremental-results"></a>Classe de várias operações com resultados incrementais homogêneos

Nesse caso, sua classe oferece suporte a vários métodos assíncronos, cada um capaz de retornar resultados incrementais, e todos esses resultados incrementais têm o mesmo tipo de dados.

Siga o modelo descrito acima para classes de operação única, pois a mesma estrutura <xref:System.EventArgs> funcionará para todos os resultados incrementais. Defina um evento `ProgressChanged` em vez de um evento _MethodName_**ProgressChanged**, já que ele se aplica a vários métodos assíncronos.

### <a name="multiple-operation-class-with-heterogeneous-incremental-results"></a>Classe de várias operações com resultados incrementais heterogêneos

Se sua classe oferece suporte a vários métodos assíncronos, cada um retornando um tipo diferente de dados, você deve:

- Separar o relatório de resultado incremental de seu relatório de progresso.

- Definir um evento _MethodName_**ProgressChanged** separado com o <xref:System.EventArgs> apropriado para cada método assíncrono manipular dados de resultados incrementais desse método.

Invocar esse manipulador de eventos no thread apropriado, conforme descrito em [Melhores práticas para implementar o Padrão assíncrono baseado em evento](best-practices-for-implementing-the-event-based-asynchronous-pattern.md).

## <a name="handling-out-and-ref-parameters-in-methods"></a>Tratamento de parâmetros Out e Ref em métodos

Embora o uso de `out` e `ref` seja, em geral, desencorajado no .NET Framework, estas são as regras quando eles estiverem presentes:

Em um método síncrono *MethodName*:

- `out`os parâmetros para *MethodName* não devem fazer parte de _MethodName_**Async**. Em vez disso, eles devem fazer parte de _MethodName_**CompletedEventArgs** com o mesmo nome que seu parâmetro equivalente em *MethodName* (a menos que haja um nome mais apropriado).

- `ref`os parâmetros *para MethodName* devem aparecer como parte _de MethodName_**Async**e como parte de _MethodName_**CompletedEventArgs** com o mesmo nome que seu parâmetro equivalente em *MethodName* (a menos que haja um nome mais apropriado).

Por exemplo, considerando que:

```vb
Public Function MethodName(ByVal arg1 As String, ByRef arg2 As String, ByRef arg3 As String) As Integer
```

```csharp
public int MethodName(string arg1, ref string arg2, out string arg3);
```

Seu método assíncrono e sua classe <xref:System.ComponentModel.AsyncCompletedEventArgs> teria esta aparência:

```vb
Public Sub MethodNameAsync(ByVal arg1 As String, ByVal arg2 As String)

Public Class MethodNameCompletedEventArgs
    Inherits System.ComponentModel.AsyncCompletedEventArgs
    Public ReadOnly Property Result() As Integer
    End Property
    Public ReadOnly Property Arg2() As String
    End Property
    Public ReadOnly Property Arg3() As String
    End Property
End Class
```

```csharp
public void MethodNameAsync(string arg1, string arg2);

public class MethodNameCompletedEventArgs : System.ComponentModel.AsyncCompletedEventArgs
{
    public int Result { get; };
    public string Arg2 { get; };
    public string Arg3 { get; };
}
```

## <a name="see-also"></a>Confira também

- <xref:System.ComponentModel.ProgressChangedEventArgs>
- <xref:System.ComponentModel.AsyncCompletedEventArgs>
- [Como implementar um componente compatível com o padrão assíncrono baseado em evento](component-that-supports-the-event-based-asynchronous-pattern.md)
- [Como executar uma operação no plano de fundo](../../framework/winforms/controls/how-to-run-an-operation-in-the-background.md)
- [Como implementar um formulário que usa uma operação em segundo plano](../../framework/winforms/controls/how-to-implement-a-form-that-uses-a-background-operation.md)
- [Decidindo quando implementar o padrão assíncrono baseado em evento](deciding-when-to-implement-the-event-based-asynchronous-pattern.md)
- [Práticas recomendadas para a implementação do padrão assíncrono baseado em evento](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)
- [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md)
