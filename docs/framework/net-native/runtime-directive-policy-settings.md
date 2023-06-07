---
title: Configurações da política da diretiva de runtime
ms.date: 03/30/2017
ms.assetid: cb52b1ef-47fd-4609-b69d-0586c818ac9e
ms.openlocfilehash: 7a8933decaec45e8000f3f3d1717847f333deddd
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "76738498"
---
# <a name="runtime-directive-policy-settings"></a>Configurações da política da diretiva de runtime

> [!NOTE]
> Este tópico refere-se ao Developer Preview do .NET Nativo, que é um software em pré-lançamento. Você pode baixar a versão prévia do [site Microsoft Connect](https://go.microsoft.com/fwlink/?LinkId=394611) (registro obrigatório).

As configurações de política de diretiva de runtime para .NET Nativo determinam a disponibilidade de metadados para tipos e membros de tipo em runtime. Sem os metadados necessários, operações que dependem de reflexão, serialização, desserialização ou marshaling dos tipos de .NET Framework para COM ou o Windows Runtime podem falhar e gerar uma exceção. As exceções mais comuns são [MissingMetadataException](missingmetadataexception-class-net-native.md) e (no caso de interoperabilidade) [MissingInteropDataException](missinginteropdataexception-class-net-native.md).

Configurações de política de runtime são controladas pelo arquivo de diretivas de runtime (.rd.xml). Cada diretiva de tempo de execução define a política para um determinado elemento do programa, como um assembly (o [\<Assembly>](assembly-element-net-native.md) elemento), um tipo (o [\<Type>](type-element-net-native.md) elemento) ou um método (o [\<Method>](method-element-net-native.md) elemento). A diretiva inclui um ou mais atributos que definem os tipos de política de reflexão, os tipos de política de serialização e os tipos de política de interoperabilidade discutidos na próxima seção. O valor do atributo define a configuração de política.

## <a name="policy-types"></a>Tipos de política

Arquivos de diretivas de runtime reconhecem três categorias de tipos de política: reflexão, serialização e interoperabilidade.

- Tipos de política de reflexão determinam quais metadados são disponibilizados no tempo de execução para reflexão:

  - O `Activate` controla o acesso de runtime a construtores para habilitar a ativação de instâncias.

  - O `Browse` controla as consultas para obter informações sobre elementos de programa.

  - O `Dynamic` controla o acesso de runtime a todos os tipos e membros para habilitar programação dinâmica.

  A tabela a seguir lista os tipos de política de reflexão e os elementos do programa com o qual podem ser usados.

  |Elemento|Ativar|Procurar|Dinâmico|
  |-------------|--------------|------------|-------------|
  |[\<Application>](application-element-net-native.md)|✔️|✔️|✔️|
  |[\<Assembly>](assembly-element-net-native.md)|✔️|✔️|✔️|
  |[\<AttributeImplies>](attributeimplies-element-net-native.md)|✔️|✔️|✔️|
  |[\<Event>](event-element-net-native.md)||✔️|✔️|
  |[\<Field>](field-element-net-native.md)||✔️|✔️|
  |[\<GenericParameter>](genericparameter-element-net-native.md)|✔️|✔️|✔️|
  |[\<ImpliesType>](impliestype-element-net-native.md)|✔️|✔️|✔️|
  |[\<Method>](method-element-net-native.md)||✔️|✔️|
  |[\<MethodInstantiation>](methodinstantiation-element-net-native.md)||✔️|✔️|
  |[\<Namespace>](namespace-element-net-native.md)|✔️|✔️|✔️|
  |[\<Parameter>](parameter-element-net-native.md)|✔️|✔️|✔️|
  |[\<Property>](property-element-net-native.md)||✔️|✔️|
  |[\<Subtypes>](subtypes-element-net-native.md)|✔️|✔️|✔️|
  |[\<Type>](type-element-net-native.md)|✔️|✔️|✔️|
  |[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|✔️|✔️|✔️|
  |[\<TypeParameter>](typeparameter-element-net-native.md)|✔️|✔️|✔️|

- Os tipos de política de serialização determinam quais metadados são disponibilizado no tempo de execução para serialização e desserialização:

  - O `Serialize` controla o acesso ao runtime para construtores, campos e propriedades para habilitar a serialização por bibliotecas de terceiros como o serializador Newtonsoft JSON.

  - O `DataContractSerializer` controla o acesso de runtime a construtores, campos e propriedades, para habilitar que instâncias de tipo sejam serializadas pela classe <xref:System.Runtime.Serialization.DataContractSerializer>.

  - O `DataContractJsonSerializer` controla o acesso de runtime a construtores, campos e propriedades, para habilitar que instâncias de tipo sejam serializadas pela classe <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>.

  - O `XmlSerializer` controla o acesso de runtime a construtores, campos e propriedades, para habilitar que instâncias de tipo sejam serializadas pela classe <xref:System.Xml.Serialization.XmlSerializer>.

  A tabela a seguir lista os tipos de política de serialização e os elementos do programa com o qual podem ser usados.

  |Elemento|Serializar|DataContractSerializer|DataContractJsonSerializer|XmlSerializer|
  |-------------|---------------|----------------------------|--------------------------------|-------------------|
  |[\<Application>](application-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Assembly>](assembly-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<AttributeImplies>](attributeimplies-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Event>](event-element-net-native.md)|||||
  |[\<Field>](field-element-net-native.md)|✔️||||
  |[\<GenericParameter>](genericparameter-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<ImpliesType>](impliestype-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Method>](method-element-net-native.md)|||||
  |[\<MethodInstantiation>](methodinstantiation-element-net-native.md)|||||
  |[\<Namespace>](namespace-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Parameter>](parameter-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Property>](property-element-net-native.md)|✔️||||
  |[\<Subtypes>](subtypes-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Type>](type-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<TypeParameter>](typeparameter-element-net-native.md)|✔️|✔️|✔️|✔️|

- Os tipos de política de interoperabilidade determinam os metadados que são disponibilizados no runtime para passar topos de referências, tipos de valor e apontadores de função ao COM e ao Windows Runtime:

  - O `MarshalObject` controla o marshaling nativo para o COM e o Windows Runtime para tipos de referência.

  - O `MarshalDelegate` controla o marshaling nativo de tipos de delegados como ponteiros de função.

  - O `MarshalStructure` controla o marshaling nativo para o COM e o Windows Runtime para tipos de valor.

  A tabela a seguir lista os tipos de política de interoperabilidade e os elementos do programa com o qual podem ser usados.

  |Elemento|MarshalObject|MarshalDelegate|MarshalStructure|
  |-------------|-------------------|---------------------|----------------------|
  |[\<Application>](application-element-net-native.md)|✔️|✔️|✔️|
  |[\<Assembly>](assembly-element-net-native.md)|✔️|✔️|✔️|
  |[\<AttributeImplies>](attributeimplies-element-net-native.md)|✔️|✔️|✔️|
  |[\<Event>](event-element-net-native.md)||||
  |[\<Field>](field-element-net-native.md)||||
  |[\<GenericParameter>](genericparameter-element-net-native.md)|✔️|✔️|✔️|
  |[\<ImpliesType>](impliestype-element-net-native.md)|✔️|✔️|✔️|
  |[\<Method>](method-element-net-native.md)||||
  |[\<MethodInstantiation>](methodinstantiation-element-net-native.md)||||
  |[\<Namespace>](namespace-element-net-native.md)|✔️|✔️|✔️|
  |[\<Parameter>](parameter-element-net-native.md)|✔️|✔️|✔️|
  |[\<Property>](property-element-net-native.md)||||
  |[\<Subtypes>](subtypes-element-net-native.md)|✔️|✔️|✔️|
  |[\<Type>](type-element-net-native.md)|✔️|✔️|✔️|
  |[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|✔️|✔️|✔️|
  |[\<TypeParameter>](typeparameter-element-net-native.md)|✔️|✔️|✔️|

## <a name="policy-settings"></a>Configurações de política

Cada tipo de política pode ser definido como um dos valores listados na tabela a seguir. Observe que elementos que representam os membros do tipo oferece suporte a um conjunto diferente de configurações de política que outros elementos.

|Configuração de política|Descrição|Elementos `Assembly`, `Namespace`, `Type` e `TypeInstantiation`|Elementos `Event`, `Field`, `Method`, `MethodInstantiation` e `Property`|
|--------------------|-----------------|-----------------------------------------------------------------------|--------------------------------------------------------------------------------|
|`All`|Habilita a política para todos os tipos e membros que a cadeia de ferramentas do .NET Nativo não remover.|✔️||
|`Auto`|Especifica que a política padrão deve ser usada para o tipo de política desse elemento de programa. Isso é idêntico a omitir uma política para esse tipo de política. O `Auto` geralmente é usado para indicar que a diretiva é herdada de um elemento pai.|✔️|✔️|
|`Excluded`|Especifica que a política está desabilitada para um elemento de programa específico. Por exemplo, a diretiva de runtime:<br /><br /> `<Type Name="BusinessClasses.Person" Browse="Excluded" Dynamic="Excluded" />`<br /><br /> Especifica que os metadados para a classe `BusinessClasses.Person` não estão disponíveis para busca ou para instanciar e modificar dinamicamente objetos `Person`.|✔️|✔️|
|`Included`|Habilita uma política se os metadados para o tipo de pai estiverem disponíveis.||✔️|
|`Public`|Habilita a política para tipos públicos ou membros, a menos que a cadeia de ferramentas determine que o tipo ou membro é desnecessário e, portanto, remove-o. Essa configuração é diferente de `Required Public`, que garante que os metadados para membros e tipos públicos estão sempre disponíveis mesmo se a cadeia de ferramentas determinar que não são necessários.|✔️||
|`PublicAndInternal`|Habilita a política para tipos ou membros públicos e internos, a menos que a cadeia de ferramentas determine que o tipo ou membro é desnecessário e, portanto, remove-o. Essa configuração é diferente de `Required PublicAndInternal`, que garante que os metadados para membros e tipos públicos e internos estão sempre disponíveis mesmo se a cadeia de ferramentas determinar que não são necessários.|✔️||
|`Required`|Especifica que a política de membro está habilitada e os metadados estarão disponíveis mesmo se o membro parecer ser usado.||✔️|
|`Required Public`|Habilita a política para tipos ou membros públicos e garante que os metadados para tipos e membros públicos estejam sempre disponíveis. Essa configuração é diferente de `Public`, disponibilizando metadados para tipos e membros públicos somente se a cadeia de ferramentas determinar que é necessário.|✔️||
|`Required PublicAndInternal`|Habilita a política para tipos ou membros públicos e internos e garante que os metadados para tipos e membros públicos e internos estejam sempre disponíveis. Essa configuração é diferente de `PublicAndInternal`, disponibilizando metadados para tipos e membros públicos e internos somente se a cadeia de ferramentas determinar que é necessário.|✔️||
|`Required All`|Necessita da cadeia de ferramentas para manter todos os tipos e membros, sejam usados ou não, e habilita a política para eles.|✔️||

## <a name="see-also"></a>Confira também

- [Referência do arquivo de configuração de diretivas do runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md)
- [Elementos da diretiva de runtime](runtime-directive-elements.md)
