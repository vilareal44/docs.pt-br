---
title: 'Como: restaurar fusos horários de um recurso inserido'
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], deserializing
- time zones [.NET Framework], restoring
ms.assetid: 6b7b4de9-da07-47e3-8f4c-823f81798ee7
ms.openlocfilehash: b1cece13c88b3a49c9c4c90045a07dd009d4282d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281317"
---
# <a name="how-to-restore-time-zones-from-an-embedded-resource"></a>Como: restaurar fusos horários de um recurso inserido

Este tópico descreve como restaurar os fusos horários que foram salvos em um arquivo de recurso. Para obter informações e instruções sobre como salvar fusos horários, consulte [como: salvar fusos horários em um recurso inserido](save-time-zones-to-an-embedded-resource.md).

### <a name="to-deserialize-a-timezoneinfo-object-from-an-embedded-resource"></a>Para desserializar um objeto TimeZoneInfo de um recurso inserido

1. Se o fuso horário a ser recuperado não for um fuso horário personalizado, tente instanciá-lo usando o <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> método.

2. Crie uma instância de um <xref:System.Resources.ResourceManager> objeto passando o nome totalmente qualificado do arquivo de recurso inserido e uma referência para o assembly que contém o arquivo de recurso.

   Se você não puder determinar o nome totalmente qualificado do arquivo de recurso inserido, use o [ILDASM. exe (desmontador de Il)](../../framework/tools/ildasm-exe-il-disassembler.md) para examinar o manifesto do assembly. Uma `.mresource` entrada identifica o recurso. No exemplo, o nome totalmente qualificado do recurso é `SerializeTimeZoneData.SerializedTimeZones` .

   Se o arquivo de recurso for inserido no mesmo assembly que contém o código de instanciação de fuso horário, você poderá recuperar uma referência a ele chamando `static` o `Shared` método (no Visual Basic) <xref:System.Reflection.Assembly.GetExecutingAssembly%2A> .

3. Se a chamada para o <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> método falhar ou se um fuso horário personalizado for ser instanciado, recupere uma cadeia de caracteres que contenha o fuso horário serializado chamando o <xref:System.Resources.ResourceManager.GetString%2A?displayProperty=nameWithType> método.

4. Desserializar os dados de fuso horário chamando o <xref:System.TimeZoneInfo.FromSerializedString%2A> método.

## <a name="example"></a>Exemplo

O exemplo a seguir desserializa um <xref:System.TimeZoneInfo> objeto armazenado em um arquivo de recurso XML do .net inserido.

[!code-csharp[TimeZone2.Serialization#3](../../../samples/snippets/csharp/VS_Snippets_CLR/TimeZone2.Serialization/cs/SerializeTimeZoneData.cs#3)]
[!code-vb[TimeZone2.Serialization#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/TimeZone2.Serialization/vb/SerializeTimeZoneData.vb#3)]

Esse código ilustra a manipulação de exceção para garantir que um <xref:System.TimeZoneInfo> objeto exigido pelo aplicativo esteja presente. Primeiro, ele tenta criar uma instância de um <xref:System.TimeZoneInfo> objeto recuperando-o do registro usando o <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> método. Se o fuso horário não puder ser instanciado, o código o recuperará do arquivo de recurso inserido.

Como os dados de fusos horários personalizados (fusos horários instanciados usando o <xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> método) não são armazenados no registro, o código não chama o <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> para instanciar o fuso horário para Palmer, Antártica. Em vez disso, ele procura imediatamente o arquivo de recurso inserido para recuperar uma cadeia de caracteres que contém os dados do fuso horário antes de chamar o <xref:System.TimeZoneInfo.FromSerializedString%2A> método.

## <a name="compiling-the-code"></a>Compilando o código

Este exemplo requer:

- Que uma referência a System. Windows. Forms. dll e System. Core. dll seja adicionada ao projeto.

- Que os seguintes namespaces sejam importados:

  [!code-csharp[TimeZone2.Serialization#2](../../../samples/snippets/csharp/VS_Snippets_CLR/TimeZone2.Serialization/cs/SerializeTimeZoneData.cs#2)]
  [!code-vb[TimeZone2.Serialization#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/TimeZone2.Serialization/vb/SerializeTimeZoneData.vb#2)]

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
- [Visão geral do fuso horário](time-zone-overview.md)
- [Como: salvar fusos horários em um recurso inserido](save-time-zones-to-an-embedded-resource.md)
