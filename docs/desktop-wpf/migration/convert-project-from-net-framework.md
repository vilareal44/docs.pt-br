---
title: Migrando aplicativos do WPF para o .NET Core 3,0
description: Saiba como migrar um aplicativo Windows Presentation Foundation (WPF) para o .NET Core 3,0.
author: mjrousos
ms.date: 09/12/2019
ms.author: mikerou
ms.openlocfilehash: fda4f618ddb4a3edbe6f2dd9fba0b10bc618e88d
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84201565"
---
# <a name="migrating-wpf-apps-to-net-core"></a>Migrando aplicativos do WPF para o .NET Core

Este artigo aborda as etapas necessárias para migrar um aplicativo Windows Presentation Foundation (WPF) do .NET Framework para o .NET Core 3,0. Se você não tiver um aplicativo WPF disponível para a porta, mas quiser experimentar o processo, poderá usar o aplicativo de exemplo do **Bean de Trader** disponível no [GitHub](https://github.com/dotnet/windows-desktop/tree/master/Samples/BeanTrader). O aplicativo original (direcionamento .NET Framework 4.7.2) está disponível na pasta NetFx\BeanTraderClient. Primeiro, explicaremos as etapas necessárias para a porta de aplicativos em geral e, em seguida, percorreremos as alterações específicas que se aplicam ao exemplo do **Bean de Trader** .

[!INCLUDE [desktop guide under construction](../../../includes/desktop-guide-preview-note.md)]

Para migrar para o .NET Core, você deve primeiro:

01. Entenda e atualize as dependências do NuGet:

    01. Atualize as dependências do NuGet para usar o `<PackageReference>` formato.
    01. Examine as dependências do NuGet de nível superior para o .NET Core ou a compatibilidade .NET Standard.
    01. Atualize os pacotes NuGet para versões mais recentes.
    01. Use o [.net Portability Analyzer](../../standard/analyzers/portability-analyzer.md) para entender as dependências do .net.

01. Migre o arquivo de projeto para o novo formato de estilo de SDK:

    01. Escolha se deseja direcionar tanto o .NET Core quanto o .NET Framework ou somente o .NET Core.
    01. Copie as propriedades e os itens de arquivo de projeto relevantes para o novo arquivo de projeto.

01. Corrigir problemas de compilação:

    01. Adicione uma referência ao pacote [Microsoft. Windows. Compatibility](https://www.nuget.org/packages/Microsoft.Windows.Compatibility/) .
    01. Encontre e corrija diferenças no nível da API.
    01. Remova as seções *app. config* que não sejam `appSettings` ou `connectionStrings` .
    01. Gere novamente o código gerado, se necessário.

01. Teste de tempo de execução:

    01. Confirme se o aplicativo portado funciona conforme o esperado.
    01. Cuidado com <xref:System.NotSupportedException> exceções.

## <a name="about-the-sample"></a>Sobre o exemplo

Este artigo faz referência ao [aplicativo de exemplo do Bean Trader](https://github.com/dotnet/windows-desktop/tree/master/Samples/BeanTrader) porque ele usa uma variedade de dependências semelhantes àquelas que os aplicativos do WPF do mundo real podem ter. O aplicativo não é grande, mas deve ser um Step up de ' Olá, Mundo ' em termos de complexidade. O aplicativo demonstra alguns problemas que os usuários podem encontrar ao portar aplicativos reais. O aplicativo se comunica com um serviço WCF, portanto, para que ele seja executado corretamente, você também precisará executar o projeto BeanTraderServer (disponível no mesmo repositório GitHub) e verificar se a configuração do BeanTraderClient aponta para o ponto de extremidade correto. (Por padrão, o exemplo supõe que o servidor esteja em execução no mesmo computador em `http://localhost:8090` , que será verdadeiro se você iniciar o BeanTraderServer localmente.)

Tenha em mente que este aplicativo de exemplo destina-se a demonstrar os desafios e as soluções de portabilidade do .NET Core. Não se destina a demonstrar as práticas recomendadas do WPF. Na verdade, ele inclui deliberadamente alguns antipadrões para garantir que você se deparasse com pelo menos alguns desafios interessantes ao portar.

## <a name="getting-ready"></a>Preparando-se

O principal desafio de migrar um aplicativo .NET Framework para o .NET Core é que suas dependências podem funcionar de maneira diferente ou não. A migração é muito mais fácil do que costumava ser; muitos pacotes NuGet agora têm como destino .NET Standard. A partir do .NET Core 2,0, as áreas de superfície .NET Framework e .NET Core se tornaram semelhantes. Mesmo assim, algumas diferenças (tanto no suporte de pacotes NuGet quanto em APIs .NET disponíveis) permanecem. A primeira etapa da migração é revisar as dependências do aplicativo e verificar se as referências estão em um formato que é facilmente migrado para o .NET Core.

### <a name="upgrade-to-packagereference-nuget-references"></a>Atualizar para `<PackageReference>` referências do NuGet

Os projetos .NET Framework mais antigos normalmente listam suas dependências do NuGet em um arquivo *Packages. config* . O novo formato de arquivo de projeto no estilo SDK referencia pacotes NuGet como [`<PackageReference>`](/nuget/consume-packages/package-references-in-project-files) elementos no próprio arquivo csproj em vez de em um arquivo de configuração separado.

Ao migrar, há duas vantagens em relação ao uso `<PackageReference>` de referências de estilo:

- Esse é o estilo da referência do NuGet que é necessário para o novo arquivo de projeto do .NET Core. Se você já estiver usando `<PackageReference>` o, os elementos do arquivo de projeto poderão ser copiados e colados diretamente no novo projeto.
- Ao contrário de um arquivo Packages. config, `<PackageReference>` os elementos referem-se apenas às dependências de nível superior das quais seu projeto depende diretamente. Todos os outros pacotes NuGet transitivos serão determinados no momento da restauração e registrados no arquivo obj\project.assets.JSON gerado automaticamente. Isso torna muito mais fácil determinar quais dependências seu projeto tem, o que é útil ao determinar se as dependências necessárias funcionarão no .NET Core ou não.

A primeira etapa para migrar um aplicativo .NET Framework para o .NET Core é atualizá-lo para usar `<PackageReference>` referências do NuGet. O Visual Studio torna isso simples. Basta clicar com o botão direito do mouse no arquivo *Packages. config* do projeto no **Gerenciador de soluções**do Visual Studio e selecionar **Migrate Packages. config para PackageReference**.

![Atualizando para o PackageReference](./media/convert-project-from-net-framework/package-reference-migration.png)

Uma caixa de diálogo é exibida mostrando dependências do NuGet de nível superior calculadas e perguntando quais outros pacotes NuGet devem ser promovidos para o nível superior. Nenhum desses outros pacotes precisa ser de nível superior para o exemplo do bean de Trader, para que você possa desmarcar todas essas caixas. Em seguida, clique em **OK** e o arquivo *Packages. config* é removido e `<PackageReference>` os elementos são adicionados ao arquivo do projeto.

`<PackageReference>`-as referências de estilo não armazenam pacotes NuGet localmente em uma pasta de pacotes. Em vez disso, eles são armazenados globalmente como uma otimização. Após a conclusão da migração, edite o arquivo csproj e remova todos os `<Analyzer>` elementos referentes aos analisadores que vieram anteriormente do *.. diretório \Packages* Não se preocupe; como você ainda tem as referências do pacote NuGet, os analisadores serão incluídos no projeto. Você só precisa limpar os elementos antigos Packages. config-Style `<Analyzer>` .

### <a name="review-nuget-packages"></a>Examinar pacotes NuGet

Agora que você pode ver os pacotes NuGet de nível superior dos quais o projeto depende, é possível examinar se esses pacotes estão disponíveis no .NET Core. Você pode determinar se um pacote dá suporte ao .NET Core examinando suas dependências em [NuGet.org](https://www.nuget.org/). O site [fuget.org](https://www.fuget.org/) criado pela Comunidade mostra essas informações em destaque na parte superior da página de informações do pacote.

Ao direcionar o .NET Core 3,0, todos os pacotes destinados ao .NET Core ou .NET Standard devem funcionar (já que o .NET Core implementa a área de superfície de .NET Standard). Em alguns casos, a versão específica de um pacote que é usada não é direcionada ao .NET Core ou .NET Standard, mas as versões mais recentes vão. Nesse caso, você deve considerar a atualização para a versão mais recente do pacote.

Você também pode usar pacotes destinados a .NET Framework, mas isso apresenta algum risco. O .NET Core para .NET Framework dependências é permitido porque as áreas de superfície do .NET Core e .NET Framework são semelhantes o suficiente para que essas dependências funcionem *frequentemente* . No entanto, se o pacote tentar usar uma API do .NET que não esteja presente no .NET Core, você encontrará uma exceção de tempo de execução. Por isso, você só deve referenciar .NET Framework pacotes quando nenhuma outra opção estiver disponível e entender que isso gerará uma carga de teste.

Se houver pacotes referenciados que não se destinam ao .NET Core ou .NET Standard, você terá que pensar em outras alternativas:

- Há outros pacotes semelhantes que podem ser usados em vez disso? Às vezes, os autores do NuGet publicam ' separados '. Core ' versões de suas bibliotecas especificamente direcionadas ao .NET Core. Os pacotes da Enterprise Library são um exemplo da publicação da Comunidade ". NetCore "alternativas. Em outros casos, os SDKs mais recentes para um serviço específico (às vezes com nomes de pacote diferentes) estão disponíveis para .NET Standard. Se não houver nenhuma alternativa disponível, você poderá continuar usando os pacotes direcionados .NET Framework, tendo em mente que precisará testá-los completamente uma vez em execução no .NET Core.

O exemplo do bean de Trader tem as seguintes dependências de NuGet de nível superior:

- [**Castle. Windsor, versão 4.1.1**](https://www.castleproject.org/projects/windsor/)  

  Este pacote se destina .NET Standard 1,6, então funciona no .NET Core.

- [**Microsoft. CodeAnalysis. FxCopAnalyzers, versão 2.6.3**](https://www.nuget.org/packages/Microsoft.CodeAnalysis.FxCopAnalyzers/2.6.3)  
  Esse é um meta-Package, portanto, não é imediatamente óbvio quais plataformas ele suporta, mas a [documentação](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisfxcopanalyzers) indica que sua versão mais recente (2.9.2) funcionará tanto para .NET Framework quanto para o .NET Core.

- [**Nito. AsyncEx, versão 4.0.1**](https://www.nuget.org/packages/Nito.AsyncEx/4.0.1)  

  Este pacote não tem como alvo o .NET Core, mas a versão 5,0 mais recente faz. Isso é comum ao migrar porque muitos pacotes NuGet adicionaram .NET Standard suporte recentemente, mas versões mais antigas do projeto só serão direcionadas .NET Framework. Se a diferença de versão for apenas uma diferença de versão secundária, geralmente será fácil atualizar para a versão mais recente. Como essa é uma alteração de versão principal, você precisa ter cuidado ao atualizar, pois pode haver alterações significativas no pacote. Há um caminho para frente, no entanto, que é bom.

- [**MahApps. metro, versão 1.6.5**](https://www.nuget.org/packages/MahApps.Metro/1.6.5)  

  Esse pacote também não tem como alvo o .NET Core, mas tem um pré-lançamento (2,0-Alpha) mais recente que o faz. Novamente, você precisa procurar alterações significativas, mas o pacote mais recente está incentivando.

As dependências do NuGet de exemplo do bean de Trader são todas direcionadas .NET Standard/. NET Core ou têm versões mais recentes que, portanto, é improvável que haja problemas de bloqueio aqui.

### <a name="upgrade-nuget-packages"></a>Atualizar pacotes NuGet

Se possível, seria bom atualizar as versões de todos os pacotes que só visam o .NET Core ou .NET Standard com versões mais recentes neste momento (com o projeto ainda destinado a .NET Framework) para descobrir e resolver quaisquer alterações significativas no início.

Se você preferir não fazer nenhuma alteração de material na versão de .NET Framework existente do aplicativo, isso poderá esperar até que você tenha um novo arquivo de projeto direcionado ao .NET Core. No entanto, atualizar os pacotes NuGet para versões compatíveis com o .NET Core antecipadamente torna o processo de migração ainda mais fácil depois de criar o novo arquivo de projeto e reduz o número de diferenças entre as versões .NET Framework e .NET Core do aplicativo.

Com o exemplo do bean de Trader, todas as atualizações necessárias podem ser feitas facilmente (usando o Gerenciador de pacotes NuGet do Visual Studio) com uma exceção: a atualização de **MahApps. metro 1.6.5** para **2,0** revela alterações significativas relacionadas às APIs de gerenciamento de tema e de ênfase.

O ideal é que o aplicativo fosse atualizado para usar a versão mais recente do pacote (já que isso é mais provável de funcionar no .NET Core). No entanto, em alguns casos, isso pode não ser viável. Nesses casos, não atualize o **MahApps. metro** porque as alterações necessárias não são triviais e este tutorial se concentra na migração para o .NET Core 3, não para o **MahApps. metro 2.** Além disso, essa é uma dependência .NET Framework de baixo risco porque o aplicativo do bean de Trader só exercita uma pequena parte do **MahApps. metro**. É claro que você precisará fazer testes para garantir que tudo esteja funcionando quando a migração for concluída. Se esse fosse um cenário do mundo real, seria bom arquivar um problema para acompanhar o trabalho para migrar para o **MahApps. metro** versão 2,0 já que não fazer a migração agora deixa por trás de uma dívida técnica.

Depois que os pacotes NuGet são atualizados para versões recentes, o `<PackageReference>` grupo de itens no arquivo de projeto de exemplo do bean de Trader deve ter esta aparência.

```xml
<ItemGroup>
  <PackageReference Include="Castle.Windsor">
    <Version>4.1.1</Version>
  </PackageReference>
  <PackageReference Include="MahApps.Metro">
    <Version>1.6.5</Version>
  </PackageReference>
  <PackageReference Include="Microsoft.CodeAnalysis.FxCopAnalyzers">
    <Version>2.9.2</Version>
  </PackageReference>
  <PackageReference Include="Nito.AsyncEx">
    <Version>5.0.0</Version>
  </PackageReference>
</ItemGroup>
```

### <a name="net-framework-portability-analysis"></a>Análise de portabilidade .NET Framework

Depois de entender o estado das dependências do NuGet do seu projeto, a próxima coisa a considerar é .NET Framework dependências de API. A ferramenta do [analisador de portabilidade .net](../../standard/analyzers/portability-analyzer.md) é útil para entender quais das APIs .NET que seu projeto usa estão disponíveis em outras plataformas .net.

A ferramenta é fornecida como um [plug-in do Visual Studio](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer), uma [ferramenta de linha de comando](https://github.com/Microsoft/dotnet-apiport/releases)ou encapsulado em uma [GUI simples](https://github.com/Microsoft/dotnet-apiport-ui), o que simplifica suas opções. Você pode ler mais sobre como usar o analisador de portabilidade .NET (porta de API) usando a GUI na postagem de blog [portando aplicativos da área de trabalho para o .NET Core](https://devblogs.microsoft.com/dotnet/porting-desktop-apps-to-net-core/) . Se você preferir usar a linha de comando, as etapas necessárias serão:

1. Baixe o [analisador de portabilidade do .net](https://github.com/Microsoft/dotnet-apiport/releases) se você ainda não o tiver.
1. Certifique-se de que o aplicativo .NET Framework que será compilado com êxito (essa é uma boa ideia antes da migração, independentemente).
1. Execute a porta de API com uma linha de comando como esta.

    ```console
    ApiPort.exe analyze -f <PathToBeanTraderBinaries> -r html -r excel -t ".NET Core"
    ```

    O `-f` argumento especifica o caminho que contém os binários a serem analisados. O `-r` argumento especifica qual formato de arquivo de saída você deseja. O `-t` argumento especifica em qual plataforma .net analisar o uso da API. Nesse caso, você deseja o .NET Core.

Quando você abrir o relatório HTML, a primeira seção listará todos os binários analisados e qual será a porcentagem das APIs do .NET que eles usam estão disponíveis na plataforma de destino. O percentual não é significativo por si só. O que é mais útil é ver as APIs específicas que estão faltando. Para fazer isso, selecione um nome de assembly ou role para baixo até os relatórios para assemblies individuais.

Concentre-se nos assemblies dos quais você possui o código-fonte. No relatório ApiPort do bean de Trader, por exemplo, há muitos binários listados, mas a maioria deles pertence a pacotes NuGet. `Castle.Windsor`mostra que depende de algumas APIs System. Web que estão ausentes no .NET Core. Isso não é uma preocupação porque você verificou anteriormente que `Castle.Windsor` dá suporte ao .NET Core. É comum que os pacotes NuGet tenham binários diferentes para uso com diferentes plataformas .NET, portanto, se a versão .NET Framework do `Castle.Windsor` usa APIs System. Web ou não é irrelevante, desde que o pacote também tenha como alvo .net Standard ou o .NET Core (o que ele faz).

Com o exemplo do bean de Trader, o único binário que você precisa considerar é **BeanTraderClient** e o relatório mostra que apenas duas APIs .net estão ausentes: `System.ServiceModel.ClientBase<T>.Close` e `System.ServiceModel.ClientBase<T>.Open` .

![Relatório de portabilidade BeanTraderClient](./media/convert-project-from-net-framework/portability-report.png)

É improvável que esses problemas sejam bloqueados porque as APIs de cliente do WCF são (principalmente) com suporte no .NET Core, portanto, deve haver alternativas disponíveis para essas APIs centrais. Na verdade, olhando para a `System.ServiceModel` área de superfície do .NET Core (usando <https://apisof.net> ), você vê que há alternativas assíncronas no .NET Core em vez disso.

Com base nesse relatório e na análise de dependência do NuGet anterior, parece que não deve haver problemas importantes ao migrar o exemplo do bean de Trader para o .NET Core. Você está pronto para a próxima etapa em que, na verdade, iniciará a migração.

## <a name="migrating-the-project-file"></a>Migrando o arquivo de projeto

Se seu aplicativo não estiver usando o novo [formato de arquivo de projeto no estilo SDK](../../core/tools/csproj.md), você precisará de um novo arquivo de projeto para o .NET Core de destino. Você pode substituir o arquivo csproj existente ou, se preferir manter o projeto existente inalterado em seu estado atual, você pode adicionar um novo arquivo csproj destinado ao .NET Core. Você pode criar versões do aplicativo para o .NET Framework e o .NET Core com um único arquivo de projeto em estilo SDK com [vários](../../standard/library-guidance/cross-platform-targeting.md) destinos (especificando várias `<TargetFrameworks>` metas).

Para criar o novo arquivo de projeto, você pode criar um novo projeto do WPF no Visual Studio ou usar o `dotnet new wpf` comando em um diretório temporário para gerar o arquivo de projeto e, em seguida, copiá-lo/renomeá-lo para o local correto. Também há uma ferramenta criada pela Comunidade, [CsprojToVs2017](https://github.com/hvanbakel/CsprojToVs2017), que pode automatizar parte da migração de arquivos do projeto. A ferramenta é útil, mas ainda precisa de um humano para revisar os resultados para garantir que todos os detalhes da migração estejam corretos. Uma área específica que a ferramenta não trata de forma ideal é migrar pacotes NuGet de arquivos *Packages. config* . Se a ferramenta for executada em um arquivo de projeto que ainda usa um arquivo *Packages. config* para referenciar os pacotes do NuGet, ele migrará para `<PackageReference>` os elementos automaticamente, mas adicionará `<PackageReference>` elementos para *todos* os pacotes em vez de apenas os de nível superior. Se você já migrou para `<PackageReference>` elementos com o Visual Studio (como fez neste exemplo), a ferramenta pode ajudar com o restante da conversão. Como Scott Hanselman recomenda em [sua postagem de blog sobre migração de arquivos csproj](https://www.hanselman.com/blog/UpgradingAnExistingNETProjectFilesToTheLeanNewCSPROJFormatFromNETCore.aspx), a portabilidade por mão é educativa e fornecerá melhores resultados se você tiver apenas alguns projetos para portar. Mas se você estiver portando dezenas ou centenas de arquivos de projeto, uma ferramenta como [CsprojToVs2017](https://github.com/hvanbakel/CsprojToVs2017) pode ser uma ajuda.

Para criar um novo arquivo de projeto para o exemplo do bean de Trader, execute `dotnet new wpf` em um diretório temporário e mova o arquivo *. csproj* gerado para a pasta *BeanTraderClient* e renomeie-o **BeanTraderClient. Core. csproj**.

Como o novo formato de arquivo de projeto inclui automaticamente arquivos C#, arquivos *resx* e arquivos XAML que ele encontra em ou sob seu diretório, o arquivo de projeto já está quase concluído! Para concluir a migração, abra os arquivos de projeto novos e antigos lado a lado e examine o antigo para ver se alguma informação que ele contém precisa ser migrada. No caso de exemplo do bean de Trader, os seguintes itens devem ser copiados para o novo projeto:

- As `<RootNamespace>` `<AssemblyName>` Propriedades, e `<ApplicationIcon>` devem ser todas copiadas.

- Você também precisa adicionar uma `<GenerateAssemblyInfo>false</GenerateAssemblyInfo>` propriedade ao novo arquivo de projeto, já que o exemplo do bean de Trader inclui atributos de nível de assembly (como `[AssemblyTitle]` ) em um arquivo AssemblyInfo.cs. Por padrão, os novos projetos no estilo SDK gerarão automaticamente esses atributos com base nas propriedades do arquivo csproj. Como você não quer que isso aconteça nesse caso (os atributos gerados automaticamente entrarão em conflito com aqueles de AssemblyInfo.cs), você desabilita os atributos gerados automaticamente com `<GenerateAssemblyInfo>` .

- Embora os arquivos *resx* sejam incluídos automaticamente como recursos incorporados, outros `<Resource>` itens, como imagens, não são. Portanto, copie os `<Resource>` elementos para inserir arquivos de ícone e imagem. Você pode simplificar as referências de png para uma única linha usando o suporte do novo formato de arquivo de projeto para padrões de mascaramento: `<Resource Include="**\*.png" />` .

- Da mesma forma, `<None>` os itens são incluídos automaticamente, mas não são copiados para o diretório de saída, por padrão. Como o projeto do bean de Trader inclui um `<None>` item que *é* copiado para o diretório de saída (usando `PreserveNewest` comportamentos), você precisa atualizar o item preenchido automaticamente `<None>` para esse arquivo, desta forma.

  ```xml
  <None Update="BeanTrader.pfx">
    <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
  </None>
  ```

- O exemplo do bean de Trader inclui um arquivo XAML (default. sotaque. XAML) como `Content` (e não como um `Page` ) porque os temas e acentos definidos nesse arquivo são carregados a partir do XAML do arquivo em tempo de execução, em vez de serem inseridos no próprio aplicativo. O novo sistema de projeto inclui automaticamente esse arquivo como um `<Page>` , no entanto, uma vez que é um arquivo XAML. Portanto, você precisa remover o arquivo XAML como uma página ( `<Page Remove="**\Default.Accent.xaml" />` ) e adicioná-lo como conteúdo.

  ```xml
  <Content Include="Resources\Themes\Default.Accent.xaml">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
  </Content>
  ```

- Por fim, adicione referências do NuGet copiando o `<ItemGroup>` com todos os `<PackageReference>` elementos. Se você não tiver atualizado anteriormente os pacotes do NuGet para versões compatíveis com o .NET Core, poderá fazer isso agora que as referências de pacote estão em um projeto específico do .NET Core.

Neste ponto, deve ser possível adicionar o novo projeto à solução BeanTrader e abri-lo no Visual Studio. O projeto deve parecer correto em **Gerenciador de soluções**e `dotnet restore BeanTraderClient.Core.csproj` deve restaurar os pacotes com êxito (com dois avisos esperados relacionados à versão MahApps. metro que você está usando para .NET Framework).

Embora seja possível manter ambos os arquivos de projeto lado a lado (e pode até mesmo ser desejável se você quiser continuar criando o projeto antigo exatamente como era), isso complica o processo de migração (os dois projetos tentarão usar as mesmas pastas bin e obj) e geralmente não é necessário. Se você quiser compilar tanto o .NET Core quanto os destinos do .NET Framework, poderá substituir a `<TargetFramework>netcoreapp3.0</TargetFramework>` propriedade no novo arquivo de projeto por `<TargetFrameworks>netcoreapp3.0;net472</TargetFrameworks>` em vez disso. Para o exemplo do bean de Trader, exclua o arquivo de projeto antigo (BeanTraderClient. csproj), pois ele não é mais necessário. Se você preferir manter ambos os arquivos de projeto, certifique-se de que eles sejam criados para caminhos de saída intermediário e de saída diferentes.

## <a name="fix-build-issues"></a>Corrigir problemas de compilação

A terceira etapa do processo de portabilidade é obter o projeto para compilar. Alguns aplicativos já serão compilados com êxito depois que o arquivo de projeto for convertido em um projeto em estilo SDK. Se esse for o caso para seu aplicativo, parabéns! Você pode ir para a etapa 4. Outros aplicativos precisarão de algumas atualizações para que sejam compiladas para o .NET Core. Se você tentar executar `dotnet build` no projeto de exemplo do Bean Trader agora, por exemplo, (ou compilá-lo no Visual Studio), haverá muitos erros, mas você os obterá corrigidos rapidamente.

### <a name="systemservicemodel-references-and-microsoftwindowscompatibility"></a>Referências de System. ServiceModel e Microsoft. Windows. Compatibility

Uma fonte comum de erros está faltando referências para APIs que estão disponíveis para o .NET Core, mas não são incluídas automaticamente no metapacote do aplicativo .NET Core. Para resolver isso, você deve fazer referência ao `Microsoft.Windows.Compatibility` pacote. O pacote de compatibilidade inclui um amplo conjunto de APIs que são comuns em aplicativos de área de trabalho do Windows, como WCF Client, serviços de diretório, registro, configuração, APIs de ACLs e muito mais.

Com o exemplo do bean de Trader, a maioria dos erros de compilação se deve a tipos ausentes <xref:System.ServiceModel> . Eles podem ser resolvidos referenciando os pacotes do WCF NuGet necessários. As APIs de cliente do WCF estão entre elas presentes no `Microsoft.Windows.Compatibility` pacote, mas, portanto, fazer referência ao pacote de compatibilidade é uma solução ainda melhor (já que ela também resolve quaisquer problemas relacionados a APIs, bem como soluções para os problemas do WCF disponibilizados pelo pacote de compatibilidade). O `Microsoft.Windows.Compatibility` pacote ajuda na maioria dos cenários de portabilidade do WPF e WinForms do .NET Core 3,0. Depois de adicionar a referência do NuGet a `Microsoft.Windows.Compatibility` , somente um erro de compilação permanece!

### <a name="cleaning-up-unused-files"></a>Limpando arquivos não utilizados

Um tipo de problema de migração que surge com freqüência é relacionado aos arquivos C# e XAML que não foram incluídos anteriormente no Build sendo coletados pelos novos projetos no estilo SDK que incluem *todas as* fontes automaticamente.

O próximo erro de compilação que você vê no exemplo do bean de Trader se refere a uma implementação de interface inadequada em *OldUnusedViewModel.cs*. O nome do arquivo é uma dica, mas, na inspeção, você descobrirá que esse arquivo de origem está incorreto. Isso não causava problemas anteriormente porque não foi incluído no projeto de .NET Framework original. Os arquivos de origem que estavam presentes no disco, mas não incluídos no *csproj* antigo, são incluídos automaticamente agora.

Para problemas únicos como esse, é fácil comparar com o *csproj* anterior para confirmar que o arquivo não é necessário e, em seguida, `<Compile Remove="" />` ou, se o arquivo de origem não for mais necessário em qualquer lugar, exclua-o. Nesse caso, é seguro excluir apenas *OldUnusedViewModel.cs*.

Se você tiver muitos arquivos de origem que precisariam ser excluídos dessa forma, você poderá desabilitar a inclusão automática de arquivos C# definindo a `<EnableDefaultCompileItems>` propriedade como false no arquivo de projeto. Em seguida, você pode copiar `<Compile Include>` itens do arquivo de projeto antigo para o novo a fim de Compilar apenas as fontes que pretende incluir. Da mesma forma, `<EnableDefaultPageItems>` pode ser usado para desativar a inclusão automática de páginas XAML e `<EnableDefaultItems>` pode controlar ambas com uma única propriedade.

### <a name="a-brief-aside-on-multi-pass-compilers"></a>Um breve lado sobre compiladores com várias passagens

Depois de remover o arquivo transgressor do exemplo do bean de Trader, você pode recompilar e receberá quatro erros. Você não tinha um antes? Por que o número de erros foi ativado? O compilador C# é um [compilador com várias passagens](https://docs.microsoft.com/archive/blogs/ericlippert/how-many-passes). Isso significa que ele passa por cada arquivo de origem duas vezes. Primeiro, o compilador apenas examina os metadados e as declarações em cada arquivo de origem e identifica quaisquer problemas no nível de declaração. Esses são os erros que você corrigiu. Em seguida, ele passa pelo código novamente para criar a origem do C# no IL; Esses são o segundo conjunto de erros que você está vendo agora.

> [!NOTE]
> O compilador C# faz [mais do que apenas duas passagens](https://docs.microsoft.com/archive/blogs/ericlippert/how-many-passes), mas o resultado final é que erros de compilador para grandes alterações de código como essa tendem a chegar em duas ondas.

### <a name="third-party-dependency-fixes-castlewindsor"></a>Correções de dependências de terceiros (Castle. Windsor)

Outra classe de problema que surge em alguns cenários de migração são as diferenças de API entre .NET Framework e as versões de dependências do .NET Core. Mesmo que um pacote NuGet tenha como alvo .NET Framework e .NET Standard ou .NET Core, pode haver diferentes bibliotecas para uso com destinos .NET diferentes. Isso permite que os pacotes ofereçam suporte a várias plataformas .NET diferentes, o que pode exigir implementações diferentes. Isso também significa que pode haver pequenas diferenças de API nas bibliotecas ao direcionar diferentes plataformas .NET.

O próximo conjunto de erros que você verá no exemplo do bean de Trader está relacionado a `Castle.Windsor` APIs. O projeto do Bean do .NET Core Traders usa a mesma versão do `Castle.Windsor` que o projeto de .NET Framework direcionado (4.1.1), mas as implementações dessas duas plataformas são um pouco diferentes.

Nesse caso, você verá os seguintes problemas que precisam ser corrigidos:

1. `Castle.MicroKernel.Registration.Classes.FromThisAssembly`Não está disponível no .NET Core. Há, no entanto, a API semelhante `Classes.FromAssemblyContaining` disponível, portanto, podemos substituir ambos os usos de `Classes.FromThisAssembly()` com chamadas para `Classes.FromAssemblyContaining(t)` , em que `t` é o tipo que faz a chamada.
1. Da mesma forma, em *bootstrapper.cs*, `Castle.Windsor.Installer.FromAssembly` . Isso não está disponível no .NET Core. Em vez disso, essa chamada pode ser substituída por `FromAssembly.Containing(typeof(Bootstrapper))` .

### <a name="updating-wcf-client-usage"></a>Atualizando o uso do cliente WCF

Depois de corrigir as `Castle.Windsor` diferenças, o último erro de compilação restante no projeto do Bean do .NET Core Traders é que `BeanTraderServiceClient` (derivado de `DuplexClientBase` ) não tem um `Open` método. Isso não é surpreendente, pois essa é uma API que foi realçada pelo .NET Portability Analyzer no início deste processo de migração. No `BeanTraderServiceClient` entanto, examinar o desenha nossa atenção para um problema maior. Este cliente WCF foi gerado automaticamente pela ferramenta [svcutil. exe](../../framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) .

**Os clientes WCF gerados por SvcUtil são destinados para uso em .NET Framework.**

As soluções que usam clientes WCF gerados por SvcUtil precisarão regenerar clientes compatíveis com .NET Standard para uso com o .NET Core. Um dos principais motivos pelos quais os clientes antigos não funcionarão é que eles dependem da configuração do aplicativo para definir associações e pontos de extremidade do WCF. Como .NET Standard APIs do WCF podem trabalhar em plataforma cruzada (onde as APIs de sistema. de configuração não estão disponíveis), os clientes WCF para .NET Core e cenários de .NET Standard devem definir associações e pontos de extremidade programaticamente em vez de na configuração.

Na verdade, qualquer uso de cliente WCF que dependa da `<system.serviceModel>` seção app. config (seja criado com SvcUtil ou manualmente) precisará ser alterado para funcionar no .NET Core.

Há duas maneiras de gerar automaticamente clientes WCF compatíveis com .NET Standard:

- A `dotnet-svcutil` ferramenta é uma ferramenta .NET que gera clientes WCF de uma maneira semelhante a como o SvcUtil funcionou anteriormente.
- O Visual Studio pode gerar clientes WCF usando a opção de [referência de serviço Web do WCF](../../core/additional-tools/wcf-web-service-reference-guide.md) de seu recurso de serviços conectados.

Qualquer uma das abordagens funciona bem. Como alternativa, é claro que você pode escrever o código do cliente WCF por conta própria. Para este exemplo, optei por usar o recurso serviço conectado do Visual Studio. Para fazer isso, clique com o botão direito do mouse no projeto *BeanTraderClient. Core* no Gerenciador de soluções do Visual Studio e selecione **Adicionar**  >  **serviço conectado**. Em seguida, escolha o provedor de referência do serviço Web WCF. Isso abrirá uma caixa de diálogo onde você pode especificar o endereço do serviço Web do bean de back-end ( `localhost:8080` se você estiver executando o servidor localmente) e o namespace que gerou os tipos deve usar (**BeanTrader. Service**, por exemplo).

![Caixa de diálogo serviço conectado de referência do serviço Web WCF](./media/convert-project-from-net-framework/connected-service-dialog.png)

Depois de selecionar o botão **concluir** , um novo nó serviços conectados é adicionado ao projeto e um arquivo Reference.cs é adicionado sob esse nó que contém o novo cliente WCF .net Standard para acessar o serviço do Bean do Trader. Se você examinar os `GetEndpointAddress` métodos ou `GetBindingForEndpoint` nesse arquivo, verá que as associações e os pontos de extremidade agora são gerados programaticamente (em vez de por meio da configuração do aplicativo). O recurso ' Adicionar serviços conectados ' também pode adicionar referências a alguns pacotes System. ServiceModel no arquivo de projeto, o que não é necessário, pois todos os pacotes WCF necessários estão incluídos por meio de Microsoft. Windows. Compatibility. Verifique o csproj para ver se algum item do System. ServiceModel extra `<PackageReference>` foi adicionado e, nesse caso, remova-os.

Nosso projeto tem novas classes de cliente WCF agora (em *Reference.cs*), mas ainda tem as antigas (em BeanTrader.cs). Há duas opções neste ponto:

- Se você quiser poder criar o projeto de .NET Framework original (juntamente com o novo .NET Core-Targeted One), poderá usar um `<Compile Remove="BeanTrader.cs" />` item no arquivo csproj do projeto do .NET Core para que as versões .NET Framework e .NET Core do aplicativo usem diferentes clientes WCF. Isso tem a vantagem de deixar o projeto de .NET Framework existente inalterado, mas tem a desvantagem de que o código que usa os clientes WCF gerados pode precisar ser um pouco diferente no caso do .NET Core do que era no projeto de .NET Framework, portanto, você provavelmente precisará usar `#if` diretivas para compilar condicionalmente algum uso de cliente do WCF (criar clientes, por exemplo) para trabalhar de uma maneira quando criado para o .NET Core e outra forma quando criado para .NET Framework.

- Se, por outro lado, alguma variação de código no projeto de .NET Framework existente for aceitável, você poderá remover *BeanTrader.cs* todos juntos. Como o novo cliente WCF foi criado para .NET Standard, ele funcionará em cenários do .NET Core e .NET Framework. Se você estiver criando para .NET Framework além do .NET Core (por vários destinos ou tendo dois arquivos csproj), poderá usar esse novo arquivo *Reference.cs* para ambos os destinos. Essa abordagem tem a vantagem de que o código não precisará bifurcar para dar suporte a dois clientes WCF diferentes; o mesmo código será usado em todos os lugares. A desvantagem é que ela envolve a alteração do projeto de .NET Framework (supostamente estável) do.

No caso do exemplo do bean de Trader, você pode fazer pequenas alterações no projeto original se ele facilitar a migração. portanto, siga estas etapas para reconciliar o uso do cliente WCF:

01. Adicione o novo arquivo Reference.cs ao projeto .NET Framework *BeanTraderClient. csproj* usando o menu de contexto ' Adicionar item existente ' do Gerenciador de soluções. Certifique-se de adicionar ' as link ' para que o mesmo arquivo seja usado por ambos os projetos (em vez de copiar o arquivo C#). Se você estiver compilando o .NET Core e .NET Framework com um único csproj (usando vários destinos), essa etapa não será necessária.

01. Exclua *BeanTrader.cs*.

01. O novo cliente WCF é semelhante ao antigo, mas vários namespaces no código gerado são diferentes. Por isso, é necessário atualizar o projeto para que os tipos de cliente do WCF sejam usados em BeanTrader. Service (ou em qualquer nome de namespace escolhido) em vez de BeanTrader. Model ou sem um namespace. A criação de *BeanTraderClient. Core. csproj* ajudará a identificar onde essas alterações precisam ser feitas. Correções serão necessárias tanto no C# quanto nos arquivos de origem XAML.

01. Por fim, você descobrirá que há um erro em *BeanTraderServiceClientFactory.cs* porque os construtores disponíveis para o `BeanTraderServiceClient` tipo foram alterados. Costumava ser possível fornecer um `InstanceContext` argumento (que foi criado usando um `CallbackHandler` do `Castle.Windsor` contêiner IoC). Os novos construtores criam novos `CallbackHandler` s. No entanto, há construtores no `BeanTraderServiceClient` tipo base do que correspondem ao que você deseja. Como o código do cliente WCF gerado automaticamente existe em classes parciais, você pode estendê-lo facilmente. Para fazer isso, crie um novo arquivo chamado *BeanTraderServiceClient.cs* e, em seguida, crie uma classe parcial com esse mesmo nome (usando o namespace BeanTrader. Service). Em seguida, adicione um construtor ao tipo parcial, conforme mostrado aqui.

    ```csharp
    public BeanTraderServiceClient(System.ServiceModel.InstanceContext callbackInstance) :
        base(callbackInstance, EndpointConfiguration.NetTcpBinding_BeanTraderService)
            { }
    ```

Com essas alterações feitas, o exemplo do bean de Trader agora estará usando um novo cliente WCF compatível com .NET Standard e você poderá fazer a correção final de alterar a `Open` chamada em *TradingService.cs* para usar `await OpenAsync` em vez disso.

Com os problemas do WCF abordados, a versão do .NET Core do exemplo do bean de Traders agora é compilada de forma limpa!

## <a name="runtime-testing"></a>Testes em tempo de execução

É fácil esquecer que o trabalho de migração não é feito assim que o projeto é compilado corretamente no .NET Core. É importante deixar tempo para testar o aplicativo portado também. Depois que as coisas forem compiladas com êxito, verifique se o aplicativo é executado e funciona conforme o esperado, especialmente se você estiver usando qualquer pacote destinado a .NET Framework.

Vamos tentar iniciar o aplicativo de beans de portador e ver o que acontece. O aplicativo não fica muito antes de falhar com a exceção a seguir.

```output
System.Configuration.ConfigurationErrorsException: 'Configuration system failed to initialize'

Inner Exception
ConfigurationErrorsException: Unrecognized configuration section system.serviceModel.
```

Isso faz sentido, é claro. Lembre-se de que o WCF não usa mais a configuração de aplicativo, portanto, a seção System. serviceModel antiga do arquivo app. config precisa ser removida. O cliente atualizado do WCF inclui todas as mesmas informações em seu código, portanto, a seção de configuração não é mais necessária. Se você quisesse que o ponto de extremidade do WCF fosse configurável no app. config, poderá adicioná-lo como uma configuração de aplicativo e atualizar o código do cliente WCF para recuperar o ponto de extremidade do serviço WCF da configuração.

Depois de remover a seção System. serviceModel de *app. config*, o aplicativo é iniciado, mas falha com outra exceção quando um usuário entra.

```output
System.PlatformNotSupportedException: 'Operation is not supported on this platform.'
```

A API sem suporte é `Func<T>.BeginInvoke` . Conforme explicado em [dotnet/corefx # 5940](https://github.com/dotnet/corefx/issues/5940), o .NET Core não dá suporte aos `BeginInvoke` `EndInvoke` métodos e em tipos delegados devido a dependências de comunicação remota subjacentes. Esse problema e sua correção são explicados mais detalhadamente na postagem de [chamadas delegate. BeginInvoke para](https://devblogs.microsoft.com/dotnet/migrating-delegate-begininvoke-calls-for-net-core/) o blog do .NET Core, mas o principal é que `BeginInvoke` e as `EndInvoke` chamadas devem ser substituídas por `Task.Run` (ou por alternativas assíncronas, se possível). Aplicando a solução geral aqui, a `BeginInvoke` chamada pode ser substituída `Invoke` por uma chamada iniciada pelo `Task.Run` .

```csharp
Task.Run(() =>
{
    return userInfoRetriever.Invoke();
}).ContinueWith(result =>
{
    // BeginInvoke's callback is replaced with ContinueWith
    var task = result.ConfigureAwait(false);
    CurrentTrader = task.GetAwaiter().GetResult();
}, TaskScheduler.Default);
```

Depois de remover o `BeginInvoke` uso, o aplicativo do Beans Trader é executado com êxito no .NET Core!

![Bean de Trader em execução no .NET Core](./media/convert-project-from-net-framework/running-on-core.png)

Todos os aplicativos são diferentes, portanto, as etapas específicas necessárias para migrar seus próprios aplicativos para o .NET Core irão variar. Mas, espero que o exemplo do bean de um dos traders demonstre o fluxo de trabalho geral e os tipos de problemas que podem ser esperados. E, apesar do comprimento do artigo, as alterações reais necessárias no exemplo do bean de Trader para fazê-lo funcionar no .NET Core eram bastante limitadas. Muitos aplicativos migram para o .NET Core da mesma maneira; com alterações limitadas ou até mesmo nenhuma alteração de código necessária.
