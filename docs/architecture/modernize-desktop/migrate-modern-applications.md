---
title: Migrando aplicativos de área de trabalho modernos
description: Tudo o que você precisa saber sobre o processo de migração para aplicativos de área de trabalho modernos.
ms.date: 05/12/2020
ms.openlocfilehash: a015b266dc5c36fcef38dad04b9f4f048ee5906a
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84446913"
---
# <a name="migrating-modern-desktop-applications"></a>Migrando aplicativos de área de trabalho modernos

Neste capítulo, estamos explorando os problemas e desafios mais comuns que você pode enfrentar ao migrar um aplicativo existente do .NET Framework para o .NET Core.

Um aplicativo de área de trabalho complexo não funciona isoladamente e precisa de algum tipo de interação com subsistemas que possam residir no computador local ou em um servidor remoto. Ele provavelmente precisará de algum tipo de banco de dados para se conectar como um armazenamento de persistência, seja local ou remotamente. Com a elevação de arquiteturas orientadas a serviços e da Internet, é comum ter seu aplicativo conectado a algum tipo de serviço que reside em um servidor remoto ou na nuvem. Talvez seja necessário acessar o sistema de arquivos da máquina para implementar algumas funcionalidades. Como alternativa, talvez você esteja usando uma parte da funcionalidade que reside dentro de um objeto COM fora do seu aplicativo, que é um cenário comum se, por exemplo, você estiver integrando assemblies do Office em seu aplicativo.

Além disso, há diferenças na superfície de API que é exposta pelo .NET Framework e pelo .NET Core, e alguns recursos que estão disponíveis em .NET Framework não estão disponíveis no .NET Core. Portanto, é importante saber e levá-los em conta ao planejar uma migração.

## <a name="configuration-files"></a>Arquivos de configuração

Os arquivos de configuração oferecem a possibilidade de armazenar conjuntos de propriedades que são lidas em tempo de execução e afetam o comportamento de nossos aplicativos, como onde localizar um banco de dados ou quantas vezes executar um loop. A beleza dessa técnica é que você pode modificar alguns aspectos do aplicativo sem a necessidade de recodificar e recompilar. Isso é útil quando, por exemplo, o mesmo código de aplicativo é executado em um ambiente de desenvolvimento com um determinado conjunto de valores de configuração e em produção com um diferente.

### <a name="configuration-on-net-framework"></a>Configuração no .NET Framework

Se você tiver um aplicativo de área de trabalho .NET Framework em funcionamento, é provável que você tenha um arquivo *app. config* acessado por meio da <xref:System.Configuration.AppSettingsSection> classe do `System.Configuration` namespace.

Dentro da infraestrutura de .NET Framework, há uma hierarquia de arquivos de configuração que herdam Propriedades de seus pais. Você pode encontrar um arquivo *Machine. config* que define muitas propriedades e seções de configuração que podem ser usadas ou substituídas em qualquer arquivo de configuração descendente.

### <a name="configuration-on-net-core"></a>Configuração no .NET Core

No mundo do .NET Core, não há nenhum arquivo *Machine. config* . E, embora você possa continuar a usar o <xref:System.Configuration> namespace antigo, você pode considerar a mudança para o moderno <xref:Microsoft.Extensions.Configuration> , que oferece um bom número de aprimoramentos.

A API de configuração dá suporte ao conceito de provedor de configuração, que define a fonte de dados a ser usada para carregar a configuração. Há diferentes tipos de provedores internos, como:

- Objetos do .NET na memória
- Arquivos INI
- Arquivos JSON
- Arquivos XML
- Argumentos de linha de comando
- Variáveis de ambiente
- Armazenamento de usuário criptografado

 Ou você pode criar o seu próprio.

A nova configuração permite uma lista de pares de nome-valor que podem ser agrupados em uma hierarquia de vários níveis. Qualquer valor armazenado é mapeado para uma cadeia de caracteres e há suporte de ligação interna que permite desserializar as configurações em um objeto POCO (objeto CLR antigo) personalizado.

O <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> objeto permite que você adicione quantos provedores de configuração talvez precisem para seu aplicativo, usando uma regra de precedência para resolver a preferência. Portanto, o último provedor adicionado em seu código substituirá os outros. Esse é um ótimo recurso para gerenciar ambientes diferentes para execução, já que você pode definir configurações diferentes para ambientes de desenvolvimento, teste e produção e gerenciá-los em uma única função dentro de seu código.

### <a name="migrating-configuration-files"></a>Migrando arquivos de configuração

Você pode continuar a usar seu arquivo XML app. config existente. No entanto, você pode aproveitar essa oportunidade para migrar sua configuração para se beneficiar de vários aprimoramentos feitos no .NET Core.

Para migrar de um estilo antigo *app. config* para um novo arquivo de configuração, você deve escolher entre um formato XML e um formato JSON.

Se você escolher XML, a conversão será simples. Como o conteúdo é o mesmo, basta renomear o arquivo *app. config* em um arquivo com a extensão XML. Em seguida, altere o código que faz referência a AppSettings para usar a `ConfigurationBuilder` classe. Essa alteração deve ser fácil.

Se você quiser usar um formato JSON e não quiser migrar manualmente, há uma ferramenta chamada [dotnet-config2json](https://www.nuget.org/packages/dotnet-config2json/) disponível no .NET Core que pode converter um arquivo *app. config* em um arquivo de configuração JSON.

Você também pode ter alguns problemas ao usar as seções de configuração que foram definidas no arquivo *Machine. config* . Por exemplo, considere a seguinte configuração:

```xml
<configuration>
    <system.diagnostics>
        <switches>
            <add name="General" value="4" />
        </switches>
        <trace autoflush="true" indentsize="2">
            <listeners>
                <add name="myListener"
                     type="System.Diagnostics.TextWriterTraceListener,
                           System, Version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
                     initializeData="MyListener.log"
                     traceOutputOptions="ProcessId, LogicalOperationStack, Timestamp, ThreadId, Callstack, DateTime" />
            </listeners>
        </trace>
    </system.diagnostics>
</configuration>
```

Se você usar essa configuração para um .NET Core, obterá uma exceção:

Seção de configuração não reconhecida System. Diagnostics

Essa exceção ocorre porque essa seção e o assembly responsável por lidar com essa seção foram definidos no arquivo *Machine. config* , que agora não existe.

Para corrigir o problema com facilidade, você pode copiar a definição da seção de seu *Machine. config* antigo para o novo arquivo de configuração:

```xml
<configSections>
    <section name="system.diagnostics"
             type="System.Diagnostics.SystemDiagnosticsSection,
                   System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>
</configSections>
```

## <a name="accessing-databases"></a>Acessando bancos de dados

Quase todos os aplicativos da área de trabalho precisam de algum tipo de banco de dados. Para área de trabalho, é comum encontrar arquiteturas de cliente-servidor com uma conexão direta entre o aplicativo de desktop e o mecanismo de banco de dados. Esses bancos de dados podem ser locais ou remotos, dependendo da necessidade de compartilhar informações entre usuários diferentes.

Da perspectiva do código, houve muitas tecnologias e estruturas para dar ao desenvolvedor a possibilidade de conectar, consultar e atualizar um banco de dados.

Os exemplos mais comuns de banco de dados que você pode encontrar ao falar sobre o aplicativo da área de trabalho do Windows são o Microsoft Access e o Microsoft SQL Server. Se você tiver mais de 20 anos de programação de experiência para a área de trabalho, nomes como ODBC, OLEDB, RDO, ADO, ADO.NET, LINQ e Entity Framework parecerão familiares.

### <a name="odbc"></a>ODBC

Você pode continuar a usar o ODBC no .NET Core, uma vez que a Microsoft está fornecendo a `System.Data.Odbc` biblioteca compatível com o .NET Standard 2,0.

### <a name="ole-db"></a>OLE DB

[OLE DB](https://docs.microsoft.com/previous-versions/windows/desktop/ms722784(v=vs.85))   tem sido uma ótima maneira de acessar várias fontes de dados de maneira uniforme. Mas foi baseado em COM, que é uma tecnologia somente Windows, e como tal não era a melhor opção para uma tecnologia de plataforma cruzada, como o .NET Core. Também não há suporte no SQL Server versões 2014 e posteriores. Por esses motivos, o OLE DB não terá suporte do .NET Core.

### <a name="adonet"></a>ADO.NET

Você ainda pode usar o ADO.NET do código da área de trabalho existente no .NET Core. Você só precisa atualizar alguns pacotes NuGet.

### <a name="ef-core-vs-ef6"></a>EF Core versus EF6

Há duas versões atualmente com suporte do Entity Framework (EF), Entity Framework 6 (EF6) e EF Core.

A última tecnologia lançada como parte do .NET Framework World é Entity Framework, com 6,4 sendo a versão mais recente. Com o lançamento do .NET Core, a Microsoft também lançou uma nova pilha de acesso a dados com base em Entity Framework e chamou Entity Framework Core.

Você pode usar o EF 6,4 e o EF Core do .NET Framework e do .NET Core. Então, quais são os drivers de decisão para ajudar a decidir entre os dois?

O EF 6,3 é a primeira versão do EF6 que pode ser executada no .NET Core e no trabalho entre plataformas. Na verdade, a principal meta dessa versão era tornar mais fácil a migração de aplicativos existentes que usam o EF6 para o .NET Core.

O EF Core foi criado para fornecer uma experiência de desenvolvedor semelhante ao EF6. A maioria das APIs de nível superior continua igual, portanto o EF Core soará familiar para os desenvolvedores que usaram o EF6.

Embora sejam compatíveis, há diferenças na implementação que você deve verificar antes de tomar uma decisão.
Para obter mais informações, consulte [comparar EF Core & EF6](/ef/efcore-and-ef6/).

A recomendação é usar EF Core se:

* O aplicativo precisa das funcionalidades do .NET Core.
* O EF Core dá suporte a todos os recursos que o aplicativo requer.

Considere usar o EF6 se as duas condições a seguir forem verdadeiras:

* O aplicativo será executado no Windows e .NET Framework 4,0 ou posterior.
* O EF6 é compatível com todos os recursos exigidos pelo aplicativo.

### <a name="relational-databases"></a>Bancos de dados relacionais

#### <a name="sql-server"></a>SQL Server

A SQL Server foi um dos bancos de dados escolhidos se você estivesse desenvolvendo para a área de trabalho há alguns anos. Com o uso de <xref:System.Data.SqlClient> no .NET Framework, você pode acessar versões do SQL Server, que encapsulam protocolos específicos do banco de dados.

No .NET Core, você pode encontrar uma nova `SqlClient` classe, totalmente compatível com aquela existente no .NET Framework, mas localizada na <xref:Microsoft.Data.SqlClient> biblioteca. Basta adicionar uma referência ao pacote NuGet [Microsoft. Data. SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/) e fazer alguns renomear para os namespaces e tudo deve funcionar conforme o esperado.

#### <a name="microsoft-access"></a>Microsoft Access

O Microsoft Access foi usado há anos em que a SQL Server sofisticada e mais escalonável não era necessária. Você ainda pode se conectar ao Microsoft Access usando a <xref:System.Data.Odbc> biblioteca do.

## <a name="consuming-services"></a>Consumindo serviços

Com o aumento das arquiteturas orientadas a serviços, os aplicativos da área de trabalho começaram a evoluir de um modelo cliente-servidor para a abordagem de três camadas. Na abordagem de cliente-servidor, uma conexão de banco de dados direta é estabelecida do cliente que contém a lógica de negócios geralmente dentro de um único arquivo EXE. Por outro lado, a abordagem de três camadas estabelece uma camada de serviço intermediária que implementa a lógica de negócios e o acesso ao banco de dados, permitindo melhor segurança, escalabilidade e reutilização. Em vez de trabalhar diretamente com conjuntos de dados, a abordagem de camada depende de um conjunto de serviços que implementam contratos e tipos de objetos como uma maneira de implementar a transferência de dados.

Se você tiver um aplicativo de desktop usando um serviço WCF e quiser migrá-lo para o .NET Core, haverá algumas coisas a serem consideradas.

A primeira coisa é como resolver a configuração para acessar o serviço. Como a configuração é diferente no .NET Core, você precisará fazer algumas atualizações em seu arquivo de configuração.

Em segundo lugar, você precisará regenerar o cliente de serviço com as novas ferramentas presentes no Visual Studio 2019. Nesta etapa, você deve considerar a ativação da geração das operações síncronas para tornar o cliente compatível com seu código existente.

Após a migração, se você achar que há bibliotecas necessárias que não estão presentes no .NET Core, você pode adicionar uma referência ao pacote NuGet [Microsoft. Windows. Compatibility](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) e ver se as funções ausentes estão lá.

Se você estiver usando a <xref:System.Net.WebRequest> classe para executar chamadas de serviço Web, poderá encontrar algumas diferenças no .NET Core. A recomendação é usar o System .net. http. HttpClient em vez disso.

## <a name="consuming-a-com-object"></a>Consumindo um objeto COM

Atualmente, não há como adicionar uma referência a um objeto COM do Visual Studio 2019 para usar com o .NET Core. Portanto, você precisa modificar manualmente o arquivo de projeto.

Insira uma `COMReference` estrutura dentro do arquivo de projeto, como no exemplo a seguir:

```xml
<ItemGroup>
    <COMReference Include="MSHTML">
        <Guid>{3050F1C5-98B5-11CF-BB82-00AA00BDCE0B}\</Guid>
        <VersionMajor>4</VersionMajor>
        <VersionMinor>0</VersionMinor>
        <Lcid>0</Lcid>
        <WrapperTool>primary</WrapperTool>
        <Isolated>false</Isolated>
    </COMReference>
</ItemGroup>
```

## <a name="more-things-to-consider"></a>Mais coisas a serem consideradas

Várias tecnologias disponíveis para .NET Framework bibliotecas não estão disponíveis para o .NET Core. Se o seu código depender de algumas dessas tecnologias, considere as abordagens alternativas descritas nesta seção.

O [pacote de compatibilidade do Windows](../../core/porting/windows-compat-pack.md) fornece acesso a APIs que estavam disponíveis anteriormente apenas para .NET Framework. Ele pode ser usado em projetos .NET Core e .NET Standard.

Para obter mais informações sobre a compatibilidade de API, você pode encontrar a documentação sobre alterações significativas e APIs preteridas/herdadas em <https://docs.microsoft.com/dotnet/core/compatibility/fx-core> .

### <a name="appdomains"></a>AppDomains

Os domínios do aplicativo (AppDomains) isolam os aplicativos uns dos outros. Os AppDomains exigem suporte a tempo de execução e são caros. Não há suporte para a criação de domínios de aplicativo adicionais. Para isolamento de código, recomendamos como alternativa o uso de processos separados ou contêineres. Para o carregamento dinâmico de assemblies, recomendamos a nova  <xref:System.Runtime.Loader.AssemblyLoadContext> classe.

Para tornar a migração de código de .NET Framework mais fácil, o .NET Core expõe parte da superfície de API do AppDomain. Algumas das APIs normalmente funcionam (por exemplo,  <xref:System.AppDomain.UnhandledException?displayProperty=nameWithType> ), alguns membros não fazem nada (por exemplo,  <xref:System.AppDomain.SetCachePath%2A> ) e alguns deles lançam <xref:System.PlatformNotSupportedException> (por exemplo,  <xref:System.AppDomain.CreateDomain%2A> ).

### <a name="remoting"></a>Comunicação remota

O .NET Remoting foi usado para comunicação entre AppDomains, que não é mais suportada. Além disso, a Comunicação Remota exige suporte de runtime, o que é caro para manter. Por esses motivos, o .NET Remoting não tem suporte no .NET Core.

Para a comunicação entre processos, você deve considerar os mecanismos de comunicação entre processos (IPC) como uma alternativa à comunicação remota, como a <xref:System.IO.Pipes?displayProperty=nameWithType> ou a <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> classe.

Entre computadores, use uma solução baseada em rede como alternativa. Preferencialmente, use um protocolo de texto sem formatação de baixa sobrecarga, como HTTP. O servidor Web Kestrel, o servidor Web usado pelo ASP.NET Core, é uma opção.

### <a name="code-access-security-cas"></a>CAS (segurança de acesso ao código)

A área restrita, que se baseia no tempo de execução ou na estrutura para restringir quais recursos um aplicativo ou uma biblioteca gerenciada usa ou executa, não tem suporte no .NET Core.

Use limites de segurança que são fornecidos pelo sistema operacional, como virtualização, contêineres ou contas de usuário para executar processos com o conjunto mínimo de privilégios.

### <a name="security-transparency"></a>Transparência de Segurança

Assim como a CAS, a Transparência de Segurança separa o código em área restrita do código crítico de segurança de uma forma declarativa, mas não é mais permitida como um limite de segurança.

Use limites de segurança que são fornecidos pelo sistema operacional, como virtualização, contêineres ou contas de usuário para executar processos com o menor conjunto de privilégios.

>[!div class="step-by-step"]
>[Anterior](whats-new-dotnet-core.md ) 
> [Avançar](windows-migration.md)
