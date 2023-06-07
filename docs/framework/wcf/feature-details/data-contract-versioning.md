---
title: Controle de versão de contrato de dados
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- versioning [WCF], data contracts
- versioning [WCF]
- data contracts [WCF], versioning
ms.assetid: 4a0700cb-5f5f-4137-8705-3a3ecf06461f
ms.openlocfilehash: 493efab41e2c6763eb95df8662e6254d9e0df2f2
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593497"
---
# <a name="data-contract-versioning"></a>Controle de versão de contrato de dados
À medida que os aplicativos evoluem, você também pode precisar alterar os contratos de dados usados pelos serviços. Este tópico explica como fazer a versão de contratos de dados. Este tópico descreve os mecanismos de controle de versão de contrato de dados. Para obter uma visão geral completa e diretrizes prescritivas de controle de versão, consulte [práticas recomendadas: controle de versão de contrato de dados](../best-practices-data-contract-versioning.md).  
  
## <a name="breaking-vs-nonbreaking-changes"></a>Quebrando alterações não interrompendo x  
 As alterações em um contrato de dados podem ser quebradas ou não separáveis. Quando um contrato de dados é alterado de forma não separável, um aplicativo que usa a versão mais antiga do contrato pode se comunicar com um aplicativo usando a versão mais recente, e um aplicativo que usa a versão mais recente do contrato pode se comunicar com um aplicativo usando a versão mais antiga. Por outro lado, uma alteração significativa impede a comunicação em uma ou em ambas as direções.  
  
 As alterações em um tipo que não afetam a forma como são transmitidas e recebidas são não separáveis. Essas alterações não alteram o contrato de dados, apenas o tipo subjacente. Por exemplo, você pode alterar o nome de um campo de modo não separável se, em seguida, definir a <xref:System.Runtime.Serialization.DataMemberAttribute.Name%2A> propriedade de <xref:System.Runtime.Serialization.DataMemberAttribute> como o nome da versão mais antiga. O código a seguir mostra a versão 1 de um contrato de dados.  
  
 [!code-csharp[C_DataContractVersioning#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractversioning/cs/source.cs#1)]
 [!code-vb[C_DataContractVersioning#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractversioning/vb/source.vb#1)]  
  
 O código a seguir mostra uma alteração sem interrupção.  
  
 [!code-csharp[C_DataContractVersioning#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractversioning/cs/source.cs#2)]
 [!code-vb[C_DataContractVersioning#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractversioning/vb/source.vb#2)]  
  
 Algumas alterações modificam os dados transmitidos, mas podem ou não ser interrompidas. As seguintes alterações estão sempre sendo interrompidas:  
  
- Alterando <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A> o <xref:System.Runtime.Serialization.DataContractAttribute.Namespace%2A> valor ou de um contrato de dados.  
  
- Alterar a ordem dos membros de dados usando a <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A> Propriedade do <xref:System.Runtime.Serialization.DataMemberAttribute> .  
  
- Renomeando um membro de dados.  
  
- Alterando o contrato de dados de um membro de dados. Por exemplo, alterar o tipo de membro de dados de um inteiro para uma cadeia de caracteres ou de um tipo com um contrato de dados denominado "Customer" para um tipo com um contrato de dados chamado "Person".  
  
 As alterações a seguir também são possíveis.  
  
## <a name="adding-and-removing-data-members"></a>Adicionando e removendo membros de dados  
 Na maioria dos casos, adicionar ou remover um membro de dados não é uma alteração significativa, a menos que você exija uma validade de esquema estrita (novas instâncias Validando em relação ao esquema antigo).  
  
 Quando um tipo com um campo extra é desserializado em um tipo com um campo ausente, as informações extras são ignoradas. (Ele também pode ser armazenado para fins de ida e volta; para obter mais informações, consulte [contratos de dados compatíveis com o encaminhamento](forward-compatible-data-contracts.md)).  
  
 Quando um tipo com um campo ausente é desserializado em um tipo com um campo extra, o campo extra é deixado com seu valor padrão, geralmente zero ou `null` . (O valor padrão pode ser alterado; para obter mais informações, consulte [retornos de chamada de serialização tolerantes à versão](version-tolerant-serialization-callbacks.md).)  
  
 Por exemplo, você pode usar a `CarV1` classe em um cliente e a `CarV2` classe em um serviço ou pode usar a `CarV1` classe em um serviço e a `CarV2` classe em um cliente.  
  
 [!code-csharp[C_DataContractVersioning#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractversioning/cs/source.cs#3)]
 [!code-vb[C_DataContractVersioning#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractversioning/vb/source.vb#3)]  
  
 O ponto de extremidade da versão 2 pode enviar dados com êxito para o ponto de extremidade da versão 1. A serialização da versão 2 do `Car` contrato de dados produz XML semelhante ao seguinte.  
  
```xml  
<Car>  
    <Model>Porsche</Model>  
    <HorsePower>300</HorsePower>  
</Car>  
```  
  
 O mecanismo de desserialização no v1 não localiza um membro de dados correspondente para o `HorsePower` campo e descarta esses dados.  
  
 Além disso, o ponto de extremidade da versão 1 pode enviar dados para o ponto de extremidade da versão 2. A serialização da versão 1 do `Car` contrato de dados produz XML semelhante ao seguinte.  
  
```xml  
<Car>  
    <Model>Porsche</Model>  
</Car>  
```  
  
 O desserializador da versão 2 não sabe de que forma definir o `HorsePower` campo, porque não há dados correspondentes no XML de entrada. Em vez disso, o campo é definido como o valor padrão de 0.  
  
## <a name="required-data-members"></a>Membros de dados necessários  
 Um membro de dados pode ser marcado como sendo necessário definindo a <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> propriedade de <xref:System.Runtime.Serialization.DataMemberAttribute> como `true` . Se os dados necessários estiverem ausentes durante a desserialização, uma exceção será lançada em vez de definir o membro de dados para seu valor padrão.  
  
 Adicionar um membro de dados obrigatório é uma alteração significativa. Ou seja, o tipo mais novo ainda pode ser enviado para pontos de extremidade com o tipo mais antigo, mas não o contrário. Remover um membro de dados que foi marcado como necessário em qualquer versão anterior também é uma alteração significativa.  
  
 A alteração do <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> valor da propriedade de `true` para `false` não está sendo interrompida, mas alterá-la de `false` para `true` pode ser interrompida se alguma das versões anteriores do tipo não tiver o membro de dados em questão.  
  
> [!NOTE]
> Embora a <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> propriedade seja definida como `true` , os dados de entrada podem ser nulos ou zero, e um tipo deve estar preparado para lidar com essa possibilidade. Não use <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> como um mecanismo de segurança para proteger contra dados de entrada inválidos.  
  
## <a name="omitted-default-values"></a>Valores padrão omitidos  
 É possível (embora não seja recomendado) definir a `EmitDefaultValue` propriedade no atributo DataMemberAttribute `false` como descrito em [valores padrão de membro de dados](data-member-default-values.md). Se essa configuração for `false` , o membro de dados não será emitido se for definido como seu valor padrão (geralmente nulo ou zero). Isso não é compatível com os membros de dados necessários em diferentes versões de duas maneiras:  
  
- Um contrato de dados com um membro de dados que é necessário em uma versão não pode receber dados padrão (nulos ou zero) de uma versão diferente na qual o membro de dados foi `EmitDefaultValue` definido como `false` .  
  
- Um membro de dados necessário que foi `EmitDefaultValue` definido como `false` não pode ser usado para serializar seu valor padrão (nulo ou zero), mas pode receber esse valor na desserialização. Isso cria um problema de ida e volta (os dados podem ser lidos, mas os mesmos dados não podem ser gravados). Portanto, se `IsRequired` for `true` e `EmitDefaultValue` estiver `false` em uma versão, a mesma combinação deverá ser aplicada a todas as outras versões, de modo que nenhuma versão do contrato de dados seja capaz de produzir um valor que não resulte em uma viagem de ida e volta.  
  
## <a name="schema-considerations"></a>Considerações sobre o esquema  
 Para obter uma explicação de qual esquema é produzido para tipos de contrato de dados, consulte [referência de esquema de contrato de dados](data-contract-schema-reference.md).  
  
 O esquema que o WCF produz para tipos de contrato de dados não faz nenhuma cláusula para controle de versão. Ou seja, o esquema exportado de uma determinada versão de um tipo contém somente os membros de dados presentes nessa versão. A implementação da <xref:System.Runtime.Serialization.IExtensibleDataObject> interface não altera o esquema para um tipo.  
  
 Os membros de dados são exportados para o esquema como elementos opcionais por padrão. Ou seja, o `minOccurs` valor (atributo XML) é definido como 0. Os membros de dados necessários são exportados com `minOccurs` definido como 1.  
  
 Muitas das alterações consideradas como sem interrupção estão na verdade se quebrando se a adesão estrita ao esquema é necessária. No exemplo anterior, uma `CarV1` instância com apenas o `Model` elemento seria validada em relação ao `CarV2` esquema (que tem `Model` e `Horsepower` , mas ambos são opcionais). No entanto, o inverso não é verdadeiro: uma `CarV2` instância falhará na validação em relação ao `CarV1` esquema.  
  
 A viagem de ida e volta também envolve algumas considerações adicionais. Para obter mais informações, consulte a seção "considerações sobre esquema" em [contratos de dados compatíveis com o encaminhamento](forward-compatible-data-contracts.md).  
  
### <a name="other-permitted-changes"></a>Outras alterações permitidas  
 A implementação da <xref:System.Runtime.Serialization.IExtensibleDataObject> interface é uma alteração sem interrupção. No entanto, o suporte de ida e volta não existe para versões do tipo antes da versão em que <xref:System.Runtime.Serialization.IExtensibleDataObject> foi implementada. Para obter mais informações, consulte [Contratos de dados compatíveis por encaminhamento](forward-compatible-data-contracts.md).  
  
## <a name="enumerations"></a>Enumerações  
 Adicionar ou remover um membro de enumeração é uma alteração significativa. A alteração do nome de um membro de enumeração está sendo interrompida, a menos que o nome do contrato seja mantido o mesmo da versão antiga usando o `EnumMemberAttribute` atributo. Para obter mais informações, consulte [tipos de enumeração em contratos de dados](enumeration-types-in-data-contracts.md).  
  
## <a name="collections"></a>Coleções  
 A maioria das alterações de coleção são não separáveis porque a maioria dos tipos de coleção são intercambiáveis entre si no modelo de contrato de dados. No entanto, tornar uma coleção não personalizada personalizada ou vice-versa é uma alteração significativa. Além disso, alterar as configurações de personalização da coleção é uma alteração significativa; ou seja, alterando o nome do contrato de dados e o namespace, o nome do elemento de repetição, o nome do elemento de chave e o nome do elemento de valor. Para obter mais informações sobre a personalização da coleção, consulte [tipos de coleção em contratos de dados](collection-types-in-data-contracts.md).  
Naturalmente, alterar o contrato de dados de conteúdo de uma coleção (por exemplo, alterar de uma lista de inteiros para uma lista de cadeias de caracteres) é uma alteração significativa.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.Serialization.DataMemberAttribute.Name%2A>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A>
- <xref:System.Runtime.Serialization.DataContractAttribute.Namespace%2A>
- <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A>
- <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A>
- <xref:System.Runtime.Serialization.SerializationException>
- <xref:System.Runtime.Serialization.IExtensibleDataObject>
- [Retornos de chamada de serialização tolerantes à versão](version-tolerant-serialization-callbacks.md)
- [Práticas recomendadas: controle de versão de contrato de dados](../best-practices-data-contract-versioning.md)
- [Usando contratos de dados](using-data-contracts.md)
- [Equivalência de contrato de dados](data-contract-equivalence.md)
- [Contratos de dados compatíveis por encaminhamento](forward-compatible-data-contracts.md)
