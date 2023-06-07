---
title: Criar um cabeçalho personalizado que é assinado e/ou criptografado
ms.date: 03/30/2017
ms.assetid: e8668b37-c79f-4714-9de5-afcb88b9ff02
ms.openlocfilehash: 0adb4100bca1add2c23ff2c802ddb5e2cb1c368c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84579652"
---
# <a name="creating-a-custom-header-that-is-signed-and-or-encrypted"></a>Criar um cabeçalho personalizado que é assinado e/ou criptografado
Ao chamar um serviço não WCF usando um cliente WCF, às vezes, é necessário usar cabeçalhos SOAP personalizados. Há um bug de canonização no WCF que impede que os cabeçalhos personalizados assinados e criptografados trabalhem com um serviço não WCF. O problema é causado pela canonização incorreta de namespaces XML padrão. Isso só é problemático ao chamar serviços não WCF com cabeçalhos personalizados assinados e/ou criptografados.  Quando o serviço recebe a mensagem que contém o cabeçalho personalizado assinado e/ou criptografado, ele não pode verificar a assinatura. Essa solução alternativa evita o bug de canonização, permite a interoperabilidade com serviços não WCF, mas não impede a interoperabilidade com os serviços WCF.  
  
## <a name="defining-the-custom-header"></a>Definindo o cabeçalho personalizado  
 Os cabeçalhos personalizados são definidos definindo um contrato de mensagem e marcando os membros que você deseja que sejam enviados como cabeçalhos com um <xref:System.ServiceModel.MessageHeaderAttribute> atributo. Para contornar o bug de canonização, você deve garantir que o serializador XML declare o namespace para o cabeçalho personalizado com um prefixo em vez de uma declaração de namespace padrão. O código a seguir mostra como definir o tipo de dados que será usado como um cabeçalho de mensagem com a declaração de namespace correta.  
  
```csharp
[System.CodeDom.Compiler.GeneratedCodeAttribute("svcutil", "3.0.4506.648")]  
[System.SerializableAttribute()]  
[System.Diagnostics.DebuggerStepThroughAttribute()]  
[System.ComponentModel.DesignerCategoryAttribute("code")]  
[System.Xml.Serialization.XmlTypeAttribute(AnonymousType=true, Namespace="http://www.example.org/getMessage/")]  
public partial class msgHeaderElement  
{  
   // Define the XML namespace and force it to use an ‘h’ prefix  
    [System.Xml.Serialization.XmlNamespaceDeclarations]  
    public System.Xml.Serialization.XmlSerializerNamespaces _xsns = new System.Xml.Serialization.XmlSerializerNamespaces(new System.Xml.XmlQualifiedName[] { new System.Xml.XmlQualifiedName("h", "http://www.example.org/getMessage/") });  
  
    private string msgHeaderInputField;  
  [System.Xml.Serialization.XmlElementAttribute(Form=System.Xml.Schema.XmlSchemaForm.Unqualified, Order=0)]  
    public string msgHeaderInput  
    {  
        get  
        {  
            return this.msgHeaderInputField;  
        }  
        set  
        {  
            this.msgHeaderInputField = value;  
        }  
    }  
}  
```  
  
 Esse código declara um novo tipo chamado `msgHeaderElement` que será serializado com o serializador XML. Quando uma instância desse tipo é serializada, ela definirá um namespace com um prefixo ' h ', trabalhando em todo o bug de canonização.  Em seguida, o contrato de mensagem definiria uma instância de `msgHeaderElement` e marcá-la com o <xref:System.ServiceModel.MessageHeaderAttribute> atributo, conforme mostrado no exemplo a seguir.  
  
```csharp
[MessageContract]  
public  class MyMessageContract  
{  
   // other message contents...  
   [MessageHeader(ProductionLevel=ProtectionLevel.EncryptAndSign)]  
   public msgHeaderElement;  
   // other message contents...  
}  
```  
  
## <a name="see-also"></a>Consulte também

- [Contrato padrão de mensagem](../samples/default-message-contract.md)
- [Contratos de mensagem](../samples/message-contracts.md)
- [Utilizando contratos de mensagem](using-message-contracts.md)
