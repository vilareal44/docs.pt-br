---
title: Programação assíncrona em C#
description: Uma visão geral do suporte de linguagem C# para programação assíncrona usando async, await, Task e Task<T>
ms.date: 06/04/2020
ms.openlocfilehash: 992ccd3a015653ea9ee13dfc309d47711ad0fca4
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619709"
---
# <a name="asynchronous-programming-with-async-and-await"></a>Programação assíncrona com async e await

O [modelo de programação assíncrona de tarefa (TAP)](task-asynchronous-programming-model.md) fornece uma abstração sobre código assíncrono. Você escreve o código como uma sequência de instruções, como usual. Você pode ler o código como se cada instrução fosse concluída antes do início da próxima. O compilador realiza uma série de transformações, porque algumas dessas instruções podem iniciar o trabalho e retornar um <xref:System.Threading.Tasks.Task> que representa o trabalho em andamento.

Essa é a meta dessa sintaxe: habilitar um código que leia como uma sequência de instruções, mas que execute em uma ordem muito mais complicada com base na alocação de recurso externo e em quando as tarefas são concluídas. Isso é semelhante à maneira como as pessoas dão instruções para processos que incluem tarefas assíncronas. Ao longo deste artigo, você usará um exemplo de instruções para fazer uma café de manhã para ver como as `async` palavras-chave e facilitam o `await` motivo do código, que inclui uma série de instruções assíncronas. Você deve escrever as instruções de maneira parecida com a lista a seguir para explicar como fazer um café da manhã:

1. Encher uma xícara de café.
1. Aquecer uma frigideira e, em seguida, fritar dois ovos.
1. Frita três fatias de bacon.
1. Torrar dois pedaços de pão.
1. Adicionar manteiga e a geleia na torrada.
1. Encher um copo com suco de laranja.

Se tivesse experiência em culinária, você executaria essas instruções **assincronamente**. Você iniciaria aquecendo a frigideira para os ovos e, em seguida, começaria a preparar o bacon. Você colocaria o pão na torradeira e começaria a preparar os ovos. Em cada etapa do processo, iniciaria uma tarefa e voltaria sua atenção para as tarefas que estivessem prontas para a sua atenção.

Preparar o café da manhã é um bom exemplo de trabalho assíncrono que não é paralelo. Uma pessoa (ou um thread) pode lidar com todas essas tarefas. Continuando com a analogia do café da manhã, uma pessoa pode fazer café da manhã assincronamente iniciando a tarefa seguinte antes de concluir a primeira. O preparo progride independentemente de haver alguém observando. Assim que inicia o aquecimento da frigideira para os ovos, você pode começar a fritar o bacon. Quando começar a preparar o bacon, você pode colocar o pão na torradeira.

Para um algoritmo paralelo, você precisaria de vários cozinheiros (ou threads). Um prepararia os ovos, outro o bacon e assim por diante. Cada um se concentraria apenas naquela tarefa específica. Cada cozinheiro (ou thread) ficaria bloqueado de forma síncrona, esperando que o bacon estivesse pronto para ser virado ou que a torrada pulasse.

Agora, considere essas mesmas instruções escritas como instruções em C#:

:::code language="csharp" source="snippets/index/AsyncBreakfast-starter/Program.cs" highlight="8-27":::

:::image type="content" source="media/synchronous-breakfast.png" alt-text="café de manhã síncrono":::

O café de manhã preparado de forma síncrona levou aproximadamente 30 minutos porque o total é a soma de cada tarefa individual.

> [!NOTE]
> As `Coffee` `Egg` classes,, `Bacon` , `Toast` e `Juice` estão vazias. Eles são simplesmente classes de marcador para fins de demonstração, não contêm propriedades e não servem para nenhuma outra finalidade.

Os computadores não interpretam essas instruções da mesma forma que as pessoas. O computador ficará bloqueado em cada instrução até que o trabalho seja concluído, antes de passar para a próxima instrução. Isso cria um café da manhã insatisfatório. As tarefas posteriores não seriam iniciadas até que as tarefas anteriores fossem concluídas. Levaria muito mais tempo para criar o café da manhã e alguns itens ficariam frios antes de serem servidos.

Se você quiser que o computador execute as instruções acima de forma assíncrona, deverá escrever o código assíncrono.

Essas questões são importantes para os programas que você escreve atualmente. Ao escrever programas de cliente, você quer que a interface do usuário responda de acordo com as solicitações do usuário. Seu aplicativo não deve fazer um telefone parecer travado enquanto ele está baixando dados da Web. Ao escrever programas de servidor, você não quer threads bloqueados. Esses threads poderiam servir a outras solicitações. O uso de código síncrono quando existem alternativas assíncronas afeta sua capacidade de aumentar de forma menos custosa. Você paga pelos threads bloqueados.

Aplicativos modernos bem-sucedidos exigem código assíncrono. Sem suporte de linguagem, escrever código assíncrono exigia retornos de chamada, eventos de conclusão ou outros meios que obscureciam a intenção original do código. A vantagem do código síncrono é que ele é fácil de entender. As ações passo a passo facilitam o exame e o entendimento. Modelos assíncronos tradicionais forçavam você a se concentrar na natureza assíncrona do código e não nas ações fundamentais do código.

## <a name="dont-block-await-instead"></a>Não bloquear, mas aguardar

O código anterior demonstra uma prática inadequada: construção de código síncrono para realizar operações assíncronas. Como escrito, esse código bloqueia o thread que o está executando, impedindo-o de realizar qualquer outra tarefa. Ele não será interrompido enquanto qualquer uma das tarefas estiver em andamento. Seria como se você fixasse o olhar na torradeira depois de colocar o pão. Você ignoraria qualquer pessoa que estivesse conversando com você até que a torrada pulasse.

Vamos começar atualizando esse código para que o thread não seja bloqueado enquanto houver tarefas em execução. A palavra-chave `await` oferece uma maneira sem bloqueio de iniciar uma tarefa e, em seguida, continuar a execução quando essa tarefa for concluída. Uma versão assíncrona simples do código de fazer café da manhã ficaria como o snippet a seguir:

:::code language="csharp" source="snippets/index/AsyncBreakfast-V2/Program.cs" id="SnippetMain":::

> [!IMPORTANT]
> O tempo total decorrido é aproximadamente o mesmo que a versão inicial do synchonous. O código ainda tem de aproveitar alguns dos principais recursos da programação assíncrona.

> [!TIP]
> Os corpos de método do `FryEggsAsync` , `FryBaconAsync` , e `ToastBreadAsync` foram atualizados para retornar `Task<Egg>` , `Task<Bacon>` e, `Task<Toast>` respectivamente. Os métodos são renomeados de sua versão original para incluir o sufixo "Async". Suas implementações são mostradas como parte da [versão final](#final-version) mais adiante neste artigo.

Esse código não bloqueia enquanto os ovos ou o bacon são preparados. Entretanto, esse código não iniciará outras tarefas. Você ainda colocaria o pão na torradeira e ficaria olhando até ele pular. Mas, pelo menos, você responderia a qualquer pessoa que quisesse sua atenção. Em um restaurante em que vários pedidos são feitos, o cozinheiro pode iniciar o preparo de outro café da manhã enquanto prepara o primeiro.

Agora, o thread trabalhando no café da manhã não fica bloqueado aguardando qualquer tarefa iniciada que ainda não tenha terminado. Para alguns aplicativos, essa alteração já basta. Um aplicativo de GUI ainda responde ao usuário com apenas essa alteração. No entanto, neste cenário, você quer mais. Você não deseja que cada uma das tarefas componentes seja executada em sequência. É melhor iniciar cada uma das tarefas componentes antes de aguardar a conclusão da tarefa anterior.

## <a name="start-tasks-concurrently"></a>Iniciar tarefas simultaneamente

Em muitos cenários, convém iniciar várias tarefas independentes imediatamente. Em seguida, conforme cada tarefa é concluída, você pode continuar outro trabalho que esteja pronto. Na analogia do café da manhã, é assim que você prepara o café da manhã muito mais rapidamente. Você também prepara tudo quase ao mesmo tempo. Você terá um café da manhã quente.

O <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> e os tipos relacionados são classes que você pode usar para pensar nas tarefas que estão em andamento. Elas permitem que você escreva código que se assemelhe mais à maneira como você realmente prepara o café da manhã. Você começaria a preparar os ovos, o bacon e a torrada ao mesmo tempo. Como cada um exige ação, você voltaria sua atenção para essa tarefa, cuidaria da próxima ação e aguardaria algo mais que exigisse sua atenção.

Você inicia uma tarefa e espera o objeto <xref:System.Threading.Tasks.Task> que representa o trabalho. Você vai `await` cada tarefa antes de trabalhar com o respectivo resultado.

Vamos fazer essas alterações no código do café da manhã. A primeira etapa é armazenar as tarefas para as operações quando elas forem iniciadas, em vez de aguardá-las:

```csharp
Coffee cup = PourCoffee();
Console.WriteLine("coffee is ready");

Task<Egg> eggsTask = FryEggsAsync(2);
Egg eggs = await eggsTask;
Console.WriteLine("eggs are ready");

Task<Bacon> baconTask = FryBaconAsync(3);
Bacon bacon = await baconTask;
Console.WriteLine("bacon is ready");

Task<Toast> toastTask = ToastBreadAsync(2);
Toast toast = await toastTask;
ApplyButter(toast);
ApplyJam(toast);
Console.WriteLine("toast is ready");

Juice oj = PourOJ();
Console.WriteLine("oj is ready");
Console.WriteLine("Breakfast is ready!");
```

Em seguida, você pode mover as instruções `await` do bacon e dos ovos até o final do método, antes de servir o café da manhã:

```csharp
Coffee cup = PourCoffee();
Console.WriteLine("coffee is ready");

Task<Egg> eggsTask = FryEggsAsync(2);
Task<Bacon> baconTask = FryBaconAsync(3);
Task<Toast> toastTask = ToastBreadAsync(2);

Toast toast = await toastTask;
ApplyButter(toast);
ApplyJam(toast);
Console.WriteLine("toast is ready");
Juice oj = PourOJ();
Console.WriteLine("oj is ready");

Egg eggs = await eggsTask;
Console.WriteLine("eggs are ready");
Bacon bacon = await baconTask;
Console.WriteLine("bacon is ready");

Console.WriteLine("Breakfast is ready!");
```

:::image type="content" source="media/asynchronous-breakfast.png" alt-text="café de manhã assíncrono":::

A manhã preparada assincronamente levou aproximadamente 20 minutos, isso porque algumas tarefas podiam ser executadas simultaneamente.

O código anterior funciona melhor. Você inicia todas as tarefas assíncronas ao mesmo tempo. Você aguarda cada tarefa somente quando precisar dos resultados. O código anterior pode ser semelhante a um código em um aplicativo Web que faz solicitações de diferentes microsserviços e combina os resultados em uma única página. Você fará todas as solicitações imediatamente e, em seguida, `await` em todas essas tarefas e comporá a página da Web.

## <a name="composition-with-tasks"></a>Composição com tarefas

 Você prepara tudo para o café da manhã ao mesmo tempo, exceto a torrada. Preparar a torrada é a composição de uma operação assíncrona (torrar o pão) com operações síncronas (adicionar a manteiga e a geleia). A atualização deste código ilustra um conceito importante:

> [!IMPORTANT]
> A composição de uma operação assíncrona seguida por trabalho síncrono é uma operação assíncrona. Explicando de outra forma, se qualquer parte de uma operação for assíncrona, toda a operação será assíncrona.

O código anterior mostrou que você pode usar objetos <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601> para manter tarefas em execução. Você `await` em cada tarefa antes de usar seu resultado. A próxima etapa é criar métodos que declarem a combinação de outro trabalho. Antes de servir o café da manhã, você quer aguardar a tarefa que representa torrar o pão antes de adicionar manteiga e geleia. Você pode declarar esse trabalho com o código a seguir:

:::code language="csharp" source="snippets/index/AsyncBreakfast-V3/Program.cs" id="SnippetComposeToastTask":::

O método anterior tem o modificador `async` na sua assinatura. Isso sinaliza ao compilador que esse método contém uma instrução `await`; ele contém operações assíncronas. Este método representa a tarefa que torra o pão e, em seguida, adiciona manteiga e geleia. Esse método retorna um <xref:System.Threading.Tasks.Task%601> que representa a composição dessas três operações. O principal bloco de código agora se torna:

:::code language="csharp" source="snippets/index/AsyncBreakfast-V3/Program.cs" id="SnippetMain":::

A alteração anterior ilustrou uma técnica importante para trabalhar com código assíncrono. Você pode compor tarefas, separando as operações em um novo método que retorna uma tarefa. Você pode escolher quando aguardar essa tarefa. Você pode iniciar outras tarefas simultaneamente.

## <a name="await-tasks-efficiently"></a>Aguardar tarefas com eficiência

A série de instruções `await` no final do código anterior pode ser melhorada usando métodos da classe `Task`. Uma dessas APIs é a <xref:System.Threading.Tasks.Task.WhenAll%2A>, que retorna um <xref:System.Threading.Tasks.Task> que é concluído ao final de todas as tarefas na lista de argumentos, conforme mostrado no código a seguir:

```csharp
await Task.WhenAll(eggsTask, baconTask, toastTask);
Console.WriteLine("eggs are ready");
Console.WriteLine("bacon is ready");
Console.WriteLine("toast is ready");
Console.WriteLine("Breakfast is ready!");
```

Outra opção é usar <xref:System.Threading.Tasks.Task.WhenAny%2A>, que retorna uma `Task<Task>` que é concluída quando qualquer um dos argumentos é concluído. Você pode aguardar a tarefa retornada, sabendo que ela já foi concluída. O código a seguir mostra como você poderia usar <xref:System.Threading.Tasks.Task.WhenAny%2A> para aguardar a primeira tarefa concluir e, em seguida, processar seu resultado. Depois de processar o resultado da tarefa concluída, você remove essa tarefa concluída da lista de tarefas passada para `WhenAny`.

```csharp
var breakfastTasks = new List<Task> { eggsTask, baconTask, toastTask };
while (breakfastTasks.Count > 0)
{
    Task finishedTask = await Task.WhenAny(breakfastTasks);
    if (finishedTask == eggsTask)
    {
        Console.WriteLine("eggs are ready");
    }
    else if (finishedTask == baconTask)
    {
        Console.WriteLine("bacon is ready");
    }
    else if (finishedTask == toastTask)
    {
        Console.WriteLine("toast is ready");
    }
    breakfastTasks.Remove(finishedTask);
}
```

Depois de todas essas alterações, a versão final do código tem esta aparência:<a id="final-version"></a>
:::code language="csharp" source="snippets/index/AsyncBreakfast-final/Program.cs" highlight="9-40":::

:::image type="content" source="media/whenany-async-breakfast.png" alt-text="Quando qualquer café assíncrono":::

A versão final do café da manhã preparado de forma assíncrona levou aproximadamente 15 minutos, isso porque algumas tarefas podiam ser executadas simultaneamente, e o código conseguiu monitorar várias tarefas de uma só vez e agir apenas quando fosse necessário.

Esse código final é assíncrono. Ele reflete mais precisamente como uma pessoa poderia preparar um café da manhã. Compare o código anterior com o primeiro exemplo de código neste artigo. As ações principais permanecem claras ao ler o código. Você pode ler esse código da mesma forma como faria ao ler essas instruções para fazer um café da manhã no início deste artigo. Os recursos de linguagem para `async` e `await` fornecem a tradução que todas as pessoas fazem para seguir essas instruções escritas: iniciar tarefas assim que possível e não ficar bloqueado ao aguardar a conclusão de tarefas.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Saiba mais sobre o modelo de programação assíncrona de tarefas](task-asynchronous-programming-model.md)
