---
title: Salvar e restaurar fusos horários
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- restoring time zones
- deserialization [.NET Framework], time zones
- serialization [.NET Framework], time zones
- time zone objects [.NET Framework], restoring
- saving time zones
- time zone objects [.NET Framework], deserializing
- time zones [.NET Framework], saving
- time zones [.NET Framework], restoring
- time zone objects [.NET Framework], serializing
- time zone objects [.NET Framework], saving
ms.assetid: 4028b310-e7ce-49d4-a646-1e83bfaf6f9d
ms.openlocfilehash: 8da26988d2e141ac704f0d3756cd8a50602cb3fd
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281044"
---
# <a name="saving-and-restoring-time-zones"></a>Salvar e restaurar fusos horários

A <xref:System.TimeZoneInfo> classe depende do registro para recuperar dados de fuso horário predefinidos. No entanto, o registro é uma estrutura dinâmica. Além disso, as informações de fuso horário contidas no registro são usadas pelo sistema operacional principalmente para lidar com ajustes de tempo e conversões para o ano atual. Isso tem duas grandes implicações para aplicativos que dependem de dados de fuso horário precisos:

- Um fuso horário exigido por um aplicativo pode não ser definido no registro ou pode ter sido renomeado ou removido do registro.

- Um fuso horário definido no registro pode não ter informações sobre as regras de ajuste específicas que são necessárias para conversões de fuso horário históricas.

A <xref:System.TimeZoneInfo> classe aborda essas limitações por meio de seu suporte para serialização (salvamento) e desserialização (restauração) de dados de fuso horário.

## <a name="time-zone-serialization-and-deserialization"></a>Serialização e desserialização de fuso horário

Salvar e restaurar um fuso horário ao serializar e desserializar dados de fuso horário envolve apenas duas chamadas de método:

- Você pode serializar um <xref:System.TimeZoneInfo> objeto chamando o método desse objeto <xref:System.TimeZoneInfo.ToSerializedString%2A> . O método não usa parâmetros e retorna uma cadeia de caracteres que contém informações de fuso horário.

- Você pode desserializar um <xref:System.TimeZoneInfo> objeto de uma cadeia de caracteres serializada passando essa cadeia de caracteres para o `static` `Shared` método (no Visual Basic) <xref:System.TimeZoneInfo.FromSerializedString%2A?displayProperty=nameWithType> .

## <a name="serialization-and-deserialization-scenarios"></a>Cenários de serialização e desserialização

A capacidade de salvar (ou serializar) um <xref:System.TimeZoneInfo> objeto em uma cadeia de caracteres e restaurá-lo (ou desserializar) para uso posterior aumenta o utilitário e a flexibilidade da <xref:System.TimeZoneInfo> classe. Esta seção examina algumas das situações em que a serialização e a desserialização são mais úteis.

### <a name="serializing-and-deserializing-time-zone-data-in-an-application"></a>Serializando e desserializando dados de fuso horário em um aplicativo

Um fuso horário serializado pode ser restaurado de uma cadeia de caracteres quando necessário. Um aplicativo pode fazer isso se o fuso horário recuperado do registro não puder converter corretamente uma data e hora dentro de um determinado intervalo de datas. Por exemplo, os dados de fuso horário no registro do Windows XP dão suporte a uma única regra de ajuste, enquanto os fusos horários definidos no registro do Windows Vista normalmente fornecem informações sobre duas regras de ajuste. Isso significa que as conversões de tempo históricas podem ser imprecisas. A serialização e a desserialização de dados de fuso horário podem lidar com essa limitação.

No exemplo a seguir, uma <xref:System.TimeZoneInfo> classe personalizada que não tem regras de ajuste é definida para representar o fuso horário padrão do leste dos EUA de 1883 a 1917, antes da introdução do horário de verão no Estados Unidos. O fuso horário personalizado é serializado em uma variável que tem escopo global. O método de conversão de fuso horário, `ConvertUtcTime` , recebe tempos de tempo universal coordenado (UTC) para converter. Se a data e a hora ocorrerem no 1917 ou anterior, o fuso horário padrão do leste personalizado será restaurado de uma cadeia de caracteres serializada e substituirá o fuso horário recuperado do registro.

[!code-csharp[System.TimeZone2.Serialization.1#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Serialization.1/cs/Serialization.cs#1)]
[!code-vb[System.TimeZone2.Serialization.1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Serialization.1/vb/Serialization.vb#1)]

### <a name="handling-time-zone-exceptions"></a>Tratamento de exceções de fuso horário

Como o registro é uma estrutura dinâmica, seu conteúdo está sujeito à modificação acidental ou deliberada. Isso significa que um fuso horário que deve ser definido no registro e que é necessário para que um aplicativo seja executado com êxito pode estar ausente. Sem o suporte para a serialização e desserialização de fuso horário, você tem pouca opção, mas para lidar com o resultado <xref:System.TimeZoneNotFoundException> , encerrando o aplicativo. No entanto, usando a serialização de fuso horário e a desserialização, você pode manipular um inesperado <xref:System.TimeZoneNotFoundException> restaurando o fuso horário necessário a partir de uma cadeia de caracteres serializada e o aplicativo continuará a ser executado.

O exemplo a seguir cria e serializa um fuso horário padrão central personalizado. Em seguida, ele tenta recuperar o fuso horário padrão central do registro. Se a operação de recuperação lançar um <xref:System.TimeZoneNotFoundException> ou um <xref:System.InvalidTimeZoneException> , o manipulador de exceção desserializará o fuso horário.

[!code-csharp[System.TimeZone2.Serialization.2#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Serialization.2/cs/Serialization2.cs#1)]
[!code-vb[System.TimeZone2.Serialization.2#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Serialization.2/vb/Serialization2.vb#1)]

### <a name="storing-a-serialized-string-and-restoring-it-when-needed"></a>Armazenando uma cadeia de caracteres serializada e restaurando-a quando necessário

Os exemplos anteriores armazenaram informações de fuso horário em uma variável de cadeia de caracteres e a restaurava quando necessário. No entanto, a cadeia de caracteres que contém informações de fuso horário serializadas pode ser armazenada em algum meio de armazenamento, como um arquivo externo, um arquivo de recurso inserido no aplicativo ou o registro. (Observe que as informações sobre fusos horários personalizados devem ser armazenadas separadas das chaves de fuso horário do sistema no registro.)

O armazenamento de uma cadeia de caracteres de fuso horário serializada dessa maneira também separa a rotina de criação de fuso horário do próprio aplicativo. Por exemplo, uma rotina de criação de fuso horário pode executar e criar um arquivo de dados que contém informações históricas de fuso horário que um aplicativo pode usar. O arquivo de dados pode ser instalado com o aplicativo e pode ser aberto e um ou mais de seus fusos horários podem ser desserializados quando o aplicativo os exige.

Para obter um exemplo que usa um recurso incorporado para armazenar dados de fuso horário serializados, consulte [como: salvar fusos horários em um recurso incorporado](save-time-zones-to-an-embedded-resource.md) e [como restaurar fusos horários de um recurso inserido](restore-time-zones-from-an-embedded-resource.md).

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
