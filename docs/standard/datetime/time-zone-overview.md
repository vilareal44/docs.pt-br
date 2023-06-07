---
title: Visão geral do fuso horário
ms.date: 04/10/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- time zones [.NET Framework], about time zones
- transition time [.NET Framework]
- TimeZoneInfo class, about TimeZoneInfo class
- time zones [.NET Framework], creating
- invalid time [.NET Framework]
- fixed rule [.NET Framework]
- ambiguous time [.NET Framework]
- floating rule [.NET Framework]
- daylight saving time [.NET Framework]
- adjustment rule [.NET Framework]
- time zones [.NET Framework], terminology
ms.assetid: c4b7ed01-5e38-4959-a3b6-ef9765d6ccf1
ms.openlocfilehash: 9cb50357931cb22fd2862ba7558154a4782e4557
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281031"
---
# <a name="time-zone-overview"></a>Visão geral do fuso horário

A <xref:System.TimeZoneInfo> classe simplifica a criação de aplicativos com reconhecimento de fuso horário. A <xref:System.TimeZone> classe dá suporte ao trabalho com o fuso horário local e UTC (tempo Universal Coordenado). A <xref:System.TimeZoneInfo> classe dá suporte a ambas as zonas, bem como a qualquer fuso horário sobre o qual as informações são predefinidas no registro. Você também pode usar <xref:System.TimeZoneInfo> o para definir fusos horários personalizados sobre os quais o sistema não tem informações.

## <a name="time-zone-essentials"></a>Fundamentos do fuso horário

Um fuso horário é uma região geográfica na qual o mesmo horário é usado. Normalmente, mas nem sempre, os fusos horários adjacentes diferem em uma hora. A hora em cada um dos fusos horários do mundo pode ser expressa como uma diferença do Tempo Universal Coordenado (UTC).

Muitos dos fusos horários do mundo permitem horário de verão. O horário de verão tenta maximizar as horas do dia com sol adiantando o horário em uma hora na primavera ou no começo do verão e retornando ao horário normal (ou padrão) no final do verão ou no outono. Essas alterações de/para o horário padrão são conhecidas como regras de ajuste.

A transição de/para o horário de verão em um determinado fuso horário pode ser definida por uma regra de ajuste fixa ou flutuante. Uma regra de ajuste fixa define uma data específica na qual a transição de/para o horário de verão ocorre todos os anos. Por exemplo, uma transição do horário de verão para o horário padrão que ocorre todos os anos em 25 de outubro segue uma regra de ajuste fixa. Muito mais comuns são as regras de ajuste flutuante, que definem um dia específico de uma semana específica de um mês específico para a transição de/para o horário de verão. Por exemplo, uma transição do horário padrão para o horário de verão que ocorre no terceiro domingo de março segue uma regra de ajuste flutuante.

Para os fusos horários que oferecem suporte às regras de ajuste, a transição de/para o horário de verão cria dois tipos de horários anômalos: horários inválidos e horários ambíguos. Um horário inválido é um horário inexistente criado pela transição do horário padrão para o horário de verão. Por exemplo, se essa transição ocorrer em um determinado dia às 2h e fizer com que o horário mude para 3h, cada intervalo de tempo entre 2h e 2h59min99 será inválido. Um horário ambíguo é aquele que pode ser mapeado para dois horários diferentes em um único fuso horário. Ele é criado pela transição do horário de verão para o horário padrão. Por exemplo, se essa transição ocorrer em um determinado dia às 2h e fizer com que o horário mude para 1h, cada intervalo de tempo entre 1h e 1h59min99 poderá ser interpretado como horário padrão ou horário de verão.

## <a name="time-zone-terminology"></a>Terminologia de fuso horário

A tabela a seguir define os termos que geralmente são usados ao trabalhar com fusos horários e ao desenvolver aplicativos com reconhecimento de fuso horário.

| Termo            | Definição |
| --------------- | ---------- |
| Regra de ajuste | Uma regra que define quando ocorre a transição do horário padrão para o horário de verão e o retorno do horário de verão para o horário padrão. Cada regra de ajuste tem uma data de início e de término que define quando a regra está em vigor (por exemplo, a regra de ajuste está em vigor de 1º de janeiro de 1986 a 31 de dezembro de 2006), um Delta (o tempo pelo qual o horário padrão é alterado como resultado do aplicativo da regra de ajuste) e informações sobre a data e hora específicas em que as transições devem ocorrer durante o período de ajuste. As transições podem seguir uma regra fixa ou uma regra flutuante. |
| Horário ambíguo  | Um horário que pode ser mapeado para dois horários diferentes em um único fuso horário. Ocorre quando o horário do relógio é atrasado, como durante a transição do horário de verão de um fuso horário para seu horário padrão. Por exemplo, se essa transição ocorrer em um determinado dia às 2h e fizer com que o horário mude para 1h, cada intervalo de tempo entre 1h e 1h59min99 poderá ser interpretado como horário padrão ou horário de verão. |
| Regra fixa      | Uma regra de ajuste que define uma data específica para a transição de/para o horário de verão. Por exemplo, uma transição do horário de verão para o horário padrão que ocorre todos os anos em 25 de outubro segue uma regra de ajuste fixa. |
| Regra flutuante   | Uma regra de ajuste que define um dia específico de uma semana específica de um mês específico para a transição de/para o horário de verão. Por exemplo, uma transição do horário padrão para o horário de verão que ocorre no terceiro domingo de março segue uma regra de ajuste flutuante. |
| Horário inválido    | Um horário inexistente que é um artefato da transição do horário padrão para o horário de verão. Ocorre quando o horário do relógio é adiantado, como durante a transição do horário padrão de um fuso horário para seu horário de verão. Por exemplo, se essa transição ocorrer em um determinado dia às 2h e fizer com que o horário mude para 3h, cada intervalo de tempo entre 2h e 2h59min99 será inválido. |
| Horário de transição | Informações sobre uma mudança de horário específica, como a mudança do horário de verão para o horário padrão ou vice-versa, em um determinado fuso horário. |

## <a name="time-zones-and-the-timezoneinfo-class"></a>Fusos horários e a classe TimeZoneInfo

No .NET, um <xref:System.TimeZoneInfo> objeto representa um fuso horário. A <xref:System.TimeZoneInfo> classe inclui um <xref:System.TimeZoneInfo.GetAdjustmentRules%2A> método que retorna uma matriz de <xref:System.TimeZoneInfo.AdjustmentRule> objetos. Cada elemento dessa matriz fornece informações sobre a transição de e para o horário de verão para um período de tempo específico. (Para fusos horários que não dão suporte ao horário de verão, o método retorna uma matriz vazia.) Cada <xref:System.TimeZoneInfo.AdjustmentRule> objeto tem um <xref:System.TimeZoneInfo.AdjustmentRule.DaylightTransitionStart%2A> e uma <xref:System.TimeZoneInfo.AdjustmentRule.DaylightTransitionEnd%2A> propriedade que define a data e a hora específicas da transição de e para o horário de verão. A <xref:System.TimeZoneInfo.TransitionTime.IsFixedDateRule%2A> propriedade indica se essa transição é fixa ou flutuante.

O .NET depende das informações de fuso horário fornecidas pelo sistema operacional Windows e armazenadas no registro. Devido ao número de fusos horários da terra, nem todos os fusos horários existentes são representados no registro. Além disso, como o registro é uma estrutura dinâmica, os fusos horários predefinidos podem ser adicionados ou removidos dele. Por fim, o registro não contém necessariamente dados de fuso horário históricos. Por exemplo, no Windows XP, o registro contém dados sobre apenas um único conjunto de ajustes de fuso horário. O Windows Vista dá suporte a dados dinâmicos de fuso horário, o que significa que um único fuso horário pode ter várias regras de ajuste que se aplicam a intervalos específicos de anos. No entanto, a maioria dos fusos horários definidos no registro do Windows Vista e dá suporte ao horário de verão tem apenas uma ou duas regras de ajuste predefinidas.

A dependência da <xref:System.TimeZoneInfo> classe no registro significa que um aplicativo com reconhecimento de fuso horário não pode ter certeza de que um determinado fuso horário está definido no registro. Como resultado, a tentativa de criar uma instância de um fuso horário específico (que não seja o fuso horário local ou o fuso horário que representa o UTC) deve usar tratamento de exceção. Ele também deve fornecer algum método para permitir que o aplicativo continue se um <xref:System.TimeZoneInfo> objeto necessário não puder ser instanciado do registro.

Para lidar com a ausência de um fuso horário necessário, a <xref:System.TimeZoneInfo> classe inclui um <xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> método, que pode ser usado para criar fusos horários personalizados que não são encontrados no registro. Para obter detalhes sobre como criar um fuso horário personalizado, consulte [como: criar fusos horários sem regras de ajuste](create-time-zones-without-adjustment-rules.md) e [como criar fusos horários com regras de ajuste](create-time-zones-with-adjustment-rules.md). Além disso, você pode usar o <xref:System.TimeZoneInfo.ToSerializedString%2A> método para converter um fuso horário recém-criado em uma cadeia de caracteres e salvá-lo em um repositório de dados (como um banco de dado, um arquivo de texto, o registro ou um recurso de aplicativo). Em seguida, você pode usar o <xref:System.TimeZoneInfo.FromSerializedString%2A> método para converter essa cadeia de caracteres de volta em um <xref:System.TimeZoneInfo> objeto. Para obter detalhes, consulte [como: salvar fusos horários em um recurso incorporado](save-time-zones-to-an-embedded-resource.md) e [como restaurar fusos horários de um recurso inserido](restore-time-zones-from-an-embedded-resource.md).

Como cada fuso horário é caracterizado por uma diferença base do UTC, bem como por uma diferença do UTC que reflete as regras de ajuste existentes, um horário em um fuso horário pode ser facilmente convertido no horário em outro fuso horário. Para essa finalidade, o <xref:System.TimeZoneInfo> objeto inclui vários métodos de conversão, incluindo:

- <xref:System.TimeZoneInfo.ConvertTimeFromUtc%2A>, que converte o UTC para a hora em um fuso horário designado.

- <xref:System.TimeZoneInfo.ConvertTimeToUtc%2A>, que converte a hora em um fuso horário designado para UTC.

- <xref:System.TimeZoneInfo.ConvertTime%2A>, que converte a hora em uma zona de tempo designada para a hora em outro fuso horário designado.

- <xref:System.TimeZoneInfo.ConvertTimeBySystemTimeZoneId%2A>, que usa identificadores de fuso horário (em vez de <xref:System.TimeZoneInfo> objetos) como parâmetros para converter a hora em uma zona de tempo designada para a hora em outro fuso horário designado.

Para obter detalhes de como converter horários entre fusos horários, consulte [Convertendo horários entre fusos horários](converting-between-time-zones.md).

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
