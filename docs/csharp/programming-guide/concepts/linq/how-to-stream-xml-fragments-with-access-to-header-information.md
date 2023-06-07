---
title: Como transmitir fragmentos XML com acesso a informações de cabeçalho (C#)
description: Saiba como transmitir fragmentos XML com acesso a informações de cabeçalho. As técnicas de streaming ajudam a evitar o uso excessivo de memória.
ms.date: 07/20/2015
ms.assetid: 7f242770-b0c7-418d-894b-643215e1f8aa
ms.openlocfilehash: 8bfded96ea1fa6b096d56ae0736002b9062d58b6
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303212"
---
# <a name="how-to-stream-xml-fragments-with-access-to-header-information-c"></a>Como transmitir fragmentos XML com acesso a informações de cabeçalho (C#)
Às vezes você precisará ler arbitrariamente grandes arquivos XML, e escreve seu aplicativo para que os vestígio de memória do aplicativo seja previsível. Se você tentar preencher uma árvore XML com um grande arquivo XML, seu uso de memória será proporcionalmente o tamanho do arquivo que é, excessivo. Portanto, você deve usar uma técnica de streaming em vez disso.  
  
Uma opção é escrever seu aplicativo usando <xref:System.Xml.XmlReader>. No entanto, talvez você queira usar o LINQ para consultar a árvore XML. Se esse for o caso, você pode escrever seu próprio método personalizado do eixo. Para obter mais informações, consulte [como escrever um método de eixo de LINQ to XML (C#)](./how-to-write-a-linq-to-xml-axis-method.md).
  
 Para escrever seu próprio método do eixo, você escreve um pequeno método que usa <xref:System.Xml.XmlReader> para ler nós até que atingiu um dos nós em que você está interessado. O método chama em <xref:System.Xml.Linq.XNode.ReadFrom%2A>, que lê de <xref:System.Xml.XmlReader> e cria uma instância de um fragmento XML. Resulta em cada fragmento com `yield return` o método que está enumerando o método personalizado do eixo. Você pode escrever consultas LINQ no método personalizado do eixo.  
  
 As técnicas de streaming são melhor aplicadas em situações onde você precisa processar o documento de origem apenas uma vez e você pode processar os elementos na ordem do documento. Determinados operadores de consulta padrão, como <xref:System.Linq.Enumerable.OrderBy%2A>, iteram sua origem, coletam todos os dados, classificam e, em seguida, geram finalmente o primeiro item na sequência. Se você usar um operador de consulta que materializa sua fonte antes de gerar o primeiro item, você não manterá uma pequena superfície de memória.  
  
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
  
 Essa abordagem tem uma pegada pequena de memória. Como cada fragmento XML de detalhe é rendido, nenhuma referência é mantido para o fragmento anterior, e está disponível para coleta de lixo. Essa técnica cria muitos objetos de vida curta no heap.  
  
 O exemplo a seguir mostra como implementar e usar um método personalizado do eixo que passa fragmentos XML do arquivo especificado por um URI. Esse eixo personalizado é escrito de modo que ele espera um documento que `Customer` tem `Name` elementos, e `Item` , e que esses elementos serão organizados como no `Source.xml` documento acima. É uma implementação simplista. Uma implementação mais robusta seria preparada para analisar um documento válido.  
  
```csharp  
static IEnumerable<XElement> StreamCustomerItem(string uri)  
{  
    using (XmlReader reader = XmlReader.Create(uri))  
    {  
        XElement name = null;  
        XElement item = null;  
  
        reader.MoveToContent();  
  
        // Parse the file, save header information when encountered, and yield the  
        // Item XElement objects as they are created.  
  
        // loop through Customer elements  
        while (reader.Read())  
        {  
            if (reader.NodeType == XmlNodeType.Element  
                && reader.Name == "Customer")  
            {  
                // move to Name element  
                while (reader.Read())  
                {  
                    if (reader.NodeType == XmlNodeType.Element &&  
                        reader.Name == "Name")  
                    {  
                        name = XElement.ReadFrom(reader) as XElement;  
                        break;  
                    }  
                }  
  
                // loop through Item elements  
                while (reader.Read())  
                {  
                    if (reader.NodeType == XmlNodeType.EndElement)  
                        break;  
                    if (reader.NodeType == XmlNodeType.Element  
                        && reader.Name == "Item")  
                    {  
                        item = XElement.ReadFrom(reader) as XElement;  
                        if (item != null) {  
                            XElement tempRoot = new XElement("Root",  
                                new XElement(name)  
                            );  
                            tempRoot.Add(item);  
                            yield return item;  
                        }  
                    }  
                }  
            }  
        }  
    }  
}  
  
static void Main(string[] args)  
{  
    XElement xmlTree = new XElement("Root",  
        from el in StreamCustomerItem("Source.xml")  
        where (int)el.Element("Key") >= 3 && (int)el.Element("Key") <= 7  
        select new XElement("Item",  
            new XElement("Customer", (string)el.Parent.Element("Name")),  
            new XElement(el.Element("Key"))  
        )  
    );  
    Console.WriteLine(xmlTree);  
}  
```  
  
 Este código produz a seguinte saída:  
  
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
