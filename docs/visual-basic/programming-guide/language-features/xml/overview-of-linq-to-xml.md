---
title: Visão Geral de LINQ to XML
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ to XML [Visual Basic], about LINQ to XML
- LINQ [Visual Basic], LINQ to XML
ms.assetid: 01c62a79-6d58-468e-84fb-039c05947701
ms.openlocfilehash: 044aca634f5a0aa6e557a7dd9c0e1de64e35dc15
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84374649"
---
# <a name="overview-of-linq-to-xml-in-visual-basic"></a>Visão geral de LINQ to XML no Visual Basic
Visual Basic fornece suporte para [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] literais XML e propriedades de eixo XML. Isso permite que você use uma sintaxe familiar e conveniente para trabalhar com XML em seu código de Visual Basic. Os *literais XML* permitem que você inclua XML diretamente em seu código. *As propriedades do eixo XML* permitem que você acesse nós filho, nós descendentes e atributos de um literal XML. Para obter mais informações, consulte [visão geral de literais XML](xml-literals-overview.md) e [acessando XML no Visual Basic](accessing-xml.md).  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]é uma API de programação XML na memória projetada especificamente para tirar proveito da consulta integrada à linguagem (LINQ). Embora você possa chamar as APIs do LINQ diretamente, somente Visual Basic permite que você declare literais XML e acesse diretamente as propriedades do eixo XML.  
  
> [!NOTE]
> Não há suporte para literais XML e propriedades de eixo XML em código declarativo em uma página ASP.NET. Para usar Visual Basic recursos XML, coloque seu código em uma página code-behind em seu aplicativo ASP.NET.  
  
 [Botão reproduzir](./media/overview-of-linq-to-xml/play-video-icon-example.gif) Para ver demonstrações de vídeo relacionadas, consulte [como faço para começar a usar LINQ to XML?](/aspnet/web-forms/videos/data-access/linq-videos-from-the-vb-team/how-do-i-get-started-with-linq-to-xml) e [como criar planilhas Do Excel usando LINQ to XML?](/aspnet/web-forms/videos/data-access/linq-videos-from-the-vb-team/how-do-i-create-excel-spreadsheets-using-linq-to-xml).
  
## <a name="creating-xml"></a>Criar XML  
 Há duas maneiras de criar árvores XML no Visual Basic. Você pode declarar um literal XML diretamente no código ou pode usar as APIs do LINQ para criar a árvore. Ambos os processos permitem que o código reflita a estrutura final da árvore XML. Por exemplo, o exemplo de código a seguir cria um elemento XML:  
  
 [!code-vb[VbXmlSamples#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples2.vb#5)]  
  
 Para obter mais informações, consulte [CREATING XML in Visual Basic](creating-xml.md).  
  
## <a name="accessing-and-navigating-xml"></a>Acessando e navegando em XML  
 Visual Basic fornece propriedades de eixo XML para acessar e navegar em estruturas XML. Essas propriedades permitem que você acesse elementos XML e atributos especificando os nomes de elemento filho XML. Como alternativa, você pode chamar explicitamente os métodos LINQ para navegar e localizar elementos e atributos. Por exemplo, o exemplo de código a seguir usa propriedades de eixo XML para se referir aos atributos e elementos filho de um elemento XML. O exemplo de código usa uma consulta LINQ para recuperar elementos filho e saídas como elementos XML, executando efetivamente uma transformação.  
  
 [!code-vb[VbXmlSamples#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples3.vb#8)]  
  
 Para obter mais informações, consulte [acessando XML no Visual Basic](accessing-xml.md).  
  
## <a name="xml-namespaces"></a>Namespaces XML  
 Visual Basic permite que você especifique um alias para um namespace XML global usando a `Imports` instrução. O exemplo a seguir mostra como usar a `Imports` instrução para importar um namespace XML:  
  
 [!code-vb[VbXMLSamples#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples1.vb#1)]  
  
 Você pode usar um alias de namespace XML ao acessar as propriedades do eixo XML e declarar literais XML para documentos e elementos XML.  
  
 Você pode recuperar um <xref:System.Xml.Linq.XNamespace> objeto para um prefixo de namespace específico usando o [Operador GetXmlNamespace](../../../language-reference/operators/getxmlnamespace-operator.md).  
  
 Para obter mais informações, consulte [instrução Imports (namespace XML)](../../../language-reference/statements/imports-statement-xml-namespace.md).  
  
### <a name="using-xml-namespaces-in-xml-literals"></a>Usando namespaces XML em literais XML  
 O exemplo a seguir mostra como criar um <xref:System.Xml.Linq.XElement> objeto que usa o namespace global `ns` :  
  
 [!code-vb[VbXMLSamples#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples1.vb#2)]  
  
 O compilador Visual Basic converte literais XML que contêm aliases de namespace XML em código equivalente que usa a notação XML para usar namespaces XML, com o `xmlns` atributo. Quando compilado, o código no exemplo da seção anterior produz essencialmente o mesmo código executável como o exemplo a seguir:  
  
 [!code-vb[VbXMLSamples#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples1.vb#3)]  
  
### <a name="using-xml-namespaces-in-xml-axis-properties"></a>Usando namespaces XML em Propriedades do eixo XML  
 Namespaces XML declarados em literais XML não estão disponíveis para uso em Propriedades de eixo XML. No entanto, namespaces globais podem ser usados com as propriedades do eixo XML. Use dois pontos para separar o prefixo do namespace XML do nome do elemento local. A seguir está um exemplo:  
  
 [!code-vb[VbXMLSamples#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples1.vb#4)]  
  
## <a name="see-also"></a>Confira também

- [XML](index.md)
- [Criando XML no Visual Basic](creating-xml.md)
- [Acessando XML no Visual Basic](accessing-xml.md)
- [Manipulando XML no Visual Basic](manipulating-xml.md)
