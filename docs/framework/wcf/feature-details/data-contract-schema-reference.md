---
title: Referência de esquema de contrato de dados
ms.date: 03/30/2017
helpviewer_keywords:
- data contracts [WCF], schema reference
ms.assetid: 9ebb0ebe-8166-4c93-980a-7c8f1f38f7c0
ms.openlocfilehash: 04d1f753e5788460404942a21a29e1612f674e90
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593562"
---
# <a name="data-contract-schema-reference"></a>Referência de esquema de contrato de dados

Este tópico descreve o subconjunto do esquema XML (XSD) usado pelo <xref:System.Runtime.Serialization.DataContractSerializer> para descrever tipos de Common Language Runtime (CLR) para serialização de XML.

## <a name="datacontractserializer-mappings"></a>Mapeamentos do DataContractSerializer

O `DataContractSerializer` mapeia os tipos CLR para xsd quando os metadados são exportados de um serviço de Windows Communication Foundation (WCF) usando um ponto de extremidade de metadados ou a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). Para obter mais informações, consulte [serializador de contrato de dados](data-contract-serializer.md).

O `DataContractSerializer` também MAPEIA XSD para tipos CLR quando svcutil. exe é usado para acessar documentos WSDL (linguagem de descrição de serviços Web) ou xsd e gerar contratos de dados para serviços ou clientes.

Somente as instâncias de esquema XML que estão em conformidade com os requisitos declarados neste documento podem ser mapeadas para tipos CLR usando o `DataContractSerializer` .

### <a name="support-levels"></a>Níveis de suporte

O `DataContractSerializer` fornece os seguintes níveis de suporte para um determinado recurso de esquema XML:

- **Com suporte**. Há um mapeamento explícito desse recurso para tipos CLR ou atributos (ou ambos) usando `DataContractSerializer` .

- **Ignorado**. O recurso é permitido em esquemas importados pelo `DataContractSerializer` , mas não tem efeito sobre a geração de código.

- **Proibido**. O `DataContractSerializer` não oferece suporte à importação de um esquema usando o recurso. Por exemplo, svcutil. exe, ao acessar um WSDL com um esquema que usa esse recurso, volta a usar o <xref:System.Xml.Serialization.XmlSerializer> em vez disso. Isso é por padrão.

## <a name="general-information"></a>Informações gerais

- O namespace do esquema é descrito em [esquema XML](https://www.w3.org/2001/XMLSchema). O prefixo "XS" é usado neste documento.

- Todos os atributos com um namespace não esquema são ignorados.

- Todas as anotações (exceto aquelas descritas neste documento) são ignoradas.

### <a name="xsschema-attributes"></a>\<xs:schema>: atributos

|Atributo|DataContract|
|---------------|------------------|
|`attributeFormDefault`|Ignorado.|
|`blockDefault`|Ignorado.|
|`elementFormDefault`|Deve ser qualificado. Todos os elementos devem ser qualificados para que um esquema seja suportado pelo `DataContractSerializer` . Isso pode ser feito por meio da definição xs:schema/@elementFormDefault de "qualified" ou pela definição xs:element/@form de "qualified" em cada declaração de elemento individual.|
|`finalDefault`|Ignorado.|
|`Id`|Ignorado.|
|`targetNamespace`|Com suporte e mapeado para o namespace de contrato de dados. Se esse atributo não for especificado, o namespace em branco será usado. Não pode ser o namespace reservado `http://schemas.microsoft.com/2003/10/Serialization/` .|
|`version`|Ignorado.|

### <a name="xsschema-contents"></a>\<xs:schema>: conteúdo

|Sumário|Esquema|
|--------------|------------|
|`include`|Com suporte. `DataContractSerializer`dá suporte a xs: include e xs: import. No entanto, svcutil. exe restringe o seguinte `xs:include/@schemaLocation` e `xs:import/@location` faz referência quando os metadados são carregados de um arquivo local. A lista de arquivos de esquema deve ser passada por meio de um mecanismo fora de banda e não `include` nesse caso; `include` os documentos de esquema d são ignorados.|
|`redefine`|Negado. O uso de `xs:redefine` é proibido por `DataContractSerializer` por motivos de segurança `x:redefine` : `schemaLocation` requer que seja seguido. Em determinadas circunstâncias, svcutil. exe usando DataContract restringe o uso de `schemaLocation` .|
|`import`|Com suporte. `DataContractSerializer`dá suporte a `xs:include` e `xs:import` . No entanto, svcutil. exe restringe o seguinte `xs:include/@schemaLocation` e `xs:import/@location` faz referência quando os metadados são carregados de um arquivo local. A lista de arquivos de esquema deve ser passada por meio de um mecanismo fora de banda e não `include` nesse caso; `include` os documentos de esquema d são ignorados.|
|`simpleType`|Com suporte. Consulte a `xs:simpleType` seção.|
|`complexType`|Com suporte, mapeia para contratos de dados. Consulte a `xs:complexType` seção.|
|`group`|Ignorado. `DataContractSerializer`não oferece suporte ao uso de `xs:group` , `xs:attributeGroup` e `xs:attribute` . Essas declarações são ignoradas como filhos de `xs:schema` , mas não podem ser referenciadas de dentro `complexType` ou de outras construções com suporte.|
|`attributeGroup`|Ignorado. `DataContractSerializer`não oferece suporte ao uso de `xs:group` , `xs:attributeGroup` e `xs:attribute` . Essas declarações são ignoradas como filhos de `xs:schema` , mas não podem ser referenciadas de dentro `complexType` ou de outras construções com suporte.|
|`element`|Com suporte. Consulte declaração de elemento global (teste).|
|`attribute`|Ignorado. `DataContractSerializer`não oferece suporte ao uso de `xs:group` , `xs:attributeGroup` e `xs:attribute` . Essas declarações são ignoradas como filhos de `xs:schema` , mas não podem ser referenciadas de dentro `complexType` ou de outras construções com suporte.|
|`notation`|Ignorado.|

## <a name="complex-types--xscomplextype"></a>Tipos complexos –\<xs:complexType>

### <a name="general-information"></a>Informações gerais

Cada tipo complexo é \<xs:complexType> mapeado para um contrato de dados.

### <a name="xscomplextype-attributes"></a>\<xs:complexType>: atributos

|Atributo|Esquema|
|---------------|------------|
|`abstract`|Deve ser false (padrão).|
|`block`|Negado.|
|`final`|Ignorado.|
|`id`|Ignorado.|
|`mixed`|Deve ser false (padrão).|
|`name`|Com suporte e mapeado para o nome do contrato de dados. Se houver períodos no nome, será feita uma tentativa de mapear o tipo para um tipo interno. Por exemplo, um tipo complexo chamado é `A.B` mapeado para um tipo de contrato de dados que é um tipo interno de um tipo com o nome do contrato de dados `A` , mas somente se existir um tipo de contrato de dados. Mais de um nível de aninhamento é possível: por exemplo, `A.B.C` pode ser um tipo interno, mas somente se `A` e `A.B` ambos existirem.|

### <a name="xscomplextype-contents"></a>\<xs:complexType>: conteúdo

|Sumário|Esquema|
|--------------|------------|
|`simpleContent`|As extensões são proibidas.<br /><br /> A restrição só é permitida a partir de `anySimpleType` .|
|`complexContent`|Com suporte. Consulte "herança".|
|`group`|Negado.|
|`all`|Negado.|
|`choice`|Proibido|
|`sequence`|Com suporte, mapeia para membros de dados de um contrato de dados.|
|`attribute`|Proibido, mesmo que use = "proibido" (com uma exceção). Há suporte apenas para atributos opcionais do namespace de esquema de serialização padrão. Eles não são mapeados para membros de dados no modelo de programação de contrato de dados. Atualmente, apenas um desses atributos tem significado e é discutido na seção ISerializable. Todos os outros são ignorados.|
|`attributeGroup`|Negado. Na versão v1 do WCF, `DataContractSerializer` o ignora a presença de `attributeGroup` Inside `xs:complexType` .|
|`anyAttribute`|Negado.|
|(vazio)|Mapeia para um contrato de dados sem membros de dados.|

### <a name="xssequence-in-a-complex-type-attributes"></a>\<xs:sequence>em um tipo complexo: atributos

|Atributo|Esquema|
|---------------|------------|
|`id`|Ignorado.|
|`maxOccurs`|Deve ser 1 (padrão).|
|`minOccurs`|Deve ser 1 (padrão).|

### <a name="xssequence-in-a-complex-type-contents"></a>\<xs:sequence>em um tipo complexo: conteúdo

|Sumário|Esquema|
|--------------|------------|
|`element`|Cada instância é mapeada para um membro de dados.|
|`group`|Negado.|
|`choice`|Negado.|
|`sequence`|Negado.|
|`any`|Negado.|
|(vazio)|Mapeia para um contrato de dados sem membros de dados.|

## <a name="elements--xselement"></a>Elementos\<xs:element>

### <a name="general-information"></a>Informações gerais

`<xs:element>`pode ocorrer nos seguintes contextos:

- Ele pode ocorrer em um `<xs:sequence>` , que descreve um membro de dados de um contrato de dados regular (não coleção). Nesse caso, o `maxOccurs` atributo deve ser 1. (Um valor de 0 não é permitido).

- Ele pode ocorrer em um `<xs:sequence>` , que descreve um membro de dados de um contrato de dados de coleção. Nesse caso, o `maxOccurs` atributo deve ser maior que 1 ou "não associado".

- Ele pode ocorrer dentro de uma `<xs:schema>` como uma declaração de elemento global (teste).

### <a name="xselement-with-maxoccurs1-within-an-xssequence-data-members"></a>\<xs:element>com maxOccurs = 1 em um \<xs:sequence> (membros de dados)

|Atributo|Esquema|
|---------------|------------|
|`ref`|Negado.|
|`name`|Com suporte, é mapeado para o nome do membro de dados.|
|`type`|Com suporte, mapeia para o tipo de membro de dados. Para obter mais informações, consulte mapeamento de tipo/primitivo. Se não for especificado (e o elemento não contiver um tipo anônimo), `xs:anyType` será assumido.|
|`block`|Ignorado.|
|`default`|Negado.|
|`fixed`|Negado.|
|`form`|Deve ser qualificado. Esse atributo pode ser definido por meio `elementFormDefault` de on `xs:schema` .|
|`id`|Ignorado.|
|`maxOccurs`|1|
|`minOccurs`|Mapeia para a <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> propriedade de um membro de dados ( `IsRequired` é verdadeiro quando `minOccurs` é 1).|
|`nillable`|Afeta o mapeamento de tipo. Consulte mapeamento de tipo/primitivo.|

### <a name="xselement-with-maxoccurs1-within-an-xssequence-collections"></a>\<xs:element>com maxOccurs>1 em um \<xs:sequence> (coleções)

- Mapeia para um <xref:System.Runtime.Serialization.CollectionDataContractAttribute> .

- Em tipos de coleção, apenas um xs: Element é permitido dentro de uma sequência xs:.

 As coleções podem ser dos seguintes tipos:

- Coleções regulares (por exemplo, matrizes).

- Coleções de dicionário (mapeando um valor para outro; por exemplo, a <xref:System.Collections.Hashtable> ).

- A única diferença entre um dicionário e uma matriz de um tipo de par chave/valor está no modelo de programação gerado. Há um mecanismo de anotação de esquema que pode ser usado para indicar que um determinado tipo é uma coleção de dicionários.

As regras para os `ref` atributos,,,, `block` `default` `fixed` `form` e `id` são as mesmas para o caso não coleção. Outros atributos incluem os da tabela a seguir.

|Atributo|Esquema|
|---------------|------------|
|`name`|Com suporte, mapeia para a <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ItemName%2A> propriedade no `CollectionDataContractAttribute` atributo.|
|`type`|Com suporte, é mapeado para o tipo armazenado na coleção.|
|`maxOccurs`|Maior que 1 ou "não associado". O esquema de DC deve usar "não associado".|
|`minOccurs`|Ignorado.|
|`nillable`|Afeta o mapeamento de tipo. Esse atributo é ignorado para coleções de dicionário.|

### <a name="xselement-within-an-xsschema-global-element-declaration"></a>\<xs:element>dentro de uma \<xs:schema> declaração de elemento global

- Uma declaração de elemento global (teste) que tem o mesmo nome e namespace como um tipo no esquema, ou que define um tipo anônimo dentro dele mesmo, é considerada associada ao tipo.

- Exportação de esquema: os GEDs associados são gerados para cada tipo gerado, simples e complexo.

- Desserialização/serialização: os GEDs associados são usados como elementos raiz para o tipo.

- Importação de esquema: os GEDs associados não são necessários e serão ignorados se seguirem as seguintes regras (a menos que eles definam tipos).

|Atributo|Esquema|
|---------------|------------|
|`abstract`|Deve ser false para GEDs associados.|
|`block`|Proibido no GEDs associado.|
|`default`|Proibido no GEDs associado.|
|`final`|Deve ser false para GEDs associados.|
|`fixed`|Proibido no GEDs associado.|
|`id`|Ignorado.|
|`name`|Com suporte. Consulte a definição de GEDs associada.|
|`nillable`|Deve ser verdadeiro para GEDs associados.|
|`substitutionGroup`|Proibido no GEDs associado.|
|`type`|Com suporte, e deve corresponder ao tipo associado para GEDs associado (a menos que o elemento contenha um tipo anônimo).|

### <a name="xselement-contents"></a>\<xs:element>: conteúdo

|Sumário|Esquema|
|--------------|------------|
|`simpleType`|Com suporte. *|
|`complexType`|Com suporte. *|
|`unique`|Ignorado.|
|`key`|Ignorado.|
|`keyref`|Ignorado.|
|(blank)|Com suporte.|

\*Ao usar o `simpleType` `complexType,` mapeamento e para tipos anônimos é o mesmo que para tipos não anônimos, exceto pelo fato de que não há nenhum contrato de dados anônimo e, portanto, um acordo de dados nomeado é criado, com um nome gerado derivado do nome do elemento. As regras para tipos anônimos estão na lista a seguir:

- Detalhe da implementação do WCF: se o `xs:element` nome não contiver pontos, o tipo anônimo será mapeado para um tipo interno do tipo de contrato de dados externo. Se o nome contiver pontos, o tipo de contrato de dados resultante será independente (não um tipo interno).

- O nome do contrato de dados gerado do tipo interno é o nome do contrato de dados do tipo externo seguido de um ponto, o nome do elemento e a cadeia de caracteres "tipo".

- Se já existir um contrato de dados com esse nome, o nome será exclusivo acrescentando "1", "2", "3" e assim por diante até que um nome exclusivo seja criado.

## <a name="simple-types---xssimpletype"></a>Tipos simples-\<xs:simpleType>

### <a name="xssimpletype-attributes"></a>\<xs:simpleType>: atributos

|Atributo|Esquema|
|---------------|------------|
|`final`|Ignorado.|
|`id`|Ignorado.|
|`name`|Com suporte, é mapeado para o nome do contrato de dados.|

### <a name="xssimpletype-contents"></a>\<xs:simpleType>: conteúdo

|Sumário|Esquema|
|--------------|------------|
|`restriction`|Com suporte. Mapeia para contratos de dados de enumeração. Esse atributo será ignorado se não corresponder ao padrão de enumeração. Consulte a `xs:simpleType` seção restrições.|
|`list`|Com suporte. Mapeia para sinalizar contratos de dados de enumeração. Consulte a `xs:simpleType` seção listas.|
|`union`|Negado.|

### \<xs:restriction>

- Há suporte para restrições de tipo complexo apenas para base = " `xs:anyType` ".

- Restrições de tipo simples de `xs:string` que não têm facetas de restrição diferentes de `xs:enumeration` são mapeadas para contratos de dados de enumeração.

- Todas as outras restrições de tipo simples são mapeadas para os tipos que elas restringem. Por exemplo, uma restrição de `xs:int` é mapeada para um número inteiro, da mesma forma como `xs:int` faz. Para obter mais informações sobre mapeamento de tipo primitivo, consulte mapeamento de tipo/primitivo.

### <a name="xsrestriction-attributes"></a>\<xs:restriction>: atributos

|Atributo|Esquema|
|---------------|------------|
|`base`|Deve ser um tipo simples com suporte ou `xs:anyType` .|
|`id`|Ignorado.|

### <a name="xsrestriction-for-all-other-cases-contents"></a>\<xs:restriction>para todos os outros casos: conteúdo

|Sumário|Esquema|
|--------------|------------|
|`simpleType`|Se presente, deve ser derivado de um tipo primitivo com suporte.|
|`minExclusive`|Ignorado.|
|`minInclusive`|Ignorado.|
|`maxExclusive`|Ignorado.|
|`maxInclusive`|Ignorado.|
|`totalDigits`|Ignorado.|
|`fractionDigits`|Ignorado.|
|`length`|Ignorado.|
|`minLength`|Ignorado.|
|`maxLength`|Ignorado.|
|`enumeration`|Ignorado.|
|`whiteSpace`|Ignorado.|
|`pattern`|Ignorado.|
|(blank)|Com suporte.|

## <a name="enumeration"></a>Enumeração

### <a name="xsrestriction-for-enumerations-attributes"></a>\<xs:restriction>para enumerações: atributos

|Atributo|Esquema|
|---------------|------------|
|`base`|Se presente, deve ser `xs:string` .|
|`id`|Ignorado.|

### <a name="xsrestriction-for-enumerations-contents"></a>\<xs:restriction>para enumerações: conteúdo

|Sumário|Esquema|
|--------------|------------|
|`simpleType`|Se presente, deve ser uma restrição de enumeração com suporte do contrato de dados (esta seção).|
|`minExclusive`|Ignorado.|
|`minInclusive`|Ignorado.|
|`maxExclusive`|Ignorado.|
|`maxInclusive`|Ignorado.|
|`totalDigits`|Ignorado.|
|`fractionDigits`|Ignorado.|
|`length`|Negado.|
|`minLength`|Negado.|
|`maxLength`|Negado.|
|`enumeration`|Com suporte. A enumeração "ID" é ignorada e "valor" é mapeado para o nome do valor no contrato de dados de enumeração.|
|`whiteSpace`|Negado.|
|`pattern`|Negado.|
|(vazio)|Com suporte, mapeia para o tipo de enumeração vazio.|

 O código a seguir mostra uma classe de enumeração C#.

```csharp
public enum MyEnum
{
  first = 3,
  second = 4,
  third =5
}
```

Essa classe é mapeada para o esquema a seguir pelo `DataContractSerializer` . Se os valores de enumeração começarem de 1, os `xs:annotation` blocos não serão gerados.

```xml
<xs:simpleType name="MyEnum">
  <xs:restriction base="xs:string">
    <xs:enumeration value="first">
      <xs:annotation>
        <xs:appinfo>
          <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
          3
          </EnumerationValue>
        </xs:appinfo>
      </xs:annotation>
    </xs:enumeration>
    <xs:enumeration value="second">
      <xs:annotation>
        <xs:appinfo>
          <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
          4
          </EnumerationValue>
        </xs:appinfo>
      </xs:annotation>
    </xs:enumeration>
  </xs:restriction>
</xs:simpleType>
```

### \<xs:list>

`DataContractSerializer`mapeia tipos de enumeração marcados com `System.FlagsAttribute` para `xs:list` derivado de `xs:string` . Não há `xs:list` suporte para nenhuma outra variação.

### <a name="xslist-attributes"></a>\<xs:list>: atributos

|Atributo|Esquema|
|---------------|------------|
|`itemType`|Negado.|
|`id`|Ignorado.|

### <a name="xslist-contents"></a>\<xs:list>: conteúdo

|Sumário|Esquema|
|--------------|------------|
|`simpleType`|Deve ser restrição do `xs:string` uso da `xs:enumeration` faceta.|

Se o valor de enumeração não seguir uma potência de 2 progressão (padrão para sinalizadores), o valor será armazenado no `xs:annotation/xs:appInfo/ser:EnumerationValue` elemento.

Por exemplo, o código a seguir sinaliza um tipo de enumeração.

```csharp
[Flags]
public enum AuthFlags
{
  AuthAnonymous = 1,
  AuthBasic = 2,
  AuthNTLM = 4,
  AuthMD5 = 16,
  AuthWindowsLiveID = 64,
}
```

Esse tipo é mapeado para o esquema a seguir.

```xml
<xs:simpleType name="AuthFlags">
    <xs:list>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:enumeration value="AuthAnonymous" />
          <xs:enumeration value="AuthBasic" />
          <xs:enumeration value="AuthNTLM" />
          <xs:enumeration value="AuthMD5">
            <xs:annotation>
              <xs:appinfo>
                <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">16</EnumerationValue>
              </xs:appinfo>
            </xs:annotation>
          </xs:enumeration>
          <xs:enumeration value="AuthWindowsLiveID">
            <xs:annotation>
              <xs:appinfo>
                <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">64</EnumerationValue>
              </xs:appinfo>
            </xs:annotation>
          </xs:enumeration>
        </xs:restriction>
      </xs:simpleType>
    </xs:list>
  </xs:simpleType>
```

## <a name="inheritance"></a>Herança

### <a name="general-rules"></a>Regras gerais

Um contrato de dados pode herdar de outro contrato de dados. Esses contratos de dados são mapeados para uma base e são derivados por tipos de extensão usando a `<xs:extension>` construção de esquema XML.

Um contrato de dados não pode herdar de um contrato de dados de coleção.

Por exemplo, o código a seguir é um contrato de dados.

```csharp
[DataContract]
public class Person
{
  [DataMember]
  public string Name;
}
[DataContract]
public class Employee : Person
{
  [DataMember]
  public int ID;
}
```

Este contrato de dados é mapeado para a seguinte declaração de tipo de esquema XML.

```xml
<xs:complexType name="Employee">
 <xs:complexContent mixed="false">
  <xs:extension base="tns:Person">
   <xs:sequence>
    <xs:element minOccurs="0" name="ID" type="xs:int"/>
   </xs:sequence>
  </xs:extension>
 </xs:complexContent>
</xs:complexType>
<xs:complexType name="Person">
 <xs:sequence>
  <xs:element minOccurs="0" name="Name"
    nillable="true" type="xs:string"/>
 </xs:sequence>
</xs:complexType>
```

### <a name="xscomplexcontent-attributes"></a>\<xs:complexContent>: atributos

|Atributo|Esquema|
|---------------|------------|
|`id`|Ignorado.|
|`mixed`|Deve ser false.|

### <a name="xscomplexcontent-contents"></a>\<xs:complexContent>: conteúdo

|Sumário|Esquema|
|--------------|------------|
|`restriction`|Proibido, exceto quando base = " `xs:anyType` ". O último é equivalente a colocar o conteúdo do `xs:restriction` diretamente sob o contêiner do `xs:complexContent` .|
|`extension`|Com suporte. Mapeia para herança de contrato de dados.|

### <a name="xsextension-in-xscomplexcontent-attributes"></a>\<xs:extension>em \<xs:complexContent> : atributos

|Atributo|Esquema|
|---------------|------------|
|`id`|Ignorado.|
|`base`|Com suporte. Mapeia para o tipo de contrato de dados base do qual este tipo herda.|

### <a name="xsextension-in-xscomplexcontent-contents"></a>\<xs:extension>em \<xs:complexContent> : conteúdo

As regras são as mesmas para o `<xs:complexType>` conteúdo.

Se um `<xs:sequence>` for fornecido, seus elementos de membro serão mapeados para membros de dados adicionais que estão presentes no contrato de dados derivado.

Se um tipo derivado contiver um elemento com o mesmo nome de um elemento em um tipo base, a declaração de elemento duplicado será mapeada para um membro de dados com um nome que é gerado para ser exclusivo. Números inteiros positivos são adicionados ao nome do membro de dados ("member1", "membro2" e assim por diante) até que um nome exclusivo seja encontrado. Por outro lado

- Se um contrato de dados derivado tiver um membro de dados com o mesmo nome e tipo como um membro de dados em um contrato de dados base, o `DataContractSerializer` gerará esse elemento correspondente no tipo derivado.

- Se um contrato de dados derivado tiver um membro de dados com o mesmo nome que um membro de dados em um contrato de dados base, mas um tipo diferente, o `DataContractSerializer` importará um esquema com um elemento do tipo `xs:anyType` no tipo base e nas declarações de tipo derivado. O nome do tipo original é preservado em `xs:annotations/xs:appInfo/ser:ActualType/@Name` .

Ambas as variações podem levar a um esquema com um modelo de conteúdo ambíguo, que depende da ordem dos respectivos membros de dados.

## <a name="typeprimitive-mapping"></a>Mapeamento de tipo/primitivo

O `DataContractSerializer` usa o mapeamento a seguir para tipos primitivos de esquema XML.

|Tipo XSD|Tipo .NET|
|--------------|---------------|
|`anyType`|<xref:System.Object>.|
|`anySimpleType`|<xref:System.String>.|
|`duration`|<xref:System.TimeSpan>.|
|`dateTime`|<xref:System.DateTime>.|
|`dateTimeOffset`|<xref:System.DateTime>e <xref:System.TimeSpan> para o deslocamento. Consulte a serialização de DateTimeOffset abaixo.|
|`time`|<xref:System.String>.|
|`date`|<xref:System.String>.|
|`gYearMonth`|<xref:System.String>.|
|`gYear`|<xref:System.String>.|
|`gMonthDay`|<xref:System.String>.|
|`gDay`|<xref:System.String>.|
|`gMonth`|<xref:System.String>.|
|`boolean`|<xref:System.Boolean>|
|`base64Binary`|Matriz <xref:System.Byte>.|
|`hexBinary`|<xref:System.String>.|
|`float`|<xref:System.Single>.|
|`double`|<xref:System.Double>.|
|`anyURI`|<xref:System.Uri>.|
|`QName`|<xref:System.Xml.XmlQualifiedName>.|
|`string`|<xref:System.String>.|
|`normalizedString`|<xref:System.String>.|
|`token`|<xref:System.String>.|
|`language`|<xref:System.String>.|
|`Name`|<xref:System.String>.|
|`NCName`|<xref:System.String>.|
|`ID`|<xref:System.String>.|
|`IDREF`|<xref:System.String>.|
|`IDREFS`|<xref:System.String>.|
|`ENTITY`|<xref:System.String>.|
|`ENTITIES`|<xref:System.String>.|
|`NMTOKEN`|<xref:System.String>.|
|`NMTOKENS`|<xref:System.String>.|
|`decimal`|<xref:System.Decimal>.|
|`integer`|<xref:System.Int64>.|
|`nonPositiveInteger`|<xref:System.Int64>.|
|`negativeInteger`|<xref:System.Int64>.|
|`long`|<xref:System.Int64>.|
|`int`|<xref:System.Int32>.|
|`short`|<xref:System.Int16>.|
|`Byte`|<xref:System.SByte>.|
|`nonNegativeInteger`|<xref:System.Int64>.|
|`unsignedLong`|<xref:System.UInt64>.|
|`unsignedInt`|<xref:System.UInt32>.|
|`unsignedShort`|<xref:System.UInt16>.|
|`unsignedByte`|<xref:System.Byte>.|
|`positiveInteger`|<xref:System.Int64>.|

## <a name="iserializable-types-mapping"></a>Mapeamento de tipos ISerializable

No .NET Framework versão 1,0, <xref:System.Runtime.Serialization.ISerializable> foi introduzido como um mecanismo geral para serializar objetos para persistência ou transferência de dados. Há muitos tipos de .NET Framework que implementam `ISerializable` e que podem ser passados entre aplicativos. <xref:System.Runtime.Serialization.DataContractSerializer>Naturalmente, o oferece suporte para `ISerializable` classes. Os `DataContractSerializer` tipos de esquema de implementação de mapas `ISerializable` que diferem somente pelo QName (nome qualificado) do tipo e são efetivamente coleções de propriedades. Por exemplo, o `DataContractSerializer` mapeia <xref:System.Exception> para o seguinte tipo xsd no `http://schemas.datacontract.org/2004/07/System` namespace.

```xml
<xs:complexType name="Exception">
 <xs:sequence>
  <xs:any minOccurs="0" maxOccurs="unbounded"
      namespace="##local" processContents="skip"/>
 </xs:sequence>
 <xs:attribute ref="ser:FactoryType"/>
</xs:complexType>
```

O atributo opcional `ser:FactoryType` declarado no esquema de serialização do contrato de dados faz referência a uma classe de fábrica que pode desserializar o tipo. A classe de fábrica deve fazer parte da coleção de tipos conhecidos da `DataContractSerializer` instância que está sendo usada. Para obter mais informações sobre tipos conhecidos, consulte [tipos conhecidos de contrato de dados](data-contract-known-types.md).

## <a name="datacontract-serialization-schema"></a>Esquema de serialização DataContract

Vários esquemas exportados pelos `DataContractSerializer` tipos, elementos e atributos de uso de um namespace de serialização de contrato de dados especial:

`http://schemas.microsoft.com/2003/10/Serialization`

A seguir está uma declaração de esquema de serialização de contrato de dados completa.

```xml
<xs:schema attributeFormDefault="qualified"
   elementFormDefault="qualified"
   targetNamespace =
    "http://schemas.microsoft.com/2003/10/Serialization/"
   xmlns:xs="http://www.w3.org/2001/XMLSchema"
   xmlns:tns="http://schemas.microsoft.com/2003/10/Serialization/">

 <!-- Top-level elements for primitive types. -->
 <xs:element name="anyType" nillable="true" type="xs:anyType"/>
 <xs:element name="anyURI" nillable="true" type="xs:anyURI"/>
 <xs:element name="base64Binary"
       nillable="true" type="xs:base64Binary"/>
 <xs:element name="boolean" nillable="true" type="xs:boolean"/>
 <xs:element name="byte" nillable="true" type="xs:byte"/>
 <xs:element name="dateTime" nillable="true" type="xs:dateTime"/>
 <xs:element name="decimal" nillable="true" type="xs:decimal"/>
 <xs:element name="double" nillable="true" type="xs:double"/>
 <xs:element name="float" nillable="true" type="xs:float"/>
 <xs:element name="int" nillable="true" type="xs:int"/>
 <xs:element name="long" nillable="true" type="xs:long"/>
 <xs:element name="QName" nillable="true" type="xs:QName"/>
 <xs:element name="short" nillable="true" type="xs:short"/>
 <xs:element name="string" nillable="true" type="xs:string"/>
 <xs:element name="unsignedByte"
       nillable="true" type="xs:unsignedByte"/>
 <xs:element name="unsignedInt"
       nillable="true" type="xs:unsignedInt"/>
 <xs:element name="unsignedLong"
       nillable="true" type="xs:unsignedLong"/>
 <xs:element name="unsignedShort"
       nillable="true" type="xs:unsignedShort"/>

 <!-- Primitive types introduced for certain .NET simple types. -->
 <xs:element name="char" nillable="true" type="tns:char"/>
 <xs:simpleType name="char">
  <xs:restriction base="xs:int"/>
 </xs:simpleType>

 <!-- xs:duration is restricted to an ordered value space,
    to map to System.TimeSpan -->
 <xs:element name="duration" nillable="true" type="tns:duration"/>
 <xs:simpleType name="duration">
  <xs:restriction base="xs:duration">
   <xs:pattern
     value="\-?P(\d*D)?(T(\d*H)?(\d*M)?(\d*(\.\d*)?S)?)?"/>
   <xs:minInclusive value="-P10675199DT2H48M5.4775808S"/>
   <xs:maxInclusive value="P10675199DT2H48M5.4775807S"/>
  </xs:restriction>
 </xs:simpleType>

 <xs:element name="guid" nillable="true" type="tns:guid"/>
 <xs:simpleType name="guid">
  <xs:restriction base="xs:string">
   <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>
  </xs:restriction>
 </xs:simpleType>

 <!-- This is used for schemas exported from ISerializable type. -->
 <xs:attribute name="FactoryType" type="xs:QName"/>
</xs:schema>
```

O seguinte deve ser observado:

- `ser:char`é introduzido para representar caracteres Unicode do tipo <xref:System.Char> .

- O `valuespace` de `xs:duration` é reduzido para um conjunto ordenado para que ele possa ser mapeado para um <xref:System.TimeSpan> .

- `FactoryType`é usado em esquemas exportados de tipos derivados de <xref:System.Runtime.Serialization.ISerializable> .

## <a name="importing-non-datacontract-schemas"></a>Importando esquemas não DataContract

`DataContractSerializer`tem a `ImportXmlTypes` opção de permitir a importação de esquemas que não estão de acordo com o `DataContractSerializer` perfil XSD (consulte a <xref:System.Runtime.Serialization.XsdDataContractImporter.Options%2A> Propriedade). Definir essa opção para `true` habilitar a aceitação de tipos de esquema não conformes e de mapeamento para a implementação a seguir, <xref:System.Xml.Serialization.IXmlSerializable> Encapsulando uma matriz de <xref:System.Xml.XmlNode> (somente o nome da classe difere).

```csharp
[GeneratedCodeAttribute("System.Runtime.Serialization", "3.0.0.0")]
[System.Xml.Serialization.XmlSchemaProviderAttribute("ExportSchema")]
[System.Xml.Serialization.XmlRootAttribute(IsNullable=false)]
public partial class Person : object, IXmlSerializable
{
  private XmlNode[] nodesField;
  private static XmlQualifiedName typeName =
new XmlQualifiedName("Person","http://Microsoft.ServiceModel.Samples");
  public XmlNode[] Nodes
  {
    get {return this.nodesField;}
    set {this.nodesField = value;}
  }
  public void ReadXml(XmlReader reader)
  {
    this.nodesField = XmlSerializableServices.ReadNodes(reader);
  }
  public void WriteXml(XmlWriter writer)
  {
    XmlSerializableServices.WriteNodes(writer, this.Nodes);
  }
  public System.Xml.Schema.XmlSchema GetSchema()
  {
    return null;
  }
  public static XmlQualifiedName ExportSchema(XmlSchemaSet schemas)
  {
    XmlSerializableServices.AddDefaultSchema(schemas, typeName);
    return typeName;
  }
}
```

## <a name="datetimeoffset-serialization"></a>Serialização de DateTimeOffset

O <xref:System.DateTimeOffset> não é tratado como um tipo primitivo. Em vez disso, ele é serializado como um elemento complexo com duas partes. A primeira parte representa a data e hora, e a segunda parte representa o deslocamento a partir da data e hora. Um exemplo de um valor de DateTimeOffset serializado é mostrado no código a seguir.

```xml
<OffSet xmlns:a="http://schemas.datacontract.org/2004/07/System">
  <DateTime i:type="b:dateTime" xmlns=""
    xmlns:b="http://www.w3.org/2001/XMLSchema">2008-08-28T08:00:00
  </DateTime>
  <OffsetMinutes i:type="b:short" xmlns=""
   xmlns:b="http://www.w3.org/2001/XMLSchema">-480
   </OffsetMinutes>
</OffSet>
```

O esquema é o seguinte.

```xml
<xs:schema targetNamespace="http://schemas.datacontract.org/2004/07/System">
   <xs:complexType name="DateTimeOffset">
      <xs:sequence minOccurs="1" maxOccurs="1">
         <xs:element name="DateTime" type="xs:dateTime"
         minOccurs="1" maxOccurs="1" />
         <xs:element name="OffsetMinutes" type="xs:short"
         minOccurs="1" maxOccurs="1" />
      </xs:sequence>
   </xs:complexType>
</xs:schema>
```

## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- <xref:System.Runtime.Serialization.XsdDataContractImporter>
- [Usando contratos de dados](using-data-contracts.md)
