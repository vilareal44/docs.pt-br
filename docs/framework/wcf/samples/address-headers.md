---
title: Cabeçalhos de endereço
ms.date: 03/30/2017
ms.assetid: b0c94d4a-3bde-4b4d-bb6d-9f12bc3a6940
ms.openlocfilehash: 133826bbbea62b660bdcdd884ce657528ad30873
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575999"
---
# <a name="address-headers"></a>Cabeçalhos de endereço

O exemplo de cabeçalhos de endereço demonstra como os clientes podem passar parâmetros de referência para um serviço usando Windows Communication Foundation (WCF).

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

A especificação WS-Addressing define a noção de uma referência de ponto de extremidade como uma maneira de resolver um ponto de extremidade de serviço Web específico. No WCF, as referências de ponto de extremidade são modeladas usando a `EndpointAddress` classe – `EndpointAddress` é o tipo do campo de endereço da `ServiceEndpoint` classe.

Parte do modelo de referência do ponto de extremidade é que cada referência pode executar alguns parâmetros de referência que adicionam informações de identificação extras. No WCF, esses parâmetros de referência são modelados como instâncias da `AddressHeader` classe.

Neste exemplo, o cliente adiciona um parâmetro de referência ao `EndpointAddress` do ponto de extremidade do cliente. O serviço procura esse parâmetro de referência e usa seu valor na lógica de sua operação de serviço "Olá".

## <a name="client"></a>Cliente

Para que o cliente envie um parâmetro de referência, ele deve adicionar um `AddressHeader` ao `EndpointAddress` do `ServiceEndpoint` . Como a `EndpointAddress` classe é imutável, a modificação de um endereço de ponto de extremidade deve ser feita usando a `EndpointAddressBuilder` classe. O código a seguir inicializa o cliente para enviar um parâmetro de referência como parte de sua mensagem.

```csharp
HelloClient client = new HelloClient();
EndpointAddressBuilder builder =
    new EndpointAddressBuilder(client.Endpoint.Address);
AddressHeader header =
    AddressHeader.CreateAddressHeader(IDName, IDNamespace, "John");
builder.Headers.Add(header);
client.Endpoint.Address = builder.ToEndpointAddress();
```

O código cria um `EndpointAddressBuilder` usando o original `EndpointAddress` como um valor inicial. Em seguida, ele adiciona um cabeçalho de endereço recém-criado; a chamada para `CreateAddressHeader` cria um cabeçalho com um nome, namespace e valor específicos. Aqui, o valor é "João". Depois que o cabeçalho é adicionado ao construtor, o `ToEndpointAddress()` método converte o Construtor (mutável) de volta em um endereço de ponto de extremidade (imutável), que é atribuído de volta ao campo de endereço do ponto de extremidade do cliente.

Agora, quando o cliente chama `Console.WriteLine(client.Hello());` , o serviço é capaz de obter o valor desse parâmetro de endereço, como visto na saída resultante do cliente.

`Hello, John`

## <a name="server"></a>Servidor

A implementação da operação de serviço `Hello()` usa a atual `OperationContext` para inspecionar os valores dos cabeçalhos na mensagem de entrada.

```csharp
string id = null;
// look at headers on incoming message
for (int i = 0;
     i < OperationContext.Current.IncomingMessageHeaders.Count;
     ++i)
{
    MessageHeaderInfo h = OperationContext.Current.IncomingMessageHeaders[i];
    // for any reference parameters with the correct name & namespace
    if (h.IsReferenceParameter &&
        h.Name == IDName &&
        h.Namespace == IDNamespace)
    {
        // read the value of that header
        XmlReader xr = OperationContext.Current.IncomingMessageHeaders.GetReaderAtHeader(i);
        id = xr.ReadElementContentAsString();
    }
}
return "Hello, " + id;
```

O código itera em todos os cabeçalhos na mensagem de entrada, procurando cabeçalhos que são parâmetros de referência com o nome específico e. Quando o parâmetro é encontrado, ele lê o valor do parâmetro e o armazena na variável "ID".

#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Client\AddressHeaders`
