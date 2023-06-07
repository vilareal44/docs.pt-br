---
title: Comando dotnet new
description: O comando dotnet new cria novos projetos .NET Core com base no modelo especificado.
no-loc:
- Blazor
- WebAssembly
ms.date: 04/10/2020
ms.openlocfilehash: ec41b3b79ed5eded7c9124d3e4d95c658ee39580
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86173114"
---
# <a name="dotnet-new"></a>dotnet new

**Este artigo aplica-se a:** ✔️ SDK do .net Core 2,0 e versões posteriores

## <a name="name"></a>Nome

`dotnet new` – Cria um novo projeto, arquivo de configuração ou solução com base no modelo especificado.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet new <TEMPLATE> [--dry-run] [--force] [-i|--install {PATH|NUGET_ID}]
    [-lang|--language {"C#"|"F#"|VB}] [-n|--name <OUTPUT_NAME>]
    [--nuget-source <SOURCE>] [-o|--output <OUTPUT_DIRECTORY>]
    [-u|--uninstall] [--update-apply] [--update-check] [Template options]

dotnet new <TEMPLATE> [-l|--list] [--type <TYPE>]

dotnet new -h|--help
```

## <a name="description"></a>Descrição

O `dotnet new` comando cria um projeto do .NET Core ou outros artefatos com base em um modelo.

O comando chama o [mecanismo de modelo](https://github.com/dotnet/templating) para criar os artefatos em disco com base no modelo e nas opções especificadas.

### <a name="implicit-restore"></a>Restauração implícita

[!INCLUDE[dotnet restore note](~/includes/dotnet-restore-note.md)]

## <a name="arguments"></a>Argumentos

- **`TEMPLATE`**

  O modelo para o qual criar uma instância quando o comando é invocado. Cada modelo pode ter opções específicas que podem ser passadas. Para obter mais informações, consulte [Opções de modelo](#template-options).

  Você pode executar `dotnet new --list` ou `dotnet new -l` para ver uma lista de todos os modelos instalados. Se o `TEMPLATE` valor não for uma correspondência exata no texto na coluna **modelos** ou **nome curto** da tabela retornada, uma correspondência de subcadeia de caracteres será executada nessas duas colunas.

  A partir do SDK do .NET Core 3,0, a CLI procura modelos em NuGet.org quando você invoca o `dotnet new` comando nas seguintes condições:

  - Se a CLI não encontrar uma correspondência de modelo ao invocar `dotnet new` , nem mesmo parcial.
  - Se houver uma versão mais recente do modelo disponível. Nesse caso, o projeto ou artefato é criado, mas a CLI avisa sobre uma versão atualizada do modelo.

  A tabela a seguir mostra os modelos que vêm pré-instalados com o SDK do .NET Core. O idioma padrão do modelo é mostrado entre parênteses. Clique no link nome curto para ver as opções de modelo específicas.

| Modelos                                    | Nome curto                      | Linguagem     | Marcas                                  | Incluída |
|----------------------------------------------|---------------------------------|--------------|---------------------------------------|------------|
| Aplicativo do Console                          | [MMC](#console)             | [C#], F#, VB | Comum/Console                        | 1.0        |
| Biblioteca de classes                                | [classlib](#classlib)           | [C#], F#, VB | Comum/Library                        | 1.0        |
| Aplicativo WPF                              | [WFP](#wpf)                     | [C#]         | Comum/WPF                            | 3.0        |
| Biblioteca de classes do WPF                            | [wpflib](#wpf)                  | [C#]         | Comum/WPF                            | 3.0        |
| Biblioteca de Controles Personalizados do WPF                   | [wpfcustomcontrollib](#wpf)     | [C#]         | Comum/WPF                            | 3.0        |
| Biblioteca de controle de usuário WPF                     | [wpfusercontrollib](#wpf)       | [C#]         | Comum/WPF                            | 3.0        |
| Aplicativo Windows Forms (WinForms)         | [WinForms](#winforms)           | [C#]         | Comum/WinForms                       | 3.0        |
| Biblioteca de classes do Windows Forms (WinForms)       | [winformslib](#winforms)        | [C#]         | Comum/WinForms                       | 3.0        |
| Serviço de trabalho                               | [funcionários](#web-others)           | [C#]         | Comum/de trabalho/Web                     | 3.0        |
| Projeto de Teste de Unidade                            | [MSTest](#test)                 | [C#], F#, VB | Teste/MSTest                           | 1.0        |
| Projeto de Teste do NUnit 3                         | [NUnit](#nunit)                  | [C#], F#, VB | Teste/NUnit                            | 2.1.400    |
| Item de Teste do NUnit 3                            | `nunit-test`                    | [C#], F#, VB | Teste/NUnit                            | 2.2        |
| Projeto de Teste xUnit                           | [xUnit](#test)                  | [C#], F#, VB | Teste/xUnit                            | 1.0        |
| Componente Razor                              | `razorcomponent`                | [C#]         | Web/ASP.NET                           | 3.0        |
| Página do Razor                                   | [Web](#page)                   | [C#]         | Web/ASP.NET                           | 2,0        |
| Importações de Exibição do MVC                              | [viewimports](#namespace)       | [C#]         | Web/ASP.NET                           | 2,0        |
| MVC ViewStart                                | `viewstart`                     | [C#]         | Web/ASP.NET                           | 2,0        |
| BlazorAplicativo de servidor                            | [blazorserver](#blazorserver)   | [C#]         | SiteBlazor                            | 3.0        |
| BlazorDo WebAssembly aplicativo                       | `blazorwasm`                    | [C#]         | SiteBlazor/WebAssembly                            | 3.1.300    |
| ASP.NET Core Vazio                           | [site](#web)                     | [C#], F#     | Web/Vazio                             | 1.0        |
| Aplicativo Web ASP.NET Core (Modelo-Exibição-Controlador) | [MVC](#web-options)             | [C#], F#     | Web/MVC                               | 1.0        |
| Aplicativo Web ASP.NET Core                         | [webapp, Razor](#web-options)   | [C#]         | Web/MVC/Razor Pages                   | 2,2, 2,0   |
| ASP.NET Core com Angular                    | [angular](#spa)                 | [C#]         | Web/MVC/SPA                           | 2,0        |
| ASP.NET Core com React.js                   | [reagir](#spa)                   | [C#]         | Web/MVC/SPA                           | 2,0        |
| ASP.NET Core com React.js e Redux         | [reactredux](#reactredux)       | [C#]         | Web/MVC/SPA                           | 2,0        |
| Biblioteca de Classes do Razor                          | [razorclasslib](#razorclasslib) | [C#]         | Web/Razor/Biblioteca/Biblioteca de Classes do Razor | 2.1        |
| API Web do ASP.NET Core                         | [webAPI](#webapi)               | [C#], F#     | Web/WebAPI                            | 1.0        |
| ASP.NET Core serviço gRPC                    | [grpc](#web-others)             | [C#]         | Web/gRPC                              | 3.0        |
| arquivo dotnet gitignore                        | `gitignore`                     |              | Config                                | 3.0        |
| Arquivo global.json                             | [globaljson](#globaljson)       |              | Config                                | 2,0        |
| Configuração do NuGet                                 | `nugetconfig`                   |              | Config                                | 1.0        |
| Arquivo de manifesto da ferramenta local dotnet              | `tool-manifest`                 |              | Config                                | 3.0        |
| Configuração da Web                                   | `webconfig`                     |              | Config                                | 1.0        |
| Arquivo de Solução                                | `sln`                           |              | Solução                              | 1.0        |
| Arquivo de buffer de protocolo                         | [proto](#namespace)             |              | Web/gRPC                              | 3.0        |

## <a name="options"></a>Opções

- **`--dry-run`**

  Exibe um resumo do que ocorreria se o comando fornecido fosse executado se resultasse na criação de um modelo. Disponível desde o SDK do .NET Core 2,2.

- **`--force`**

  Força o conteúdo a ser gerado mesmo se ele alterasse os arquivos existentes. Isso é necessário quando o modelo escolhido substituiria os arquivos existentes no diretório de saída.

- **`-h|--help`**

  Imprime uma ajuda para o comando. Ele pode ser invocado para o `dotnet new` próprio comando ou para qualquer modelo. Por exemplo, `dotnet new mvc --help`.

- **`-i|--install <PATH|NUGET_ID>`**

  Instala um pacote de modelos do `PATH` ou `NUGET_ID` fornecido. Se você deseja instalar uma versão de pré-lançamento de um pacote de modelo, é necessário especificar a versão no formato `<package-name>::<package-version>`. Por padrão, `dotnet new` \* o passa para a versão, que representa a versão de pacote estável mais recente. Veja um exemplo na seção [exemplos](#examples) .
  
  Se uma versão do modelo já tiver sido instalada quando você executar esse comando, o modelo será atualizado para a versão especificada ou para a versão estável mais recente se nenhuma versão tiver sido especificada.

  Para obter informações sobre a criação de modelos personalizados, consulte [Custom templates for dotnet new](custom-templates.md) (Modelos personalizados para dotnet new).

- **`-l|--list`**

  Lista os modelos que contêm o nome especificado. Se nenhum nome for especificado, listará todos os modelos.

- **`-lang|--language {C#|F#|VB}`**

  A linguagem do modelo a ser criada. A linguagem aceita varia de acordo com o modelo (consulte os padrões na seção [Argumentos](#arguments)). Não é válida para alguns modelos.

  > [!NOTE]
  > Alguns shells interpretam `#` como um caractere especial. Nesses casos, coloque o valor do parâmetro de idioma entre aspas. Por exemplo, `dotnet new console -lang "F#"`.

- **`-n|--name <OUTPUT_NAME>`**

  O nome para a saída criada. Se nenhum nome for especificado, o nome do diretório atual será usado.

- **`--nuget-source <SOURCE>`**

  Especifica uma origem do NuGet a ser usada durante a instalação. Disponível desde o SDK do .NET Core 2,1.

- **`-o|--output <OUTPUT_DIRECTORY>`**

  Local para colocar a saída gerada. O padrão é o diretório atual.

- **`--type <TYPE>`**

  Filtra modelos com base em tipos disponíveis. Os valores predefinidos são `project` , `item` e `other` .

- **`-u|--uninstall [PATH|NUGET_ID]`**

  Desinstala um pacote de modelos no `PATH` ou `NUGET_ID` fornecido. Quando o `<PATH|NUGET_ID>` valor não for especificado, todos os pacotes de modelos instalados atualmente e seus modelos associados serão exibidos. Ao especificar `NUGET_ID` , não inclua o número de versão.

  Se você não especificar um parâmetro para essa opção, o comando listará os modelos instalados e os detalhes sobre eles.

  > [!NOTE]
  > Para desinstalar um modelo usando um `PATH`, você precisa qualificar totalmente o caminho. Por exemplo, *C:/Users/ \<USER> /Documents/templates/GarciaSoftware.ConsoleTemplate.CSharp* funcionará, mas *./GarciaSoftware.ConsoleTemplate.CSharp* da pasta que a contém não irá.
  > Não inclua uma barra de diretório final de encerramento no caminho do modelo.

- **`--update-apply`**

  Verifica se há atualizações disponíveis para os pacotes de modelos que estão instalados no momento e os instala. Disponível desde o SDK do .NET Core 3.0.

- **`--update-check`**

  Verifica se há atualizações disponíveis para os pacotes de modelos que estão instalados no momento. Disponível desde o SDK do .NET Core 3.0.

## <a name="template-options"></a>Opções de modelo

Cada modelo de projeto pode ter opções adicionais disponíveis. Os principais modelos têm as seguintes opções adicionais:

### <a name="console"></a>console

- **`-f|--framework <FRAMEWORK>`**

  Especifica a [estrutura](../../standard/frameworks.md) a ser direcionada. Disponível desde o SDK do .NET Core 3.0.

  A tabela a seguir lista os valores padrão de acordo com o número de versão do SDK que você está usando:

  | Versão do SDK | Valor padrão   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`--langVersion <VERSION_NUMBER>`**

  Define a `LangVersion` propriedade no arquivo de projeto criado. Por exemplo, use `--langVersion 7.3` para usar C# 7.3. Sem suporte para F#. Disponível desde o SDK do .NET Core 2,2.

  Para obter uma lista de versões padrão do C#, consulte [padrões](../../csharp/language-reference/configure-language-version.md#defaults).

- **`--no-restore`**

  Se especificado, não executa uma restauração implícita durante a criação do projeto. Disponível desde o SDK do .NET Core 2,2.

***

### <a name="classlib"></a>classlib

- **`-f|--framework <FRAMEWORK>`**

  Especifica a [estrutura](../../standard/frameworks.md) a ser direcionada. Valores: `netcoreapp<version>` para criar uma Biblioteca de classes do .NET Core ou `netstandard<version>` para criar uma Biblioteca de classes .NET Standard. O valor padrão é `netstandard2.0`.

- **`--langVersion <VERSION_NUMBER>`**

  Define a `LangVersion` propriedade no arquivo de projeto criado. Por exemplo, use `--langVersion 7.3` para usar C# 7.3. Sem suporte para F#. Disponível desde o SDK do .NET Core 2,2.

  Para obter uma lista de versões padrão do C#, consulte [padrões](../../csharp/language-reference/configure-language-version.md#defaults).

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

***

### <a name="wpf-wpflib-wpfcustomcontrollib-wpfusercontrollib"></a><a name="wpf"></a>WPF, wpflib, wpfcustomcontrollib, wpfusercontrollib

- **`-f|--framework <FRAMEWORK>`**

  Especifica a [estrutura](../../standard/frameworks.md) a ser direcionada. O valor padrão é `netcoreapp3.1`. Disponível desde o SDK do .NET Core 3,1.

- **`--langVersion <VERSION_NUMBER>`**

  Define a `LangVersion` propriedade no arquivo de projeto criado. Por exemplo, use `--langVersion 7.3` para usar C# 7.3.

  Para obter uma lista de versões padrão do C#, consulte [padrões](../../csharp/language-reference/configure-language-version.md#defaults).

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

***

### <a name="winforms-winformslib"></a><a name="winforms"></a>WinForms, winformslib

- **`--langVersion <VERSION_NUMBER>`**

  Define a `LangVersion` propriedade no arquivo de projeto criado. Por exemplo, use `--langVersion 7.3` para usar C# 7.3.

  Para obter uma lista de versões padrão do C#, consulte [padrões](../../csharp/language-reference/configure-language-version.md#defaults).

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

***

### <a name="worker-grpc"></a><a name="web-others"></a>trabalho, grpc

- **`-f|--framework <FRAMEWORK>`**

  Especifica a [estrutura](../../standard/frameworks.md) a ser direcionada. O valor padrão é `netcoreapp3.1`. Disponível desde o SDK do .NET Core 3,1.

- **`--exclude-launch-settings`**

  Exclui *launchSettings.js* do modelo gerado.

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

***

### <a name="mstest-xunit"></a><a name="test"></a>MSTest, xUnit

- **`-f|--framework <FRAMEWORK>`**

  Especifica a [estrutura](../../standard/frameworks.md) a ser direcionada. Opção disponível desde o SDK do .NET Core 3,0.

  A tabela a seguir lista os valores padrão de acordo com o número de versão do SDK que você está usando:

  | Versão do SDK | Valor padrão   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`-p|--enable-pack`**

  Habilita o empacotamento para o projeto usando o [pacote dotnet](dotnet-pack.md).

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

***

### <a name="nunit"></a>NUnit

- **`-f|--framework <FRAMEWORK>`**

  Especifica a [estrutura](../../standard/frameworks.md) a ser direcionada.

  A tabela a seguir lista os valores padrão de acordo com o número de versão do SDK que você está usando:

  | Versão do SDK | Valor padrão   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.2         | `netcoreapp2.2` |
  | 2.1         | `netcoreapp2.1` |

- **`-p|--enable-pack`**

  Habilita o empacotamento para o projeto usando o [pacote dotnet](dotnet-pack.md).

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

***

### <a name="page"></a>página

- **`-na|--namespace <NAMESPACE_NAME>`**

  Namespace para o código gerado. O valor padrão é `MyApp.Namespace`.

- **`-np|--no-pagemodel`**

  Cria a página sem um PageModel.

***

### <a name="viewimports-proto"></a><a name="namespace"></a>viewimports, proto

- **`-na|--namespace <NAMESPACE_NAME>`**

  Namespace para o código gerado. O valor padrão é `MyApp.Namespace`.

***

### <a name="blazorserver"></a>blazorserver

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  O tipo de autenticação a ser usado. Os valores possíveis são:

  - `None` – Nenhuma autenticação (Padrão).
  - `Individual` – Autenticação individual.
  - `IndividualB2C` – Autenticação individual com o Azure AD B2C.
  - `SingleOrg` – Autenticação organizacional para um único locatário.
  - `MultiOrg` – Autenticação organizacional para vários locatários.
  - `Windows` – Autenticação do Windows.

- **`--aad-b2c-instance <INSTANCE>`**

  A instância de Azure Active Directory B2C à qual se conectar. Use com a autenticação do `IndividualB2C`. O valor padrão é `https://login.microsoftonline.com/tfp/`.

- **`-ssp|--susi-policy-id <ID>`**

  A ID da política de entrada e inscrição deste projeto. Use com a autenticação do `IndividualB2C`.

- **`-rp|--reset-password-policy-id <ID>`**

  A ID da política de senha de redefinição para este projeto. Use com a autenticação do `IndividualB2C`.

- **`-ep|--edit-profile-policy-id <ID>`**

  A ID de política de perfil de edição deste projeto. Use com a autenticação do `IndividualB2C`.

- **`--aad-instance <INSTANCE>`**

  A instância de Azure Active Directory à qual se conectar. Use com a autenticação `SingleOrg` ou `MultiOrg`. O valor padrão é `https://login.microsoftonline.com/`.

- **`--client-id <ID>`**

  A ID do cliente para este projeto. Use com a autenticação `IndividualB2C`, `SingleOrg` ou `MultiOrg`. O valor padrão é `11111111-1111-1111-11111111111111111`.

- **`--domain <DOMAIN>`**

  O domínio para o locatário do diretório. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `qualified.domain.name`.

- **`--tenant-id <ID>`**

  A ID de Tenantid do diretório ao qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `22222222-2222-2222-2222-222222222222`.

- **`--callback-path <PATH>`**

  O caminho de solicitação dentro do caminho base do aplicativo do URI de redirecionamento. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `/signin-oidc`.

- **`-r|--org-read-access`**

  Permite que este aplicativo Leia o acesso ao diretório. Aplicável apenas à autenticação `SingleOrg` ou `MultiOrg`.

- **`--exclude-launch-settings`**

  Exclui *launchSettings.js* do modelo gerado.

- **`--no-https`**

  Desativa o HTTPS. Essa opção só se aplica se `Individual` , `IndividualB2C` , `SingleOrg` ou `MultiOrg` não estiverem sendo usados para `--auth` .

- **`-uld|--use-local-db`**

  Especifica LocalDB deve ser usado em vez do SQLite. Aplicável apenas à autenticação `Individual` ou `IndividualB2C`.

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

***

### <a name="web"></a>web

- **`--exclude-launch-settings`**

  Exclui *launchSettings.js* do modelo gerado.

- **`-f|--framework <FRAMEWORK>`**

  Especifica a [estrutura](../../standard/frameworks.md) a ser direcionada. Opção não disponível no SDK do .NET Core 2,2.

  A tabela a seguir lista os valores padrão de acordo com o número de versão do SDK que você está usando:

  | Versão do SDK | Valor padrão   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.1` |

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

- **`--no-https`**

  Desativa o HTTPS.

***

### <a name="mvc-webapp"></a><a name="web-options"></a>MVC, webapp

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  O tipo de autenticação a ser usado. Os valores possíveis são:

  - `None` – Nenhuma autenticação (Padrão).
  - `Individual` – Autenticação individual.
  - `IndividualB2C` – Autenticação individual com o Azure AD B2C.
  - `SingleOrg` – Autenticação organizacional para um único locatário.
  - `MultiOrg` – Autenticação organizacional para vários locatários.
  - `Windows` – Autenticação do Windows.

- **`--aad-b2c-instance <INSTANCE>`**

  A instância de Azure Active Directory B2C à qual se conectar. Use com a autenticação do `IndividualB2C`. O valor padrão é `https://login.microsoftonline.com/tfp/`.

- **`-ssp|--susi-policy-id <ID>`**

  A ID da política de entrada e inscrição deste projeto. Use com a autenticação do `IndividualB2C`.

- **`-rp|--reset-password-policy-id <ID>`**

  A ID da política de senha de redefinição para este projeto. Use com a autenticação do `IndividualB2C`.

- **`-ep|--edit-profile-policy-id <ID>`**

  A ID de política de perfil de edição deste projeto. Use com a autenticação do `IndividualB2C`.

- **`--aad-instance <INSTANCE>`**

  A instância de Azure Active Directory à qual se conectar. Use com a autenticação `SingleOrg` ou `MultiOrg`. O valor padrão é `https://login.microsoftonline.com/`.

- **`--client-id <ID>`**

  A ID do cliente para este projeto. Use com a autenticação `IndividualB2C`, `SingleOrg` ou `MultiOrg`. O valor padrão é `11111111-1111-1111-11111111111111111`.

- **`--domain <DOMAIN>`**

  O domínio para o locatário do diretório. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `qualified.domain.name`.

- **`--tenant-id <ID>`**

  A ID de Tenantid do diretório ao qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `22222222-2222-2222-2222-222222222222`.

- **`--callback-path <PATH>`**

  O caminho de solicitação dentro do caminho base do aplicativo do URI de redirecionamento. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `/signin-oidc`.

- **`-r|--org-read-access`**

  Permite que este aplicativo Leia o acesso ao diretório. Aplicável apenas à autenticação `SingleOrg` ou `MultiOrg`.

- **`--exclude-launch-settings`**

  Exclui *launchSettings.js* do modelo gerado.

- **`--no-https`**

  Desativa o HTTPS. Essa opção é aplicável somente se `Individual`, `IndividualB2C`, `SingleOrg` ou `MultiOrg` não estão sendo usados.

- **`-uld|--use-local-db`**

  Especifica LocalDB deve ser usado em vez do SQLite. Aplicável apenas à autenticação `Individual` ou `IndividualB2C`.

- **`-f|--framework <FRAMEWORK>`**

  Especifica a [estrutura](../../standard/frameworks.md) a ser direcionada. Opção disponível desde o SDK do .NET Core 3,0.

  A tabela a seguir lista os valores padrão de acordo com o número de versão do SDK que você está usando:

  | Versão do SDK | Valor padrão   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

- **`--use-browserlink`**

  Inclui BrowserLink no projeto. Opção não disponível no SDK do .NET Core 2,2 e 3,1.

- **`-rrc|--razor-runtime-compilation`**

  Determina se o projeto está configurado para usar a [compilação de tempo de execução do Razor](/aspnet/core/mvc/views/view-compilation#runtime-compilation) em compilações de depuração. Opção disponível desde o SDK do 3.1.201 do .NET Core.

***

### <a name="angular-react"></a><a name="spa"></a>angular, reagir

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  O tipo de autenticação a ser usado. Disponível desde o SDK do .NET Core 3.0.
  
  Os valores possíveis são:

  - `None` – Nenhuma autenticação (Padrão).
  - `Individual` – Autenticação individual.

- **`--exclude-launch-settings`**

  Exclui *launchSettings.js* do modelo gerado.

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

- **`--no-https`**

  Desativa o HTTPS. Essa opção se aplica somente se a autenticação for `None` .

- **`-uld|--use-local-db`**

  Especifica LocalDB deve ser usado em vez do SQLite. Aplicável apenas à autenticação `Individual` ou `IndividualB2C`. Disponível desde o SDK do .NET Core 3.0.

- **`-f|--framework <FRAMEWORK>`**

  Especifica a [estrutura](../../standard/frameworks.md) a ser direcionada. Opção não disponível no SDK do .NET Core 2,2.

  A tabela a seguir lista os valores padrão de acordo com o número de versão do SDK que você está usando:

  | Versão do SDK | Valor padrão   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.0` |

***

### <a name="reactredux"></a>reactredux

- **`--exclude-launch-settings`**

  Exclui *launchSettings.js* do modelo gerado.

- **`-f|--framework <FRAMEWORK>`**

  Especifica a [estrutura](../../standard/frameworks.md) a ser direcionada. Opção não disponível no SDK do .NET Core 2,2.

  A tabela a seguir lista os valores padrão de acordo com o número de versão do SDK que você está usando:

  | Versão do SDK | Valor padrão   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.0` |

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

- **`--no-https`**

  Desativa o HTTPS.

***

### <a name="razorclasslib"></a>razorclasslib

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

- **`-s|--support-pages-and-views`**

  Dá suporte à adição de páginas e exibições do Razor tradicionais, além dos componentes dessa biblioteca. Disponível desde o SDK do .NET Core 3.0.

***
  
### <a name="webapi"></a>webapi

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  O tipo de autenticação a ser usado. Os valores possíveis são:

  - `None` – Nenhuma autenticação (Padrão).
  - `IndividualB2C` – Autenticação individual com o Azure AD B2C.
  - `SingleOrg` – Autenticação organizacional para um único locatário.
  - `Windows` – Autenticação do Windows.

- **`--aad-b2c-instance <INSTANCE>`**

  A instância de Azure Active Directory B2C à qual se conectar. Use com a autenticação do `IndividualB2C`. O valor padrão é `https://login.microsoftonline.com/tfp/`.

- **`-ssp|--susi-policy-id <ID>`**

  A ID da política de entrada e inscrição deste projeto. Use com a autenticação do `IndividualB2C`.

- **`--aad-instance <INSTANCE>`**

  A instância de Azure Active Directory à qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `https://login.microsoftonline.com/`.

- **`--client-id <ID>`**

  A ID do cliente para este projeto. Use com a autenticação `IndividualB2C` ou `SingleOrg`. O valor padrão é `11111111-1111-1111-11111111111111111`.

- **`--domain <DOMAIN>`**

  O domínio para o locatário do diretório. Use com a autenticação `IndividualB2C` ou `SingleOrg`. O valor padrão é `qualified.domain.name`.

- **`--tenant-id <ID>`**

  A ID de Tenantid do diretório ao qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `22222222-2222-2222-2222-222222222222`.

- **`-r|--org-read-access`**

  Permite que este aplicativo Leia o acesso ao diretório. Aplica-se somente à `SingleOrg` autenticação.

- **`--exclude-launch-settings`**

  Exclui *launchSettings.js* do modelo gerado.

- **`--no-https`**

  Desativa o HTTPS. `app.UseHsts` e `app.UseHttpsRedirection` não são adicionados ao `Startup.Configure`. Essa opção só se aplica se `IndividualB2C` ou `SingleOrg` não estiver sendo usada para autenticação.

- **`-uld|--use-local-db`**

  Especifica LocalDB deve ser usado em vez do SQLite. Aplica-se somente à `IndividualB2C` autenticação.

- **`-f|--framework <FRAMEWORK>`**

  Especifica a [estrutura](../../standard/frameworks.md) a ser direcionada. Opção não disponível no SDK do .NET Core 2,2.

  A tabela a seguir lista os valores padrão de acordo com o número de versão do SDK que você está usando:

  | Versão do SDK | Valor padrão   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.1` |

- **`--no-restore`**

  Não executa uma restauração implícita durante a criação do projeto.

***

### <a name="globaljson"></a>globaljson

- **`--sdk-version <VERSION_NUMBER>`**

  Especifica a versão do SDK do .NET Core a ser usada na *global.jsno* arquivo.

***

## <a name="examples"></a>Exemplos

- Crie um projeto de aplicativo de console em C# especificando o nome do modelo:

  ```dotnetcli
  dotnet new "Console Application"
  ```

- Crie um projeto de aplicativo de console F# no diretório atual:

  ```dotnetcli
  dotnet new console -lang "F#"
  ```

- Crie um projeto de biblioteca de classe .NET Standard no diretório especificado:

  ```dotnetcli
  dotnet new classlib -lang VB -o MyLibrary
  ```

- Crie um projeto de MVC em C# do ASP.NET Core no diretório atual sem autenticação:

  ```dotnetcli
  dotnet new mvc -au None
  ```

- Crie um projeto de xUnit:

  ```dotnetcli
  dotnet new xunit
  ```

- Listar todos os modelos disponíveis para modelos de aplicativo de página única (SPA):

  ```dotnetcli
  dotnet new spa -l
  ```

- Liste todos os modelos que correspondem à substring *we*. Nenhuma correspondência exata é encontrada, portanto, a correspondência de substring é executada em relação tanto ao nome curto quanto às colunas de nome.

  ```dotnetcli
  dotnet new we -l
  ```

- Tentativa de invocar o modelo correspondente a *ng*. Se não for possível determinar uma única correspondência, liste os modelos que são correspondências parciais.

  ```dotnetcli
  dotnet new ng
  ```

- Instale a versão 2,0 dos modelos SPA para ASP.NET Core:

  ```dotnetcli
  dotnet new -i Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
  ```

- Liste os modelos instalados e os detalhes sobre eles, incluindo como desinstalá-los:

  ```dotnetcli
  dotnet new -u
  ```

- Crie um *global.jsno* diretório atual definindo a versão do SDK para 3.1.101:

  ```dotnetcli
  dotnet new globaljson --sdk-version 3.1.101
  ```

## <a name="see-also"></a>Veja também

- [Modelos personalizados para dotnet new](custom-templates.md)
- [Create a custom template for dotnet new](../tutorials/cli-templates-create-item-template.md) (Criar um modelo personalizado para dotnet new)
- [Repositório do GitHub de dotnet/dotnet-template-samples](https://github.com/dotnet/dotnet-template-samples)
- [Modelos disponíveis para dotnet new](https://github.com/dotnet/templating/wiki/Available-templates-for-dotnet-new)
