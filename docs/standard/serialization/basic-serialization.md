---
title: Serialização básica
description: Este artigo mostra como tornar uma classe serializável com o SerializableAttribute e inclui exemplos de serialização e desserialização.
ms.date: 03/30/2017
helpviewer_keywords:
- binary serialization, basic serialization
- serialization, basic serialization
ms.assetid: d899d43c-335a-433e-a589-cd187192984f
dev_langs:
- CSharp
ms.openlocfilehash: 98ea6f23467b85dc270aa323e72a8a9b0934994a
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "83378423"
---
# <a name="basic-serialization"></a>Serialização básica

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

A maneira mais fácil de tornar uma classe serializável é marcá-la com o atributo <xref:System.SerializableAttribute>, conforme mostrado a seguir.  
  
```csharp  
[Serializable]  
public class MyObject {  
  public int n1 = 0;  
  public int n2 = 0;  
  public String str = null;  
}  
```  
  
O exemplo de código a seguir mostra como uma instância dessa classe pode ser serializada para um arquivo.  
  
```csharp  
MyObject obj = new MyObject();  
obj.n1 = 1;  
obj.n2 = 24;  
obj.str = "Some String";  
IFormatter formatter = new BinaryFormatter();  
Stream stream = new FileStream("MyFile.bin", FileMode.Create, FileAccess.Write, FileShare.None);  
formatter.Serialize(stream, obj);  
stream.Close();  
```  
  
Este exemplo usa um formatador binário para fazer a serialização. Basta criar uma instância do fluxo e do formatador que você pretende usar e, em seguida, chamar o método **Serialize** no formatador. O fluxo e o objeto a serem serializados são fornecidos como parâmetros para essa chamada. Embora não seja explicitamente demonstrado neste exemplo, todas as variáveis de membro de uma classe serão serializadas — mesmo variáveis marcadas como particulares. Neste aspecto, a serialização binária é diferente da classe <xref:System.Xml.Serialization.XmlSerializer>, que serializa somente campos públicos. Para obter informações sobre como excluir variáveis de membro da serialização binária, consulte [Serialização seletiva](selective-serialization.md).  
  
A restauração do objeto de volta para o estado anterior também é fácil. Primeiro, crie um fluxo para leitura e um <xref:System.Runtime.Serialization.Formatter> e, em seguida, instrua o formatador a desserializar o objeto. O exemplo de código a seguir mostra como isso é feito.  
  
```csharp  
IFormatter formatter = new BinaryFormatter();  
Stream stream = new FileStream("MyFile.bin", FileMode.Open, FileAccess.Read, FileShare.Read);  
MyObject obj = (MyObject) formatter.Deserialize(stream);  
stream.Close();  
  
// Here's the proof.  
Console.WriteLine("n1: {0}", obj.n1);  
Console.WriteLine("n2: {0}", obj.n2);  
Console.WriteLine("str: {0}", obj.str);  
```  
  
O <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> usado acima é muito eficiente e produz um fluxo de bytes compacto. Todos os objetos serializados com esse formatador também podem ser desserializados com ele, o que o torna uma ferramenta ideal para serializar objetos que serão desserializados no .NET Framework. É importante observar que os construtores não são chamados quando um objeto é desserializado. Essa restrição é colocada na desserialização por razões de desempenho. No entanto, isso viola alguns dos contratos normais que o runtime cria com o gravador de objeto, e os desenvolvedores devem compreender as ramificações ao marcar um objeto como serializável.  
  
Se a portabilidade for um requisito, use o <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> em vez disso. Basta substituir o **BinaryFormatter** no código acima por **SoapFormatter** e chamar **Serialize** e **Deserialize** como antes. Esse formatador produz a seguinte saída para o exemplo usado acima.  
  
```xml  
<SOAP-ENV:Envelope  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"  
  xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"  
  SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"  
  xmlns:a1="http://schemas.microsoft.com/clr/assem/ToFile">  
  
  <SOAP-ENV:Body>  
    <a1:MyObject id="ref-1">  
      <n1>1</n1>  
      <n2>24</n2>  
      <str id="ref-3">Some String</str>  
    </a1:MyObject>  
  </SOAP-ENV:Body>  
</SOAP-ENV:Envelope>  
```  
  
É importante observar que o atributo [Serializable](xref:System.SerializableAttribute) não pode ser herdado. Se você derivar uma nova classe `MyObject`, a nova classe também deverá ser marcada com o atributo ou não poderá ser serializada. Por exemplo, ao tentar serializar uma instância da classe abaixo, você obterá uma <xref:System.Runtime.Serialization.SerializationException> informando que o tipo `MyStuff` não está marcado como serializável.  
  
```csharp  
public class MyStuff : MyObject
{  
  public int n3;  
}  
```  
  
 O uso do atributo [Serializable](xref:System.SerializableAttribute) é conveniente, mas tem limitações, conforme demonstrado anteriormente. Consulte as [Diretrizes de serialização](serialization-guidelines.md) para obter informações sobre quando é necessário marcar uma classe para serialização. A serialização não pode ser adicionada a uma classe depois que ela foi compilada.  
  
## <a name="see-also"></a>Confira também

- [Serialização binária](binary-serialization.md)
- [Serialização de XML e SOAP](xml-and-soap-serialization.md)
