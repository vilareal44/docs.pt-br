---
title: Visão geral sobre namespaces (LINQ to XML)
description: Apresente-se aos namespaces, à classe XName, à classe XNamespace e a outros aspectos de nomes XML.
ms.date: 07/20/2015
ms.assetid: 16283322-8238-4918-ab11-802ac6748eb7
ms.openlocfilehash: 56dca481dd5f3066fa5359fc57b3311cf2569ee0
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87300157"
---
# <a name="namespaces-overview-linq-to-xml"></a>Visão geral sobre namespaces (LINQ to XML)

Este artigo apresenta namespaces, a classe <xref:System.Xml.Linq.XName> e a classe <xref:System.Xml.Linq.XNamespace>.

## <a name="xml-names"></a>Nomes XML

Os nomes XML são geralmente uma fonte de complexidade na programação XML. Um nome de XML consiste em um namespace XML (também chamado um URI de um namespace XML) e um nome local. Um namespace XML é semelhante a um namespace em um programa .NET. Permite que você determine exclusivamente os nomes de elementos e atributos. Isso ajuda a evitar conflitos de nome entre várias partes de um documento XML. Quando você declarar um namespace de XML, poderá selecionar um nome local que somente deve ser exclusivo dentro desse namespace.

Outro aspecto de nomes XML são os *prefixos de namespace* de XML. Os prefixos XML causam a maioria da complexidade de nomes XML. Esses prefixos permite que você crie um atalho para um namespace XML, que faz o documento XML mais concisas e legível. No entanto, os prefixos XML depende do seu contexto para ter significado, que adiciona a complexidade. Por exemplo, o prefixo `aw` XML pode ser associado com a um namespace XML de uma parte de uma árvore XML, e com um outro namespace XML em uma parte diferente da árvore XML.

Uma das vantagens de usar [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] com C# é que você não precisa usar prefixos XML. Quando o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] carrega ou analisa um documento XML, cada prefixo XML é resolvido para o namespace de XML correspondente. Após isso, quando você trabalha com um documento que usar namespaces, você acessa quase sempre namespaces com o URI de namespace, e não com o prefixo de namespace. Quando os desenvolvedores trabalham com nomes XML em [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] sempre trabalham com um nome XML totalmente qualificado (isto é, um namespace de XML e um nome local). No entanto, quando necessário, [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] permite que você trabalhe com controle e prefixos de namespace.

Em [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], a classe que representa nomes XML é <xref:System.Xml.Linq.XName>. Os nomes XML aparecem com frequência em toda a API [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] e, em qualquer lugar em que um nome XML for necessário, você encontrará um parâmetro <xref:System.Xml.Linq.XName>. No entanto, raramente você trabalha diretamente com um <xref:System.Xml.Linq.XName>. <xref:System.Xml.Linq.XName> contém uma conversão implícita de cadeia de caracteres.

Para obter mais informações, consulte <xref:System.Xml.Linq.XNamespace> e <xref:System.Xml.Linq.XName>.
