---
title: 'Como: definir e usar provedores de formatos numéricos personalizados'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- numeric format strings [.NET Framework]
- formatting [.NET Framework], numbers
- number formatting [.NET Framework]
- custom numeric format strings
- numbers [.NET Framework], custom numeric format strings
- displaying date and time data
- format providers [.NET Framework]
- custom format strings
ms.assetid: a281bfbf-6596-45ed-a2d6-3782d535ada2
ms.openlocfilehash: d12899fff7d9e6cb63728ba0b160b70fa2a41a1a
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290507"
---
# <a name="how-to-define-and-use-custom-numeric-format-providers"></a>Como: definir e usar provedores de formatos numéricos personalizados
O .NET Framework oferece controle abrangente sobre a representação de cadeias de caracteres de valores numéricos. Ele dá suporte aos seguintes recursos para personalizar o formato de valores numéricos:  
  
- Cadeias de caracteres de formato numérico padrão, que fornecem um conjunto predefinido de formatos para converter números para suas representações de cadeia de caracteres. Você pode usá-las com qualquer método de formatação numérica, como <xref:System.Decimal.ToString%28System.String%29?displayProperty=nameWithType>, que tem um parâmetro `format`. Para obter detalhes, confira [Cadeias de caracteres de formato numérico padrão](standard-numeric-format-strings.md).  
  
- Cadeias de caracteres de formato numérico personalizado, que fornecem um conjunto de símbolos que podem ser combinados para definir especificadores de formato numérico personalizado. Elas também podem ser usadas com qualquer método de formatação numérica, como <xref:System.Decimal.ToString%28System.String%29?displayProperty=nameWithType>, que tem um parâmetro `format`. Para ver detalhes, confira [Cadeias de caracteres de formato numérico personalizado](custom-numeric-format-strings.md).  
  
- Os objetos <xref:System.Globalization.CultureInfo> ou <xref:System.Globalization.NumberFormatInfo> personalizados que definem os símbolos e padrões de formato usados para exibir as representações de cadeia de caracteres de valores numéricos. Você pode usá-las com qualquer método de formatação numérica, como <xref:System.Int32.ToString%2A>, que tem um parâmetro `provider`. Normalmente, o parâmetro `provider` é usado para especificar a formatação específica à cultura.  
  
 Em alguns casos (como quando um aplicativo deve exibir um número de conta formatado, um número de identificação ou um código postal) essas três técnicas são inadequadas. O .NET Framework também permite que você defina um objeto de formatação que não é um objeto <xref:System.Globalization.CultureInfo> ou <xref:System.Globalization.NumberFormatInfo> para determinar como um valor numérico é formatado. Este tópico fornece informações passo a passo para implementar esse tipo de objeto, bem como um exemplo que formata números de telefone.  
  
### <a name="to-define-a-custom-format-provider"></a>Para definir um provedor de formato personalizado  
  
1. Defina uma classe que implementa as interfaces <xref:System.IFormatProvider> e <xref:System.ICustomFormatter>.  
  
2. Implementar o método de <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> . <xref:System.IFormatProvider.GetFormat%2A> é um método de retorno de chamada que o método de formatação (como o método <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>) invoca para recuperar o objeto que é o responsável por executar a formatação personalizada. Uma implementação típica de <xref:System.IFormatProvider.GetFormat%2A> faz o seguinte:  
  
    1. Determina se o objeto <xref:System.Type> passado como método de parâmetro representa uma interface <xref:System.ICustomFormatter>.  
  
    2. Se o parâmetro representar a interface <xref:System.ICustomFormatter>, <xref:System.IFormatProvider.GetFormat%2A> retorna um objeto que implementa a interface <xref:System.ICustomFormatter> que é responsável por fornecer a formatação personalizada. Normalmente, o objeto de formatação personalizado retorna a si próprio.  
  
    3. Se o parâmetro não representar a interface<xref:System.ICustomFormatter>, <xref:System.IFormatProvider.GetFormat%2A> retorna `null`.  
  
3. Implementar o método de <xref:System.ICustomFormatter.Format%2A> . Este método é chamado pelo método <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> e é responsável por retornar a representação de cadeia de caracteres de um número. Implementar o método normalmente envolve o seguinte:  
  
    1. Opcionalmente, verifique se o método é legitimamente destinado a fornecer serviços de formatação examinando o parâmetro `provider`. Para formatar objetos que implementam <xref:System.IFormatProvider> e <xref:System.ICustomFormatter>, isso envolve testar o parâmetro `provider` para igualdade com o objeto de formatação atual.  
  
    2. Determine se o objeto de formatação deve dar suporte a especificadores de formato personalizado. (Por exemplo, um especificador de formato "N" pode indicar que um número de telefone dos EUA deve ser de saída no formato NANP e um "I" pode indicar a saída no formato de recomendação E. 123 do ITU-T.) Se os especificadores de formato forem usados, o método deve lidar com o especificador de formato específico. Ele é passado para o método no parâmetro `format`. Se nenhum especificador estiver presente, o valor do parâmetro `format` será <xref:System.String.Empty?displayProperty=nameWithType>.  
  
    3. Recupere o valor numérico passado para o método como o parâmetro `arg`. Execute as manipulações que forem necessárias para convertê-lo em sua representação de cadeia de caracteres.  
  
    4. Retorne a representação de cadeia de caracteres do parâmetro `arg`.  
  
### <a name="to-use-a-custom-numeric-formatting-object"></a>Para usar um objeto de formatação numérico personalizado  
  
1. Crie uma nova instância da classe de formatação personalizada.  
  
2. Chame o método de formatação <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> e passe para ele o objeto de formatação personalizado, o especificador de formatação (ou <xref:System.String.Empty?displayProperty=nameWithType>, se um não for usado) e o valor numérico a ser formatado.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir define um provedor de formato numérico personalizado chamado `TelephoneFormatter`, que converte um número que representa um número de telefone dos EUA para seu formato NANP ou E.123. O método lida com dois especificadores de formato, "N" (que gera o formato NANP) e "I" (que gera o formato internacional E.123).  
  
 [!code-csharp[Formatting.HowTo.NumericValue#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.NumericValue/cs/Telephone1.cs#1)]
 [!code-vb[Formatting.HowTo.NumericValue#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.NumericValue/vb/Telephone1.vb#1)]  
  
 O provedor de formato numérico personalizado pode ser usado apenas com o método <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>. As outras sobrecargas de métodos de formatação numérica (como `ToString`) que têm um parâmetro do tipo <xref:System.IFormatProvider> passam para a implementação <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> um objeto <xref:System.Type> que representa o tipo <xref:System.Globalization.NumberFormatInfo>. Por sua vez, eles esperam que o método retorne um objeto <xref:System.Globalization.NumberFormatInfo>. Se isso não acontecer, o provedor de formato numérico personalizado será ignorado e o objeto <xref:System.Globalization.NumberFormatInfo> da cultura atual será usada em seu lugar. No exemplo, o método `TelephoneFormatter.GetFormat` trata da possibilidade de ele ser passado inadequadamente para um método de formatação numérico examinando o parâmetro do método e retornando `null` se ele representar um tipo diferente de <xref:System.ICustomFormatter>.  
  
 Se um provedor de formato numérico personalizado der suporte a um conjunto de especificadores de formato, forneça um comportamento padrão se nenhum especificador de formato for fornecido no item de formato usado na chamada de método <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>. No exemplo, "N" é o especificador de formato padrão. Isso permite que um número a ser convertido em um número de telefone formatado, fornecendo um especificador de formato explícito. O exemplo a seguir ilustra essa chamada de método.  
  
 [!code-csharp[Formatting.HowTo.NumericValue#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.NumericValue/cs/Telephone1.cs#2)]
 [!code-vb[Formatting.HowTo.NumericValue#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.NumericValue/vb/Telephone1.vb#2)]  
  
 Mas também permite que a conversão ocorra se nenhum especificador de formato estiver presente. O exemplo a seguir ilustra essa chamada de método.  
  
 [!code-csharp[Formatting.HowTo.NumericValue#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.NumericValue/cs/Telephone1.cs#3)]
 [!code-vb[Formatting.HowTo.NumericValue#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.NumericValue/vb/Telephone1.vb#3)]  
  
 Se nenhum especificador de formato padrão for definido, sua implementação do método <xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> deve incluir código como o seguinte para que o .NET possa fornecer a formatação a que seu código não dá suporte.  
  
 [!code-csharp[System.ICustomFormatter.Format#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.ICustomFormatter.Format/cs/format.cs#1)]
 [!code-vb[System.ICustomFormatter.Format#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.ICustomFormatter.Format/vb/Format.vb#1)]  
  
 No caso deste exemplo, o método que implementa <xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> se destina a servir como um método de retorno de chamada para o método <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>. Portanto, ele examina o parâmetro `formatProvider` para determinar se ele contém uma referência ao objeto `TelephoneFormatter` atual. No entanto, o método também pode ser chamado diretamente do código. Nesse caso, você pode usar o parâmetro `formatProvider` para fornecer um objeto <xref:System.Globalization.CultureInfo> ou <xref:System.Globalization.NumberFormatInfo> que fornece informações de formatação específicas da cultura.
