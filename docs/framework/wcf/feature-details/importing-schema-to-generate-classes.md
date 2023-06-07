---
title: Importando esquema para gerar classes
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, schema import and export
- XsdDataContractImporter class
ms.assetid: b9170583-8c34-43bd-97bb-6c0c8dddeee0
ms.openlocfilehash: 01f5162727a213fa5dcdf8a70e4e8e4c3627f086
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596897"
---
# <a name="importing-schema-to-generate-classes"></a>Importando esquema para gerar classes
Para gerar classes a partir de esquemas utilizáveis com o Windows Communication Foundation (WCF), use a <xref:System.Runtime.Serialization.XsdDataContractImporter> classe. Este tópico descreve o processo e as variações.  
  
## <a name="the-import-process"></a>O processo de importação
 O processo de importação de esquema começa com um <xref:System.Xml.Schema.XmlSchemaSet> e produz um <xref:System.CodeDom.CodeCompileUnit> .  
  
 O `XmlSchemaSet` é uma parte do som (modelo de objeto de esquema) do .NET Framework que representa um conjunto de documentos de esquema XSD (linguagem de definição de esquema XML). Para criar um `XmlSchemaSet` objeto a partir de um conjunto de documentos XSD, desserializar cada documento em um <xref:System.Xml.Schema.XmlSchema> objeto (usando o <xref:System.Xml.Serialization.XmlSerializer> ) e adicionar esses objetos a um novo `XmlSchemaSet` .  
  
 O `CodeCompileUnit` faz parte do modelo de objeto do documento de código do .NET Framework (CodeDom) que representa o código de .NET Framework de forma abstrata. Para gerar o código real de a `CodeCompileUnit` , use uma subclasse da <xref:System.CodeDom.Compiler.CodeDomProvider> classe, como a <xref:Microsoft.CSharp.CSharpCodeProvider> <xref:Microsoft.VisualBasic.VBCodeProvider> classe ou.  
  
### <a name="to-import-a-schema"></a>Para importar um esquema  
  
1. Crie uma instância de <xref:System.Runtime.Serialization.XsdDataContractImporter>.  
  
2. Opcional. Passe um `CodeCompileUnit` no construtor. Os tipos gerados durante a importação de esquema são adicionados a essa `CodeCompileUnit` instância em vez de começar com um espaço em branco `CodeCompileUnit` .  
  
3. Opcional. Chame um dos <xref:System.Runtime.Serialization.XsdDataContractImporter.CanImport%2A> métodos. O método determina se o esquema fornecido é um esquema de contrato de dados válido e pode ser importado. O `CanImport` método tem as mesmas sobrecargas que `Import` (a próxima etapa).  
  
4. Chame um dos métodos sobrecarregados `Import` , por exemplo, o <xref:System.Runtime.Serialization.XsdDataContractImporter.Import%28System.Xml.Schema.XmlSchemaSet%29> método.  
  
     A sobrecarga mais simples pega um `XmlSchemaSet` e importa todos os tipos, incluindo tipos anônimos, encontrados nesse conjunto de esquema. Outras sobrecargas permitem que você especifique o tipo XSD ou uma lista de tipos a serem importados (na forma de um <xref:System.Xml.XmlQualifiedName> ou de uma coleção de `XmlQualifiedName` objetos). Nesse caso, somente os tipos especificados são importados. Uma sobrecarga usa um <xref:System.Xml.Schema.XmlSchemaElement> que importa um elemento específico do `XmlSchemaSet` , bem como seu tipo associado (seja anônimo ou não). Essa sobrecarga retorna um `XmlQualifiedName` , que representa o nome do contrato de dados do tipo gerado para esse elemento.  
  
     Várias chamadas do `Import` método resultam em vários itens sendo adicionados ao mesmo `CodeCompileUnit` . Um tipo não será gerado no `CodeCompileUnit` se ele já existir. Chame `Import` várias vezes no mesmo `XsdDataContractImporter` , em vez de usar vários `XsdDataContractImporter` objetos. Essa é a maneira recomendada para evitar que tipos duplicados sejam gerados.  
  
    > [!NOTE]
    > Se houver uma falha durante a importação, o `CodeCompileUnit` estará em um estado imprevisível. O uso `CodeCompileUnit` de um resultado de uma importação com falha pode expô-lo a vulnerabilidades de segurança.  
  
5. Acesse o `CodeCompileUnit` por meio da <xref:System.Runtime.Serialization.XsdDataContractImporter.CodeCompileUnit%2A> propriedade.  
  
### <a name="import-options-customizing-the-generated-types"></a>Opções de importação: Personalizando os tipos gerados  
 Você pode definir a <xref:System.Runtime.Serialization.XsdDataContractImporter.Options%2A> propriedade de <xref:System.Runtime.Serialization.XsdDataContractImporter> como uma instância da <xref:System.Runtime.Serialization.ImportOptions> classe para controlar vários aspectos do processo de importação. Várias opções influenciam diretamente os tipos gerados.  
  
#### <a name="controlling-the-access-level-generateinternal-or-the-internal-switch"></a>Controlando o nível de acesso (GenerateInternal ou a opção/Internal)  
 Isso corresponde à opção **/Internal** na ferramenta de [Utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md).  
  
 Normalmente, os tipos públicos são gerados do esquema, com campos privados e propriedades de membro de dados públicos correspondentes. Para gerar tipos internos, defina a <xref:System.Runtime.Serialization.ImportOptions.GenerateInternal%2A> propriedade como `true` .  
  
 O exemplo a seguir mostra um esquema transformado em uma classe interna quando a <xref:System.Runtime.Serialization.ImportOptions.GenerateInternal%2A> propriedade é definida como`true.`  
  
 [!code-csharp[c_SchemaImportExport#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#2)]
 [!code-vb[c_SchemaImportExport#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#2)]  
  
#### <a name="controlling-namespaces-namespaces-or-the-namespace-switch"></a>Controlando namespaces (namespaces ou a opção/namespace)  
 Isso corresponde à opção **/namespace** na `Svcutil.exe` ferramenta.  
  
 Normalmente, os tipos gerados do esquema são gerados em namespaces .NET Framework, com cada namespace XSD correspondente a um namespace de .NET Framework específico, de acordo com um mapeamento descrito na [referência do esquema de contrato de dados](data-contract-schema-reference.md). Você pode personalizar esse mapeamento pela <xref:System.Runtime.Serialization.ImportOptions.Namespaces%2A> propriedade para um <xref:System.Collections.Generic.Dictionary%602> . Se um namespace XSD fornecido for encontrado no dicionário, o namespace de .NET Framework correspondente também será obtido do seu dicionário.  
  
 Por exemplo, considere o esquema a seguir.  
  
 [!code-xml[c_SchemaImportExport#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/common/source.config#10)]  
  
 O exemplo a seguir usa a `Namespaces` propriedade para mapear o `http://schemas.contoso.com/carSchema` namespace para "contoso. carros".  
  
 [!code-csharp[c_SchemaImportExport#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#8)]
 [!code-vb[c_SchemaImportExport#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#8)]  
  
#### <a name="adding-the-serializableattribute-generateserializable-or-the-serializable-switch"></a>Adicionando o SerializableAttribute (GenerateSerializable ou o comutador/Serializable)  
 Isso corresponde à opção **/Serializable** na `Svcutil.exe` ferramenta.  
  
 Às vezes, é importante que os tipos gerados a partir do esquema sejam utilizáveis com .NET Framework mecanismos de serialização de tempo de execução (por exemplo, as <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=nameWithType> <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> classes e). Isso é útil ao usar tipos para .NET Framework comunicação remota. Para habilitar isso, você deve aplicar o <xref:System.SerializableAttribute> atributo aos tipos gerados, além do <xref:System.Runtime.Serialization.DataContractAttribute> atributo regular. O atributo será gerado automaticamente se a `GenerateSerializable` opção de importação for definida como `true` .  
  
 O exemplo a seguir mostra a `Vehicle` classe gerada com a `GenerateSerializable` opção de importação definida como `true` .  
  
 [!code-csharp[c_SchemaImportExport#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#4)]
 [!code-vb[c_SchemaImportExport#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#4)]  
  
#### <a name="adding-data-binding-support-enabledatabinding-or-the-enabledatabinding-switch"></a>Adicionando suporte à vinculação de dados (EnableDataBinding ou a opção/enableDataBinding)  
 Isso corresponde à opção **/EnableDataBinding** na ferramenta svcutil. exe.  
  
 Às vezes, talvez você queira associar os tipos gerados do esquema aos componentes gráficos da interface do usuário para que qualquer atualização das instâncias desses tipos atualize automaticamente a interface do usuário. O `XsdDataContractImporter` pode gerar tipos que implementam a <xref:System.ComponentModel.INotifyPropertyChanged> interface de forma que qualquer alteração de propriedade dispare um evento. Se você estiver gerando tipos para uso com um ambiente de programação de interface do usuário cliente que dá suporte a essa interface (como Windows Presentation Foundation (WPF)), defina a <xref:System.Runtime.Serialization.ImportOptions.EnableDataBinding%2A> propriedade como `true` para habilitar esse recurso.  
  
 O exemplo a seguir mostra a `Vehicle` classe gerada com o <xref:System.Runtime.Serialization.ImportOptions.EnableDataBinding%2A> definido como `true` .  
  
 [!code-csharp[C_SchemaImportExport#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#5)]
 [!code-vb[C_SchemaImportExport#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#5)]  
  
### <a name="import-options-choosing-collection-types"></a>Opções de importação: escolhendo tipos de coleção  
 Dois padrões especiais em XML representam coleções de itens: listas de itens e associações entre um item e outro. Veja a seguir um exemplo de uma lista de cadeias de caracteres.  
  
 [!code-xml[C_SchemaImportExport#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/common/source.config#11)]  
  
 Veja a seguir um exemplo de uma associação entre uma cadeia de caracteres e um inteiro ( `city name` e `population` ).  
  
 [!code-xml[C_SchemaImportExport#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/common/source.config#12)]  
  
> [!NOTE]
> Qualquer associação também pode ser considerada uma lista. Por exemplo, você pode exibir a Associação anterior como uma lista de `city` objetos complexos que acontecem com dois campos (um campo de cadeia de caracteres e um campo de número inteiro). Ambos os padrões têm uma representação no esquema XSD. Não há como diferenciar entre uma lista e uma associação, de modo que esses padrões são sempre tratados como listas, a menos que uma anotação especial específica ao WCF esteja presente no esquema. A anotação indica que um determinado padrão representa uma associação. Para obter mais informações, consulte [referência de esquema de contrato de dados](data-contract-schema-reference.md).  
  
 Normalmente, uma lista é importada como um contrato de dados de coleção que deriva de uma lista genérica ou como uma matriz de .NET Framework, dependendo se o esquema segue ou não o padrão de nomenclatura padrão para coleções. Isso é descrito mais detalhadamente em [tipos de coleção em contratos de dados](collection-types-in-data-contracts.md). As associações normalmente são importadas como um <xref:System.Collections.Generic.Dictionary%602> contrato de dados de coleção ou a, que deriva do objeto Dictionary. Por exemplo, considere o esquema a seguir.  
  
 [!code-xml[c_SchemaImportExport#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/common/source.config#13)]  
  
 Isso seria importado da seguinte maneira (os campos são mostrados em vez de propriedades para facilitar a leitura).  
  
 [!code-csharp[c_SchemaImportExport#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#6)]
 [!code-vb[c_SchemaImportExport#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#6)]  
  
 É possível personalizar os tipos de coleção gerados para esses padrões de esquema. Por exemplo, talvez você queira gerar coleções derivadas do <xref:System.ComponentModel.BindingList%601> em vez da <xref:System.Collections.Generic.List%601> classe para associar o tipo a uma caixa de listagem e fazer com que ela seja atualizada automaticamente quando o conteúdo da coleção for alterado. Para fazer isso, defina a <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A> propriedade da <xref:System.Runtime.Serialization.ImportOptions> classe como uma lista de tipos de coleção a ser usada (daqui em diante, conhecida como os tipos referenciados). Ao importar qualquer coleção, essa lista de tipos de coleção referenciados é verificada e a melhor coleção de correspondência é usada se um for encontrado. As associações são correspondidas apenas em tipos que implementam a interface genérica ou não genérica <xref:System.Collections.IDictionary> , enquanto as listas são correspondidas em qualquer tipo de coleção com suporte.  
  
 Por exemplo, se a <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A> propriedade for definida como a <xref:System.ComponentModel.BindingList%601> , o `people` tipo no exemplo anterior será gerado da seguinte maneira.  
  
 [!code-csharp[C_SchemaImportExport#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#7)]
 [!code-vb[C_SchemaImportExport#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#7)]  
  
 Um genérico fechado é considerado a melhor correspondência. Por exemplo, se os tipos `BindingList(Of Integer)` e <xref:System.Collections.ArrayList> forem passados para a coleção de tipos referenciados, todas as listas de inteiros encontrados no esquema serão importadas como um `BindingList(Of Integer)` . Qualquer outra lista, por exemplo, a `List(Of String)` , é importada como um `ArrayList` .  
  
 Se um tipo que implementa a `IDictionary` interface genérica for adicionado à coleção de tipos referenciados, seus parâmetros de tipo deverão estar totalmente abertos ou totalmente fechados.  
  
 Não são permitidas duplicatas. Por exemplo, você não pode adicionar um `List(Of Integer)` e um `Collection(Of Integer)` aos tipos referenciados. Isso tornaria impossível determinar qual deve ser usado quando uma lista de inteiros for encontrada no esquema. As duplicatas serão detectadas somente se houver um tipo no esquema que expõe o problema de duplicatas. Por exemplo, se o esquema importado não contiver listas de inteiros, ele terá permissão para ter tanto o `List(Of Integer)` quanto o `Collection(Of Integer)` na coleção de tipos referenciados, mas nenhum deles terá nenhum efeito.  
  
 O mecanismo de tipos de coleção referenciado funciona igualmente bem para coleções de tipos complexos (incluindo coleções de outras coleções) e não apenas para coleções de primitivos.  
  
 A `ReferencedCollectionTypes` propriedade corresponde à opção **/collectionType** na ferramenta svcutil. exe. Observe que para fazer referência a vários tipos de coleção, a opção **/collectionType** deve ser especificada várias vezes. Se o tipo não estiver no MsCorLib. dll, seu assembly também deverá ser referenciado usando a opção **/Reference** .  
  
#### <a name="import-options-referencing-existing-types"></a>Opções de importação: referenciando tipos existentes  
 Ocasionalmente, os tipos no esquema correspondem a tipos de .NET Framework existentes e não há necessidade de gerar esses tipos a partir do zero. (Esta seção aplica-se somente a tipos que não são de coleção. Para tipos de coleção, consulte a seção anterior.)  
  
 Por exemplo, você pode ter um tipo de contrato de dados "Person" padrão para toda a empresa que sempre queira usar ao representar uma pessoa. Sempre que algum serviço usa esse tipo, e seu esquema aparece nos metadados do serviço, talvez você queira reutilizar o tipo existente `Person` ao importar esse esquema em vez de gerar um novo para cada serviço.  
  
 Para fazer isso, passe uma lista de tipos de .NET Framework que você deseja reutilizar na coleção que a <xref:System.Runtime.Serialization.ImportOptions.ReferencedTypes%2A> propriedade retorna na <xref:System.Runtime.Serialization.ImportOptions> classe. Se qualquer um desses tipos tiver um nome de contrato de dados e um namespace que corresponda ao nome e ao namespace de um tipo de esquema, uma comparação estrutural será executada. Se for determinado que os tipos têm nomes correspondentes e estruturas correspondentes, o tipo de .NET Framework existente será reutilizado em vez de gerar um novo. Se apenas o nome corresponder, mas não a estrutura, uma exceção será lançada. Observe que não há nenhuma concessão para controle de versão ao referenciar tipos (por exemplo, adicionar novos membros de dados opcionais). As estruturas devem corresponder exatamente.  
  
 É legal adicionar vários tipos com o mesmo nome de contrato de dados e namespace à coleção de tipos referenciados, desde que nenhum tipo de esquema seja importado com esse nome e namespace. Isso permite que você adicione facilmente todos os tipos em um assembly à coleção sem se preocupar com duplicatas de tipos que não ocorrem de fato no esquema.  
  
 A `ReferencedTypes` propriedade corresponde à opção **/Reference** em determinados modos de operação da ferramenta svcutil. exe.  
  
> [!NOTE]
> Ao usar o SvcUtil. exe ou (no Visual Studio) as ferramentas de **Adicionar referência de serviço** , todos os tipos em MsCorLib. dll são automaticamente referenciados.  
  
#### <a name="import-options-importing-non-datacontract-schema-as-ixmlserializable-types"></a>Opções de importação: importando o esquema não DataContract como tipos IXmlSerializable  
 O <xref:System.Runtime.Serialization.XsdDataContractImporter> oferece suporte a um subconjunto limitado do esquema. Se constructos de esquema sem suporte estiverem presentes (por exemplo, atributos XML), a tentativa de importação falhará com uma exceção. No entanto, definir a <xref:System.Runtime.Serialization.ImportOptions.ImportXmlType%2A> propriedade para `true` estender o intervalo de esquema com suporte. Quando definido como `true` , o <xref:System.Runtime.Serialization.XsdDataContractImporter> gera os tipos que implementam a <xref:System.Xml.Serialization.IXmlSerializable> interface. Isso permite o acesso direto à representação XML desses tipos.  
  
##### <a name="design-considerations"></a>Considerações de criação  
  
- Pode ser difícil trabalhar com a representação XML de tipo fraco diretamente. Considere o uso de um mecanismo de serialização alternativo, como o <xref:System.Xml.Serialization.XmlSerializer> , para trabalhar com esquema não compatível com contratos de dados de maneira fortemente tipada. Para obter mais informações, consulte [usando a classe XmlSerializer](using-the-xmlserializer-class.md).  
  
- Algumas construções de esquema não podem ser importadas pelo <xref:System.Runtime.Serialization.XsdDataContractImporter> mesmo quando a <xref:System.Runtime.Serialization.ImportOptions.ImportXmlType%2A> propriedade é definida como `true` . Novamente, considere o uso do <xref:System.Xml.Serialization.XmlSerializer> para esses casos.  
  
- As construções de esquema exatas que têm suporte tanto quando são <xref:System.Runtime.Serialization.ImportOptions.ImportXmlType%2A> `true` ou `false` estão descritas em [referência de esquema de contrato de dados](data-contract-schema-reference.md).  
  
- O esquema para <xref:System.Xml.Serialization.IXmlSerializable> tipos gerados não retém fidelidade quando importado e exportado. Ou seja, exportar o esquema dos tipos gerados e importar como classes não retorna o esquema original.  
  
 É possível combinar a <xref:System.Runtime.Serialization.ImportOptions.ImportXmlType%2A> opção com a <xref:System.ServiceModel.Description.ServiceContractGenerator.ReferencedTypes%2A> opção descrita anteriormente. Para tipos que precisam ser gerados como <xref:System.Xml.Serialization.IXmlSerializable> implementações, a verificação estrutural é ignorada ao usar o <xref:System.ServiceModel.Description.ServiceContractGenerator.ReferencedTypes%2A> recurso.  
  
 A <xref:System.Runtime.Serialization.ImportOptions.ImportXmlType%2A> opção corresponde à opção **/importXmlTypes** na ferramenta svcutil. exe.  
  
##### <a name="working-with-generated-ixmlserializable-types"></a>Trabalhando com tipos IXmlSerializable gerados  
 Os `IXmlSerializable` tipos gerados contêm um campo privado, chamado "nodefield", que retorna uma matriz de <xref:System.Xml.XmlNode> objetos. Ao desserializar uma instância desse tipo, você pode acessar os dados XML diretamente por meio desse campo usando o Modelo de Objeto do Documento XML. Ao serializar uma instância desse tipo, você pode definir esse campo para os dados XML desejados e ele será serializado.  
  
 Isso é feito por meio da `IXmlSerializable` implementação. No tipo gerado `IXmlSerializable` , a <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> implementação chama o <xref:System.Runtime.Serialization.XmlSerializableServices.ReadNodes%2A> método da <xref:System.Runtime.Serialization.XmlSerializableServices> classe. O método é um método auxiliar que converte XML fornecido por meio de um <xref:System.Xml.XmlReader> para uma matriz de <xref:System.Xml.XmlNode> objetos. A <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> implementação faz o oposto e converte a matriz de `XmlNode` objetos em uma sequência de <xref:System.Xml.XmlWriter> chamadas. Isso é feito usando o <xref:System.Runtime.Serialization.XmlSerializableServices.WriteNodes%2A> método.  
  
 É possível executar o processo de exportação de esquema nas classes geradas `IXmlSerializable` . Como mencionado anteriormente, você não obterá o esquema original de volta. Em vez disso, você obterá o tipo XSD padrão "anyType", que é um caractere curinga para qualquer tipo XSD.  
  
 Isso é feito aplicando o <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> atributo às classes geradas `IXmlSerializable` e especificando um método que chama o <xref:System.Runtime.Serialization.XmlSerializableServices.AddDefaultSchema%2A> método para gerar o tipo "anyType".  
  
> [!NOTE]
> O <xref:System.Runtime.Serialization.XmlSerializableServices> tipo existe somente para dar suporte a esse recurso específico. Não é recomendável para uso para qualquer outra finalidade.  
  
#### <a name="import-options-advanced-options"></a>Opções de importação: opções avançadas  
 A seguir estão as opções avançadas de importação:  
  
- Propriedade <xref:System.Runtime.Serialization.ImportOptions.CodeProvider%2A>. Especifique o a <xref:System.CodeDom.Compiler.CodeDomProvider> ser usado para gerar o código para as classes geradas. O mecanismo de importação tenta evitar recursos aos quais o <xref:System.CodeDom.Compiler.CodeDomProvider> não oferece suporte. Se o <xref:System.Runtime.Serialization.ImportOptions.CodeProvider%2A> não estiver definido, o conjunto completo de recursos de .NET Framework será usado sem restrições.  
  
- Propriedade <xref:System.Runtime.Serialization.ImportOptions.DataContractSurrogate%2A>. Uma <xref:System.Runtime.Serialization.IDataContractSurrogate> implementação pode ser especificada com essa propriedade. O <xref:System.Runtime.Serialization.IDataContractSurrogate> personaliza o processo de importação. Para obter mais informações, consulte [substitutos de contrato de dados](../extending/data-contract-surrogates.md). Por padrão, nenhum substituto é usado.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.XsdDataContractImporter>
- <xref:System.Runtime.Serialization.XsdDataContractExporter>
- <xref:System.Runtime.Serialization.ImportOptions>
- [Referência de esquema de contrato de dados](data-contract-schema-reference.md)
- [Alternativas de contrato de dados](../extending/data-contract-surrogates.md)
- [Exportação e importação de esquema](schema-import-and-export.md)
- [Exportando esquemas de Classes](exporting-schemas-from-classes.md)
- [Referência de esquema de contrato de dados](data-contract-schema-reference.md)
