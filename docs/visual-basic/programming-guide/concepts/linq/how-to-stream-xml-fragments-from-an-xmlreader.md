---
title: 'Como: transmitir fragmentos XML de um XmlReader'
ms.date: 07/20/2015
ms.assetid: f67ce598-4a12-4dcb-9a07-24deca02a111
ms.openlocfilehash: ff22625767c4e0752ca19d5a315395934b566230
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397694"
---
# <a name="how-to-stream-xml-fragments-from-an-xmlreader-visual-basic"></a>Como: transmitir fragmentos XML de um XmlReader (Visual Basic)
Quando você tem que processa grandes arquivos XML, talvez não seja possível carregar a árvore inteira XML na memória. Este tópico mostra como passar informações usando <xref:System.Xml.XmlReader>.  
  
 Um dos modos de efetivas usar <xref:System.Xml.XmlReader> para ler objetos de <xref:System.Xml.Linq.XElement> é escrever seu próprio método personalizado do eixo. Um método do eixo normalmente retorna uma coleção como <xref:System.Collections.Generic.IEnumerable%601> de <xref:System.Xml.Linq.XElement>, conforme mostrado no exemplo neste tópico. No método personalizado do eixo, depois de criar o fragmento XML chamando o método <xref:System.Xml.Linq.XNode.ReadFrom%2A> , retornar a coleção usando `yield return`. Isso fornece a semântica de execução adiada ao método personalizado do eixo.  
  
 Quando você cria uma árvore XML de um objeto de <xref:System.Xml.XmlReader> , <xref:System.Xml.XmlReader> deve ser posicionado em um elemento. O método de <xref:System.Xml.Linq.XNode.ReadFrom%2A> não retorna até que lê a marca do elemento.  
  
 Se você desejar criar uma árvore parcial, você pode criar uma instância <xref:System.Xml.XmlReader>, posiciona o leitor no nó que você deseja converter a <xref:System.Xml.Linq.XElement> uma árvore e em seguida, cria o objeto de <xref:System.Xml.Linq.XElement> .  
  
 O tópico [como transmitir fragmentos XML com acesso a informações de cabeçalho (Visual Basic)](how-to-stream-xml-fragments-with-access-to-header-information.md) contém informações e um exemplo de como transmitir um documento mais complexo.  
  
 O tópico [como executar a transformação de streaming de documentos XML grandes (Visual Basic)](how-to-perform-streaming-transform-of-large-xml-documents.md) contém um exemplo de como usar LINQ to XML para transformar documentos XML extremamente grandes enquanto mantém uma pequena superfície de memória.  
  
## <a name="example"></a>Exemplo  
 Este exemplo cria um método personalizado do eixo. Você pode consultá-lo usando uma consulta LINQ. O método de eixo personalizado `StreamRootChildDoc` é um método que foi projetado especificamente para ler um documento que tenha um elemento `Child` de repetição.  
  
```vb  
Module Module1  
    Sub Main()  
        Dim markup = "<Root>" &  
                     "  <Child Key=""01"">" &  
                     "    <GrandChild>aaa</GrandChild>" &  
                     "  </Child>" &  
                     "  <Child Key=""02"">" &  
                     "    <GrandChild>bbb</GrandChild>" &  
                     "  </Child>" &  
                     "  <Child Key=""03"">" &  
                     "    <GrandChild>ccc</GrandChild>" &  
                     "  </Child>" &  
                     "</Root>"  
  
        Dim grandChildData =  
             From el In New StreamRootChildDoc(New IO.StringReader(markup))  
             Where CInt(el.@Key) > 1  
             Select el.<GrandChild>.Value  
  
        For Each s In grandChildData  
            Console.WriteLine(s)  
        Next  
    End Sub  
End Module  
  
Public Class StreamRootChildDoc  
    Implements IEnumerable(Of XElement)  
  
    Private _stringReader As IO.StringReader  
  
    Public Sub New(ByVal stringReader As IO.StringReader)  
        _stringReader = stringReader  
    End Sub  
  
    Public Function GetEnumerator() As IEnumerator(Of XElement) Implements IEnumerable(Of XElement).GetEnumerator  
        Return New StreamChildEnumerator(_stringReader)  
    End Function  
  
    Public Function GetEnumerator1() As IEnumerator Implements IEnumerable.GetEnumerator  
        Return Me.GetEnumerator()  
    End Function  
End Class  
  
Public Class StreamChildEnumerator  
    Implements IEnumerator(Of XElement)  
  
    Private _current As XElement  
    Private _reader As Xml.XmlReader  
    Private _stringReader As IO.StringReader  
  
    Public Sub New(ByVal stringReader As IO.StringReader)  
        _stringReader = stringReader  
        _reader = Xml.XmlReader.Create(_stringReader)  
        _reader.MoveToContent()  
    End Sub  
  
    Public ReadOnly Property Current As XElement Implements IEnumerator(Of XElement).Current  
        Get  
            Return _current  
        End Get  
    End Property  
  
    Public ReadOnly Property Current1 As Object Implements IEnumerator.Current  
        Get  
            Return Me.Current  
        End Get  
    End Property  
  
    Public Function MoveNext() As Boolean Implements IEnumerator.MoveNext  
        While _reader.Read()  
            Select Case _reader.NodeType  
                Case Xml.XmlNodeType.Element  
                    Dim el = TryCast(XElement.ReadFrom(_reader), XElement)  
                    If el IsNot Nothing Then  
                        _current = el  
                        Return True  
                    End If  
            End Select  
        End While  
  
        Return False  
    End Function  
  
    Public Sub Reset() Implements IEnumerator.Reset  
        _reader = Xml.XmlReader.Create(_stringReader)  
        _reader.MoveToContent()  
    End Sub  
  
#Region "IDisposable Support"  
    Private disposedValue As Boolean ' To detect redundant calls  
  
    ' IDisposable  
    Protected Overridable Sub Dispose(ByVal disposing As Boolean)  
        If Not Me.disposedValue Then  
            If disposing Then  
                _reader.Close()  
            End If  
        End If  
        Me.disposedValue = True  
    End Sub  
  
    Public Sub Dispose() Implements IDisposable.Dispose  
        Dispose(True)  
        GC.SuppressFinalize(Me)  
    End Sub  
#End Region  
  
End Class  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```console  
bbb  
ccc  
```  
  
 Nesse exemplo, o documento de origem é muito pequeno. No entanto, mesmo se houver milhões de elementos de `Child` , este exemplo ainda terá uma pegada pequena de memória.  
  
## <a name="see-also"></a>Confira também

- [Instruções passo a passo: implementando IEnumerable(Of T) no Visual Basic](../../language-features/control-flow/walkthrough-implementing-ienumerable-of-t.md)
- [Analisando XML (Visual Basic)](parsing-xml.md)
