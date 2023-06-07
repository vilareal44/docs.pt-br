---
title: Tipos de contratos de dados conhecidos
description: Saiba como o modelo de contrato de dados usa a classe KnownTypeattribute para especificar os tipos a serem incluídos durante a desserialização no WCF.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data contracts [WCF], known types
- KnownTypeAttribute [WCF]
- KnownTypes [WCF]
ms.assetid: 1a0baea1-27b7-470d-9136-5bbad86c4337
ms.openlocfilehash: 52b0caaaac976893dcf5ef5c228ccc4f53bdbe9e
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247475"
---
# <a name="data-contract-known-types"></a>Tipos de contratos de dados conhecidos
A <xref:System.Runtime.Serialization.KnownTypeAttribute> classe permite que você especifique, com antecedência, os tipos que devem ser incluídos para consideração durante a desserialização. Para obter um exemplo de trabalho, consulte o exemplo [tipos conhecidos](../samples/known-types.md) .  
  
 Normalmente, ao passar parâmetros e retornar valores entre um cliente e um serviço, ambos os pontos de extremidade compartilham todos os contratos de dados dos dados a serem transmitidos. No entanto, esse não é o caso nas seguintes circunstâncias:  
  
- O contrato de dados enviado é derivado do contrato de dados esperado. Para obter mais informações, consulte a seção sobre herança em [equivalência de contrato de dados](data-contract-equivalence.md)). Nesse caso, os dados transmitidos não têm o mesmo contrato de dados que o esperado pelo ponto de extremidade de recebimento.  
  
- O tipo declarado para as informações a serem transmitidas é uma interface, em oposição a uma classe, estrutura ou enumeração. Portanto, não pode ser conhecido com antecedência qual tipo que implementa a interface é realmente enviado e, portanto, o ponto de extremidade de recebimento não pode determinar antecipadamente o contrato de dados para os dados transmitidos.  
  
- O tipo declarado para as informações a serem transmitidas é <xref:System.Object> . Como cada tipo é herdado de <xref:System.Object> , e não pode ser conhecido com antecedência qual tipo é realmente enviado, o ponto de extremidade de recebimento não pode determinar antecipadamente o contrato de dados para os dados transmitidos. Este é um caso especial do primeiro item: cada contrato de dados deriva do padrão, um contrato de dados em branco que é gerado para o <xref:System.Object> .  
  
- Alguns tipos, que incluem tipos de .NET Framework, têm Membros que estão em uma das três categorias anteriores. Por exemplo, <xref:System.Collections.Hashtable> usa <xref:System.Object> para armazenar os objetos reais na tabela de hash. Ao serializar esses tipos, o lado de recebimento não pode determinar antecipadamente o contrato de dados para esses membros.  
  
## <a name="the-knowntypeattribute-class"></a>A classe KnownTypeattribute  
 Quando os dados chegam em um ponto de extremidade de recebimento, o tempo de execução do WCF tenta desserializar os dados em uma instância de um tipo de Common Language Runtime (CLR). O tipo instanciado para desserialização é escolhido primeiro inspecionando a mensagem de entrada para determinar o contrato de dados ao qual o conteúdo da mensagem está em conformidade. Em seguida, o mecanismo de desserialização tenta encontrar um tipo CLR que implementa um contrato de dados compatível com o conteúdo da mensagem. O conjunto de tipos candidatos que o mecanismo de desserialização permite durante esse processo é conhecido como o conjunto de "tipos conhecidos" do desserializador.  
  
 Uma maneira de permitir que o mecanismo de desserialização saiba sobre um tipo é usando o <xref:System.Runtime.Serialization.KnownTypeAttribute> . O atributo não pode ser aplicado a membros de dados individuais, somente a tipos de contrato de dados inteiros. O atributo é aplicado a um *tipo externo* que pode ser uma classe ou uma estrutura. Em seu uso mais básico, a aplicação do atributo especifica um tipo como "tipo conhecido". Isso faz com que o tipo conhecido seja uma parte do conjunto de tipos conhecidos sempre que um objeto do tipo externo ou qualquer objeto referido por seus membros está sendo desserializado. Mais de um <xref:System.Runtime.Serialization.KnownTypeAttribute> atributo pode ser aplicado ao mesmo tipo.  
  
## <a name="known-types-and-primitives"></a>Tipos conhecidos e primitivos  
 Tipos primitivos, bem como determinados tipos tratados como primitivos (por exemplo, <xref:System.DateTime> e <xref:System.Xml.XmlElement> ) são sempre "conhecidos" e nunca precisam ser adicionados por meio desse mecanismo. No entanto, as matrizes de tipos primitivos precisam ser adicionadas explicitamente. A maioria das coleções é considerada equivalente a matrizes. (As coleções não genéricas são consideradas equivalentes a matrizes de <xref:System.Object> ). Para obter um exemplo de como usar primitivas, matrizes primitivas e coleções primitivas, consulte o exemplo 4.  
  
> [!NOTE]
> Ao contrário de outros tipos primitivos, a <xref:System.DateTimeOffset> estrutura não é um tipo conhecido por padrão, portanto, ela deve ser adicionada manualmente à lista de tipos conhecidos.  
  
## <a name="examples"></a>Exemplos  
 Os exemplos a seguir mostram a <xref:System.Runtime.Serialization.KnownTypeAttribute> classe em uso.  
  
#### <a name="example-1"></a>Exemplo 1  
 Há três classes com uma relação de herança.  
  
 [!code-csharp[C_KnownTypeAttribute#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#1)]
 [!code-vb[C_KnownTypeAttribute#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#1)]  
  
 A classe a seguir `CompanyLogo` pode ser serializada, mas não poderá ser desserializada se o `ShapeOfLogo` membro for definido como um `CircleType` ou um `TriangleType` objeto, porque o mecanismo de desserialização não reconhece nenhum tipo com os nomes de contrato de dados "Circle" ou "triângulo".  
  
 [!code-csharp[C_KnownTypeAttribute#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#2)]
 [!code-vb[C_KnownTypeAttribute#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#2)]  
  
 A maneira correta de gravar o `CompanyLogo` tipo é mostrada no código a seguir.  
  
 [!code-csharp[C_KnownTypeAttribute#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#3)]
 [!code-vb[C_KnownTypeAttribute#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#3)]  
  
 Sempre que o tipo externo `CompanyLogo2` está sendo desserializado, o mecanismo de desserialização sabe `CircleType` e `TriangleType` e, portanto, é capaz de encontrar tipos correspondentes para os contratos de dados "Circle" e "triângulo".  
  
#### <a name="example-2"></a>Exemplo 2  
 No exemplo a seguir, embora ambos `CustomerTypeA` e `CustomerTypeB` tenham o `Customer` contrato de dados, uma instância do `CustomerTypeB` é criada sempre que um `PurchaseOrder` é desserializado, porque apenas `CustomerTypeB` é conhecido pelo mecanismo de desserialização.  
  
 [!code-csharp[C_KnownTypeAttribute#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#4)]
 [!code-vb[C_KnownTypeAttribute#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#4)]  
  
#### <a name="example-3"></a>Exemplo 3  
 No exemplo a seguir, um <xref:System.Collections.Hashtable> armazena seu conteúdo internamente como <xref:System.Object> . Para desserializar uma tabela de hash com êxito, o mecanismo de desserialização deve saber o conjunto de possíveis tipos que podem ocorrer lá. Nesse caso, sabemos com antecedência que somente os `Book` objetos e `Magazine` são armazenados no `Catalog` , portanto, eles são adicionados usando o <xref:System.Runtime.Serialization.KnownTypeAttribute> atributo.  
  
 [!code-csharp[C_KnownTypeAttribute#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#5)]
 [!code-vb[C_KnownTypeAttribute#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#5)]  
  
#### <a name="example-4"></a>Exemplo 4  
 No exemplo a seguir, um contrato de dados armazena um número e uma operação a ser executada no número. O `Numbers` membro de dados pode ser um inteiro, uma matriz de inteiros ou um <xref:System.Collections.Generic.List%601> que contenha inteiros.  
  
> [!CAUTION]
> Isso só funcionará no lado do cliente se SVCUTIL.EXE for usado para gerar um proxy WCF. SVCUTIL.EXE recupera metadados do serviço, incluindo qualquer tipo conhecido. Sem essas informações, um cliente não poderá desserializar os tipos.  
  
 [!code-csharp[C_KnownTypeAttribute#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#6)]
 [!code-vb[C_KnownTypeAttribute#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#6)]  
  
 Este é o código do aplicativo.  
  
 [!code-csharp[C_KnownTypeAttribute#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#7)]
 [!code-vb[C_KnownTypeAttribute#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#7)]  
  
## <a name="known-types-inheritance-and-interfaces"></a>Tipos, herança e interfaces conhecidos  
 Quando um tipo conhecido é associado a um tipo específico usando o `KnownTypeAttribute` atributo, o tipo conhecido também é associado a todos os tipos derivados desse tipo. Por exemplo, consulte o código a seguir.  
  
 [!code-csharp[C_KnownTypeAttribute#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#8)]
 [!code-vb[C_KnownTypeAttribute#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#8)]  
  
 A `DoubleDrawing` classe não requer o `KnownTypeAttribute` atributo a ser usado `Square` e `Circle` , no `AdditionalShape` campo, porque a classe base ( `Drawing` ) já tem esses atributos aplicados.  
  
 Tipos conhecidos podem ser associados somente a classes e estruturas, não a interfaces.  
  
## <a name="known-types-using-open-generic-methods"></a>Tipos conhecidos usando métodos genéricos abertos  
 Pode ser necessário adicionar um tipo genérico como um tipo conhecido. No entanto, um tipo genérico aberto não pode ser passado como um parâmetro para o `KnownTypeAttribute` atributo.  
  
 Esse problema pode ser resolvido usando um mecanismo alternativo: escreva um método que retorne uma lista de tipos a serem adicionados à coleção de tipos conhecidos. O nome do método é especificado como um argumento de cadeia de caracteres para o `KnownTypeAttribute` atributo devido a algumas restrições.  
  
 O método deve existir no tipo ao qual o `KnownTypeAttribute` atributo é aplicado, deve ser estático, não deve aceitar nenhum parâmetro e deve retornar um objeto que possa ser atribuído a <xref:System.Collections.IEnumerable> de <xref:System.Type> .  
  
 Você não pode combinar o `KnownTypeAttribute` atributo com um nome de método e `KnownTypeAttribute` atributos com tipos reais no mesmo tipo. Além disso, você não pode aplicar mais de um `KnownTypeAttribute` com um nome de método ao mesmo tipo.  
  
 Consulte a classe a seguir.  
  
 [!code-csharp[C_KnownTypeAttribute#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#9)]
 [!code-vb[C_KnownTypeAttribute#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#9)]  
  
 O `theDrawing` campo contém instâncias de uma classe genérica `ColorDrawing` e uma classe genérica `BlackAndWhiteDrawing` , ambas herdadas de uma classe genérica `Drawing` . Normalmente, ambos devem ser adicionados a tipos conhecidos, mas a sintaxe a seguir não é válida para atributos.  
  
```csharp  
// Invalid syntax for attributes:  
// [KnownType(typeof(ColorDrawing<T>))]  
// [KnownType(typeof(BlackAndWhiteDrawing<T>))]  
```  
  
```vb  
' Invalid syntax for attributes:  
' <KnownType(GetType(ColorDrawing(Of T))), _  
' KnownType(GetType(BlackAndWhiteDrawing(Of T)))>  
```  
  
 Portanto, um método deve ser criado para retornar esses tipos. A maneira correta de escrever esse tipo, em seguida, é mostrada no código a seguir.  
  
 [!code-csharp[C_KnownTypeAttribute#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#10)]
 [!code-vb[C_KnownTypeAttribute#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#10)]  
  
## <a name="additional-ways-to-add-known-types"></a>Outras maneiras de adicionar tipos conhecidos  
 Além disso, tipos conhecidos podem ser adicionados por meio de um arquivo de configuração. Isso é útil quando você não controla o tipo que requer tipos conhecidos para desserialização adequada, como ao usar bibliotecas de tipo de terceiros com o Windows Communication Foundation (WCF).  
  
 O arquivo de configuração a seguir mostra como especificar um tipo conhecido em um arquivo de configuração.  
  
 `<configuration>`  
  
 `<system.runtime.serialization>`  
  
 `<dataContractSerializer>`  
  
 `<declaredTypes>`  
  
 `<add type="MyCompany.Library.Shape,`  
  
 `MyAssembly, Version=2.0.0.0, Culture=neutral,`  
  
 `PublicKeyToken=XXXXXX, processorArchitecture=MSIL">`  
  
 `<knownType type="MyCompany.Library.Circle,`  
  
 `MyAssembly, Version=2.0.0.0, Culture=neutral,`  
  
 `PublicKeyToken=XXXXXX, processorArchitecture=MSIL"/>`  
  
 `</add>`  
  
 `</declaredTypes>`  
  
 `</dataContractSerializer>`  
  
 `</system.runtime.serialization>`  
  
 `</configuration>`  
  
 No arquivo de configuração anterior, um tipo de contrato de dados chamado `MyCompany.Library.Shape` é declarado `MyCompany.Library.Circle` como tendo um tipo conhecido.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Runtime.Serialization.KnownTypeAttribute>
- <xref:System.Collections.Hashtable>
- <xref:System.Object>
- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.DataContractSerializer.KnownTypes%2A>
- [Tipos conhecidos](../samples/known-types.md)
- [Equivalência de contrato de dados](data-contract-equivalence.md)
- [Criando contratos de serviço](../designing-service-contracts.md)
