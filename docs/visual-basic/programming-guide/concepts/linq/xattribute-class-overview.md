---
title: Visão geral da classe XAttribute
ms.date: 07/20/2015
ms.assetid: 7781580a-9583-4a1b-ae1e-91c5936eb0b1
ms.openlocfilehash: 5b165044b4bea83e1a0789e3dd00367ed27b43e8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413200"
---
# <a name="xattribute-class-overview-visual-basic"></a>Visão geral da classe XAttribute (Visual Basic)
Os atributos são pares nome/valor que são associados a um elemento. A classe de <xref:System.Xml.Linq.XAttribute> representa atributos XML.  
  
## <a name="overview"></a>Visão geral  
 Trabalhar com atributos em [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] é semelhante a trabalhar com elementos. Os construtores são semelhantes. Os métodos que você usa para recuperar coleções deless são semelhantes. Uma expressão de consulta LINQ para uma coleção de atributos parece muito semelhante a uma expressão de consulta LINQ para uma coleção de elementos.  
  
 A ordem em que os atributos foram adicionados a um elemento é preservada. Isto é, quando você itera através de atributos, você ver na mesma ordem que foram adicionados.  
  
## <a name="the-xattribute-constructor"></a>O construtor de XAttribute  
 O seguinte construtor de classe <xref:System.Xml.Linq.XAttribute> é aquele que você usará mais comumente:  
  
|Construtor|Descrição|  
|-----------------|-----------------|  
|`XAttribute(XName name, object content)`|Cria um objeto <xref:System.Xml.Linq.XAttribute>. O argumento de `name` especifica o nome do atributo; `content` especifica o conteúdo de atributo.|  
  
### <a name="creating-an-element-with-an-attribute"></a>Criando um elemento com um atributo  
 O código a seguir mostra um elemento que contém um atributo usando literais XML no Visual Basic:  
  
```vb  
Dim phone As XElement = <Phone Type="Home">555-555-5555</Phone>  
Console.WriteLine(phone)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Phone Type="Home">555-555-5555</Phone>  
```  
  
### <a name="functional-construction-of-attributes"></a>Compilação funcional de atributos  
 Você pode criar objetos de <xref:System.Xml.Linq.XAttribute> na linha de compilação de objetos <xref:System.Xml.Linq.XElement> , como segue:  
  
```vb  
Dim c As XElement = _  
    <Customers>  
        <Customer>  
            <Name>John Doe</Name>  
            <PhoneNumbers>  
                <Phone type="home">555-555-5555</Phone>  
                <Phone type="work">666-666-6666</Phone>  
            </PhoneNumbers>  
        </Customer>  
    </Customers>  
Console.WriteLine(c)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Customers>  
  <Customer>  
    <Name>John Doe</Name>  
    <PhoneNumbers>  
      <Phone type="home">555-555-5555</Phone>  
      <Phone type="work">666-666-6666</Phone>  
    </PhoneNumbers>  
  </Customer>  
</Customers>  
```  
  
### <a name="attributes-are-not-nodes"></a>Os atributos não são nós  
 Há algumas diferenças entre atributos e elementos. os objetos de<xref:System.Xml.Linq.XAttribute> não são nós na árvore XML. São pares nome/valor associados com um elemento XML. Em contraste com Document Object Model (DOM), este reflete mais apertadamente a estrutura XML. Embora os objetos de <xref:System.Xml.Linq.XAttribute> não são realmente nós na árvore XML, trabalhar com objetos de <xref:System.Xml.Linq.XAttribute> é muito semelhante a trabalhar com objetos de <xref:System.Xml.Linq.XElement> .  
  
 Essa distinção importante é primeiro somente para os desenvolvedores que estão escrevendo código que funciona com as árvores XML no nível do nó. Muitos desenvolvedores não serão preocupados com essa distinção.  
  
## <a name="see-also"></a>Confira também

- [Visão geral da programação de LINQ to XML (Visual Basic)](linq-to-xml-programming-overview.md)
