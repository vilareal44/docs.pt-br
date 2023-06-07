---
title: Usando a classe XmlSerializer
description: Saiba mais sobre o XmlSerializer, que o WCF usa para serializar os dados em seu aplicativo em XML que é transmitido entre clientes e serviços.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XmlSerializer [WCF], using
ms.assetid: c680602d-39d3-44f1-bf22-8e6654ad5069
ms.openlocfilehash: f7473de3f34ba543b4fabfe93167ea267f16dda5
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246370"
---
# <a name="using-the-xmlserializer-class"></a>Usando a classe XmlSerializer

O Windows Communication Foundation (WCF) pode usar duas tecnologias de serialização diferentes para transformar os dados em seu aplicativo em XML que é transmitido entre clientes e serviços, um processo chamado serialização.

## <a name="datacontractserializer-as-the-default"></a>DataContractSerializer como o padrão

Por padrão, o WCF usa a <xref:System.Runtime.Serialization.DataContractSerializer> classe para serializar tipos de dados. Esse serializador dá suporte aos seguintes tipos:

- Tipos primitivos (por exemplo, inteiros, cadeias de caracteres e matrizes de bytes), bem como alguns tipos especiais, como <xref:System.Xml.XmlElement> e <xref:System.DateTime> , que são tratados como primitivos.

- Tipos de contrato de dados (tipos marcados com o <xref:System.Runtime.Serialization.DataContractAttribute> atributo).

- Tipos marcados com o <xref:System.SerializableAttribute> atributo, que incluem tipos que implementam a <xref:System.Runtime.Serialization.ISerializable> interface.

- Tipos que implementam a <xref:System.Xml.Serialization.IXmlSerializable> interface.

- Muitos tipos de coleção comuns, que incluem muitos tipos de coleção genéricos.

Muitos tipos de .NET Framework se enquadram nas duas últimas categorias e, portanto, são serializáveis. Matrizes de tipos serializáveis também são serializáveis. Para obter uma lista completa, consulte [especificando transferência de dados em contratos de serviço](specifying-data-transfer-in-service-contracts.md).

O <xref:System.Runtime.Serialization.DataContractSerializer> , usado junto com os tipos de contrato de dados, é a maneira recomendada para escrever novos serviços WCF. Para obter mais informações, consulte [usando contratos de dados](using-data-contracts.md).

## <a name="when-to-use-the-xmlserializer-class"></a>Quando usar a classe XmlSerializer

O WCF também dá suporte à <xref:System.Xml.Serialization.XmlSerializer> classe. A <xref:System.Xml.Serialization.XmlSerializer> classe não é exclusiva para o WCF. É o mesmo mecanismo de serialização que o ASP.NET Web Services usa. A <xref:System.Xml.Serialization.XmlSerializer> classe dá suporte a um conjunto muito mais estreito de tipos do que a <xref:System.Runtime.Serialization.DataContractSerializer> classe, mas permite muito mais controle sobre o XML resultante e dá suporte a muito mais do padrão XSD (linguagem de definição de esquema XML). Ele também não requer nenhum atributo declarativo nos tipos serializáveis. Para obter mais informações, consulte o tópico serialização de XML na documentação do .NET Framework. A <xref:System.Xml.Serialization.XmlSerializer> classe não oferece suporte a tipos de contrato de dados.

Ao usar Svcutil.exe ou o recurso de **Adicionar referência de serviço** no Visual Studio para gerar o código do cliente para um serviço de terceiros ou para acessar um esquema de terceiros, um serializador apropriado é selecionado automaticamente para você. Se o esquema não for compatível com o <xref:System.Runtime.Serialization.DataContractSerializer> , o <xref:System.Xml.Serialization.XmlSerializer> será selecionado.

## <a name="manually-switching-to-the-xmlserializer"></a>Alternando manualmente para o XmlSerializer

Às vezes, pode ser necessário alternar manualmente para o <xref:System.Xml.Serialization.XmlSerializer> . Isso acontece, por exemplo, nos seguintes casos:

- Ao migrar um aplicativo do ASP.NET Web Services para o WCF, talvez você queira reutilizar os tipos existentes e <xref:System.Xml.Serialization.XmlSerializer> compatíveis em vez de criar novos tipos de contrato de dados.

- Quando um controle preciso sobre o XML que aparece nas mensagens é importante, mas um documento WSDL (Web Services Description Language) não está disponível, por exemplo, ao criar um serviço com tipos que precisam estar em conformidade com um determinado esquema padronizado e publicado que não é compatível com o DataContractSerializer.

- Ao criar serviços que seguem o padrão de codificação SOAP herdado.

Nesses e em outros casos, você pode alternar manualmente para a <xref:System.Xml.Serialization.XmlSerializer> classe aplicando o `XmlSerializerFormatAttribute` atributo ao seu serviço, conforme mostrado no código a seguir.

[!code-csharp[c_XmlSerializer#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_xmlserializer/cs/source.cs#1)]
[!code-vb[c_XmlSerializer#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_xmlserializer/vb/source.vb#1)]

## <a name="security-considerations"></a>Considerações de segurança

> [!NOTE]
> É importante ter cuidado ao alternar os mecanismos de serialização. O mesmo tipo pode serializar em XML de forma diferente, dependendo do serializador que está sendo usado. Se você usar acidentalmente o serializador errado, você pode estar desfechando informações do tipo que você não pretendia divulgar.

Por exemplo, a <xref:System.Runtime.Serialization.DataContractSerializer> classe serializa apenas os membros marcados com o <xref:System.Runtime.Serialization.DataMemberAttribute> atributo ao serializar tipos de contrato de dados. A <xref:System.Xml.Serialization.XmlSerializer> classe serializa qualquer membro público. Consulte o tipo no código a seguir.

[!code-csharp[c_XmlSerializer#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_xmlserializer/cs/source.cs#2)]
[!code-vb[c_XmlSerializer#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_xmlserializer/vb/source.vb#2)]

Se o tipo for usado inadvertidamente em um contrato de serviço em que a <xref:System.Xml.Serialization.XmlSerializer> classe está selecionada, o `creditCardNumber` membro será serializado, o que provavelmente não será pretendido.

Embora a <xref:System.Runtime.Serialization.DataContractSerializer> classe seja o padrão, você pode selecioná-la explicitamente para seu serviço (embora fazer isso nunca deve ser necessário) aplicando o <xref:System.ServiceModel.DataContractFormatAttribute> atributo ao tipo de contrato de serviço.

O serializador usado para o serviço é parte integrante do contrato e não pode ser alterado selecionando-se uma associação diferente ou alterando outras definições de configuração.

Outras considerações de segurança importantes se aplicam à <xref:System.Xml.Serialization.XmlSerializer> classe. Primeiro, é altamente recomendável que qualquer aplicativo WCF que use a <xref:System.Xml.Serialization.XmlSerializer> classe seja assinado com uma chave que seja protegida contra divulgação. Essa recomendação se aplica quando um comutador manual para o <xref:System.Xml.Serialization.XmlSerializer> é executado e quando um comutador automático é executado (por Svcutil.exe, Adicionar referência de serviço ou uma ferramenta semelhante). Isso ocorre porque o <xref:System.Xml.Serialization.XmlSerializer> mecanismo de serialização dá suporte ao carregamento de *assemblies de serialização gerados previamente* , contanto que eles sejam assinados com a mesma chave que o aplicativo. Um aplicativo não assinado é completamente desprotegido da possibilidade de um assembly mal-intencionado corresponder ao nome esperado do assembly de serialização gerado previamente que está sendo colocado na pasta do aplicativo ou no cache de assembly global. É claro que um invasor deve primeiro obter acesso de gravação a um desses dois locais para tentar essa ação.

Outra ameaça que existe sempre que você usa <xref:System.Xml.Serialization.XmlSerializer> está relacionada ao acesso de gravação à pasta temporária do sistema. O <xref:System.Xml.Serialization.XmlSerializer> mecanismo de serialização cria e usa *assemblies de serialização* temporários nessa pasta. Você deve estar ciente de que qualquer processo com acesso de gravação para a pasta temporária pode substituir esses assemblies de serialização por código mal-intencionado.

## <a name="rules-for-xmlserializer-support"></a>Regras para suporte a XmlSerializer

Você não pode aplicar diretamente <xref:System.Xml.Serialization.XmlSerializer> atributos compatíveis a parâmetros de operação de contrato ou valores de retorno. No entanto, eles podem ser aplicados a mensagens digitadas (partes do corpo do contrato de mensagem), conforme mostrado no código a seguir.

[!code-csharp[c_XmlSerializer#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_xmlserializer/cs/source.cs#3)]
[!code-vb[c_XmlSerializer#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_xmlserializer/vb/source.vb#3)]

Quando aplicado a membros de mensagem digitados, esses atributos substituem as propriedades que entram em conflito nos atributos da mensagem digitada. Por exemplo, no código a seguir, `ElementName` substitui `Name` .

[!code-csharp[c_XmlSerializer#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_xmlserializer/cs/source.cs#4)]
[!code-vb[c_XmlSerializer#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_xmlserializer/vb/source.vb#4)]

<xref:System.ServiceModel.MessageHeaderArrayAttribute>Não há suporte para o atributo ao usar o <xref:System.Xml.Serialization.XmlSerializer> .

> [!NOTE]
> Nesse caso, o <xref:System.Xml.Serialization.XmlSerializer> gera a seguinte exceção, que é liberada antes do WCF: "um elemento declarado no nível superior de um esquema não pode ter `maxOccurs` > 1. Forneça um elemento wrapper para ' more ' usando `XmlArray` ou `XmlArrayItem` em vez de `XmlElementAttribute` , ou usando o estilo de parâmetro encapsulado. "
>
> Se você receber essa exceção, investigue se essa situação se aplica.

O WCF não dá suporte <xref:System.Xml.Serialization.SoapIncludeAttribute> aos <xref:System.Xml.Serialization.XmlIncludeAttribute> atributos e em contratos de mensagem e contratos de operação; <xref:System.Runtime.Serialization.KnownTypeAttribute> em vez disso, use o atributo.

## <a name="types-that-implement-the-ixmlserializable-interface"></a>Tipos que implementam a interface IXmlSerializable

Os tipos que implementam a `IXmlSerializable` interface têm suporte total do `DataContractSerializer` . O <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> atributo sempre deve ser aplicado a esses tipos para controlar seu esquema.

> [!WARNING]
> Se você estiver serializando tipos polimórficos, deverá aplicar o <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> ao tipo para garantir que o tipo correto seja serializado.

Há três variedades de tipos que implementam `IXmlSerializable` : tipos que representam conteúdo arbitrário, tipos que representam um único elemento e tipos herdados <xref:System.Data.DataSet> .

- Os tipos de conteúdo usam um método de provedor de esquema especificado pelo `XmlSchemaProviderAttribute` atributo. O método não retorna `null` e a <xref:System.Xml.Serialization.XmlSchemaProviderAttribute.IsAny%2A> propriedade no atributo é deixada com seu valor padrão de `false` . Esse é o uso mais comum de `IXmlSerializable` tipos.

- Tipos de elemento são usados quando um `IXmlSerializable` tipo deve controlar seu próprio nome de elemento raiz. Para marcar um tipo como um tipo de elemento, defina a <xref:System.Xml.Serialization.XmlSchemaProviderAttribute.IsAny%2A> propriedade no <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> atributo como `true` ou retorne `null` do método de provedor de esquema. Ter um método de provedor de esquema é opcional para tipos de elementos – você pode especificar `null` , em vez do nome do método, no `XmlSchemaProviderAttribute` . No entanto, se `IsAny` for `true` e um método de provedor de esquema for especificado, o método deverá retornar `null` .

- Tipos herdados <xref:System.Data.DataSet> são `IXmlSerializable` tipos que não são marcados com o `XmlSchemaProviderAttribute` atributo. Em vez disso, eles dependem do <xref:System.Xml.Serialization.IXmlSerializable.GetSchema%2A> método de geração de esquema. Esse padrão é usado para o `DataSet` tipo e seu dataset tipado deriva uma classe em versões anteriores do .NET Framework, mas agora é obsoleto e tem suporte apenas por motivos herdados. Não confie nesse padrão e sempre aplique o `XmlSchemaProviderAttribute` aos seus `IXmlSerializable` tipos.

### <a name="ixmlserializable-content-types"></a>Tipos de conteúdo IXmlSerializable

Ao serializar um membro de dados de um tipo que implementa `IXmlSerializable` e é um tipo de conteúdo conforme definido anteriormente, o serializador grava o elemento wrapper para o membro de dados e passa o controle para o <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> método. A <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> implementação pode gravar qualquer XML, que inclui a adição de atributos ao elemento wrapper. Após a `WriteXml` conclusão, o serializador fecha o elemento.

Ao desserializar um membro de dados de um tipo que implementa `IXmlSerializable` e é um tipo de conteúdo definido anteriormente, o desserializador posiciona o leitor XML no elemento wrapper do membro de dados e passa o controle para o <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> método. O método deve ler o elemento inteiro, incluindo as marcas de início e de fim. Verifique se seu `ReadXml` código manipula o caso em que o elemento está vazio. Além disso, sua `ReadXml` implementação não deve depender do elemento wrapper que está sendo nomeado de forma específica. O nome é escolhido pelo serializador pode variar.

É permitido atribuir tipos de `IXmlSerializable` conteúdo polimorficamente, por exemplo, a membros de dados do tipo <xref:System.Object> . Também é permitido que as instâncias de tipo sejam nulas. Por fim, é possível usar `IXmlSerializable` tipos com preservação de grafo de objetos habilitada e com o <xref:System.Runtime.Serialization.NetDataContractSerializer> . Todos esses recursos exigem que o serializador do WCF anexe determinados atributos ao elemento wrapper ("nil" e "Type" no namespace da instância de esquema XML e "ID", "ref", "Type" e "assembly" em um namespace específico do WCF).

#### <a name="attributes-to-ignore-when-implementing-readxml"></a>Atributos a serem ignorados ao implementar ReadXml

Antes de passar o controle para seu `ReadXml` código, o desserializador examina o elemento XML, detecta esses atributos XML especiais e atua neles. Por exemplo, se "nil" for `true` , um valor nulo será desserializado e `ReadXml` não será chamado. Se polimorfismo for detectado, o conteúdo do elemento será desserializado como se fosse um tipo diferente. A implementação do tipo atribuído por polimorficamente do `ReadXml` é chamada. Em qualquer caso, uma `ReadXml` implementação deve ignorar esses atributos especiais porque eles são tratados pelo desserializador.

### <a name="schema-considerations-for-ixmlserializable-content-types"></a>Considerações de esquema para tipos de conteúdo IXmlSerializable

Ao exportar o esquema e um `IXmlSerializable` tipo de conteúdo, o método de provedor de esquema é chamado. Um <xref:System.Xml.Schema.XmlSchemaSet> é passado para o método de provedor de esquema. O método pode adicionar qualquer esquema válido ao conjunto de esquema. O conjunto de esquema contém o esquema que já é conhecido no momento em que a exportação de esquema ocorre. Quando o método do provedor de esquema deve adicionar um item ao conjunto de esquema, ele deve determinar se um <xref:System.Xml.Schema.XmlSchema> com o namespace apropriado já existe no conjunto. Se tiver, o método de provedor de esquema deverá adicionar o novo item ao existente `XmlSchema` . Caso contrário, ele deve criar uma nova `XmlSchema` instância. Isso é importante se as matrizes de `IXmlSerializable` tipos estiverem sendo usadas. Por exemplo, se você tiver um `IXmlSerializable` tipo que é exportado como o tipo "A" no namespace "b", é possível que, no momento em que o método do provedor de esquema for chamado, o conjunto de esquema já contenha o esquema para "B" para manter o tipo "ArrayOfA".

Além de adicionar tipos ao <xref:System.Xml.Schema.XmlSchemaSet> , o método de provedor de esquema para tipos de conteúdo deve retornar um valor não nulo. Ele pode retornar um <xref:System.Xml.XmlQualifiedName> que especifica o nome do tipo de esquema a ser usado para o `IXmlSerializable` tipo fornecido. Esse nome qualificado também serve como o nome do contrato de dados e o namespace para o tipo. É permitido retornar um tipo que não existe no conjunto de esquema imediatamente quando o método de provedor de esquema retorna. No entanto, é esperado que, no momento em que todos os tipos relacionados forem exportados (o <xref:System.Runtime.Serialization.XsdDataContractExporter.Export%2A> método for chamado para todos os tipos relevantes no <xref:System.Runtime.Serialization.XsdDataContractExporter> e a <xref:System.Runtime.Serialization.XsdDataContractExporter.Schemas%2A> propriedade for acessada), o tipo exista no conjunto de esquema. O acesso à `Schemas` propriedade antes que todas as `Export` chamadas relevantes tenham sido feitas pode resultar em um <xref:System.Xml.Schema.XmlSchemaException> . Para obter mais informações sobre o processo de exportação, consulte [exportando esquemas de classes](exporting-schemas-from-classes.md).

O método de provedor de esquema também pode retornar o <xref:System.Xml.Schema.XmlSchemaType> a ser usado. O tipo pode ou não ser anônimo. Se for anônimo, o esquema para o `IXmlSerializable` tipo será exportado como um tipo anônimo toda vez que o `IXmlSerializable` tipo for usado como um membro de dados. O `IXmlSerializable` tipo ainda tem um nome e namespace de contrato de dados. (Isso é determinado conforme descrito em [nomes de contrato de dados](data-contract-names.md) , exceto que o <xref:System.Runtime.Serialization.DataContractAttribute> atributo não pode ser usado para personalizar o nome.) Se não for anônimo, ele deverá ser um dos tipos no `XmlSchemaSet` . Esse caso é equivalente a retornar o `XmlQualifiedName` do tipo.

Além disso, uma declaração de elemento global é exportada para o tipo. Se o tipo não tiver o <xref:System.Xml.Serialization.XmlRootAttribute> atributo aplicado a ele, o elemento terá o mesmo nome e namespace que o contrato de dados, e sua propriedade "anulável" será `true` . A única exceção é o namespace do esquema ( `http://www.w3.org/2001/XMLSchema` ) – se o contrato de dados do tipo estiver nesse namespace, o elemento global correspondente estará no namespace em branco porque é proibido adicionar novos elementos ao namespace do esquema. Se o tipo tiver o `XmlRootAttribute` atributo aplicado, a declaração de elemento global será exportada usando as propriedades a seguir: <xref:System.Xml.Serialization.XmlRootAttribute.ElementName%2A> <xref:System.Xml.Serialization.XmlRootAttribute.Namespace%2A> e <xref:System.Xml.Serialization.XmlRootAttribute.IsNullable%2A> . Os padrões com `XmlRootAttribute` aplicado são o nome do contrato de dados, um namespace em branco e "anulável" `true` .

As mesmas regras de declaração de elemento global se aplicam a tipos de conjuntos de conjunto de DataSet. Observe que o `XmlRootAttribute` não pode substituir as declarações de elemento global adicionadas por meio de código personalizado, adicionadas ao `XmlSchemaSet` usando o método de provedor de esquema ou por meio `GetSchema` de tipos de conjuntos de conjunto de código herdado.

### <a name="ixmlserializable-element-types"></a>Tipos de elemento IXmlSerializable

`IXmlSerializable`os tipos de elemento têm a `IsAny` propriedade definida como `true` ou seu método de provedor de esquema retorna `null` .

A serialização e a desserialização de um tipo de elemento são muito semelhantes à serialização e à desserialização de um tipo de conteúdo. No entanto há algumas diferenças importantes:

- Espera-se que a `WriteXml` implementação grave exatamente um elemento (que, naturalmente, pode conter vários elementos filho). Ele não deve estar gravando atributos fora deste único elemento, vários elementos irmãos ou conteúdo misto. O elemento pode estar vazio.

- A `ReadXml` implementação não deve ler o elemento wrapper. Espera-se que seja lido um elemento que `WriteXml` produz.

- Ao serializar um tipo de elemento regularmente (por exemplo, como um membro de dados em um contrato de dados), o serializador gera um elemento wrapper antes de chamar `WriteXml` , como com os tipos de conteúdo. No entanto, ao serializar um tipo de elemento no nível superior, o serializador normalmente não gera um elemento de wrapper em todo o elemento que `WriteXml` grava, a menos que um nome de raiz e namespace sejam explicitamente especificados ao construir o serializador nos `DataContractSerializer` `NetDataContractSerializer` construtores ou. Para obter mais informações, consulte [serialização e desserialização](serialization-and-deserialization.md).

- Ao serializar um tipo de elemento no nível superior sem especificar o nome raiz e o namespace no momento da construção <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteStartObject%2A> e, <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteEndObject%2A> essencialmente, não faz nada e <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteObjectContent%2A> chama `WriteXml` . Nesse modo, o objeto que está sendo serializado não pode ser `null` e não pode ser polimorficamente atribuído. Além disso, a preservação do grafo de objetos não pode ser habilitada e `NetDataContractSerializer` não pode ser usada.

- Ao desserializar um tipo de elemento no nível superior sem especificar o nome raiz e o namespace no momento da construção, <xref:System.Runtime.Serialization.XmlObjectSerializer.IsStartObject%2A> retorna `true` se ele pode encontrar o início de qualquer elemento. <xref:System.Runtime.Serialization.XmlObjectSerializer.ReadObject%2A>com o `verifyObjectName` parâmetro definido como se `true` comporta da mesma maneira que `IsStartObject` antes de realmente ler o objeto. `ReadObject`em seguida, passa o controle para o `ReadXml` método.

O esquema exportado para tipos de elementos é o mesmo para o `XmlElement` tipo, conforme descrito em uma seção anterior, exceto pelo fato de que o método de provedor de esquema pode adicionar qualquer esquema adicional ao <xref:System.Xml.Schema.XmlSchemaSet> como com os tipos de conteúdo. O uso do `XmlRootAttribute` atributo com tipos de elemento não é permitido e as declarações de elemento global nunca são emitidas para esses tipos.

### <a name="differences-from-the-xmlserializer"></a>Diferenças do XmlSerializer

A `IXmlSerializable` interface e os `XmlSchemaProviderAttribute` `XmlRootAttribute` atributos e também são compreendidos pelo <xref:System.Xml.Serialization.XmlSerializer> . No entanto, há algumas diferenças em como elas são tratadas no modelo de contrato de dados. As diferenças importantes são resumidas na lista a seguir:

- O método de provedor de esquema deve ser público para ser usado no `XmlSerializer` , mas não precisa ser público para ser usado no modelo de contrato de dados.

- O método de provedor de esquema é chamado quando `IsAny` está `true` no modelo de contrato de dados, mas não com o `XmlSerializer` .

- Quando o `XmlRootAttribute` atributo não está presente para o conteúdo ou tipos de conjuntos de conjunto de DataSet herdados, o `XmlSerializer` exporta uma declaração de elemento global no namespace em branco. No modelo de contrato de dados, o namespace usado normalmente é o namespace de contrato de dados, conforme descrito anteriormente.

Esteja atento a essas diferenças ao criar tipos que são usados com as duas tecnologias de serialização.

### <a name="importing-ixmlserializable-schema"></a>Importando esquema IXmlSerializable

Ao importar um esquema gerado a partir de `IXmlSerializable` tipos, há algumas possibilidades:

- O esquema gerado pode ser um esquema de contrato de dados válido, conforme descrito em [referência de esquema de contrato de dados](data-contract-schema-reference.md). Nesse caso, o esquema pode ser importado como de costume e os tipos de contrato de dados regulares são gerados.

- O esquema gerado pode não ser um esquema de contrato de dados válido. Por exemplo, o método de provedor de esquema pode gerar um esquema que envolve atributos XML que não têm suporte no modelo de contrato de dados. Nesse caso, você pode importar o esquema como `IXmlSerializable` tipos. Esse modo de importação não está ativado por padrão, mas pode ser facilmente habilitado – por exemplo, com a `/importXmlTypes` opção de linha de comando para a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). Isso é descrito em detalhes no [esquema de importação para gerar classes](importing-schema-to-generate-classes.md). Observe que você deve trabalhar diretamente com o XML para suas instâncias de tipo. Você também pode considerar o uso de uma tecnologia de serialização diferente que dá suporte a um grande intervalo de esquema – consulte o tópico sobre como usar o `XmlSerializer` .

- Talvez você queira reutilizar os `IXmlSerializable` tipos existentes no proxy em vez de gerar novos. Nesse caso, o recurso de tipos referenciados descrito no tópico importando o esquema para gerar tipos pode ser usado para indicar o tipo a ser reutilizado. Isso corresponde ao uso da `/reference` opção on svcutil.exe, que especifica o assembly que contém os tipos a serem reutilizados.

### <a name="xmlserializer-legacy-behavior"></a>Comportamento herdado XmlSerializer

No .NET Framework 4,0 e anteriores, o XmlSerializer gerou assemblies de serialização temporários escrevendo código C# em um arquivo. O arquivo foi então compilado em um assembly.  Esse comportamento tinha algumas consequências indesejáveis, como a lentidão do tempo de inicialização para o serializador. No .NET Framework 4,5, esse comportamento foi alterado para gerar os assemblies sem a necessidade de uso do compilador. Alguns desenvolvedores podem querer ver o código C# gerado. Você pode especificar para usar esse comportamento herdado pela seguinte configuração:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.xml.serialization>
    <xmlSerializer tempFilesLocation='e:\temp\XmlSerializerBug' useLegacySerializerGeneration="true" />
  </system.xml.serialization>
  <system.diagnostics>
    <switches>
      <add name="XmlSerialization.Compilation" value="1" />
    </switches>
  </system.diagnostics>
</configuration>
```

Se você tiver problemas de compatibilidade, como a `XmlSerializer` falha em serializar uma classe derivada com uma nova substituição não pública, poderá voltar para o `XMLSerializer` comportamento herdado usando a seguinte configuração:

```xml
<configuration>
  <appSettings>
    <add key="System:Xml:Serialization:UseLegacySerializerGeneration" value="true" />
  </appSettings>
</configuration>
```

Como alternativa à configuração acima, você pode usar a seguinte configuração em um computador que executa o .NET Framework 4,5 ou versão posterior:

```xml
<configuration>
  <system.xml.serialization>
    <xmlSerializer useLegacySerializerGeneration="true"/>
  </system.xml.serialization>
</configuration>
```

> [!NOTE]
> A `<xmlSerializer useLegacySerializerGeneration="true"/>` opção funciona apenas em um computador que executa o .NET Framework 4,5 ou versão posterior. A `appSettings` abordagem acima funciona em todas as versões de .NET Framework.

## <a name="see-also"></a>Veja também

- <xref:System.ServiceModel.DataContractFormatAttribute>
- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Xml.Serialization.XmlSerializer>
- <xref:System.ServiceModel.MessageHeaderArrayAttribute>
- [Especificando transferência de dados em contratos de serviço](specifying-data-transfer-in-service-contracts.md)
- [Usando contratos de dados](using-data-contracts.md)
- [Como melhorar o tempo de inicialização dos aplicativos do cliente WCF usando o XmlSerializer](startup-time-of-wcf-client-applications-using-the-xmlserializer.md)
