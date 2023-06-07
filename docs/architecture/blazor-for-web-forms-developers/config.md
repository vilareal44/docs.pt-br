---
title: Configuração do aplicativo
description: Saiba como configurar Blazor aplicativos sem usar ConfigurationManager.
author: csharpfritz
ms.author: jefritz
no-loc:
- Blazor
ms.date: 04/01/2020
ms.openlocfilehash: 6154b4f8c7a5bff42e603b12d5ef85468b80224e
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267497"
---
# <a name="app-configuration"></a>Configuração do aplicativo

A principal maneira de carregar a configuração de aplicativo no Web Forms é com entradas no arquivo *web.config* &mdash; no servidor ou em um arquivo de configuração relacionado referenciado por *web.config*. Você pode usar o `ConfigurationManager` objeto estático para interagir com as configurações do aplicativo, cadeias de conexão do repositório de dados e outros provedores de configuração estendidos que são adicionados ao aplicativo. É comum ver interações com a configuração do aplicativo, como visto no código a seguir:

```csharp
var configurationValue = ConfigurationManager.AppSettings["ConfigurationSettingName"];
var connectionString = ConfigurationManager.ConnectionStrings["MyDatabaseConnectionName"].ConnectionString;
```

Com ASP.NET Core e servidor Blazor , o arquivo de *web.config* pode estar presente se seu aplicativo estiver hospedado em um servidor IIS do Windows. No entanto, não há nenhuma `ConfigurationManager` interação com essa configuração, e você pode receber mais configurações de aplicativo estruturado de outras fontes. Vamos dar uma olhada em como a configuração é coletada e como você ainda pode acessar as informações de configuração de um arquivo *web.config* .

## <a name="configuration-sources"></a>Fontes de configuração

ASP.NET Core reconhece que há muitas fontes de configuração que você pode querer usar para seu aplicativo. A estrutura tenta oferecer o melhor desses recursos por padrão. A configuração é lida e agregada dessas várias fontes por ASP.NET Core. Os valores carregados posteriormente para a mesma chave de configuração têm precedência sobre os valores anteriores.

A ASP.NET Core foi projetada para ser compatível com a nuvem e para facilitar a configuração de aplicativos para operadores e desenvolvedores. ASP.NET Core tem reconhecimento de ambiente e sabe se ele está em execução no `Production` seu `Development` ambiente ou. O indicador de ambiente é definido na `ASPNETCORE_ENVIRONMENT` variável de ambiente do sistema. Se nenhum valor for configurado, o aplicativo usa como padrão a execução no `Production` ambiente.

Seu aplicativo pode disparar e adicionar a configuração de várias fontes com base no nome do ambiente. Por padrão, a configuração é carregada dos seguintes recursos na ordem listada:

1. *appsettings.jsno* arquivo, se presente
1. *appSettings. Arquivo {ENVIRONMENT_NAME}. JSON* , se presente
1. Arquivo de segredos do usuário em disco, se presente
1. Variáveis de ambiente
1. Argumentos de linha de comando

## <a name="appsettingsjson-format-and-access"></a>appsettings.jsno formato e acesso

O *appsettings.jsno* arquivo pode ser hierárquico com valores estruturados como o JSON a seguir:

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

Quando apresentado com o JSON anterior, o sistema de configuração mescla valores filho e faz referência a seus caminhos hierárquicos totalmente qualificados. Um caractere de dois-pontos ( `:` ) separa cada propriedade na hierarquia. Por exemplo, a chave de configuração `section1:key0` acessa o `section1` valor do literal do objeto `key0` .

## <a name="user-secrets"></a>Segredos do usuário

Os segredos do usuário são:

* Valores de configuração que são armazenados em um arquivo JSON na estação de trabalho do desenvolvedor, fora da pasta de desenvolvimento do aplicativo.
* Carregado somente quando executado no `Development` ambiente.
* Associado a um aplicativo específico.
* Gerenciado com o comando do CLI do .NET Core `user-secrets` .

Configure seu aplicativo para o armazenamento de segredos executando o `user-secrets` comando:

```dotnetcli
dotnet user-secrets init
```

O comando anterior adiciona um `UserSecretsId` elemento ao arquivo de projeto. O elemento contém um GUID, que é usado para associar segredos ao aplicativo. Em seguida, você pode definir um segredo com o `set` comando. Por exemplo:

```dotnetcli
dotnet user-secrets set "Parent:ApiKey" "12345"
```

O comando anterior disponibiliza a `Parent:ApiKey` chave de configuração na estação de trabalho de um desenvolvedor com o valor `12345` .

Para obter mais informações sobre como criar, armazenar e gerenciar segredos do usuário, consulte o [armazenamento seguro de segredos do aplicativo em desenvolvimento no ASP.NET Core](/aspnet/core/security/app-secrets) documento.

## <a name="environment-variables"></a>Variáveis de ambiente

O próximo conjunto de valores carregados em sua configuração de aplicativo são as variáveis de ambiente do sistema. Todas as configurações de variável de ambiente do seu sistema agora estão acessíveis para você por meio da API de configuração. Os valores hierárquicos são achatados e separados por caracteres de dois-pontos quando lidos dentro de seu aplicativo. No entanto, alguns sistemas operacionais não permitem os nomes de variável de ambiente de caractere de dois pontos. ASP.NET Core resolve essa limitação convertendo valores que têm sublinhados duplos ( `__` ) em dois pontos quando eles são acessados. O `Parent:ApiKey` valor da seção segredos do usuário acima pode ser substituído pela variável de ambiente `Parent__ApiKey` .

## <a name="command-line-arguments"></a>Argumentos de linha de comando

A configuração também pode ser fornecida como argumentos de linha de comando quando seu aplicativo é iniciado. Use a notação de traço duplo ( `--` ) ou de barra ( `/` ) para indicar o nome do valor de configuração a ser definido e o valor a ser configurado. A sintaxe é semelhante aos seguintes comandos:

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run Parent:ApiKey=67890
```

## <a name="the-return-of-webconfig"></a>O retorno de web.config

Se você tiver implantado seu aplicativo no Windows no IIS, o arquivo de *web.config* ainda configurará o IIS para gerenciar seu aplicativo. Por padrão, o IIS adiciona uma referência ao módulo de ASP.NET Core (ANCM). ANCM é um módulo do IIS nativo que hospeda seu aplicativo no lugar do servidor Web Kestrel. Esta seção *web.config* é semelhante à marcação XML a seguir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

A configuração específica do aplicativo pode ser definida aninhando-se um `environmentVariables` elemento no `aspNetCore` elemento. Os valores definidos nesta seção são apresentados ao aplicativo ASP.NET Core como variáveis de ambiente. As variáveis de ambiente são carregadas adequadamente durante o segmento de inicialização do aplicativo.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="Parent:ApiKey" value="67890" />
  </environmentVariables>
</aspNetCore>
```

## <a name="read-configuration-in-the-app"></a>Ler a configuração no aplicativo

ASP.NET Core fornece a configuração de aplicativo por meio da <xref:Microsoft.Extensions.Configuration.IConfiguration> interface. Essa interface de configuração deve ser solicitada por seus Blazor componentes, Blazor páginas e qualquer outra classe gerenciada ASP.NET Core que precise de acesso à configuração. A estrutura de ASP.NET Core preencherá automaticamente essa interface com a configuração resolvida configurada anteriormente. Em uma Blazor página ou marcação Razor de um componente, você pode injetar o `IConfiguration` objeto com uma `@inject` diretiva na parte superior do arquivo *. Razor* como este:

```razor
@inject IConfiguration Configuration
```

Essa instrução anterior torna o `IConfiguration` objeto disponível como a `Configuration` variável em todo o restante do modelo Razor.

As definições de configuração individuais podem ser lidas especificando-se a hierarquia de definição de configuração procurada como um parâmetro de indexador:

```csharp
var mySetting = Configuration["section1:key0"];
```

Você pode buscar seções de configuração inteiras usando o <xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection%2A> método para recuperar uma coleção de chaves em um local específico com uma sintaxe semelhante a para `GetSection("section1")` recuperar a configuração de section1 do exemplo anterior.

## <a name="strongly-typed-configuration"></a>Configuração com rigidez de tipos

Com Web Forms, era possível criar um tipo de configuração fortemente tipado herdado do <xref:System.Configuration.ConfigurationSection> tipo e dos tipos associados. Um `ConfigurationSection` permitia que você configure algumas regras de negócio e o processamento para esses valores de configuração.

No ASP.NET Core, você pode especificar uma hierarquia de classe que receberá os valores de configuração. Essas classes:

* Não é necessário herdar de uma classe pai.
* Deve incluir `public` Propriedades que correspondam às propriedades e referências de tipo para a estrutura de configuração que você deseja capturar.

Para o *appsettings.jsanterior no* exemplo, você pode definir as seguintes classes para capturar os valores:

```csharp
public class MyConfig
{
    public MyConfigSection section0 { get; set;}

    public MyConfigSection section1 { get; set;}
}

public class MyConfigSection
{
    public string key0 { get; set; }

    public string key1 { get; set; }
}
```

Essa hierarquia de classe pode ser populada adicionando a seguinte linha ao `Startup.ConfigureServices` método:

```csharp
services.Configure<MyConfig>(Configuration);
```

No restante do aplicativo, você pode adicionar um parâmetro de entrada a classes ou uma `@inject` diretiva em modelos do Razor do tipo `IOptions<MyConfig>` para receber as definições de configuração com rigidez de tipos. A `IOptions<MyConfig>.Value` Propriedade produzirá o `MyConfig` valor populado a partir das definições de configuração.

```razor
@inject IOptions<MyConfig> options
@code {
    var MyConfiguration = options.Value;
    var theSetting = MyConfiguration.section1.key0;
}
```

Mais informações sobre o recurso de opções podem ser encontradas no [padrão de opções no documento ASP.NET Core](/aspnet/core/fundamentals/configuration/options#options-interfaces) .

>[!div class="step-by-step"]
>[Anterior](middleware.md) 
> [Avançar](security-authentication-authorization.md)
