---
title: Carregando um DataSet a partir de XML
description: Saiba como adicionar conteúdo a um conjunto de ADO.NET de XML. O .NET Framework oferece flexibilidade para o que carregar e a estrutura do conjunto de dados.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 49c083b7-a5ed-41cf-aabc-5aaba96f00e6
ms.openlocfilehash: 8c81e6e29678fe2e30af7c15d8d6e90f23dd0762
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286877"
---
# <a name="loading-a-dataset-from-xml"></a>Carregando um DataSet a partir de XML
O conteúdo de um <xref:System.Data.DataSet> ADO.NET pode ser criado de um fluxo ou documento XML. Além disso, com o .NET Framework, você tem grande flexibilidade sobre quais informações são carregadas do XML e como o esquema ou estrutura relacional do <xref:System.Data.DataSet> é criado.  
  
 Para preencher um <xref:System.Data.DataSet> com dados do XML, use o método **ReadXml** do <xref:System.Data.DataSet> objeto. O método **ReadXml** lê de um arquivo, de um fluxo ou de um **XmlReader**e usa como argumentos a origem do XML, além de um argumento **XmlReadMode** opcional. Para obter mais informações sobre o **XmlReader**, consulte [lendo dados XML com XmlTextReader](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/tfz3cz6w(v=vs.100)). O método **ReadXml** lê o conteúdo do fluxo ou do documento XML e carrega o <xref:System.Data.DataSet> com os dados. Ele também criará o esquema relacional do <xref:System.Data.DataSet> dependendo do **XmlReadMode** especificado e se um esquema relacional já existe ou não.  
  
 A tabela a seguir descreve as opções para o argumento **XmlReadMode** .  
  
|Opção|Descrição|  
|------------|-----------------|  
|**Automático**|Este é o padrão. Examina o XML e escolhe a opção mais apropriada na seguinte ordem:<br /><br /> -Se o XML for um DiffGram, o **DiffGram** será usado.<br />-Se o <xref:System.Data.DataSet> contiver um esquema ou o XML contiver um esquema embutido, **ReadSchema** será usado.<br />-Se o não <xref:System.Data.DataSet> contiver um esquema e o XML não contiver um esquema embutido, **InferSchema** será usado.<br /><br /> Se você souber o formato do XML que está sendo lido, para obter o melhor desempenho, é recomendável que você defina um **XmlReadMode**explícito, em vez de aceitar o padrão **automático** .|  
|**ReadSchema**|Lê qualquer esquema embutido e carrega os dados e o esquema.<br /><br /> Se o <xref:System.Data.DataSet> já contiver um esquema, novas tabelas serão adicionadas do esquema embutido para o esquema existente no <xref:System.Data.DataSet>. Se qualquer tabela no esquema embutido já existir no <xref:System.Data.DataSet>, uma exceção será gerada. Você não poderá modificar o esquema de uma tabela existente usando **XmlReadMode. ReadSchema**.<br /><br /> Se o <xref:System.Data.DataSet> não contiver um esquema, e não houver nenhum esquema embutido, nenhum dado será lido.<br /><br /> O esquema embutido pode ser definido usando o esquema da linguagem XSD. Para obter detalhes sobre como gravar o esquema embutido como esquema XML, consulte [derivando a estrutura relacional do conjunto de dados do esquema XML (XSD)](deriving-dataset-relational-structure-from-xml-schema-xsd.md).|  
|**IgnoreSchema**|Ignora qualquer esquema embutido e carrega os dados no esquema <xref:System.Data.DataSet> existente. Os dados que não corresponderem ao esquema existente serão descartados. Se nenhum esquema existir no <xref:System.Data.DataSet>, nenhum dado será carregado.<br /><br /> Se os dados forem um DiffGram, **IgnoreSchema** terá a mesma funcionalidade que **DiffGram** *.*|  
|**InferSchema**|Ignora qualquer esquema embutido e infere o esquema pela estrutura dos dados XML, para então carregar os dados.<br /><br /> Se o <xref:System.Data.DataSet> já contiver um esquema, o esquema atual será estendido adicionando colunas às tabelas existentes. As tabelas adicionais não serão adicionadas se não houver tabelas existentes. Uma exceção será lançada se uma tabela inferida já existir com um namespace diferente, ou se qualquer coluna inferida estiver em conflito com colunas existentes.<br /><br /> Para obter detalhes sobre como o **ReadXmlSchema** infere um esquema de um documento XML, consulte [inferindo a estrutura relacional do conjunto de dados do XML](inferring-dataset-relational-structure-from-xml.md).|  
|**DiffGram**|Lê um DiffGram e adiciona os dados no esquema atual. O **DiffGram** mescla novas linhas com linhas existentes nas quais os valores de identificador exclusivos correspondem. Consulte "Mesclando dados de XML” no final deste tópico. Para obter mais informações sobre DiffGrams, consulte [DiffGrams](diffgrams.md).|  
|**Fragmento**|Continua a ler múltiplos fragmentos XML até que o final do fluxo seja alcançado. Os fragmentos que corresponderem ao esquema <xref:System.Data.DataSet> são adicionados às tabelas apropriadas. Os fragmentos que não corresponderem ao esquema <xref:System.Data.DataSet> serão descartados.|  
  
> [!NOTE]
> Se você passar um **XmlReader** para **ReadXml** que está posicionado parte do caminho em um documento XML, o **ReadXml** lerá para o próximo nó do elemento e tratará isso como o elemento raiz, lendo até o final do nó do elemento. Isso não se aplicará se você especificar **XmlReadMode. Fragment**.  
  
## <a name="dtd-entities"></a>Entidades DTD  
 Se o XML contiver entidades definidas em um esquema de definição de tipo de documento (DTD), uma exceção será gerada se você tentar carregar um <xref:System.Data.DataSet> passando um nome de arquivo, um fluxo ou um **XmlReader** de não validação para **ReadXml**. Em vez disso, você deve criar um **XmlValidatingReader**, com **EntityHandling** definido como **EntityHandling. ExpandEntities**e passar seu **XmlValidatingReader** para **ReadXml**. O **XmlValidatingReader** expandirá as entidades antes de serem lidas pelo <xref:System.Data.DataSet> .  
  
 Os seguintes exemplos de código mostram como carregar um <xref:System.Data.DataSet> de um fluxo XML. O primeiro exemplo mostra um nome de arquivo que está sendo passado para o método **ReadXml** . O segundo exemplo mostra uma cadeia de caracteres que contém o XML que está sendo carregado usando um <xref:System.IO.StringReader>.  
  
```vb  
Dim dataSet As DataSet = New DataSet  
dataSet.ReadXml("input.xml", XmlReadMode.ReadSchema)  
```  
  
```csharp  
DataSet dataSet = new DataSet();  
dataSet.ReadXml("input.xml", XmlReadMode.ReadSchema);  
```  
  
```vb  
Dim dataSet As DataSet = New DataSet  
Dim dataTable As DataTable = New DataTable("table1")  
dataTable.Columns.Add("col1", Type.GetType("System.String"))  
dataSet.Tables.Add(dataTable)  
  
Dim xmlData As String = "<XmlDS><table1><col1>Value1</col1></table1><table1><col1>Value2</col1></table1></XmlDS>"  
  
Dim xmlSR As System.IO.StringReader = New System.IO.StringReader(xmlData)  
  
dataSet.ReadXml(xmlSR, XmlReadMode.IgnoreSchema)  
```  
  
```csharp  
DataSet dataSet = new DataSet();  
DataTable dataTable = new DataTable("table1");  
dataTable.Columns.Add("col1", typeof(string));  
dataSet.Tables.Add(dataTable);  
  
string xmlData = "<XmlDS><table1><col1>Value1</col1></table1><table1><col1>Value2</col1></table1></XmlDS>";  
  
System.IO.StringReader xmlSR = new System.IO.StringReader(xmlData);  
  
dataSet.ReadXml(xmlSR, XmlReadMode.IgnoreSchema);  
```  
  
> [!NOTE]
> Se você chamar a **ReadXml** para carregar um arquivo muito grande, você poderá encontrar um desempenho lento. Para garantir o melhor desempenho para o **ReadXml**, em um arquivo grande, chame o <xref:System.Data.DataTable.BeginLoadData%2A> método para cada tabela no <xref:System.Data.DataSet> e chame **ReadXml**. Finalmente, chame <xref:System.Data.DataTable.EndLoadData%2A> para cada tabela no <xref:System.Data.DataSet>, conforme mostrado no exemplo a seguir.  
  
```vb  
Dim dataTable As DataTable  
  
For Each dataTable In dataSet.Tables  
   dataTable.BeginLoadData()  
Next  
  
dataSet.ReadXml("file.xml")  
  
For Each dataTable in dataSet.Tables  
   dataTable.EndLoadData()  
Next  
```  
  
```csharp  
foreach (DataTable dataTable in dataSet.Tables)  
   dataTable.BeginLoadData();  
  
dataSet.ReadXml("file.xml");
  
foreach (DataTable dataTable in dataSet.Tables)  
   dataTable.EndLoadData();  
```  
  
> [!NOTE]
> Se o esquema XSD para o <xref:System.Data.DataSet> incluir um **targetNamespace**, os dados poderão não ser lidos e você poderá encontrar exceções ao chamar **ReadXml** para carregar o <xref:System.Data.DataSet> com XML que contém elementos sem namespace qualificado. Para ler elementos não qualificados nesse caso, defina **elementFormDefault** igual a "qualified" no seu esquema XSD. Por exemplo:  
  
```xml  
<xsd:schema id="customDataSet"
  elementFormDefault="qualified"  
  targetNamespace="http://www.tempuri.org/customDataSet.xsd"
  xmlns="http://www.tempuri.org/customDataSet.xsd"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">  
</xsd:schema>  
```  
  
## <a name="merging-data-from-xml"></a>Mesclando dados de XML  
 Se o <xref:System.Data.DataSet> já contiver dados, os novos dados XML serão adicionados aos dados já presentes no <xref:System.Data.DataSet>. A **ReadXml** não faz a mesclagem do XML <xref:System.Data.DataSet> com as informações de linha com chaves primárias correspondentes. Para substituir as informações de linha existentes por novas informações de XML, use o **ReadXml** para criar um novo <xref:System.Data.DataSet> e, em seguida, <xref:System.Data.DataSet.Merge%2A> o novo <xref:System.Data.DataSet> no existente <xref:System.Data.DataSet> . Observe que o carregamento de um DiffGram usando **ReadXml** com um **XmlReadMode** de **DiffGram** mesclará as linhas que têm o mesmo identificador exclusivo.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Data.DataSet.Merge%2A?displayProperty=nameWithType>
- [Using XML in a DataSet](using-xml-in-a-dataset.md) (Usando XML em um DataSet)
- [DiffGrams](diffgrams.md)
- [Derivando a estrutura relacional do DataSet do esquema XML (XSD)](deriving-dataset-relational-structure-from-xml-schema-xsd.md)
- [Inferir a estrutura relacional do DataSet do esquema XML](inferring-dataset-relational-structure-from-xml.md)
- [Carregando informações de esquema de conjunto de dados de XML](loading-dataset-schema-information-from-xml.md)
- [DataSets, DataTables e DataViews](index.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
