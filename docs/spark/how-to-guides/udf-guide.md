---
title: Criar funções definidas pelo usuário (UDF) no .NET para Apache Spark
description: Saiba como implementar o UDF (funções definidas pelo usuário) no .NET para aplicativos Apache Spark.
ms.date: 06/25/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 97afda8ed17d3719c534d72ad3ad026745a70922
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85620918"
---
# <a name="create-user-defined-functions-udf-in-net-for-apache-spark"></a>Criar funções definidas pelo usuário (UDF) no .NET para Apache Spark

Neste artigo, você aprenderá a usar o UDF (funções definidas pelo usuário) no .NET para Apache Spark. [UDFs)](https://spark.apache.org/docs/latest/api/java/org/apache/spark/sql/expressions/UserDefinedFunction.html) o é um recurso do Spark que permite usar funções personalizadas para estender a funcionalidade interna do sistema. Os UDFs transformam valores de uma única linha dentro de uma tabela para produzir um único valor de saída correspondente por linha com base na lógica definida no UDF.

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="define-udfs"></a>Definir UDFs

Examine a seguinte definição de UDF:

```csharp
string s1 = "hello";
Func<Column, Column> udf = Udf<string, string>(
    str => $"{s1} {str}");
```

O UDF usa uma `string` como entrada na forma de uma [coluna](https://github.com/dotnet/spark/blob/master/src/csharp/Microsoft.Spark/Sql/Column.cs#L14) de um [dataframe](https://github.com/dotnet/spark/blob/master/src/csharp/Microsoft.Spark/Sql/DataFrame.cs#L24)e retorna um `string` com `hello` anexado na frente da entrada.

O dataframe a seguir `df` contém uma lista de nomes:

```text
+-------+
|   name|
+-------+
|Michael|
|   Andy|
| Justin|
+-------+
```

Agora, vamos aplicar o definido acima `udf` ao dataframe `df` :

```csharp
DataFrame udfResult = df.Select(udf(df["name"]));
```

O dataframe a seguir `udfResult` é o resultado do UDF:

```text
+-------------+
|         name|
+-------------+
|hello Michael|
|   hello Andy|
| hello Justin|
+-------------+
```

Para entender melhor como implementar UDFs, examine as [funções auxiliares UDF](https://github.com/dotnet/spark/blob/master/src/csharp/Microsoft.Spark/Sql/Functions.cs#L3616) e [exemplos](https://github.com/dotnet/spark/blob/master/src/csharp/Microsoft.Spark.E2ETest/UdfTests/UdfSimpleTypesTests.cs#L49) no github.

## <a name="udf-serialization"></a>Serialização de UDF

Como as UDFs são funções que precisam ser executadas em trabalhadores, elas precisam ser serializadas e enviadas aos trabalhadores como parte da carga do driver. O [delegado](../../csharp/programming-guide/delegates/index.md), que é uma referência ao método, precisa ser serializado, bem como seu [destino](xref:System.Delegate.Target%2A), que é a instância de classe na qual o delegado atual invoca o método de instância. Examine este [exemplo de código no GitHub](https://github.com/dotnet/spark/blob/master/src/csharp/Microsoft.Spark/Utils/CommandSerDe.cs#L149) para entender melhor como a serialização UDF está sendo feita.

O .NET para Apache Spark usa o .NET Core, que não dá suporte a delegados de serialização. Em vez disso, a reflexão é usada para serializar o destino onde o delegado é definido. Quando vários delegados são definidos em um escopo comum, eles têm um fechamento compartilhado que se torna o destino de reflexão para serialização.

### <a name="serialization-example"></a>Exemplo de serialização

O trecho de código a seguir define duas variáveis String que estão sendo referenciadas em dois delegados de função que retornam as respectivas cadeias de caracteres como resultado:

```csharp
using System;

public class C {
    public void M() {
        string s1 = "s1";
        string s2 = "s2";
        Func<string, string> a = str => s1;
        Func<string, string> b = str => s2;
    }
}
```

O código C# acima gera o seguinte código de desmontagem de C# (fonte de crédito: [sharplab.Io](https://sharplab.io)) do compilador:

```csharp
public class C
{
    [CompilerGenerated]
    private sealed class <>c__DisplayClass0_0
    {
        public string s1;

        public string s2;

        internal string <M>b__0(string str)
        {
            return s1;
        }

        internal string <M>b__1(string str)
        {
            return s2;
        }
    }

    public void M()
    {
        <>c__DisplayClass0_0 <>c__DisplayClass0_ = new <>c__DisplayClass0_0();
        <>c__DisplayClass0_.s1 = "s1";
        <>c__DisplayClass0_.s2 = "s2";
        Func<string, string> func = new Func<string, string>(<>c__DisplayClass0_.<M>b__0);
        Func<string, string> func2 = new Func<string, string>(<>c__DisplayClass0_.<M>b__1);
    }
}
```

Ambos `func` e `func2` compartilham o mesmo fechamento `<>c__DisplayClass0_0` , que é o destino que é serializado ao serializar os delegados `func` e `func2` . Embora `Func<string, string> a` o só faça referência a `s1` , `s2` o também é serializado quando os bytes são enviados aos trabalhadores.

Isso pode levar a alguns comportamentos inesperados no tempo de execução (como no caso do uso de [variáveis de difusão](broadcast-guide.md)), motivo pelo qual é recomendável restringir a visibilidade das variáveis usadas em uma função para o escopo dessa função.

O trecho de código a seguir é a maneira recomendada para implementar o comportamento de serialização desejado:

```csharp
using System;

public class C {
    public void M() {
        {
            string s1 = "s1";
            Func<string, string> a = str => s1;
        }
        {
            string s2 = "s2";
            Func<string, string> b = str => s2;
        }
    }
}
```

O código C# acima gera o seguinte código de desmontagem de C# (fonte de crédito: [sharplab.Io](https://sharplab.io)) do compilador:

```csharp
public class C
{
    [CompilerGenerated]
    private sealed class <>c__DisplayClass0_0
    {
        public string s1;

        internal string <M>b__0(string str)
        {
            return s1;
        }
    }

    [CompilerGenerated]
    private sealed class <>c__DisplayClass0_1
    {
        public string s2;

        internal string <M>b__1(string str)
        {
            return s2;
        }
    }

    public void M()
    {
        <>c__DisplayClass0_0 <>c__DisplayClass0_ = new <>c__DisplayClass0_0();
        <>c__DisplayClass0_.s1 = "s1";
        Func<string, string> func = new Func<string, string>(<>c__DisplayClass0_.<M>b__0);
        <>c__DisplayClass0_1 <>c__DisplayClass0_2 = new <>c__DisplayClass0_1();
        <>c__DisplayClass0_2.s2 = "s2";
        Func<string, string> func2 = new Func<string, string>(<>c__DisplayClass0_2.<M>b__1);
    }
}
```

Observe que `func` e `func2` não compartilha mais um fechamento e eles têm seus próprios fechamentos separados `<>c__DisplayClass0_0` e, `<>c__DisplayClass0_1` respectivamente. Quando usado como o destino para serialização, nada além das variáveis referenciadas será serializado para o delegado. Esse comportamento é importante para se ter em mente ao implementar vários UDFs em um escopo comum.

## <a name="some-spark-udf-caveats"></a>Algumas advertências de UDF do Spark

* Valores nulos em UDFs podem gerar exceções. É responsabilidade do desenvolvedor tratá-los.
* As UDFs não aproveitam as otimizações fornecidas pelas funções internas do Spark, portanto, é recomendável usar funções internas sempre que possível.

## <a name="next-steps"></a>Próximas etapas

* [Introdução ao .NET para Apache Spark](../tutorials/get-started.md)
* [Depurar um aplicativo .NET para Apache Spark no Windows](debug.md)
