---
title: Acessar XML
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ to XML [Visual Basic], accessing XML
- Visual Basic code, XML
- accessing XML [Visual Basic], axis properties
- XML [Visual Basic], axis properties
- XML [Visual Basic], accessing
ms.assetid: c47f88b2-3cbc-4bb1-b4b9-be60f71ffc6a
ms.openlocfilehash: 282b7d91ec7cfe2f587c67bc9a982f0da22ad925
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410303"
---
# <a name="accessing-xml-in-visual-basic"></a>Acessando XML no Visual Basic
Visual Basic fornece propriedades de eixo XML para acessar e navegar em [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] estruturas. Essas propriedades usam uma sintaxe especial para permitir que você acesse elementos e atributos especificando os nomes XML.  
  
 A tabela a seguir lista os recursos de linguagem que permitem acessar elementos e atributos XML no Visual Basic.  
  
### <a name="xml-axis-properties"></a>Propriedades do eixo XML  
  
|Descrição da propriedade|Exemplo|Descrição|  
|--------------------------|-------------|-----------------|  
|*eixo filho*|`contact.<phone>`|Obtém todos os `phone` elementos que são elementos filho do `contact` elemento.|  
|*eixo de atributo*|`phone.@type`|Obtém todos os `type` atributos do `phone` elemento.|  
|*eixo descendente*|`contacts...<name>`|Obtém todos os `name` elementos do `contacts` elemento, independentemente da profundidade na hierarquia que eles ocorrem.|  
|*indexador de extensão*|`contacts...<name>(0)`|Obtém o primeiro `name` elemento da sequência.|  
|*value*|`contacts...<name>.Value`|Obtém a representação da cadeia de caracteres do primeiro objeto na sequência ou `Nothing` se a sequência está vazia.|  
  
## <a name="in-this-section"></a>Nesta seção  
 [Como acessar elementos descendentes XML](how-to-access-xml-descendant-elements.md)  
 Mostra como usar uma propriedade de eixo descendente para acessar todos os elementos XML que têm um nome especificado e que estão contidos em um elemento XML especificado.  
  
 [Como acessar elementos filho XML](how-to-access-xml-child-elements.md)  
 Mostra como usar uma propriedade de eixo filho para acessar todos os elementos filho XML que têm um nome especificado em um elemento XML.  
  
 [Como acessar atributos XML](how-to-access-xml-attributes.md)  
 Mostra como usar uma propriedade de eixo de atributo para acessar todos os atributos XML que têm um nome especificado em um elemento XML.  
  
 [Como declarar e usar prefixos de namespace XML](how-to-declare-and-use-xml-namespace-prefixes.md)  
 Mostra como declarar um prefixo de namespace XML e usá-lo para criar e acessar elementos XML.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Propriedades do eixo XML](../../../language-reference/xml-axis/index.md)  
 Fornece links para seções que descrevem as várias propriedades de acesso de XML.  
  
 [Visão geral de LINQ to XML no Visual Basic](overview-of-linq-to-xml.md)  
 Fornece uma introdução ao uso [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] do no Visual Basic.  
  
 [Criando XML no Visual Basic](creating-xml.md)  
 Fornece uma introdução ao uso de literais XML no Visual Basic.  
  
 [Manipulando XML no Visual Basic](manipulating-xml.md)  
 Fornece links para seções sobre como carregar e modificar XML no Visual Basic.  
  
 [XML](index.md)  
 Fornece links para seções que descrevem como usar o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] no Visual Basic.
