---
title: Tipos de coleção em contratos de dados
description: Saiba mais sobre como o modelo de contrato de dados trata as coleções no .NET Framework e como o WCF dá suporte à serialização de dados para tipos de coleção.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- collection types [WCF], data contracts
- data contracts [WCF], collection types
- collection types [WCF]
ms.assetid: 9b45b28e-0a82-4ea3-8c33-ec0094aff9d5
ms.openlocfilehash: 83acf1f74bf3cb117f3f94743eda32d3f2cc4b82
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245174"
---
# <a name="collection-types-in-data-contracts"></a>Tipos de coleção em contratos de dados

Uma *coleção* é uma lista de itens de um determinado tipo. No .NET Framework, essas listas podem ser representadas usando matrizes ou uma variedade de outros tipos (lista genérica, genérica <xref:System.ComponentModel.BindingList%601> , <xref:System.Collections.Specialized.StringCollection> ou <xref:System.Collections.ArrayList> ). Por exemplo, uma coleção pode conter uma lista de endereços para um determinado cliente. Essas coleções são chamadas de *coleções de listas*, independentemente do seu tipo real.

Existe uma forma especial de coleção que representa uma associação entre um item (a "chave") e outro (o "valor"). No .NET Framework, eles são representados por tipos como <xref:System.Collections.Hashtable> e o dicionário genérico. Por exemplo, uma coleção de associação pode mapear uma cidade ("chave") para sua população ("valor"). Essas coleções são chamadas de *coleções de dicionário*, independentemente de seu tipo real.

As coleções recebem tratamento especial no modelo de contrato de dados.

Os tipos que implementam a <xref:System.Collections.IEnumerable> interface, incluindo matrizes e coleções genéricas, são reconhecidos como coleções. Daqueles, tipos que implementam as <xref:System.Collections.IDictionary> interfaces genéricas <xref:System.Collections.Generic.IDictionary%602> são coleções de dicionário; todos os outros são coleções de lista.

Requisitos adicionais em tipos de coleção, como ter um método chamado `Add` e um construtor sem parâmetros, são discutidos em detalhes nas seções a seguir. Isso garante que os tipos de coleção possam ser serializados e desserializados. Isso significa que algumas coleções não têm suporte direto, como o genérico <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> (porque ele não tem um construtor sem parâmetros). No entanto, para obter informações sobre como burlar essas restrições, consulte a seção "usando tipos de interface de coleção e coleções somente leitura", mais adiante neste tópico.

Os tipos contidos em coleções devem ser tipos de contrato de dados ou ser serializáveis de outra forma. Para obter mais informações, consulte [tipos com suporte no serializador de contrato de dados](types-supported-by-the-data-contract-serializer.md).

Para obter mais informações sobre o que é e o que não é considerado uma coleção válida, bem como sobre como as coleções são serializadas, consulte as informações sobre como serializar coleções na seção "regras avançadas de coleção" deste tópico.

## <a name="interchangeable-collections"></a>Coleções intercambiáveis

Todas as coleções de lista do mesmo tipo são consideradas como tendo o mesmo contrato de dados (a menos que sejam personalizadas usando o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo, conforme discutido posteriormente neste tópico). Portanto, por exemplo, os seguintes contratos de dados são equivalentes.

[!code-csharp[c_collection_types_in_data_contracts#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#0)]
[!code-vb[c_collection_types_in_data_contracts#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#0)]

Ambos os contratos de dados resultam em XML semelhante ao código a seguir.

```xml
<PurchaseOrder>
    <customerName>...</customerName>
    <items>
        <Item>...</Item>
        <Item>...</Item>
        <Item>...</Item>
        ...
    </items>
    <comments>
        <string>...</string>
        <string>...</string>
        <string>...</string>
        ...
    </comments>
</PurchaseOrder>
```

A interalteração da coleção permite que você use, por exemplo, um tipo de coleção otimizado para desempenho no servidor e um tipo de coleção projetado para ser associado aos componentes da interface do usuário no cliente.

Semelhante às coleções de lista, todas as coleções de dicionário que têm os mesmos tipos de chave e valor são consideradas como tendo o mesmo contrato de dados (a menos que sejam personalizadas pelo <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo).

Somente o tipo de contrato de dados é importante, à medida que a equivalência da coleção está preocupada, não com tipos .NET. Ou seja, uma coleção de Type1 é considerada equivalente a uma coleção de type2 se type1 e type2 tiverem contratos de dados equivalentes.

As coleções não genéricas são consideradas como tendo o mesmo contrato de dados que as coleções genéricas do tipo `Object` . (Por exemplo, os contratos de dados <xref:System.Collections.ArrayList> e genéricos <xref:System.Collections.Generic.List%601> `Object` são os mesmos.)

## <a name="using-collection-interface-types-and-read-only-collections"></a>Usando tipos de interface de coleção e coleções somente leitura

Os tipos de interface de coleção ( <xref:System.Collections.IEnumerable> , <xref:System.Collections.IDictionary> , genéricas <xref:System.Collections.Generic.IDictionary%602> ou interfaces derivadas dessas interfaces) também são considerados como tendo contratos de dados de coleção, equivalentes aos contratos de dados de coleção para tipos de coleção reais. Portanto, é possível declarar o tipo que está sendo serializado como um tipo de interface de coleção e os resultados são os mesmos de se um tipo de coleção real tivesse sido usado. Por exemplo, os contratos de dados a seguir são equivalentes.

[!code-csharp[c_collection_types_in_data_contracts#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#1)]
[!code-vb[c_collection_types_in_data_contracts#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#1)]

Durante a serialização, quando o tipo declarado é uma interface, o tipo de instância real usado pode ser qualquer tipo que implemente essa interface. As restrições discutidas anteriormente (tendo um construtor sem parâmetros e um `Add` método) não se aplicam. Por exemplo, você pode definir endereços em customer2 para uma instância genérica <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> de address, mesmo que não seja possível declarar diretamente um membro de dados do tipo generic <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> .

Durante a desserialização, quando o tipo declarado é uma interface, o mecanismo de serialização escolhe um tipo que implementa a interface declarada e o tipo é instanciado. O mecanismo de tipos conhecidos (descrito em [tipos conhecidos de contrato de dados](data-contract-known-types.md)) não tem nenhum efeito aqui; a escolha do tipo é interna ao WCF.

## <a name="customizing-collection-types"></a>Personalizando tipos de coleção

Você pode personalizar os tipos de coleção usando o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo, que tem vários usos.

Observe que a personalização de tipos de coleção compromete a troca de coleção, portanto, geralmente é recomendável evitar a aplicação desse atributo sempre que possível. Para obter mais informações sobre esse problema, consulte a seção "regras avançadas de coleta", mais adiante neste tópico.

### <a name="collection-data-contract-naming"></a>Nomenclatura de contrato de dados de coleção

As regras para os tipos de coleção de nomenclatura são semelhantes às de nomenclatura de tipos de contrato de dados regulares, conforme descrito em [nomes de contrato de dados](data-contract-names.md), embora existam algumas diferenças importantes:

- O <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo é usado para personalizar o nome, em vez do <xref:System.Runtime.Serialization.DataContractAttribute> atributo. O <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo também tem `Name` `Namespace` Propriedades e.

- Quando o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo não é aplicado, o nome padrão e o namespace para tipos de coleção dependem dos nomes e namespaces dos tipos contidos na coleção. Eles não são afetados pelo nome e namespace do próprio tipo de coleção. Para obter um exemplo, consulte os tipos a seguir.

  ```csharp
  public CustomerList1 : Collection<string> {}
  public StringList1 : Collection<string> {}
  ```

O nome do contrato de dados dos dois tipos é "ArrayOfstring" e não "CustomerList1" ou "StringList1". Isso significa que a serialização de qualquer um desses tipos no nível raiz produz XML semelhante ao código a seguir.

```xml
<ArrayOfstring>
    <string>...</string>
    <string>...</string>
    <string>...</string>
    ...
</ArrayOfstring>
```

Essa regra de nomenclatura foi escolhida para garantir que qualquer tipo não personalizado que represente uma lista de cadeias de caracteres tenha o mesmo contrato de dados e representação XML. Isso torna possível a interalteração da coleção. Neste exemplo, CustomerList1 e StringList1 são totalmente intercambiáveis.

No entanto, quando o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo é aplicado, a coleção se torna um contrato de dados de coleção personalizado, mesmo que nenhuma propriedade esteja definida no atributo. O nome e o namespace do contrato de dados da coleção dependem do próprio tipo de coleção. Para obter um exemplo, consulte o seguinte tipo.

[!code-csharp[c_collection_types_in_data_contracts#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#2)]
[!code-vb[c_collection_types_in_data_contracts#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#2)]

Quando serializado, o XML resultante é semelhante ao seguinte.

```xml
<CustomerList2>
    <string>...</string>
    <string>...</string>
    <string>...</string>
    ...
</CustomerList2>
```

Observe que isso não é mais equivalente à representação XML dos tipos não personalizados.

- Você pode usar as `Name` `Namespace` Propriedades e para personalizar ainda mais a nomenclatura. Consulte a classe a seguir.

  [!code-csharp[c_collection_types_in_data_contracts#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#3)]
  [!code-vb[c_collection_types_in_data_contracts#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#3)]

O XML resultante é semelhante ao seguinte.

```xml
<cust_list>
    <string>...</string>
    <string>...</string>
    <string>...</string>
    ...
</cust_list>
```

Para obter mais informações, consulte a seção "regras de coleção avançada" mais adiante neste tópico.

### <a name="customizing-the-repeating-element-name-in-list-collections"></a>Personalizando o nome do elemento repetitivo em coleções de listas

As coleções de lista contêm entradas repetidas. Normalmente, cada entrada repetida é representada como um elemento chamado de acordo com o nome do contrato de dados do tipo contido na coleção.

Nos `CustomerList` exemplos, as coleções continham cadeias de caracteres. O nome do contrato de dados para o tipo primitivo da cadeia de caracteres é "String", portanto, o elemento repetitivo era " \<string> ".

No entanto, usando a <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ItemName%2A> propriedade no <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo, esse nome de elemento repetitivo pode ser personalizado. Para obter um exemplo, consulte o seguinte tipo.

[!code-csharp[c_collection_types_in_data_contracts#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#4)]
[!code-vb[c_collection_types_in_data_contracts#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#4)]

O XML resultante é semelhante ao seguinte.

```xml
<CustomerList4>
    <customer>...</customer>
    <customer>...</customer>
    <customer>...</customer>
    ...
</CustomerList4>
```

O namespace do elemento repetitivo é sempre o mesmo que o namespace do contrato de dados da coleção, que pode ser personalizado usando a `Namespace` propriedade, conforme descrito anteriormente.

### <a name="customizing-dictionary-collections"></a>Personalizando coleções de dicionário

As coleções de dicionário são essencialmente listas de entradas, em que cada entrada tem uma chave seguida por um valor. Assim como acontece com listas regulares, você pode alterar o nome do elemento que corresponde ao elemento repetitivo usando a <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ItemName%2A> propriedade.

Além disso, você pode alterar os nomes de elemento que representam a chave e o valor usando <xref:System.Runtime.Serialization.CollectionDataContractAttribute.KeyName%2A> as <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ValueName%2A> Propriedades e. Os namespaces desses elementos são iguais ao namespace do contrato de dados da coleção.

Para obter um exemplo, consulte o seguinte tipo.

[!code-csharp[c_collection_types_in_data_contracts#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#5)]
[!code-vb[c_collection_types_in_data_contracts#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#5)]

Quando serializado, o XML resultante é semelhante ao seguinte.

```xml
<CountriesOrRegionsWithCapitals>
    <entry>
        <countryorregion>USA</countryorregion>
        <capital>Washington</capital>
    </entry>
    <entry>
        <countryorregion>France</countryorregion>
        <capital>Paris</capital>
    </entry>
    ...
</CountriesOrRegionsWithCapitals>
```

Para obter mais informações sobre coleções de dicionário, consulte a seção "regras avançadas de coleta" mais adiante neste tópico.

## <a name="collections-and-known-types"></a>Coleções e tipos conhecidos

Não é necessário adicionar tipos de coleção a tipos conhecidos quando usados polimorficamente no lugar de outras coleções ou interfaces de coleção. Por exemplo, se você declarar um membro de dados do tipo <xref:System.Collections.IEnumerable> e usá-lo para enviar uma instância do <xref:System.Collections.ArrayList> , não será necessário adicionar <xref:System.Collections.ArrayList> a tipos conhecidos.

Quando você usa coleções polimorficamente em vez de tipos que não são de coleção, elas devem ser adicionadas a tipos conhecidos. Por exemplo, se você declarar um membro de dados do tipo `Object` e usá-lo para enviar uma instância do <xref:System.Collections.ArrayList> , adicione <xref:System.Collections.ArrayList> a tipos conhecidos.

Isso não permite que você Serialize nenhum polimorficamente de coleção equivalente. Por exemplo, quando você adiciona <xref:System.Collections.ArrayList> à lista de tipos conhecidos no exemplo anterior, isso não permite que você atribua a `Array of Object` classe, embora ela tenha um contrato de dados equivalente. Isso não é diferente do comportamento normal de tipos conhecidos na serialização para tipos que não são de coleção, mas é especialmente importante entender no caso de coleções, pois é muito comum que as coleções sejam equivalentes.

Durante a serialização, apenas um tipo pode ser conhecido em um determinado escopo para um determinado contrato de dados, e as coleções equivalentes têm os mesmos contratos de dados. Isso significa que, no exemplo anterior, você não pode adicionar ambos <xref:System.Collections.ArrayList> e `Array of Object` a tipos conhecidos no mesmo escopo. Novamente, isso é equivalente ao comportamento de tipos conhecidos para tipos que não são de coleção, mas é especialmente importante entender para coleções.

Tipos conhecidos também podem ser necessários para o conteúdo de coleções. Por exemplo, se uma <xref:System.Collections.ArrayList> realmente contiver instâncias de `Type1` e `Type2` , ambos os tipos devem ser adicionados a tipos conhecidos.

O exemplo a seguir mostra um grafo de objeto construído corretamente usando coleções e tipos conhecidos. O exemplo é um pouco forçado, porque em um aplicativo real normalmente você não definiria os seguintes membros de dados como `Object` e, portanto, não tem nenhum problema de tipo/polimorfismo conhecido.

[!code-csharp[c_collection_types_in_data_contracts#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#6)]
[!code-vb[c_collection_types_in_data_contracts#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#6)]

Na desserialização, se o tipo declarado for um tipo de coleção, o tipo declarado será instanciado, independentemente do tipo que foi realmente enviado. Se o tipo declarado for uma interface de coleção, o desserializador escolherá um tipo a ser instanciado sem considerar os tipos conhecidos.

Também na desserialização, se o tipo declarado não for um tipo de coleção, mas um tipo de coleção estiver sendo enviado, um tipo de coleção correspondente será retirada da lista de tipos conhecidos. É possível adicionar tipos de interface de coleção à lista de tipos conhecidos na desserialização. Nesse caso, o mecanismo de desserialização seleciona novamente um tipo a ser instanciado.

## <a name="collections-and-the-netdatacontractserializer-class"></a>Coleções e a classe NetDataContractSerializer

Quando a <xref:System.Runtime.Serialization.NetDataContractSerializer> classe está em uso, os tipos de coleção não personalizados (sem o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo) que não são matrizes perdem seu significado especial.

Os tipos de coleção não personalizados marcados com o <xref:System.SerializableAttribute> atributo ainda podem ser serializados pela <xref:System.Runtime.Serialization.NetDataContractSerializer> classe de acordo com o <xref:System.SerializableAttribute> atributo ou as <xref:System.Runtime.Serialization.ISerializable> regras de interface.

Tipos de coleção personalizados, interfaces de coleção e matrizes ainda são tratados como coleções, mesmo quando a <xref:System.Runtime.Serialization.NetDataContractSerializer> classe está em uso.

## <a name="collections-and-schema"></a>Coleções e esquema

Todas as coleções equivalentes têm a mesma representação no esquema XSD (linguagem de definição de esquema XML). Por isso, normalmente, você não obtém o mesmo tipo de coleção no código do cliente gerado como aquele no servidor. Por exemplo, o servidor pode usar um contrato de dados com um <xref:System.Collections.Generic.List%601> membro de dados de inteiro genérico, mas no código de cliente gerado, o mesmo membro de dados pode se tornar uma matriz de inteiros.

As coleções de dicionário são marcadas com uma anotação de esquema específica do WCF que indicam que são dicionários; caso contrário, eles são indistinguíveis de listas simples que contêm entradas com uma chave e um valor. Para obter uma descrição exata de como as coleções são representadas no esquema de contrato de dados, consulte [referência de esquema de contrato de dados](data-contract-schema-reference.md).

Por padrão, os tipos não são gerados para coleções não personalizadas no código importado. Os membros de dados dos tipos de coleção de lista são importados como matrizes e os membros de dados dos tipos de coleção de dicionário são importados como dicionário genérico.

No entanto, para coleções personalizadas, tipos separados são gerados, marcados com o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo. (Um tipo de coleção personalizado no esquema é aquele que não usa o namespace padrão, o nome, o nome do elemento repetitivo ou os nomes de elemento de chave/valor.) Esses tipos são tipos vazios que derivam de genérico <xref:System.Collections.Generic.List%601> para tipos de lista e dicionário genérico para tipos de dicionário.

Por exemplo, você pode ter os seguintes tipos no servidor.

[!code-csharp[c_collection_types_in_data_contracts#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#7)]
[!code-vb[c_collection_types_in_data_contracts#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#7)]

Quando o esquema é exportado e importado de volta, o código do cliente gerado é semelhante ao seguinte (os campos são mostrados em vez de propriedades para facilitar a leitura).

[!code-csharp[c_collection_types_in_data_contracts#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#8)]
[!code-vb[c_collection_types_in_data_contracts#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#8)]

Talvez você queira usar tipos diferentes no código gerado do que os padrões. Por exemplo, talvez você queira usar <xref:System.ComponentModel.BindingList%601> matrizes genéricas em vez de regulares para seus membros de dados para facilitar sua vinculação a componentes de interface do usuário.

Para escolher os tipos de coleção a serem gerados, passe uma lista de tipos de coleção que você deseja usar na <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A> Propriedade do <xref:System.Runtime.Serialization.ImportOptions> objeto ao importar o esquema. Esses tipos são chamados de *tipos de coleção referenciados*.

Quando tipos genéricos estão sendo referenciados, eles devem ser genéricos totalmente abertos ou genéricos totalmente fechados.

> [!NOTE]
> Ao usar a ferramenta de Svcutil.exe, essa referência pode ser realizada usando a opção de linha de comando **/collectionType** (forma abreviada: **/CT**). Tenha em mente que você também deve especificar o assembly para os tipos de coleção referenciados usando a opção **/Reference** (forma abreviada: **/r**). Se o tipo for genérico, ele deverá ser seguido por uma aspa posterior e o número de parâmetros genéricos. A aspa de fundo ( \` ) não deve ser confundida com o caractere de aspa simples ('). Você pode especificar vários tipos de coleção referenciados usando a opção **/collectionType** mais de uma vez.

Por exemplo, para fazer com que todas as listas sejam importadas como genéricas <xref:System.Collections.Generic.List%601> .

```console
svcutil.exe MyService.wsdl MyServiceSchema.xsd /r:C:\full_path_to_system_dll\System.dll /ct:System.Collections.Generic.List`1
```

Ao importar qualquer coleção, essa lista de tipos de coleção referenciados é verificada, e a melhor coleção de correspondência é usada se um for encontrado, seja como um tipo de membro de dados (para coleções não personalizadas) ou como um tipo base para derivar (para coleções personalizadas). Os dicionários são combinados somente em dicionários, enquanto as listas são combinadas em listas.

Por exemplo, se você adicionar o genérico <xref:System.ComponentModel.BindingList%601> e <xref:System.Collections.Hashtable> à lista de tipos referenciados, o código do cliente gerado para o exemplo anterior será semelhante ao seguinte.

[!code-csharp[c_collection_types_in_data_contracts#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#9)]
[!code-vb[c_collection_types_in_data_contracts#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#9)]

Você pode especificar tipos de interface de coleção como parte dos tipos de coleção referenciados, mas não pode especificar tipos de coleção inválidos (como aqueles sem nenhum `Add` método ou construtor público).

Um genérico fechado é considerado a melhor correspondência. (Tipos não genéricos são considerados equivalentes a genéricos fechados de `Object` ). Por exemplo, se os tipos genéricos <xref:System.Collections.Generic.List%601> de <xref:System.DateTime> , genérico <xref:System.ComponentModel.BindingList%601> (genérico aberto) e <xref:System.Collections.ArrayList> forem os tipos de coleção referenciados, o seguinte será gerado.

[!code-csharp[c_collection_types_in_data_contracts#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#10)]
[!code-vb[c_collection_types_in_data_contracts#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#10)]

Para coleções de lista, somente os casos na tabela a seguir têm suporte.

|Tipo referenciado|Interface implementada por tipo referenciado|Exemplo|Tipo tratado como:|
|---------------------|----------------------------------------------|-------------|----------------------|
|Genérico não genérico ou fechado (qualquer número de parâmetros)|Não genérico|`MyType : IList`<br /><br /> ou<br /><br /> `MyType<T> : IList`<br /><br /> em que T =`int`|Genérico fechado de `Object` (por exemplo, `IList<object>` )|
|Genérico não genérico ou fechado (qualquer número de parâmetros que não necessariamente correspondem ao tipo de coleção)|Genérico fechado|`MyType : IList<string>`<br /><br /> ou<br /><br /> `MyType<T> : IList<string>`em que T =`int`|Genérico fechado (por exemplo, `IList<string>` )|
|Genérico fechado com qualquer número de parâmetros|Abrir genérico usando qualquer um dos parâmetros do tipo|`MyType<T,U,V> : IList<U>`<br /><br /> em que T = `int` , U = `string` , V =`bool`|Genérico fechado (por exemplo, `IList<string>` )|
|Abrir genérico com um parâmetro|Abrir genérico usando o parâmetro do tipo|`MyType<T> : IList<T>`, T está aberto|Abrir genérico (por exemplo, `IList<T>` )|

Se um tipo implementa mais de uma interface de coleção de lista, as seguintes restrições se aplicam:

- Se o tipo implementar genérico <xref:System.Collections.Generic.IEnumerable%601> (ou suas interfaces derivadas) várias vezes para tipos diferentes, o tipo não será considerado um tipo de coleção referenciado válido e será ignorado. Isso é verdadeiro mesmo que algumas implementações sejam inválidas ou usem genéricos abertos. Por exemplo, um tipo que implementa generic <xref:System.Collections.Generic.IEnumerable%601> de `int` T e genérico <xref:System.Collections.Generic.IEnumerable%601> nunca seria usado como uma coleção referenciada de `int` ou qualquer outro tipo, independentemente de o tipo ter um `Add` método aceitando `int` ou um método que `Add` aceitasse um parâmetro do tipo T, ou ambos.

- Se o tipo implementar uma interface de coleção genérica, bem como <xref:System.Collections.IList> , o tipo nunca será usado como um tipo de coleção referenciado, a menos que a interface de coleção genérica seja um genérico fechado do tipo <xref:System.Object> .

Para coleções de dicionário, há suporte apenas para os casos na tabela a seguir.

|Tipo referenciado|Interface implementada por tipo referenciado|Exemplo|Tipo tratado como|
|---------------------|----------------------------------------------|-------------|---------------------|
|Genérico não genérico ou fechado (qualquer número de parâmetros)|<xref:System.Collections.IDictionary>|`MyType : IDictionary`<br /><br /> ou<br /><br /> `MyType<T> : IDictionary`em que T =`int`|Genérico fechado`IDictionary<object,object>`|
|Genérico fechado (qualquer número de parâmetros)|<xref:System.Collections.Generic.IDictionary%602>, fechado|`MyType<T> : IDictionary<string, bool>`em que T =`int`|Genérico fechado (por exemplo, `IDIctionary<string,bool>` )|
|Genérico fechado (qualquer número de parâmetros)|Genérico <xref:System.Collections.Generic.IDictionary%602> , uma das chaves ou do valor é fechado, o outro está aberto e usa um dos parâmetros do tipo|`MyType<T,U,V> : IDictionary<string,V>`em que T = `int` , U = `float` , V =`bool`<br /><br /> ou<br /><br /> `MyType<Z> : IDictionary<Z,bool>`em que Z =`string`|Genérico fechado (por exemplo, `IDictionary<string,bool>` )|
|Genérico fechado (qualquer número de parâmetros)|Genérico <xref:System.Collections.Generic.IDictionary%602> , a chave e o valor estão abertos e cada um deles usa um dos parâmetros do tipo|`MyType<T,U,V> : IDictionary<V,U>`em que T = `int` , U = `bool` , V =`string`|Genérico fechado (por exemplo, `IDictionary<string,bool>` )|
|Abrir genérico (dois parâmetros)|Genérico <xref:System.Collections.Generic.IDictionary%602> , aberto, usa ambos os parâmetros genéricos do tipo na ordem em que aparecem|`MyType<K,V> : IDictionary<K,V>`, K e V são abertos|Abrir genérico (por exemplo, `IDictionary<K,V>` )|

Se o tipo implementa <xref:System.Collections.IDictionary> e genérico <xref:System.Collections.Generic.IDictionary%602> , apenas genérico <xref:System.Collections.Generic.IDictionary%602> é considerado.

Não há suporte para a referência a tipos genéricos parciais.

Não são permitidas duplicatas, por exemplo, você não pode adicionar o genérico <xref:System.Collections.Generic.List%601> de `Integer` e a coleção genérica de `Integer` para <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A> , porque isso torna impossível determinar qual deles usar quando uma lista de inteiros é encontrada no esquema. Duplicatas serão detectadas somente se houver um tipo no esquema que expõe o problema de duplicatas. Por exemplo, se o esquema que está sendo importado não contiver listas de inteiros, ele terá permissão para ter tanto o genérico <xref:System.Collections.Generic.List%601> de `Integer` quanto a coleção genérica de `Integer` no <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A> , mas nenhum deles terá efeito.

## <a name="advanced-collection-rules"></a>Regras avançadas de coleta

### <a name="serializing-collections"></a>Serializando coleções

Veja a seguir uma lista de regras de coleta para serialização:

- A combinação de tipos de coleção (com coleções de coleções) é permitida. As matrizes denteadas são tratadas como coleções de coleções. Não há suporte para matrizes multidimensionais.

- Matrizes de bytes e matrizes <xref:System.Xml.XmlNode> são tipos de matriz especiais que são tratados como primitivos, não coleções. A serialização de uma matriz de resultados de bytes em um único elemento XML que contém um bloco de dados codificados em base64, em vez de um elemento separado para cada byte. Para obter mais informações sobre como uma matriz de <xref:System.Xml.XmlNode> é tratada, consulte [tipos XML e ADO.net em contratos de dados](xml-and-ado-net-types-in-data-contracts.md). É claro que esses tipos especiais podem participar de coleções: uma matriz de resultados de bytes em vários elementos XML, com cada um contendo uma parte de dados codificados em base64.

- Se o <xref:System.Runtime.Serialization.DataContractAttribute> atributo for aplicado a um tipo de coleção, o tipo será tratado como um tipo de contrato de dados regular, não como uma coleção.

- Se um tipo de coleção implementa a <xref:System.Xml.Serialization.IXmlSerializable> interface, as regras a seguir se aplicam, dado um tipo `myType:IList<string>, IXmlSerializable` :

  - Se o tipo declarado for `IList<string>` , o tipo será serializado como uma lista.

  - Se o tipo declarado for `myType` , ele será serializado como `IXmlSerializable` .

  - Se o tipo declarado for `IXmlSerializable` , ele será serializado como `IXmlSerializable` , mas somente se você adicionar `myType` à lista de tipos conhecidos.

- As coleções são serializadas e desserializadas usando os métodos mostrados na tabela a seguir.

|O tipo de coleção implementa|Métodos chamados na serialização|Métodos chamados na desserialização|
|--------------------------------|-----------------------------------------|-------------------------------------------|
|<xref:System.Collections.Generic.IDictionary%602>genérico|`get_Keys`, `get_Values`|Adição genérica|
|<xref:System.Collections.IDictionary>|`get_Keys`, `get_Values`|`Add`|
|<xref:System.Collections.Generic.IList%601>genérico|<xref:System.Collections.Generic.IList%601>Indexador genérico|Adição genérica|
|<xref:System.Collections.Generic.ICollection%601>genérico|Enumerador|Adição genérica|
|<xref:System.Collections.IList>|<xref:System.Collections.IList>Indexador|`Add`|
|<xref:System.Collections.Generic.IEnumerable%601>genérico|`GetEnumerator`|Um método não estático chamado `Add` que usa um parâmetro do tipo apropriado (o tipo do parâmetro genérico ou um de seus tipos base). Esse método deve existir para que o serializador trate um tipo de coleção como uma coleção durante a serialização e a desserialização.|
|<xref:System.Collections.IEnumerable>(e <xref:System.Collections.ICollection> , portanto, que deriva dele)|`GetEnumerator`|Um método não estático chamado `Add` que usa um parâmetro do tipo `Object` . Esse método deve existir para que o serializador trate um tipo de coleção como uma coleção durante a serialização e a desserialização.|

A tabela anterior lista as interfaces de coleção em ordem decrescente de precedência. Isso significa, por exemplo, que se um tipo implementa <xref:System.Collections.IList> e genérico <xref:System.Collections.Generic.IEnumerable%601> , a coleção é serializada e desserializada de acordo com as <xref:System.Collections.IList> regras:

- Na desserialização, todas as coleções são desserializadas primeiro criando uma instância do tipo chamando o construtor sem parâmetros, que deve estar presente para que o serializador trate um tipo de coleção como uma coleção durante a serialização e a desserialização.

- Se a mesma interface de coleção genérica for implementada mais de uma vez (por exemplo, se um tipo implementar generic <xref:System.Collections.Generic.ICollection%601> of `Integer` e generic <xref:System.Collections.Generic.ICollection%601> of <xref:System.String> ) e nenhuma interface de precedência mais alta for encontrada, a coleção não será tratada como uma coleção válida.

- Os tipos de coleção podem ter o <xref:System.SerializableAttribute> atributo aplicado a eles e podem implementar a <xref:System.Runtime.Serialization.ISerializable> interface. Ambos são ignorados. No entanto, se o tipo não atender totalmente aos requisitos de tipo de coleção (por exemplo, o `Add` método está ausente), o tipo não será considerado um tipo de coleção e, portanto, o <xref:System.SerializableAttribute> atributo e a <xref:System.Runtime.Serialization.ISerializable> interface serão usados para determinar se o tipo pode ser serializado.

- Aplicar o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo a uma coleção para personalizá-lo remove o <xref:System.SerializableAttribute> mecanismo de fallback anterior. Em vez disso, se uma coleção personalizada não atender aos requisitos de tipo de coleção, uma <xref:System.Runtime.Serialization.InvalidDataContractException> exceção será lançada. A cadeia de caracteres de exceção geralmente contém informações que explicam por que um determinado tipo não é considerado uma coleção válida (nenhum `Add` método, nenhum construtor sem parâmetros e assim por diante), portanto, geralmente é útil aplicar o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo para fins de depuração.

### <a name="collection-naming"></a>Nomenclatura de coleção

Veja a seguir uma lista de regras de nomenclatura de coleção:

- O namespace padrão para todos os contratos de dados de coleção de dicionário, bem como para os contratos de dados de coleção de lista que contêm tipos primitivos, é `http://schemas.microsoft.com/2003/10/Serialization/Arrays` a menos que seja substituído usando o namespace. Tipos que são mapeados para tipos XSD internos, bem como `char` tipos, `Timespan` e, `Guid` são considerados primitivos para essa finalidade.

- O namespace padrão para tipos de coleção que contêm tipos não primitivos, a menos que seja substituído usando o namespace, seja o mesmo que o namespace do contrato de dados do tipo contido na coleção.

- O nome padrão para contratos de dados de coleção de lista, a menos que seja substituído usando Name, é a cadeia de caracteres "ArrayOf" combinada com o nome do contrato de dados do tipo contido na coleção. Por exemplo, o nome do contrato de dados para uma lista genérica de inteiros é "ArrayOfint". Tenha em mente que o nome do contrato de dados `Object` é "anyType", portanto, o nome do contrato de dados de listas não genéricas como <xref:System.Collections.ArrayList> é "ArrayOfanyType".

O nome padrão para contratos de dados de coleção de dicionário, a menos que substituído usando `Name` , é a cadeia de caracteres "ArrayOfKeyValueOf" combinada com o nome do contrato de dados do tipo de chave seguido pelo nome do contrato de dados do tipo de valor. Por exemplo, o nome do contrato de dados para um dicionário genérico de cadeia de caracteres e inteiro é "ArrayOfKeyValueOfstringint". Além disso, se os tipos de chave ou de valor não forem tipos primitivos, um hash de namespace dos namespaces de contrato de dados dos tipos de chave e valor será anexado ao nome. Para obter mais informações sobre hashes de namespace, consulte [nomes de contrato de dados](data-contract-names.md).

Cada contrato de dados de coleção de dicionário tem um contrato de dados complementar que representa uma entrada no dicionário. Seu nome é o mesmo para o contrato de dados do dicionário, exceto para o prefixo "ArrayOf", e seu namespace é o mesmo para o contrato de dados do dicionário. Por exemplo, para o contrato de dados de dicionário "ArrayOfKeyValueOfstringint", o contrato de dados "KeyValueofstringint" representa uma entrada no dicionário. Você pode personalizar o nome desse contrato de dados usando a `ItemName` propriedade, conforme descrito na próxima seção.

Regras de nomenclatura de tipo genérico, conforme descrito em [nomes de contrato de dados](data-contract-names.md), se aplicam totalmente aos tipos de coleção; ou seja, você pode usar chaves dentro do nome para indicar parâmetros de tipo genérico. No entanto, os números dentro das chaves referem-se aos parâmetros genéricos e não aos tipos contidos na coleção.

## <a name="collection-customization"></a>Personalização da coleção

Os seguintes usos do <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo são proibidos e resultam em uma <xref:System.Runtime.Serialization.InvalidDataContractException> exceção:

- Aplicando o <xref:System.Runtime.Serialization.DataContractAttribute> atributo a um tipo ao qual o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo foi aplicado ou a um de seus tipos derivados.

- Aplicando o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo a um tipo que implementa a <xref:System.Xml.Serialization.IXmlSerializable> interface.

- Aplicando o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo a um tipo que não é de coleção.

- Tentativa de definir <xref:System.Runtime.Serialization.CollectionDataContractAttribute.KeyName%2A> ou <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ValueName%2A> em um <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo aplicado a um tipo que não é de dicionário.

### <a name="polymorphism-rules"></a>Regras de polimorfismo

Como mencionado anteriormente, a personalização de coleções usando o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo pode interferir na interalteração da coleção. Dois tipos de coleção personalizados só podem ser considerados equivalentes se o nome, o namespace, o nome do item, bem como os nomes de chave e valor (se forem coleções de dicionário) forem correspondentes.

Devido às personalizações, é possível usar inadvertidamente um contrato de dados de coleção em que outro é esperado. Isso deve ser evitado. Consulte os tipos a seguir.

[!code-csharp[c_collection_types_in_data_contracts#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#11)]
[!code-vb[c_collection_types_in_data_contracts#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#11)]

Nesse caso, uma instância do `Marks1` pode ser atribuída a `testMarks` . No entanto, `Marks2` não deve ser usado porque seu contrato de dados não é considerado equivalente ao `IList<int>` contrato de dados. O nome do contrato de dados é "Marks2" e não "ArrayOfint", e o nome do elemento repetitivo é " \<mark> " e não " \<int> ".

As regras na tabela a seguir se aplicam à atribuição polimórfica de coleções.

|Tipo declarado|Atribuindo uma coleção não personalizada|Atribuindo uma coleção personalizada|
|-------------------|--------------------------------------------|---------------------------------------|
|Objeto|O nome do contrato é serializado.|O nome do contrato é serializado.<br /><br /> A personalização é usada.|
|Interface da coleção|O nome do contrato não é serializado.|O nome do contrato não é serializado.<br /><br /> A personalização não é usada.\*|
|Coleção não personalizada|O nome do contrato não é serializado.|O nome do contrato é serializado.<br /><br /> A personalização é usada. * *|
|Coleção personalizada|O nome do contrato é serializado. A personalização não é usada.\*\*|O nome do contrato é serializado.<br /><br /> A personalização do tipo atribuído é usada.\*\*|

\*Com a <xref:System.Runtime.Serialization.NetDataContractSerializer> classe, a personalização é usada nesse caso. A <xref:System.Runtime.Serialization.NetDataContractSerializer> classe também serializa o nome do tipo real nesse caso; portanto, a desserialização funciona conforme o esperado.

\*\*Esses casos resultam em instâncias inválidas de esquema e, portanto, devem ser evitados.

Nos casos em que o nome do contrato é serializado, o tipo de coleção atribuído deve estar na lista tipos conhecidos. O oposto também é verdadeiro: nos casos em que o nome não é serializado, não é necessário adicionar o tipo à lista de tipos conhecidos.

Uma matriz de um tipo derivado pode ser atribuída a uma matriz de um tipo base. Nesse caso, o nome do contrato para o tipo derivado é serializado para cada elemento repetitivo. Por exemplo, se um tipo `Book` derivar do tipo `LibraryItem` , você poderá atribuir uma matriz de `Book` a uma matriz de `LibraryItem` . Isso não se aplica a outros tipos de coleção. Por exemplo, você não pode atribuir um `Generic List of Book` a um `Generic List of LibraryItem` . No entanto, você pode atribuir um `Generic List of LibraryItem` que contém `Book` instâncias. Na matriz e no caso não matriz, o `Book` deve estar na lista de tipos conhecidos.

## <a name="collections-and-object-reference-preservation"></a>Preservação de coleções e referência de objeto

Quando um serializador funciona em um modo onde preserva referências de objeto, a preservação de referência de objeto também se aplica a coleções. Especificamente, a identidade do objeto é preservada para todas as coleções e itens individuais contidos em coleções. Para dicionários, a identidade do objeto é preservada para os objetos de par chave/valor e os objetos chave e valor individuais.

## <a name="see-also"></a>Confira também

- <xref:System.Runtime.Serialization.CollectionDataContractAttribute>
