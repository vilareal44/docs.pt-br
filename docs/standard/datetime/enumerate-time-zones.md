---
title: 'Como: enumerar fusos horários presentes em um computador'
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], enumerating
- enumerating time zones [.NET Framework]
ms.assetid: bb7a42ab-6bd9-4c5c-b734-5546d51f8669
ms.openlocfilehash: 8f1cc9d58bc0f169d458854eac6568caaa4481c7
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286126"
---
# <a name="how-to-enumerate-time-zones-present-on-a-computer"></a>Como: enumerar fusos horários presentes em um computador

Trabalhar com êxito com um fuso horário designado requer que informações sobre o fuso horário em questão estejam disponíveis no sistema. Os sistemas operacionais Windows XP e Windows Vista armazenam essas informações no registro. Embora o número total de fusos horários existentes seja grande, o Registro contém informações apenas sobre um subconjunto deles. Além disso, o Registro em si é uma estrutura dinâmica cujo conteúdo está sujeito a alterações deliberadas ou acidentais. Como resultado, um aplicativo nem sempre pode presumir que um determinado fuso horário esteja definido e disponível no sistema. A primeira etapa para muitos aplicativos que usam informações de fuso horário é determinar se os fusos horários necessários estão disponíveis no sistema local ou dar ao usuário uma lista dos fusos horários entre os quais escolher. Isso requer que o aplicativo enumere os fusos horários definidos no sistema local.

> [!NOTE]
> Se um aplicativo depender da presença de um fuso horário específico que não possa ser definido em um sistema local, o aplicativo poderá garantir sua presença serializando e desserializando as informações sobre o fuso horário. O fuso horário pode então ser adicionado a um controle de lista para que o usuário do aplicativo possa selecioná-lo. Para obter detalhes, consulte [como: salvar fusos horários em um recurso incorporado](save-time-zones-to-an-embedded-resource.md) e [como restaurar fusos horários de um recurso inserido](restore-time-zones-from-an-embedded-resource.md).

### <a name="to-enumerate-the-time-zones-present-on-the-local-system"></a>Para enumerar os fusos horários presentes no sistema local

1. Chame o método <xref:System.TimeZoneInfo.GetSystemTimeZones%2A?displayProperty=nameWithType> . O método retorna uma <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> coleção genérica de <xref:System.TimeZoneInfo> objetos. As entradas na coleção são classificadas por sua <xref:System.TimeZoneInfo.DisplayName%2A> propriedade. Por exemplo:

   [!code-csharp[System.TimeZone2.Concepts#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#1)]
   [!code-vb[System.TimeZone2.Concepts#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#1)]

2. Enumere os <xref:System.TimeZoneInfo> objetos individuais na coleção usando um `foreach` loop (em C#) ou um `For Each` ...`Next` loop (em Visual Basic) e execute qualquer processamento necessário em cada objeto. Por exemplo, o código a seguir enumera a <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> coleção de <xref:System.TimeZoneInfo> objetos retornados na etapa 1 e lista o nome de exibição de cada fuso horário no console.

   [!code-csharp[System.TimeZone2.Concepts#12](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#12)]
   [!code-vb[System.TimeZone2.Concepts#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#12)]

### <a name="to-present-the-user-with-a-list-of-time-zones-present-on-the-local-system"></a>Para apresentar ao usuário uma lista de fusos horários presentes no sistema local

1. Chame o método <xref:System.TimeZoneInfo.GetSystemTimeZones%2A?displayProperty=nameWithType> . O método retorna uma <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> coleção genérica de <xref:System.TimeZoneInfo> objetos.

2. Atribua a coleção retornada na etapa 1 à `DataSource` propriedade de um controle de lista do Windows Forms ou ASP.net.

3. Recupere o <xref:System.TimeZoneInfo> objeto que o usuário selecionou.

O exemplo fornece uma ilustração para um aplicativo do Windows.

## <a name="example"></a>Exemplo

O exemplo inicia um aplicativo do Windows que exibe os fusos horários definidos em um sistema em uma caixa de listagem. Em seguida, o exemplo exibe uma caixa de diálogo que contém o valor da <xref:System.TimeZoneInfo.DisplayName%2A> Propriedade do objeto de fuso horário selecionado pelo usuário.

[!code-csharp[System.TimeZone2.Concepts#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#2)]
[!code-vb[System.TimeZone2.Concepts#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#2)]

A maioria dos controles de lista (como o <xref:System.Windows.Forms.ListBox?displayProperty=nameWithType> <xref:System.Web.UI.WebControls.BulletedList?displayProperty=nameWithType> controle ou) permite que você atribua uma coleção de variáveis de objeto à sua `DataSource` propriedade, desde que essa coleção implemente a <xref:System.Collections.IEnumerable> interface. (A <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> classe genérica faz isso.) Para exibir um objeto individual na coleção, o controle chama o método desse objeto `ToString` para extrair a cadeia de caracteres usada para representar o objeto. No caso de <xref:System.TimeZoneInfo> objetos, o `ToString` método retorna o <xref:System.TimeZoneInfo> nome de exibição do objeto (o valor de sua <xref:System.TimeZoneInfo.DisplayName%2A> Propriedade).

> [!NOTE]
> Como os controles de lista chamam o método de um objeto `ToString` , você pode atribuir uma coleção de <xref:System.TimeZoneInfo> objetos ao controle, fazer com que o controle exiba um nome significativo para cada objeto e recuperar o <xref:System.TimeZoneInfo> objeto que o usuário selecionou. Isso elimina a necessidade de extrair uma cadeia de caracteres para cada objeto na coleção, atribuir a cadeia de caracteres a uma coleção que, por sua vez, é atribuída à propriedade do controle `DataSource` , recuperar a cadeia de caracteres que o usuário selecionou e, em seguida, usar essa cadeia de caracteres para extrair o objeto que ele descreve.

## <a name="compiling-the-code"></a>Compilando o código

Este exemplo requer:

- Que os seguintes namespaces sejam importados:

  <xref:System>(em código C#)

  <xref:System.Collections.ObjectModel>

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
- [Como: salvar fusos horários em um recurso inserido](save-time-zones-to-an-embedded-resource.md)
- [Como: restaurar fusos horários de um recurso inserido](restore-time-zones-from-an-embedded-resource.md)
