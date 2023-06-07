---
title: Usando o WCF Moniker com clientes COM
ms.date: 03/30/2017
ms.assetid: e2799bfe-88bd-49d7-9d6d-ac16a9b16b04
ms.openlocfilehash: 76b7697f431575e7bde83204739cb23f96d27064
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596481"
---
# <a name="using-the-wcf-moniker-with-com-clients"></a>Usando o WCF Moniker com clientes COM
Este exemplo demonstra como usar o moniker do serviço Windows Communication Foundation (WCF) para integrar serviços Web em ambientes de desenvolvimento baseados em COM, como Microsoft Office Visual Basic for Applications (Office VBA) ou Visual Basic 6,0. Este exemplo consiste em um cliente do Windows Script Host (. vbs), uma biblioteca de cliente de suporte (. dll) e uma biblioteca de serviço (. dll) hospedada pelo Serviços de Informações da Internet (IIS). O serviço é um serviço de calculadora e o cliente COM chama operações matemáticas — adicionar, subtrair, multiplicar e dividir — no serviço. A atividade do cliente fica visível nas janelas da caixa de mensagens.  
  
> [!NOTE]
> Os procedimentos de instalação e as instruções de compilação para esse exemplo estão localizadas no final deste tópico.  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Interop\COM`  
  
 O serviço implementa um `ICalculator` contrato definido conforme mostrado no exemplo de código a seguir.  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface ICalculator  
{  
    [OperationContract]  
    double Add(double n1, double n2);  
    [OperationContract]  
    double Subtract(double n1, double n2);  
    [OperationContract]  
    double Multiply(double n1, double n2);  
    [OperationContract]  
    double Divide(double n1, double n2);  
}  
```  
  
 O exemplo demonstra as três abordagens alternativas para usar o moniker:  
  
- Contrato tipado – o contrato é registrado como um tipo visível COM no computador cliente.  
  
- Contrato WSDL – o contrato é fornecido na forma de um documento WSDL.  
  
- Contrato de troca de metadados – o contrato é recuperado em tempo de execução de um ponto de extremidade de intercâmbio de metadados (MEX).  
  
## <a name="typed-contract"></a>Contrato digitado  
 Para usar o moniker com um uso de contrato tipado, os tipos atribuídos adequadamente para o contrato de serviço devem ser registrados com com. Primeiro, um cliente deve ser gerado usando a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). Execute o comando a seguir em um prompt de comando no diretório do cliente para gerar o proxy de tipo.  
  
```console  
svcutil.exe /n:http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples http://localhost/servicemodelsamples/service.svc /out:generatedClient.cs  
```  
  
 Essa classe deve ser incluída em um projeto e o projeto deve ser configurado para gerar um assembly assinado COM e visível quando compilado. O atributo a seguir deve ser incluído no arquivo AssemblyInfo.cs.  
  
```csharp
[assembly: ComVisible(true)]  
```  
  
 Depois de criar o projeto, registre os tipos visíveis COM usando `regasm` , conforme mostrado no exemplo a seguir.  
  
```console  
regasm.exe /tlb:CalcProxy.tlb client.dll  
```  
  
 O assembly criado deve ser adicionado ao cache de assembly global. Embora não seja estritamente necessário, isso simplifica o processo de tempo de execução que localiza o assembly. O comando a seguir adiciona o assembly ao cache de assembly global.  
  
```console  
gacutil.exe /i client.dll  
```  
  
> [!NOTE]
> O moniker do serviço requer apenas o registro do tipo e não usa o proxy para se comunicar com o serviço.  
  
 O aplicativo cliente ComCalcClient. vbs usa a `GetObject` função para construir um proxy para o serviço, usando a sintaxe do moniker do serviço para especificar o endereço, a associação e o contrato para o serviço.  
  
```vbscript
Set typedServiceMoniker = GetObject(  
"service4:address=http://localhost/ServiceModelSamples/service.svc, binding=wsHttpBinding,
contractType={9213C6D2-5A6F-3D26-839B-3BA9B82228D3}")  
```  
  
 Os parâmetros usados pelo moniker especificam:  
  
- O endereço do ponto de extremidade de serviço.  
  
- A associação que o cliente deve usar para se conectar com esse ponto de extremidade. Nesse caso, a wsHttpBinding definida pelo sistema é usada por meio de associações personalizadas que podem ser definidas em arquivos de configuração do cliente. Para uso com o Windows Script Host, a associação personalizada é definida em um arquivo cscript. exe. config no mesmo diretório que cscript. exe.  
  
- O tipo de contrato com suporte no ponto de extremidade. Esse é o tipo que foi gerado e registrado acima. Como Visual Basic script não fornece um ambiente COM COM rigidez de tipos, um identificador para o contrato deve ser especificado. Esse GUID é o `interfaceID` de CalcProxy. tlb, que pode ser exibido usando ferramentas com, como o Visualizador de objetos OLE/com (Oleview. exe). Para ambientes fortemente tipados, como o Office VBA ou o Visual Basic 6,0, adicionar uma referência explícita à biblioteca de tipos e, em seguida, declarar o tipo do objeto proxy pode ser usado no lugar do parâmetro Contract. Isso também fornece suporte ao IntelliSense durante o desenvolvimento de aplicativos cliente.  
  
 Tendo construído a instância de proxy com o moniker do serviço, o aplicativo cliente pode chamar métodos no proxy, o que resulta na infraestrutura do moniker do serviço que chama as operações de serviço correspondentes.  
  
```vbscript  
' Call the service operations using the moniker object  
WScript.Echo "Typed service moniker: 100 + 15.99 = " & typedServiceMoniker.Add(100, 15.99)  
```  
  
 Quando você executa o exemplo, a resposta da operação é exibida em uma janela da caixa de mensagem do host de script do Windows. Isso demonstra um cliente COM fazendo chamadas COM usando o moniker tipado para se comunicar com um serviço WCF. Apesar do uso de COM no aplicativo cliente, a comunicação com o serviço consiste apenas em chamadas de serviço Web.  
  
## <a name="wsdl-contract"></a>Contrato WSDL  
 Para usar o moniker com um contrato WSDL, nenhum registro de biblioteca de cliente é necessário, mas o contrato WSDL para o serviço deve ser recuperado por meio de um mecanismo fora de banda, como usar um navegador para acessar o ponto de extremidade WSDL para o serviço. O moniker pode então acessar esse contrato no momento da execução.  
  
 O aplicativo cliente ComCalcClient. vbs usa o `FileSystemObject` para acessar o arquivo WSDL salvo localmente e, em seguida, usa a `GetObject` função para construir um proxy para o serviço.  
  
```vbscript  
' Open the WSDL contract file and read it all into the wsdlContract string  
Const ForReading = 1  
Set objFSO = CreateObject("Scripting.FileSystemObject")  
Set objFile = objFSO.OpenTextFile("serviceWsdl.xml", ForReading)  
wsdlContract = objFile.ReadAll  
objFile.Close  
  
' Create a string for the service moniker including the content of the WSDL contract file  
wsdlMonikerString = "service4:address='http://localhost/ServiceModelSamples/service.svc'"  
wsdlMonikerString = wsdlMonikerString + ", binding=WSHttpBinding_ICalculator, bindingNamespace='http://Microsoft.ServiceModel.Samples'"  
wsdlMonikerString = wsdlMonikerString + ", wsdl='" & wsdlContract & "'"  
wsdlMonikerString = wsdlMonikerString + ", contract=ICalculator, contractNamespace='http://Microsoft.ServiceModel.Samples'"  
  
' Create the service moniker object  
Set wsdlServiceMoniker = GetObject(wsdlMonikerString)  
```  
  
 Os parâmetros usados pelo moniker especificam:  
  
- O endereço do ponto de extremidade de serviço.  
  
- A associação que o cliente deve usar para se conectar com esse ponto de extremidade e o namespace no qual essa associação está definida. Nesse caso, o `wsHttpBinding_ICalculator` é usado.  
  
- O WSDL que define o contrato. Nesse caso, essa é a cadeia de caracteres que foi lida do arquivo. xml.  
  
- O nome e o namespace do contrato. Essa identificação é necessária porque o WSDL pode conter mais de um contrato.  
  
    > [!NOTE]
    > Por padrão, os serviços WCF geram arquivos WSDL separados para cada namespace usado pelo. Eles são vinculados com o uso da construção de importação WSDL. Como o moniker espera uma única definição WSDL, o serviço deve usar um único namespace, como demonstrado neste exemplo, ou os arquivos separados devem ser mesclados manualmente.  
  
 Tendo construído a instância de proxy com o moniker do serviço, o aplicativo cliente pode chamar métodos no proxy, o que resulta na infraestrutura do moniker do serviço que chama as operações de serviço correspondentes.  
  
```vbscript  
' Call the service operations using the moniker object  
WScript.Echo "WSDL service moniker: 145 - 76.54 = " & wsdlServiceMoniker.Subtract(145, 76.54)  
```  
  
 Quando você executa o exemplo, a resposta da operação é exibida em uma janela da caixa de mensagem do host de script do Windows. Isso demonstra um cliente COM fazendo chamadas COM usando o moniker com um contrato WSDL para se comunicar com um serviço WCF.  
  
## <a name="metadata-exchange-contract"></a>Contrato de troca de metadados  
 Para usar o moniker com um contrato MEX, como com o contrato WSDL, nenhum registro de cliente é necessário. O contrato para o serviço é recuperado no momento da execução por meio do uso interno da troca de metadados.  
  
 O aplicativo cliente ComCalcClient. vbs novamente usa a `GetObject` função para construir um proxy para o serviço.  
  
```vbscript  
' Create a string for the service moniker specifying the address to retrieve the service metadata from  
mexMonikerString = "service4:mexAddress='http://localhost/servicemodelsamples/service.svc/mex'"  
mexMonikerString = mexMonikerString + ", address='http://localhost/ServiceModelSamples/service.svc'"  
mexMonikerString = mexMonikerString + ", binding=WSHttpBinding_ICalculator, bindingNamespace='http://Microsoft.ServiceModel.Samples'"  
mexMonikerString = mexMonikerString + ", contract=ICalculator, contractNamespace='http://Microsoft.ServiceModel.Samples'"  
  
' Create the service moniker object  
Set mexServiceMoniker = GetObject(mexMonikerString)  
```  
  
 Os parâmetros usados pelo moniker especificam:  
  
- O endereço do ponto de extremidade de troca de metadados do serviço.  
  
- O endereço do ponto de extremidade de serviço.  
  
- A associação que o cliente deve usar para se conectar com esse ponto de extremidade e o namespace no qual essa associação está definida. Nesse caso, o `wsHttpBinding_ICalculator` é usado.  
  
- O nome e o namespace do contrato. Essa identificação é necessária porque o WSDL pode conter mais de um contrato.  
  
 Tendo construído a instância de proxy com o moniker do serviço, o aplicativo cliente pode chamar métodos no proxy, o que resulta na infraestrutura do moniker do serviço que chama as operações de serviço correspondentes.  
  
```vbscript  
' Call the service operations using the moniker object  
WScript.Echo "MEX service moniker: 9 * 81.25 = " & mexServiceMoniker.Multiply(9, 81.25)  
```  
  
 Quando você executa o exemplo, a resposta da operação é exibida em uma janela da caixa de mensagem do host de script do Windows. Isso demonstra um cliente COM fazendo chamadas COM usando o moniker com um contrato MEX para se comunicar com um serviço WCF.  
  
#### <a name="to-set-up-and-build-the-sample"></a>Para configurar e compilar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Em um Prompt de Comando do Desenvolvedor para o Visual Studio, abra a pasta \client\bin, na pasta específica do idioma.  
  
    > [!NOTE]
    > Se você estiver usando o Windows Vista, o Windows Server 2008, o Windows 7 ou o Windows Server 2008 R2, certifique-se de executar o prompt de comando com privilégios de administrador.  
  
4. Digite `tlbexp.exe client.dll /out:CalcProxy.tlb` para exportar a dll para um arquivo tlb. Um "aviso de exportador da biblioteca de tipos" é esperado, mas não é um problema porque o tipo genérico não é necessário.  
  
5. Digite `regasm.exe /tlb:CalcProxy.tlb client.dll` para registrar os tipos com com. Um "aviso de exportador da biblioteca de tipos" é esperado, mas não é um problema porque o tipo genérico não é necessário.  
  
6. Digite `gacutil.exe /i client.dll` para adicionar o assembly ao cache de assembly global.  
  
#### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador  
  
1. Teste se você pode acessar o serviço usando um navegador digitando o seguinte endereço: `http://localhost/servicemodelsamples/service.svc` . Uma página de confirmação deve ser exibida em resposta.  
  
2. Execute ComCalcClient. vbs de \Client, de dentro da pasta específica do idioma. A atividade do cliente é exibida nas janelas da caixa de mensagens.  
  
3. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
#### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores  
  
1. No computador do serviço, crie um diretório virtual chamado ServiceModelSamples. O script Setupvroot. bat incluído com o exemplo pode ser usado para criar o diretório de disco e o diretório virtual.  
  
2. Copie os arquivos de programa do serviço de%SystemDrive%\Inetpub\wwwroot\servicemodelsamples para o diretório virtual ServiceModelSamples no computador do serviço. Certifique-se de incluir os arquivos no diretório \bin.  
  
3. Copie o arquivo de script do cliente da pasta \Client, na pasta específica do idioma, para o computador cliente.  
  
4. No arquivo de script, altere o valor de endereço da definição do ponto de extremidade para corresponder ao novo endereço do serviço. Substitua todas as referências a "localhost" por um nome de domínio totalmente qualificado no endereço.  
  
5. Copie o arquivo WSDL no computador cliente. No arquivo WSDL, Service WSDL. xml, substitua todas as referências a "localhost" por um nome de domínio totalmente qualificado no endereço.  
  
6. Copie a biblioteca Client. dll da pasta \client\bin, na pasta específica do idioma, para um diretório no computador cliente.  
  
7. Em um prompt de comando, navegue até o diretório de destino no computador cliente. Se estiver usando o Windows Vista ou o Windows Server 2008, certifique-se de executar o prompt de comando como administrador.  
  
8. Digite `tlbexp.exe client.dll /out:CalcProxy.tlb` para exportar a dll para um arquivo tlb. Um "aviso de exportador da biblioteca de tipos" é esperado, mas não é um problema porque o tipo genérico não é necessário.  
  
9. Digite `regasm.exe /tlb:CalcProxy.tlb client.dll` para registrar os tipos com com. Verifique se o caminho foi definido para a pasta que contém `regasm.exe` antes de executar o comando.  
  
10. Digite `gacutil.exe /i client.dll` para adicionar o assembly ao cache de assembly global. Verifique se o caminho foi definido para a pasta que contém `gacutil.exe` antes de executar o comando.  
  
11. Teste se você pode acessar o serviço do computador cliente usando um navegador.  
  
12. No computador cliente, inicie o ComCalcClient. vbs.  
  
#### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
- Para fins de segurança, remova a definição de diretório virtual e as permissões concedidas nas etapas de instalação quando você tiver concluído os exemplos.  
