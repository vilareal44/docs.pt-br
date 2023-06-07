---
title: Usar variáveis de difusão no .NET para Apache Spark
description: Saiba como usar variáveis de difusão no .NET para aplicativos Apache Spark.
ms.date: 06/25/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: d86b160855cc4d3f3a6502f5606d4766b7c06aa0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617850"
---
# <a name="use-broadcast-variables-in-net-for-apache-spark"></a>Usar variáveis de difusão no .NET para Apache Spark

Neste artigo, você aprenderá a usar variáveis de difusão no .NET para Apache Spark. As [variáveis de difusão no Apache Spark](https://spark.apache.org/docs/2.2.0/rdd-programming-guide.html#broadcast-variables) são mecanismos para compartilhar variáveis em executores que se destinam a ser somente leitura. As variáveis de difusão permitem manter uma variável somente leitura armazenada em cache em cada máquina, em vez de enviar uma cópia dela com tarefas. Você pode usar variáveis de difusão para dar a cada nó uma cópia de um conjunto de dados de entrada grande de maneira eficiente.

Como os dados são enviados apenas uma vez, as variáveis de difusão têm benefícios de desempenho quando comparadas com as variáveis locais que são enviadas aos executores com cada tarefa. Consulte a [documentação da variável de difusão oficial](https://spark.apache.org/docs/2.2.0/rdd-programming-guide.html#broadcast-variables) para obter uma compreensão mais profunda das variáveis de difusão e por que elas são usadas.

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="create-broadcast-variables"></a>Criar variáveis de difusão

Para criar uma variável de difusão, chame `SparkContext.Broadcast(v)` para qualquer variável `v` . A variável de difusão é um wrapper em volta da variável `v` e seu valor pode ser acessado chamando o `Value()` método.

No trecho de código a seguir, uma variável de cadeia de caracteres `v` é criada e uma variável de difusão `bv` é criada quando `SparkContext.Broadcast(v)` é chamado. Observe que o parâmetro de tipo para `Broadcast` , String, corresponde ao tipo da variável que está sendo transmitida. A UDF (função definida pelo usuário) retorna o valor de `bv` .

```csharp
string v = "Variable to be broadcasted";
Broadcast<string> bv = SparkContext.Broadcast(v);

Func<Column, Column> udf = Udf<string, string>(
    str => $"{str}: {bv.Value()}");
```

## <a name="delete-broadcast-variables"></a>Excluir variáveis de difusão

A variável de difusão pode ser excluída de todos os executores chamando o `Destroy()` método.

```csharp
bv.Destroy();
```

`Destroy()`exclui todos os dados e metadados relacionados à variável de difusão e devem ser usados com cautela. Depois que uma variável de difusão é destruída, ela não pode ser usada novamente.

## <a name="limit-broadcast-variable-scope-in-udfs"></a>Limitar o escopo da variável de difusão em UDFs

Ao usar variáveis de difusão em UDFs, você precisa limitar o escopo da variável somente ao UDF que está fazendo referência à variável. O [guia para usar UDFs](udf-guide.md) descreve esse fenômeno em detalhes. O escopo é especialmente crucial quando você chama `Destroy()` na variável de difusão.

Se a variável de difusão que foi destruída estiver visível ou acessível de outros UDFs, ela será selecionada para a serialização por todas as UDFs, mesmo se não estiver sendo referenciada por elas. O .NET para Apache Spark não pode serializar a variável de difusão destruída, o que resulta em um erro. O trecho de código a seguir demonstra esse erro:

```csharp
string v = "Variable to be broadcasted";
Broadcast<string> bv = SparkContext.Broadcast(v);

// Using the broadcast variable in a UDF:
Func<Column, Column> udf1 = Udf<string, string>(
    str => $"{str}: {bv.Value()}");

// Destroying bv
bv.Destroy();

// Calling udf1 after destroying bv throws the following expected exception:
// org.apache.spark.SparkException: Attempted to use Broadcast(0) after it was destroyed
df.Select(udf1(df["_1"])).Show();

// Different UDF udf2 that is not referencing bv
Func<Column, Column> udf2 = Udf<string, string>(
    str => $"{str}: not referencing broadcast variable");

// Calling udf2 throws the following (unexpected) exception:
// [Error] [JvmBridge] org.apache.spark.SparkException: Task not serializable
df.Select(udf2(df["_1"])).Show();
```

O trecho de código a seguir demonstra como garantir que a destruição `bv` não afete `udf2` devido a um comportamento de serialização inesperado:

```csharp
string v = "Variable to be broadcasted";
// Restricting the visibility of bv to only the UDF referencing it
{
    Broadcast<string> bv = SparkContext.Broadcast(v);

    // Using the broadcast variable in a UDF:
    Func<Column, Column> udf1 = Udf<string, string>(
        str => $"{str}: {bv.Value()}");

    // Destroying bv
    bv.Destroy();
}

// Different UDF udf2 that is not referencing bv
Func<Column, Column> udf2 = Udf<string, string>(
    str => $"{str}: not referencing broadcast variable");

// Calling udf2 works fine as expected
df.Select(udf2(df["_1"])).Show();
```

## <a name="next-steps"></a>Próximas etapas

* [Introdução ao .NET para Apache Spark](../tutorials/get-started.md)
* [Depurar um aplicativo .NET para Apache Spark no Windows](debug.md)
* [Implantar .NET para Apache Spark trabalho e binários de função definidos pelo usuário](deploy-worker-udf-binaries.md)
