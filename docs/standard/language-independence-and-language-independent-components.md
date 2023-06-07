---
title: Independência da linguagem e componentes independentes da linguagem
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- language interoperability
- Common Language Specification
- cross-language interoperability
- CLS
- runtime, language interoperability
- common language runtime, language interoperability
ms.assetid: 4f0b77d0-4844-464f-af73-6e06bedeafc6
ms.openlocfilehash: aa569c0da5b963243596ef440ef37c08b4fae37f
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288232"
---
# <a name="language-independence-and-language-independent-components"></a>Independência da linguagem e componentes independentes da linguagem

O .NET Framework independe da linguagem. Isso significa que, como desenvolvedor, você pode desenvolver em uma das muitas linguagens que segmentam o .NET Framework, como C#, C++/CLI, Eiffel, F#, IronPython, IronRuby, PowerBuilder, Visual Basic, Visual COBOL e Windows PowerShell. É possível acessar os tipos e os membros das bibliotecas de classes desenvolvidas para o .NET Framework sem que seja necessário conhecer a linguagem em que foram originalmente gravados e sem precisar seguir as convenções da linguagem original. Se você for um desenvolvedor de componentes, o componente poderá ser acessado por qualquer aplicativo do .NET Framework, independentemente da linguagem.

> [!NOTE]
> A primeira parte deste artigo discorre sobre a criação de componentes independentes de linguagem, ou seja, componentes que podem ser consumidos por aplicativos escritos em qualquer linguagem. Você também pode criar um único componente ou aplicativo de código-fonte gravado em várias linguagens; consulte [Interoperabilidade em qualquer idioma](#CrossLang) na segunda parte deste artigo.

Para interagir completamente com outros objetos gravados em qualquer linguagem, os objetos devem expor aos chamadores somente os recursos comuns a todas as linguagens. Esse conjunto comum de recursos é definido pela CLS (Common Language Specification), que é um conjunto de regras que se aplicam aos assemblies gerados. A Common Language Specification é definida na Partição I, cláusulas 7 a 11 do [Padrão ECMA-335: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm).

Se o componente estiver de acordo com a Common Language Specification, ele será compatível com a CLS e poderá ser acessado pelo código em assemblies gravados em qualquer linguagem de programação que dê suporte a CLS. É possível determinar se o componente está de acordo com a Common Language Specification no tempo de compilação aplicando-se o atributo <xref:System.CLSCompliantAttribute> ao código-fonte. Para obter mais informações, consulte [o atributo CLSCompliantAttribute](#CLSAttribute).

Neste artigo:

- [Regras de conformidade com CLS](#Rules)

  - [Tipos e assinaturas de membro de tipo](#Types)

  - [Convenções de nomenclatura](#naming)

  - [Conversão de tipo](#conversion)

  - [Matrizes](#arrays)

  - [Interfaces](#Interfaces)

  - [Enumerações](#enums)

  - [Membros de tipo em geral](#members)

  - [Acessibilidade de membro](#MemberAccess)

  - [Tipos e membros genéricos](#Generics)

  - [Construtores](#ctors)

  - [Propriedades](#properties)

  - [Eventos](#events)

  - [Sobrecargas](#overloads)

  - [Exceções](#exceptions)

  - [Atributos](#attributes)

- [O atributo CLSCompliantAttribute](#CLSAttribute)

- [Interoperabilidade em qualquer idioma](#CrossLang)

<a name="Rules"></a>

## <a name="cls-compliance-rules"></a>Regras de conformidade com CLS

Esta seção discute as regras para criar um componente compatível com CLS. Para obter uma lista completa de regras, consulte Partição I, Cláusula 11 do [Padrão ECMA-335: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm).

> [!NOTE]
> A Common Language Specification aborda cada regra de conformidade com CLS à medida que se aplica a consumidores (desenvolvedores que estão acessando programaticamente um componente em conformidade com CLS), estruturas (desenvolvedores que estão usando um compilador de linguagem para criar bibliotecas em conformidade com CLS) e extensores (desenvolvedores que estão criando uma ferramenta, como um compilador de linguagem ou um analisador de código que cria componentes em conformidade com CLS). Este artigo enfoca as regras que se aplicam às estruturas. Entretanto, algumas das regras que se aplicam a extensores também podem ser aplicadas a assemblies criados usando-se Reflection.Emit.

Para criar um componente independente de linguagem, você só precisa aplicar as regras de compatibilidade com CLS à interface pública do componente. A implementação privada não precisa estar de acordo com a especificação.

> [!IMPORTANT]
> As regras de conformidade com CLS só se aplicam à interface pública de um componente e não à implementação privada.

Por exemplo, inteiros sem sinal que não sejam <xref:System.Byte> não são compatíveis com CLS. Como a classe `Person` no exemplo a seguir expõe uma propriedade `Age` de tipo <xref:System.UInt16>, o código a seguir exibe um aviso do compilador.

[!code-csharp[Conceptual.CLSCompliant#1](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/public1.cs#1)]
[!code-vb[Conceptual.CLSCompliant#1](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/public1.vb#1)]

É possível tornar a classe `Person` compatível com CLS alternado-se o tipo de propriedade `Age` de <xref:System.UInt16> para <xref:System.Int16>, que é um inteiro com sinal 16 bits compatível com CLS. Não é necessário alterar o tipo do campo `personAge` privado.

[!code-csharp[Conceptual.CLSCompliant#2](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/public2.cs#2)]
[!code-vb[Conceptual.CLSCompliant#2](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/public2.vb#2)]

A interface pública de uma biblioteca consiste no seguinte:

- Definições de classes públicas.

- Definições dos membros públicos de classes públicas e definições de membros acessíveis para classes derivadas (ou seja, membros protegidos).

- Parâmetros e tipos de retorno de métodos públicos de classes públicas e parâmetros e tipos de retorno de métodos acessíveis para classes derivadas.

As regras de conformidade com CLS estão listadas na tabela a seguir. O texto das regras é tirado literalmente do [Padrão ECMA-335: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm), com direitos autorais de 2012 da Ecma International. Informações mais detalhadas sobre essas regras são encontradas nas seções a seguir.

|Categoria|Consulte|Regra|Número da regra|
|--------------|---------|----------|-----------------|
|Acessibilidade|[Acessibilidade de membro](#MemberAccess)|A acessibilidade não deverá ser alterada ao substituir métodos herdados, exceto na substituição de um método herdado de um assembly diferente com acessibilidade `family-or-assembly`. Nesse caso, a substituição deverá ter a acessibilidade `family`.|10|
|Acessibilidade|[Acessibilidade de membro](#MemberAccess)|A visibilidade e a acessibilidade de tipos e membros deverão ser de tal forma que os tipos na assinatura de qualquer membro sejam visíveis e acessíveis sempre que o próprio membro estiver visível e acessível. Por exemplo, um método público visível fora do assembly não deve ter um argumento cujo tipo seja visível somente dentro do assembly. A visibilidade e a acessibilidade dos tipos que compõem um tipo genérico instanciado usado na assinatura de qualquer membro deverão estar visíveis e acessíveis sempre que o próprio membro estiver visível e acessível. Por exemplo, um tipo genérico instanciado presente na assinatura de um membro visível fora do assembly não deverá ter um argumento genérico cujo tipo seja visível somente dentro do assembly.|12|
|Matrizes|[Matrizes](#arrays)|As matrizes deverão ter elementos com um tipo compatível com CLS e todas as dimensões da matriz deverão ter limites inferiores iguais a zero. Se o item for uma matriz, o tipo do elemento da matriz será necessário para diferenciar as sobrecargas. Quando a sobrecarga é baseada em dois ou mais tipos de matriz, os tipos de elemento deverão ser chamados de tipos.|16|
|Atributos|[Atributos](#attributes)|Os atributos deverão ser do tipo <xref:System.Attribute?displayProperty=nameWithType> ou de um tipo que o herde.|41|
|Atributos|[Atributos](#attributes)|A CLS só permite um subconjunto das codificações de atributos personalizados. Os únicos tipos que deverão ser exibidos nessas codificações são (consulte a Partição IV): <xref:System.Type?displayProperty=nameWithType>, <xref:System.String?displayProperty=nameWithType>, <xref:System.Char?displayProperty=nameWithType>, <xref:System.Boolean?displayProperty=nameWithType>, <xref:System.Byte?displayProperty=nameWithType>, <xref:System.Int16?displayProperty=nameWithType>, <xref:System.Int32?displayProperty=nameWithType>, <xref:System.Int64?displayProperty=nameWithType>, <xref:System.Single?displayProperty=nameWithType>, <xref:System.Double?displayProperty=nameWithType> e qualquer tipo de enumeração baseado em um tipo inteiro de base compatível com CLS.|34|
|Atributos|[Atributos](#attributes)|A CLS não permite modificadores obrigatórios visíveis publicamente (`modreq`, consulte a Partição II), mas permite modificadores opcionais (`modopt`, consulte a Partição II) que ela não entende.|35|
|Construtores|[Construtores](#ctors)|Um construtor de objeto deverá chamar um construtor de instância de sua classe base antes de qualquer acesso aos dados da instância herdados. (Isso não se aplica a tipos de valor, que não precisam ter construtores.)|21|
|Construtores|[Construtores](#ctors)|Um construtor de objeto não deverá ser chamado, exceto como parte da criação de um objeto e um objeto não deve ser inicializado duas vezes.|22|
|Enumerações|[Enumerações](#enums)|O tipo subjacente de um enum deverá ser um tipo de inteiro CLS interno, o nome do campo deverá ser "value__", e esse campo deverá ser marcado como `RTSpecialName`.|7|
|Enumerações|[Enumerações](#enums)|Há dois tipos diferentes de enums, indicados pela presença ou pela ausência do atributo personalizado <xref:System.FlagsAttribute?displayProperty=nameWithType> (consulte a Biblioteca da Partição IV). Um representa valores de inteiro nomeados; o outro representa sinalizadores de bit nomeados que podem ser combinados para gerar um valor sem nome. O valor de um `enum` não está limitado aos valores especificados.|8|
|Enumerações|[Enumerações](#enums)|Campos estáticos de literais de um enum deverão ter o tipo do próprio enum.|9|
|Eventos|[Eventos](#events)|Os métodos que implementam um evento deverão ser marcados como `SpecialName` nos metadados.|29|
|Eventos|[Eventos](#events)|A acessibilidade de um evento e de seus acessadores deverá ser idêntica.|30|
|Eventos|[Eventos](#events)|Os métodos `add` e `remove` de um evento deverão estar presentes ou ausentes.|31|
|Eventos|[Eventos](#events)|Os métodos `add` e `remove` de um evento deverão utilizar um parâmetro cada um cujo tipo define o tipo do evento, e ele deverá ser derivado de <xref:System.Delegate?displayProperty=nameWithType>.|32|
|Eventos|[Eventos](#events)|Os eventos deverão respeitar um padrão de nomenclatura específico. O atributo `SpecialName` mencionado na regra 29 da CLS deverá ser ignorado em comparações de nome apropriadas e respeitar as regras do identificador.|33|
|Exceções|[Exceções](#exceptions)|Os atributos acionados deverão ser do tipo <xref:System.Exception?displayProperty=nameWithType> ou de um tipo herdado dele. Mesmo assim, os métodos compatíveis com CLS não precisam bloquear a propagação de outros tipos de exceção.|40|
|Geral|[Conformidade com CLS: as regras](#Rules)|As regras CLS só se aplicam a essas partes de um tipo acessíveis ou visíveis fora do assembly de definição.|1|
|Geral|[Conformidade com CLS: as regras](#Rules)|Membros de tipos sem conformidade com CLS não deverão ser marcados como em conformidade com CLS.|2|
|Genéricos|[Tipos e membros genéricos](#Generics)|Os tipos aninhados deverão ter, pelo menos, tantos parâmetros genéricos quanto o tipo delimitador. Os parâmetros genéricos em um tipo aninhado correspondem, por posição, aos parâmetros genéricos no tipo delimitador.|42|
|Genéricos|[Tipos e membros genéricos](#Generics)|O nome de um tipo genérico deverá codificar o número de parâmetros de tipo declarados no tipo não aninhado ou recém-introduzidos no tipo, se aninhado, de acordo com as regras definidas anteriormente.|43|
|Genéricos|[Tipos e membros genéricos](#Generics)|Um tipo genérico deverá redeclarar restrições suficientes para assegurar que todas as restrições no tipo base ou nas interfaces sejam atendidas pelas restrições de tipo genérico.|4444|
|Genéricos|[Tipos e membros genéricos](#Generics)|Tipos usados como restrições em parâmetros genéricos deverão ser compatíveis com CLS.|45|
|Genéricos|[Tipos e membros genéricos](#Generics)|A visibilidade e a acessibilidade de membros (incluindo tipos aninhados) em um tipo genérico instanciado deverão ser consideradas no escopo da instanciação específica, em vez da declaração de tipo genérico como um todo. Supondo isso, as regras de visibilidade e acessibilidade da regra 12 da CLS continuam sendo aplicáveis.|46|
|Genéricos|[Tipos e membros genéricos](#Generics)|Para cada método genérico abstrato ou virtual, deverá haver uma implementação concreta (não abstrata) padrão.|47|
|Interfaces|[Interfaces](#Interfaces)|As interfaces em conformidade com CLS não deverão exigir a definição de métodos incompatíveis com CLS para implementá-los.|18|
|Interfaces|[Interfaces](#Interfaces)|As interfaces compatíveis com CLS não deverão definir métodos estáticos, nem devem definir campos.|19|
|Membros|[Membros de tipo em geral](#members)|Campos e métodos estáticos globais não são compatíveis com CLS.|36|
|Membros|--|O valor de um estático literal é especificado por meio do uso de metadados de inicialização do campo. Um literal compatível com CLS deve ter um valor especificado em metadados de inicialização de campo que sejam exatamente do mesmo tipo que o literal (ou do tipo subjacente, se esse literal for um `enum`).|13|
|Membros|[Membros de tipo em geral](#members)|A restrição vararg não faz parte da CLS e a única convenção de chamada com suporte pela CLS é a convenção de chamada gerenciada padrão.|15|
|Convenções de nomenclatura|[Convenções de nomenclatura](#naming)|Os assemblies deverão seguir o Anexo 7 do Relatório Técnico 15 do Padrão Unicode 3.0 que controla o conjunto de caracteres que podem iniciar e ser incluídos em identificadores, disponíveis online em <https://www.unicode.org/unicode/reports/tr15/tr15-18.html>. Os identificadores deverão estar no formato canônico definido pelo Formulário C de Normalização de Unicode. Para fins de CLS, dois identificadores serão iguais se os mapeamentos em minúsculas (conforme especificado pelos mapeamentos em minúsculas um para um, insensíveis a localidade Unicode) forem os mesmos. Ou seja, para dois identificadores serem considerados diferentes na CLS, eles deverão ser diferentes além de apenas maiúsculas e minúsculas. No entanto, para substituir uma definição herdada, a CLI exige que a codificação precisa da declaração original seja usada.|4|
|Sobrecarga|[Convenções de nomenclatura](#naming)|Todos os nomes introduzidos em um escopo compatível com CLS deverão ser independentes e distintos do tipo, exceto quando os nomes forem idênticos e resolvidos por meio da sobrecarga. Ou seja, embora o CTS permita que um único tipo use o mesmo nome para um método e um campo, a CLS não permite.|5|
|Sobrecarga|[Convenções de nomenclatura](#naming)|Campos e tipos aninhados deverão ser diferenciados apenas por comparação de identificador, mesmo que o CTS permita que assinaturas diferentes sejam distinguidas. Métodos, propriedades e eventos com o mesmo nome (por comparação de identificador) deverão ser diferentes além apenas do tipo de retorno, exceto conforme especificado na Regra 39 da CLS.|6|
|Sobrecarga|[Sobrecargas](#overloads)|Somente propriedades e métodos podem ser sobrecarregados.|37|
|Sobrecarga|[Sobrecargas](#overloads)|As propriedades e os métodos só podem ser sobrecarregados com base no número e nos tipos de seus parâmetros, exceto os operadores de conversão chamados `op_Implicit` e `op_Explicit`, que também podem ser sobrecarregados com base no tipo de retorno.|38|
|Sobrecarga|--|Se dois ou mais métodos em conformidade com CLS declarados em um tipo tiverem o mesmo nome e, para um conjunto específico de instanciações de tipo, tiverem os mesmos tipos de parâmetro e retorno, esses métodos deverão ser semanticamente equivalentes nessas instanciações de tipo.|48|
|Tipos|[Tipo e assinaturas de membro de tipo](#Types)|<xref:System.Object?displayProperty=nameWithType> é compatível com CLS. Qualquer outra classe compatível com CLS deverá herdar de uma classe compatível com CLS.|23|
|Propriedades|[Propriedades](#properties)|Os métodos que implementam os métodos getter e setter de uma propriedade deverão ser marcados como `SpecialName` nos metadados.|24|
|Propriedades|[Propriedades](#properties)|Os acessadores de uma propriedade deverão ser todos estáticos, virtuais ou de instância.|26|
|Propriedades|[Propriedades](#properties)|O tipo de uma propriedade deverá ser o tipo de retorno do getter e o tipo do último argumento do setter. Os tipos dos parâmetros da propriedade deverão ser os tipos dos parâmetros do getter e os tipos de todos os parâmetros, menos o parâmetro final do setter. Todos esses tipos deverão ser compatíveis com CLS e não deverão ser ponteiros gerenciados (por exemplo, não deverão ser passados por referência).|27|
|Propriedades|[Propriedades](#properties)|As propriedades deverão seguir um padrão de nomenclatura específico. O atributo `SpecialName` mencionado na regra 24 da CLS deverá ser ignorado em comparações de nome apropriadas e respeitar as regras do identificador. Uma propriedade deverá ter um método getter, um método setter ou ambos.|28|
|Conversão de tipos|[Conversão de tipo](#conversion)|Se `op_Implicit` ou `op_Explicit` for fornecido, um meio alternativo de fornecimento da coerção deverá ser fornecido.|39|
|Tipos|[Tipo e assinaturas de membro de tipo](#Types)|Tipos de valor demarcado não estão em conformidade com CLS.|3|
|Tipos|[Tipo e assinaturas de membro de tipo](#Types)|Todos os tipos exibidos em uma assinatura deverão ser compatíveis com CLS. Todos os tipos que compõem um tipo genérico instanciado deverão ser compatíveis com CLS.|11|
|Tipos|[Tipo e assinaturas de membro de tipo](#Types)|Referências com tipo não são compatíveis com CLS.|14|
|Tipos|[Tipo e assinaturas de membro de tipo](#Types)|Tipos de ponteiro não gerenciados não são compatíveis com CLS.|17|
|Tipos|[Tipo e assinaturas de membro de tipo](#Types)|Classes compatíveis com CLS, tipos de valor e interfaces não deverão exigir a implementação de membros incompatíveis com CLS.|20|

<a name="Types"></a>

### <a name="types-and-type-member-signatures"></a>Tipos e assinaturas de membro de tipo

O tipo <xref:System.Object?displayProperty=nameWithType> é compatível com CLS e é o tipo base de todos os tipos de objeto no sistema de tipos do .NET Framework. A herança no .NET Framework é implícita (por exemplo, a classe <xref:System.String> herda implicitamente da classe <xref:System.Object>) ou explícita (por exemplo, a classe <xref:System.Globalization.CultureNotFoundException> herda explicitamente da classe <xref:System.ArgumentException>, que herda explicitamente da classe <xref:System.SystemException>, que herda explicitamente da classe <xref:System.Exception>). Para que um tipo derivado esteja em conformidade com CLS, seu tipo base também deverá estar em conformidade com CLS.

O exemplo a seguir mostra um tipo derivado cujo tipo de base não é compatível com CLS. Ele define uma classe `Counter` base que usa um inteiro de 32 bits sem sinal como um contador. Como a classe fornece funcionalidade de contador encapsulando um inteiro sem sinal, a classe é marcada como não compatível com CLS. Assim, uma classe derivada, `NonZeroCounter`, também não é compatível com CLS.

[!code-csharp[Conceptual.CLSCompliant#12](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/type3.cs#12)]
[!code-vb[Conceptual.CLSCompliant#12](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/type3.vb#12)]

Todos os tipos exibidos em assinaturas de membro, incluindo um tipo de retorno de método ou um tipo de propriedade, devem ser compatíveis com CLS. Além disso, para tipos genéricos:

- Todos os tipos que compõe um tipo genérico instanciado devem ser compatíveis com CLS.

- Todos os tipos usados como restrições em parâmetros genéricos devem ser compatíveis com CLS.

O [Common Type System](base-types/common-type-system.md) do .NET Framework inclui vários tipos internos que têm suporte direto do Common Language Runtime e são codificados especialmente nos metadados de um assembly. Desses tipos intrínsecos, os tipos listados na tabela a seguir estão em conformidade com CLS.

|Tipo em conformidade com CLS|Descrição|
|-------------------------|-----------------|
|<xref:System.Byte>|Inteiro sem sinal de 8 bits|
|<xref:System.Int16>|Inteiro com sinal de 16 bits|
|<xref:System.Int32>|Inteiro com sinal de 32 bits|
|<xref:System.Int64>|Inteiro com sinal de 64 bits|
|<xref:System.Single>|Valor do ponto flutuante de precisão simples|
|<xref:System.Double>|Valor de ponto flutuante de precisão dupla|
|<xref:System.Boolean>|Tipo de valor `true` ou `false`|
|<xref:System.Char>|unidade de código codificado UTF-16|
|<xref:System.Decimal>|Número decimal de ponto não flutuante|
|<xref:System.IntPtr>|Ponteiro ou identificador de um tamanho definido por plataforma|
|<xref:System.String>|Coleção de zero, um ou mais objetos <xref:System.Char>|

Os tipos intrínsecos listados na tabela a seguir não são compatíveis com CLS.

|Tipo não compatível|Descrição|Alternativa em conformidade com CLS|
|-------------------------|-----------------|--------------------------------|
|<xref:System.SByte>|Tipo de dados inteiro com sinal de 8 bits|<xref:System.Int16>|
|<xref:System.TypedReference>|Ponteiro para um objeto e seu tipo de runtime|Nenhum|
|<xref:System.UInt16>|Inteiro sem sinal de 16 bits|<xref:System.Int32>|
|<xref:System.UInt32>|Inteiro sem sinal de 32 bits|<xref:System.Int64>|
|<xref:System.UInt64>|Inteiro sem sinal de 64 bits|<xref:System.Int64> (pode estourar), <xref:System.Numerics.BigInteger> ou <xref:System.Double>|
|<xref:System.UIntPtr>|Ponteiro ou identificador sem sinal|<xref:System.IntPtr>|

A biblioteca de classes .NET Framework ou qualquer outra biblioteca de classes pode incluir outros tipos que não sejam compatíveis com CLS; por exemplo:

- Tipos de valor demarcado. O exemplo do C# a seguir cria uma classe que tem uma propriedade pública do tipo `int*` chamada `Value`. Como um `int*` é um tipo de valor demarcado, o compilador o sinaliza como incompatível com CLS.

  [!code-csharp[Conceptual.CLSCompliant#26](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/box2.cs#26)]

- Referências de tipo, que são constructos especiais que contêm referência a um objeto e referência a um tipo. As referências tipadas são representadas no .NET Framework pela classe <xref:System.TypedReference>.

Se um tipo não for compatível com CLS, você deverá aplicar o atributo <xref:System.CLSCompliantAttribute> com um valor `isCompliant` de `false` a ele. Para obter mais informações, consulte [a seção atributo CLSCompliantAttribute](#CLSAttribute) .

O exemplo a seguir ilustra o problema de conformidade com CLS em uma assinatura do método e em uma instanciação de tipo genérico. Ele define uma classe `InvoiceItem` com uma propriedade do tipo <xref:System.UInt32>, uma propriedade do tipo `Nullable(Of UInt32)` e um construtor com parâmetros de tipo <xref:System.UInt32> e `Nullable(Of UInt32)`. Você recebe quatro avisos do compilador ao tentar de compilar esse exemplo.

[!code-csharp[Conceptual.CLSCompliant#3](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/type1.cs#3)]
[!code-vb[Conceptual.CLSCompliant#3](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/type1.vb#3)]

Para eliminar os avisos do compilador, substitua os tipos não compatíveis com CLS na interface pública `InvoiceItem` por tipos compatíveis:

[!code-csharp[Conceptual.CLSCompliant#4](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/type2.cs#4)]
[!code-vb[Conceptual.CLSCompliant#4](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/type2.vb#4)]

Além dos tipos específicos listados, algumas categorias de tipos não são compatíveis com CLS. Entre eles estão tipos de ponteiro não gerenciados e tipos de ponteiro de função. O exemplo a seguir gera um aviso do compilador porque ele usa um ponteiro para um inteiro a fim de criar uma matriz de inteiros.

[!code-csharp[Conceptual.CLSCompliant#5](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/unmanagedptr1.cs#5)]

Para classes abstratas compatíveis com CLS (ou seja, classes marcadas como `abstract` no C# ou como `MustInherit` no Visual Basic), todos os membros da classe também devem ser compatíveis com CLS.

<a name="naming"></a>

### <a name="naming-conventions"></a>Convenções de nomenclatura

Como algumas linguagens de programação não diferenciam maiúsculas de minúsculas, os identificadores (como nomes de namespaces, tipos e membros) devem se diferenciar além de maiúsculas e minúsculas. Dois identificadores serão considerados equivalentes se seus mapeamentos em minúsculas forem os mesmos. O exemplo do C# a seguir define duas classes públicas, `Person` e `person`. Como elas são diferentes apenas em maiúsculas e minúsculas, o compilador do C# as sinaliza como não compatíveis com CLS.

[!code-csharp[Conceptual.CLSCompliant#16](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/naming1.cs#16)]

Identificadores de linguagem de programação, como nomes de namespaces, tipos e membros, devem estar de acordo com o [Padrão Unicode 3.0, Relatório Técnico 15, Anexo 7](https://www.unicode.org/reports/tr15/tr15-18.html). Isso significa que:

- O primeiro caractere de um identificador pode ser qualquer letra maiúscula Unicode, letra minúscula, letra maiúscula do título, letra modificadora, outra letra ou o número da letra. Para obter informações sobre categorias de caracteres Unicode, consulte a enumeração <xref:System.Globalization.UnicodeCategory?displayProperty=nameWithType>.

- Os caracteres subsequentes podem ser de qualquer uma das categorias, como o primeiro caractere e também podem incluir marcas sem espaçamento, marcas que combinam espaçamento, números decimais, pontuações de conector e códigos de formatação.

Antes de comparar identificadores, você deve filtrar códigos de formatação e converter os identificadores em Formulário C de Normalização de Unicode, porque um caractere único pode ser representado por várias unidades de código codificadas em UTF-16. As sequências de caracteres que produzem as mesmas unidades de código no Formulário C de Normalização de Unicode não são compatíveis com CLS. O exemplo a seguir define uma propriedade chamada `Å`, que consiste no caractere SÍMBOLO DE ANGSTROM (U+212B) e uma segunda propriedade chamada `Å`, que consiste no caractere LETRA LATINA MAIÚSCULA A COM ANEL SUPERIOR (U+00C5). Os compiladores do C# e do Visual Basic sinalizam o código-fonte como incompatível com CLS.

[!code-csharp[Conceptual.CLSCompliant#17](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/naming1.cs#17)]
[!code-vb[Conceptual.CLSCompliant#17](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/naming1.vb#17)]

Os nomes de membro em um escopo específico (como os namespaces em um assembly, os tipos em um namespace ou os membros em um tipo) devem ser exclusivos, exceto os nomes resolvidos por meio de sobrecarga. Esse requisito é mais rígido do que o do Common Type System, que permite que vários membros em um escopo tenham nomes idênticos desde que sejam tipos diferentes de membros (por exemplo, um é um método e outro é um campo). Em particular, para membros de tipo:

- Campos e tipos aninhados são diferenciados apenas por nome.

- Métodos, propriedades e eventos que tenham o mesmo nome devem ser diferentes além apenas do tipo de retorno.

O exemplo a seguir ilustra o requisito de que nomes de membros devem ser exclusivos dentro de seu escopo. Ele define uma classe chamada `Converter` que inclui quatro membros chamados `Conversion`. Três são métodos e um é uma propriedade. O método que inclui um parâmetro <xref:System.Int64> tem um nome exclusivo, mas os dois métodos com um parâmetro <xref:System.Int32> não têm, porque o valor retornado não é considerado parte da assinatura de um membro. A propriedade `Conversion` também viola esse requisito porque as propriedades não podem ter o mesmo nome dos métodos sobrecarregados.

[!code-csharp[Conceptual.CLSCompliant#19](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/naming3.cs#19)]
[!code-vb[Conceptual.CLSCompliant#19](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/naming3.vb#19)]

Linguagens individuais incluem palavras-chave exclusivas, portanto, linguagens que apontam para o Common Language Runtime também devem oferecer algum mecanismo para fazer referência a identificadores (como nomes de tipo) que coincidam com palavras-chave. Por exemplo, `case` é uma palavra-chave no C# e no Visual Basic. No entanto, o exemplo do Visual Basic a seguir pode remover a ambiguidade de uma classe chamada `case` da palavra-chave `case`, usando chaves de abertura e fechamento. Caso contrário, o exemplo produziria a mensagem de erro "A palavra-chave não é válida como um identificador", e não seria compilado.

[!code-vb[Conceptual.CLSCompliant#22](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/keyword1.vb#22)]

O exemplo do C# a seguir pode criar uma instância da classe `case` usando o símbolo `@` para remover a ambiguidade do identificador da palavras-chave da linguagem. Sem ele, o compilador do C# exibiria duas mensagens de erro, "Tipo esperado" e "'Maiúsculas e minúsculas' do termo de expressão inválido".

[!code-csharp[Conceptual.CLSCompliant#23](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/keyword2.cs#23)]

<a name="conversion"></a>

### <a name="type-conversion"></a>Conversão de tipos

A Common Language Specification define dois operadores de conversão:

- `op_Implicit`, que é usado para conversões de ampliação que não resultam em perda de dados ou precisão. Por exemplo, a estrutura <xref:System.Decimal> inclui um operador `op_Implicit` sobrecarregado para converter valores de tipos integrais e valores <xref:System.Char> em valores <xref:System.Decimal>.

- `op_Explicit`, que é usado para conversões de redução que possam resultar em perda de magnitude (um valor é convertido em um valor com um intervalo menor) ou precisão. Por exemplo, a estrutura <xref:System.Decimal> inclui um operador `op_Explicit` sobrecarregado para converter <xref:System.Double> e valores <xref:System.Single> em <xref:System.Decimal> e para converter valores <xref:System.Decimal> em valores inteiros, o <xref:System.Double>, <xref:System.Single> e <xref:System.Char>.

No entanto, nem todas as linguagens dão suporte à sobrecarga de operador ou à definição de operadores personalizados. Se optar por implementar esses operadores de conversão, você também deverá fornecer uma maneira alternativa para realizar a conversão. Recomendamos que você forneça os `From` métodos *xxx* e `To` *xxx* .

O exemplo a seguir define conversões explícitas e implícitas em conformidade com CLS. Ele cria uma classe `UDouble` que representa um número de ponto flutuante de precisão dupla com sinal. Ele fornece conversões implícitas de `UDouble` em <xref:System.Double> e conversões explícitas de `UDouble` em <xref:System.Single>, de <xref:System.Double> em `UDouble` e de <xref:System.Single> em `UDouble`. Ele também define um método `ToDouble` como uma alternativa ao operador de conversão implícita e os métodos `ToSingle`, `FromDouble` e `FromSingle` como alternativas aos operadores de conversão explícita.

[!code-csharp[Conceptual.CLSCompliant#15](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/convert1.cs#15)]
[!code-vb[Conceptual.CLSCompliant#15](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/convert1.vb#15)]

<a name="arrays"></a>

### <a name="arrays"></a>Matrizes

As matrizes compatíveis com CLS estão em conformidade com as seguintes regras:

- Todas as dimensões de uma matriz devem ter um limite inferior igual a zero. O exemplo a seguir cria uma matriz não compatível com CLS com um limite inferior de um. Independentemente da presença do atributo <xref:System.CLSCompliantAttribute>, o compilador não detecta se a matriz retornada pelo método `Numbers.GetTenPrimes` não é compatível com CLS.

  [!code-csharp[Conceptual.CLSCompliant#8](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/array1.cs#8)]
  [!code-vb[Conceptual.CLSCompliant#8](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/array1.vb#8)]

- Todos os elementos de matriz devem consistir em tipos compatíveis com CLS. O exemplo a seguir define dois métodos que retornam matrizes não em conformidade com CLS. O primeiro retorna uma matriz de valores <xref:System.UInt32>. O segundo retorna uma matriz <xref:System.Object> que inclui valores <xref:System.Int32> e <xref:System.UInt32>. Embora o compilador identifique a primeira matriz como não em conformidade devido ao seu tipo <xref:System.UInt32>, ele não reconhece que a segunda matriz inclui elementos não em conformidade com CLS.

  [!code-csharp[Conceptual.CLSCompliant#9](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/array2.cs#9)]
  [!code-vb[Conceptual.CLSCompliant#9](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/array2.vb#9)]

- A resolução de sobrecarga para métodos que tenham parâmetros de matriz se baseia no fato de que são matrizes e em seu tipo de elemento. Por esse motivo, a seguinte definição de um método `GetSquares` sobrecarregado é compatível com CLS.

  [!code-csharp[Conceptual.CLSCompliant#10](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/array3.cs#10)]
  [!code-vb[Conceptual.CLSCompliant#10](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/array3.vb#10)]

<a name="Interfaces"></a>

### <a name="interfaces"></a>Interfaces

Interfaces compatíveis com CLS podem definir propriedades, eventos e métodos virtuais (métodos sem implementação). Uma interface compatível com CLS não pode ter nenhum dos seguintes itens:

- Métodos estáticos ou campos estáticos. Os compiladores do C# e do Visual Basic gerarão erros de compilador se você definir um membro estático em uma interface.

- Campos. Os compiladores do C# e do Visual Basic gerarão erros de compilador se você definir um campo em uma interface.

- Métodos que não são compatíveis com CLS. Por exemplo, a definição a seguir da interface inclui um método, `INumber.GetUnsigned`, que está marcado como não compatível com CLS. Este exemplo gera um aviso do compilador.

  [!code-csharp[Conceptual.CLSCompliant#6](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/interface2.cs#6)]
  [!code-vb[Conceptual.CLSCompliant#6](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/interface2.vb#6)]

  Devido a essa regra, os tipos compatíveis com CLS não são necessários para implementar membros não compatíveis com CLS. Se uma estrutura compatível com CLS expuser uma classe que implementa uma interface não compatível com CLS, ela também deverá fornecer implementações concretas de todos os membros não compatíveis com CLS.

Compiladores de linguagem compatíveis com CLS também devem permitir que uma classe forneça implementações separadas dos membros com o mesmo nome e a assinatura em várias interfaces.  O C# e o Visual Basic dão suporte a [implementações explícitas de interface](../csharp/programming-guide/interfaces/explicit-interface-implementation.md) para fornecer implementações diferentes de métodos com nomes idênticos. O Visual Basic também dá suporte à palavra-chave `Implements`, que permite que você designe explicitamente qual interface e membro um determinado membro implementa. O exemplo a seguir ilustra esse cenário, definindo uma classe `Temperature` que implementa as interfaces `ICelsius` e `IFahrenheit` como implementações explícitas de interface.

[!code-csharp[Conceptual.CLSCompliant#24](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/eii1.cs#24)]
[!code-vb[Conceptual.CLSCompliant#24](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/eii1.vb#24)]

<a name="enums"></a>

### <a name="enumerations"></a>Enumerações

Enumerações compatíveis com CLS devem seguir estas regras:

- O tipo subjacente da enumeração deve ser um inteiro intrínseco compatível com CLS (<xref:System.Byte>, <xref:System.Int16>, <xref:System.Int32> ou <xref:System.Int64>). Por exemplo, o código a seguir tenta definir uma enumeração cujo tipo subjacente é <xref:System.UInt32> e gera um aviso do compilador.

  [!code-csharp[Conceptual.CLSCompliant#7](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/enum3.cs#7)]
  [!code-vb[Conceptual.CLSCompliant#7](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/enum3.vb#7)]

- Um tipo de enumeração deve ter um campo de instância única chamado `Value__` que foi marcado com o atributo <xref:System.Reflection.FieldAttributes.RTSpecialName?displayProperty=nameWithType>. Isso permite que você referencie o valor do campo implicitamente.

- Uma enumeração inclui campos estáticos literais, cujos tipos correspondem ao tipo da própria enumeração. Por exemplo, se você definir uma enumeração `State` com valores de `State.On` e `State.Off`, `State.On` e `State.Off` serão campos literais estáticos cujo tipo será `State`.

- Há dois tipos de enumeração:

  - Uma enumeração que representa um conjunto de valores mutuamente excludentes, valores inteiros nomeados. Esse tipo de enumeração é indicado pela ausência do atributo personalizado <xref:System.FlagsAttribute?displayProperty=nameWithType>.

  - Uma enumeração que representa um conjunto de sinalizadores de bit que podem ser combinados para produzir um valor sem nome. Esse tipo de enumeração é indicado pela presença do atributo personalizado <xref:System.FlagsAttribute?displayProperty=nameWithType>.

  Para obter mais informações, consulte a documentação da estrutura <xref:System.Enum>.

- O valor de uma enumeração não está limitado ao intervalo de seus valores especificados. Em outras palavras, o intervalo de valores em uma enumeração é o intervalo de seu valor subjacente. Você pode usar o método <xref:System.Enum.IsDefined%2A?displayProperty=nameWithType> para determinar se um valor especificado é membro de uma enumeração.

<a name="members"></a>

### <a name="type-members-in-general"></a>Membros de tipo em geral

O Common Language Specification requer que todos os campos e métodos sejam acessados como membros de uma classe específica. Portanto, campos e métodos estáticos globais (ou seja, campos ou métodos estáticos que são definidos independentemente de um tipo) não são compatíveis com CLS. Se você tentar incluir um campo ou um método global em seu código fonte, os compiladores do C# e do Visual Basic gerarão um erro de compilador.

A Common Language Specification dá suporte somente à convenção de chamada gerenciada padrão. Ela não dá suporte a convenções e métodos de chamada não gerenciados com listas de argumentos de variável marcadas com a palavra-chave `varargs`. Para as listas de argumentos variáveis que são compatíveis com a convenção de chamada gerenciada padrão, use o atributo <xref:System.ParamArrayAttribute> ou a implementação da linguagem individual, como a palavra-chave `params` no C# e a palavra-chave `ParamArray` no Visual Basic.

<a name="MemberAccess"></a>

### <a name="member-accessibility"></a>Acessibilidade de membro

A substituição de um membro herdado não pode alterar a acessibilidade desse membro. Por exemplo, um método público em uma classe base não pode ser substituído por um método privado em uma classe derivada. Há uma exceção: um membro `protected internal` (em C#) ou `Protected Friend` (em Visual Basic) em um assembly que é substituído por um tipo em um assembly diferente. Nesse caso, a acessibilidade da substituição é `Protected`.

O exemplo a seguir ilustra o erro que é gerado quando o atributo <xref:System.CLSCompliantAttribute> é definido como `true` e `Human`, que é uma classe derivada de `Animal` tenta alterar a acessibilidade da propriedade `Species` de chave pública para privada. O exemplo será compilado com êxito se sua acessibilidade for alterada para pública.

[!code-csharp[Conceptual.CLSCompliant#28](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/accessibility1.cs#28)]
[!code-vb[Conceptual.CLSCompliant#28](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/accessibility1.vb#28)]

Os tipos na assinatura de um membro deverão ser acessíveis sempre que o membro for acessível. Por exemplo, isso significa que um membro público não pode incluir um parâmetro cujo tipo seja privado, protegido ou interno. O exemplo a seguir ilustra o erro do compilador que resulta quando um construtor de classe `StringWrapper` expõe um valor de enumeração `StringOperationType` interno que determina como um valor de cadeia de caracteres deve ser encapsulado.

[!code-csharp[Conceptual.CLSCompliant#27](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/accessibility3.cs#27)]
[!code-vb[Conceptual.CLSCompliant#27](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/accessibility3.vb#27)]

<a name="Generics"></a>

### <a name="generic-types-and-members"></a>Tipos e membros genéricos

Tipos aninhados sempre têm pelo menos o mesmo número de parâmetros genéricos do tipo delimitador. Eles correspondem por posição aos parâmetros genéricos no tipo delimitador. O tipo genérico também pode incluir novos parâmetros genéricos.

A relação entre os parâmetros de tipo genérico de um tipo de contenção e seus tipos aninhados pode ser ocultada pela sintaxe de linguagens individuais. No exemplo a seguir, um tipo genérico `Outer<T>` contém duas classes aninhadas, `Inner1A` e `Inner1B<U>`. As chamadas para o método `ToString`, que cada classe herda de <xref:System.Object.ToString%2A?displayProperty=nameWithType>, mostra que cada classe aninhada inclui parâmetros de tipo de sua classe de contenção.

[!code-csharp[Conceptual.CLSCompliant#29](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/nestedgenerics2.cs#29)]
[!code-vb[Conceptual.CLSCompliant#29](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/nestedgenerics2.vb#29)]

Os nomes de tipo genérico são codificados no formato *nome \` n*, em que *Name* é o nome do tipo, \` é um literal de caractere e *n* é o número de parâmetros declarados no tipo, ou, para tipos genéricos aninhados, o número de parâmetros de tipo introduzidos recentemente. Essa codificação de nomes de tipo genéricos é principalmente de interesse de desenvolvedores que usam a reflexão para acessar tipos genéricos compatíveis com CLS em uma biblioteca.

Se as restrições forem aplicadas a um tipo genérico, qualquer tipo usado como restrição também deverá ser compatível com CLS. O exemplo a seguir define uma classe chamada `BaseClass` que não está em conformidade com CLS e uma classe genérica chamada `BaseCollection` cujo parâmetro de tipo deve derivar de `BaseClass`. Mas como `BaseClass` não é compatível com CLS, o compilador emite um aviso.

[!code-csharp[Conceptual.CLSCompliant#34](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/generics5.cs#34)]
[!code-vb[Conceptual.CLSCompliant#34](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/generics5.vb#34)]

Se um tipo genérico for derivado de um tipo de base genérico, ele deverá redeclarar todas as restrições, para assegurar que as restrições no tipo de base também sejam atendidas. O exemplo a seguir define um `Number<T>` que pode representar qualquer tipo numérico. Ele também define uma classe `FloatingPoint<T>` que representa um valor de ponto flutuante. No entanto, o código-fonte falha na compilação porque não aplica a restrição em `Number<T>` (esse T deve ser um tipo de valor) a `FloatingPoint<T>`.

[!code-csharp[Conceptual.CLSCompliant#30](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/generics2a.cs#30)]
[!code-vb[Conceptual.CLSCompliant#30](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/generics2a.vb#30)]

O exemplo será compilado com êxito se a restrição for adicionada à classe `FloatingPoint<T>`.

[!code-csharp[Conceptual.CLSCompliant#31](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/generics2.cs#31)]
[!code-vb[Conceptual.CLSCompliant#31](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/generics2.vb#31)]

A Common Language Specification impõe um modelo por instanciação conservador para tipos aninhados e membros protegidos. Tipos genéricos abertos não podem expor campos ou membros com assinaturas que contenham uma instanciação específica de um tipo genérico aninhado, protegido. Tipos não genéricos que estendam uma instanciação específica de uma interface ou classe base genérica não podem expor campos ou membros com assinaturas que contenham uma instanciação diferente de um tipo genérico aninhado e protegido.

O exemplo a seguir define um tipo genérico, `C1<T>` (ou `C1(Of T)` no Visual Basic) e uma classe protegida, `C1<T>.N` (ou `C1(Of T).N` no Visual Basic). `C1<T>` possui dois métodos, `M1` e `M2`. No entanto, `M1` o não tem conformidade com CLS porque tenta retornar um `C1<int>.N` (ou `C1(Of Integer).N` ) objeto de C1 \<T> (ou `C1(Of T)` ). Uma segunda classe, `C2`, é derivada de `C1<long>` (ou de `C1(Of Long)`). Tem dois métodos, `M3` e `M4`. No entanto, `M3` não é compatível com CLS porque tenta retornar um objeto `C1<int>.N` (ou `C1(Of Integer).N`) de uma subclasse de `C1<long>`. Os compiladores de linguagens podem ser ainda mais restritivos. Neste exemplo, o Visual Basic exibe um erro ao tentar compilar `M4`.

[!code-csharp[Conceptual.CLSCompliant#32](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/generics4.cs#32)]
[!code-vb[Conceptual.CLSCompliant#32](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/generics4.vb#32)]

<a name="ctors"></a>

### <a name="constructors"></a>Construtores

Os construtores em classes compatíveis com CLS e em estruturas devem seguir estas regras:

- Um construtor de uma classe derivada deve chamar o construtor de instância de sua classe base antes de acessar quaisquer dados de instância herdados. Esse requisito deve-se ao fato de que construtores de classe base não são herdados por suas classes derivadas. Essa regra não se aplica a estruturas que não dão suporte a herança direta.

  Normalmente, os compiladores aplicam essa regra independentemente da conformidade com CLS, conforme mostrado no exemplo a seguir. Ele cria uma classe `Doctor` que é derivada de uma classe `Person`, mas a classe `Doctor` falha ao chamar o construtor de classe `Person` para inicializar campos herdados da instância.

  [!code-csharp[Conceptual.CLSCompliant#11](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/ctor1.cs#11)]
  [!code-vb[Conceptual.CLSCompliant#11](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/ctor1.vb#11)]

- Um construtor de objeto não pode ser chamado, exceto para criar um objeto. Além disso, um objeto não pode ser inicializado duas vezes. Por exemplo, isso significa que <xref:System.Object.MemberwiseClone%2A?displayProperty=nameWithType> e métodos de desserialização como <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType> não devem chamar construtores.

<a name="properties"></a>

### <a name="properties"></a>Propriedades

As propriedades em tipos em conformidade com CLS devem seguir estas regras:

- Uma propriedade deve ter um setter, um getter ou ambos. Em um assembly, eles são implementados como métodos especiais, o que significa que aparecerão como métodos separados (o getter é chamado de `get_`*propertyname* e o setter é `set_`*propertyname*) marcados como `SpecialName` nos metadados do assembly. Os compiladores do C# e do Visual Basic aplicam automaticamente essa regra, sem a necessidade de aplicar o atributo <xref:System.CLSCompliantAttribute>.

- Um tipo de propriedade é o tipo de retorno do getter da propriedade e o último argumento do setter. Esses tipos devem estar em conformidade com CLS e os argumentos não podem ser atribuídos à propriedade por referência (ou seja, não podem ser ponteiros gerenciados).

- Se uma propriedade tiver um getter e um setter, ambos deverão ser virtuais, estáticos ou instâncias. Os compiladores do C# e do Visual Basic aplicam automaticamente essa regra por meio de sua sintaxe de definição da propriedade.

<a name="events"></a>

### <a name="events"></a>Eventos

Um evento é definido por seu nome e tipo. O tipo de evento é um delegado que é usado para indicar o evento. Por exemplo, o evento <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> é do tipo <xref:System.ResolveEventHandler>. Além do evento em si, três métodos com nomes com base no nome do evento fornecem a implementação do evento e estão marcados como `SpecialName` nos metadados do assembly:

- Um método para adicionar um manipulador de eventos, chamado `add_` *EventName*. Por exemplo, o método de assinatura do evento para o evento <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> é chamado `add_AssemblyResolve`.

- Um método para remover um manipulador de eventos chamado `remove_` *EventName*. Por exemplo, o método de remoção para o evento <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> é chamado `remove_AssemblyResolve`.

- Um método para indicar que o evento ocorreu, chamado `raise_` *EventName*.

> [!NOTE]
> A maioria das regras da Common Language Specification em relação a eventos é implementada por compiladores de linguagem e é transparente para desenvolvedores de componente.

Os métodos para adicionar, remover e acionar o evento devem ter a mesma acessibilidade. Eles também devem ser todos estáticos, instâncias ou virtuais. Os métodos para adicionar e remover um evento têm um parâmetro cujo tipo é o tipo de delegado do evento. Os métodos para adicionar e remover devem estar ambos presentes ou ausentes.

O exemplo a seguir define uma classe compatível com CLS chamada `Temperature` que acionará um evento `TemperatureChanged` se a mudança de temperatura entre as duas leituras for igual ou exceder o valor de limite. A classe `Temperature` define explicitamente um método `raise_TemperatureChanged` para executar seletivamente os manipuladores de eventos.

[!code-csharp[Conceptual.CLSCompliant#20](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/event1.cs#20)]
[!code-vb[Conceptual.CLSCompliant#20](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/event1.vb#20)]

<a name="overloads"></a>

### <a name="overloads"></a>Sobrecargas

A Common Language Specification impõe os seguintes requisitos em membros sobrecarregados:

- Os membros podem ser sobrecarregados com base no número de parâmetros e no tipo de qualquer parâmetro. A convenção de chamada, o tipo de retorno, os modificadores personalizados aplicados ao método ou seus parâmetros e se os parâmetros são passados por valor ou por referência não são considerados na diferenciação entre sobrecargas. Para obter um exemplo, consulte o código para o requisito de que os nomes devem ser exclusivos em um escopo na seção [Convenções de nomenclatura](#naming).

- Somente propriedades e métodos podem ser sobrecarregados. Campos e eventos não podem ser sobrecarregados.

- Métodos genéricos podem ser sobrecarregados com base no número de seus parâmetros genéricos.

> [!NOTE]
> Os operadores `op_Explicit` e `op_Implicit` são exceções à regra de que o valor retornado não é considerado parte de uma assinatura de método para resolução de sobrecarga. Esses dois operadores podem ser sobrecarregados com base nos parâmetros e valor retornado.

<a name="exceptions"></a>

### <a name="exceptions"></a>Exceções

Objetos de exceção devem derivar de <xref:System.Exception?displayProperty=nameWithType> ou de outro tipo derivado de <xref:System.Exception?displayProperty=nameWithType>. O exemplo a seguir ilustra o erro do compilador que resulta quando uma classe personalizada chamada `ErrorClass` é usada para tratamento de exceções.

[!code-csharp[Conceptual.CLSCompliant#13](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/exceptions1.cs#13)]
[!code-vb[Conceptual.CLSCompliant#13](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/exceptions1.vb#13)]

Para corrigir este erro, a classe `ErrorClass` deve herdar de <xref:System.Exception?displayProperty=nameWithType>. Além disso, a propriedade `Message` deve ser substituída. O exemplo a seguir corrige esses erros para definir uma classe `ErrorClass` que seja compatível com CLS.

[!code-csharp[Conceptual.CLSCompliant#14](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/exceptions2.cs#14)]
[!code-vb[Conceptual.CLSCompliant#14](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/exceptions2.vb#14)]

<a name="attributes"></a>

### <a name="attributes"></a>Atributos

Em assemblies do .NET Framework, atributos personalizados fornecem um mecanismo extensível para armazenamento de atributos personalizados e recuperação de metadados sobre objetos de programação, como assemblies, tipos, membros e parâmetros de método. Atributos personalizados devem derivar de <xref:System.Attribute?displayProperty=nameWithType> ou de um tipo derivado de <xref:System.Attribute?displayProperty=nameWithType>.

O exemplo a seguir viola essa regra. Ele define uma classe `NumericAttribute` que não deriva de <xref:System.Attribute?displayProperty=nameWithType>. Observe que um erro de compilador só acontece quando o atributo não compatível com CLS é aplicado e não quando a classe é definida.

[!code-csharp[Conceptual.CLSCompliant#18](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/attribute1.cs#18)]
[!code-vb[Conceptual.CLSCompliant#18](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/attribute1.vb#18)]

O construtor ou as propriedades de um atributo compatível com CLS podem expor somente os seguintes tipos:

- <xref:System.Boolean>

- <xref:System.Byte>

- <xref:System.Char>

- <xref:System.Double>

- <xref:System.Int16>

- <xref:System.Int32>

- <xref:System.Int64>

- <xref:System.Single>

- <xref:System.String>

- <xref:System.Type>

- Qualquer tipo de enumeração cujo tipo subjacente seja <xref:System.Byte>, <xref:System.Int16>, <xref:System.Int32> ou <xref:System.Int64>.

O exemplo a seguir define uma classe `DescriptionAttribute` que é derivada de <xref:System.Attribute>. O construtor de classe tem um parâmetro do tipo `Descriptor`, portanto a classe não é compatível com CLS. Observe que o compilador do C# emite um aviso, mas compila com êxito, enquanto o compilador do Visual Basic nem emite um aviso ou um erro.

[!code-csharp[Conceptual.CLSCompliant#33](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/attribute2.cs#33)]
[!code-vb[Conceptual.CLSCompliant#33](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/attribute2.vb#33)]

<a name="CLSAttribute"></a>

## <a name="the-clscompliantattribute-attribute"></a>O atributo CLSCompliantAttribute

O atributo <xref:System.CLSCompliantAttribute> é usado para indicar se um elemento do programa está em conformidade com a Common Language Specification. O construtor <xref:System.CLSCompliantAttribute.%23ctor%28System.Boolean%29> inclui um único parâmetro necessário, `isCompliant`, que indica se o elemento de programa é compatível com CLS.

No tempo de compilação, o compilador detecta os elementos não compatíveis que provavelmente são compatíveis com CLS e emite um aviso. O compilador não emite avisos para tipos ou membros explicitamente declarados como não compatíveis.

Os desenvolvedores de componentes podem usar o atributo <xref:System.CLSCompliantAttribute> de duas maneiras:

- Para definir as partes da interface pública exposta por um componente que são compatíveis com CLS e as partes que não são compatíveis com CLS. Quando o atributo é usado para marcar elementos de programa específicos como em conformidade com CLS, seu uso garante que os elementos sejam acessíveis em todas as linguagens e ferramentas direcionadas ao .NET Framework.

- Para garantir que a interface pública da biblioteca de componentes exponha apenas elementos de programa que são compatíveis com CLS. Se os elementos não forem compatíveis com CLS, os compiladores geralmente emitirão um aviso.

> [!WARNING]
> Em alguns casos, os compiladores de linguagem aplicam as regras de compatibilidade com CLS independentemente do uso ou não do atributo <xref:System.CLSCompliantAttribute>. Por exemplo, definir um membro estático em uma interface viola uma regra CLS. Neste sentido, se você definir um membro `static` (no C#) ou `Shared` (no Visual Basic) em uma interface, os compiladores do C# e do Visual Basic exibirão uma mensagem de erro e não compilarão o aplicativo.

O atributo <xref:System.CLSCompliantAttribute> é marcado com um atributo <xref:System.AttributeUsageAttribute> que tem um valor de <xref:System.AttributeTargets.All?displayProperty=nameWithType>. Esse valor permite que você aplique o atributo <xref:System.CLSCompliantAttribute> a qualquer elemento de programa, incluindo assemblies, módulos, tipos (classes, estruturas, interfaces, enumerações e delegados), membros de tipo (construtores, métodos, propriedades, campos e eventos), parâmetros, parâmetros genéricos e valores de retorno. No entanto, na prática, você deve aplicar o atributo somente a assemblies, tipos e membros de tipo. Caso contrário, os compiladores ignoram o atributo e continuam gerando avisos do compilador sempre que encontrarem um parâmetro não compatível, parâmetro genérico ou valor retornado na interface pública da biblioteca.

O valor do atributo <xref:System.CLSCompliantAttribute> é herdado pelos elementos contidos no programa. Por exemplo, se um assembly for marcado como compatível com CLS, seus tipos também serão compatíveis com CLS. Se um tipo for marcado como em conformidade com CLS, seus membros e tipos aninhados também tem conformidade com CLS.

Você pode substituir explicitamente a compatibilidade herdada aplicando o atributo <xref:System.CLSCompliantAttribute> a um elemento contido no programa. Por exemplo, é possível usar o atributo <xref:System.CLSCompliantAttribute> com um valor `isCompliant` de `false` para definir um tipo não compatível em um assembly compatível, e é possível usar o atributo com um valor `isCompliant` de `true` para definir um tipo compatível em um assembly não compatível. Você também pode definir membros não compatíveis em um tipo compatível. No entanto, um tipo não compatível não pode ter membros compatíveis. Portanto, você não pode usar o atributo com um valor `isCompliant` de `true` para substituir a herança de um tipo não compatível.

Ao desenvolver componentes, você sempre deve usar o atributo <xref:System.CLSCompliantAttribute> para indicar se o assembly, seus tipos e membros são compatíveis com CLS.

Para criar componentes compatíveis com CLS:

1. Use <xref:System.CLSCompliantAttribute> para marcar o assembly como compatível com CLS.

2. Marque qualquer tipo exposto publicamente no assembly que não esteja em conformidade com CLS como não em conformidade.

3. Marque qualquer membro publicamente exposto em tipos compatíveis com CLS como não compatíveis.

4. Forneça uma alternativa em conformidade com CLS para membros não em conformidade com CLS.

Se você marcou com êxito todos os tipos e membros não em conformidade, o compilador não deverá emitir avisos de não conformidade. Entretanto, você deve indicar quais membros não são compatíveis com CLS e listar suas alternativas compatíveis com CLS na documentação do produto.

O exemplo a seguir usa o atributo <xref:System.CLSCompliantAttribute> para definir um assembly compatível com CLS e um tipo, `CharacterUtilities`, que tem dois membros não compatíveis com CLS. Como ambos os membros são marcados com o atributo `CLSCompliant(false)`, o compilador não produz avisos. A classe também fornece uma alternativa compatível com CLS para ambos os métodos. Normalmente, nós adicionaríamos apenas duas sobrecargas ao método `ToUTF16` para fornecer alternativas compatíveis com CLS. Entretanto, como os métodos não podem ser sobrecarregados com base no valor retornado, os nomes dos métodos em conformidade com CLS são diferentes dos nomes dos métodos não em conformidade.

[!code-csharp[Conceptual.CLSCompliant#35](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/indicator3.cs#35)]
[!code-vb[Conceptual.CLSCompliant#35](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/indicator3.vb#35)]

Se você estiver desenvolvendo um aplicativo em vez de uma biblioteca (ou seja, se não estiver expondo tipos ou membros que possam ser consumidos por outros desenvolvedores de aplicativos), a conformidade com CLS dos elementos do programa que seu aplicativo consome só serão de interesse se sua linguagem não der suporte a eles. Nesse caso, seu compilador de linguagem gerará um erro quando você tentar usar um elemento não compatível com CLS.

<a name="CrossLang"></a>

## <a name="cross-language-interoperability"></a>Interoperabilidade em qualquer idioma

A independência de linguagem tem vários significados possíveis. Um significado, discutido no artigo [Independência de linguagem e componentes independentes de linguagem](language-independence-and-language-independent-components.md), envolve o consumo pleno de tipos escritos em uma linguagem de um aplicativo escrito em outra linguagem. Um segundo significado, que é o enfoque deste artigo, envolve combinar o código gravado em várias linguagens em um único assembly do .NET Framework.

O exemplo a seguir ilustra a interoperabilidade em qualquer idioma com a criação de uma biblioteca de classes chamada Utilities.dll que inclui duas classes, `NumericLib` e `StringLib`. A classe `NumericLib` é gravada em C#, e a classe `StringLib` é gravada em Visual Basic. Aqui está o código-fonte de StringUtil.vb, que inclui um único membro, `ToTitleCase`, em sua classe `StringLib`.

[!code-vb[Conceptual.CrossLanguage#1](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.crosslanguage/vb/stringutil.vb#1)]

Aqui está o código-fonte de NumberUtil.cs, que define uma classe `NumericLib` com dois membros, `IsEven` e `NearZero`.

[!code-csharp[Conceptual.CrossLanguage#2](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.crosslanguage/cs/numberutil.cs#2)]

Para empacotar as duas classes em um único assembly, você deve compilá-las em módulos. Para compilar o arquivo de código-fonte do Visual Basic em um módulo, use este comando:

```console
vbc /t:module StringUtil.vb
```

Para obter mais informações sobre a sintaxe de linha de comando do compilador do Visual Basic, consulte [Compilando através da linha de comando](../visual-basic/reference/command-line-compiler/building-from-the-command-line.md).

Para compilar o arquivo de código-fonte do C# em um módulo, use este comando:

```console
csc /t:module NumberUtil.cs
```

Para obter mais informações sobre a sintaxe de linha de comando do compilador do C#, consulte [Compilando na linha de comando com csc.exe](../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md).

Então você usa as [Opções do vinculador](/cpp/build/reference/linker-options) para compilar os dois módulos em um assembly:

```console
link numberutil.netmodule stringutil.netmodule /out:UtilityLib.dll /dll
```

O exemplo a seguir chama os métodos `NumericLib.NearZero` e `StringLib.ToTitleCase`. Observe que o código do Visual Basic e o código do C# podem acessar os métodos em ambas as classes.

[!code-csharp[Conceptual.CrossLanguage#3](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.crosslanguage/cs/useutilities1.cs#3)]
[!code-vb[Conceptual.CrossLanguage#3](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.crosslanguage/vb/useutilities1.vb#3)]

Para compilar o código do Visual Basic, use este comando:

```console
vbc example.vb /r:UtilityLib.dll
```

Para compilar com C#, altere o nome do compilador de **Vbc** para **CSC**e altere a extensão de arquivo de. vb para. cs:

```console
csc example.cs /r:UtilityLib.dll
```

## <a name="see-also"></a>Confira também

- <xref:System.CLSCompliantAttribute>
