---
title: Globalização
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- globalization [.NET Framework], about globalization
- global applications, globalization
- international applications [.NET Framework], globalization
- world-ready applications, globalization
- application development [.NET Framework], globalization
- culture, globalization
ms.assetid: 4e919934-6b19-42f2-b770-275a4fae87c9
ms.openlocfilehash: adc617362cf3ba07ff63f1095968e2bd88df88d9
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291910"
---
# <a name="globalization"></a>Globalização

A globalização envolve projetar e desenvolver um aplicativo preparado para o mundo que dá suporte a interfaces localizadas e dados regionais para usuários em várias culturas. Antes de iniciar a fase de design, você deve determinar a quais culturas seu aplicativo dará suporte. Embora um aplicativo seja direcionado para uma única cultura ou região por padrão, você pode projetá-lo e gravá-lo para que ele possa ser facilmente estendido para usuários em outras culturas ou regiões.

Como desenvolvedores, todos nós temos suposições sobre as interfaces do usuário e dados que são formados pelas nossas culturas. Por exemplo, para um desenvolvedor falante de inglês nos Estados Unidos, a serialização de dados de data e hora como uma cadeia de caracteres no formato `MM/dd/yyyy hh:mm:ss` parece perfeitamente razoável. No entanto, desserializar essa cadeia de caracteres em um sistema em uma cultura diferente é provavelmente lançará uma exceção <xref:System.FormatException> ou produzirá dados imprecisos. A globalização nos permite identificar essas suposições específicas de culturas e nos certificarmos de que elas não afetem o design ou código de nosso aplicativo.

Este artigo discute alguns dos principais problemas que você deve considerar e as práticas recomendadas que você pode seguir ao lidar com cadeias de caracteres, valores de data e hora e valores numéricos em um aplicativo globalizado.

## <a name="strings"></a>Cadeias de caracteres

A manipulação de caracteres e cadeias de caracteres é um foco central da globalização, porque cada cultura ou região pode usar caracteres e conjuntos de caracteres diferentes e classificá-los de maneira diferente. Esta seção fornece recomendações para usar cadeias de caracteres em aplicativos globalizados.

### <a name="use-unicode-internally"></a>Usar Unicode internamente

Por padrão, o .NET usa cadeias de caracteres Unicode. Uma cadeia de caracteres do Unicode consiste em zero e um ou mais objetos <xref:System.Char>, cada um dos quais representa uma unidade de código UTF-16. Há uma representação Unicode para quase todos os caracteres em cada conjunto de caracteres no uso em todo o mundo.

Muitos aplicativos e sistemas operacionais, incluindo o sistema operacional Windows, também podem usar as páginas de código para representar conjuntos de caracteres. Páginas de código geralmente contêm valores ASCII padrão de 0x00 a 0x7F e mapeiam outros caracteres para os valores restantes de 0x80 a 0xFF. A interpretação dos valores de 0x80 a 0xFF depende da página de código específica. Por isso, se possível, evite usar as páginas de código em um aplicativo globalizado.

O exemplo a seguir ilustra os perigos da interpretação de dados de página de código quando a página de código padrão em um sistema é diferente da página de código no qual os dados foram salvos. (Para simular esse cenário, o exemplo especifica explicitamente páginas de código diferentes.) Primeiro, o exemplo define uma matriz que consiste nos caracteres maiúsculos do alfabeto grego. Ele os codifica em uma matriz de bytes usando a página de código 737 (também conhecida como MS-DOS grego) e salva a matriz de bytes em um arquivo. Se o arquivo é recuperado e sua matriz de bytes é decodificada usando a página de código 737, os caracteres originais são restaurados. No entanto, se o arquivo é recuperado e sua matriz de bytes é decodificada usando a página de código 1252 (ou Windows-1252, que representa os caracteres do alfabeto latino), os caracteres originais são perdidos.

[!code-csharp[Conceptual.Globalization#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/codepages1.cs#1)]
[!code-vb[Conceptual.Globalization#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/codepages1.vb#1)]

O uso do Unicode garante que as mesmas unidades de código sempre sejam mapeadas para os mesmos caracteres e que os mesmos caracteres sempre sejam mapeados para as mesmas matrizes de bytes.

### <a name="use-resource-files"></a>Usar arquivos de recurso

Mesmo se você estiver desenvolvendo um aplicativo que tenha como alvo uma única cultura ou a região, use arquivos de recurso para armazenar cadeias de caracteres e outros recursos que sejam exibidos na interface do usuário. Você nunca deve adicioná-los diretamente em seu código. Usar arquivos de recurso tem uma série de vantagens:

- Todas as cadeias de caracteres estão em um único local. Você não precisa pesquisar todo o seu código-fonte para identificar cadeias de caracteres para modificá-las para um determinado idioma ou cultura.

- Não há nenhuma necessidade de duplicar cadeias de caracteres. Os desenvolvedores que não usam arquivos de recursos frequentemente definem a mesma cadeia de caracteres em vários arquivos de código-fonte. Essa duplicação aumenta a probabilidade de que uma ou mais instâncias sejam esquecidas quando uma cadeia de caracteres é modificada.

- Você pode incluir recursos que não sejam cadeia de caracteres tais como imagens ou dados binários no arquivo de recurso, em vez de armazená-los em um arquivo autônomo separado, para que eles possam ser recuperados facilmente.

Usar arquivos de recurso tem vantagens específicas se você está criando um aplicativo localizado. Quando você implanta recursos em assemblies satélite, o Common Language Runtime seleciona automaticamente um recurso apropriado com base na cultura da interface do usuário atual, conforme definido pela propriedade <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType>. Desde que você forneça um recurso específico de cultura apropriado e instancie corretamente um objeto <xref:System.Resources.ResourceManager> ou então use uma classe de recurso fortemente tipada, o runtime cuidará dos detalhes para recuperar os recursos apropriados.

Para obter mais informações sobre como criar arquivos de recursos, consulte [Criar arquivos de recurso](../../framework/resources/creating-resource-files-for-desktop-apps.md). Para obter informações sobre como criar e implantar assemblies satélite, consulte [Criar assemblies satélite](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md) e [Empacotar e implantar recursos](../../framework/resources/packaging-and-deploying-resources-in-desktop-apps.md).

### <a name="search-and-compare-strings"></a>Pesquisar e comparar cadeias de caracteres

Sempre que possível, você deve tratar cadeias de caracteres como cadeias de caracteres inteiras em vez de tratá-las como uma série de caracteres individuais. Isso é especialmente importante quando você classifica ou pesquisa subcadeias de caracteres, para evitar problemas associados à análise de caracteres combinados.

> [!TIP]
> Você pode usar a classe <xref:System.Globalization.StringInfo> para trabalhar com os elementos de texto em vez dos caracteres individuais em uma cadeia de caracteres.

Em comparações e pesquisas de cadeia de caracteres, um erro comum é tratar a cadeia de caracteres como uma coleção de caracteres, cada um dos quais é representado por um objeto <xref:System.Char>. Na verdade, um único caractere pode ser formado por um, dois ou mais objetos <xref:System.Char>. Esses caracteres são encontrados com mais frequência em cadeias de caracteres de culturas cujos alfabetos consistem em caracteres fora do intervalo de caracteres do Latim Básico Unicode (U+0021 até U+007E). O exemplo a seguir tenta localizar o índice do caractere LETRA A MAIÚSCULA COM ACENTO GRAVE LATINA (U+00C0) em uma cadeia de caracteres. No entanto, esse caractere pode ser representado de duas maneiras diferentes: como uma única unidade de código (U + 00C0) ou como um caractere composto (duas unidades de código: U + 0041 e U + 0300). Nesse caso, o caractere é representado na instância de cadeia de caracteres por dois <xref:System.Char> objetos, U + 0041 e u + 0300. O código de exemplo chama as sobrecargas <xref:System.String.IndexOf%28System.Char%29?displayProperty=nameWithType> e <xref:System.String.IndexOf%28System.String%29?displayProperty=nameWithType> para localizar a posição desse caractere na instância de cadeia de caracteres, mas elas retornam resultados diferentes. A primeira chamada de método tem um argumento <xref:System.Char>; ele executa uma comparação ordinal e, portanto, não é capaz de encontrar uma correspondência. A segunda chamada tem um argumento <xref:System.String>; ele executa uma comparação com diferenciação entre culturas e, portanto, encontra uma correspondência.

[!code-csharp[Conceptual.Globalization#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/search1.cs#18)]
[!code-vb[Conceptual.Globalization#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/search1.vb#18)]

Você pode evitar alguma ambiguidade deste exemplo (chamadas para duas sobrecargas semelhantes de um método que retorna resultados diferentes) chamando uma sobrecarga que inclui um parâmetro <xref:System.StringComparison>, como o método <xref:System.String.IndexOf%28System.String%2CSystem.StringComparison%29?displayProperty=nameWithType> ou <xref:System.String.LastIndexOf%28System.String%2CSystem.StringComparison%29?displayProperty=nameWithType>.

No entanto, pesquisas nem sempre são com diferenciação entre culturas. Se a finalidade da pesquisa é tomar uma decisão de segurança ou permitir ou não o acesso a alguns recursos, a comparação deve ser ordinal, conforme discutido na próxima seção.

### <a name="test-strings-for-equality"></a>Testar a igualdade das cadeias de caracteres

Se você quiser testar a igualdade de duas cadeias de caracteres em vez de determinar a comparação delas na ordem de classificação, use o método <xref:System.String.Equals%2A?displayProperty=nameWithType> em vez de um método de comparação de cadeia de caracteres, como <xref:System.String.Compare%2A?displayProperty=nameWithType> ou <xref:System.Globalization.CompareInfo.Compare%2A?displayProperty=nameWithType>.

Comparações de igualdade normalmente são executadas para acessar algum recurso de modo condicional. Por exemplo, você pode executar uma comparação de igualdade para verificar uma senha ou confirmar a existência de um arquivo. Essas comparações não linguísticas devem sempre ser de tipo ordinal, em vez de com diferenciação entre culturas. Em geral, você deve chamar o método <xref:System.String.Equals%28System.String%2CSystem.StringComparison%29?displayProperty=nameWithType> da instância ou o método estático <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> com um valor de <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> para cadeias de caracteres como senhas, e um valor de <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> para cadeias de caracteres como nomes de arquivo ou URIs.

Algumas vezes, as comparações de igualdade envolvem pesquisas ou comparações de subcadeias de caracteres em vez de chamadas para o método <xref:System.String.Equals%2A?displayProperty=nameWithType>. Em alguns casos, você pode usar uma subsequência de pesquisa para determinar se aquela subcadeia de caracteres é igual a uma outra cadeia de caracteres. Se a finalidade desta comparação é não linguística, a pesquisa também deve ser ordinal em vez de com diferenciação entre culturas.

O exemplo a seguir ilustra o perigo de uma pesquisa com diferenciação entre culturas em dados não linguísticos. O método `AccessesFileSystem` é projetado para proibir o acesso de URIs que começam com a subcadeia de caracteres "FILE" ao sistema de arquivos. Para fazer isso, ele executa uma comparação com diferenciação entre culturas mas sem diferenciação de maiúsculas e minúsculas do início da URI com a cadeia de caracteres "FILE". Já que um URI que acessa o sistema de arquivos pode começar com "FILE:" ou "file:", a suposição implícita é que esse "i" (U+0069) é sempre o equivalente em minúsculas de "I" (U+0049). No entanto, em turco e azerbaijano, a versão maiúscula de "i" é "İ" (U+0130). Devido a essa discrepância, a comparação com diferenciação entre culturas permite o acesso ao sistema de arquivos quando ele deve ser proibido.

[!code-csharp[Conceptual.Globalization#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/equals1.cs#12)]
[!code-vb[Conceptual.Globalization#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/equals1.vb#12)]

Você pode evitar esse problema ao executar uma comparação ordinal que diferencia maiúsculas de minúsculas, conforme mostra o exemplo a seguir.

[!code-csharp[Conceptual.Globalization#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/equals2.cs#13)]
[!code-vb[Conceptual.Globalization#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/equals2.vb#13)]

### <a name="order-and-sort-strings"></a>Ordenar e classificar cadeias de caracteres

Normalmente, cadeias de caracteres ordenadas que devem ser exibidas na interface do usuário devem ser classificadas com base na cultura. Geralmente, essas comparações de cadeia de caracteres são tratadas implicitamente pelo .NET quando você chama um método que classifica as cadeias de caracteres, como <xref:System.Array.Sort%2A?displayProperty=nameWithType> ou <xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=nameWithType>. Por padrão, as cadeias de caracteres são classificadas pelo uso das convenções de classificação da cultura atual. O exemplo a seguir ilustra a diferença entre quando uma matriz de cadeias de caracteres é classificada usando as convenções das culturas Inglês (Estados Unidos) e Sueco (Suécia).

[!code-csharp[Conceptual.Globalization#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/sort1.cs#14)]
[!code-vb[Conceptual.Globalization#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/sort1.vb#14)]

A comparação de cadeia de caracteres sensíveis à cultura é definida pelo objeto <xref:System.Globalization.CompareInfo>, que é retornado pela propriedade <xref:System.Globalization.CultureInfo.CompareInfo%2A?displayProperty=nameWithType> de cada cultura. Comparações de cadeia de caracteres sensíveis à cultura que usam sobrecargas do método <xref:System.String.Compare%2A?displayProperty=nameWithType> também usam o objeto <xref:System.Globalization.CompareInfo>.

O .NET usa tabelas para realizar classificações com detecção de cultura em dados de cadeia de caracteres. O conteúdo dessas tabelas, que contêm dados sobre os pesos de classificação e a normalização de cadeias de caracteres, é determinado pela versão do padrão Unicode implementada por uma versão específica do .NET. A tabela a seguir lista as versões do Unicode implementadas pelas versões especificadas do .NET Framework e pelo .NET Core. Observe que essa lista de versões com suporte do Unicode aplica-se somente à comparação e classificação de caracteres. ela não se aplica à classificação de caracteres Unicode por categoria. Para saber mais, confira a seção "Cadeias de caracteres e o padrão Unicode" no artigo <xref:System.String>.

|Versão do .NET Framework|Sistema operacional|Versão Unicode|
|----------------------------|----------------------|---------------------|
|.NET Framework 2.0|Todos os sistemas operacionais|Unicode 4.1|
|.NET Framework 3.0|Todos os sistemas operacionais|Unicode 4.1|
|.NET Framework 3.5|Todos os sistemas operacionais|Unicode 4.1|
|.NET Framework 4|Todos os sistemas operacionais|Unicode 5.0|
|.NET Framework 4.5 e posterior no Windows 7|Unicode 5.0|
|.NET Framework 4.5 e posterior nos sistemas operacionais Windows 8 e posteriores|Unicode 6.3.0|
|.NET Core (todas as versões)|Depende da versão do padrão Unicode compatível com o sistema operacional subjacente.|

Começando com o .NET Framework 4.5 e em todas as versões do .NET Core, a comparação e a classificação de cadeia de caracteres dependem do sistema operacional. O .NET Framework 4.5 e posteriores em execução no Windows 7 recuperam dados de suas próprias tabelas que implementam o Unicode 5.0. O .NET Framework 4.5 e posteriores em execução no Windows 8 e nas versões posteriores recuperam dados das tabelas do sistema operacional que implementam o Unicode 6.3. No .NET Core, a versão compatível do Unicode depende do sistema operacional subjacente. Ao serializar os dados classificados com detecção de cultura, você pode usar a classe <xref:System.Globalization.SortVersion> para determinar quando os dados serializados precisam ser classificados para que fiquem consistentes com o .NET e a ordem de classificação do sistema operacional. Para obter um exemplo, consulte o tópico da classe <xref:System.Globalization.SortVersion>.

Se seu aplicativo executa classificações específicas de cultura abrangentes de dados de cadeia de caracteres, você pode trabalhar com a classe <xref:System.Globalization.SortKey> para comparar cadeias de caracteres. Uma chave de classificação reflete os pesos de classificação específicos de uma determinada cultura, incluindo os pesos alfabético, de maiúsculas e minúsculas e de diacríticos de uma determinada cadeia de caracteres. Já que as comparações usando chaves de classificação são binárias, elas são mais rápidas que comparações que usam um objeto <xref:System.Globalization.CompareInfo> implícita ou explicitamente. Crie uma chave de classificação específica à cultura para uma determinada cadeia de caracteres passando a cadeia de caracteres para o método <xref:System.Globalization.CompareInfo.GetSortKey%2A?displayProperty=nameWithType>.

O exemplo a seguir é semelhante ao exemplo anterior. No entanto, em vez de chamar o método <xref:System.Array.Sort%28System.Array%29?displayProperty=nameWithType>, que chama implicitamente o método <xref:System.Globalization.CompareInfo.Compare%2A?displayProperty=nameWithType>, ele define uma implementação <xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType> que compara as chaves de classificação, que ele instancia e passa para o método <xref:System.Array.Sort%60%601%28%60%600%5B%5D%2CSystem.Collections.Generic.IComparer%7B%60%600%7D%29?displayProperty=nameWithType>.

[!code-csharp[Conceptual.Globalization#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/sortkey1.cs#15)]
[!code-vb[Conceptual.Globalization#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/sortkey1.vb#15)]

### <a name="avoid-string-concatenation"></a>Evitar a concatenação da cadeia de caracteres

Se possível, evite usar cadeias de caracteres compostas compiladas em tempo de execução de frases concatenadas. Cadeias de caracteres compostas são difíceis de localizar, porque elas muitas vezes pressupõem uma ordem gramatical no idioma original do aplicativo que não se aplica a outros idiomas localizados.

## <a name="handle-dates-and-times"></a>Lidar com datas e horas

Como você lida com valores de data/hora depende de como estão esses valores: persistentes ou exibidos na interface do usuário. Esta seção examina os dois usos. Ela também aborda como você pode manipular diferenças de fuso horário e operações aritméticas ao trabalhar com datas e horas.

### <a name="display-dates-and-times"></a>Exibir datas e horas

Normalmente, quando as datas e horas são exibidas na interface do usuário, você deve usar as convenções de formatação da cultura do usuário, definidas pela propriedade <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> e pelo objeto <xref:System.Globalization.DateTimeFormatInfo> retornado pela propriedade `CultureInfo.CurrentCulture.DateTimeFormat`. As convenções de formatação da cultura atual são usadas automaticamente quando você formata uma data usando qualquer um dos seguintes métodos:

- O método <xref:System.DateTime.ToString?displayProperty=nameWithType> sem parâmetro

- O método <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType>, que inclui uma cadeia de caracteres de formato

- O método <xref:System.DateTimeOffset.ToString?displayProperty=nameWithType> sem parâmetro

- O <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType>, que inclui uma cadeia de caracteres de formato

- O recurso de [formatação de composição](../base-types/composite-formatting.md), quando ele é usado com datas

O exemplo a seguir exibe dados do pôr-do-sol e nascer do sol duas vezes para 11 de outubro de 2012. Primeiro, ele define a cultura atual para Croata (Croácia) e, em seguida, para Inglês (Grã-Bretanha). Em cada caso, as datas e horas são exibidas no formato apropriado para aquela cultura.

[!code-csharp[Conceptual.Globalization#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates1.cs#2)]
[!code-vb[Conceptual.Globalization#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates1.vb#2)]

### <a name="persist-dates-and-times"></a>Manter datas e horas

Você nunca deve persistir dados de data e hora em um formato que possa variar de acordo com a cultura. Esse é um erro de programação comum, que resulta em dados corrompidos ou em uma exceção de tempo de execução. O exemplo a seguir serializa duas datas, 9 de janeiro de 2013 e 18 de agosto de 2013, como cadeias de caracteres usando as convenções de formatação da cultura Inglês (Estados Unidos). Quando os dados são recuperados e analisados usando as convenções da cultura Inglês (Estados Unidos), eles são restaurado com êxito. No entanto, quando eles são recuperados e analisados usando as convenções da cultura Inglês (Reino Unido), a primeira data é interpretada incorretamente como 1º de setembro e a segunda falha ao ser analisada porque o calendário gregoriano não tem um décimo oitavo mês.

[!code-csharp[Conceptual.Globalization#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates2.cs#3)]
[!code-vb[Conceptual.Globalization#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates2.vb#3)]

Você pode evitar esse problema de qualquer uma de três maneiras:

- Serialize a data e hora no formato binário em vez de uma cadeia de caracteres.

- Salve e analise a representação da data e hora usando uma cadeia de caracteres de formato personalizado, que é a mesma independentemente da cultura do usuário.

- Salve a cadeia de caracteres usando as convenções de formatação da cultura invariável.

O exemplo a seguir ilustra a última abordagem. Ele usa as convenções de formatação da cultura invariável retornada pela propriedade estática <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType>.

[!code-csharp[Conceptual.Globalization#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates3.cs#4)]
[!code-vb[Conceptual.Globalization#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates3.vb#4)]

### <a name="serialization-and-time-zone-awareness"></a>Reconhecimento de serialização e de fuso horário

Um valor de data/hora pode ter várias interpretações, variando de uma hora geral ("As lojas abrem em 2 de janeiro de 2013, às 9h") para um ponto específico no tempo ("Data de nascimento: 2 de janeiro de 2013, 6h32"). Quando um valor temporal representa um ponto específico no tempo e você o restaura de um valor serializado, você deve certificar-se de que ele representa o mesmo ponto no tempo, independentemente da localização geográfica ou do fuso horário do usuário.

O exemplo a seguir ilustra esse problema. Ele salva um único valor de data/hora local como uma cadeia de caracteres em três [formatos padrão](../base-types/standard-date-and-time-format-strings.md) ("G" para data geral com tempo em formato longo, "s" para data/hora classificável e "o" para data/hora de ida e volta), bem como no formato binário.

[!code-csharp[Conceptual.Globalization#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates4.cs#10)]
[!code-vb[Conceptual.Globalization#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates4.vb#10)]

Quando os dados são restaurados em um sistema no mesmo fuso horário do sistema no qual eles foram serializados, os valores de data/hora desserializados refletem com precisão o valor original, como se pode ver na saída:

```console
'3/30/2013 6:00:00 PM' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00.0000000-07:00' --> 3/30/2013 6:00:00 PM Local

3/30/2013 6:00:00 PM Local
```

No entanto, se você restaurar os dados em um sistema em um fuso horário diferente, apenas o valor de data/hora que tiver sido formatado com a cadeia de caracteres de formato padrão "o" (ida e volta) preservará informações de fuso horário e, por isso, representará o mesmo instante no tempo. Aqui está a saída quando os dados de data e hora são restaurados em um sistema no fuso horário padrão românico:

```console
'3/30/2013 6:00:00 PM' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00.0000000-07:00' --> 3/31/2013 3:00:00 AM Local

3/30/2013 6:00:00 PM Local
```

Para refletir com precisão um valor de data/hora que representa um único ponto de tempo, independentemente do fuso horário do sistema no qual os dados são desserializados, você pode realizar qualquer uma das seguintes ações:

- Salvar o valor como uma cadeia de caracteres usando a cadeia de caracteres de formato padrão "o" (ida e volta). Em seguida, desserializá-lo no sistema de destino.

- Convertê-lo para UTC e salvá-lo como uma cadeia de caracteres usando a cadeia de caracteres de formato padrão "r" (RFC1123). Em seguida, desserializá-lo no sistema de destino e convertê-lo para o horário local.

- Convertê-lo em UTC e salvá-lo como uma cadeia de caracteres usando a cadeia de caracteres de formato padrão "u" (classificável universal). Em seguida, desserializá-lo no sistema de destino e convertê-lo para o horário local.

- Convertê-lo em UTC e salvá-lo no formato binário. Em seguida, desserializá-lo no sistema de destino e convertê-lo para o horário local.

O exemplo a seguir ilustra cada uma das técnicas.

[!code-csharp[Conceptual.Globalization#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates8.cs#11)]
[!code-vb[Conceptual.Globalization#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates8.vb#11)]

Quando os dados são serializados em um sistema no fuso horário padrão do Pacífico e desserializados em um sistema no fuso horário padrão românico, o exemplo exibe a seguinte saída:

```console
'2013-03-30T18:00:00.0000000-07:00' --> 3/31/2013 3:00:00 AM Local
'Sun, 31 Mar 2013 01:00:00 GMT' --> 3/31/2013 3:00:00 AM Local
'2013-03-31 01:00:00Z' --> 3/31/2013 3:00:00 AM Local

3/31/2013 3:00:00 AM Local
```

Para obter mais informações, consulte [Convertendo horários entre fusos horários](../datetime/converting-between-time-zones.md).

### <a name="perform-date-and-time-arithmetic"></a>Executar aritmética de data e hora

Tanto os tipos <xref:System.DateTime> e <xref:System.DateTimeOffset> dão suporte a operações aritméticas. Você pode calcular a diferença entre dois valores de data ou então você pode adicionar ou subtrair intervalos de tempo específicos para ou de um valor de data. No entanto, operações aritméticas em valores de data/hora não levam em consideração fusos horários e regras de ajuste de fuso horário. Por isso, data e hora em valores que representam pontos no tempo podem retornar resultados imprecisos.

Por exemplo, ocorre a transição da hora padrão do Pacífico para o horário de verão do Pacífico no segundo domingo de março, que é de 10 de março do ano de 2013. Conforme demonstrado pelo exemplo a seguir, se você calcular a data e hora, o resultado será 48 horas após 9 de março de 2013 às 10:30 da manhã. Em um sistema no fuso horário padrão do Pacífico, o resultado, 11 de março de 2013 às 10:30 da manhã, não leva em consideração o ajuste de horário que ocorre nesse intermédio.

[!code-csharp[Conceptual.Globalization#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates5.cs#8)]
[!code-vb[Conceptual.Globalization#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates5.vb#8)]

Para garantir que uma operação aritmética em valores de data/hora produza resultados precisos, siga estas etapas:

1. Converta o horário no fuso horário de origem em UTC.

2. Execute a operação aritmética.

3. Se o resultado for um valor de data/hora, converta-o de UTC para a hora no fuso horário de origem.

O exemplo a seguir é semelhante ao exemplo anterior, exceto pelo fato de que ele segue essas três etapas para adicionar corretamente 48 horas a 9 de março de 2013, às 10:30 da manhã.

[!code-csharp[Conceptual.Globalization#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates6.cs#9)]
[!code-vb[Conceptual.Globalization#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates6.vb#9)]

Para obter mais informações, consulte [Executando operações aritméticas com datas e horas](../datetime/performing-arithmetic-operations.md).

### <a name="use-culture-sensitive-names-for-date-elements"></a>Usar nomes com detecção de cultura para elementos de data

Seu aplicativo pode precisar exibir o nome do mês ou dia da semana. Para fazer isso, código como o mostrado a seguir é comum.

[!code-csharp[Conceptual.Globalization#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/monthname1.cs#19)]
[!code-vb[Conceptual.Globalization#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/monthname1.vb#19)]

No entanto, esse código sempre retorna os nomes dos dias da semana em inglês. O código que extrai o nome do mês é muitas vezes ainda mais inflexível. Com frequência, ele assume um calendário de doze meses com nomes dos meses em um idioma específico.

Usando [cadeias de caracteres com formato de data e hora personalizado](../base-types/custom-date-and-time-format-strings.md) ou as propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo>, é fácil extrair cadeias de caracteres que refletem os nomes dos dias da semana ou meses na cultura do usuário, conforme ilustrado pelo exemplo a seguir. Altera a cultura atual para o francês (França) e exibe o nome do dia da semana e o nome do mês para 1º de julho de 2013.

[!code-csharp[Conceptual.Globalization#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/monthname2.cs#20)]
[!code-vb[Conceptual.Globalization#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/monthname2.vb#20)]

## <a name="numeric-values"></a>Valores numéricos

Como você lida com números depende de como estão esses valores: persistentes ou exibidos na interface do usuário. Esta seção examina os dois usos.

> [!NOTE]
> Nas operações de análise e formatação, o .NET reconhece somente os caracteres latinos básicos de 0 a 9 (U + 0030 a U + 0039) como dígitos numéricos.

### <a name="display-numeric-values"></a>Exibir valores numéricos

Normalmente, quando os números são exibidos na interface do usuário, você deve usar as convenções de formatação da cultura do usuário, definidas pela propriedade <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> e pelo objeto <xref:System.Globalization.NumberFormatInfo> retornado pela propriedade `CultureInfo.CurrentCulture.NumberFormat`. As convenções de formatação da cultura atual são usadas automaticamente quando você formata uma data usando qualquer um dos seguintes métodos:

- O método sem parâmetro `ToString` de qualquer tipo numérico

- O método `ToString(String)` de qualquer tipo numérico, que inclui uma cadeia de caracteres de formato como um argumento

- O recurso de [formatação de composição](../base-types/composite-formatting.md), quando ele é usado com valores numéricos

O exemplo a seguir exibe a temperatura média por mês em Paris, na França. Ele primeiro define a cultura atual para Francês (França) antes de exibir os dados e, em seguida, define-a como Inglês (Estados Unidos). Em cada caso, as temperaturas e os nomes dos meses são exibidos no formato apropriado para aquela cultura. Observe que as duas culturas usam separadores decimais diferentes no valor da temperatura. Observe também que o exemplo usa a cadeia de caracteres de formato personalizado de data e hora "MMMM" para exibir o nome completo do mês e que aloca a quantidade apropriada de espaço para o nome do mês na cadeia de caracteres de resultado, determinando o comprimento do nome do mês mais longo na matriz <xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A?displayProperty=nameWithType>.

[!code-csharp[Conceptual.Globalization#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/numbers1.cs#5)]
[!code-vb[Conceptual.Globalization#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/numbers1.vb#5)]

### <a name="persist-numeric-values"></a>Manter valores numéricos

Você nunca deve persistir dados numéricos em um formato específico de cultura. Esse é um erro de programação comum, que resulta em dados corrompidos ou em uma exceção de tempo de execução. O exemplo a seguir gera dez números de ponto flutuante aleatórios e, em seguida, serializa-os como cadeias de caracteres usando as convenções de formatação da cultura Inglês (Estados Unidos). Quando os dados são recuperados e analisados usando as convenções da cultura Inglês (Estados Unidos), eles são restaurado com êxito. No entanto, quando eles são recuperados e analisados usando as convenções da cultura Francês (França), nenhum dos números pode ser analisado porque as culturas usam separadores decimais diferentes.

[!code-csharp[Conceptual.Globalization#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/numbers2.cs#6)]
[!code-vb[Conceptual.Globalization#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/numbers2.vb#6)]

Para evitar esse problema, você pode usar uma destas técnicas:

- Salve e analise a representação do número usando uma cadeia de caracteres de formato personalizado, que é a mesma independentemente da cultura do usuário.

- Salve o número como uma cadeia de caracteres usando as convenções de formatação da cultura invariável, retornadas pela propriedade <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType>.

- Serialize o número em formato binário em vez do formato de cadeia de caracteres.

O exemplo a seguir ilustra a última abordagem. Ele serializa a matriz de valores <xref:System.Double> e, em seguida, as desserializa e exibe usando as convenções de formatação das culturas Inglês (Estados Unidos) e Francês (França).

[!code-csharp[Conceptual.Globalization#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/numbers3.cs#7)]
[!code-vb[Conceptual.Globalization#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/numbers3.vb#7)]

A serialização de valores de moeda é um caso especial. Já que um valor de moeda depende da unidade monetária na qual ele é expresso, faz pouco sentido para tratá-lo como um valor numérico independente. No entanto, se você salvar um valor de moeda como uma cadeia de caracteres formatada que inclui um símbolo de moeda, ele não poderá ser desserializado em um sistema cuja cultura padrão use um símbolo de moeda diferente, assim como mostra o exemplo a seguir.

[!code-csharp[Conceptual.Globalization#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/currency1.cs#16)]
[!code-vb[Conceptual.Globalization#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/currency1.vb#16)]

Em vez disso, você deve serializar o valor numérico juntamente com algumas informações culturais como o nome da cultura, para que o valor e o símbolo de moeda possam ser desserializados independentemente da cultura atual. O exemplo a seguir faz isso definindo uma estrutura `CurrencyValue` com dois membros: o valor de <xref:System.Decimal> e o nome da cultura à qual o valor pertence.

[!code-csharp[Conceptual.Globalization#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/currency2.cs#17)]
[!code-vb[Conceptual.Globalization#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/currency2.vb#17)]

## <a name="work-with-culture-specific-settings"></a>Trabalhar com configurações específicas da cultura

No .NET, a classe <xref:System.Globalization.CultureInfo> representa uma cultura ou região específica. Algumas de suas propriedades retornam objetos que fornecem informações específicas sobre alguns aspectos de uma cultura:

- A propriedade <xref:System.Globalization.CultureInfo.CompareInfo%2A?displayProperty=nameWithType> retorna um objeto <xref:System.Globalization.CompareInfo> que contém informações sobre como a cultura compara e ordena as cadeias de caracteres.

- A propriedade <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> retorna um objeto <xref:System.Globalization.DateTimeFormatInfo> que fornece informações específicas à cultura usadas na formatação dos dados de data e hora.

- A propriedade <xref:System.Globalization.CultureInfo.NumberFormat%2A?displayProperty=nameWithType> retorna um objeto <xref:System.Globalization.NumberFormatInfo> que fornece informações específicas à cultura usadas na formatação de dados numéricos.

- A propriedade <xref:System.Globalization.CultureInfo.TextInfo%2A?displayProperty=nameWithType> retorna um objeto <xref:System.Globalization.TextInfo> que fornece informações sobre o sistema de gravação da cultura.

Em geral, não faça suposições sobre os valores de propriedades <xref:System.Globalization.CultureInfo> específicas e seus objetos relacionados. Em vez disso, você deve exibir os dados específicos da cultura como sujeitos a alterações, por estes motivos:

- Valores de propriedade individuais estão sujeitos a alteração e revisão ao longo do tempo; conforme os dados são corrigidos, dados melhores ficam disponíveis, ou então as convenções específicas da cultura são alteradas.

- Os valores de propriedade individuais podem variar entre as versões do .NET ou as versões do sistema operacional.

- O .NET é compatível com culturas de substituição. Isso torna possível definir uma nova cultura personalizada que complementa as culturas padrão existentes ou substitui completamente uma cultura padrão existente.

- Nos sistemas Windows, o usuário pode personalizar as configurações específicas da cultura usando o aplicativo **Região e Idioma** no Painel de Controle. Quando você instancia um objeto <xref:System.Globalization.CultureInfo>, é possível determinar se ele reflete as personalizações desse usuário chamando o construtor <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29>. Normalmente, para aplicativos de usuário final, você deve respeitar as preferências do usuário para que o usuário seja apresentado aos dados em um formato que eles esperam.

## <a name="see-also"></a>Veja também

- [Globalização e localização](index.md)
- [Práticas recomendadas para usar cadeias de caracteres](../base-types/best-practices-strings.md)
