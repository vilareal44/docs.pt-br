---
title: Como utilizar o Svcutil.exe para exportar metadados de código de serviço compilado
ms.date: 03/30/2017
ms.assetid: 95d0aed3-16a2-4398-89bb-39418eeb7355
ms.openlocfilehash: 9acefdec63a6f518ead6cdbcb19ebc8c75609dd6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595356"
---
# <a name="how-to-use-svcutilexe-to-export-metadata-from-compiled-service-code"></a>Como utilizar o Svcutil.exe para exportar metadados de código de serviço compilado
Svcutil. exe pode exportar metadados para serviços, contratos e tipos de dados em assemblies compilados, da seguinte maneira:  
  
- Para exportar metadados para todos os contratos de serviço compilados para um conjunto de assemblies usando svcutil. exe, especifique os assemblies como parâmetros de entrada. Esse é o comportamento padrão.  
  
- Para exportar metadados para um serviço compilado usando svcutil. exe, especifique o assembly de serviço ou assemblies como parâmetros de entrada. Você deve usar a `/serviceName` opção para indicar o nome da configuração do serviço que deseja exportar. Svcutil. exe carrega automaticamente o arquivo de configuração para o assembly executável especificado.  
  
- Para exportar todos os tipos de contrato de dados dentro de um conjunto de assemblies, use a `/dataContractOnly` opção.  
  
> [!NOTE]
> Use a `/reference` opção para especificar os caminhos de arquivo para quaisquer assemblies dependentes.  
  
### <a name="to-export-metadata-for-compiled-service-contracts"></a>Para exportar metadados para contratos de serviço compilados  
  
1. Compile as implementações do contrato de serviço em uma ou mais bibliotecas de classe. 1  
  
2. Execute svcutil. exe nos assemblies compilados.  
  
    > [!NOTE]
    > Talvez seja necessário usar a `/reference` opção para especificar o caminho do arquivo para quaisquer assemblies dependentes.  
  
    ```console
    svcutil.exe Contracts.dll  
    ```  
  
### <a name="to-export-metadata-for-a-compiled-service"></a>Para exportar metadados para um serviço compilado  
  
1. Compile sua implementação de serviço em um assembly executável.  
  
2. Crie um arquivo de configuração para o executável do serviço e adicione uma configuração de serviço.  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.serviceModel>  
        <services>  
          <service name="MyService" >  
            <endpoint address="finder" contract="IPeopleFinder" binding="wsHttpBinding" />  
          </service>  
        </services>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
3. Execute svcutil. exe no executável do serviço compilado usando a `/serviceName` opção para especificar o nome da configuração do serviço.  
  
    > [!NOTE]
    > Talvez seja necessário usar a `/reference` opção para especificar o caminho do arquivo para quaisquer assemblies dependentes.  
  
    ```console  
    svcutil.exe /serviceName:MyService Service.exe /reference:path/Contracts.dll  
    ```  
  
### <a name="to-export-metadata-for-compiled-data-contracts"></a>Para exportar metadados para contratos de dados compilados  
  
1. Compile suas implementações de contrato de dados em uma ou mais bibliotecas de classes.  
  
2. Execute svcutil. exe nos assemblies compilados usando a `/dataContract` opção para especificar que somente os metadados para contratos de dados devem ser gerados.  
  
    > [!NOTE]
    > Talvez seja necessário usar a `/reference` opção para especificar o caminho do arquivo para quaisquer assemblies dependentes.  
  
    ```console  
    svcutil.exe /dataContractOnly Contracts.dll  
    ```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra como gerar metadados para uma configuração e implementação de serviço simples.  
  
 Para exportar metadados para o contrato de serviço.  
  
```console  
svcutil.exe Contracts.dll  
```  
  
 Para exportar metadados para os contratos de dados.  
  
```console  
svcutil.exe /dataContractOnly Contracts.dll  
```  
  
 Para exportar metadados para a implementação do serviço.  
  
```console  
svcutil.exe /serviceName:MyService Service.exe /reference:<path>/Contracts.dll  
```  
  
 O `<path>` é o caminho para Contracts. dll.  
  
```csharp
// The following service contract and data contracts are compiled into
// Contracts.dll.  
[ServiceContract(ConfigurationName="IPeopleFinder")]  
public interface IPersonFinder  
{  
    [OperationContract]  
    Address GetAddress(Person s);  
}  
  
[DataContract]  
public class Person  
{  
    [DataMember]  
    public string firstName;  
    [DataMember]  
    public string lastName;  
    [DataMember]  
    public int age;  
}  
  
[DataContract]  
public class Address  
{  
    [DataMember]  
    public string city;  
    [DataMember]  
    public string state;  
    [DataMember]  
    public string street;  
    [DataMember]  
    public int zipCode;  
    [DataMember]  
    public Person person;  
}  
```

```csharp
// The following service implementation is compiled into Service.exe.
// This service uses the contracts specified in Contracts.dll.  
[ServiceBehavior(ConfigurationName = "MyService")]  
public class MyService : IPersonFinder  
{  
    public Address GetAddress(Person person)  
    {  
        Address address = new Address();  
        address.person = person;  
        return address;  
    }  
}  
```

```xml  
<!-- The following is the configuration file for Service.exe. -->  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service name="MyService">  
         <endpoint  address="finder"  
                    binding="basicHttpBinding"  
                    contract="IPeopleFinder"/>  
      </service>  
    </services>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
- [Exportando e importando metadados](exporting-and-importing-metadata.md)
