---
title: 'Como: criar fusos horários com regras de ajuste'
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], creating
- time zones [.NET Framework], and adjustment rules
- adjustment rule [.NET Framework]
ms.assetid: c52ef192-13a9-435f-8015-3b12eae8c47c
ms.openlocfilehash: b7e938581dfde3f1566aa2506302292686c2fc5c
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84278162"
---
# <a name="how-to-create-time-zones-with-adjustment-rules"></a>Como: criar fusos horários com regras de ajuste

As informações precisas de fuso horário exigidas por um aplicativo podem não estar presentes em um sistema específico por vários motivos:

- O fuso horário nunca foi definido no registro do sistema local.

- Os dados sobre o fuso horário foram modificados ou removidos do registro.

- O fuso horário não tem informações precisas sobre os ajustes de fuso horário de um período histórico específico.

Nesses casos, você pode chamar o <xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> método para definir o fuso horário exigido pelo seu aplicativo. Você pode usar as sobrecargas desse método para criar um fuso horário com ou sem regras de ajuste. Se o fuso horário der suporte ao horário de verão, você poderá definir ajustes com regras de ajuste fixos ou flutuantes. (Para obter as definições desses termos, consulte a seção "terminologia de fuso horário" em [visão geral de fuso horário](time-zone-overview.md).)

> [!IMPORTANT]
> Os fusos horários personalizados criados chamando o <xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> método não são adicionados ao registro. Em vez disso, eles podem ser acessados somente por meio da referência de objeto retornada pela <xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> chamada de método.

Este tópico mostra como criar um fuso horário com regras de ajuste. Para criar um fuso horário que não ofereça suporte às regras de ajuste de horário de verão, consulte [como: criar fusos horários sem regras de ajuste](create-time-zones-without-adjustment-rules.md).

### <a name="to-create-a-time-zone-with-floating-adjustment-rules"></a>Para criar um fuso horário com regras de ajuste flutuantes

1. Para cada ajuste (ou seja, para cada transição de distância de e para o horário padrão em um intervalo de tempo específico), faça o seguinte:

    1. Defina o tempo de transição inicial para o ajuste de fuso horário.

       Você deve chamar o <xref:System.TimeZoneInfo.TransitionTime.CreateFloatingDateRule%2A?displayProperty=nameWithType> método e passá-lo para um <xref:System.DateTime> valor que define a hora da transição, um valor inteiro que define o mês da transição, um valor inteiro que define a semana na qual a transição ocorre e um <xref:System.DayOfWeek> valor que define o dia da semana em que ocorre a transição. Essa chamada de método instancia um <xref:System.TimeZoneInfo.TransitionTime> objeto.

    2. Defina o tempo de transição final para o ajuste de fuso horário. Isso requer outra chamada para o <xref:System.TimeZoneInfo.TransitionTime.CreateFloatingDateRule%2A?displayProperty=nameWithType> método. Essa chamada de método instancia um segundo <xref:System.TimeZoneInfo.TransitionTime> objeto.

    3. Chame o <xref:System.TimeZoneInfo.AdjustmentRule.CreateAdjustmentRule%2A> método e passe-o para as datas de início e término efetivas do ajuste, um <xref:System.TimeSpan> objeto que define a quantidade de tempo na transição e os dois <xref:System.TimeZoneInfo.TransitionTime> objetos que definem quando ocorre a transição de e para o horário de verão. Essa chamada de método instancia um <xref:System.TimeZoneInfo.AdjustmentRule> objeto.

    4. Atribua o <xref:System.TimeZoneInfo.AdjustmentRule> objeto a uma matriz de <xref:System.TimeZoneInfo.AdjustmentRule> objetos.

2. Defina o nome de exibição do fuso horário. O nome de exibição segue um formato bastante padrão no qual o deslocamento do fuso horário do tempo universal coordenado (UTC) é colocado entre parênteses e é seguido por uma cadeia de caracteres que identifica o fuso horário, uma ou mais cidades no fuso horário ou um ou mais países ou regiões no fuso horário.

3. Defina o nome do horário padrão do fuso horário. Normalmente, essa cadeia de caracteres também é usada como o identificador do fuso horário.

4. Defina o nome do horário de Verão do fuso horário.

5. Se você quiser usar um identificador diferente do nome padrão do fuso horário, defina o identificador de fuso horário.

6. Crie uma instância de um <xref:System.TimeSpan> objeto que define o deslocamento do fuso horário do UTC. Fusos horários com horas posteriores a UTC têm um deslocamento positivo. Fusos horários com horários que são anteriores ao UTC têm um deslocamento negativo.

7. Chame o <xref:System.TimeZoneInfo.CreateCustomTimeZone%28System.String%2CSystem.TimeSpan%2CSystem.String%2CSystem.String%2CSystem.String%2CSystem.TimeZoneInfo.AdjustmentRule%5B%5D%29?displayProperty=nameWithType> método para instanciar o novo fuso horário.

## <a name="example"></a>Exemplo

O exemplo a seguir define um fuso horário padrão central para o Estados Unidos que inclui regras de ajuste para uma variedade de intervalos de tempo de 1918 para o presente.

[!code-csharp[System.TimeZone2.CreateTimeZone#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/cs/System.TimeZone2.CreateTimeZone.cs#5)]
[!code-vb[System.TimeZone2.CreateTimeZone#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/vb/System.TimeZone2.CreateTimeZone.vb#5)]

O fuso horário criado neste exemplo tem várias regras de ajuste. Deve-se ter cuidado para garantir que as datas de início e término efetivas de qualquer regra de ajuste não se sobreponham às datas de outra regra de ajuste. Se houver uma sobreposição, um <xref:System.InvalidTimeZoneException> será lançado.

Para regras de ajuste flutuantes, o valor 5 é passado para o `week` parâmetro do <xref:System.TimeZoneInfo.TransitionTime.CreateFloatingDateRule%2A> método para indicar que a transição ocorre na última semana de um mês específico.

Ao criar a matriz de <xref:System.TimeZoneInfo.AdjustmentRule> objetos a ser usada na <xref:System.TimeZoneInfo.CreateCustomTimeZone%28System.String%2CSystem.TimeSpan%2CSystem.String%2CSystem.String%2CSystem.String%2CSystem.TimeZoneInfo.AdjustmentRule%5B%5D%29?displayProperty=nameWithType> chamada do método, o código pode inicializar a matriz para o tamanho exigido pelo número de ajustes a serem criados para o fuso horário. Em vez disso, este exemplo de código chama o <xref:System.Collections.Generic.List%601.Add%2A> método para adicionar cada regra de ajuste a uma <xref:System.Collections.Generic.List%601> coleção genérica de <xref:System.TimeZoneInfo.AdjustmentRule> objetos. Em seguida, o código chama o <xref:System.Collections.Generic.List%601.CopyTo%2A> método para copiar os membros dessa coleção para a matriz.

O exemplo também usa o <xref:System.TimeZoneInfo.TransitionTime.CreateFixedDateRule%2A> método para definir ajustes de data fixas. Isso é semelhante à chamada do <xref:System.TimeZoneInfo.TransitionTime.CreateFloatingDateRule%2A> método, exceto pelo fato de que ele requer apenas a hora, o mês e o dia dos parâmetros de transição.

O exemplo pode ser testado usando um código como o seguinte:

[!code-csharp[System.TimeZone2.CreateTimeZone#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/cs/System.TimeZone2.CreateTimeZone.cs#7)]
[!code-vb[System.TimeZone2.CreateTimeZone#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/vb/System.TimeZone2.CreateTimeZone.vb#7)]

## <a name="compiling-the-code"></a>Compilando o código

Este exemplo requer:

- Que os seguintes namespaces sejam importados:

  [!code-csharp[System.TimeZone2.CreateTimeZone#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/cs/System.TimeZone2.CreateTimeZone.cs#6)]
  [!code-vb[System.TimeZone2.CreateTimeZone#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.CreateTimeZone/vb/System.TimeZone2.CreateTimeZone.vb#6)]

## <a name="see-also"></a>Veja também

- [Datas, horas e fusos horários](index.md)
- [Visão geral do fuso horário](time-zone-overview.md)
- [Como: criar fusos horários sem regras de ajuste](create-time-zones-without-adjustment-rules.md)
