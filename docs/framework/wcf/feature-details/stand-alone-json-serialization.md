---
title: Serialização JSON autônoma usando DataContractJsonSerializer
ms.date: 03/30/2017
ms.assetid: 312bd7b2-1300-4b12-801e-ebe742bd2287
ms.openlocfilehash: 5561cddb22a02fdae9f792b1d1ec71d01c4fc916
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600900"
---
# <a name="stand-alone-json-serialization-using-datacontractjsonserializer"></a>Serialização JSON autônoma usando DataContractJsonSerializer

> [!NOTE]
> Este artigo é sobre <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> . Para a maioria dos cenários que envolvem a serialização e desserialização do JSON, recomendamos as APIs no [namespace System. Text. JSON](../../../standard/serialization/system-text-json-overview.md).

O JSON (JavaScript Object Notation) é um formato de dados projetado especificamente para ser usado pelo código JavaScript em execução em páginas da Web dentro do navegador. É o formato de dados padrão usado pelos serviços AJAX ASP.NET criados no Windows Communication Foundation (WCF).

Esse formato também pode ser usado ao criar serviços AJAX sem integração com ASP.NET-nesse caso, XML é o padrão, mas JSON pode ser escolhido.

Por fim, se você precisar de suporte a JSON, mas não estiver criando um serviço AJAX, o <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> tornará possível serializar diretamente objetos .net em dados JSON e desserializar esses dados de volta em instâncias de tipos .net. Para obter uma descrição de como fazer isso, consulte [como serializar e desserializar dados JSON](how-to-serialize-and-deserialize-json-data.md).

Ao trabalhar com JSON, os mesmos tipos .NET têm suporte, com algumas exceções, assim como têm suporte no <xref:System.Runtime.Serialization.DataContractSerializer> . Para obter uma lista dos tipos com suporte, consulte [tipos com suporte no serializador de contrato de dados](types-supported-by-the-data-contract-serializer.md). Isso inclui a maioria dos tipos primitivos, a maioria dos tipos de matriz e coleção, bem como tipos complexos que usam o <xref:System.Runtime.Serialization.DataContractAttribute> e o <xref:System.Runtime.Serialization.DataMemberAttribute> .

## <a name="mapping-net-types-to-json-types"></a>Mapeando tipos .NET para tipos JSON

A tabela a seguir mostra a correspondência entre tipos .NET e tipos JSON/JavaScript quando mapeados por procedimentos de serialização e desserialização.

|Tipos .NET|JSON/JavaScript|Observações|
|----------------|----------------------|-----------|
|Todos os tipos numéricos, por exemplo <xref:System.Int32> , <xref:System.Decimal> ou<xref:System.Double>|Número|Valores especiais, como `Double.NaN` , `Double.PositiveInfinity` e `Double.NegativeInfinity` não têm suporte e resultam em JSON inválido.|
|<xref:System.Enum>|Número|Consulte "enumerações e JSON", mais adiante neste tópico.|
|<xref:System.Boolean>|Boolean|--|
|<xref:System.String>, <xref:System.Char>|String|--|
|<xref:System.TimeSpan>, <xref:System.Guid>, <xref:System.Uri>|String|O formato desses tipos em JSON é o mesmo que em XML (essencialmente, TimeSpan no formato de duração ISO 8601, GUID no formato "12345678-ABCD-ABCD-ABCD-1234567890AB" e URI em seu formulário de cadeia de caracteres natural, como " http://www.example.com "). Para obter informações precisas, consulte [referência de esquema de contrato de dados](data-contract-schema-reference.md).|
|<xref:System.Xml.XmlQualifiedName>|String|O formato é "nome: namespace" (qualquer coisa antes do primeiro dois-pontos é o nome). O nome ou o namespace pode estar ausente. Se não houver nenhum namespace, os dois pontos também poderão ser omitidos.|
|<xref:System.Array>do tipo<xref:System.Byte>|Matriz de números|Cada número representa o valor de um byte.|
|<xref:System.DateTime>|DateTime ou cadeia de caracteres|Confira datas/horas e JSON mais adiante neste tópico.|
|<xref:System.DateTimeOffset>|Tipo complexo|Confira datas/horas e JSON mais adiante neste tópico.|
|Tipos XML e ADO.NET ( <xref:System.Xml.XmlElement> ,<br /><br /> <xref:System.Xml.Linq.XElement>. Matrizes de <xref:System.Xml.XmlNode> ,<br /><br /> <xref:System.Runtime.Serialization.ISerializable>,<br /><br /> <xref:System.Data.DataSet>).|String|Consulte a seção tipos de XML e JSON deste tópico.|
|<xref:System.DBNull>|Tipo complexo vazio|--|
|Coleções, dicionários e matrizes|Array|Consulte a seção coleções, dicionários e matrizes deste tópico.|
|Tipos complexos (com o <xref:System.Runtime.Serialization.DataContractAttribute> ou <xref:System.SerializableAttribute> aplicado)|Tipo complexo|Os membros de dados tornam-se membros do tipo complexo do JavaScript.|
|Tipos complexos que implementam a <xref:System.Runtime.Serialization.ISerializable> interface)|Tipo complexo|O mesmo que outros tipos complexos, mas <xref:System.Runtime.Serialization.ISerializable> não há suporte para alguns tipos – consulte a parte de suporte de ISerializable da seção informações avançadas deste tópico.|
|`Null`valor para qualquer tipo|Null|Também há suporte para tipos de valores anuláveis e mapear para JSON da mesma forma que os tipos de valor não anuláveis.|

### <a name="enumerations-and-json"></a>Enumerações e JSON

Os valores de membro de enumeração são tratados como números em JSON, que é diferente de como eles são tratados em contratos de dados, onde eles são incluídos como nomes de membros. Para obter mais informações sobre o tratamento de contrato de dados, consulte [tipos de enumeração em contratos de dados](enumeration-types-in-data-contracts.md).

- Por exemplo, se você tiver `public enum Color {red, green, blue, yellow, pink}` , a serialização `yellow` produzirá o número 3 e não a cadeia de caracteres "amarelo".

- Todos os `enum` Membros são serializáveis. O <xref:System.Runtime.Serialization.EnumMemberAttribute> e os <xref:System.NonSerializedAttribute> atributos serão ignorados se forem usados.

- É possível desserializar um valor inexistente `enum` , por exemplo, o valor 87 pode ser desserializado no enum de cor anterior, embora não haja nenhum nome de cor correspondente definido.

- Um sinalizador `enum` não é especial e é tratado da mesma forma que qualquer outro `enum` .

### <a name="datestimes-and-json"></a>Datas/horas e JSON

O formato JSON não dá suporte direto a datas e horários. No entanto, eles são muito usados e o ASP.NET AJAX fornece suporte especial para esses tipos. Ao usar proxies AJAX ASP.NET, o <xref:System.DateTime> tipo no .NET corresponde totalmente ao `DateTime` tipo em JavaScript.

- Quando não estiver usando ASP.NET, um <xref:System.DateTime> tipo será representado em JSON como uma cadeia de caracteres com um formato especial descrito na seção informações avançadas deste tópico.

- <xref:System.DateTimeOffset>é representado em JSON como um tipo complexo: {"DateTime":d ateTime, "OffsetMinutes": offsetMinutes}. O `offsetMinutes` membro é o deslocamento de hora local do GMT (tempo médio de Greenwich), também conhecido como UTC (tempo Universal Coordenado), associado ao local do evento de interesse. O `dateTime` membro representa a instância no tempo em que o evento de interesse ocorreu (novamente, se torna um `DateTime` em JavaScript quando o ASP.NET AJAX está em uso e uma cadeia de caracteres quando não está). Na serialização, o `dateTime` membro é sempre serializado em GMT. Portanto, se descrever 3:00 AM New York time, `dateTime` o tem um componente de tempo de 8:00 am e `offsetMinutes` é 300 (menos de 300 minutos ou 5 horas a partir de GMT).

  > [!NOTE]
  > <xref:System.DateTime>e os <xref:System.DateTimeOffset> objetos, quando serializados para JSON, só preservam as informações para a precisão de milissegundos. Valores de submilissegundos (micro/nanossegundos) são perdidos durante a serialização.

### <a name="xml-types-and-json"></a>Tipos XML e JSON

Os tipos XML se tornam cadeias de caracteres JSON.

- Por exemplo, se um membro de dados "q" do tipo XElement contiver \<abc/> , o JSON será {"q": " \<abc/> "}.

- Há algumas regras especiais que especificam como o XML é encapsulado-para obter mais informações, consulte a seção informações avançadas mais adiante neste tópico.

- Se você estiver usando o ASP.NET AJAX e não quiser usar cadeias de caracteres no JavaScript, mas quiser o DOM XML, defina a <xref:System.ServiceModel.Web.WebGetAttribute.ResponseFormat%2A> propriedade como XML em <xref:System.ServiceModel.Web.WebGetAttribute> ou a <xref:System.ServiceModel.Web.WebInvokeAttribute.ResponseFormat%2A> propriedade como XML no <xref:System.ServiceModel.Web.WebInvokeAttribute> .

### <a name="collections-dictionaries-and-arrays"></a>Coleções, dicionários e matrizes

Todas as coleções, dicionários e matrizes são representadas em JSON como matrizes.

- Qualquer personalização que usa o <xref:System.Runtime.Serialization.CollectionDataContractAttribute> é ignorada na representação JSON.

- Os dicionários não são uma maneira de trabalhar diretamente com o JSON. \<string,object>O dicionário pode não ter suporte da mesma maneira no WCF, conforme esperado, para trabalhar com outras tecnologias JSON. Por exemplo, se "ABC" for mapeado para "XYZ" e "def" for mapeado para 42 em um dicionário, a representação JSON não é {"ABC": "XYZ", "def": 42} Mas é [{"Key": "ABC", "value": "XYZ"}, {"Key": "def", "value": 42}] em vez disso.

- Se você quiser trabalhar com o JSON diretamente (acessando chaves e valores dinamicamente, sem definir previamente um contrato rígido), terá várias opções:

  - Considere usar o exemplo de [SERIALIZAÇÃO JSON (Ajax) de tipo fraco](../samples/weakly-typed-json-serialization-sample.md) .

  - Considere usar os <xref:System.Runtime.Serialization.ISerializable> construtores de interface e desserialização-esses dois mecanismos permitem acessar pares de chave/valor JSON na serialização e desserialização, respectivamente, mas não funcionam em cenários de confiança parcial.

  - Considere trabalhar com o [mapeamento entre JSON e XML](mapping-between-json-and-xml.md) em vez de usar um serializador.

  - *Polimorfismo* no contexto de serialização refere-se à capacidade de serializar um tipo derivado em que seu tipo base é esperado. Há regras especiais específicas de JSON ao usar coleções polimorficamente, quando, por exemplo, atribuir uma coleção a um <xref:System.Object> . Esse problema é discutido mais detalhadamente na seção informações avançadas, mais adiante neste tópico.

## <a name="additional-details"></a>Detalhes adicionais

### <a name="order-of-data-members"></a>Ordem dos membros de dados

A ordem dos membros de dados não é importante ao usar o JSON. Especificamente, mesmo se <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A> estiver definido, os dados JSON ainda poderão ser desserializados em qualquer ordem.

### <a name="json-types"></a>Tipos JSON

O tipo JSON não precisa corresponder à tabela anterior na desserialização. Por exemplo, um `Int` normalmente é mapeado para um número JSON, mas ele também pode ser desserializado com êxito de uma cadeia de caracteres JSON, desde que essa cadeia de caracteres contenha um número válido. Ou seja, {"q": 42} e {"q": "42"} serão válidos se houver um membro de `Int` dados chamado "q".

### <a name="polymorphism"></a>Polimorfismo

A serialização polimórfica consiste na capacidade de serializar um tipo derivado em que seu tipo base é esperado. Isso tem suporte para serialização JSON pelo WCF comparável à maneira como a serialização de XML tem suporte. Por exemplo, você pode serializar `MyDerivedType` onde `MyBaseType` é esperado ou serializar `Int` onde `Object` é esperado.

Informações de tipo podem ser perdidas ao desserializar um tipo derivado se o tipo base for esperado, a menos que você esteja desserializando um tipo complexo. Por exemplo, se <xref:System.Uri> for serializado em que <xref:System.Object> é esperado, ele resultará em uma cadeia de caracteres JSON. Se essa cadeia de caracteres for desserializada novamente no <xref:System.Object> , um .NET <xref:System.String> será retornado. O desserializador não sabe que a cadeia de caracteres era inicialmente do tipo <xref:System.Uri> . Em geral, quando esperado <xref:System.Object> , todas as cadeias de caracteres JSON são desserializadas como cadeias de caracteres .net, e todas as matrizes JSON usadas para serializar coleções, dicionários e matrizes do .NET são desserializadas como .NET <xref:System.Array> do tipo <xref:System.Object> , independentemente do tipo original real. Um booliano JSON é mapeado para um .NET <xref:System.Boolean> . No entanto, ao esperar um <xref:System.Object> , os números JSON são desserializados como <xref:System.Int32> <xref:System.Decimal> o .net ou <xref:System.Double> , onde o tipo mais apropriado é escolhido automaticamente.

Ao desserializar em um tipo de interface, o <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> desserializa como se o tipo declarado fosse Object.

Ao trabalhar com seus próprios tipos base e derivados, usar o <xref:System.Runtime.Serialization.KnownTypeAttribute> , <xref:System.ServiceModel.ServiceKnownTypeAttribute> ou um mecanismo equivalente normalmente é necessário. Por exemplo, se você tiver uma operação que tenha um `Animal` valor de retorno e, na verdade, retornar uma instância de `Cat` (derivada de `Animal` ), deverá aplicar o <xref:System.Runtime.Serialization.KnownTypeAttribute> , ao `Animal` tipo ou à <xref:System.ServiceModel.ServiceKnownTypeAttribute> operação e especificar o `Cat` tipo nesses atributos. Para obter mais informações, consulte [tipos conhecidos de contrato de dados](data-contract-known-types.md).

Para obter detalhes de como a serialização polimórfica funciona e uma discussão sobre algumas das limitações que devem ser respeitadas ao usá-la, consulte a seção informações avançadas mais adiante neste tópico.

### <a name="versioning"></a>Controle de versão

Os recursos de controle de versão do contrato de dados, incluindo a <xref:System.Runtime.Serialization.IExtensibleDataObject> interface, têm suporte total em JSON. Além disso, na maioria dos casos, é possível desserializar um tipo em um formato (por exemplo, XML) e, em seguida, serializá-lo em outro formato (por exemplo, JSON) e ainda preservar os dados no <xref:System.Runtime.Serialization.IExtensibleDataObject> . Para obter mais informações, consulte [Contratos de dados compatíveis por encaminhamento](forward-compatible-data-contracts.md). Lembre-se de que JSON não é ordenado, portanto, qualquer informação de pedido é perdida. Além disso, o JSON não dá suporte a vários pares de chave/valor com o mesmo nome de chave. Por fim, todas as operações em <xref:System.Runtime.Serialization.IExtensibleDataObject> são inerentemente polimórficas – que é o tipo derivado atribuído a <xref:System.Object> , o tipo base para todos os tipos.

## <a name="json-in-urls"></a>JSON em URLs

Ao usar pontos de extremidade do ASP.NET AJAX com o verbo HTTP GET (usando o <xref:System.ServiceModel.Web.WebGetAttribute> atributo), os parâmetros de entrada aparecem na URL da solicitação em vez do corpo da mensagem. O JSON tem suporte mesmo na URL de solicitação, portanto, se você tiver uma operação que usa um `Int` "número" chamado e um `Person` tipo complexo chamado "p", a URL poderá ser semelhante à URL a seguir.

```html
http://example.com/myservice.svc/MyOperation?number=7&p={"name":"John","age":42}
```

Se você estiver usando um controle e proxy do Gerenciador de script do ASP.NET AJAX para chamar o serviço, essa URL será gerada automaticamente pelo proxy e não será vista. JSON não pode ser usado em URLs em pontos de extremidade do non-ASP.NET AJAX.

## <a name="advanced-information"></a>Informações avançadas

### <a name="iserializable-support"></a>Suporte a ISerializable

#### <a name="supported-and-unsupported-iserializable-types"></a>Tipos ISerializable com e sem suporte

Em geral, os tipos que implementam a <xref:System.Runtime.Serialization.ISerializable> interface têm suporte total ao serializar/desserializar JSON. No entanto, alguns desses tipos (incluindo alguns tipos de .NET Framework) são implementados de forma que os aspectos de serialização específicos do JSON façam com que eles não sejam desserializados corretamente:

- Com <xref:System.Runtime.Serialization.ISerializable> o, o tipo de membros de dados individuais nunca é conhecido com antecedência. Isso leva a uma situação polimórfica semelhante à desserialização de tipos em um objeto. Como mencionado anteriormente, isso pode levar à perda de informações de tipo em JSON. Por exemplo, um tipo que serializa um `enum` em sua <xref:System.Runtime.Serialization.ISerializable> implementação e tenta desserializar diretamente em um `enum` (sem as conversões apropriadas) falha, porque um `enum` é SERIALIZADO usando números em JSON e números JSON desserializando em tipos numéricos .net internos (Int32, decimal ou Double). Portanto, o fato de que o número usado para ser um `enum` valor é perdido.

- Um <xref:System.Runtime.Serialization.ISerializable> tipo que depende de uma determinada ordem de desserialização em seu construtor de desserialização também pode falhar ao desserializar alguns dados JSON, pois a maioria dos serializadores JSON não garante nenhum pedido específico.

#### <a name="factory-types"></a>Tipos de fábrica

Embora a <xref:System.Runtime.Serialization.IObjectReference> interface tenha suporte em JSON em geral, todos os tipos que exigem o recurso de "tipo de fábrica" (retornando uma instância de um tipo diferente do <xref:System.Runtime.Serialization.IObjectReference.GetRealObject%28System.Runtime.Serialization.StreamingContext%29> tipo que implementa a interface) não têm suporte.

### <a name="datetime-wire-format"></a>Formato de fio de DateTime

<xref:System.DateTime>os valores aparecem como cadeias de caracteres JSON na forma de "/Date (700000 + 0500)/", em que o primeiro número (700000 no exemplo fornecido) é o número de milissegundos no fuso horário GMT, tempo normal (economia que não é de verão) desde a meia-noite, 1º de janeiro de 1970. O número pode ser negativo para representar os horários anteriores. A parte que consiste em "+ 0500" no exemplo é opcional e indica que a hora é do <xref:System.DateTimeKind.Local> tipo-ou seja, deve ser convertida no fuso horário local na desserialização. Se estiver ausente, o tempo será desserializado como <xref:System.DateTimeKind.Utc> . O número real ("0500" neste exemplo) e seu sinal (+ ou-) são ignorados.

Ao serializar <xref:System.DateTime> , as <xref:System.DateTimeKind.Local> <xref:System.DateTimeKind.Unspecified> horas são escritas com um deslocamento e <xref:System.DateTimeKind.Utc> são escritas sem.

O código JavaScript do cliente ASP.NET AJAX converte automaticamente essas cadeias de caracteres em instâncias de JavaScript `DateTime` . Se houver outras cadeias de caracteres que tenham um formato semelhante que não seja do tipo <xref:System.DateTime> no .net, elas também serão convertidas.

A conversão só ocorrerá se os caracteres "/" tiverem escape (ou seja, o JSON for semelhante a " \\ /Date (700000 + 0500) \\ /") e, por esse motivo, o codificador JSON do WCF (habilitado pelo <xref:System.ServiceModel.WebHttpBinding> ) sempre irá escapar do caractere "/".

### <a name="xml-in-json-strings"></a>XML em cadeias de caracteres JSON

#### <a name="xmlelement"></a>XmlElement

<xref:System.Xml.XmlElement>é serializado como está, sem quebra automática. Por exemplo, o membro de dados "x" do tipo <xref:System.Xml.XmlElement> que contém \<abc/> é representado da seguinte maneira:

```json
{"x":"<abc/>"}
```

#### <a name="arrays-of-xmlnode"></a>Matrizes de XmlNode

<xref:System.Array>objetos do tipo <xref:System.Xml.XmlNode> são encapsulados em um elemento chamado ArrayOfXmlNode no namespace de contrato de dados padrão para o tipo. Se "x" for uma matriz que contém o nó de atributo "N" no namespace "NS" que contém "value" e um nó de elemento vazio "M", a representação será a seguinte.

```json
{"x":"<ArrayOfXmlNode xmlns=\"http://schemas.datacontract.org/2004/07/System.Xml\" a:N=\"value\" xmlns:a=\"ns\"><M/></ArrayOfXmlNode>"}
```

 Não há suporte para atributos no namespace vazio no início de matrizes XmlNode (antes de outros elementos).

#### <a name="ixmlserializable-types-including-xelement-and-dataset"></a>Tipos IXmlSerializable, incluindo XElement e DataSet

<xref:System.Runtime.Serialization.ISerializable>os tipos subdividam em "tipos de conteúdo", "tipos de conjunto de conjuntos" e "tipos de elemento". Para obter definições desses tipos, consulte [tipos XML e ADO.net em contratos de dados](xml-and-ado-net-types-in-data-contracts.md).

Os tipos "content" e "DataSet" são serializados semelhante aos <xref:System.Array> objetos de <xref:System.Xml.XmlNode> discutidos na seção anterior. Eles são encapsulados em um elemento cujo nome e namespace correspondem ao nome do contrato de dados e ao namespace do tipo em questão.

Tipos de "elemento", como <xref:System.Xml.Linq.XElement> , são serializados como estão, semelhantes aos <xref:System.Xml.XmlElement> discutidos anteriormente neste tópico.

### <a name="polymorphism"></a>Polimorfismo

#### <a name="preserving-type-information"></a>Preservando informações de tipo

Como mencionado anteriormente, o polimorfismo tem suporte em JSON com algumas limitações. O JavaScript é uma linguagem de tipo fraco e a identidade de tipos normalmente não é um problema. No entanto, ao usar o JSON para se comunicar entre um sistema com rigidez de tipos (.NET) e um sistema de tipo fraco (JavaScript), é útil preservar a identidade do tipo. Por exemplo, tipos com nomes de contrato de dados "Square" e "Circle" derivam de um tipo com um nome de contrato de dados de "Shape". Se "Circle" for enviado do .NET para o JavaScript e, posteriormente, for retornado a um método .NET que espera "Shape", será útil para o lado do .NET saber que o objeto em questão foi originalmente um "Circle". caso contrário, qualquer informação específica ao tipo derivado (por exemplo, "RADIUS" de dados "de raio" em "Circle") poderá ser perdida.

Para preservar a identidade do tipo, ao serializar tipos complexos para JSON, é possível adicionar uma "dica de tipo", e o desserializador reconhece a dica e age adequadamente. A "dica de tipo" é um par de chave/valor JSON com o nome de chave " \_ \_ Type" (dois sublinhados seguidos da palavra "Type"). O valor é uma cadeia de caracteres JSON no formato "datacontractname: DataContractNamespace" (algo até o primeiro dois pontos é o nome). Usando o exemplo anterior, "Circle" pode ser serializado da seguinte maneira.

```json
{"__type":"Circle:http://example.com/myNamespace","x":50,"y":70,"radius":10}
```

A dica de tipo é muito semelhante ao `xsi:type` atributo definido pelo padrão de instância de esquema XML e usado ao serializar/desserializar XML.

Os membros de dados chamados " \_ \_ Type" são proibidos devido ao conflito em potencial com a dica de tipo.

#### <a name="reducing-the-size-of-type-hints"></a>Reduzindo o tamanho das dicas de tipo

Para reduzir o tamanho das mensagens JSON, o prefixo do namespace do contrato de dados padrão ( `http://schemas.datacontract.org/2004/07/` ) é substituído pelo caractere "#". (Para fazer essa substituição reversível, uma regra de escape é usada: se o namespace começar com os caracteres "#" ou " \\ ", eles serão anexados com um \\ caractere "" extra). Portanto, se "Circle" for um tipo no namespace .NET "MyApp. Shapes", seu namespace de contrato de dados padrão é `http://schemas.datacontract.org/2004/07/MyApp` . As formas e a representação JSON são as seguintes.

```json
{"__type":"Circle:#MyApp.Shapes","x":50,"y":70,"radius":10}
```

Tanto o truncado (#MyApp. Shapes) quanto os nomes Full ( <http://schemas.datacontract.org/2004/07/MyApp.Shapes> ) são compreendidos na desserialização.

#### <a name="type-hint-position-in-json-objects"></a>Posição de dica de tipo em objetos JSON

Observe que a dica de tipo deve aparecer primeiro na representação JSON. Esse é o único caso em que a ordem dos pares chave/valor é importante no processamento JSON. Por exemplo, a seguir não é uma maneira válida de especificar a dica de tipo.

```json
{"x":50,"y":70,"radius":10,"__type":"Circle:#MyApp.Shapes"}
```

As <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> páginas de cliente usadas pelo WCF e pelo ASP.NET AJAX sempre emitem a dica de tipo primeiro.

#### <a name="type-hints-apply-only-to-complex-types"></a>Dicas de tipo aplicam-se somente a tipos complexos

Não é possível emitir uma dica de tipo para tipos não complexos. Por exemplo, se uma operação tiver um <xref:System.Object> tipo de retorno, mas retornar um círculo, a representação JSON poderá ser conforme mostrado anteriormente e as informações de tipo serão preservadas. No entanto, se o URI for retornado, a representação JSON será uma cadeia de caracteres e o fato de que a cadeia de caracteres usada para representar um URI será perdida. Isso se aplica não apenas a tipos primitivos, mas também a coleções e matrizes.

#### <a name="when-are-type-hints-emitted"></a>Quando as dicas de tipo são emitidas

Dicas de tipo podem aumentar significativamente o tamanho da mensagem (uma maneira de mitigar isso é usar namespaces de contrato de dados menores, se possível). Portanto, as regras a seguir regem se as dicas de tipo são emitidas:

- Ao usar o ASP.NET AJAX, as dicas de tipo sempre são emitidas sempre que possível, mesmo que não haja nenhuma atribuição base/derivada, por exemplo, mesmo se um círculo for atribuído a um círculo. (Isso é necessário para habilitar totalmente o processo de chamada do ambiente JSON de tipo fraco para o ambiente .NET fortemente tipado sem perda surpreendente de informações.)

- Ao usar serviços AJAX sem integração ASP.NET, as dicas de tipo são emitidas somente quando há uma atribuição base/derivada, ou seja, emitida quando o círculo é atribuído à forma ou <xref:System.Object> , mas não quando atribuído a um círculo. Isso fornece as informações mínimas necessárias para implementar corretamente um cliente JavaScript, melhorando assim o desempenho, mas não protege contra perda de informações de tipo em clientes incorretamente projetados. Evite as atribuições de base/derivadas totalmente no servidor se desejar evitar lidar com esse problema no cliente.

- Ao usar o <xref:System.Runtime.Serialization.DataContractSerializer> tipo, o `alwaysEmitTypeInformation` parâmetro do construtor permite que você escolha entre os dois modos anteriores, com o padrão sendo " `false` " (apenas as dicas de tipo de emissão quando necessário).

#### <a name="duplicate-data-member-names"></a>Nomes de membros de dados duplicados

As informações de tipo derivado estão presentes no mesmo objeto JSON junto com informações de tipo base e podem ocorrer em qualquer ordem. Por exemplo, `Shape` pode ser representado da seguinte maneira.

```json
{"__type":"Shape:#MyApp.Shapes","x":50,"y":70}
```

Enquanto o círculo pode ser representado da seguinte maneira.

```json
{"__type":"Circle:#MyApp.Shapes","x":50, "radius":10,"y":70}
```

Se o `Shape` tipo base também contivesse um membro de dados chamado " `radius` ", isso leva a uma colisão na serialização (porque os objetos JSON não podem ter nomes de chave repetidos) e a desserialização (porque ele não está claro se "RADIUS" refere-se a `Shape.radius` ou `Circle.radius` ). Portanto, embora o conceito de "ocultação de propriedade" (membros de dados de mesmo nome em classes derivadas e baseadas) geralmente não seja recomendado em classes de contrato de dados, na verdade é proibido no caso do JSON.

#### <a name="polymorphism-and-ixmlserializable-types"></a>Tipos polimorfismo e IXmlSerializable

<xref:System.Xml.Serialization.IXmlSerializable>os tipos podem ser polimorficamente atribuídos entre si como de costume, desde que os requisitos de tipos conhecidos sejam atendidos, de acordo com as regras de contrato de dados usuais. No entanto, serializar um <xref:System.Xml.Serialization.IXmlSerializable> tipo no lugar de <xref:System.Object> resultados em perda de informações de tipo como resultado é uma cadeia de caracteres JSON.

#### <a name="polymorphism-and-certain-interface-types"></a>Polimorfismo e determinados tipos de interface

É proibido serializar um tipo de coleção ou um tipo que implementa <xref:System.Xml.Serialization.IXmlSerializable> onde um tipo de não coleção que não é <xref:System.Xml.Serialization.IXmlSerializable> (exceto para <xref:System.Object> ) é esperado. Por exemplo, uma interface personalizada chamada `IMyInterface` e um tipo `MyType` que implementam ambos os <xref:System.Collections.Generic.IEnumerable%601> tipos `int` e `IMyInterface` . É proibido retornar `MyType` de uma operação cujo tipo de retorno é `IMyInterface` . Isso ocorre porque o `MyType` deve ser serializado como uma matriz JSON e requer uma dica de tipo e, conforme declarado antes que você não possa incluir uma dica de tipo com matrizes, somente com tipos complexos.

#### <a name="known-types-and-configuration"></a>Tipos e configurações conhecidos

Todos os mecanismos de tipo conhecidos usados pelo <xref:System.Runtime.Serialization.DataContractSerializer> também têm suporte da mesma forma pelo <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> . Ambos os serializadores lêem o mesmo elemento de configuração, [\<dataContractSerializer>](../../configure-apps/file-schema/wcf/datacontractserializer-of-system-runtime-serialization.md) no [\<system.runtime.serialization>](../../configure-apps/file-schema/wcf/system-runtime-serialization.md) , para descobrir tipos conhecidos adicionados por meio de um arquivo de configuração.

#### <a name="collections-assigned-to-object"></a>Coleções atribuídas ao objeto

As coleções atribuídas ao objeto são serializadas como se fossem coleções que implementam <xref:System.Collections.Generic.IEnumerable%601> : uma matriz JSON com cada entrada que tenha uma dica de tipo, se for um tipo complexo. Por exemplo, um <xref:System.Collections.Generic.List%601> do tipo `Shape` atribuído <xref:System.Object> é semelhante ao seguinte.

```json
[{"__type":"Shape:#MyApp.Shapes","x":50,"y":70},
{"__type":"Shape:#MyApp.Shapes","x":58,"y":73},
{"__type":"Shape:#MyApp.Shapes","x":41,"y":32}]
```

Quando desserializado de volta em <xref:System.Object> :

- `Shape`deve estar na lista de tipos conhecidos. Ter <xref:System.Collections.Generic.List%601> do tipo `Shape` em tipos conhecidos não tem nenhum efeito. Observe que você não precisa adicionar `Shape` a tipos conhecidos na serialização, nesse caso, isso é feito automaticamente.

- A coleção é desserializada como um <xref:System.Array> do tipo <xref:System.Object> que contém `Shape` instâncias.

#### <a name="derived-collections-assigned-to-base-collections"></a>Coleções derivadas atribuídas a coleções base

Quando uma coleção derivada é atribuída a uma coleção de base, a coleção geralmente é serializada como se fosse uma coleção do tipo base. No entanto, se o tipo de item da coleção derivada não puder ser atribuído ao tipo de item da coleção base, uma exceção será lançada.

#### <a name="type-hints-and-dictionaries"></a>Dicas de tipo e dicionários

Quando um dicionário é atribuído a um <xref:System.Object> , cada entrada de chave e valor no dicionário é tratada como se ele fosse atribuído a <xref:System.Object> e obtém uma dica de tipo.

Ao serializar tipos de dicionário, o objeto JSON que contém os membros de "chave" e "valor" não é afetado pela `alwaysEmitTypeInformation` configuração e contém apenas uma dica de tipo quando as regras de coleção anteriores exigem isso.

### <a name="valid-json-key-names"></a>Nomes de chave JSON válidos

O serializador XML-codifica nomes de chave que não são nomes XML válidos. Por exemplo, um membro de dados com o nome "123" teria um nome codificado, como " \_ x0031 \_ \_ x0032 \_ \_ x0033 \_ " porque "123" é um nome de elemento XML inválido (começa com um dígito). Uma situação semelhante pode surgir com alguns conjuntos de caracteres internacionais não válidos em nomes XML. Para obter uma explicação desse efeito de XML no processamento JSON, consulte [mapeamento entre JSON e XML](mapping-between-json-and-xml.md).

## <a name="see-also"></a>Consulte também

- [Suporte para JSON e outros formatos de transferência de dados](support-for-json-and-other-data-transfer-formats.md)
