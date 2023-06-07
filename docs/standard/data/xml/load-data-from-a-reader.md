---
title: Carregando dados de um leitor
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 7e74918c-bc72-4977-a49b-e1520a6d8f60
ms.openlocfilehash: 1c048b08380bebce3a627670d88ff6ae48084535
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289155"
---
# <a name="load-data-from-a-reader"></a>Carregando dados de um leitor
Se um documento XML é carregado usando o método <xref:System.Xml.XmlDocument.Load%2A> e um parâmetro de <xref:System.Xml.XmlReader>, existem diferenças no comportamento que ocorre quando comparado ao comportamento de dados de carregamento de outro formata. Se o leitor está no estado inicial, <xref:System.Xml.XmlDocument.Load%2A> consome todo o conteúdo do leitor e compila o modelo de objeto (DOM) de documento de todos os dados no leitor.  
  
 Se o leitor já está localizado em um nó em algum lugar no documento, e o leitor é então passado para o método de <xref:System.Xml.XmlDocument.Load%2A> , tentativas de <xref:System.Xml.XmlDocument.Load%2A> de ler o nó atual e todos os seus irmãos, até a marca de fim que fecha a profundidade atual na memória. O sucesso de <xref:System.Xml.XmlDocument.Load%2A> tentado depende do nó que o leitor está na carga quando é tentada, porque <xref:System.Xml.XmlDocument.Load%2A> verifica que XML do leitor seja bem formado. Se XML bem formado, não é <xref:System.Xml.XmlDocument.Load%2A> gerencie uma exceção. Por exemplo, o seguinte conjunto de nós contém dois elementos de nível raiz, não é XML bem formado, e gera de <xref:System.Xml.XmlDocument.Load%2A> uma exceção.  
  
- Nó de comentário, seguido por um nó de elemento, seguido por um nó de elemento, seguido por um nó de EndElement.  
  
 O seguinte conjunto de nós cria os DOM incompletos, porque não há nenhum elemento de nível raiz.  
  
- Nó de comentário seguido por um nó de ProcessingInstruction seguido por um nó de comentário seguido por um nó de EndElement.  
  
 Isso não gerencie uma exceção, e os dados estão carregados. Você pode adicionar um elemento raiz na parte superior desses nós e criar o XML bem formado que pode ser salvo sem erro.  
  
 Se o leitor é posicionado em um nó folha que não é válido para o nível raiz de um documento (por exemplo, um espaço em branco ou um nó de atributo), o leitor continua a ler até que está localizado em um nó que pode ser usado para a raiz. O documento começa a carregar no momento.  
  
 Por padrão, <xref:System.Xml.XmlDocument.Load%2A> não verifica se o XML é válida usando o Document type definition (DTD) ou validação de esquema. Verifica somente se está XML bem formado. Para que a validação ocorre, você precisa criar <xref:System.Xml.XmlReader> usando a classe de <xref:System.Xml.XmlReaderSettings> . A classe <xref:System.Xml.XmlReader> pode impor a validação usando um esquema da linguagem XSD ou DTD. A propriedade de <xref:System.Xml.ValidationType> na classe de <xref:System.Xml.XmlReaderSettings> determina se a instância de <xref:System.Xml.XmlReader> aplica a validação. Para obter mais informações sobre a validação de dados XML, consulte a seção comentários da página de referência <xref:System.Xml.XmlReader>.  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
