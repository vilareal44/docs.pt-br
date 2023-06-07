---
title: Como serializar e desserializar JSON usando C#-.NET
description: Este artigo mostra como usar o System.Text.Json namespace para serializar e desserializar do JSON no .net. Ele inclui o código de exemplo.
ms.date: 05/13/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: 7ad2721f12c5d14b61b35ecf7696ff0d6a6f27da
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84289506"
---
# <a name="how-to-serialize-and-deserialize-marshal-and-unmarshal-json-in-net"></a>Como serializar e desserializar (empacotar e desempacotar) JSON no .NET

Este artigo mostra como usar o <xref:System.Text.Json> namespace para serializar e desserializar de e para o JavaScript Object Notation (JSON). Se você estiver portando um código existente do `Newtonsoft.Json` , consulte [como migrar `System.Text.Json` para o ](system-text-json-migrate-from-newtonsoft-how-to.md).

As direções e o código de exemplo usam a biblioteca diretamente, não por meio de uma estrutura como [ASP.NET Core](/aspnet/core/).

A maior parte do código de exemplo de serialização define <xref:System.Text.Json.JsonSerializerOptions.WriteIndented?displayProperty=nameWithType> como `true` "muito impresso" o JSON (com recuo e espaço em branco para legibilidade humana). Para uso em produção, você normalmente aceitaria o valor padrão de `false` para essa configuração.

Os exemplos de código referem-se à seguinte classe e variantes dela:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWF)]

## <a name="namespaces"></a>Namespaces

O <xref:System.Text.Json> namespace contém todos os pontos de entrada e os tipos principais. O <xref:System.Text.Json.Serialization> namespace contém atributos e APIs para cenários avançados e personalização específicas para serialização e desserialização. Os exemplos de código mostrados neste artigo exigem `using` diretivas para um ou ambos os namespaces:

```csharp
using System.Text.Json;
using System.Text.Json.Serialization;
```

<xref:System.Runtime.Serialization>Atualmente, não há suporte para atributos do namespace no `System.Text.Json` .

## <a name="how-to-write-net-objects-to-json-serialize"></a>Como gravar objetos .NET em JSON (Serialize)

Para gravar JSON em uma cadeia de caracteres ou em um arquivo, chame o <xref:System.Text.Json.JsonSerializer.Serialize%2A?displayProperty=nameWithType> método.

O exemplo a seguir cria JSON como uma cadeia de caracteres:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToString.cs?name=SnippetSerialize)]

O exemplo a seguir usa código síncrono para criar um arquivo JSON:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToFile.cs?name=SnippetSerialize)]

O exemplo a seguir usa código assíncrono para criar um arquivo JSON:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToFileAsync.cs?name=SnippetSerialize)]

Os exemplos anteriores usam a inferência de tipos para o tipo que está sendo serializado. Uma sobrecarga de `Serialize()` usa um parâmetro de tipo genérico:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToString.cs?name=SnippetSerializeWithGenericParameter)]

### <a name="serialization-example"></a>Exemplo de serialização

Aqui está uma classe de exemplo que contém coleções e uma classe aninhada:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPOCOs)]

A saída JSON da serialização de uma instância do tipo anterior é semelhante ao exemplo a seguir. A saída JSON é reduzidos por padrão:

```json
{"Date":"2019-08-01T00:00:00-07:00","TemperatureCelsius":25,"Summary":"Hot","DatesAvailable":["2019-08-01T00:00:00-07:00","2019-08-02T00:00:00-07:00"],"TemperatureRanges":{"Cold":{"High":20,"Low":-10},"Hot":{"High":60,"Low":20}},"SummaryWords":["Cool","Windy","Humid"]}
```

O exemplo a seguir mostra o mesmo JSON, formatado (ou seja, muito impresso com espaço em branco e recuo):

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "TemperatureRanges": {
    "Cold": {
      "High": 20,
      "Low": -10
    },
    "Hot": {
      "High": 60,
      "Low": 20
    }
  },
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

### <a name="serialize-to-utf-8"></a>Serializar para UTF-8

Para serializar para UTF-8, chame o <xref:System.Text.Json.JsonSerializer.SerializeToUtf8Bytes%2A?displayProperty=nameWithType> método:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToUtf8.cs?name=SnippetSerialize)]

Uma <xref:System.Text.Json.JsonSerializer.Serialize%2A> sobrecarga que usa um <xref:System.Text.Json.Utf8JsonWriter> também está disponível.

A serialização para UTF-8 é de cerca de 5-10% mais rápida do que usar os métodos baseados em cadeia de caracteres. A diferença é porque os bytes (como UTF-8) não precisam ser convertidos em cadeias de caracteres (UTF-16).

## <a name="serialization-behavior"></a>Comportamento de serialização

* Por padrão, todas as propriedades públicas são serializadas. Você pode [especificar propriedades a serem excluídas](#exclude-properties-from-serialization).
* O [codificador padrão](xref:System.Text.Encodings.Web.JavaScriptEncoder.Default) escapa caracteres não ASCII, caracteres sensíveis a HTML dentro do intervalo ASCII e caracteres que devem ser ignorados de acordo com [a especificação JSON RFC 8259](https://tools.ietf.org/html/rfc8259#section-7).
* Por padrão, o JSON é reduzidos. Você pode [muito imprimir o JSON](#serialize-to-formatted-json).
* Por padrão, a capitalização de nomes JSON corresponde aos nomes .NET. Você pode personalizar o uso de [maiúsculas e minúsculas de nomes](#customize-json-names-and-values).
* Referências circulares são detectadas e exceções geradas.
* No momento, os campos são excluídos.

Os tipos com suporte incluem:

* Primitivos .NET que mapeiam para primitivos JavaScript, como tipos numéricos, cadeias de caracteres e booliano.
* [Pocos (objetos CLR antigos)](https://stackoverflow.com/questions/250001/poco-definition)definidos pelo usuário.
* Matrizes unidimensionais e denteadas ( `ArrayName[][]` ).
* `Dictionary<string,TValue>`onde `TValue` é `object` , `JsonElement` , ou um poco.
* Coleções dos namespaces a seguir.
  * <xref:System.Collections>
  * <xref:System.Collections.Generic>
  * <xref:System.Collections.Immutable>

Você pode [implementar conversores personalizados](system-text-json-converters-how-to.md) para lidar com tipos adicionais ou para fornecer funcionalidade que não é suportada pelos conversores internos.

## <a name="how-to-read-json-into-net-objects-deserialize"></a>Como ler JSON em objetos .NET (desserializar)

Para desserializar de uma cadeia de caracteres ou de um arquivo, chame o <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> método.

O exemplo a seguir lê JSON de uma cadeia de caracteres e cria uma instância da `WeatherForecast` classe mostrada anteriormente para o [exemplo de serialização](#serialization-example):

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToString.cs?name=SnippetDeserialize)]

Para desserializar de um arquivo usando código síncrono, leia o arquivo em uma cadeia de caracteres, conforme mostrado no exemplo a seguir:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToFile.cs?name=SnippetDeserialize)]

Para desserializar de um arquivo usando código assíncrono, chame o <xref:System.Text.Json.JsonSerializer.DeserializeAsync%2A> método:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToFileAsync.cs?name=SnippetDeserialize)]

### <a name="deserialize-from-utf-8"></a>Desserializar de UTF-8

Para desserializar do UTF-8, chame uma <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> sobrecarga que usa um `Utf8JsonReader` ou um `ReadOnlySpan<byte>` , conforme mostrado nos exemplos a seguir. Os exemplos pressupõem que o JSON esteja em uma matriz de bytes chamada jsonUtf8Bytes.

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToUtf8.cs?name=SnippetDeserialize1)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToUtf8.cs?name=SnippetDeserialize2)]

## <a name="deserialization-behavior"></a>Comportamento de desserialização

* Por padrão, a correspondência de nome de propriedade diferencia maiúsculas de minúsculas. Você pode [especificar a não diferenciação de maiúsculas e minúsculas](#case-insensitive-property-matching).
* Se o JSON contiver um valor para uma propriedade somente leitura, o valor será ignorado e nenhuma exceção será lançada.
* Não há suporte para desserialização para tipos de referência sem um construtor com parâmetros.
* A desserialização para objetos imutáveis ou propriedades somente leitura não tem suporte.
* Por padrão, há suporte para enums como números. Você pode [serializar nomes de enumeração como cadeias de caracteres](#enums-as-strings).
* Não há suporte para campos.
* Por padrão, comentários ou vírgulas à direita nas exceções do JSON throw. Você pode [permitir comentários e vírgulas à direita](#allow-comments-and-trailing-commas).
* A [profundidade máxima padrão](xref:System.Text.Json.JsonReaderOptions.MaxDepth) é 64.

Você pode [implementar conversores personalizados](system-text-json-converters-how-to.md) para fornecer funcionalidade que não é suportada pelos conversores internos.

## <a name="serialize-to-formatted-json"></a>Serializar para JSON formatado

Para imprimir a saída JSON com muita impressão, defina <xref:System.Text.Json.JsonSerializerOptions.WriteIndented?displayProperty=nameWithType> como `true` :

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToString.cs?name=SnippetSerializePrettyPrint)]

Aqui está um tipo de exemplo a ser serializado e a saída JSON bem impressa:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWF)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

## <a name="customize-json-names-and-values"></a>Personalizar nomes e valores JSON

Por padrão, os nomes de propriedade e as chaves de dicionário são inalterados na saída JSON, incluindo maiúsculas e minúsculas. Os valores de enumeração são representados como números. Esta seção explica como:

* [Personalizar nomes de propriedade individuais](#customize-individual-property-names)
* [Converter todos os nomes de propriedade em camel case](#use-camel-case-for-all-json-property-names)
* [Implementar uma política de nomenclatura de propriedade personalizada](#use-a-custom-json-property-naming-policy)
* [Converter chaves de dicionário em camel case](#camel-case-dictionary-keys)
* [Converter enums em cadeias de caracteres e camel case](#enums-as-strings)

Para outros cenários que exigem tratamento especial de valores e nomes de propriedade JSON, você pode [implementar conversores personalizados](system-text-json-converters-how-to.md).

### <a name="customize-individual-property-names"></a>Personalizar nomes de propriedade individuais

Para definir o nome de propriedades individuais, use o atributo [[JsonPropertyName]](xref:System.Text.Json.Serialization.JsonPropertyNameAttribute) .

Veja um exemplo de tipo para serializar e o JSON resultante:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPropertyNameAttribute)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "Wind": 35
}
```

O nome da propriedade definido por este atributo:

* Aplica-se em ambas as direções, para serialização e desserialização.
* Tem precedência sobre as políticas de nomenclatura de propriedade.

### <a name="use-camel-case-for-all-json-property-names"></a>Usar o camel case para todos os nomes de propriedade JSON

Para usar o camel case para todos os nomes de propriedade JSON, defina como <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy?displayProperty=nameWithType> `JsonNamingPolicy.CamelCase` , conforme mostrado no exemplo a seguir:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundTripCamelCasePropertyNames.cs?name=Serialize)]

Aqui está uma classe de exemplo para serializar e a saída JSON:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPropertyNameAttribute)]

```json
{
  "date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "summary": "Hot",
  "Wind": 35
}
```

A política de nomenclatura de Propriedade do camel case:

* Aplica-se à serialização e desserialização.
* É substituído por `[JsonPropertyName]` atributos. É por isso que o nome da propriedade JSON `Wind` no exemplo não é camel case.

### <a name="use-a-custom-json-property-naming-policy"></a>Usar uma política de nomenclatura de propriedade JSON personalizada

Para usar uma política de nomenclatura de propriedade JSON personalizada, crie uma classe derivada de <xref:System.Text.Json.JsonNamingPolicy> e substitua o <xref:System.Text.Json.JsonNamingPolicy.ConvertName%2A> método, conforme mostrado no exemplo a seguir:

[!code-csharp[](snippets/system-text-json-how-to/csharp/UpperCaseNamingPolicy.cs)]

Em seguida, defina a <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy?displayProperty=nameWithType> propriedade como uma instância da sua classe de política de nomenclatura:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripPropertyNamingPolicy.cs?name=SnippetSerialize)]

Aqui está uma classe de exemplo para serializar e a saída JSON:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPropertyNameAttribute)]

```json
{
  "DATE": "2019-08-01T00:00:00-07:00",
  "TEMPERATURECELSIUS": 25,
  "SUMMARY": "Hot",
  "Wind": 35
}
```

A política de nomenclatura de propriedade JSON:

* Aplica-se à serialização e desserialização.
* É substituído por `[JsonPropertyName]` atributos. É por isso que o nome da propriedade JSON `Wind` no exemplo não é maiúscula.

### <a name="camel-case-dictionary-keys"></a>Chaves de dicionário do camel case

Se uma propriedade de um objeto a ser serializado for do tipo `Dictionary<string,TValue>` , as `string` chaves poderão ser convertidas em camel case. Para fazer isso, defina <xref:System.Text.Json.JsonSerializerOptions.DictionaryKeyPolicy> como `JsonNamingPolicy.CamelCase` , conforme mostrado no exemplo a seguir:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCamelCaseDictionaryKeys.cs?name=SnippetSerialize)]

Serializar um objeto com um dicionário chamado `TemperatureRanges` que tem pares chave-valor `"ColdMinTemp", 20` e `"HotMinTemp", 40` resultaria em uma saída JSON como o exemplo a seguir:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "TemperatureRanges": {
    "coldMinTemp": 20,
    "hotMinTemp": 40
  }
}
```

A política de nomenclatura do camel case para chaves de dicionário se aplica somente à serialização. Se você desserializar um dicionário, as chaves corresponderão ao arquivo JSON mesmo que você especifique `JsonNamingPolicy.CamelCase` para o `DictionaryKeyPolicy` .

### <a name="enums-as-strings"></a>Enumerações como cadeias de caracteres

Por padrão, as enumerações são serializadas como números. Para serializar nomes de enumeração como cadeias de caracteres, use o <xref:System.Text.Json.Serialization.JsonStringEnumConverter> .

Por exemplo, suponha que você precise serializar a seguinte classe que tem uma enumeração:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithEnum)]

Se o resumo for `Hot` , por padrão, o JSON serializado terá o valor numérico 3:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": 3
}
```

O código de exemplo a seguir serializa os nomes de enumeração em vez dos valores numéricos e converte os nomes em camel case:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripEnumAsString.cs?name=SnippetSerialize)]

O JSON resultante é semelhante ao exemplo a seguir:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "hot"
}
```

Os nomes de cadeias de caracteres de enumeração também podem ser desserializados, conforme mostrado no exemplo a seguir:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripEnumAsString.cs?name=SnippetDeserialize)]

## <a name="exclude-properties-from-serialization"></a>Excluir propriedades da serialização

Por padrão, todas as propriedades públicas são serializadas. Se você não quiser que alguns deles apareçam na saída JSON, terá várias opções. Esta seção explica como excluir:

* [Propriedades individuais](#exclude-individual-properties)
* [Todas as propriedades somente leitura](#exclude-all-read-only-properties)
* [Todas as propriedades de valor nulo](#exclude-all-null-value-properties)

### <a name="exclude-individual-properties"></a>Excluir propriedades individuais

Para ignorar as propriedades individuais, use o atributo [[JsonIgnore]](xref:System.Text.Json.Serialization.JsonIgnoreAttribute) .

Aqui está um tipo de exemplo para serializar e a saída JSON:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithIgnoreAttribute)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
}
```

### <a name="exclude-all-read-only-properties"></a>Excluir todas as propriedades somente leitura

Uma propriedade será somente leitura se ela contiver um getter público, mas não um setter público. Para excluir todas as propriedades somente leitura, defina como <xref:System.Text.Json.JsonSerializerOptions.IgnoreReadOnlyProperties?displayProperty=nameWithType> `true` , conforme mostrado no exemplo a seguir:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeExcludeReadOnlyProperties.cs?name=SnippetSerialize)]

Aqui está um tipo de exemplo para serializar e a saída JSON:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithROProperty)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
}
```

Essa opção se aplica somente à serialização. Durante a desserialização, as propriedades somente leitura são ignoradas por padrão.

### <a name="exclude-all-null-value-properties"></a>Excluir todas as propriedades de valor nulo

Para excluir todas as propriedades de valor nulo, defina a <xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues> propriedade como `true` , conforme mostrado no exemplo a seguir:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeExcludeNullValueProperties.cs?name=SnippetSerialize)]

Aqui está um objeto de exemplo para serializar e a saída JSON:

|Propriedade |Valor  |
|---------|---------|
| Data    | 8/1/2019 12:00:00 AM-07:00|
| TemperatureCelsius| 25 |
| Resumo| nulo|

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25
}
```

Essa configuração se aplica à serialização e desserialização. Para obter informações sobre o efeito na desserialização, consulte [ignorar nulo ao desserializar](#ignore-null-when-deserializing).

## <a name="customize-character-encoding"></a>Personalizar codificação de caracteres

Por padrão, o serializador escapa todos os caracteres não ASCII.  Ou seja, ele substitui por `\uxxxx` onde `xxxx` está o código Unicode do caractere.  Por exemplo, se a `Summary` propriedade for definida como cirílico жарко, o `WeatherForecast` objeto será serializado, conforme mostrado neste exemplo:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "\u0436\u0430\u0440\u043A\u043E"
}
```

### <a name="serialize-language-character-sets"></a>Serializar conjuntos de caracteres de idioma

Para serializar os conjuntos de caracteres de um ou mais idiomas sem saída, especifique os [intervalos Unicode](xref:System.Text.Unicode.UnicodeRanges) ao criar uma instância do <xref:System.Text.Encodings.Web.JavaScriptEncoder?displayProperty=fullName> , conforme mostrado no exemplo a seguir:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetUsings)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetLanguageSets)]

Este código não sai de escape de caracteres cirílico ou grego. Se a `Summary` propriedade for definida como cirílico жарко, o `WeatherForecast` objeto será serializado, conforme mostrado neste exemplo:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "жарко"
}
```

Para serializar todos os conjuntos de idiomas sem saída, use <xref:System.Text.Unicode.UnicodeRanges.All?displayProperty=nameWithType> .

### <a name="serialize-specific-characters"></a>Serializar caracteres específicos

Uma alternativa é especificar os caracteres individuais que você deseja permitir sem ter escape. O exemplo a seguir serializa apenas os dois primeiros caracteres de жарко:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetUsings)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetSelectedCharacters)]

Aqui está um exemplo de JSON produzido pelo código anterior:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "жа\u0440\u043A\u043E"
}
```

### <a name="serialize-all-characters"></a>Serializar todos os caracteres

Para minimizar a saída, você pode usar <xref:System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping?displayProperty=nameWithType> , conforme mostrado no exemplo a seguir:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetUsings)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetUnsafeRelaxed)]

> [!CAUTION]
> Em comparação com o codificador padrão, o `UnsafeRelaxedJsonEscaping` codificador é mais permissivo de permitir que os caracteres passem sem escape:
>
> * Ele não sai de caracteres sensíveis a HTML, como `<` ,, `>` `&` e `'` .
> * Ele não oferece nenhuma proteção adicional de defesa profunda contra ataques XSS ou de divulgação de informações, como aqueles que podem resultar do cliente e do servidor que estão descordando no conjunto de *caracteres*.
>
> Use o codificador não seguro somente quando for conhecido que o cliente irá interpretar a carga resultante como JSON codificado em UTF-8. Por exemplo, você pode usá-lo se o servidor estiver enviando o cabeçalho de resposta `Content-Type: application/json; charset=utf-8` . Nunca permita que a `UnsafeRelaxedJsonEscaping` saída bruta seja emitida em uma página HTML ou em um `<script>` elemento.

## <a name="serialize-properties-of-derived-classes"></a>Serializar Propriedades de classes derivadas

Não há suporte para a serialização de uma hierarquia de tipo polimórfico. Por exemplo, se uma propriedade for definida como uma interface ou uma classe abstrata, somente as propriedades definidas na interface ou na classe abstrata serão serializadas, mesmo que o tipo de tempo de execução tenha propriedades adicionais. As exceções a esse comportamento são explicadas nesta seção.

Por exemplo, suponha que você tenha uma `WeatherForecast` classe e uma classe derivada `WeatherForecastDerived` :

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWF)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFDerived)]

E suponha que o argumento de tipo do `Serialize` método em tempo de compilação seja `WeatherForecast` :

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs?name=SnippetSerializeDefault)]

Nesse cenário, a `WindSpeed` propriedade não é serializada, mesmo que o `weatherForecast` objeto seja, na verdade, um `WeatherForecastDerived` objeto. Somente as propriedades da classe base são serializadas:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

Esse comportamento destina-se a ajudar a evitar a exposição acidental de dados em um tipo criado de tempo de execução derivado.

Para serializar as propriedades do tipo derivado no exemplo anterior, use uma das seguintes abordagens:

* Chame uma sobrecarga de <xref:System.Text.Json.JsonSerializer.Serialize%2A> que permite especificar o tipo em tempo de execução:

  [!code-csharp[](snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs?name=SnippetSerializeGetType)]

* Declare o objeto a ser serializado como `object` .

  [!code-csharp[](snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs?name=SnippetSerializeObject)]

No cenário de exemplo anterior, ambas as abordagens fazem com que a `WindSpeed` propriedade seja incluída na saída JSON:

```json
{
  "WindSpeed": 35,
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

> [!IMPORTANT]
> Essas abordagens fornecem serialização polimórfica somente para o objeto raiz a ser serializado, não para propriedades desse objeto raiz.

Você pode obter a serialização polimórfica para objetos de nível inferior se defini-los como tipo `object` . Por exemplo, suponha que sua `WeatherForecast` classe tenha uma propriedade chamada `PreviousForecast` que possa ser definida como tipo `WeatherForecast` ou `object` :

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPrevious)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPreviousAsObject)]

Se a `PreviousForecast` Propriedade contiver uma instância de `WeatherForecastDerived` :

* A saída JSON da serialização `WeatherForecastWithPrevious` **não inclui** `WindSpeed` .
* A saída JSON da serialização `WeatherForecastWithPreviousAsObject` **inclui** `WindSpeed` .

Para serializar `WeatherForecastWithPreviousAsObject` , não é necessário chamar `Serialize<object>` ou `GetType` porque o objeto raiz não é aquele que pode ser de um tipo derivado. O exemplo de código a seguir não chama `Serialize<object>` ou `GetType` :

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs?name=SnippetSerializeSecondLevel)]

O código anterior serializa corretamente `WeatherForecastWithPreviousAsObject` :

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "PreviousForecast": {
    "WindSpeed": 35,
    "Date": "2019-08-01T00:00:00-07:00",
    "TemperatureCelsius": 25,
    "Summary": "Hot"
  }
}
```

A mesma abordagem de definir propriedades como `object` funciona com interfaces. Suponha que você tenha a seguinte interface e implementação e queira serializar uma classe com propriedades que contêm instâncias de implementação:

[!code-csharp[](snippets/system-text-json-how-to/csharp/IForecast.cs)]

Quando você serializa uma instância do `Forecasts` , `Tuesday` o mostra apenas a `WindSpeed` propriedade, porque `Tuesday` é definido como `object` :

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs?name=SnippetSerializeInterface)]

O exemplo a seguir mostra o JSON que resulta do código anterior:

```json
{
  "Monday": {
    "Date": "2020-01-06T00:00:00-08:00",
    "TemperatureCelsius": 10,
    "Summary": "Cool"
  },
  "Tuesday": {
    "Date": "2020-01-07T00:00:00-08:00",
    "TemperatureCelsius": 11,
    "Summary": "Rainy",
    "WindSpeed": 10
  }
}
```

Para obter mais informações sobre a **serialização**polimórfica e informações sobre a **desserialização**, consulte [como migrar do Newtonsoft.Json para System.Text.Json o ](system-text-json-migrate-from-newtonsoft-how-to.md#polymorphic-serialization).

## <a name="allow-comments-and-trailing-commas"></a>Permitir comentários e vírgulas à direita

Por padrão, comentários e vírgulas à direita não são permitidos em JSON. Para permitir comentários no JSON, defina a <xref:System.Text.Json.JsonSerializerOptions.ReadCommentHandling?displayProperty=nameWithType> propriedade como `JsonCommentHandling.Skip` .
E para permitir vírgulas à direita, defina a <xref:System.Text.Json.JsonSerializerOptions.AllowTrailingCommas?displayProperty=nameWithType> propriedade como `true` . O exemplo a seguir mostra como permitir:

[!code-csharp[](snippets/system-text-json-how-to/csharp/DeserializeCommasComments.cs?name=SnippetDeserialize)]

Aqui está um exemplo de JSON com comentários e uma vírgula à direita:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25, // Fahrenheit 77
  "Summary": "Hot", /* Zharko */
}
```

## <a name="case-insensitive-property-matching"></a>Correspondência de propriedade que não diferencia maiúsculas de minúsculas

Por padrão, a desserialização procura as correspondências de nome de propriedade que diferenciam maiúsculas de minúsculas entre JSON e as propriedades do objeto de destino. Para alterar esse comportamento, defina <xref:System.Text.Json.JsonSerializerOptions.PropertyNameCaseInsensitive?displayProperty=nameWithType> como `true` :

[!code-csharp[](snippets/system-text-json-how-to/csharp/DeserializeCaseInsensitive.cs?name=SnippetDeserialize)]

Aqui está um exemplo de JSON com nomes de Propriedade do camel case. Ele pode ser desserializado no seguinte tipo que tem nomes de propriedade de caso de Pascal.

```json
{
  "date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "summary": "Hot",
}
```

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWF)]

## <a name="handle-overflow-json"></a>Processar JSON de estouro

Durante a desserialização, você pode receber dados no JSON que não são representados pelas propriedades do tipo de destino. Por exemplo, suponha que o tipo de destino seja o seguinte:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWF)]

E o JSON a ser desserializado é o seguinte:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "Summary": "Hot",
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

Se você desserializar o JSON mostrado no tipo mostrado, as `DatesAvailable` Propriedades e `SummaryWords` ficarão sem lugar e serão perdidas. Para capturar dados adicionais, como essas propriedades, aplique o atributo [JsonExtensionData](xref:System.Text.Json.Serialization.JsonExtensionDataAttribute) a uma propriedade do tipo `Dictionary<string,object>` ou `Dictionary<string,JsonElement>` :

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithExtensionData)]

Quando você desserializar o JSON mostrado anteriormente neste tipo de exemplo, os dados extras se tornarão pares de chave-valor da `ExtensionData` Propriedade:

|Propriedade |Valor  |Observações  |
|---------|---------|---------|
| Data    | 8/1/2019 12:00:00 AM-07:00||
| TemperatureCelsius| 0 | Incompatibilidade de maiúsculas e minúsculas ( `temperatureCelsius` no JSON), portanto, a propriedade não está definida. |
| Resumo | Dinâmica ||
| ExtensionData | temperatureCelsius: 25 |Como o caso não corresponde, essa propriedade JSON é um extra e se torna um par chave-valor no dicionário.|
|| DatesAvailable:<br>  8/1/2019 12:00:00 AM-07:00<br>8/2/2019 12:00:00 AM-07:00 |A propriedade extra do JSON torna-se um par chave-valor, com uma matriz como o objeto de valor.|
| |SummaryWords:<br>Estática<br>Vento<br>Humid |A propriedade extra do JSON torna-se um par chave-valor, com uma matriz como o objeto de valor.|

Quando o objeto de destino é serializado, os pares de valor de chave de dados de extensão se tornam propriedades JSON, assim como no JSON de entrada:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 0,
  "Summary": "Hot",
  "temperatureCelsius": 25,
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

Observe que o `ExtensionData` nome da propriedade não aparece no JSON. Esse comportamento permite que o JSON faça uma viagem de ida e volta sem perder nenhum dado extra que, de outra forma, não seria desserializado.

## <a name="ignore-null-when-deserializing"></a>Ignorar nulo ao desserializar

Por padrão, se uma propriedade em JSON for nula, a propriedade correspondente no objeto de destino será definida como NULL. Em alguns cenários, a propriedade de destino pode ter um valor padrão e você não quer um valor nulo para substituir o padrão.

Por exemplo, suponha que o código a seguir represente o objeto de destino:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithDefault)]

E suponha que o JSON a seguir seja desserializado:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": null
}
```

Após a desserialização, a `Summary` Propriedade do `WeatherForecastWithDefault` objeto é nula.

Para alterar esse comportamento, defina <xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues?displayProperty=nameWithType> como `true` , conforme mostrado no exemplo a seguir:

[!code-csharp[](snippets/system-text-json-how-to/csharp/DeserializeIgnoreNull.cs?name=SnippetDeserialize)]

Com essa opção, a `Summary` Propriedade do `WeatherForecastWithDefault` objeto é o valor padrão "sem Resumo" após a desserialização.

Os valores nulos no JSON serão ignorados somente se forem válidos. Valores nulos para tipos de valores não anuláveis causam exceções.

## <a name="utf8jsonreader-utf8jsonwriter-and-jsondocument"></a>Utf8JsonReader, Utf8JsonWriter e JsonDocument

<xref:System.Text.Json.Utf8JsonReader?displayProperty=fullName>é um leitor de alto desempenho, de baixa alocação e somente de encaminhamento para texto JSON codificado em UTF-8, lido de um `ReadOnlySpan<byte>` ou `ReadOnlySequence<byte>` . O `Utf8JsonReader` é um tipo de baixo nível que pode ser usado para criar analisadores e desserializadores personalizados. O <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> método usa `Utf8JsonReader` nos bastidores.

<xref:System.Text.Json.Utf8JsonWriter?displayProperty=fullName>é uma maneira de alto desempenho para escrever texto JSON codificado em UTF-8 de tipos comuns do .NET como `String` , `Int32` e `DateTime` . O gravador é um tipo de baixo nível que pode ser usado para criar serializadores personalizados. O <xref:System.Text.Json.JsonSerializer.Serialize%2A?displayProperty=nameWithType> método usa `Utf8JsonWriter` nos bastidores.

<xref:System.Text.Json.JsonDocument?displayProperty=fullName>fornece a capacidade de criar um Modelo de Objeto do Documento somente leitura (DOM) usando o `Utf8JsonReader` . O DOM fornece acesso aleatório aos dados em uma carga JSON. Os elementos JSON que compõem a carga podem ser acessados por meio do <xref:System.Text.Json.JsonElement> tipo. O `JsonElement` tipo fornece enumeradores de matriz e de objeto juntamente com APIs para converter texto JSON em tipos .net comuns. `JsonDocument`expõe uma <xref:System.Text.Json.JsonDocument.RootElement> propriedade.

As seções a seguir mostram como usar essas ferramentas para ler e gravar o JSON.

## <a name="use-jsondocument-for-access-to-data"></a>Usar o JsonDocument para acessar dados

O exemplo a seguir mostra como usar a <xref:System.Text.Json.JsonDocument> classe para acesso aleatório a dados em uma cadeia de caracteres JSON:

[!code-csharp[](snippets/system-text-json-how-to/csharp/JsonDocumentDataAccess.cs?name=SnippetAverageGrades1)]

O código anterior:

* Assume que o JSON a ser analisado está em uma cadeia de caracteres chamada `jsonString` .
* Calcula uma classificação média para objetos em uma `Students` matriz que tem uma `Grade` propriedade.
* Atribui uma classificação padrão de 70 para alunos que não têm uma classificação.
* Conta os alunos incrementando uma `count` variável com cada iteração. Uma alternativa é chamar <xref:System.Text.Json.JsonElement.GetArrayLength%2A> , conforme mostrado no exemplo a seguir:

  [!code-csharp[](snippets/system-text-json-how-to/csharp/JsonDocumentDataAccess.cs?name=SnippetAverageGrades2)]

Veja um exemplo do JSON que esse código processa:

[!code-json[](snippets/system-text-json-how-to/csharp/GradesPrettyPrint.json)]

## <a name="use-jsondocument-to-write-json"></a>Usar JsonDocument para gravar JSON

O exemplo a seguir mostra como gravar JSON de um <xref:System.Text.Json.JsonDocument> :

[!code-csharp[](snippets/system-text-json-how-to/csharp/JsonDocumentWriteJson.cs?name=SnippetSerialize)]

O código anterior:

* Lê um arquivo JSON, carrega os dados em um `JsonDocument` e grava JSON formatado (bem-impresso) em um arquivo.
* Usa <xref:System.Text.Json.JsonDocumentOptions> para especificar que os comentários no JSON de entrada são permitidos, mas ignorados.
* Quando terminar, chama <xref:System.Text.Json.Utf8JsonWriter.Flush%2A> o gravador. Uma alternativa é deixar o gravador AutoFlush quando ele é Descartado.

Aqui está um exemplo de entrada JSON a ser processada pelo código de exemplo:

[!code-json[](snippets/system-text-json-how-to/csharp/Grades.json)]

O resultado é a seguinte saída JSON bem impressa:

[!code-json[](snippets/system-text-json-how-to/csharp/GradesPrettyPrint.json)]

## <a name="use-utf8jsonwriter"></a>Usar Utf8JsonWriter

O exemplo a seguir mostra como usar a <xref:System.Text.Json.Utf8JsonWriter> classe:

[!code-csharp[](snippets/system-text-json-how-to/csharp/Utf8WriterToStream.cs?name=SnippetSerialize)]

## <a name="use-utf8jsonreader"></a>Usar Utf8JsonReader

O exemplo a seguir mostra como usar a <xref:System.Text.Json.Utf8JsonReader> classe:

[!code-csharp[](snippets/system-text-json-how-to/csharp/Utf8ReaderFromBytes.cs?name=SnippetDeserialize)]

O código anterior pressupõe que a `jsonUtf8` variável é uma matriz de bytes que contém um JSON válido, codificado como UTF-8.

### <a name="filter-data-using-utf8jsonreader"></a>Filtrar dados usando Utf8JsonReader

O exemplo a seguir mostra como ler um arquivo de forma síncrona e procurar um valor:

[!code-csharp[](snippets/system-text-json-how-to/csharp/Utf8ReaderFromFile.cs)]

O código anterior:

* Assume que o JSON contém uma matriz de objetos e cada objeto pode conter uma propriedade "Name" do tipo cadeia de caracteres.
* Conta objetos e valores de propriedade "Name" que terminam com "University".
* Pressupõe que o arquivo é codificado como UTF-16 e o codifica em UTF-8. Um arquivo codificado como UTF-8 pode ser lido diretamente em um `ReadOnlySpan<byte>` , usando o seguinte código:

  ```csharp
  ReadOnlySpan<byte> jsonReadOnlySpan = File.ReadAllBytes(fileName);
  ```

  Se o arquivo contiver uma marca de ordem de byte (BOM) UTF-8, remova-a antes de passar os bytes para o `Utf8JsonReader` , uma vez que o leitor espera o texto. Caso contrário, a BOM será considerada JSON inválido e o leitor lançará uma exceção.

Aqui está um exemplo de JSON que o código anterior pode ler. A mensagem de resumo resultante é "2 de 4 têm nomes que terminam com" University "":

[!code-json[](snippets/system-text-json-how-to/csharp/Universities.json)]

### <a name="read-from-a-stream-using-utf8jsonreader"></a>Ler de um fluxo usando Utf8JsonReader

Ao ler um arquivo grande (um gigabyte ou mais, por exemplo), talvez você queira evitar ter que carregar todo o arquivo na memória de uma só vez. Para esse cenário, você pode usar um <xref:System.IO.FileStream> .

Ao usar o `Utf8JsonReader` para ler de um fluxo, as seguintes regras se aplicam:

* O buffer que contém a carga JSON parcial deve ser pelo menos tão grande quanto o maior token JSON dentro dele, de modo que o leitor possa progredir em encaminhar.
* O buffer deve ser pelo menos tão grande quanto a maior sequência de espaços em branco dentro do JSON.
* O leitor não mantém o controle dos dados lidos até que ele leia completamente o próximo <xref:System.Text.Json.Utf8JsonReader.TokenType%2A> na carga JSON. Assim, quando houver bytes restantes no buffer, você precisa passá-los para o leitor novamente. Você pode usar <xref:System.Text.Json.Utf8JsonReader.BytesConsumed%2A> para determinar quantos bytes serão deixados.

O código a seguir ilustra como ler de um fluxo. O exemplo mostra um <xref:System.IO.MemoryStream> . O código semelhante funcionará com um <xref:System.IO.FileStream> , exceto quando o `FileStream` contiver uma bom UTF-8 no início. Nesse caso, você precisa remover esses três bytes do buffer antes de passar os bytes restantes para o `Utf8JsonReader` . Caso contrário, o leitor lançaria uma exceção, uma vez que a BOM não é considerada uma parte válida do JSON.

O código de exemplo começa com um buffer de 4 KB e dobra o tamanho do buffer sempre que ele descobre que o tamanho não é grande o suficiente para se ajustar a um token JSON completo, que é necessário para que o leitor faça o progresso no conteúdo JSON. O exemplo de JSON fornecido no trecho de código dispara um aumento de tamanho de buffer somente se você definir um tamanho de buffer inicial muito pequeno, por exemplo, 10 bytes. Se você definir o tamanho do buffer inicial como 10, as `Console.WriteLine` instruções ilustrarão a causa e o efeito do tamanho do buffer aumentará. No tamanho do buffer inicial de 4 KB, o JSON de exemplo inteiro é mostrado por cada um `Console.WriteLine` , e o tamanho do buffer nunca precisa ser aumentado.

[!code-csharp[](snippets/system-text-json-how-to/csharp/Utf8ReaderPartialRead.cs)]

O exemplo anterior não define nenhum limite quanto ao tamanho do buffer pode crescer. Se o tamanho do token for muito grande, o código poderá falhar com uma <xref:System.OutOfMemoryException> exceção. Isso pode acontecer se o JSON contiver um token com cerca de 1 GB ou mais de tamanho, porque dobrar o tamanho de 1 GB resulta em um tamanho muito grande para caber em um `int32` buffer.

## <a name="additional-resources"></a>Recursos adicionais

* [System.Text.Jsonsobre](system-text-json-overview.md)
* [Como gravar conversores personalizados](system-text-json-converters-how-to.md)
* [Como migrar doNewtonsoft.Json](system-text-json-migrate-from-newtonsoft-how-to.md)
* [Suporte a DateTime e DateTimeOffset emSystem.Text.Json](../datetime/system-text-json-support.md)
* [System.Text.JsonReferência de API](xref:System.Text.Json)
<!-- * [System.Text.Json roadmap](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/roadmap/README.md)-->
