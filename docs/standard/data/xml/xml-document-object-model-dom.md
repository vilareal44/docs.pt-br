---
title: XML Document Object Model (DOM)
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: b5e52844-4820-47c0-a61d-de2da33e9f54
ms.openlocfilehash: dbc53d713d77cfdc9d0dbb8a201f2b5627a76921
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84283384"
---
# <a name="xml-document-object-model-dom"></a>XML Document Object Model (DOM)

A classe do DOM (Document Object Model) XML é uma representação na memória de um documento XML. O DOM permite que você leia, manipule e modifique programaticamente um documento XML. A classe **XmlReader** também lê XML. No entanto, ela fornece acesso não armazenado em cache, apenas de encaminhamento e somente leitura. Isso significa que não há nenhum recurso para editar os valores de um atributo ou o conteúdo de um elemento ou a capacidade de inserir e remover nós com **XmlReader**. A edição é a função primária do DOM. É a maneira comum e estruturada como os dados XML são representados na memória, embora os dados XML reais sejam armazenados de uma forma linear quando em um arquivo ou ao vir de outro objeto. Os seguintes são dados XML.

## <a name="input"></a>Entrada

```xml
<?xml version="1.0"?>
  <books>
    <book>
        <author>Carson</author>
        <price format="dollar">31.95</price>
        <pubdate>05/01/2001</pubdate>
    </book>
    <pubinfo>
        <publisher>MSPress</publisher>
        <state>WA</state>
    </pubinfo>
  </books>
```

A ilustração a seguir mostra como a memória é estruturada quando esses dados XML são lidos na estrutura DOM.

![Estrutura do documento XML](media/xml-to-domtree.gif "XML_To_DOMTree") Estrutura do documento XML

Na estrutura de documento XML, cada círculo nesta ilustração representa um nó, que é chamado um objeto **XmlNode**. O objeto **XmlNode** é o objeto básico na árvore DOM. A classe **XmlDocument**, que estende **XmlNode**, dá suporte a métodos para execução de operações no documento como um todo (por exemplo, carregando-o na memória ou salvando o XML em um arquivo. Além disso, **XmlDocument** fornece um meio de exibir e manipular os nós no documento XML inteiro. **XmlNode** e **XmlDocument** têm aprimoramentos de desempenho e de usabilidade e têm métodos e propriedades para:

- Acessar e modificar nós específicos para o DOM, como nós de elemento, nós de referência de entidade e assim por diante.

- Recuperar nós inteiros, além das informações que o nó contém, como texto em um nó de elemento.

  > [!NOTE]
  > Se um aplicativo não exigir os recursos de estrutura ou de edição fornecidos pelo DOM, as classes **XmlReader** e **XmlWriter** fornecem acesso não armazenado em cache, fluxo apenas de encaminhamento ao XML. Para obter mais informações, consulte <xref:System.Xml.XmlReader> e <xref:System.Xml.XmlWriter>.

Os objetos **Node** têm um conjunto de métodos e propriedades e características básicas e bem-definidas. Algumas dessas características são:

- Os nós possuem um único nó pai - um nó pai que está um nó diretamente acima deles. Os únicos nós que não possuem um pai são a raiz do documento, pois são os nós de nível superior e contêm o próprio documento e fragmentos do documento.

- A maioria dos nós pode possuir vários nós filho, que são nós diretamente abaixo deles. A seguir está uma lista de tipos de nós que podem possuir nós filho.

  - **Documento**

  - **DocumentFragment**

  - **EntityReference**

  - **Elemento**

  - **Atributo**

  Os nós **XmlDeclaration**, **Notation**, **Entity**, **CDATASection**, **Text**, **Comment**, **ProcessingInstruction** e **DocumentType** não têm nós filho.

- Os nós que estão no mesmo nível, representados no diagrama pelos nós **book** e **pubinfo**, são irmãos.

Uma característica do DOM é como ele lida com atributos. Os atributos não são nós que fazem parte de relações de pai, filho e irmão. Os atributos são considerados uma propriedade do nó do elemento e são compostos de um par nome/valor. Por exemplo, se você tiver dados XML consistindo em `format="dollar` associado ao elemento `price`, a palavra `format` será o nome, e o valor do atributo `format` será `dollar`. Para recuperar o atributo `format="dollar"` do nó **price**, você chama o método **GetAttribute** quando o cursor está localizado no nó do elemento `price`. Para saber mais, confira [Acessando atributos no DOM](accessing-attributes-in-the-dom.md).

À medida que o XML é lido na memória, os nós são criados. No entanto, nem todos os nós são do mesmo tipo. Um elemento em XML tem regras e sintaxe diferentes de uma instrução de processamento. Portanto, como vários dados são lidos, um tipo é atribuído a cada nó. Esse tipo de nó determina as características e a funcionalidade do nó.

Para saber mais sobre os tipos de nós gerados na memória, confira [Tipos de nós XML](types-of-xml-nodes.md). Para saber mais sobre objetos criados na árvore de nós, confira [Mapeando a hierarquia de objetos para dados XML](mapping-the-object-hierarchy-to-xml-data.md).

A Microsoft estendeu as APIs que estão disponíveis no Nível 1 e no Nível 2 do DOM do W3C (World Wide Web Consortium) para facilitar o trabalho com um documento XML. Para dar suporte total aos padrões do W3C, as classes, métodos e propriedades adicionais aumentam a funcionalidade além daquilo que pode ser feito com o DOM XML do W3C. As novas classes permitem que você acesse dados relacionais, oferecendo métodos para sincronização com dados ADO.NET, expondo, simultaneamente, os dados como XML. Para saber mais, confira [Sincronizando um DataSet com um XmlDataDocument](../../../framework/data/adonet/dataset-datatable-dataview/dataset-and-xmldatadocument-synchronization.md).

O DOM é mais útil para ler dados XML na memória para alterar sua estrutura, para adicionar ou remover nós ou para modificar os dados mantidos por um nó, como no texto contido por um elemento. No entanto, estão disponíveis outras classes que são mais rápidas do que o DOM em outros cenários. Para acesso rápido, não armazenado em cache, de fluxo apenas de encaminhamento ao XML, use **XmlReader** e **XmlWriter**. Se você precisar de acesso aleatório com um modelo de cursor e **XPath**, use a classe **XPathNavigator**.

## <a name="see-also"></a>Veja também

- [Tipos de nós XML](types-of-xml-nodes.md)
- [Mapeando a hierarquia do objeto para dados XML](mapping-the-object-hierarchy-to-xml-data.md)
