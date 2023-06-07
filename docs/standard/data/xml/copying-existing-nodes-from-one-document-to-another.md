---
title: Copiando nós existentes de um documento para outro
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 3caa78c1-3448-4b7b-b83c-228ee857635e
ms.openlocfilehash: 8ae7fd04e5c85e59ca9bd629c6957ad470d36b48
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289194"
---
# <a name="copying-existing-nodes-from-one-document-to-another"></a>Copiando nós existentes de um documento para outro
O método **ImportNode** é o mecanismo pelo qual um nó ou uma subárvore inteira do nó são copiados de um **XmlDocument** para outro. O nó retornado de chamada é uma cópia do nó do documento de origem, incluindo valores de atributo, nome de nó, tipo de nó, e todos os atributos URL relacionados como o prefixo, o nome local, e namespace Uniform Resource Identifier (URI). O documento de origem não é alterado. Depois que você importou o nó, você ainda precisará adicioná-lo à árvore usando um dos métodos usados para nós de inserção.  
  
 Quando o nó é anexado ao novo documento, o novo documento possui o nó. O motivo é que cada nó, quando criado, tem um documento proprietário, mesmo se os nós são criados em partes separados do documento. Esse é um requisito do DOM (Modelo de Objeto do Documento) e é imposto pelo design de criação de fábrica na classe **XmlDocument**. Por exemplo, **CreateElement** é a única maneira de criar novos nós.  
  
 Dependendo do tipo do nó importado e do valor do parâmetro *deep*, informações adicionais são copiadas conforme apropriado. Este método tenta espelhar o comportamento esperado se um fragmento XML ou de código HTML foi copiado de um documento para outro, responsabilizando-se pelo fato que para XML, os dois documentos pode ter definições de tipos diferentes (DTDs) do documento.  
  
 A tabela a seguir descreve o comportamento específico para cada tipo de nó que pode ser importado.  
  
|Tipo de nó|O parâmetro *deep* é true|O parâmetro *deep* é false|  
|---------------|------------------------------|-------------------------------|  
|XmlAttribute|O <xref:System.Xml.XmlAttribute.Specified%2A> é definido como **true** no XmlAttribute. Os descendentes da origem **XmlAttribute** são importados recursivamente, e os nós resultantes são remontados para formar a subárvore correspondente.|O parâmetro *deep* não se aplica a nós **XmlAttribute**, pois eles sempre levam seus filhos quando importados.|  
|XmlCDataSection|Copia o nó, incluindo seus dados.|Copia o nó, incluindo seus dados.|  
|XmlComment|Copia o nó, incluindo seus dados.|Copia o nó, incluindo seus dados.|  
|XmlDocumentFragment|Os descendentes nó de origem recursivamente serão importados e os nós resultantes são remontados para formar a subárvore correspondente.|Um **XmlDocumentFragment** vazio é criado.|  
|XmlDocumentType|Copia o nó, incluindo seu data.*|Copia o nó, incluindo seu data.*|  
|XmlElement|Os descendentes do elemento de origem recursivamente serão importados e os nós resultantes são remontados para formar a subárvore correspondente. **Observação:** os atributos padrão não são copiados. Se o documento que está sendo importado define atributos padrão para este nome de elemento, esses são atribuídos.|Os nós de atributo especificados do elemento de origem são importados, e os nós **XmlAttribute** gerados são anexados ao novo elemento. Os nós descendentes não são copiados. **Observação:** os atributos padrão não são copiados. Se o documento que está sendo importado define atributos padrão para este nome de elemento, esses são atribuídos.|  
|XmlEntityReference|Como os documentos de origem e destino podem ter as entidades definidas de maneira diferente, esse método copia somente o nó **XmlEntityReference**. O texto de substituição não é incluído. Se o documento de destino tem a entidade definida, o valor é atribuído.|Como os documentos de origem e destino podem ter as entidades definidas de maneira diferente, esse método copia somente o nó **XmlEntityReference**. O texto de substituição não é incluído. Se o documento de destino tem a entidade definida, o valor é atribuído.|  
|XmlProcessingInstruction|Copia o destino e o valor do nó importado.|Copia o destino e o valor do nó importado.|  
|XmlText|Copia o nó, incluindo seus dados.|Copia o nó, incluindo seus dados.|  
|XmlSignificantWhitespace|Copia o nó, incluindo seus dados.|Copia o nó, incluindo seus dados.|  
|XmlWhitespace|Copia o nó, incluindo seus dados.|Copia o nó, incluindo seus dados.|  
|XmlDeclaration|Copia o destino e o valor do nó importado.|Copia o destino e o valor do nó importado.|  
|Todos os outros tipos de nó|Esses tipos de nós não podem ser importados.|Esses tipos de nós não podem ser importados.|  
  
> [!NOTE]
> Embora os nós de DocumentType possam ser importados, um documento pode ter apenas um DocumentType. Assim, uma vez que você importou o tipo de documento, antes de inseri-lo na árvore você precisará certificar-se que não há documento tipo no documento. Para saber mais sobre como remover nós, confira [Removendo nós, conteúdo e valores de um documento XML](removing-nodes-content-and-values-from-an-xml-document.md).  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
