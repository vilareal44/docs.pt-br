---
title: Novidades no C# 7.1
description: Uma visão geral dos novos recursos no C# 7.1.
ms.date: 04/09/2019
ms.openlocfilehash: fe6e49eb01e24a27bc7970900c05150378ab194a
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174764"
---
# <a name="whats-new-in-c-71"></a>Novidades no C# 7.1

O C# 7.1 é a primeira versão de ponto da linguagem C#. Ele marca uma cadência de versão maior para a linguagem. Use os novos recursos mais cedo, de preferência, quando cada novo recurso estiver pronto. O C# 7.1 adiciona a capacidade de configurar o compilador para que ele corresponda a uma versão especificada da linguagem. Isso permite que você separe a decisão de atualizar ferramentas da decisão de atualizar versões da linguagem.

O C# 7.1 adiciona a [seleção de versão da linguagem](../language-reference/configure-language-version.md) elemento de configuração, três novos recursos de linguagem e um novo comportamento do compilador.

Os novos recursos de linguagem nesta versão são:

- [`async``Main`método](#async-main)
  - O ponto de entrada para um aplicativo pode ter o modificador `async`.
- [`default`expressões literais](#default-literal-expressions)
  - Use expressões literais padrão em expressões de valor padrão quando o tipo de destino pode ser inferido.
- [Nomes de elementos de tupla inferidos](#inferred-tuple-element-names)
  - Em muitos casos, os nomes dos elementos de tupla podem ser inferidos com base na inicialização da tupla.
- [Restrições em parâmetros de tipo genérico](#pattern-matching-on-generic-type-parameters)
  - Você pode usar expressões de correspondência de padrão em variáveis cujo tipo é um parâmetro de tipo genérico.

Por fim, o compilador traz duas opções `-refout` e `-refonly`, que controlam a [geração de assembly de referência](#reference-assembly-generation).

Para usar as últimas funcionalidades em uma versão de ponto, você precisa [configurar a versão da linguagem do compilador](../language-reference/configure-language-version.md) e selecionar a versão.

O restante deste artigo fornece uma visão geral de cada recurso. Para cada recurso, você aprenderá o raciocínio por trás dele. Você aprenderá a sintaxe. Você pode explorar esses recursos em seu ambiente usando a ferramenta global `dotnet try`:

1. Instale a ferramenta global [dotnet-try](https://github.com/dotnet/try/blob/master/README.md#setup).
1. Clone o repositório [dotnet/try-samples](https://github.com/dotnet/try-samples).
1. Definir o diretório atual do subdiretório *csharp7* para o repositório *try-samples*.
1. Execute `dotnet try`.

## <a name="async-main"></a>Async main

Um método *async main* permite que você use `await` no método `Main`.
Anteriormente, você precisava escrever:

```csharp
static int Main()
{
    return DoAsyncWork().GetAwaiter().GetResult();
}
```

Agora, você pode escrever:

```csharp
static async Task<int> Main()
{
    // This could also be replaced with the body
    // DoAsyncWork, including its await expressions:
    return await DoAsyncWork();
}
```

Se o programa não retorna um código de saída, declare um método `Main` que retorna um <xref:System.Threading.Tasks.Task>:

```csharp
static async Task Main()
{
    await SomeAsyncMethod();
}
```

Leia mais sobre os detalhes no tópico [async main](../programming-guide/main-and-command-args/index.md) no guia de programação.

## <a name="default-literal-expressions"></a>Expressões literais padrão

Expressões literais padrão são uma melhoria das expressões de valor padrão.
Essas expressões inicializam uma variável com o valor padrão. Nos casos em que você anteriormente escrevia:

```csharp
Func<string, bool> whereClause = default(Func<string, bool>);
```

Agora você pode omitir o tipo no lado direito da inicialização:

```csharp
Func<string, bool> whereClause = default;
```

Para saber mais, confira a seção [Literais padrão](../language-reference/operators/default.md#default-literal) do artigo do [operador padrão](../language-reference/operators/default.md).

## <a name="inferred-tuple-element-names"></a>Nomes de elementos de tupla inferidos

Esse recurso é uma pequena melhoria do recurso de tuplas introduzido no C# 7.0. Muitas vezes quando você inicializa uma tupla, as variáveis usadas para o lado direito da atribuição são iguais aos nomes que você deseja usar para os elementos de tupla:

```csharp
int count = 5;
string label = "Colors used in the map";
var pair = (count: count, label: label);
```

Os nomes dos elementos de tupla podem ser inferidos com base nas variáveis usadas para inicializar a tupla no C# 7.1:

```csharp
int count = 5;
string label = "Colors used in the map";
var pair = (count, label); // element names are "count" and "label"
```

Você pode saber mais sobre esse recurso no artigo [tipos de tupla](../language-reference/builtin-types/value-tuples.md) .

## <a name="pattern-matching-on-generic-type-parameters"></a>Restrições em parâmetros de tipo genérico

A partir do C# 7.1, a expressão de padrão para o padrão de tipo `is` e `switch` pode ter o tipo de um parâmetro de tipo genérico. Isso pode ser mais útil ao verificar tipos que podem ser do tipo `struct` ou `class` e você deseja evitar conversão boxing.

## <a name="reference-assembly-generation"></a>Geração de assembly de referência

Há duas novas opções de compilador que geram *assemblies somente de referência*: [-refout](../language-reference/compiler-options/refout-compiler-option.md) e [-refonly](../language-reference/compiler-options/refonly-compiler-option.md).
Os artigos vinculados explicam essas opções e os assemblies de referência mais detalhadamente.
