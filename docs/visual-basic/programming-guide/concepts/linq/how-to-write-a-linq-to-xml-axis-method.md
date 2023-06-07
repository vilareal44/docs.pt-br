---
title: 'Como: escrever um método de eixo LINQ to XML'
ms.date: 07/20/2015
ms.assetid: b676f025-a24c-4076-8713-aa809b2b8ce0
ms.openlocfilehash: 0a20bef800c07b0db8ed47c7a04fd102c1076588
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397642"
---
# <a name="how-to-write-a-linq-to-xml-axis-method-visual-basic"></a>Como escrever um método de eixo de LINQ to XML (Visual Basic)
Você pode escrever seus próprios métodos do eixo para recuperar coleções de uma árvore XML. Uma das melhores maneiras de fazer isso é gravar um método de extensão que retorna uma coleção de elementos ou atributos. Você pode escrever seu método de extensão para subconjuntos específicos de retorno de elementos ou atributos, com base nos requisitos do seu aplicativo.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa dois métodos de extensão. O primeiro método de extensão, `GetXPath`, opera em <xref:System.Xml.Linq.XObject>, e retorna uma expressão XPath que quando avaliada retorna o nó ou do atributo. O segundo método de extensão, `Find`, opera em <xref:System.Xml.Linq.XElement>. Retorna uma coleção de objetos <xref:System.Xml.Linq.XAttribute> e objetos de <xref:System.Xml.Linq.XElement> que contêm texto especificado.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: vários pedidos de compra (LINQ to XML)](sample-xml-file-multiple-purchase-orders-linq-to-xml.md).  
  
```vb  
Imports System.Runtime.CompilerServices  
Imports System.Text  
  
Module Module1  
    Sub Main()  
        Dim purchaseOrders = XElement.Load("..\..\PurchaseOrders.xml")  
  
        Dim subset = From xobj In purchaseOrders.Find("1999")  
  
        For Each obj In subset  
            Console.WriteLine(obj.GetXPath())  
            If obj.GetType() = GetType(XElement) Then  
                Console.WriteLine(CType(obj, XElement).Value)  
            ElseIf obj.GetType() = GetType(XAttribute) Then  
                Console.WriteLine(CType(obj, XAttribute).Value)  
            End If  
        Next  
    End Sub  
End Module  
  
Public Module MyExtensions  
  
    Private Function GetQName(ByVal xe As XElement) As String  
        Dim prefix = xe.GetPrefixOfNamespace(xe.Name.Namespace)  
        If xe.Name.Namespace = XNamespace.None OrElse prefix Is Nothing Then  
            Return xe.Name.LocalName  
        Else  
            Return prefix & ":" & xe.Name.LocalName  
        End If  
    End Function  
  
    Private Function GetQName(ByVal xa As XAttribute) As String  
        Dim prefix = xa.Parent.GetPrefixOfNamespace(xa.Name.Namespace)  
        If xa.Name.Namespace = XNamespace.None OrElse prefix Is Nothing Then  
            Return xa.Name.LocalName  
        Else  
            Return prefix & ":" & xa.Name.LocalName  
        End If  
    End Function  
  
    Private Function NameWithPredicate(ByVal el As XElement) As String  
        If el.Parent IsNot Nothing AndAlso  
           el.Parent.Elements(el.Name).Count() <> 1 Then  
            Return GetQName(el) & "[" &  
                (el.ElementsBeforeSelf(el.Name).Count() + 1) & "]"  
        Else  
            Return GetQName(el)  
        End If  
    End Function  
  
    <Extension()>  
    Public Function StrCat(Of T)(ByVal source As IEnumerable(Of T),  
                                 ByVal separator As String) As String  
        Return source.Aggregate(New StringBuilder,  
                                Function(sb, i) sb.  
                                    Append(i.ToString()).  
                                    Append(separator),  
                                    Function(s) s.ToString())  
    End Function  
  
    <Extension()>  
    Public Function GetXPath(ByVal xobj As XObject) As String  
  
        If xobj.Parent Is Nothing Then  
            Dim doc = TryCast(xobj, XDocument)  
            If doc IsNot Nothing Then Return "."  
  
            Dim el = TryCast(xobj, XElement)  
            If el IsNot Nothing Then Return "/" + NameWithPredicate(el)  
  
            ' the XPath data model does not include white space text nodes  
            ' that are children of a document, so this method returns null.  
  
            Dim xt = TryCast(xobj, XText)  
            If xt IsNot Nothing Then Return Nothing  
  
            Dim com = TryCast(xobj, XComment)  
            If com IsNot Nothing Then  
                Return "/" &  
                    If(com.Document.Nodes().OfType(Of XComment)().Count() <> 1,  
                       "comment()[" & (com.NodesBeforeSelf().OfType(Of XComment)().Count() + 1) & "]",  
                       "comment()")  
            End If  
  
            Dim pi = TryCast(xobj, XProcessingInstruction)  
            If pi IsNot Nothing Then  
                Return "/" &  
                    If(pi.Document.Nodes().OfType(Of XProcessingInstruction)().Count() <> 1,  
                       "processing-instruction()[" &  
                           (pi.NodesBeforeSelf().OfType(Of XProcessingInstruction)().Count() + 1) & "]",  
                       "processing-instruction()")  
            End If  
            Return Nothing  
        Else  
            Dim el = TryCast(xobj, XElement)  
            If el IsNot Nothing Then  
                Return "/" &  
                    el.Ancestors().  
                    InDocumentOrder().  
                    Select(Function(e) NameWithPredicate(e)).StrCat("/") & NameWithPredicate(el)  
            End If  
            Dim at = TryCast(xobj, XAttribute)  
            If at IsNot Nothing Then  
                Return "/" &  
                    at.Parent.  
                    AncestorsAndSelf().  
                    InDocumentOrder().  
                    Select(Function(e) NameWithPredicate(e)).StrCat("/") & "@" & GetQName(at)  
            End If  
            Dim com = TryCast(xobj, XComment)  
            If com IsNot Nothing Then  
                Return "/" &  
                    com.Parent.  
                    AncestorsAndSelf().  
                    InDocumentOrder().  
                    Select(Function(e) NameWithPredicate(e)).StrCat("/") &  
                        If(com.Parent.Nodes().OfType(Of XComment)().Count() <> 1,  
                           "comment()[" & (com.NodesBeforeSelf().OfType(Of XComment)().Count() + 1) & "]",  
                           "comment()")  
            End If  
  
            Dim cd = TryCast(xobj, XCData)  
            If cd IsNot Nothing Then  
                Return "/" &  
                    cd.Parent.  
                    AncestorsAndSelf().  
                    InDocumentOrder().  
                    Select(Function(e) NameWithPredicate(e)).StrCat("/") &  
                        If(cd.Parent.Nodes().OfType(Of XText)().Count() <> 1,  
                           "text()[" & (cd.NodesBeforeSelf().OfType(Of XText)().Count() + 1) & "]",  
                           "text()")  
            End If  
            Dim tx = TryCast(xobj, XText)  
            If tx IsNot Nothing Then  
                Return "/" &  
                    tx.Parent.  
                    AncestorsAndSelf().  
                    InDocumentOrder().  
                    Select(Function(e) NameWithPredicate(e)).StrCat("/") &  
                        If(tx.Parent.Nodes().OfType(Of XText)().Count() <> 1,  
                           "text()[" & (tx.NodesBeforeSelf().OfType(Of XText)().Count() + 1) & "]",  
                           "text()")  
            End If  
            Dim pi As XProcessingInstruction = TryCast(xobj, XProcessingInstruction)  
            If pi IsNot Nothing Then  
                Return "/" &  
                    pi.Parent.  
                    AncestorsAndSelf().  
                    InDocumentOrder().  
                    Select(Function(e) NameWithPredicate(e)).StrCat("/") &  
                        If(pi.Parent.Nodes().OfType(Of XProcessingInstruction)().Count() <> 1,  
                           "processing-instruction()[" &  
                               (pi.NodesBeforeSelf().OfType(Of XProcessingInstruction)().Count() + 1) & "]",  
                           "processing-instruction()")  
            End If  
            Return Nothing  
        End If  
    End Function  
  
    <Extension()>  
    Public Function Find(ByVal source As XElement, ByVal value As String) As IEnumerable(Of XObject)  
        Dim results = From att In source.Attributes()  
                      Where att.Value.Contains(value)  
                      Let a As XObject = att  
                      Select a  
  
        If source.Elements().Any Then  
            For Each result In From child In source.Elements() Select Find(child, value)  
                results = If(results Is Nothing, result, results.Union(result))  
            Next  
        Else  
            If source.Value.Contains(value) Then  
                results = If(results Is Nothing,  
                             New List(Of XObject) From {source},  
                             results.Union(New List(Of XObject) From {source}))  
            End If  
        End If  
  
        Return results  
    End Function  
  
End Module  
```  
  
 Esse código gera a seguinte saída:  
  
```console  
/PurchaseOrders/PurchaseOrder[1]/@OrderDate  
1999-10-20  
/PurchaseOrders/PurchaseOrder[1]/Items/Item[2]/ShipDate  
1999-05-21  
/PurchaseOrders/PurchaseOrder[2]/@OrderDate  
1999-10-22  
/PurchaseOrders/PurchaseOrder[3]/@OrderDate  
1999-10-22  
```  
  
## <a name="see-also"></a>Confira também

- [Técnicas de consulta avançada (LINQ to XML) (Visual Basic)](advanced-query-techniques-linq-to-xml.md)
