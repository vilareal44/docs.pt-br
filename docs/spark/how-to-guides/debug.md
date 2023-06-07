---
title: Depurar um aplicativo .NET para Apache Spark no Windows
description: Saiba como depurar seu aplicativo .NET para Apache Spark no Windows.
ms.date: 06/25/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 9209d5bdec6dd85f6d21a502fb07204effef1934
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617750"
---
# <a name="debug-a-net-for-apache-spark-application"></a>Depurar um aplicativo .NET para Apache Spark

Este "como" fornece as etapas para depurar o aplicativo .NET para Apache Spark no Windows.

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="debug-your-application"></a>Depurar seu aplicativo

Abra uma nova janela de prompt de comando e execute o seguinte comando:

```shell
spark-submit \
  --class org.apache.spark.deploy.dotnet.DotnetRunner \
  --master local \
  <path-to-microsoft-spark-jar> \
  debug
```

Quando você executa o comando, você vê a seguinte saída:

```console
***********************************************************************
* .NET Backend running debug mode. Press enter to exit *
***********************************************************************
```

No modo de depuração, o DotnetRunner não inicia o aplicativo .NET, mas, em vez disso, espera que você inicie o aplicativo .NET. Deixe essa janela do prompt de comando aberta e inicie seu aplicativo .NET por meio do depurador do C# para depurar seu aplicativo. Inicie seu aplicativo .NET com um depurador do C# ([depurador do Visual Studio para Windows/MacOS](https://visualstudio.microsoft.com/vs/) ou [extensão do depurador do c# no Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging)) para depurar seu aplicativo.

## <a name="debug-a-user-defined-function-udf"></a>Depurar uma UDF (função definida pelo usuário)

> [!NOTE]
> As funções definidas pelo usuário têm suporte apenas no Windows com o depurador do Visual Studio.

Antes de executar `spark-submit` , defina a seguinte variável de ambiente:

```bat
set DOTNET_WORKER_DEBUG=1
```

Quando você executar o aplicativo Spark, uma `Choose Just-In-Time Debugger` janela será exibida. Escolha um depurador do Visual Studio.

O depurador será interrompido no seguinte local em [TaskRunner.cs](https://github.com/dotnet/spark/blob/5e9c08b430b4bc56b5f42252c4b73437377afaed/src/csharp/Microsoft.Spark.Worker/TaskRunner.cs#L52):

```csharp
if (EnvironmentUtils.GetEnvironmentVariableAsBool("DOTNET_WORKER_DEBUG"))
{
    Debugger.Launch(); // <-- The debugger will break here.
}
```

Navegue até o arquivo *. cs* que contém o UDF que você planeja depurar e [defina um ponto de interrupção](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints?view=vs-2019). O ponto de interrupção dirá `The breakpoint will not currently be hit` porque o trabalho ainda não carregou o assembly que contém o UDF.

Pressione `F5` para continuar seu aplicativo e o ponto de interrupção eventualmente será atingido.

> [!NOTE]
> A janela escolher depurador just-in-time é exibida para cada tarefa. Para evitar pop-ups em excesso, defina o número de executores como um número baixo. Por exemplo, você pode usar a opção **--Master local [1]** para o Spark-Submit para definir o número de tarefas como 1, que inicia uma única instância do depurador.

## <a name="debug-scala-code"></a>Depurar código Scala

Se você precisar depurar o código do lado do escala ( `DotnetRunner` , `DotnetBackendHandler` etc.), poderá usar o comando a seguir e anexar um depurador ao processo em execução usando [IntelliJ](https://www.jetbrains.com/help/idea/attaching-to-local-process.html):

```shell
spark-submit \
  --driver-java-options -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 \
  --class org.apache.spark.deploy.dotnet.DotnetRunner \
  --master local \
  <path-to-microsoft-spark-jar> \
  <path-to-your-app-exe> <argument(s)-to-your-app>
```

Após executar o comando, anexe um depurador ao processo em execução usando o [IntelliJ](https://www.jetbrains.com/help/idea/attaching-to-local-process.html).

## <a name="next-steps"></a>Próximas etapas

* [Introdução ao .NET para Apache Spark](../tutorials/get-started.md)
* [Implantar um aplicativo .NET para Apache Spark no Azure HDInsight](../tutorials/hdinsight-deployment.md)
* [Implantar um aplicativo .NET para Apache Spark no Databricks](../tutorials/databricks-deployment.md)
* [Implantar um aplicativo .NET para Apache Spark no Amazon EMR Spark](../tutorials/amazon-emr-spark-deployment.md)
