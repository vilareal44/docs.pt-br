---
title: Noções básicas sobre a autenticação nas bibliotecas do Azure para .NET
description: Explica as diferentes maneiras de se autenticar com o SDK do Azure para .NET.
ms.date: 06/19/2020
ms.custom: azure-sdk-dotnet
ms.openlocfilehash: e588499a789fc5e7da7eb51009f97090ca75e562
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916611"
---
# <a name="authenticate-with-the-azure-sdk-for-net"></a>Autenticar com o SDK do Azure para .NET

## <a name="recommended-azureidentity"></a>Recomendado: Azure. Identity

Os pacotes mais recentes no SDK do Azure para .NET usam um pacote de autenticação comum para autenticar, `Azure.Identity` . `Azure.Identity`O uso do é recomendado em outros mecanismos de autenticação descritos mais adiante neste documento. Os pacotes que dão suporte às credenciais fornecidas pelo `Azure.Identity` são criados sobre o `Azure.Core` e têm identificadores de pacote começando com o *Azure.* [Consulte a lista](packages.md) de pacotes para obter um inventário das embalagens que usam `Azure.Core` .

Para obter instruções completas sobre como usar o `Azure.Identity` em seu projeto, consulte a documentação para o [cliente de identidade do Azure para .net](/dotnet/api/overview/azure/identity-readme).

> [!TIP]
> Consulte o [exemplo de identidade, gerenciamento de recursos e armazenamento do Azure](/samples/dotnet/samples/azure-identity-resource-management-storage/) para obter exemplos de como usar a identidade do Azure para gerenciar e acessar recursos do Azure.

Para autenticar com bibliotecas que não dão suporte ao Azure. Identity, consulte o restante deste tópico.

## <a name="access-azure-resources"></a>Acessar recursos do Azure

Para interagir com os recursos do Azure, como recuperar um segredo de Key Vault ou armazenar um blob no armazenamento, muitas bibliotecas de serviço do Azure exigem uma cadeia de conexão ou chaves para autenticação. Por exemplo, o banco de dados SQL usa uma [cadeia de conexão SQL padrão](https://docs.microsoft.com/azure/azure-sql/database/connect-query-dotnet-core). As cadeias de conexão de serviço são usadas em outros serviços do Azure, como [CosmosDB](/azure/cosmos-db/), [cache do Azure para Redis](/azure/azure-cache-for-redis/cache-dotnet-how-to-use-azure-redis-cache)e [barramento de serviço](/azure/service-bus-messaging/service-bus-dotnet-get-started-with-queues). Você pode obter essas cadeias de caracteres usando o portal do Azure, a CLI ou o PowerShell. Você também pode usar as bibliotecas de gerenciamento do Azure para .NET a fim de consultar recursos para criar cadeias de conexão no seu código.

Os métodos para usar uma cadeia de conexão variam de acordo com o produto. [Consulte a documentação do seu produto do Azure](/azure/?product=featured).

## <a name="manage-azure-resources"></a>Gerenciar recursos do Azure

[!include[Create service principal](includes/create-sp.md)]

Agora que a entidade de serviço foi criada, duas opções estão disponíveis para fazer a autenticação na entidade de serviço a fim de criar e gerenciar recursos.

Para ambas as opções, você precisará adicionar os seguintes pacotes NuGet ao seu projeto.

```powershell
Install-Package Microsoft.Azure.Management.Fluent
Install-Package Microsoft.Azure.Management.ResourceManager.Fluent
```

### <a name="authenticate-with-token-credentials"></a>Fazer autenticação com credenciais do token

O primeiro método é compilar o objeto de credencial de token no código. Você deve armazenar as credenciais com segurança em um arquivo de configuração, no registro ou no Azure Key Vault.

```csharp
var credentials = SdkContext.AzureCredentialsFactory
    .FromServicePrincipal(clientId,
        clientSecret,
        tenantId,
        AzureEnvironment.AzureGlobalCloud);
```

Use os valores *clientId*, *clientSecret* e *tenantId* da saída do JSON quando criou a entidade de serviço.

Em seguida, crie o objeto `Azure` do ponto de entrada para começar a trabalhar com a API:

```csharp
var azure = Microsoft.Azure.Management.Fluent.Azure
    .Configure()
    .Authenticate(credentials)
    .WithDefaultSubscription();
```

É recomendável que você forneça explicitamente a *SubscriptionId* da saída JSON para o `Azure` objeto:

```csharp
var azure = Microsoft.Azure.Management.Fluent.Azure
    .Configure()
    .Authenticate(credentials)
    .WithSubscription(subscriptionId);
```

### <a name="file-based-authentication"></a><a name="mgmt-file"></a>Autenticação baseada em arquivo

A autenticação baseada em arquivo permite que você coloque as credenciais da entidade de serviço em um arquivo de texto sem formatação e proteja-o no sistema de arquivos.

[!include[File-based authentication](includes/file-based-auth.md)]

Leia o conteúdo do arquivo e crie o objeto `Azure` de ponto de entrada para começar a trabalhar com a API:

```csharp
// pull in the location of the authentication properties file from the environment
var credentials = SdkContext.AzureCredentialsFactory
    .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

var azure = Microsoft.Azure.Management.Fluent.Azure
    .Configure()
    .Authenticate(credentials)
    .WithDefaultSubscription();
```
