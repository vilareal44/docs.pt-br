---
title: Gerenciando namespaces em um documento XML
description: Saiba como gerenciar namespaces em um documento XML. Namespaces XML e nomes de elementos e atributos em um documento XML com o URIs personalizado e predefinido.
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 682643fc-b848-4e42-8c0d-50deeaeb5f2a
ms.openlocfilehash: 3a3abd2e932b1afecab85e285b0e2c42eb1eb20f
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769256"
---
# <a name="managing-namespaces-in-an-xml-document"></a>Gerenciando namespaces em um documento XML
Namespaces XML e nomes de elementos e atributos em um documento XML com o URIs personalizado e predefinido. Para criar essas associações, você define prefixos para URIs de namespace e usa os prefixos para qualificar nomes de atributo e elemento nos dados XML. Namespaces impedem conflitos de nomes de elementos e atributos e permitem que elementos e atributos de mesmo nome sejam tratados e validados de maneira diferente.  
  
<a name="declare"></a>
## <a name="declaring-namespaces"></a>Declaração de namespaces  
 Para declarar um namespace em um elemento, você usa o atributo `xmlns:`:  
  
 `xmlns:<name>=<"uri">`  
  
 em que `<name>` é o prefixo de namespace e `<"uri">` é o URI que identifica o namespace. Após declarar o prefixo, você pode usá-lo para qualificar os elementos e atributos em um documento XML e associá-los ao URI de namespace. Porque o prefixo de namespace é usado em todo um documento, deve ser curto de tamanho.  
  
 Este exemplo define dois elementos `BOOK`. O primeiro elemento é qualificado pelo prefixo, `mybook`, e o segundo elemento é qualificado pelo prefixo, `bb`. Cada prefixo está associado a um URI de namespace diferente:  
  
```xml  
<mybook:BOOK xmlns:mybook="http://www.contoso.com/books.dtd">  
<bb:BOOK xmlns:bb="urn:blueyonderairlines" />
</mybook>
```  
  
 Para indicar que um elemento é uma parte de um namespace específico, adicione o prefixo de namespace a ele. Por exemplo, se um elemento `Author` pertence ao namespace `mybook`, ele é declarado como `<mybook:Author>`.  
  
<a name="scope"></a>
## <a name="declaration-scope"></a>Escopo de declaração  
 Um namespace é efetivo do ponto de declaração até o fim do elemento em que foi declarado. Neste exemplo, o namespace definido no elemento `BOOK` não se aplica a elementos fora do elemento `BOOK`, como o elemento `Publisher`:  
  
```xml  
<Author>Joe Smith</Author>  
<BOOK xmlns:book="http://www.contoso.com">  
    <title>My Wonderful Day</title>  
      <price>$3.95</price>  
</BOOK>  
<Publisher>  
    <Name>MSPress</Name>  
</Publisher>  
```  
  
 Um namespace deve ser declarado para poder ser usado, mas não precisa aparecer na parte superior do documento XML.  
  
 Ao usar vários namespaces em um documento XML, você pode definir um namespace como o namespace padrão para criar um documento com aparência mais clara. O namespace padrão é declarado no elemento raiz e se aplica a todos os elementos não qualificados no documento. Namespaces padrão se aplicam apenas a elementos, não a atributos.  
  
 Para usar o namespace padrão, omita o prefixo e os dois-pontos da declaração no elemento:  
  
```xml  
<BOOK xmlns="http://www.contoso.com/books.dtd">  
...
</BOOK>
```  
  
## <a name="managing-namespaces"></a>Gerenciando namespaces  
 A classe <xref:System.Xml.XmlNamespaceManager> armazena uma coleção de URIs de namespace e seus prefixos e permite que você veja, adicione e remova namespaces dessa coleção. Em determinados contextos, essa classe é necessária para fornecer melhor desempenho de processamento de XML. Por exemplo, a classe <xref:System.Xml.Xsl.XsltContext> usa <xref:System.Xml.XmlNamespaceManager> para suporte a XPath.  
  
 O gerenciador de namespace não executa nenhuma validação nos namespaces, mas presume que namespaces e prefixos já tenham sido verificados e estejam de acordo com a especificação [Namespaces do W3C](https://www.w3.org/TR/REC-xml-names/).  
  
> [!NOTE]
> LINQ TO XML no [C#](../../../csharp/programming-guide/concepts/linq/linq-to-xml-overview.md) e [Visual Basic](../../../visual-basic/programming-guide/concepts/linq/linq-to-xml.md) não usam <xref:System.Xml.XmlNamespaceManager> para gerenciar namespaces. Confira [Trabalhando com namespaces de XML(C#)](../../../csharp/programming-guide/concepts/linq/namespaces-overview-linq-to-xml.md) e [Trabalhando com namespaces de XML (Visual Basic)](../../../visual-basic/programming-guide/concepts/linq/working-with-xml-namespaces.md) na documentação do LINQ para obter informações sobre como gerenciar namespaces ao usar o LINQ to XML.  
  
 Aqui estão algumas das tarefas de gerenciamento e de pesquisa podem ser executadas com a classe <xref:System.Xml.XmlNamespaceManager>. Para saber mais e exemplos, siga os links para a página de referência para cada método ou propriedade.  
  
|Para|Use|  
|--------|---------|  
|Adicionar um namespace|Método <xref:System.Xml.XmlNamespaceManager.AddNamespace%2A>|  
|Remover um namespace|Método <xref:System.Xml.XmlNamespaceManager.RemoveNamespace%2A>|  
|Localizar o URI do namespace padrão|Propriedade <xref:System.Xml.XmlNamespaceManager.DefaultNamespace%2A>|  
|Localizar o URI para um prefixo de namespace|Método <xref:System.Xml.XmlNamespaceManager.LookupNamespace%2A>|  
|Localizar o prefixo para um URI de namespace|Método <xref:System.Xml.XmlNamespaceManager.LookupPrefix%2A>|  
|Obter uma lista de namespaces do nó atual|Método <xref:System.Xml.XmlNamespaceManager.GetNamespacesInScope%2A>|  
|Definir o escopo de um namespace|Métodos <xref:System.Xml.XmlNamespaceManager.PushScope%2A> e <xref:System.Xml.XmlNamespaceManager.PopScope%2A>|  
|Verificar se um prefixo é definido no escopo atual|Método <xref:System.Xml.XmlNamespaceManager.HasNamespace%2A>|  
|Obter a tabela de nomes usada para pesquisar URIs e prefixos|Propriedade <xref:System.Xml.XmlNamespaceManager.NameTable%2A>|  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlNamespaceManager>
- [Documentos e dados XML](index.md)
