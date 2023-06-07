---
title: Integração XML com dados relacionais e o ADO.NET
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: f6ebb1a1-f2ca-49b9-92c9-0150940cf6e6
ms.openlocfilehash: f54c7a890ada01f2cffdd54c024cfbc98777200d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289012"
---
# <a name="xml-integration-with-relational-data-and-adonet"></a>Integração XML com dados relacionais e o ADO.NET
A classe **XmlDataDocument** é uma classe derivada de **XmlDocument**, e contém dados XML. A vantagem de **XmlDataDocument** é que fornece uma ponte entre dados relacionais e hierárquicos. É um **XmlDocument** que pode ser associado a **Conjunto de Dados** e ambas as classes podem sincronizar as alterações feitas aos dados contidos nas duas classes. Um **XmlDocument** que é associado a um **Conjunto de Dados** permite ao XML integrar-se a dados relacionais, e você não precisa representar seus dados como XML ou em um formato relacional. Você pode fazer ambos e não ser restrito a uma única representação dos dados.  
  
 Os benefícios de ter os dados disponíveis em dois modos de exibição são:  
  
- A parte estruturada de um documento XML pode ser mapeado para um dataset, e com eficiência é armazenada, indexada, e pesquisada.  
  
- As transformações, a validação, e navegação podem ser feitas com eficiência através de um modelo de cursor sobre os dados XML que são armazenados relacional. Às vezes, pode ser feito com mais eficiência em estruturas relacionais em comparação ao armazenamento de XML em um modelo **XmlDocument**.  
  
- O **Conjunto de Dados** pode armazenar uma parte do XML. Ou seja, você pode usar **XPath** ou **XslTransform** para armazenar em um **Conjunto de Dados** somente os elementos e atributos de interesse. A partir daí, as alterações podem ser feitas no subconjunto de dados menor e filtrado, com a propagação das alterações nos dados maiores em **XmlDataDocument**.  
  
 Você também pode executar uma transformação sobre os dados que foram carregados no **Conjunto de Dados** a partir do SQL Server. Outra opção é associar controles WinForm e WebForm gerenciados por estilo de classes do .NET Framework a um **Conjunto de Dados** que foi preenchido a partir um fluxo de entrada XML.  
  
 Além de dar suporte a **XslTransform**, um **XmlDataDocument** expõe dados relacionais a consultas e validação de **XPath**.  Basicamente, todos os serviços XML estão disponíveis sobre dados relacionais, e os recursos relacionais, como associação de controle, gerador, e assim por diante, estão disponíveis sobre uma projeção estruturada XML sem confirmar a fidelidade XML.  
  
 Como **XmlDataDocument** é herdado de um **XmlDocument**, ele fornece uma implementação DOM de W3C. O fato de **XmlDataDocument** estar associado a, e armazenar um subconjunto de seus dados dentro de, um **Conjunto de Dados** não restringe ou altera seu uso como **XmlDocument** de forma alguma. O código escrito para consumir **XmlDocument** funciona sem alteração com **XmlDataDocument**. O **Conjunto de Dados** fornece a exibição relacional dos mesmos dados definindo tabelas, colunas, relações e restrições, e é um repositório de dados de usuário autônomo e na memória.  
  
 A ilustração a seguir mostra a associações diferentes que os dados XML têm com **Conjunto de Dados** e **XmlDataDocument**:
  
 ![Diagrama que mostra as diferentes associações com o Conjunto de Dados XML.](./media/xml-integration-with-relational-data-and-adonet/xml-integration-relational-data-adodotnet.gif)  
  
 A ilustração mostra que os dados XML podem ser carregados diretamente em um **Conjunto de Dados**, que permitem o tratamento direto com XML na forma relacional. Ou o XML pode ser carregado em uma classe derivada de DOM, que é **XmlDataDocument**, e depois ser carregado e sincronizado com o **Conjunto de Dados**. Como o **Conjunto de Dados** e **XmlDataDocument** são sincronizados sobre um único conjunto de dados, as alterações feitas nos dados em um armazenamento são refletidas no outro armazenamento.  
  
 O **XmlDataDocument** herda toda edição e os recursos de navegação de **XmlDocument**. Às vezes, usar **XmlDataDocument** e seus recursos herdados, sincronizados com um **Conjunto de Dados**, é uma opção mais apropriada do que carregar o XML diretamente no **Conjunto de Dados**. A tabela a seguir mostra os itens a serem considerados ao escolher qual método usar para carregar o **Conjunto de Dados**.  
  
|Quando carregar XML diretamente em um dataset|Quando um XmlDataDocument sincronizar com um dataset|  
|----------------------------------------------|-----------------------------------------------------------|  
|As consultas de dados no **Conjunto de Dados** são mais fáceis usando SQL do que XPath.|As consultas XPath são necessárias nos dados no **Conjunto de Dados**.|  
|Preservação do elemento de ordenação em XML de origem não é essencial.|Preservação do elemento de ordenação no XML fonte é essencial.|  
|Espaço em branco entre elementos e formatação não precisa ser mantido no XML fonte.|Preservação de espaço em branco e de formatação no XML fonte é essencial.|  
  
 Se carregar e escrever XML diretamente no **Conjunto de Dados** atender às suas necessidades, consulte [Carregar um Conjunto de Dados a partir de XML](../../../framework/data/adonet/dataset-datatable-dataview/loading-a-dataset-from-xml.md) e [Escrever um Conjunto de Dados como dados de XML](../../../framework/data/adonet/dataset-datatable-dataview/writing-dataset-contents-as-xml-data.md).  
  
 Se carregar o **Conjunto de Dados** de um **XmlDataDocument** atender às suas necessidades, consulte [Sincronizar um Conjunto de Dados com um documento XML](../../../framework/data/adonet/dataset-datatable-dataview/dataset-and-xmldatadocument-synchronization.md).  
  
## <a name="see-also"></a>Veja também

- [Using XML in a DataSet](../../../framework/data/adonet/dataset-datatable-dataview/using-xml-in-a-dataset.md) (Usando XML em um DataSet)
