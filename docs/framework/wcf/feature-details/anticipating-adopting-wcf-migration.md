---
title: 'Antecipando a adoção do Windows Communication Foundation: facilitando a migração futura'
ms.date: 03/30/2017
ms.assetid: f49664d9-e9e0-425c-a259-93f0a569d01b
ms.openlocfilehash: b43f509bd49ebe89d7ed0be4c37b3ed179aaeb8c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84576493"
---
# <a name="anticipating-adopting-the-windows-communication-foundation-easing-future-migration"></a>Antecipando a adoção do Windows Communication Foundation: facilitando a migração futura
Para garantir uma migração futura mais fácil de novos aplicativos ASP.NET para o WCF, siga as recomendações anteriores, bem como as recomendações a seguir.  
  
## <a name="protocols"></a>Protocolos  
 Desabilite o suporte do ASP.NET 2.0 para SOAP 1,2:  
  
```xml  
<configuration>  
     <system.web>  
      <webServices >  
          <protocols>  
           <remove name="HttpSoap12"/>  
          </protocols>
      </webServices>  
     </system.web>
</configuration>  
```  
  
 Isso é aconselhável porque o WCF requer mensagens em conformidade com protocolos diferentes, como SOAP 1,1 e SOAP 1,2, para usar pontos de extremidade diferentes. Se um serviço Web ASP.NET 2,0 estiver configurado para dar suporte a SOAP 1,1 e SOAP 1,2, que é a configuração padrão, ele não poderá ser migrado para um único ponto de extremidade do WCF no endereço original que seria certamente compatível com todos os clientes existentes do serviço Web do ASP.NET. Além disso, escolher SOAP 1,2 em vez de 1,1 restringirá de forma mais grave o clientele do serviço.  
  
## <a name="service-development"></a>Desenvolvimento do serviço  
 O WCF permite que você defina contratos de serviço aplicando- <xref:System.ServiceModel.ServiceContractAttribute> se a interfaces ou a classes. É recomendável aplicar o atributo a uma interface em vez de a uma classe, pois isso cria uma definição de um contrato que pode ser implementado de forma implementada por qualquer número de classes. O ASP.NET 2,0 dá suporte à opção de aplicar o <xref:System.Web.Services.WebService> atributo às interfaces, bem como às classes. No entanto, como já mencionado, há um defeito no ASP.NET 2,0, pelo qual o parâmetro namespace do <xref:System.Web.Services.WebService> atributo não tem efeito quando esse atributo é aplicado a uma interface em vez de uma classe. Como é geralmente aconselhável modificar o namespace de um serviço a partir do valor padrão, `http://tempuri.org` usando o parâmetro namespace do <xref:System.Web.Services.WebService> atributo, é necessário continuar definindo os serviços Web ASP.NET aplicando o <xref:System.ServiceModel.ServiceContractAttribute> atributo a interfaces ou a classes.  
  
- Ter o mínimo de código possível nos métodos pelos quais essas interfaces são definidas. Faça com que eles deleguem seu trabalho para outras classes. Os novos tipos de serviço WCF também poderiam delegar seu trabalho de substando a essas classes.  
  
- Forneça nomes explícitos para as operações de um serviço usando o `MessageName` parâmetro do <xref:System.Web.Services.WebMethodAttribute> .  
  
    ```csharp  
    [WebMethod(MessageName="ExplicitName")]  
    string Echo(string input);  
    ```  
  
     Fazer isso é importante, porque os nomes padrão para operações em ASP.NET são diferentes dos nomes padrão fornecidos pelo WCF. Ao fornecer nomes explícitos, você evita depender dos padrões.  
  
- Não implemente operações de serviço Web ASP.NET com métodos polimórficos, porque o WCF não oferece suporte à implementação de operações com métodos polimórficos.  
  
- Use o <xref:System.Web.Services.Protocols.SoapDocumentMethodAttribute> para fornecer valores explícitos para os cabeçalhos HTTP SoapAction pelos quais as solicitações HTTP serão roteadas para os métodos.  
  
    ```csharp  
    [WebMethod]  
    [SoapDocumentMethod(RequestElementName="ExplicitAction")]  
    string Echo(string input);  
    ```  
  
     Assumir essa abordagem evitará ter que contar com os valores SOAPaction padrão usados pelo ASP.NET e pelo WCF sendo o mesmo.  
  
- Evite usar extensões SOAP. Se as extensões SOAP forem necessárias, determine se a finalidade para a qual elas estão sendo consideradas é um recurso que já é fornecido pelo WCF. Se isso for realmente o caso, reconsidere a opção de não adotar o WCF imediatamente.  
  
## <a name="state-management"></a>Gerenciamento do estado  
 Evite ter de manter o estado em serviços. Não apenas manter o Estado tende a comprometer a escalabilidade de um aplicativo, mas os mecanismos de gerenciamento de estado do ASP.NET e do WCF são muito diferentes, embora o WCF ofereça suporte aos mecanismos ASP.NET no modo de compatibilidade ASP.NET.  
  
## <a name="exception-handling"></a>Tratamento de Exceção  
 Ao criar as estruturas dos tipos de dados a serem enviados e recebidos por um serviço, também crie estruturas para representar os vários tipos de exceções que podem ocorrer dentro de um serviço que talvez queira transmitir a um cliente.  
  
```csharp  
[Serializable]  
[XmlRoot(Namespace="ExplicitNamespace", IsNullable=true)]  
public partial class AnticipatedException
{
    private string anticipatedExceptionInformationField;  

    public string AnticipatedExceptionInformation
    {  
        get {
            return this.anticipatedExceptionInformationField;  
        }  
        set {  
            this.anticipatedExceptionInformationField = value;  
        }  
    }  
}  
```  
  
 Dê a essas classes a capacidade de se serializar em XML:  
  
```csharp  
public XmlNode ToXML()  
{  
     XmlSerializer serializer = new XmlSerializer(  
      typeof(AnticipatedException));  
     MemoryStream memoryStream = new MemoryStream();  
     XmlTextWriter writer = new XmlTextWriter(  
     memoryStream, UnicodeEncoding.UTF8);  
     serializer.Serialize(writer, this);  
     XmlDocument document = new XmlDocument();  
     document.LoadXml(new string(  
     UnicodeEncoding.UTF8.GetChars(  
     memoryStream.GetBuffer())).Trim());  
    return document.DocumentElement;  
}  
```  
  
 As classes podem então ser usadas para fornecer os detalhes para instâncias explicitamente geradas <xref:System.Web.Services.Protocols.SoapException> :  
  
```csharp  
AnticipatedException exception = new AnticipatedException();  
exception.AnticipatedExceptionInformation = "…";  
throw new SoapException(  
     "Fault occurred",  
     SoapException.ClientFaultCode,  
     Context.Request.Url.AbsoluteUri,  
     exception.ToXML());  
```  
  
 Essas classes de exceção serão prontamente reutilizáveis com a <xref:System.ServiceModel.FaultException%601> classe WCF para gerar um novo`FaultException<AnticipatedException>(anticipatedException);`  
  
## <a name="security"></a>Segurança  
 A seguir estão algumas recomendações de segurança.  
  
- Evite usar perfis do ASP.NET 2,0, pois usá-los restringiria o uso do modo de integração do ASP.NET se o serviço foi migrado para o WCF.  
  
- Evite usar ACLs para controlar o acesso aos serviços, pois os serviços Web do ASP.NET dão suporte a ACLs usando o Serviços de Informações da Internet (IIS), o WCF não, porque os serviços Web do ASP.NET dependem do IIS para hospedagem, e o WCF não precisa ser hospedado no IIS.  
  
- Considere o uso de provedores de função do ASP.NET 2,0 para autorizar o acesso aos recursos de um serviço.  
  
## <a name="see-also"></a>Consulte também

- [Antecipando a adoção do Windows Communication Foundation: facilitando a futura integração](anticipating-adopting-the-wcf-easing-future-integration.md)
