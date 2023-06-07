---
title: Gerenciamento e acesso a dados
description: Saiba como acessar e manipular dados no ASP.NET Web Forms e Blazor .
author: csharpfritz
ms.author: jefritz
no-loc:
- Blazor
ms.date: 04/26/2020
ms.openlocfilehash: 8bd326e6952708b2099c3a575d6811990335df17
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267588"
---
# <a name="work-with-data"></a>Trabalhar com dados

Acesso a dados é o backbone de um aplicativo ASP.NET Web Forms. Se você estiver criando formulários para a Web, o que acontecerá com esses dados? Com Web Forms, havia várias técnicas de acesso a dados que podem ser usadas para interagir com um banco de dados:

- Fontes de dados
- ADO.NET
- Entity Framework

Fontes de dados eram controles que você pode fazer em uma Web Forms página e configurar como outros controles. O Visual Studio forneceu um conjunto amigável de caixas de diálogo para configurar e associar os controles às suas páginas de Web Forms. Os desenvolvedores que apreciam uma abordagem de "código baixo" ou "sem código" preferem essa técnica quando Web Forms foi lançado pela primeira vez.

![Fontes de dados](media/data/datasources.png)

ADO.NET é a abordagem de nível baixo para interagir com um banco de dados. Seus aplicativos podem criar uma conexão com o banco de dados com comandos, conjuntos de registros e conjuntos de dados para interação. Os resultados podem então ser associados a campos na tela sem muito código. A desvantagem dessa abordagem era que cada conjunto de objetos ADO.NET ( `Connection` , `Command` e `Recordset` ) foi associado a bibliotecas fornecidas por um fornecedor de banco de dados. O uso desses componentes tornou o código rígido e difícil de migrar para um banco de dados diferente.

## <a name="entity-framework"></a>Entity Framework

Entity Framework (EF) é a estrutura de mapeamento relacional de objeto de software livre mantida pelo .NET Foundation. Lançado inicialmente com .NET Framework, o EF permite gerar código para as conexões de banco de dados, esquemas de armazenamento e interações. Com essa abstração, você pode se concentrar nas regras de negócios do seu aplicativo e permitir que o banco de dados seja gerenciado por um administrador de banco de dados confiável. No .NET Core, você pode usar uma versão atualizada do EF chamada EF Core. EF Core ajuda a gerar e manter as interações entre seu código e o banco de dados com uma série de comandos que estão disponíveis para você usando a `dotnet ef` ferramenta de linha de comando. Vamos dar uma olhada em alguns exemplos para ajudá-lo a trabalhar com um banco de dados.

### <a name="ef-code-first"></a>Code First EF

Uma maneira rápida de começar a criar suas interações de banco de dados é começar com os objetos de classe com os quais você deseja trabalhar. O EF fornece uma ferramenta para ajudar a gerar o código de banco de dados apropriado para suas classes. Essa abordagem é chamada de desenvolvimento "Code First". Considere a seguinte `Product` classe para um aplicativo de vitrine de exemplo que desejamos armazenar em um banco de dados relacional como Microsoft SQL Server.

```csharp
public class Product
{
    public int Id { get; set; }

    [Required]
    public string Name { get; set; }

    [MaxLength(4000)]
    public string Description { get; set; }

    [Range(0, 99999,99)]
    [DataType(DataType.Currency)]
    public decimal Price { get; set; }
}
```

O produto tem uma chave primária e três campos adicionais que seriam criados em nosso banco de dados:  

- O EF identificará a `Id` propriedade como uma chave primária por convenção.
- `Name` será armazenado em uma coluna configurada para armazenamento de texto. O `[Required]` atributo decoração dessa propriedade irá adicionar uma `not null` restrição para ajudar a impor esse comportamento declarado da propriedade.
- `Description` será armazenado em uma coluna configurada para armazenamento de texto e terá um comprimento máximo configurado de 4000 caracteres, conforme determinado pelo `[MaxLength]` atributo. O esquema de banco de dados será configurado com uma coluna chamada `MaxLength` usando o tipo de dados `varchar(4000)` .
- A `Price` propriedade será armazenada como moeda. O `[Range]` atributo irá gerar restrições apropriadas para impedir o armazenamento de dados fora dos valores mínimo e máximo declarados.

Precisamos adicionar essa `Product` classe a uma classe de contexto de banco de dados que define as operações de conexão e conversão com nosso banco de dados.

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
}
```

A `MyDbContext` classe fornece uma propriedade que define o acesso e a tradução da `Product` classe.  Seu aplicativo configura essa classe para interação com o banco de dados usando as seguintes entradas no `Startup` método da classe `ConfigureServices` :

```csharp
services.AddDbContext<MyDbContext>(options =>
    options.UseSqlServer("MY DATABASE CONNECTION STRING"));
```

O código anterior se conectará a um banco de dados SQL Server com a cadeia de conexão especificada. Você pode colocar a cadeia de conexão em seu *appsettings.jsem* arquivo, variáveis de ambiente ou outros locais de armazenamento de configuração e substituir essa cadeia de caracteres inserida adequadamente.

Em seguida, você pode gerar a tabela de banco de dados apropriada para essa classe usando os seguintes comandos:

```dotnetcli
dotnet ef migrations add 'Create Product table'
dotnet ef database update
```

O primeiro comando define as alterações que você está fazendo no esquema de banco de dados como uma nova migração do EF chamada `Create Product table` .  Uma migração define como aplicar e remover as novas alterações no banco de dados.

Depois de aplicado, você tem uma `Product` tabela simples em seu banco de dados e algumas novas classes adicionadas ao projeto que ajudam a gerenciar o esquema de banco de dados.  Você pode encontrar essas classes geradas, por padrão, em uma nova pasta chamada *migrações*.  Ao fazer alterações na `Product` classe ou adicionar mais classes relacionadas que você gostaria de interagir com o banco de dados, você precisa executar os comandos de linha de comando novamente com um novo nome da migração.  Esse comando gerará outro conjunto de classes de migração para atualizar o esquema de banco de dados.

### <a name="ef-database-first"></a>Database First EF

Para bancos de dados existentes, você pode gerar as classes para EF Core usando as ferramentas de linha de comando do .NET. Para Scaffold as classes, use uma variação do seguinte comando:

```dotnetcli
dotnet ef dbcontext scaffold "CONNECTION STRING" Microsoft.EntityFrameworkCore.SqlServer -c MyDbContext -t Product -t Customer
```

O comando anterior se conecta ao banco de dados usando a cadeia de conexão especificada e o `Microsoft.EntityFrameworkCore.SqlServer` provedor. Uma vez conectado, uma classe de contexto de banco de dados chamada `MyDbContext` é criada. Além disso, as classes de suporte são criadas para as `Product` `Customer` tabelas e que foram especificadas com as `-t` opções. Há muitas opções de configuração para esse comando gerar a hierarquia de classe apropriada para seu banco de dados. Para obter uma referência completa, consulte [a documentação do comando](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold).

Mais informações sobre [EF Core](/ef/core/) podem ser encontradas no site do Microsoft docs.

## <a name="interact-with-web-services"></a>Interagir com os serviços Web

Quando o ASP.NET foi lançado pela primeira vez, os serviços SOAP eram a maneira preferida de servidores Web e clientes de trocar dados. Muito mudou desde esse tempo, e as interações preferenciais com serviços mudaram para direcionar interações de cliente HTTP. Com ASP.NET Core e Blazor , você pode registrar a configuração de seu `HttpClient` no `Startup` método da classe `ConfigureServices` . Use essa configuração quando precisar interagir com o ponto de extremidade HTTP. Considere o seguinte código de configuração:

```csharp
services.AddHttpClient("github", client =>
{
    client.BaseAddress = new Uri("http://api.github.com/");
    // Github API versioning
    client.DefaultRequestHeaders.Add("Accept", "application/vnd.github.v3+json");
    // Github requires a user-agent
    client.DefaultRequestHeaders.Add("User-Agent", "BlazorWebForms-Sample");
});
```

Sempre que você precisar acessar dados do GitHub, crie um cliente com um nome de `github` . O cliente é configurado com o endereço base e os cabeçalhos de solicitação são definidos adequadamente. Insira o `IHttpClientFactory` em seus Blazor componentes com a `@inject` diretiva ou um `[Inject]` atributo em uma propriedade. Crie seu cliente nomeado e interaja com os serviços usando a seguinte sintaxe:

```razor
@inject IHttpClientFactory factory

...

@code {
    protected override async Task OnInitializedAsync()
    {
        var client = factory.CreateClient("github");
        var response = await client.GetAsync("repos/dotnet/docs/issues");
        response.EnsureStatusCode();
        var content = async response.Content.ReadAsStringAsync();
    }
}
```

Esse método retorna a cadeia de caracteres que descreve a coleção de problemas no repositório do GitHub *dotnet/docs* . Ele retorna o conteúdo no formato JSON e é desserializado em objetos de problema do GitHub apropriados. Há várias maneiras pelas quais você pode configurar o `HttpClientFactory` para entregar `HttpClient` objetos pré-configurados. Tente configurar várias `HttpClient` instâncias com nomes e pontos de extremidade diferentes para os vários serviços Web com os quais você trabalha. Essa abordagem fará com que as interações com esses serviços sejam mais fáceis de trabalhar em cada página. Para obter mais detalhes, leia [a documentação do IHttpClientFactory](/aspnet/core/fundamentals/http-requests).

>[!div class="step-by-step"]
>[Anterior](forms-validation.md) 
> [Avançar](middleware.md)
