---
title: Modificando nós, conteúdo e valores em documentos XML
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 761773e0-db72-4986-b9f5-a522213d8397
ms.openlocfilehash: f544b7d8472285095af9a71b1c24f94f61f93bc6
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288817"
---
# <a name="modifying-nodes-content-and-values-in-an-xml-document"></a>Modificando nós, conteúdo e valores em documentos XML
Existem várias maneiras de modificar os nós e o conteúdo de um documento. Você pode:  
  
- Alterar o valor dos nós usando a propriedade <xref:System.Xml.XmlNode.Value%2A>.  
  
- Modificar um conjunto inteiro de nós substituindo os nós por novos nós. Isso é feito usando a propriedade <xref:System.Xml.XmlNode.InnerXml%2A>.  
  
- Substituir nós existentes por novos nós usando o método <xref:System.Xml.XmlNode.RemoveChild%2A>.  
  
- Adicionar caracteres aos nós que herdam da classe <xref:System.Xml.XmlCharacterData> usando os métodos <xref:System.Xml.XmlCharacterData.AppendData%2A>, <xref:System.Xml.XmlCharacterData.InsertData%2A> ou <xref:System.Xml.XmlCharacterData.ReplaceData%2A>.  
  
- Modificar o conteúdo removendo um intervalo de caracteres usando o método <xref:System.Xml.XmlCharacterData.DeleteData%2A> nos tipos de nós que herdam de <xref:System.Xml.XmlCharacterData>.  
  
 Uma técnica simples de alterar o valor de um nó é usar `node.Value = "new value";`. A tabela a seguir lista os tipos de nós utilizados por essa linha de código único e os dados exatos alterados por esse tipo de nó.  
  
|Tipo de nó|Dados alterados|  
|---------------|------------------|  
|Atributo|O valor do atributo.|  
|CDATASection|O conteúdo de CDATASection.|  
|Comentário|O conteúdo do comentário.|  
|ProcessingInstruction|O conteúdo, excluindo o destino.|  
|Texto|O conteúdo do texto.|  
|XmlDeclaration|O conteúdo da declaração, excluindo as marcações `<?xml` e `?>`.|  
|Espaço em branco|O valor do espaço em branco. Você pode definir o valor como um dos quatro caracteres de espaço em branco XML reconhecidos: espaço, tabulação, CR ou LF.|  
|SignificantWhitespace|O valor do espaço em branco significativo. Você pode definir o valor como um dos quatro caracteres de espaço em branco XML reconhecidos: espaço, tabulação, CR ou LF.|  
  
 Qualquer tipo de nó não listado na tabela não é um tipo de nó válido no qual definir um valor. Definir um valor em qualquer outro tipo de nó gera <xref:System.InvalidOperationException>.  
  
 A propriedade <xref:System.Xml.XmlNode.InnerXml%2A> altera a marcação dos nós filho do nó atual. Definir essa propriedade substitui os nós filho pelo conteúdo analisado da cadeia de caracteres especificada. A análise é feita no contexto do namespace atual. Além disso, <xref:System.Xml.XmlNode.InnerXml%2A> remove as declarações redundantes de namespace. Como resultado, várias operações de recortar e colar não aumentam o tamanho do documento com declarações redundantes de namespace. Para um exemplo de código que mostra o efeito de namespaces na operação <xref:System.Xml.XmlNode.InnerXml%2A>, consulte a propriedade <xref:System.Xml.XmlDocument.InnerXml%2A>.  
  
 Ao usar os métodos <xref:System.Xml.XmlCharacterData.ReplaceData%2A> e <xref:System.Xml.XmlNode.RemoveChild%2A>, os métodos retornam o nó substituído ou o nó removido. Esse nó pode ser reinserido em outro lugar no DOM (Document Object Model) XML. O método <xref:System.Xml.XmlCharacterData.ReplaceData%2A> faz duas verificações de validação no nó que está sendo inserido no documento. A primeira verificação garante que o nó está se tornando um nó filho que pode ter nós filho do seu tipo. A segunda verificação garante que o nó que está sendo inserido não é um ancestral do nó do qual está se tornando um filho. Violar qualquer uma dessas condições gera uma <xref:System.InvalidOperationException>.  
  
 É válido adicionar ou remover um filho somente leitura de um nó que pode ser editado. No entanto, tentativas de modificar o próprio nó somente leitura geram uma <xref:System.InvalidOperationException>. Um exemplo disso é modificar os filhos de um nó <xref:System.Xml.XmlEntityReference>. Os filhos são somente leitura e não podem ser modificados. Qualquer tentativa de modificá-los gera uma <xref:System.InvalidOperationException>.  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
