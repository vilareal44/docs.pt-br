---
title: Weakly-typed JSON Serialization Sample
ms.date: 03/30/2017
ms.assetid: 0b30e501-4ef5-474d-9fad-a9d559cf9c52
ms.openlocfilehash: a503878f1cbb60090b648da8dfec741edbf02d1b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602317"
---
# <a name="weakly-typed-json-serialization-sample"></a>Weakly-typed JSON Serialization Sample
Ao serializar um tipo definido pelo usuário para um determinado formato de conexão ou desserializar um formato de conexão de volta para um tipo definido pelo usuário, o tipo definido pelo usuário fornecido deve estar disponível no serviço e no cliente. Normalmente, para fazer isso, o <xref:System.Runtime.Serialization.DataContractAttribute> atributo é aplicado a esses tipos definidos pelo usuário e o <xref:System.Runtime.Serialization.DataMemberAttribute> atributo é aplicado aos seus membros. Esse mecanismo também se aplica ao trabalhar com objetos JavaScript Object Notation (JSON), conforme descrito no tópico [como serializar e desserializar dados JSON](../feature-details/how-to-serialize-and-deserialize-json-data.md).  
  
 Em alguns cenários, um serviço ou cliente do Windows Communication Foundation (WCF) deve acessar objetos JSON gerados por um serviço ou cliente que está fora do controle do desenvolvedor. À medida que mais serviços Web expõem publicamente APIs JSON, pode se tornar impraticável para o desenvolvedor do WCF construir tipos locais definidos pelo usuário para desserializar objetos JSON arbitrários. Este exemplo fornece um mecanismo que permite que os desenvolvedores do WCF trabalhem com objetos JSON desserializados e arbitrários, sem criar tipos definidos pelo usuário. Isso é conhecido como *serialização de tipo fraco* de objetos JSON, pois o tipo no qual um objeto JSON é desserializado não é conhecido no momento da compilação.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Por exemplo, uma API de serviço Web pública retorna o seguinte objeto JSON, que descreve algumas informações sobre um usuário do serviço.  
  
```json  
{"personal": {"name": "Paul", "age": 23, "height": 1.7, "isSingle": true, "luckyNumbers": [5,17,21]}, "favoriteBands": ["Band ABC", "Band XYZ"]}  
```  
  
 Para desserializar esse objeto, um cliente WCF deve implementar os seguintes tipos definidos pelo usuário.  
  
```csharp  
[DataContract]  
 public class MemberProfile  
 {  
     [DataMember]  
     public PersonalInfo personal;  
  
     [DataMember]  
     public string[] favoriteBands;  
 }  
  
 [DataContract]  
 public class PersonalInfo  
 {  
     [DataMember]  
     public string name;  
  
     [DataMember]  
     public int age;  
  
     [DataMember]  
     public double height;  
  
     [DataMember]  
     public bool isSingle;  
  
     [DataMember]  
     public int[] luckyNumbers;  
 }  
```  
  
 Isso pode ser complicado, especialmente se o cliente tiver que lidar com mais de um tipo de objeto JSON.  
  
 O `JsonObject` tipo fornecido por este exemplo apresenta uma representação de tipo fraco do objeto JSON desserializado. `JsonObject`o depende do mapeamento natural entre objetos JSON e dicionários de .NET Framework e o mapeamento entre matrizes JSON e matrizes de .NET Framework. O código a seguir mostra o `JsonObject` tipo.  
  
```csharp  
// Instantiation of JsonObject json omitted  
  
string name = json["root"]["personal"]["name"];  
int age = json["root"]["personal"]["age"];  
double height = json["root"]["personal"]["height"];  
bool isSingle = json["root"]["personal"]["isSingle"];  
int[] luckyNumbers = {  
                                     json["root"]["personal"]["luckyNumbers"][0],  
                                     json["root"]["personal"]["luckyNumbers"][1],  
                                     json["root"]["personal"]["luckyNumbers"][2]
                                 };  
string[] favoriteBands = {  
                                        json["root"]["favoriteBands"][0],  
                                        json["root"]["favoriteBands"][1]  
                                    };  
```  
  
 Observe que você pode "procurar" objetos JSON e matrizes sem a necessidade de declarar seu tipo no momento da compilação. Para obter uma explicação sobre o requisito para o objeto de nível superior `["root"]` , consulte o tópico [mapeamento entre JSON e XML](../feature-details/mapping-between-json-and-xml.md).  
  
> [!NOTE]
> A `JsonObject` classe é fornecida apenas como exemplo. Ele não foi totalmente testado e não deve ser usado em ambientes de produção. Uma implicação óbvia de serialização JSON de tipo fraco é a falta de segurança de tipos ao trabalhar com o `JsonObject` .  
  
 Para usar o `JsonObject` tipo, o contrato de operação do cliente deve usar <xref:System.ServiceModel.Channels.Message> como seu tipo de retorno.  
  
```csharp  
[ServiceContract]  
    interface IClientSideProfileService  
    {  
        // There is no need to write a DataContract for the complex type returned by the service.  
        // The client will use a JsonObject to browse the JSON in the received message.  
  
        [OperationContract]  
        [WebGet(ResponseFormat = WebMessageFormat.Json)]  
        Message GetMemberProfile();  
    }  
```  
  
 O `JsonObject` é então instanciado, conforme mostrado no código a seguir.  
  
```csharp  
// Code to instantiate IClientSideProfileService channel omitted…  
  
// Make a request to the service and obtain the Json response  
XmlDictionaryReader reader = channel.GetMemberProfile().GetReaderAtBodyContents();  
  
// Go through the Json as though it is a dictionary. There is no need to map it to a .NET CLR type.  
JsonObject json = new JsonObject(reader);  
```  
  
 O `JsonObject` construtor usa um <xref:System.Xml.XmlDictionaryReader> , que é obtido por meio do <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A> método. O leitor contém uma representação XML da mensagem JSON recebida pelo cliente. Para obter mais informações, consulte o tópico [mapeamento entre JSON e XML](../feature-details/mapping-between-json-and-xml.md).  
  
 O programa produz a seguinte saída:  
  
```console  
Service listening at http://localhost:8000/.  
To view the JSON output from the sample, navigate to http://localhost:8000/GetMemberProfile  
This is Paul's page. I am 23 years old and I am 1.7 meters tall.  
I am single.  
My lucky numbers are 5, 17, and 21.  
My favorite bands are Band ABC and Band XYZ.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Crie a solução WeaklyTypedJson. sln, conforme descrito em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Execute a solução.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\Ajax\WeaklyTypedJson`  
