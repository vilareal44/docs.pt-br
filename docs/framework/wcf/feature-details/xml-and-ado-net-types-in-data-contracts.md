---
title: Tipos de XML e ADO.NET em contratos de dados
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c2ce8461-3c15-4c41-8c81-1cb78f5b59a6
ms.openlocfilehash: 975d4f4f37bbbd895cab4e87686f15017e90f382
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600069"
---
# <a name="xml-and-adonet-types-in-data-contracts"></a>Tipos de XML e ADO.NET em contratos de dados
O modelo de contrato de dados do Windows Communication Foundation (WCF) dá suporte a determinados tipos que representam XML diretamente. Quando esses tipos são serializados para XML, o serializador grava o conteúdo XML desses tipos sem nenhum processamento adicional. Os tipos com suporte são <xref:System.Xml.XmlElement> , matrizes <xref:System.Xml.XmlNode> (mas não o `XmlNode` próprio tipo), bem como tipos que implementam <xref:System.Xml.Serialization.IXmlSerializable> . O <xref:System.Data.DataSet> <xref:System.Data.DataTable> tipo e, bem como os conjuntos de dados tipados, são comumente usados na programação de banco de dados. Esses tipos implementam a `IXmlSerializable` interface e, portanto, são serializáveis no modelo de contrato de dados. Algumas considerações especiais para esses tipos são listadas no final deste tópico.  
  
## <a name="xml-types"></a>Tipos XML  
  
### <a name="xml-element"></a>Elemento XML  
 O `XmlElement` tipo é serializado usando seu conteúdo XML. Por exemplo, usando o seguinte tipo.  
  
 [!code-csharp[DataContractAttribute#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#4)]
 [!code-vb[DataContractAttribute#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#4)]  
  
 Isso é serializado para XML da seguinte maneira:  
  
```xml  
<MyDataContract xmlns="http://schemas.contoso.com">  
    <myDataMember>  
        <myElement xmlns="" myAttribute="myValue">  
            myContents  
        </myElement>  
    </myDataMember>  
</MyDataContract>  
```  
  
 Observe que um elemento de membro de dados de wrapper `<myDataMember>` ainda está presente. Não há nenhuma maneira de remover esse elemento no modelo de contrato de dados. Os serializadores que manipulam esse modelo ( <xref:System.Runtime.Serialization.DataContractSerializer> e <xref:System.Runtime.Serialization.NetDataContractSerializer> ) podem emitir atributos especiais para esse elemento wrapper. Esses atributos incluem o atributo "nil" da instância de esquema XML padrão (permitindo que seja `XmlElement` `null` ) e o atributo "Type" (permitindo que `XmlElement` seja usado polimorficamente). Além disso, os seguintes atributos XML são específicos para o WCF: "ID", "ref", "Type" e "assembly". Esses atributos podem ser emitidos para dar suporte ao uso do `XmlElement` com o modo de preservação de grafo de objeto habilitado ou com o <xref:System.Runtime.Serialization.NetDataContractSerializer> . (Para obter mais informações sobre o modo de preservação de grafo de objetos, consulte [serialização e desserialização](serialization-and-deserialization.md).)  
  
 Matrizes ou coleções de `XmlElement` são permitidas e são tratadas como qualquer outra matriz ou coleção. Ou seja, há um elemento wrapper para a coleção inteira e um elemento wrapper separado (semelhante ao `<myDataMember>` no exemplo anterior) para cada `XmlElement` uma na matriz.  
  
 Na desserialização, um `XmlElement` é criado pelo desserializador do XML de entrada. Um pai válido <xref:System.Xml.XmlDocument> é fornecido pelo desserializador.  
  
 Verifique se o fragmento XML que é desserializado para um `XmlElement` define todos os prefixos que ele usa e se não depende de definições de prefixo de elementos ancestrais. Essa é uma preocupação apenas ao usar o `DataContractSerializer` para acessar o XML de uma fonte diferente (não `DataContractSerializer` ).  
  
 Quando usado com o `DataContractSerializer` , o `XmlElement` pode ser atribuído a polimorficamente, mas somente a um membro de dados do tipo <xref:System.Object> . Mesmo que ele implemente <xref:System.Collections.IEnumerable> , um `XmlElement` não pode ser usado como um tipo de coleção e não pode ser atribuído a um <xref:System.Collections.IEnumerable> membro de dados. Assim como acontece com todas as atribuições polimórficas, o `DataContractSerializer` emite o nome do contrato de dados no XML resultante – nesse caso, é "XmlElement" no http://schemas.datacontract.org/2004/07/System.Xml namespace "".  
  
 Com o `NetDataContractSerializer` , há suporte para qualquer atribuição polimórfica válida de `XmlElement` (para `Object` ou `IEnumerable` ).  
  
 Não tente usar nenhum dos serializadores com tipos derivados de `XmlElement` , se eles forem atribuídos a polimorficamente ou não.  
  
### <a name="array-of-xmlnode"></a>Matriz de XmlNode  
 O uso de matrizes do <xref:System.Xml.XmlNode> é muito semelhante ao uso do `XmlElement` . O uso de matrizes `XmlNode` proporciona mais flexibilidade do que o uso do `XmlElement` . Você pode escrever vários elementos dentro do elemento de quebra de membro de dados. Você também pode injetar conteúdo que não seja elementos dentro do elemento de quebra de membro de dados, como comentários XML. Por fim, você pode colocar atributos no elemento membro de dados de encapsulamento. Tudo isso pode ser obtido preenchendo a matriz de `XmlNode` com classes derivadas específicas de como `XmlNode` <xref:System.Xml.XmlAttribute> , `XmlElement` ou <xref:System.Xml.XmlComment> . Por exemplo, usando o seguinte tipo.  
  
 [!code-csharp[DataContractAttribute#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#5)]
 [!code-vb[DataContractAttribute#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#5)]  
  
 Quando serializado, o XML resultante é semelhante ao código a seguir.  
  
```xml  
<MyDataContract xmlns="http://schemas.contoso.com">  
  <myDataMember myAttribute="myValue">  
     <!--myComment-->  
     <myElement xmlns="" myAttribute="myValue">  
 myContents  
     </myElement>  
     <myElement xmlns="" myAttribute="myValue">  
       myContents  
     </myElement>  
  </myDataMember>  
</MyDataContract>  
```  
  
 Observe que o elemento wrapper de membro de dados `<myDataMember>` contém um atributo, um comentário e dois elementos. Essas são as quatro `XmlNode` instâncias que foram serializadas.  
  
 Uma matriz do `XmlNode` que resulta em um XML inválido não pode ser serializada. Por exemplo, uma matriz de duas `XmlNode` instâncias em que a primeira é uma `XmlElement` e a segunda é <xref:System.Xml.XmlAttribute> inválida, porque essa sequência não corresponde a nenhuma instância XML válida (não há nenhum local para anexar o atributo).  
  
 Na desserialização de uma matriz do `XmlNode` , os nós são criados e preenchidos com informações do XML de entrada. Um pai válido <xref:System.Xml.XmlDocument> é fornecido pelo desserializador. Todos os nós são desserializados, incluindo quaisquer atributos no elemento membro de dados do wrapper, mas excluindo os atributos colocados ali pelos serializadores do WCF (como os atributos usados para indicar a atribuição polimórfica). A limitação sobre a definição de todos os prefixos de namespace no fragmento XML se aplica à desserialização de matrizes da `XmlNode` mesma forma que a desserialização `XmlElement` .  
  
 Ao usar os serializadores com preservação de grafo de objetos ativada, a igualdade de objeto é preservada apenas no nível de `XmlNode` matrizes, não em `XmlNode` instâncias individuais.  
  
 Não tente serializar uma matriz de `XmlNode` onde um ou mais nós está definido como `null` . É permitido que todo o membro da matriz seja `null` , mas não para qualquer indivíduo `XmlNode` contido na matriz. Se todo o membro da matriz for nulo, o elemento do membro de dados do wrapper conterá um atributo especial que indica que ele é nulo. Na desserialização, todo o membro da matriz também se torna nulo.  
  
 Somente matrizes regulares do `XmlNode` são tratadas especialmente pelo serializador. Os membros de dados declarados como outros tipos de coleção que contêm `XmlNode` ou membros de dados declarados como matrizes de tipos derivados de `XmlNode` , não são tratados especialmente. Portanto, eles normalmente não são serializáveis, a menos que também atendam a um dos outros critérios para a serialização.  
  
 Matrizes ou coleções de matrizes `XmlNode` são permitidas. Há um elemento wrapper para toda a coleção e um elemento wrapper separado (semelhante ao `<myDataMember>` exemplo anterior) para cada matriz de `XmlNode` na matriz ou coleção externa.  
  
 Popular um membro de dados do <xref:System.Array> tipo `Object` de `Array` ou `IEnumerable` com `XmlNode` instâncias não resulta no membro de dados sendo tratado como uma `Array` das `XmlNode` instâncias. Cada membro da matriz é serializado separadamente.  
  
 Quando usado com o `DataContractSerializer` , as matrizes de `XmlNode` podem ser atribuídas polimorficamente, mas somente a um membro de dados do tipo `Object` . Mesmo que ele implemente `IEnumerable` , uma matriz de `XmlNode` não pode ser usada como um tipo de coleção e ser atribuída a um `IEnumerable` membro de dados. Assim como acontece com todas as atribuições polimórficas, o `DataContractSerializer` emite o nome do contrato de dados no XML resultante – nesse caso, é "ArrayOfXmlNode" no http://schemas.datacontract.org/2004/07/System.Xml namespace "". Quando usado com o `NetDataContractSerializer` , há suporte para qualquer atribuição válida de uma `XmlNode` matriz.  
  
### <a name="schema-considerations"></a>Considerações sobre o esquema  
 Para obter detalhes sobre o mapeamento de esquema de tipos XML, consulte [referência de esquema de contrato de dados](data-contract-schema-reference.md). Esta seção fornece um resumo dos pontos importantes.  
  
 Um membro de dados do tipo `XmlElement` é mapeado para um elemento definido usando o seguinte tipo anônimo.  
  
```xml  
<xsd:complexType>  
   <xsd:sequence>  
      <xsd:any minOccurs="0" processContents="lax" />  
   </xsd:sequence>  
</xsd:complexType>  
```  
  
 Um membro de dados do tipo matriz de `XmlNode` é mapeado para um elemento definido usando o seguinte tipo anônimo.  
  
```xml  
<xsd:complexType mixed="true">  
   <xsd:sequence>  
      <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax" />  
   </xsd:sequence>  
   <xsd:anyAttribute/>  
</xsd:complexType>  
```  
  
## <a name="types-implementing-the-ixmlserializable-interface"></a>Tipos que implementam a interface IXmlSerializable  
 Os tipos que implementam a `IXmlSerializable` interface têm suporte total do `DataContractSerializer` . O <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> atributo sempre deve ser aplicado a esses tipos para controlar seu esquema.  
  
 Há três variedades de tipos que implementam `IXmlSerializable` : tipos que representam conteúdo arbitrário, tipos que representam um único elemento e tipos herdados <xref:System.Data.DataSet> .  
  
- Os tipos de conteúdo usam um método de provedor de esquema especificado pelo `XmlSchemaProviderAttribute` atributo. O método não retorna `null` , e a <xref:System.Xml.Serialization.XmlSchemaProviderAttribute.IsAny%2A> propriedade no atributo é deixada com seu valor padrão de `false` . Esse é o uso mais comum de `IXmlSerializable` tipos.  
  
- Tipos de elemento são usados quando um `IXmlSerializable` tipo deve controlar seu próprio nome de elemento raiz. Para marcar um tipo como um tipo de elemento, defina a <xref:System.Xml.Serialization.XmlSchemaProviderAttribute.IsAny%2A> propriedade no <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> atributo como `true` ou retorne NULL do método de provedor de esquema. Ter um método de provedor de esquema é opcional para tipos de elementos – você pode especificar NULL em vez do nome do método no `XmlSchemaProviderAttribute` . No entanto, se `IsAny` for `true` e um método de provedor de esquema for especificado, o método deverá retornar NULL.  
  
- Tipos herdados <xref:System.Data.DataSet> são `IXmlSerializable` tipos que não são marcados com o `XmlSchemaProviderAttribute` atributo. Em vez disso, eles dependem do <xref:System.Xml.Serialization.IXmlSerializable.GetSchema%2A> método de geração de esquema. Esse padrão é usado para o `DataSet` tipo e seu dataset tipado deriva uma classe em versões anteriores do .NET Framework, mas agora é obsoleto e tem suporte apenas por motivos herdados. Não confie nesse padrão e sempre aplique o `XmlSchemaProviderAttribute` aos seus `IXmlSerializable` tipos.  
  
### <a name="ixmlserializable-content-types"></a>Tipos de conteúdo IXmlSerializable  
 Ao serializar um membro de dados de um tipo que implementa `IXmlSerializable` e é um tipo de conteúdo conforme definido anteriormente, o serializador grava o elemento wrapper para o membro de dados e passa o controle para o <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> método. A <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> implementação pode gravar qualquer XML, incluindo a adição de atributos ao elemento wrapper. Após a `WriteXml` conclusão, o serializador fecha o elemento.  
  
 Ao desserializar um membro de dados de um tipo que implementa `IXmlSerializable` e é um tipo de conteúdo definido anteriormente, o desserializador posiciona o leitor XML no elemento wrapper do membro de dados e passa o controle para o <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> método. O método deve ler o elemento inteiro, incluindo as marcas de início e de fim. Verifique se seu `ReadXml` código manipula o caso em que o elemento está vazio. Além disso, sua `ReadXml` implementação não deve depender do elemento wrapper que está sendo nomeado de forma específica. O nome é escolhido pelo serializador pode variar.  
  
 É permitido atribuir tipos de `IXmlSerializable` conteúdo polimorficamente, por exemplo, a membros de dados do tipo <xref:System.Object> . Também é permitido que as instâncias de tipo sejam nulas. Por fim, é possível usar `IXmlSerializable` tipos com preservação de grafo de objetos habilitada e com o <xref:System.Runtime.Serialization.NetDataContractSerializer> . Todos esses recursos exigem que o serializador do WCF anexe determinados atributos ao elemento wrapper ("nil" e "Type" no namespace da instância de esquema XML e "ID", "ref", "Type" e "assembly" em um namespace específico do WCF).  
  
#### <a name="attributes-to-ignore-when-implementing-readxml"></a>Atributos a serem ignorados ao implementar ReadXml  
 Antes de passar o controle para seu `ReadXml` código, o desserializador examina o elemento XML, detecta esses atributos XML especiais e atua neles. Por exemplo, se "nil" for `true` , um valor nulo será desserializado e `ReadXml` não será chamado. Se polimorfismo for detectado, o conteúdo do elemento será desserializado como se fosse um tipo diferente. A implementação do tipo atribuído polimorficamente de `ReadXml` é chamada. Em qualquer caso, uma `ReadXml` implementação deve ignorar esses atributos especiais porque eles são tratados pelo desserializador.  
  
### <a name="schema-considerations-for-ixmlserializable-content-types"></a>Considerações de esquema para tipos de conteúdo IXmlSerializable  
 Ao exportar o esquema de um `IXmlSerializable` tipo de conteúdo, o método de provedor de esquema é chamado. Um <xref:System.Xml.Schema.XmlSchemaSet> é passado para o método de provedor de esquema. O método pode adicionar qualquer esquema válido ao conjunto de esquema. O conjunto de esquema contém o esquema que já é conhecido no momento em que a exportação de esquema ocorre. Quando o método do provedor de esquema deve adicionar um item ao conjunto de esquema, ele deve determinar se um <xref:System.Xml.Schema.XmlSchema> com o namespace apropriado já existe no conjunto. Se tiver, o método de provedor de esquema deverá adicionar o novo item ao existente `XmlSchema` . Caso contrário, ele deve criar uma nova `XmlSchema` instância. Isso é importante se as matrizes de `IXmlSerializable` tipos estiverem sendo usadas. Por exemplo, se você tiver um `IXmlSerializable` tipo que é exportado como o tipo "A" no namespace "b", é possível que, no momento em que o método do provedor de esquema for chamado, o conjunto de esquema já contenha o esquema para "B" para manter o tipo "ArrayOfA".  
  
 Além de adicionar tipos ao <xref:System.Xml.Schema.XmlSchemaSet> , o método de provedor de esquema para tipos de conteúdo deve retornar um valor não nulo. Ele pode retornar um <xref:System.Xml.XmlQualifiedName> que especifica o nome do tipo de esquema a ser usado para o `IXmlSerializable` tipo fornecido. Esse nome qualificado também serve como o nome do contrato de dados e o namespace para o tipo. É permitido retornar um tipo que não existe no conjunto de esquema imediatamente quando o método de provedor de esquema retorna. No entanto, é esperado que, no momento em que todos os tipos relacionados forem exportados (o <xref:System.Runtime.Serialization.XsdDataContractExporter.Export%2A> método for chamado para todos os tipos relevantes no <xref:System.Runtime.Serialization.XsdDataContractExporter> e a <xref:System.Runtime.Serialization.XsdDataContractExporter.Schemas%2A> propriedade for acessada), o tipo exista no conjunto de esquema. O acesso à `Schemas` propriedade antes que todas as `Export` chamadas relevantes tenham sido feitas pode resultar em um <xref:System.Xml.Schema.XmlSchemaException> . Para obter mais informações sobre o processo de exportação, consulte [exportando esquemas de classes](exporting-schemas-from-classes.md).  
  
 O método de provedor de esquema também pode retornar o <xref:System.Xml.Schema.XmlSchemaType> a ser usado. O tipo pode ou não ser anônimo. Se for anônimo, o esquema para o `IXmlSerializable` tipo será exportado como um tipo anônimo toda vez que o `IXmlSerializable` tipo for usado como um membro de dados. O `IXmlSerializable` tipo ainda tem um nome e namespace de contrato de dados. (Isso é determinado conforme descrito em [nomes de contrato de dados](data-contract-names.md) , exceto que o <xref:System.Runtime.Serialization.DataContractAttribute> atributo não pode ser usado para personalizar o nome.) Se não for anônimo, ele deverá ser um dos tipos no `XmlSchemaSet` . Esse caso é equivalente a retornar o `XmlQualifiedName` do tipo.  
  
 Além disso, uma declaração de elemento global é exportada para o tipo. Se o tipo não tiver o <xref:System.Xml.Serialization.XmlRootAttribute> atributo aplicado a ele, o elemento terá o mesmo nome e namespace que o contrato de dados, e sua propriedade "anulável" será verdadeira. A única exceção é o namespace do esquema (" http://www.w3.org/2001/XMLSchema ") – se o contrato de dados do tipo estiver nesse namespace, o elemento global correspondente estará no namespace em branco porque é proibido adicionar novos elementos ao namespace do esquema. Se o tipo tiver o `XmlRootAttribute` atributo aplicado, a declaração de elemento global será exportada usando as propriedades a seguir: <xref:System.Xml.Serialization.XmlRootAttribute.ElementName%2A> <xref:System.Xml.Serialization.XmlRootAttribute.Namespace%2A> e <xref:System.Xml.Serialization.XmlRootAttribute.IsNullable%2A> . Os padrões com `XmlRootAttribute` aplicado são o nome do contrato de dados, um namespace em branco e "anulável" são verdadeiros.  
  
 As mesmas regras de declaração de elemento global se aplicam a tipos de conjuntos de conjunto de DataSet. Observe que o `XmlRootAttribute` não pode substituir as declarações de elemento global adicionadas por meio de código personalizado, adicionadas ao `XmlSchemaSet` usando o método de provedor de esquema ou por meio `GetSchema` de tipos de conjuntos de conjunto de código herdado.  
  
### <a name="ixmlserializable-element-types"></a>Tipos de elemento IXmlSerializable  
 `IXmlSerializable`os tipos de elemento têm a `IsAny` propriedade definida como `true` ou seu método de provedor de esquema retorna `null` .  
  
 A serialização e a desserialização de um tipo de elemento são muito semelhantes à serialização e à desserialização de um tipo de conteúdo. No entanto há algumas diferenças importantes:  
  
- Espera-se que a `WriteXml` implementação grave exatamente um elemento (que, naturalmente, pode conter vários elementos filho). Ele não deve estar gravando atributos fora deste único elemento, vários elementos irmãos ou conteúdo misto. O elemento pode estar vazio.  
  
- A `ReadXml` implementação não deve ler o elemento wrapper. Espera-se que seja lido um elemento que `WriteXml` produz.  
  
- Ao serializar um tipo de elemento regularmente (por exemplo, como um membro de dados em um contrato de dados), o serializador gera um elemento wrapper antes de chamar `WriteXml` , como com os tipos de conteúdo. No entanto, ao serializar um tipo de elemento no nível superior, o serializador normalmente não gera um elemento de wrapper em todo o elemento que `WriteXml` grava, a menos que um nome de raiz e um namespace tenham sido explicitamente especificados ao construir o serializador nos `DataContractSerializer` `NetDataContractSerializer` construtores ou. Para obter mais informações, consulte [serialização e desserialização](serialization-and-deserialization.md).  
  
- Ao serializar um tipo de elemento no nível superior sem especificar o nome raiz e o namespace no momento da construção <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteStartObject%2A> e, <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteEndObject%2A> essencialmente, não faz nada e <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteObjectContent%2A> chama `WriteXml` . Nesse modo, o objeto que está sendo serializado não pode ser nulo e não pode ser polimorficamente atribuído. Além disso, a preservação do grafo de objetos não pode ser habilitada e `NetDataContractSerializer` não pode ser usada.  
  
- Ao desserializar um tipo de elemento no nível superior sem especificar o nome raiz e o namespace no momento da construção, <xref:System.Runtime.Serialization.XmlObjectSerializer.IsStartObject%2A> retorna `true` se ele pode encontrar o início de qualquer elemento. <xref:System.Runtime.Serialization.XmlObjectSerializer.ReadObject%2A>com o `verifyObjectName` parâmetro definido como se `true` comporta da mesma maneira que `IsStartObject` antes de realmente ler o objeto. `ReadObject`em seguida, passa o controle para o `ReadXml` método.  
  
 O esquema exportado para tipos de elementos é o mesmo para o `XmlElement` tipo, conforme descrito em uma seção anterior, exceto pelo fato de que o método de provedor de esquema pode adicionar qualquer esquema adicional ao <xref:System.Xml.Schema.XmlSchemaSet> como com os tipos de conteúdo. O uso do `XmlRootAttribute` atributo com tipos de elemento não é permitido e as declarações de elemento global nunca são emitidas para esses tipos.  
  
### <a name="differences-from-the-xmlserializer"></a>Diferenças do XmlSerializer  
 A `IXmlSerializable` interface e os `XmlSchemaProviderAttribute` `XmlRootAttribute` atributos e também são compreendidos pelo <xref:System.Xml.Serialization.XmlSerializer> . No entanto, há algumas diferenças em como elas são tratadas no modelo de contrato de dados. As diferenças importantes são resumidas no seguinte:  
  
- O método de provedor de esquema deve ser público para ser utilizável no `XmlSerializer` , mas não precisa ser público para ser utilizável no modelo de contrato de dados.  
  
- O método de provedor de esquema é chamado quando `IsAny` é verdadeiro no modelo de contrato de dados, mas não com o `XmlSerializer` .  
  
- Quando o `XmlRootAttribute` atributo não está presente para o conteúdo ou tipos de conjuntos de conjunto de DataSet herdados, o `XmlSerializer` exporta uma declaração de elemento global no namespace em branco. No modelo de contrato de dados, o namespace usado normalmente é o namespace de contrato de dados, conforme descrito anteriormente.  
  
 Esteja atento a essas diferenças ao criar tipos que são usados com as duas tecnologias de serialização.  
  
### <a name="importing-ixmlserializable-schema"></a>Importando esquema IXmlSerializable  
 Ao importar um esquema gerado a partir de `IXmlSerializable` tipos, há algumas possibilidades:  
  
- O esquema gerado pode ser um esquema de contrato de dados válido, conforme descrito em [referência de esquema de contrato de dados](data-contract-schema-reference.md). Nesse caso, o esquema pode ser importado como de costume e os tipos de contrato de dados regulares são gerados.  
  
- O esquema gerado pode não ser um esquema de contrato de dados válido. Por exemplo, o método de provedor de esquema pode gerar um esquema que envolva atributos XML que não têm suporte no modelo de contrato de dados. Nesse caso, você pode importar o esquema como `IXmlSerializable` tipos. Esse modo de importação não está ativado por padrão, mas pode ser facilmente habilitado – por exemplo, com a `/importXmlTypes` opção de linha de comando para a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). Isso é descrito em detalhes no [esquema de importação para gerar classes](importing-schema-to-generate-classes.md). Observe que você deve trabalhar diretamente com o XML para suas instâncias de tipo. Você também pode considerar o uso de uma tecnologia de serialização diferente que dá suporte a um grande intervalo de esquema – consulte o tópico sobre como usar o `XmlSerializer` .  
  
- Talvez você queira reutilizar os `IXmlSerializable` tipos existentes no proxy em vez de gerar novos. Nesse caso, o recurso de tipos referenciados descrito no tópico importando o esquema para gerar tipos pode ser usado para indicar o tipo a ser reutilizado. Isso corresponde ao uso da `/reference` opção em svcutil. exe, que especifica o assembly que contém os tipos a serem reutilizados.  
  
## <a name="representing-arbitrary-xml-in-data-contracts"></a>Representando XML arbitrário em contratos de dados  
 A `XmlElement` matriz e os `XmlNode` `IXmlSerializable` tipos permitem injetar XML arbitrário no modelo de contrato de dados. O `DataContractSerializer` e `NetDataContractSerializer` passar esse conteúdo XML para o gravador de XML em uso, sem interferir no processo. No entanto, os gravadores XML podem impor determinadas restrições no XML que gravam. Especificamente, aqui estão alguns exemplos importantes:  
  
- Os gravadores de XML normalmente não permitem uma declaração de documento XML (por exemplo, \<?xml version=’1.0’ ?> ) no meio da gravação de outro documento. Você não pode usar um documento XML completo e serializá-lo como um `Array` `XmlNode` membro de dados. Para fazer isso, você precisa remover a declaração do documento ou usar seu próprio esquema de codificação para representá-la.  
  
- Todos os gravadores XML fornecidos com o WCF rejeitam instruções de processamento XML ( \<? … ?> ) e definições de tipo de documento ( \<! … > ), porque não são permitidas em mensagens SOAP. Novamente, você pode usar seu próprio mecanismo de codificação para contornar essa restrição. Se você precisar incluí-los em seu XML resultante, poderá escrever um codificador personalizado que usa os gravadores XML que dão suporte a eles.  
  
- Ao implementar `WriteXml` , evite chamar <xref:System.Xml.XmlWriter.WriteRaw%2A> o método no gravador de XML. O WCF usa uma variedade de codificações XML (incluindo Binary), é muito difícil ou impossível de usar `WriteRaw` , de modo que o resultado seja utilizável em qualquer codificação.  
  
- Ao implementar `WriteXml` , evite usar os <xref:System.Xml.XmlWriter.WriteEntityRef%2A> <xref:System.Xml.XmlWriter.WriteNmToken%2A> métodos e que não têm suporte nos gravadores de XML fornecidos com o WCF.  
  
## <a name="using-dataset-typed-dataset-and-datatable"></a>Usando DataSet, DataSet tipado e DataTable  
 O uso desses tipos tem suporte total no modelo de contrato de dados. Ao usar esses tipos, considere os seguintes pontos:  
  
- O esquema para esses tipos (especialmente <xref:System.Data.DataSet> e suas classes derivadas tipadas) pode não ser interoperável com algumas plataformas não WCF ou pode resultar em má usabilidade quando usado com essas plataformas. Além disso, o uso do `DataSet` tipo pode ter implicações de desempenho. Por fim, pode dificultar a versão do seu aplicativo no futuro. Considere o uso de tipos de contrato de dados definidos explicitamente em vez de `DataSet` tipos em seus contratos.  
  
- Ao importar `DataSet` ou `DataTable` esquema, é importante fazer referência a esses tipos. Com a ferramenta de linha de comando svcutil. exe, isso pode ser feito passando o nome do assembly System. Data. dll para o `/reference` comutador. Ao importar o esquema do conjunto de texto tipado, você deve referenciar o tipo do conjunto de texto tipado. Com svcutil. exe, passe o local do assembly do conjunto de módulos tipado para a `/reference` opção. Para obter mais informações sobre tipos de referência, consulte o [esquema de importação para gerar classes](importing-schema-to-generate-classes.md).  
  
 O suporte para datasets tipados no modelo de contrato de dados é limitado. Os conjuntos de linhas tipados podem ser serializados e desserializados e podem exportar seu esquema. No entanto, a importação de esquema de contrato de dados não é capaz de gerar novos tipos de DataSet tipados a partir do esquema, pois ele só pode reutilizar os existentes. Você pode apontar para um DataSet tipado existente usando a `/r` opção de svcutil. exe. Se você tentar usar um svcutil. exe sem a `/r` opção em um serviço que usa um dataset tipado, um serializador alternativo (XmlSerializer) será selecionado automaticamente. Se você precisar usar o DataContractSerializer e precisar gerar conjuntos de valores do esquema, poderá usar o seguinte procedimento: gerar os tipos de conjunto de valores tipados (usando a ferramenta xsd. exe com a `/d` opção no serviço), compilar os tipos e, em seguida, apontar para eles usando a `/r` opção svcutil. exe.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Xml.Serialization.IXmlSerializable>
- [Usando contratos de dados](using-data-contracts.md)
- [Tipos com suporte fornecido pelo serializador de contrato de dados](types-supported-by-the-data-contract-serializer.md)
