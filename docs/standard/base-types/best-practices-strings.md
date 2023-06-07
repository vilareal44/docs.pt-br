---
title: Práticas recomendadas para o uso de cadeias de caracteres no .NET
description: Saiba como usar cadeias de caracteres com eficiência em aplicativos .NET.
ms.date: 05/01/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- strings [.NET Framework],searching
- best practices,string comparison and sorting
- strings [.NET Framework],best practices
- strings [.NET Framework],basic string operations
- sorting strings
- strings [.NET Framework],sorting
- string comparison [.NET Framework],best practices
- string sorting
- comparing strings
- strings [.NET Framework],comparing
ms.assetid: b9f0bf53-e2de-4116-8ce9-d4f91a1df4f7
ms.openlocfilehash: 28c1397c71debeed181acb2c1acb01b0f8cee7c9
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289359"
---
# <a name="best-practices-for-using-strings-in-net"></a>Práticas recomendadas para o uso de cadeias de caracteres no .NET

O .NET dá um amplo suporte para desenvolvimento de aplicativos localizados e globalizados e torna mais fácil aplicar as convenções da cultura atual ou de uma cultura específica ao executar operações comuns, como a classificação e a exibição cadeias de caracteres. Mas a classificação ou a comparação de cadeias de caracteres nem sempre é uma operação que leva em conta a cultura. Por exemplo, cadeias de caracteres que são usadas internamente por um aplicativo normalmente devem ser manipuladas de maneira idêntica em todas as culturas. Quando os dados de cadeias de caracteres culturalmente independentes, como marcações XML, marcações HTML, nomes de usuário, caminhos de arquivo e nomes de objetos do sistema, são interpretados como se levassem em conta a cultura, o código do aplicativo pode estar sujeito a bugs sutis, desempenho ruim e, em alguns casos, problemas de segurança.

Este tópico examina os métodos de classificação, comparação e o uso de maiúsculas e minúsculas de cadeias de caracteres no .NET, apresenta recomendações para a seleção de um método de manipulação de cadeia de caracteres adequado e fornece informações adicionais sobre os métodos de manipulação de cadeia de caracteres. Ela também examina como os dados formatados, como dados numéricos e dados de data e hora, são manipulados para exibição e armazenamento.

## <a name="recommendations-for-string-usage"></a>Recomendações para uso da cadeia de caracteres

Ao desenvolver com o .NET, siga estas recomendações simples quando usar cadeias de caracteres:

- Use sobrecargas que especificam explicitamente as regras de comparação de cadeias de caracteres para operações de cadeia de caracteres. Normalmente, isso envolve chamar uma sobrecarga de método que tem um parâmetro do tipo <xref:System.StringComparison>.
- Use <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> ou <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> para comparações como segurança padrão para correspondência de cadeia de caracteres independente de cultura.
- Use as comparações com <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> ou <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> para melhorar o desempenho.
- Usar operações de cadeia de caracteres com base em <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType> quando você exibir a saída para o usuário.
- Use os valores não linguísticos <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> ou <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> em vez de operações de cadeia de caracteres com base em <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> quando a comparação for irrelevante linguisticamente (simbólica, por exemplo).
- Use o método <xref:System.String.ToUpperInvariant%2A?displayProperty=nameWithType> em vez do método <xref:System.String.ToLowerInvariant%2A?displayProperty=nameWithType> ao normalizar cadeias de caracteres para comparação.
- Use uma sobrecarga do método <xref:System.String.Equals%2A?displayProperty=nameWithType> para testar se duas cadeias de caracteres são iguais.
- Use os métodos <xref:System.String.Compare%2A?displayProperty=nameWithType> e <xref:System.String.CompareTo%2A?displayProperty=nameWithType> para classificar cadeias de caracteres, não para verificar a igualdade.
- Use a formatação que leva em conta a cultura para exibir dados que não são de cadeias de caracteres, como números e datas, em uma interface do usuário. Use a formatação com a [cultura invariável](xref:System.Globalization.CultureInfo.InvariantCulture) para persistir os dados que não são de cadeias de caracteres no formato de cadeia de caracteres.

Evite as práticas a seguir ao usar cadeias de caracteres:

- Não use sobrecargas que não especificam explicita ou implicitamente as regras de comparação de cadeias de caracteres para operações de cadeia de caracteres.
- Não use operações de cadeia de caracteres com base em <xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType> na maioria dos casos. Uma das poucas exceções é quando você persiste dados de forma linguisticamente significativa, mas independente de cultura.
- Não use uma sobrecarga dos métodos <xref:System.String.Compare%2A?displayProperty=nameWithType> ou <xref:System.String.CompareTo%2A> e teste para um valor retornado de zero para determinar se duas cadeias de caracteres são iguais.
- Não use a formatação que leva em conta a cultura para persistir dados numéricos ou dados de data e hora no formato de cadeia de caracteres.

## <a name="specifying-string-comparisons-explicitly"></a>Especificação explícita de comparações da cadeia de caracteres

A maioria dos métodos de manipulação de cadeia de caracteres no .NET é sobrecarregada. Normalmente, uma ou mais sobrecargas aceitam as configurações padrão, enquanto outras não aceitam nenhum padrão e definem a maneira exata em que as cadeias de caracteres devem ser comparadas ou manipuladas. A maioria dos métodos que não dependem de padrões inclui um parâmetro do tipo <xref:System.StringComparison>, que é uma enumeração que especifica explicitamente as regras de comparação de cadeia de caracteres por cultura e maiúsculas e minúsculas. A tabela a seguir descreve os membros de enumeração <xref:System.StringComparison>.

|Membro de StringComparison|Description|
|-----------------------------|-----------------|
|<xref:System.StringComparison.CurrentCulture>|Executa uma comparação que diferencia maiúsculas de minúsculas usando a cultura atual.|
|<xref:System.StringComparison.CurrentCultureIgnoreCase>|Executa uma comparação que não diferencia maiúsculas de minúsculas usando a cultura atual.|
|<xref:System.StringComparison.InvariantCulture>|Executa uma comparação que diferencia maiúsculas de minúsculas usando a cultura invariável.|
|<xref:System.StringComparison.InvariantCultureIgnoreCase>|Executa uma comparação que não diferencia maiúsculas de minúsculas usando a cultura invariável.|
|<xref:System.StringComparison.Ordinal>|Executa uma comparação ordinal.|
|<xref:System.StringComparison.OrdinalIgnoreCase>|Executa uma comparação ordinal que não diferencia maiúsculas de minúsculas.|

Por exemplo, o método <xref:System.String.IndexOf%2A>, que retorna um índice de uma subcadeia de caracteres em um objeto <xref:System.String> que corresponde a um caractere ou cadeia de caracteres, tem nove sobrecargas:

- <xref:System.String.IndexOf%28System.Char%29>, <xref:System.String.IndexOf%28System.Char%2CSystem.Int32%29> e <xref:System.String.IndexOf%28System.Char%2CSystem.Int32%2CSystem.Int32%29>, que, por padrão, realiza, uma pesquisa de ordinal (diferencia maiúsculas de minúsculas e não diferencia a cultura) para um caractere na cadeia de caracteres.
- <xref:System.String.IndexOf%28System.String%29>, <xref:System.String.IndexOf%28System.String%2CSystem.Int32%29> e <xref:System.String.IndexOf%28System.String%2CSystem.Int32%2CSystem.Int32%29>, que, por padrão, realizam uma pesquisa que diferencia maiúsculas de minúsculas e cultura de uma subcadeia de caracteres na cadeia de caracteres.
- <xref:System.String.IndexOf%28System.String%2CSystem.StringComparison%29>, <xref:System.String.IndexOf%28System.String%2CSystem.Int32%2CSystem.StringComparison%29> e <xref:System.String.IndexOf%28System.String%2CSystem.Int32%2CSystem.Int32%2CSystem.StringComparison%29>, que incluem um parâmetro de tipo <xref:System.StringComparison> que permite que o formato da comparação seja especificado.

É recomendável que você selecione uma sobrecarga que não usa valores padrão, pelos seguintes motivos:

- Algumas sobrecargas com parâmetros padrão (aqueles que pesquisam um <xref:System.Char> na instância de cadeia de caracteres) realizam uma comparação ordinal, enquanto outras (aquelas que pesquisam uma cadeia de caracteres na instância de cadeia de caracteres) levam em consideração a cultura. É difícil lembrar qual método usa o valor padrão e é fácil confundir as sobrecargas.
- A intenção do código que depende de valores padrão para chamadas de método não é clara. No exemplo a seguir, que se baseia em padrões, é difícil saber se o desenvolvedor realmente planejava uma comparação ordinal ou linguística de duas cadeias de caracteres ou se uma diferença entre maiúsculas e minúsculas entre `protocol` e “http” pode fazer com que o teste de igualdade retorne `false`.

     [!code-csharp[Conceptual.Strings.BestPractices#1](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/explicitargs1.cs#1)]
     [!code-vb[Conceptual.Strings.BestPractices#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/explicitargs1.vb#1)]

Em geral, é recomendável que você chame um método que não se baseie em padrões, pois ele torna a intenção do código clara. Isso, por sua vez, torna o código mais legível e fácil de depurar e manter. O exemplo a seguir aborda as questões levantadas sobre o exemplo anterior. Ele deixa claro que a comparação ordinal é usada e que as diferenças de maiúsculas e minúsculas são ignoradas.

[!code-csharp[Conceptual.Strings.BestPractices#2](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/explicitargs1.cs#2)]
[!code-vb[Conceptual.Strings.BestPractices#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/explicitargs1.vb#2)]

## <a name="the-details-of-string-comparison"></a>Os detalhes da comparação de cadeia de caracteres

A comparação de cadeia de caracteres é o centro de muitas operações relacionadas à cadeia de caracteres, especialmente a classificação e teste de igualdade. As cadeias de caracteres são classificadas em uma determinada ordem: se “my” aparece antes de “string” em uma lista classificada de cadeias de caracteres, “my” deve comparar menos que ou igual a “string”. Além disso, a comparação define implicitamente a igualdade. A operação de comparação retorna zero para cadeias de caracteres que considerar iguais. Uma boa interpretação é que nenhuma cadeia de caracteres é menor que a outra. Operações mais significativas envolvendo cadeias de caracteres incluem um ou ambos destes procedimentos: comparação com outra cadeia de caracteres e execução de uma operação de classificação bem definida.

> [!NOTE]
> Você pode baixar as [Tabelas de peso de classificação](https://www.microsoft.com/download/details.aspx?id=10921), um conjunto de arquivos de texto que contêm informações sobre os pesos de caracteres usados em operações de classificação e comparação dos sistemas operacionais Windows, e a [Tabela de elemento de ordenação Unicode padrão](https://www.unicode.org/Public/UCA/latest/allkeys.txt), a versão mais recente da tabela de peso de classificação para Linux e macOS. A versão específica da tabela de peso de classificação do Linux e macOS depende da versão das bibliotecas de [Componentes internacionais para Unicode](http://site.icu-project.org/) instaladas no sistema. Para obter informações sobre versões de ICU e as versões Unicode que elas implementam, veja [Baixar ICU](http://site.icu-project.org/download).

No entanto, avaliar duas cadeias de caracteres quanto à igualdade ou ordem de classificação não gera um único resultado correto, o resultado depende dos critérios usados para comparar as cadeias de caracteres. Em particular, as comparações de cadeia de caracteres ordinais ou baseadas no uso de maiúsculas e minúsculas e convenções classificação da cultura atual ou da [cultura invariável](xref:System.Globalization.CultureInfo.InvariantCulture) (uma cultura independente de localidade com base no idioma inglês) podem gerar resultados diferentes.

Além disso, comparações de cadeia de caracteres usando versões diferentes do .NET ou usando o .NET em diferentes sistemas operacionais ou versões do sistema operacional podem retornar resultados diferentes. Para obter mais informações, veja [Cadeias de caracteres e o padrão Unicode](xref:System.String#Unicode).

### <a name="string-comparisons-that-use-the-current-culture"></a>Comparações de cadeia de caracteres que usam a cultura atual

Um critério envolve o uso das convenções da cultura atual ao comparar cadeias de caracteres. As comparações que se baseiam na cultura atual usam a localidade ou a cultura atual do thread. Se a cultura não for definida pelo usuário, o padrão será a configuração na janela **Opções Regionais** no Painel de Controle. Você deve sempre usar comparações que se baseiam na cultura atual quando os dados forem linguisticamente relevantes e quando eles refletirem a interação do usuário que leva em conta a cultura.

No entanto, o comportamento da comparação e do uso de maiúsculas e minúsculas no .NET muda quando a cultura muda. Isso acontece quando um aplicativo é executado em um computador que tem uma cultura diferente do computador em que o aplicativo foi desenvolvido ou quando o thread em execução muda sua cultura. Esse comportamento é intencional, mas permanece não óbvio para muitos desenvolvedores. O exemplo a seguir ilustra as diferenças na ordem de classificação entre as culturas do inglês americano ("en-US") e de sueco ("SV-SE"). Observe que as palavras "ångström", "Windows" e "Visual Studio" aparecem em posições diferentes nas matrizes de cadeias de caracteres classificadas.

[!code-csharp[Conceptual.Strings.BestPractices#3](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison1.cs#3)]
[!code-vb[Conceptual.Strings.BestPractices#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison1.vb#3)]

As comparações que não diferenciam maiúsculas de minúsculas que usam a cultura atual são iguais às comparações que levam em conta a cultura, exceto que elas ignoram as maiúsculas e minúsculas conforme determinado pela cultura atual do thread. Esse comportamento pode se manifestar em ordens de classificação também.

As comparações que usam a semântica de cultura atual são o padrão para os seguintes métodos:

- As sobrecargas de <xref:System.String.Compare%2A?displayProperty=nameWithType> que não incluem um parâmetro <xref:System.StringComparison>.
- As sobrecargas de <xref:System.String.CompareTo%2A?displayProperty=nameWithType>.
- O método padrão <xref:System.String.StartsWith%28System.String%29?displayProperty=nameWithType> e o método <xref:System.String.StartsWith%28System.String%2CSystem.Boolean%2CSystem.Globalization.CultureInfo%29?displayProperty=nameWithType> com um parâmetro `null`<xref:System.Globalization.CultureInfo>.
- O método padrão <xref:System.String.EndsWith%28System.String%29?displayProperty=nameWithType> e o método <xref:System.String.EndsWith%28System.String%2CSystem.Boolean%2CSystem.Globalization.CultureInfo%29?displayProperty=nameWithType> com um parâmetro `null`<xref:System.Globalization.CultureInfo>.
- As sobrecargas de <xref:System.String.IndexOf%2A?displayProperty=nameWithType> que aceitam um <xref:System.String> como um parâmetro de pesquisa e que não têm um parâmetro <xref:System.StringComparison>.
- As sobrecargas de <xref:System.String.LastIndexOf%2A?displayProperty=nameWithType> que aceitam um <xref:System.String> como um parâmetro de pesquisa e que não têm um parâmetro <xref:System.StringComparison>.

Em qualquer caso, é recomendável que você chame uma sobrecarga que tenha um parâmetro <xref:System.StringComparison> para deixar a intenção da chamada do método clara.

Bugs sutis e não tão sutis podem surgir quando dados de cadeias de caracteres não linguísticas são interpretados linguisticamente ou quando os dados da cadeia de caracteres de uma cultura específica são interpretados usando as convenções de outra cultura. O exemplo canônico é o problema do I turco.

Para quase todos os alfabetos latinos, incluindo inglês americano, o caractere "i" (\u0069) é a versão em minúsculas do caractere "I" (\u0049). Essa regra de maiúsculas e minúsculas rapidamente se torna o padrão para alguém programando em tal cultura. No entanto, o alfabeto turco ("tr-TR") inclui um caractere "I com um ponto", "İ" (\u0130), que é a versão maiúscula de "i". O turco também inclui um caractere minúsculo "i sem um ponto", "ı" (\u0131), que em maiúscula é “I”. Esse comportamento também ocorre na cultura azerbaijana ("az").

Portanto, as suposições feitas sobre a colocação do "i" em maiúscula ou do "I" em minúscula não são válidas entre todas as culturas. Se você usar as sobrecargas padrão para rotinas de comparação de cadeias de caracteres, elas estarão sujeitas à variação entre culturas. Se os dados a serem comparados são forem linguísticos, o uso das sobrecargas padrão pode gerar resultados indesejáveis, como a tentativa a seguir de realizar uma comparação que não diferencia maiúsculas de minúsculas das cadeias de caracteres “file” e “FILE” ilustra.

[!code-csharp[Conceptual.Strings.BestPractices#11](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/turkish1.cs#11)]
[!code-vb[Conceptual.Strings.BestPractices#11](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/turkish1.vb#11)]

Essa comparação pode causar problemas significativos se a cultura for usada inadvertidamente nas configurações sensíveis à segurança, como no exemplo a seguir. Uma chamada de método como `IsFileURI("file:")` retorna `true` se a cultura atual for o inglês dos EUA, mas `false` se a cultura atual for turco. Assim, em sistemas turcos, alguém poderia driblar as medidas de segurança que bloqueiam o acesso a URIs que não diferenciam maiúsculas de minúsculas que começam com “FILE:”.

[!code-csharp[Conceptual.Strings.BestPractices#12](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/turkish1.cs#12)]
[!code-vb[Conceptual.Strings.BestPractices#12](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/turkish1.vb#12)]

Nesse caso, como "file:" deve ser interpretado como um identificador não-linguístico, que não diferencia a cultura, o código deverá ser escrito conforme mostrado no exemplo a seguir:

[!code-csharp[Conceptual.Strings.BestPractices#13](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/turkish1.cs#13)]
[!code-vb[Conceptual.Strings.BestPractices#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/turkish1.vb#13)]

### <a name="ordinal-string-operations"></a>Operações de cadeia de caracteres ordinal

Especificar o valor <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> ou <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> em uma chamada de método significa uma comparação não linguística em que os recursos de linguagens naturais são ignorados. Os métodos que são invocados com esses valores de <xref:System.StringComparison> baseiam as decisões de operação da cadeia de caracteres em comparações de byte simples em vez de no uso de maiúsculas e minúsculas ou tabelas de equivalência que são parametrizadas pela cultura. Na maioria dos casos, essa abordagem se adapta melhor à interpretação pretendida de cadeias de caracteres, enquanto torna o código mais rápido e confiável.

Comparações ordinais são comparações de cadeia de caracteres nas quais cada byte de cada cadeia de caracteres é comparado sem a interpretação linguística, por exemplo, “windows” não corresponde a “Windows”. Isso é basicamente uma chamada para a função `strcmp` de runtime de C. Use essa comparação quando o contexto determinar que as cadeias de caracteres devem corresponder exatamente ou exigir uma política de correspondência conservadora. Além disso, a comparação ordinal é a operação de comparação mais rápida porque ela não aplica nenhuma regra linguística ao determinar um resultado.

As cadeias de caracteres no .NET podem conter caracteres nulos inseridos. Uma das diferenças mais clara entre a comparação ordinal e a que leva em conta a cultura (incluindo comparações que usam a cultura invariável) diz respeito à manipulação de caracteres nulos inseridos em uma cadeia de caracteres. Esses caracteres são ignorados quando você usa os métodos <xref:System.String.Compare%2A?displayProperty=nameWithType> e <xref:System.String.Equals%2A?displayProperty=nameWithType> para realizar comparações que levam em conta a cultura (incluindo comparações que usam a cultura invariável). Como resultado, em comparações que levam em conta a cultura, cadeias de caracteres que contêm caracteres nulos inseridos podem ser consideradas iguais a cadeias de caracteres que não contêm.

> [!IMPORTANT]
> Embora os métodos de comparação de cadeia de caracteres ignorem caracteres nulos inseridos, os métodos de pesquisa de cadeia de caracteres, como <xref:System.String.Contains%2A?displayProperty=nameWithType>, <xref:System.String.EndsWith%2A?displayProperty=nameWithType>, <xref:System.String.IndexOf%2A?displayProperty=nameWithType>, <xref:System.String.LastIndexOf%2A?displayProperty=nameWithType> e <xref:System.String.StartsWith%2A?displayProperty=nameWithType>, não o fazem.

O exemplo a seguir executa uma comparação sensível à cultura da cadeia de caracteres "AA" com uma cadeia de caracteres semelhante que contém vários caracteres nulos inseridos entre "A" e "a" e mostra como as duas cadeias são consideradas iguais:

[!code-csharp[Conceptual.Strings.BestPractices#19](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/embeddednulls1.cs#19)]
 [!code-vb[Conceptual.Strings.BestPractices#19](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/embeddednulls1.vb#19)]

No entanto, as cadeias de caracteres não são consideradas iguais quando você usa a comparação ordinal, como mostra o exemplo a seguir:
  
[!code-csharp[Conceptual.Strings.BestPractices#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/embeddednulls2.cs#20)]
[!code-vb[Conceptual.Strings.BestPractices#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/embeddednulls2.vb#20)]

As comparações ordinais que não diferenciam maiúsculas de minúsculas são a próxima abordagem conservadora. Essas comparações ignoram a maioria das maiúsculas e minúsculas, por exemplo, "windows" corresponde a "Windows". Ao lidar com caracteres ASCII, essa política é equivalente a <xref:System.StringComparison.Ordinal?displayProperty=nameWithType>, exceto que ela ignora as maiúsculas e minúsculas de ASCII normais. Portanto, qualquer caractere em [A, Z] (\u0041-\u005A) corresponde ao caractere correspondente em [a,z] (\u0061-\007A). As maiúsculas e minúsculas fora do intervalo de ASCII usam as tabelas de cultura invariável. Portanto, a comparação a seguir:

[!code-csharp[Conceptual.Strings.BestPractices#4](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison2.cs#4)]
[!code-vb[Conceptual.Strings.BestPractices#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison2.vb#4)]

é equivalente a (mas mais rápida do que) esta comparação:

[!code-csharp[Conceptual.Strings.BestPractices#5](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison2.cs#5)]
[!code-vb[Conceptual.Strings.BestPractices#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison2.vb#5)]

Essas comparações ainda são muito rápidas.

> [!NOTE]
> O comportamento de cadeia de caracteres do sistema de arquivos, chaves do Registro e valores e variáveis de ambiente é melhor representado por <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType>.

<xref:System.StringComparison.Ordinal?displayProperty=nameWithType> e <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> usam os valores binários diretamente e são mais adequados para correspondência. Quando você não tiver certeza sobre as configurações de comparação, use um destes dois valores. No entanto, como elas executam uma comparação byte por byte, elas não classificam por uma ordem de classificação linguística (como um dicionário de inglês), mas por ordem de classificação binária. Os resultados podem parecer estranhos na maioria dos contextos se exibido aos usuários.

A semântica ordinal é o padrão para sobrecargas de <xref:System.String.Equals%2A?displayProperty=nameWithType> que não incluem um argumento <xref:System.StringComparison> (incluindo o operador de igualdade). Em qualquer caso, é recomendável que você chame uma sobrecarga que tenha um parâmetro <xref:System.StringComparison>.

### <a name="string-operations-that-use-the-invariant-culture"></a>operações de cadeia de caracteres que usam a cultura invariável

As comparações com a cultura invariável usam a propriedade <xref:System.Globalization.CultureInfo.CompareInfo%2A> retornada pela propriedade estática <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType>. Esse comportamento é o mesmo em todos os sistemas, ele converte qualquer caractere fora de seu intervalo no que ele acredita que sejam caracteres invariáveis equivalentes. Essa política pode ser útil para manter um conjunto de comportamentos de cadeia de caracteres entre culturas, mas geralmente fornece resultados inesperados.

As comparações que não diferenciam maiúsculas de minúsculas com a cultura invariável usam a propriedade <xref:System.Globalization.CultureInfo.CompareInfo%2A> estática retornada pela propriedade <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> estática para informações de comparação também. As diferenças de maiúsculas e minúsculas entre esses caracteres convertidos são ignoradas.

As comparações que usam <xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType> e <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> funcionam de forma idêntica em cadeias de caracteres ASCII. No entanto, <xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType> toma decisões linguísticas que podem não ser apropriadas para cadeias de caracteres que devem ser interpretadas como um conjunto de bytes. O objeto `CultureInfo.InvariantCulture.CompareInfo` faz o método <xref:System.String.Compare%2A> interpretar determinados conjuntos de caracteres como equivalentes. Por exemplo, a equivalência a seguir é válida na cultura invariável:

InvariantCulture: a + ̊ = å

O caractere de LETRA A MINÚSCULA LATINA "a" (\u0061), quando está próximo ao CARACTERE DE ANEL SUPERIOR COMBINÁVEL "+ " ̊" (\u030a), é interpretado como a LETRA A MINÚSCULA LATINA COM O CARACTERE DE ANEL SUPERIOR "å" (\u00e5). Como mostra o exemplo a seguir, esse comportamento é diferente da comparação ordinal.

[!code-csharp[Conceptual.Strings.BestPractices#15](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison3.cs#15)]
[!code-vb[Conceptual.Strings.BestPractices#15](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison3.vb#15)]

Ao interpretar nomes de arquivo, cookies ou qualquer outro elemento em que uma combinação como "å" pode aparecer, as comparações ordinais ainda oferecem o comportamento mais transparente e adequado.

De forma geral, a cultura invariável tem muito poucas propriedades que a tornam útil para comparação. Ela faz a comparação de maneira linguisticamente relevante, o que impede que ela garanta a equivalência simbólica total, mas não é a opção de exibição em nenhuma cultura. Um dos motivos para usar <xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType> para comparação é persistir dados ordenados, para uma exibição idêntica entre culturas. Por exemplo, se um arquivo de dados grande que contém uma lista de identificadores classificados para exibição acompanha um aplicativo, a adição a essa lista exige uma inserção com a inserção de estilo invariável.

## <a name="choosing-a-stringcomparison-member-for-your-method-call"></a>Escolha de um membro StringComparison para a chamada de método

A tabela a seguir descreve o mapeamento do contexto da cadeia de caracteres semântica para um <xref:System.StringComparison> membro de enumeração:

|Dados|Comportamento|System.StringComparison correspondente<br /><br /> value|
|----------|--------------|-----------------------------------------------------|
|Identificadores internos que diferenciam maiúsculas de minúsculas.<br /><br /> Identificadores que diferenciam maiúsculas e minúsculas nos padrões como XML e HTTP.<br /><br /> Configurações relacionadas à segurança que diferenciam maiúsculas de minúsculas.|Um identificador não linguístico, em que bytes correspondem exatamente.|<xref:System.StringComparison.Ordinal>|
|Identificadores internos que não diferenciam maiúsculas de minúsculas.<br /><br /> Identificadores que não diferenciam maiúsculas e minúsculas em padrões como XML e HTTP.<br /><br /> Caminhos de arquivo.<br /><br /> Chaves do Registro e valores.<br /><br /> Variáveis de ambiente.<br /><br /> Identificadores de recurso (por exemplo, nomes de identificador).<br /><br /> Configurações relacionadas à segurança que não diferenciam maiúsculas de minúsculas.|Um identificador não linguístico, em que as maiúsculas e minúsculas são irrelevantes; especialmente, dados armazenados na maioria dos serviços de sistema do Windows.|<xref:System.StringComparison.OrdinalIgnoreCase>|
|Alguns dados persistentes, linguisticamente relevantes.<br /><br /> Exibição de dados linguísticos que requer uma ordem de classificação fixa.|Dados independentes de cultura que ainda são linguisticamente relevantes.|<xref:System.StringComparison.InvariantCulture><br /><br /> -ou-<br /><br /> <xref:System.StringComparison.InvariantCultureIgnoreCase>|
|Dados exibidos para o usuário.<br /><br /> A maioria das entradas do usuário.|Dados que exigem os costumes linguísticos locais.|<xref:System.StringComparison.CurrentCulture><br /><br /> -ou-<br /><br /> <xref:System.StringComparison.CurrentCultureIgnoreCase>|

## <a name="common-string-comparison-methods-in-net"></a>Métodos comuns de comparação de cadeia de caracteres no .NET

As seções a seguir descrevem os métodos que são mais comumente usados para a comparação de cadeias de caracteres.

### <a name="stringcompare"></a>String.Compare

Interpretação padrão: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>.

Como a operação mais central da interpretação de cadeia de caracteres, todas as instâncias de chamadas desse método devem ser examinadas para determinar se as cadeias de caracteres devem ser interpretadas de acordo com a cultura atual ou dissociadas da cultura (simbolicamente). Normalmente, é o último e uma comparação <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> deve ser usada em vez disso.

A classe <xref:System.Globalization.CompareInfo?displayProperty=nameWithType>, que é retornada pelo <xref:System.Globalization.CultureInfo.CompareInfo%2A?displayProperty=nameWithType> também inclui uma propriedade, um método <xref:System.Globalization.CompareInfo.Compare%2A> que fornece um grande número de opções correspondentes (ordinal, ignorando espaço em branco, ignorando tipo kana e assim por diante) por meio da enumeração de sinalizador <xref:System.Globalization.CompareOptions>.

### <a name="stringcompareto"></a>String.CompareTo

Interpretação padrão: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>.

Esse método no momento não oferece uma sobrecarga que especifica um tipo <xref:System.StringComparison>. É possível converter esse método para o formulário recomendado <xref:System.String.Compare%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType>.

Os tipos que implementam as interfaces <xref:System.IComparable> e <xref:System.IComparable%601> implementam este método. Como ele não oferece a opção de um parâmetro <xref:System.StringComparison>, os tipos de implementação geralmente permitem que o usuário especifique um <xref:System.StringComparer> em seu construtor. O exemplo a seguir define uma classe `FileName` cujo construtor de classe inclui um parâmetro <xref:System.StringComparer>. Esse objeto <xref:System.StringComparer> é usado no método `FileName.CompareTo`.

[!code-csharp[Conceptual.Strings.BestPractices#6](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/api1.cs#6)]
[!code-vb[Conceptual.Strings.BestPractices#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/api1.vb#6)]

### <a name="stringequals"></a>String.Equals

Interpretação padrão: <xref:System.StringComparison.Ordinal?displayProperty=nameWithType>.

A classe <xref:System.String> permite que você teste a igualdade chamando as sobrecargas do método <xref:System.String.Equals%2A> ou estáticas ou usando o operador de igualdade estático. O operador e as sobrecargas utilizam a comparação ordinal por padrão. No entanto, ainda é recomendável que você chame uma sobrecarga que especifique explicitamente o tipo <xref:System.StringComparison> mesmo se você desejar executar uma comparação ordinal. Isso facilita a pesquisa de código para uma determinada interpretação da cadeia de caracteres.

### <a name="stringtoupper-and-stringtolower"></a>String.ToUpper e String.ToLower

Interpretação padrão: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>.

Você deve ter cuidado ao usar esses métodos, pois forçar uma cadeia de caracteres para maiúsculas ou minúsculas normalmente é usado como uma normalização pequena para comparar cadeias de caracteres independentemente de maiúsculas e minúsculas. Nesse caso, considere o uso de uma comparação que não diferencie maiúsculas de minúsculas.

Os métodos <xref:System.String.ToUpperInvariant%2A?displayProperty=nameWithType> e <xref:System.String.ToLowerInvariant%2A?displayProperty=nameWithType> também estão disponíveis. <xref:System.String.ToUpperInvariant%2A> é o modo padrão para normalizar maiúsculas e minúsculas. As comparações feitas usando <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> são a composição de duas chamadas de maneira comportamental: chamar <xref:System.String.ToUpperInvariant%2A> em ambos os argumentos de cadeia de caracteres e fazer uma comparação usando <xref:System.StringComparison.Ordinal?displayProperty=nameWithType>.

Também há sobrecargas disponíveis para converter para maiúsculas e minúsculas em uma cultura específica, passando um objeto <xref:System.Globalization.CultureInfo> que representa aquela cultura para o método.

### <a name="chartoupper-and-chartolower"></a>Char.ToUpper e Char.ToLower

Interpretação padrão: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>.

Esses métodos funcionam da mesma forma que os métodos <xref:System.String.ToUpper%2A?displayProperty=nameWithType> e <xref:System.String.ToLower%2A?displayProperty=nameWithType> descritos na seção anterior.

### <a name="stringstartswith-and-stringendswith"></a>String.StartsWith e String.EndsWith

Interpretação padrão: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>.

Por padrão, esses dois métodos executam uma comparação que leva em conta a cultura.

### <a name="stringindexof-and-stringlastindexof"></a>String.IndexOf e String.LastIndexOf

Interpretação padrão: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>.

Há uma falta de consistência em como as sobrecargas padrão desses métodos realizam comparações. Todos os métodos <xref:System.String.IndexOf%2A?displayProperty=nameWithType> e <xref:System.String.LastIndexOf%2A?displayProperty=nameWithType> que incluem um parâmetro <xref:System.Char> realizam uma comparação ordinal, mas os métodos padrão <xref:System.String.IndexOf%2A?displayProperty=nameWithType> e <xref:System.String.LastIndexOf%2A?displayProperty=nameWithType> que incluem um parâmetro <xref:System.String> executam uma comparação que diferencia a cultura.

Se você chamar o método <xref:System.String.IndexOf%28System.String%29?displayProperty=nameWithType> ou <xref:System.String.LastIndexOf%28System.String%29?displayProperty=nameWithType> e passar a ele uma cadeia de caracteres para localizar na instância atual, é recomendável que você chame uma sobrecarga que especifique explicitamente o tipo <xref:System.StringComparison>. As sobrecargas que incluem um argumento <xref:System.Char> não permitem que você especifique um tipo <xref:System.StringComparison>.

## <a name="methods-that-perform-string-comparison-indirectly"></a>Métodos que realizam comparação indireta de cadeia de caracteres

Alguns métodos que não de cadeias de caracteres que têm a comparação de cadeia de caracteres como uma operação central usam o tipo <xref:System.StringComparer>. A classe <xref:System.StringComparer> inclui quatro propriedades estáticas que retornam instâncias <xref:System.StringComparer> cujos métodos <xref:System.StringComparer.Compare%2A?displayProperty=nameWithType> realizam os seguintes tipos de comparações de cadeias de caracteres:

- Comparações de cadeias de caracteres que levam em conta a cultura usando a cultura atual. Este objeto <xref:System.StringComparer> é retornado pela propriedade <xref:System.StringComparer.CurrentCulture%2A?displayProperty=nameWithType>.
- Comparações que não diferenciam maiúsculas de minúsculas usando a cultura atual. Este objeto <xref:System.StringComparer> é retornado pela propriedade <xref:System.StringComparer.CurrentCultureIgnoreCase%2A?displayProperty=nameWithType>.
- Comparações sem diferenciação de cultura usando as regras de comparação de palavras da cultura invariável. Este objeto <xref:System.StringComparer> é retornado pela propriedade <xref:System.StringComparer.InvariantCulture%2A?displayProperty=nameWithType>.
- Comparações que não diferenciam maiúsculas e minúsculas e a cultura usando as regras de comparação de palavras da cultura invariável. Este objeto <xref:System.StringComparer> é retornado pela propriedade <xref:System.StringComparer.InvariantCultureIgnoreCase%2A?displayProperty=nameWithType>.
- Comparação ordinal. Este objeto <xref:System.StringComparer> é retornado pela propriedade <xref:System.StringComparer.Ordinal%2A?displayProperty=nameWithType>.
- Comparação ordinal que não diferencia maiúsculas de minúsculas. Este objeto <xref:System.StringComparer> é retornado pela propriedade <xref:System.StringComparer.OrdinalIgnoreCase%2A?displayProperty=nameWithType>.

### <a name="arraysort-and-arraybinarysearch"></a>Array.Sort e Array.BinarySearch

Interpretação padrão: <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType>.

Ao armazenar quaisquer dados em uma coleção ou ler dados persistentes de um arquivo ou banco de dados em uma coleção, alternar a cultura atual pode invalidar as invariáveis na coleção. O método <xref:System.Array.BinarySearch%2A?displayProperty=nameWithType> presume que os elementos na matriz a serem pesquisados já estão classificados. Para classificar qualquer elemento de cadeia de caracteres na matriz, o método <xref:System.Array.Sort%2A?displayProperty=nameWithType> chama o método <xref:System.String.Compare%2A?displayProperty=nameWithType> para ordenar os elementos individuais. Usar um comparador que leva em conta a cultura pode ser perigoso se a cultura mudar entre o momento em que a matriz é ordenada e que seu conteúdo é pesquisado. Por exemplo, no código a seguir, o armazenamento e a recuperação operam no comparador que é fornecido implicitamente pela propriedade `Thread.CurrentThread.CurrentCulture`. Se a cultura puder mudar entre as chamadas para `StoreNames` e `DoesNameExist`, e especialmente se os conteúdos da matriz forem mantidos em algum lugar entre as duas chamadas de método, a pesquisa binária poderá falhar.

[!code-csharp[Conceptual.Strings.BestPractices#7](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect1.cs#7)]
 [!code-vb[Conceptual.Strings.BestPractices#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect1.vb#7)]

Uma variação recomendada é exibida no exemplo a seguir, que usa o mesmo método de comparação (que não leva em conta a cultura) ordinal para classificar e pesquisar a matriz. O código de alteração é refletido nas linhas rotuladas como `Line A` e `Line B` nos dois exemplos.

[!code-csharp[Conceptual.Strings.BestPractices#8](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect1.cs#8)]
[!code-vb[Conceptual.Strings.BestPractices#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect1.vb#8)]

Se esses dados forem mantidos e movidos entre as culturas e a classificação for usada para apresentar esses dados para o usuário, você pode considerar usar <xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType>, que opera linguisticamente para a melhor saída do usuário, mas não é afetado pelas alterações na cultura. O exemplo a seguir modifica os dois exemplos anteriores para usar a cultura invariável para classificar e pesquisar a matriz.

[!code-csharp[Conceptual.Strings.BestPractices#9](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect1.cs#9)]
[!code-vb[Conceptual.Strings.BestPractices#9](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect1.vb#9)]

### <a name="collections-example-hashtable-constructor"></a>Exemplo de coleções: Construtor de Hashtable

As cadeias de caracteres de hash fornecem um segundo exemplo de uma operação que é afetada pela maneira como as cadeias de caracteres são comparadas.

O exemplo a seguir cria um objeto <xref:System.Collections.Hashtable> passando-o para o objeto <xref:System.StringComparer> que é retornado pela propriedade <xref:System.StringComparer.OrdinalIgnoreCase%2A?displayProperty=nameWithType>. Como uma classe <xref:System.StringComparer> que é derivada de <xref:System.StringComparer> implementa a interface <xref:System.Collections.IEqualityComparer>, seu método <xref:System.Collections.IEqualityComparer.GetHashCode%2A> é usado para calcular o código hash de cadeias de caracteres na tabela de hash.

[!code-csharp[Conceptual.Strings.BestPractices#10](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect2.cs#10)]
[!code-vb[Conceptual.Strings.BestPractices#10](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect2.vb#10)]

## <a name="displaying-and-persisting-formatted-data"></a>Exibição e persistência de dados formatados

Quando você exibir dados que não são de cadeias de caracteres como números e datas e horas para os usuários, formate-os usando as configurações culturais do usuário. Por padrão, todos itens a seguir usam a cultura do thread atual em operações de formatação:

- Cadeias de caracteres interpoladas compatíveis com os compiladores [C#](../../csharp/language-reference/tokens/interpolated.md) e [Visual Basic](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md).
- Operações de concatenação de cadeias de caracteres que usam os operadores de concatenação do [C#](../../csharp/language-reference/operators/addition-operator.md#string-concatenation) ou [Visual Basic](../../visual-basic/programming-guide/language-features/operators-and-expressions/concatenation-operators.md), ou que chamam o método <xref:System.String.Concat%2A?displayProperty=nameWithType> diretamente.
- O método <xref:System.String.Format%2A?displayProperty=nameWithType>.
- Os métodos `ToString` dos tipos numéricos e dos tipos de data e hora.

Para especificar explicitamente que uma cadeia de caracteres deve ser formatada usando as convenções de uma cultura específica ou a [cultura invariável](xref:System.Globalization.CultureInfo.InvariantCulture), você pode fazer o seguinte:

- Ao usar os métodos <xref:System.String.Format%2A?displayProperty=nameWithType> e `ToString`, chame uma sobrecarga que tenha um parâmetro `provider`, tal como <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> ou <xref:System.DateTime.ToString%28System.IFormatProvider%29?displayProperty=nameWithType>, e passe a ela a propriedade <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType>, a propriedade <xref:System.Globalization.CultureInfo.InvariantCulture?displayProperty=nameWithType> ou uma instância de <xref:System.Globalization.CultureInfo> que represente a cultura desejada.

- Para concatenação de cadeias de caracteres, não permita que o compilador execute nenhuma conversão implícita. Em vez disso, execute uma conversão explícita, chamando uma sobrecarga `ToString` que tenha um parâmetro `provider`. Por exemplo, o compilador usa implicitamente a cultura atual ao converter um <xref:System.Double> valor em uma cadeia de caracteres no código a seguir:

  [!code-csharp[Implicit String Conversion](./snippets/best-practices-strings/csharp/tostring/Program.cs#1)]
  [!code-vb[Implicit String Conversion](./snippets/best-practices-strings/vb/tostring/Program.vb#1)]

  Em vez disso, você pode especificar explicitamente a cultura cujas convenções de formatação são usadas na conversão chamando o <xref:System.Double.ToString(System.IFormatProvider)?displayProperty=nameWithType> método, como o código a seguir:

  [!code-csharp[Explicit String Conversion](./snippets/best-practices-strings/csharp/tostring/Program.cs#2)]
  [!code-vb[Implicit String Conversion](./snippets/best-practices-strings/vb/tostring/Program.vb#2)]

- Para a interpolação de cadeia de caracteres, em vez de atribuir uma cadeia de caracteres interpolada a uma instância de <xref:System.String>, atribua-a a um <xref:System.FormattableString>. Em seguida, você pode chamar o respectivo método <xref:System.FormattableString.ToString?displayProperty=nameWithType> para produzir uma cadeia de caracteres de resultado que reflete as convenções da cultura atual, ou então pode chamar o método <xref:System.FormattableString.ToString(System.IFormatProvider)?displayProperty=nameWithType> para produzir uma cadeia de caracteres de resultado que reflete as convenções de uma cultura específica. Você também pode passar a cadeia de caracteres formatável para o método <xref:System.FormattableString.Invariant%2A?displayProperty=nameWithType> estático para produzir uma cadeia de caracteres de resultado que reflete as convenções da cultura invariável. O exemplo a seguir ilustra esta abordagem. (A saída do exemplo reflete uma cultura atual de en-US.)

  [!code-csharp[String interpolation](./snippets/best-practices-strings/csharp/formattable/Program.cs)]
  [!code-vb[String interpolation](./snippets/best-practices-strings/vb/formattable/Program.vb)]

Você pode manter os dados que não são de cadeias de caracteres como dados binários ou como dados formatados. Se optar por salvá-los como dados formatados, você deverá chamar uma sobrecarga de método de formatação que inclua um parâmetro `provider` e passar para ele a propriedade <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType>. A cultura invariável fornece um formato consistente para os dados formatados que é independente da cultura e do computador. Em contraste, dados persistentes que são formatados usando culturas diferentes da cultura invariável têm várias limitações:

- É provável que os dados não sejam utilizáveis se forem recuperados em um sistema que tem uma cultura diferente ou se o usuário do sistema atual alterar a cultura atual e tentar recuperar os dados.
- As propriedades de uma cultura em um computador específico podem ser diferentes dos valores padrão. A qualquer momento, um usuário pode personalizar as configurações de exibição que levam em conta a cultura. Devido a isso, os dados formatados que são salvos em um sistema podem não ser legíveis após o usuário personalizar as configurações culturais. É provável que a portabilidade dos dados formatados entre computadores seja ainda mais limitada.
- Os padrões internacionais, regionais ou nacionais que controlam a formatação de números ou datas e horas são alterados ao longo do tempo e essas alterações são incorporadas nas atualizações do sistema operacional Windows. Quando as convenções de formatação mudam, os dados que foram formatados usando as convenções anteriores podem se tornar ilegíveis.

O exemplo a seguir ilustra a portabilidade limitada resultante do uso da formatação que leva em conta a cultura para manter os dados. O exemplo salva uma matriz de valores de data e hora em um arquivo. Eles são formatados usando as convenções da cultura do inglês (Estados Unidos). Depois que o aplicativo altera a cultura do thread atual para francês (Suíça), ele tenta ler os valores salvos usando as convenções de formatação da cultura atual. A tentativa de ler dois dos itens de dados gera uma exceção <xref:System.FormatException> e a matriz de datas agora contém dois elementos incorretos que são iguais a <xref:System.DateTime.MinValue>.

[!code-csharp[Conceptual.Strings.BestPractices#21](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/persistence.cs#21)]
[!code-vb[Conceptual.Strings.BestPractices#21](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/persistence.vb#21)]

No entanto, se você substituir a <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> Propriedade por <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> nas chamadas para <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> e <xref:System.DateTime.Parse%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> , os dados persistentes de data e hora serão restaurados com êxito, como mostra a seguinte saída:

```console
06.05.1758 21:26
05.05.1818 07:19
22.04.1870 23:54
08.09.1890 06:47
18.02.1905 15:12
```
