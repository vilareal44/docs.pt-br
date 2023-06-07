---
title: 'Como: transmitir fragmentos XML com acesso a informações de cabeçalho'
ms.date: 07/20/2015
ms.assetid: effd10df-87c4-4d7a-8a9a-1434d829dca5
ms.openlocfilehash: 8d0543ff5a772768292c0e3117f41bea9367d23a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397681"
---
# <a name="how-to-stream-xml-fragments-with-access-to-header-information-visual-basic"></a>Como: transmitir fragmentos XML com acesso a informações de cabeçalho (Visual Basic)
Às vezes você precisará ler arbitrariamente grandes arquivos XML, e escreve seu aplicativo para que os vestígio de memória do aplicativo seja previsível. Se você tentar preencher uma árvore XML com um grande arquivo XML, seu uso de memória será proporcionalmente o tamanho do arquivo que é, excessivo. Portanto, você deve usar uma técnica de streaming em vez disso.  
  
 Uma opção é escrever seu aplicativo usando <xref:System.Xml.XmlReader>. No entanto, talvez você queira usar o LINQ para consultar a árvore XML. Se esse for o caso, você pode escrever seu próprio método personalizado do eixo. Para obter mais informações, consulte [como escrever um método de eixo de LINQ to XML (Visual Basic)](how-to-write-a-linq-to-xml-axis-method.md).  
  
 Para escrever seu próprio método do eixo, você escreve um pequeno método que usa <xref:System.Xml.XmlReader> para ler nós até que atingiu um dos nós em que você está interessado. O método chama em <xref:System.Xml.Linq.XNode.ReadFrom%2A>, que lê de <xref:System.Xml.XmlReader> e cria uma instância de um fragmento XML. Você pode escrever consultas LINQ no método personalizado do eixo.  
  
 As técnicas de streaming são melhor aplicadas em situações onde você precisa processar o documento de origem apenas uma vez e você pode processar os elementos na ordem do documento. Determinados operadores de consulta padrão, como <xref:System.Linq.Enumerable.OrderBy%2A>, iteram sua origem, coletam todos os dados, classificam e, em seguida, geram finalmente o primeiro item na sequência. Observe que se você usar um operador de consulta que materialize sua origem antes de como o primeiro item, você não manterá pegada uma pequena de memória.  
  
## <a name="example"></a>Exemplo  
 Às vezes o problema é apenas um pouco mais interessante. No documento XML a seguir, o consumidor do método personalizado do eixo também precisa saber o nome do cliente que cada item pertence.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<Root>  
  <Customer>  
    <Name>A. Datum Corporation</Name>  
    <Item>  
      <Key>0001</Key>  
    </Item>  
    <Item>  
      <Key>0002</Key>  
    </Item>  
    <Item>  
      <Key>0003</Key>  
    </Item>  
    <Item>  
      <Key>0004</Key>  
    </Item>  
  </Customer>  
  <Customer>  
    <Name>Fabrikam, Inc.</Name>  
    <Item>  
      <Key>0005</Key>  
    </Item>  
    <Item>  
      <Key>0006</Key>  
    </Item>  
    <Item>  
      <Key>0007</Key>  
    </Item>  
    <Item>  
      <Key>0008</Key>  
    </Item>  
  </Customer>  
  <Customer>  
    <Name>Southridge Video</Name>  
    <Item>  
      <Key>0009</Key>  
    </Item>  
    <Item>  
      <Key>0010</Key>  
    </Item>  
  </Customer>  
</Root>  
```  
  
 A abordagem que este exemplo usa é inspecione também para essas informações de cabeçalho, salvar informações de cabeçalho, e então cria uma árvore XML pequena que contém informações de cabeçalho e o detalhe que você está enumerando. O método do eixo resulta nessa nova árvore, pequena XML. A consulta possui o acesso a informações de cabeçalho bem como para informações detalhadas.  
  
 Essa abordagem tem uma pegada pequena de memória. Como cada fragmento XML de detalhe é rendido, nenhuma referência é mantido para o fragmento anterior, e está disponível para coleta de lixo. Observe que essa técnica cria uma breve muitos objetos na heap.  
  
 O exemplo a seguir mostra como implementar e usar um método personalizado do eixo que passa fragmentos XML do arquivo especificado por um URI. Esse eixo personalizado é escrito especificamente para que espera um documento que tem `Customer`, `Name`, e os elementos de `Item` , e que esses elementos serão organizados como no documento anterior de `Source.xml` . É uma implementação simplista. Uma implementação mais robusta seria preparada para analisar um documento válido.  
  
```vb  
Module Module1  
  
    Sub Main()  
        Dim xmlTree = <Root>  
                          <%=  
                              From el In New StreamCustomerItem("Source.xml")  
                              Let itemKey = CInt(el.<Key>.Value)  
                              Where itemKey >= 3 AndAlso itemKey <= 7  
                              Select <Item>  
                                         <Customer><%= el.Parent.<Name>.Value %></Customer>  
                                         <%= el.<Key> %>  
                                     </Item>  
                          %>  
                      </Root>  
  
        Console.WriteLine(xmlTree)  
    End Sub  
  
End Module  
  
Public Class StreamCustomerItem  
    Implements IEnumerable(Of XElement)  
  
    Private _uri As String  
  
    Public Sub New(ByVal uri As String)  
        _uri = uri  
    End Sub  
  
    Public Function GetEnumerator() As IEnumerator(Of XElement) Implements IEnumerable(Of XElement).GetEnumerator  
        Return New StreamCustomerItemEnumerator(_uri)  
    End Function  
  
    Public Function GetEnumerator1() As IEnumerator Implements IEnumerable.GetEnumerator  
        Return Me.GetEnumerator()  
    End Function  
End Class  
  
Public Class StreamCustomerItemEnumerator  
    Implements IEnumerator(Of XElement)  
  
    Private _current As XElement  
    Private _customerName As String  
    Private _reader As Xml.XmlReader  
    Private _uri As String  
  
    Public Sub New(ByVal uri As String)  
        _uri = uri  
        _reader = Xml.XmlReader.Create(_uri)  
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
        Dim item As XElement  
        Dim name As XElement  
  
        ' Parse the file, save header information when encountered, and return the  
        ' current Item XElement.  
  
        ' loop through Customer elements  
        While _reader.Read()  
            If _reader.NodeType = Xml.XmlNodeType.Element Then  
                Select Case _reader.Name  
                    Case "Customer"  
                        ' move to Name element  
                        While _reader.Read()  
  
                            If _reader.NodeType = Xml.XmlNodeType.Element AndAlso  
                                _reader.Name = "Name" Then  
  
                                name = TryCast(XElement.ReadFrom(_reader), XElement)  
                                _customerName = If(name IsNot Nothing, name.Value, "")  
                                Exit While  
                            End If  
  
                        End While  
                    Case "Item"  
                        item = TryCast(XElement.ReadFrom(_reader), XElement)  
                        Dim tempRoot = <Root>  
                                           <Name><%= _customerName %></Name>  
                                           <%= item %>  
                                       </Root>  
                        _current = item  
                        Return True  
                End Select  
            End If  
        End While  
  
        Return False  
    End Function  
  
    Public Sub Reset() Implements IEnumerator.Reset  
        _reader = Xml.XmlReader.Create(_uri)  
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
  
 Esse código gera a seguinte saída:  
  
```xml  
<Root>  
  <Item>  
    <Customer>A. Datum Corporation</Customer>  
    <Key>0003</Key>  
  </Item>  
  <Item>  
    <Customer>A. Datum Corporation</Customer>  
    <Key>0004</Key>  
  </Item>  
  <Item>  
    <Customer>Fabrikam, Inc.</Customer>  
    <Key>0005</Key>  
  </Item>  
  <Item>  
    <Customer>Fabrikam, Inc.</Customer>  
    <Key>0006</Key>  
  </Item>  
  <Item>  
    <Customer>Fabrikam, Inc.</Customer>  
    <Key>0007</Key>  
  </Item>  
</Root>  
```  
  
## <a name="see-also"></a>Confira também

- [Programação de LINQ to XML avançada (Visual Basic)](advanced-linq-to-xml-programming.md)
