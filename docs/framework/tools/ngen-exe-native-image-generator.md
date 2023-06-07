---
title: Ngen.exe (Gerador de Imagens Nativas)
description: Examine Ngen.exe, o gerador de imagem nativa. Melhore o desempenho do aplicativo gerenciado criando imagens nativas e instalando no cache de imagem nativa local.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Native Image Generator
- images [.NET Framework], native
- side-by-side execution, native images
- assemblies [.NET Framework], native image
- Ngen.exe
- native image generation
- native image cache
- publisher policy applied for native images
- invalid images
- BypassNGenAttribute
- System.Runtime.BypassNGenAttribute
ms.assetid: 44bf97aa-a9a4-4eba-9a0d-cfaa6fc53a66
ms.openlocfilehash: ae86aed773a9a13f102b1ad111cac5a3ee563508
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517263"
---
# <a name="ngenexe-native-image-generator"></a>Ngen.exe (Gerador de Imagens Nativas)

O Gerador de Imagem Nativa (Ngen.exe) é uma ferramenta que melhora o desempenho de aplicativos gerenciados. Ngen.exe cria imagens nativas, que são arquivos que contém o código de máquina específico do processamento compilado e as instala no cache de imagem nativa do computador local. O runtime pode usar imagens nativas do cache em vez de usar o compilador JIT (Just-In-Time) para compilar o assembly original.

> [!NOTE]
> Ngen.exe compila imagens nativas para assemblies que visam somente o .NET Framework. O gerador de imagem nativa equivalente para o .NET Core é [CrossGen](https://github.com/dotnet/runtime/blob/master/docs/workflow/building/coreclr/crossgen.md).

Alterações feitas em Ngen.exe no .NET Framework versão 4:

- Ngen.exe agora compila assemblies com confiança total, e a política de segurança de acesso de código (CAS) deixa de ser avaliada.

- As imagens nativas geradas com Ngen.exe não podem mais ser carregadas em aplicativos em execução com confiança parcial.

Alterações feitas em Ngen.exe no .NET Framework versão 2.0:

- A instalação de um assembly também instala suas dependências, o que simplifica a sintaxe de Ngen.exe.

- As imagens nativas agora podem ser compartilhadas entre domínios de aplicativo.

- Uma nova ação, `update`, recria imagens que foram invalidadas.

- As ações podem ser adiadas para execução por um serviço que usa o tempo ocioso no computador para gerar e instalar imagens.

- Algumas causas da invalidação de imagem são eliminadas.

No Windows 8, confira [Tarefa de Imagem Nativa](#native-image-task).

Para obter mais informações sobre como usar o Ngen.exe e o serviço de imagem nativa, consulte [Serviço de imagem nativa](#native-image-service).

> [!NOTE]
> A sintaxe de Ngen.exe para as versões 1.0 e 1.1 do .NET Framework pode ser encontrada em [Sintaxe herdada do Gerador de Imagens Nativas (Ngen.exe)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms165073(v=vs.100)).

Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).

No prompt de comando, digite o seguinte:

## <a name="syntax"></a>Sintaxe

```console
ngen action [options]
```

```console
ngen /? | /help
```

## <a name="actions"></a>Ações

A tabela a seguir mostra a sintaxe de cada `action`. Para obter descrições das partes individuais de um `action`, confira as tabelas [Argumentos](#ArgumentTable), [Níveis de Prioridade](#PriorityTable), [Cenários](#ScenarioTable) e [Configuração](#ConfigTable). A tabela [Opções](#OptionTable) descreve o `options` e as opções da ajuda.

|Ação|Descrição|
|------------|-----------------|
|`install` [`assemblyName` &#124; `assemblyPath`] [`scenarios`] [`config`] [`/queue`[`:`{`1`&#124;`2`&#124;`3`}]]|Gere imagens nativas para um assembly e suas dependências e instale as imagens no cache de imagem nativa.<br /><br /> Se `/queue` for especificado, a ação será enfileirada para o serviço de imagem nativa. A prioridade padrão é 3. Confira a tabela [Níveis de Prioridade](#PriorityTable).|
|`uninstall` [`assemblyName` &#124; `assemblyPath`] [`scenarios`] [`config`]|Exclua as imagens nativas de um assembly e suas dependências do cache de imagem nativa.<br /><br /> Para desinstalar uma única imagem e suas dependências, use os mesmos argumentos de linha de comando que foram usados para instalar a imagem. **Observação:**  A partir do .NET Framework 4, a ação `uninstall` * não é mais suportada.|
|`update` [`/queue`]|Atualize imagens nativas que se tornaram inválidas.<br /><br /> Se `/queue` for especificado, as atualizações serão enfileiradas para o serviço de imagem nativa. Como as atualizações estão sempre programadas na prioridade 3, elas são executadas quando o computador está ocioso.|
|`display` [`assemblyName` &#124; `assemblyPath`]|Exiba o estado das imagens nativas para um assembly e suas dependências.<br /><br /> Se nenhum argumento for fornecido, tudo no cache de imagem nativa será exibido.|
|`executeQueuedItems` [<code>1&#124;2&#124;3</code>]<br /><br /> -ou-<br /><br /> `eqi` [1&#124;2&#124;3]|Execute o trabalho de compilação enfileirado.<br /><br /> Se uma prioridade for especificada, trabalhos de compilação com prioridade maior ou igual serão executados. Se nenhuma prioridade for especificada, todos os trabalhos de compilação enfileirados serão executados.|
|`queue` {`pause` &#124; `continue` &#124; `status`}|Pause o serviço de imagem nativa, deixe o serviço pausado continuar ou consulte o status do serviço.|

<a name="ArgumentTable"></a>

## <a name="arguments"></a>Argumentos

|Argumento|Descrição|
|--------------|-----------------|
|`assemblyName`|O nome para exibição completo do assembly. Por exemplo, `"myAssembly, Version=2.0.0.0, Culture=neutral, PublicKeyToken=0038abc9deabfle5"`. **Observação:** é possível fornecer um nome de assembly parcial, por exemplo, `myAssembly` para as ações `display` e `uninstall`. <br /><br /> Somente um assembly pode ser especificado por linha de comando de Ngen.exe.|
|`assemblyPath`|O caminho explícito do assembly. É possível especificar um caminho completo ou relativo.<br /><br /> Se você especificar um nome de arquivo sem um caminho, o assembly deverá estar localizado no diretório atual.<br /><br /> Somente um assembly pode ser especificado por linha de comando de Ngen.exe.|

<a name="PriorityTable"></a>

## <a name="priority-levels"></a>Níveis de Prioridade

|Prioridade|Descrição|
|--------------|-----------------|
|`1`|As imagens nativas são geradas e instaladas imediatamente, sem aguardar o tempo ocioso.|
|`2`|As imagens nativas são geradas e instaladas sem aguardar o tempo ocioso, mas depois de todas as ações de prioridade 1 (e suas dependências) serem concluídas.|
|`3`|As imagens nativas são instaladas quando o serviço de imagem nativa detecta que o computador está ocioso. Consulte [Serviço de imagem nativa](#native-image-service).|

<a name="ScenarioTable"></a>

## <a name="scenarios"></a>Cenários

|Cenário|Descrição|
|--------------|-----------------|
|`/Debug`|Gere imagens nativas que possam ser usadas em um depurador.|
|`/Profile`|Gere imagens nativas que possam ser usadas em um criador de perfil.|
|`/NoDependencies`|Gere o número mínimo de imagens nativas exigidas pelas opções de cenário especificadas.|

<a name="ConfigTable"></a>

## <a name="config"></a>Config

|Configuração|Descrição|
|-------------------|-----------------|
|`/ExeConfig:` `exePath`|Use a configuração do assembly executável especificado.<br /><br /> Ngen.exe precisa tomar as mesmas decisões do carregador durante a associação a dependências. Quando um componente compartilhado é carregado no tempo de execução, usando-se o método <xref:System.Reflection.Assembly.Load%2A>, o arquivo de configuração do aplicativo determina as dependências carregadas para o componente compartilhado — por exemplo, a versão de uma dependência carregada. A opção `/ExeConfig` dá a Ngen.exe orientações sobre quais dependências seriam carregadas no tempo de execução.|
|`/AppBase:` `directoryPath`|Para localizar dependências, use o diretório especificado como a base do aplicativo.|

<a name="OptionTable"></a>

## <a name="options"></a>Opções

|Opção|DESCRIÇÃO|
|------------|-----------------|
|`/nologo`|Suprima a exibição do banner de inicialização da Microsoft.|
|`/silent`|Suprima a exibição das mensagens de êxito.|
|`/verbose`|Exiba informações detalhadas da depuração. **Observação:** devido a limitações de sistema operacional, essa opção não exibe o máximo de informações adicionais sobre Windows 98 e Windows Millennium.|
|`/help`, `/?`|Exiba a sintaxe de comando e as opções da versão atual.|

## <a name="remarks"></a>Comentários

Para executar Ngen.exe, você deve ter privilégios administrativos.

> [!CAUTION]
> Não execute Ngen.exe em assemblies que não sejam totalmente confiáveis. A partir do .NET Framework 4, o Ngen.exe compila assemblies com confiança total, e a política de segurança de acesso de código (CAS) deixa de ser avaliada.

Do .NET Framework 4 em diante, as imagens nativas geradas com Ngen.exe não podem mais ser carregadas em aplicativos em execução com confiança parcial. Em vez disso, o compilador JIT é invocado.

O Ngen.exe gerencia imagens nativas para o assembly especificado pelo argumento `assemblyname` para a ação `install` e todas as suas dependências. As dependências são determinadas com base nas referências do manifesto do assembly. O único cenário no qual você precisa instalar uma dependência separadamente é quando o aplicativo a carrega usando a reflexão, por exemplo, chamando o método <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>.

> [!IMPORTANT]
> Não use o método <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> com imagens nativas. Uma imagem carregada com esse método não pode ser usada por outros assemblies no contexto da execução.

Ngen.exe mantém uma contagem das dependências. Por exemplo, suponhamos que `MyAssembly.exe` e `YourAssembly.exe` estejam ambos instalados no cache de imagem nativa, e ambos tenham referências para `OurDependency.dll`. Se `MyAssembly.exe` for desinstalado, `OurDependency.dll` não será desinstalado. Ele só é removido quando `YourAssembly.exe` também é desinstalado.

Se você estiver gerando uma imagem nativa para um assembly no cache de assembly global, especifique seu nome para exibição. Consulte <xref:System.Reflection.Assembly.FullName%2A?displayProperty=nameWithType>.

As imagens nativas geradas por Ngen.exe podem ser compartilhadas entre domínios de aplicativo. Isso significa que é possível usar Ngen.exe em cenários de aplicativo que exijam que os assemblies sejam compartilhados entre domínios de aplicativo. Para especificar a neutralidade do domínio:

- Aplique o atributo <xref:System.LoaderOptimizationAttribute> ao aplicativo.

- Defina a propriedade <xref:System.AppDomainSetup.LoaderOptimization%2A?displayProperty=nameWithType> quando você cria informações de configuração para um novo domínio de aplicativo.

Sempre use o código neutro de domínio ao carregar o mesmo assembly em vários domínios de aplicativo. Se uma imagem nativa for carregada em um domínio de aplicativo não compartilhado depois de ter sido carregada em um domínio compartilhado, ela não poderá ser usada.

> [!NOTE]
> O código neutro de domínio não pode ser descarregado, e o desempenho pode ser um pouco mais lento, especialmente durante o acesso a membros estáticos.

Nesta seção Observações:

- [Gerar imagens para cenários diferentes](#Scenarios)

- [Determinar quando usar imagens nativas](#WhenToUse)

  - [Uso de memória aprimorado](#Memory)

  - [Inicialização mais rápida do aplicativo](#Startup)

  - [Resumo das considerações de uso](#UsageSummary)

- [Importância de endereços de base do assembly](#BaseAddresses)

- [Associação forçada](#HardBinding)

  - [Especificar uma dica de associação para uma dependência](#DependencyHint)

  - [Especificar uma dica de associação padrão para um assembly](#AssemblyHint)

- [Processamento adiado](#Deferred)

- [Imagens nativas e compilação JIT](#JITCompilation)

  - [Imagens inválidas](#InvalidImages)

- [Solução de problemas](#Troubleshooting)

  - [Visualizador de log da associação do assembly](#Fusion)

  - [O assistente de depuração gerenciado JITCompilationStart](#MDA)

  - [Recusa da geração automática de imagem nativa](#OptOut)

<a name="Scenarios"></a>

## <a name="generating-images-for-----different-scenarios"></a>Gerar imagens para cenários diferentes

Depois que você tiver gerado uma imagem nativa para um assembly, o runtime tenta localizar e usar automaticamente essa imagem nativa sempre que executa o assembly. Várias imagens podem ser geradas, dependendo dos cenários de uso.

Por exemplo, se você executar um assembly em um cenário de depuração ou de criação de perfil, o runtime procurará uma imagem nativa gerada com as opções `/Debug` ou `/Profile`. Caso ele não consiga encontrar uma imagem nativa correspondente, o runtime é revertido para a compilação JIT padrão. A única maneira de depurar imagens nativas é criando uma imagem nativa com a opção `/Debug`.

Como a ação `uninstall` também reconhece cenários, é possível desinstalar todos os cenários ou apenas os cenários selecionados.

<a name="WhenToUse"></a>

## <a name="determining-when-to-use-native-images"></a>Determinar quando usar imagens nativas

Imagens nativas podem oferecer melhorias de desempenho em duas áreas: melhor uso da memória e tempo de inicialização reduzido.

> [!NOTE]
> O desempenho de imagens nativas depende de vários fatores que dificultam a análise como, por exemplo, padrões de acesso ao código e aos dados, quantas chamadas são feitas entre os limites do módulo e quantas dependências já foram carregadas por outros aplicativos. A única maneira de determinar se as imagens nativas beneficiam o aplicativo é por meio de medidas de desempenho cuidadosas nos principais cenários de implantação.

<a name="Memory"></a>

### <a name="improved-memory-use"></a>Uso de memória aprimorado

Imagens nativas podem melhorar significativamente o uso da memória quando o código é compartilhado entre processos. Como imagens nativas são arquivos do Windows PE, uma única cópia de um arquivo .dll pode ser compartilhada por vários processos; por outro lado, o código nativo produzido pelo compilador JIT é armazenado em memória privada e não pode ser compartilhada.

Aplicativos executados em serviços de terminal também podem aproveitar páginas de código compartilhadas.

Além disso, o não carregamento do compilador JIT salva uma quantidade de memória fixa para cada instância do aplicativo.

<a name="Startup"></a>

### <a name="faster-application-startup"></a>Inicialização mais rápida do aplicativo

A pré-compilação de assemblies com Ngen.exe pode melhorar o tempo de inicialização de alguns aplicativos. Em geral, os ganhos podem acontecer quando aplicativos compartilham assemblies de componente, porque depois que o primeiro aplicativo foi iniciado os componentes compartilhados já estão carregados para aplicativos subsequentes. A inicialização fria, em que todos os assemblies em um aplicativo devem ser carregados do disco rígido, não beneficia tanto quanto imagens nativas porque o tempo de acesso ao disco rígido predomina.

A associação forçada pode afetar o tempo de inicialização, pois todas as imagens solidamente associadas ao assembly do aplicativo principal devem ser carregadas ao mesmo tempo.

> [!NOTE]
> Antes do .NET Framework 3.5 Service Pack 1, você tinha que colocar componentes compartilhados, de nome forte, no cache de assembly global, pois o carregador realiza a validação extra em assemblies de nome forte que não estão no cache de assembly global, o que elimina efetivamente qualquer melhoria no tempo da inicialização obtido usando imagens nativas. As otimizações que foram introduzidas no .NET Framework 3.5 SP1 removeram a validação extra.

<a name="UsageSummary"></a>

### <a name="summary-of-usage-considerations"></a>Resumo das considerações de uso

As seguintes considerações gerais e as considerações de aplicativo podem ajudá-lo na decisão de assumir o esforço de avaliação das imagens nativas para o aplicativo:

- Imagens nativas são carregadas mais rapidamente do que MSIL porque eliminam a necessidade de muitas atividades de inicialização, como a compilação JIT e a verificação da segurança de tipos.

- As imagens nativas exigem um conjunto de trabalho inicial menor porque não há necessidade do compilador JIT.

- As imagens nativas permitem o compartilhamento de código entre processos.

- As imagens nativas exigem mais espaço em disco rígido do que assemblies MSIL e podem exigir um tempo considerável para geração.

- As imagens nativas devem ser mantidas.

  - As imagens precisam ser regeneradas quando o assembly original ou uma de suas dependências é atendida.

  - Um único assembly pode precisar de várias imagens nativas que serão usadas em aplicativos diferentes ou em cenários diferentes. Por exemplo, as informações de configuração em dois aplicativos podem resultar em decisões de associação diferentes para o mesmo assembly dependente.

  - As imagens nativas devem ser geradas por um administrador; ou seja, com base em uma conta do Windows no grupo Administradores.

Além dessas considerações gerais, a natureza do aplicativo deve ser levada em consideração durante a determinação da possibilidade das imagens nativas podem oferecer um benefício de desempenho:

- Se o aplicativo for executado em um ambiente que usa vários componentes compartilhados, as imagens nativas permitirão que os componentes sejam compartilhados por vários processos.

- Se o aplicativo usar vários domínios de aplicativo, as imagens nativas permitirão que as páginas de código sejam compartilhadas entre domínios.

    > [!NOTE]
    > No .NET Framework versões 1.0 e 1.1, as imagens nativas não podem ser compartilhadas entre domínios de aplicativo. Esse pode não ser o caso na versão 2.0 ou posterior.

- Se o aplicativo for executado no servidor Host da Sessão da Área de Trabalho Remota, as imagens nativas permitirão o compartilhamento das páginas de código.

- Os aplicativos grandes normalmente se beneficiam da compilação para imagens nativas. Os aplicativos pequenos normalmente não beneficiam.

- Para aplicativos executados por muito tempo, a compilação JIT do tempo de execução é um pouco melhor do que a de imagens nativas. (A associação forçada pode atenuar essa diferença de desempenho em algum grau.)

<a name="BaseAddresses"></a>

## <a name="importance-of-assembly-base-addresses"></a>Importância de endereços de base do assembly

Como são arquivos do Windows PE, as imagens nativas estão sujeitas aos mesmos problemas de rebase de outros arquivos executáveis. O custo de desempenho da realocação ficará ainda mais evidente se a associação forçada não for utilizada.

Para definir o endereço base para uma imagem nativa, use a opção apropriada do compilador para definir o endereço base do assembly. Ngen.exe usa esse endereço base para a imagem nativa.

> [!NOTE]
> As imagens nativas são maiores que os assemblies gerenciados a partir dos quais foram criadas. Os endereços de base devem ser calculados para possibilitar tamanhos maiores.

É possível usar uma ferramenta como dumpbin.exe para exibir o endereço de base preferencial de uma imagem nativa.

<a name="HardBinding"></a>

## <a name="hard-binding"></a>Associação forçada

A associação forçada aumenta a taxa de transferência e reduz o tamanho do conjunto de trabalho de imagens nativas. A desvantagem da associação forçada é que todas as imagens associadas forçadamente a um assembly devem ser carregadas quando o assembly é carregado. Isso pode aumentar significativamente o tempo de inicialização de um aplicativo grande.

A associação forçada é apropriada para dependências carregadas em todos os cenários de desempenho crítico do aplicativo. Assim como acontece com qualquer aspecto do uso da imagem nativa, medidas de desempenho cuidadosas são a única maneira de determinar se a associação forçada melhora o desempenho do aplicativo.

Os atributos <xref:System.Runtime.CompilerServices.DependencyAttribute> e <xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> permitem que você dê dicas de associação forçada a Ngen.exe.

> [!NOTE]
> Esses atributos são dicas para Ngen.exe, e não comandos. O uso deles não garante a associação forçada. O significado desses atributos pode ser alterado em versões futuras.

<a name="DependencyHint"></a>

### <a name="specifying-a-binding-hint-for-a-dependency"></a>Especificar uma dica de associação para uma dependência

Aplique o <xref:System.Runtime.CompilerServices.DependencyAttribute> a um assembly para indicar a probabilidade de que uma dependência especificada será carregada. <xref:System.Runtime.CompilerServices.LoadHint.Always?displayProperty=nameWithType> indica que a associação forçada é apropriada, <xref:System.Runtime.CompilerServices.LoadHint.Default> indica que o padrão da dependência deve ser usado e <xref:System.Runtime.CompilerServices.LoadHint.Sometimes> indica que a associação forçada não é apropriada.

O código a seguir mostra os atributos de um assembly que possui duas dependências. A primeira dependência (Assembly1) é uma candidata apropriada à associação forçada, mas a segunda (Assembly2) não é.

```vb
Imports System.Runtime.CompilerServices
<Assembly:DependencyAttribute("Assembly1", LoadHint.Always)>
<Assembly:DependencyAttribute("Assembly2", LoadHint.Sometimes)>
```

```csharp
using System.Runtime.CompilerServices;
[assembly:DependencyAttribute("Assembly1", LoadHint.Always)]
[assembly:DependencyAttribute("Assembly2", LoadHint.Sometimes)]
```

```cpp
using namespace System::Runtime::CompilerServices;
[assembly:DependencyAttribute("Assembly1", LoadHint.Always)];
[assembly:DependencyAttribute("Assembly2", LoadHint.Sometimes)];
```

O nome do assembly não inclui a extensão do nome de arquivo. Os nomes para exibição podem ser usados.

<a name="AssemblyHint"></a>

### <a name="specifying-a-default-binding-hint-for-an-assembly"></a>Especificar uma dica de associação padrão para um assembly

As dicas de associação padrão só são necessárias para assemblies que serão usados imediata e frequentemente por qualquer aplicativo que tenha uma dependência neles. Aplique o <xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> com <xref:System.Runtime.CompilerServices.LoadHint.Always?displayProperty=nameWithType> a esses assemblies para especificar que a associação forçada deve ser usada.

> [!NOTE]
> Não há motivo para aplicar <xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> a assemblies .dll que não estejam nessa categoria, porque a aplicação do atributo com qualquer valor diferente de <xref:System.Runtime.CompilerServices.LoadHint.Always?displayProperty=nameWithType> tem o mesmo efeito de não aplicar o atributo.

A Microsoft usa o <xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> para especificar que a associação forçada é o padrão para um número muito pequeno de assemblies no .NET Framework como, por exemplo, mscorlib.dll.

<a name="Deferred"></a>

## <a name="deferred-processing"></a>Processamento adiado

A geração de imagens nativas para um aplicativo muito grande pode demorar um tempo considerável. Da mesma forma, alterações feitas em um componente compartilhado ou alterações feitas em configurações do computador podem exigir a atualização de muitas imagens nativas. As ações `install` e `update` têm uma opção `/queue` que enfileira a operação para execução adiada pelo serviço de imagem nativa. Além disso, Ngen.exe tem ações `queue` e `executeQueuedItems` que oferecem certo controle sobre o serviço. Para obter mais informações, consulte [Serviço de imagem nativa](#native-image-service).

<a name="JITCompilation"></a>

## <a name="native-images-and-jit-compilation"></a>Imagens nativas e compilação JIT

Se encontrar algum método em um assembly que não puder gerar, Ngen.exe os excluirá da imagem nativa. Ao executar esse assembly, o runtime o reverte para compilação JIT dos métodos que não foram incluídos na imagem nativa.

Além disso, as imagens nativas não serão usadas se o assembly tiver sido atualizado, ou se a imagem tiver sido invalidada por qualquer motivo.

<a name="InvalidImages"></a>

### <a name="invalid-images"></a>Imagens inválidas

Quando você usa Ngen.exe para criar uma imagem nativa de um assembly, a saída depende das opções de linha de comando especificadas e de determinadas configurações do computador. Essas configurações incluem o seguinte:

- A versão do .NET Framework.

- A versão do sistema operacional, se a alteração for da família Windows 9x para a família Windows NT.

- A identidade exata do assembly (recompilação altera a identidade).

- A identidade exata de todos os assemblies aos quais o assembly faz referência (recompilação altera a identidade).

- Fatores de segurança.

Ngen.exe registra essas informações ao gerar uma imagem nativa. Quando você executa um assembly, o runtime procura a imagem nativa gerada com opções e configurações correspondentes ao ambiente atual do computador. O runtime será revertido para a compilação JIT de um assembly se não conseguir encontrar uma imagem nativa correspondente. As seguintes alterações feitas nas configurações e no ambiente de um computador invalidam imagens nativas:

- A versão do .NET Framework.

     Se você aplicar uma atualização ao .NET Framework, todas as imagens nativas criadas usando-se Ngen.exe serão invalidadas. Por esse motivo, todas as atualizações do .NET Framework executam o comando `Ngen Update`, para garantir que todas as imagens nativas sejam regeneradas. O .NET Framework cria automaticamente novas imagens nativas para as bibliotecas do .NET Framework instaladas.

- A versão do sistema operacional, se a alteração for da família Windows 9x para a família Windows NT.

     Por exemplo, se a versão do sistema operacional em execução em um computador mudar do Windows 98 para o Windows XP, todas as imagens nativas armazenadas no cache de imagem nativa serão invalidadas. No entanto, se o sistema operacional mudar do Windows 2000 para o Windows XP, as imagens não serão invalidadas.

- A identidade exata do assembly.

     Se você recompilar um assembly, a imagem nativa correspondente do assembly será invalidada.

- A identidade exata de todos os assemblies aos quais o assembly faz referência.

     Se você atualizar um assembly gerenciado, todas as imagens nativas que dependam direta ou indiretamente desse assembly serão invalidadas e precisarão ser regeneradas. Isso inclui as referências comuns e as dependências associadas forçadamente. Sempre que uma atualização de software for aplicada, o programa de instalação deverá executar um comando `Ngen Update` para garantir que as todas as imagens nativas dependentes sejam regeneradas.

- Fatores de segurança.

     A alteração da política de segurança do computador para restringir permissões concedidas anteriormente a um assembly pode invalidar uma imagem nativa compilada anteriormente para esse assembly.

     Para obter informações detalhadas sobre como o Common Language Runtime administra a segurança de acesso do código e como usar permissões, confira [Segurança de acesso do código](../misc/code-access-security.md).

<a name="Troubleshooting"></a>

## <a name="troubleshooting"></a>Solução de problemas

Os tópicos de solução de problemas a seguir permitem que você veja quais imagens nativas estão sendo usadas e quais não podem ser usadas por seu aplicativo, para determinar quando o compilador JIT começa a compilar um método, e mostra como recusar a compilação de imagem nativa dos métodos especificados.

<a name="Fusion"></a>

### <a name="assembly-binding-log-viewer"></a>Visualizador de Log de Associação do Assembly

Para confirmar que as imagens nativas estão sendo usadas pelo aplicativo, é possível usar [Fuslogvw.exe (Visualizador do Log de Associações de Assembly)](fuslogvw-exe-assembly-binding-log-viewer.md). Selecione **Imagens Nativas** na caixa **Categorias de Log** na janela do visualizador de log da associação. O Fuslogvw.exe fornece informações sobre por que uma imagem nativa foi rejeitada.

<a name="MDA"></a>

### <a name="the-jitcompilationstart-managed-debugging-assistant"></a>O assistente de depuração gerenciado JITCompilationStart

É possível usar o MDA (Assistente para depuração gerenciada) [jitCompilationStart](../debug-trace-profile/jitcompilationstart-mda.md) para determinar quando o compilador JIT começa a compilar uma função.

<a name="OptOut"></a>

### <a name="opting-out-of-native-image-generation"></a>Recusa da geração automática de imagem nativa

Em alguns casos, o NGen.exe pode ter dificuldade para gerar uma imagem nativa para um método específico, ou talvez você prefira que o método seja compilado pelo JIT, em vez de ser compilado para uma imagem nativa. Nesse caso, você pode usar o atributo `System.Runtime.BypassNGenAttribute` para impedir que o NGen.exe gere uma imagem nativa para um método específico. O atributo deve ser aplicado individualmente a cada método cujo código você não quer incluir na imagem nativa. O NGen.exe reconhece o atributo e não gera um código na imagem nativa para o método correspondente.

No entanto, observe que `BypassNGenAttribute` não está definido como um tipo na Biblioteca de Classes .NET Framework. Para consumir o atributo em seu código, primeiro definia-o da seguinte maneira:

[!code-csharp[System.Runtime.BypassNGenAttribute#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/cs/Optout1.cs#1)]
[!code-vb[System.Runtime.BypassNGenAttribute#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/vb/Optout1.vb#1)]

Depois, você pode aplicar o atributo de acordo com o método. O exemplo a seguir instrui o Gerador de imagem nativa que ele não deve gerar uma imagem nativa para o método `ExampleClass.ToJITCompile`.

[!code-csharp[System.Runtime.BypassNGenAttribute#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/cs/Optout1.cs#2)]
[!code-vb[System.Runtime.BypassNGenAttribute#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/vb/Optout1.vb#2)]

## <a name="examples"></a>Exemplos

O comando a seguir gera uma imagem nativa para `ClientApp.exe`, localizado no diretório atual, e instala a imagem no cache de imagem nativa. Se existir um arquivo de configuração para o assembly, Ngen.exe o usará. Além de isso, imagens nativas são geradas para todos os arquivos .dll a que `ClientApp.exe` faz referência.

```console
ngen install ClientApp.exe
```

Uma imagem instalada com Ngen.exe também é chamada de raiz. Uma raiz pode ser um aplicativo ou um componente compartilhado.

O comando a seguir gera uma imagem nativa para `MyAssembly.exe` com o caminho especificado.

```console
ngen install c:\myfiles\MyAssembly.exe
```

Para localizar assemblies e as suas dependências, Ngen.exe usa a mesma lógica de investigação usada pelo Common Language Runtime. Por padrão, o diretório que contém `ClientApp.exe` é usado como diretório base do aplicativo, e toda investigação de assembly começa nesse diretório. É possível substituir esse comportamento usando-se a opção `/AppBase`.

> [!NOTE]
> Essa é uma alteração no comportamento de Ngen.exe no .NET Framework versões 1.0 e 1.1, em que a base do aplicativo é definida como o diretório atual.

Um assembly pode ter uma dependência sem uma referência, por exemplo, se carregar um arquivo .dll usando o método <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>. É possível criar uma imagem nativa para esse arquivo .dll usando-se as informações de configuração do assembly do aplicativo, com a opção `/ExeConfig`. O comando a seguir gera uma imagem nativa para `MyLib.dll,` usando as informações de configuração de `MyApp.exe`.

```console
ngen install c:\myfiles\MyLib.dll /ExeConfig:c:\myapps\MyApp.exe
```

Os assemblies instalados dessa forma não são removidos quando o aplicativo é removido.

Para desinstalar uma dependência, use as mesmas opções de linha de comando que foram usadas para instalá-la. O comando a seguir desinstala o `MyLib.dll` do exemplo anterior.

```console
ngen uninstall c:\myfiles\MyLib.dll /ExeConfig:c:\myapps\MyApp.exe
```

Para criar uma imagem nativa para um assembly no cache de assembly global, use o nome para exibição do assembly. Por exemplo:

```console
ngen install "ClientApp, Version=1.0.0.0, Culture=neutral,
  PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL"
```

NGen.exe gera um conjunto separado de imagens para cada cenário instalado. Por exemplo, os seguintes comandos instalam um conjunto completo de imagens nativas para operação normal, outro conjunto completo para depuração e um terceiro para criação de perfil:

```console
ngen install MyApp.exe
ngen install MyApp.exe /debug
ngen install MyApp.exe /profile
```

### <a name="displaying-the-native-image-cache"></a>Exibindo o Cache de Imagem Nativa

Depois de serem instaladas no cache, as imagens nativas poderão ser exibidas usando-se Ngen.exe. O comando a seguir exibe todas as imagens nativas no cache de imagem nativa.

```console
ngen display
```

A ação `display` lista todos os assemblies raiz primeiro, seguido de uma lista de todas as imagens nativas no computador.

Use o nome simples de um assembly para exibir informações somente desse assembly. O seguinte comando exibe todas as imagens nativas no cache de imagem nativa que correspondem ao nome parcial `MyAssembly`, suas dependências e todas as raízes que tenham uma dependência em `MyAssembly`:

```console
ngen display MyAssembly
```

Saber quais raízes dependem de um assembly de componente compartilhado é útil na avaliação do impacto de uma ação `update` depois que o componente compartilhado é atualizado.

Se especificar a extensão de arquivo de um assembly, você deverá especificar o caminho ou executar Ngen.exe no diretório que contém o assembly:

```console
ngen display c:\myApps\MyAssembly.exe
```

O comando a seguir exibe todas as imagens nativas no cache de imagem nativa com o nome `MyAssembly`e a versão 1.0.0.0.

```console
ngen display "myAssembly, version=1.0.0.0"
```

### <a name="updating-images"></a>Atualizando Imagens

As imagens normalmente são atualizadas depois que um componente compartilhado é atualizado. Para atualizar todas as imagens nativas alteradas, ou cujas dependências foram alteradas, use a ação `update` sem argumentos.

```console
ngen update
```

A atualização de todas as imagens pode ser um processo demorado. É possível enfileirar as atualizações para execução pelo serviço de imagem nativa usando-se a opção `/queue`. Para obter mais informações sobre a opção `/queue` e as prioridades de instalação, consulte [Serviço de imagem nativa](#native-image-service).

```console
ngen update /queue
```

### <a name="uninstalling-images"></a>Desinstalando Imagens

Como Ngen.exe mantém uma lista de dependências, os componentes compartilhados só são removidos quando todos os assemblies que dependam dele forem removidos. Além de isso, um componente compartilhado não será removido se tiver sido instalado como raiz.

O seguinte comando desinstala todos os cenários da raiz `ClientApp.exe`:

```console
ngen uninstall ClientApp
```

A ação `uninstall` pode ser usada para remover cenários específicos. O seguinte comando desinstala todos os cenários de depuração de `ClientApp.exe`:

```console
ngen uninstall ClientApp /debug
```

> [!NOTE]
> A desinstalação de cenários `/debug` não desinstalará um cenário que inclua `/profile` e `/debug.`

O seguinte comando desinstala todos os cenários de uma versão específica de `ClientApp.exe`:

```console
ngen uninstall "ClientApp, Version=1.0.0.0"
```

Os seguintes comandos desinstalam todos os cenários de `"ClientApp, Version=1.0.0.0, Culture=neutral, PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL",` ou apenas o cenário de depuração desse assembly:

```console
ngen uninstall "ClientApp, Version=1.0.0.0, Culture=neutral,
  PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL"
ngen uninstall "ClientApp, Version=1.0.0.0, Culture=neutral,
  PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL" /debug
```

Assim como acontece com a ação `install`, o fornecimento de uma extensão exige a execução de Ngen.exe no diretório que contém o assembly ou a especificação de um caminho completo.

Para obter exemplos relacionados ao serviço de imagem nativa, consulte [Serviço de imagem nativa](#native-image-service).

## <a name="native-image-task"></a>Tarefa de imagem nativa

A tarefa de imagem nativa é uma tarefa do Windows que gera e mantém as imagens nativas. A tarefa de imagem nativa gera e recupera automaticamente as imagens nativas para cenários com suporte. Também permite que os instaladores usem o [Ngen.exe (Gerador de imagens nativas)](ngen-exe-native-image-generator.md) para criar e atualizar as imagens nativas em um tempo adiado.

A tarefa de imagem nativa é registrada uma vez para cada arquitetura de CPU com suporte em um computador, a fim de permitir a compilação para aplicativos direcionados a cada arquitetura:

|Nome da tarefa|Computador de 32 bits|Computador de 64 bits|
|---------------|----------------------|----------------------|
|NET Framework NGEN v4.0.30319|Sim|Sim|
|NET Framework NGEN v4.0.30319 64|Não|Sim|

A tarefa de imagem nativa está disponível no .NET Framework 4.5 e em versões posteriores ao ser executado no Windows 8 ou versão mais recente. Em versões anteriores do Windows, o .NET Framework usa o [Serviço de Imagem Nativa](#native-image-service).

### <a name="task-lifetime"></a>Tempo de vida da tarefa

Em geral, o Agendador de Tarefas do Windows inicia a tarefa de imagem nativa toda noite, quando o computador está ocioso. A tarefa procura qualquer trabalho adiado que foi enfileirado pelos instaladores de aplicativo, quaisquer solicitações de atualização de imagem nativa adiadas e qualquer criação automática de imagem. A tarefa conclui itens de trabalho pendentes e depois desliga. Se o computador deixar o estado ocioso durante a execução da tarefa, a tarefa será interrompida.

Você também pode iniciar manualmente a tarefa de imagem nativa por meio da interface de usuário do Agendador de Tarefas ou por chamadas manuais ao NGen.exe. Se a tarefa for iniciada por um desses métodos, ele continuará em execução quando o computador não estiver mais ocioso. As imagens criadas manualmente com o NGen.exe recebem prioridade para permitir o comportamento previsível para os instaladores de aplicativos.

## <a name="native-image-service"></a>Serviço de imagens nativas

O serviço de imagem nativa é um serviço do Windows que gera e mantém as imagens nativas. Com o serviço de imagem nativa, o desenvolvedor pode adiar a instalação e a atualização de imagens nativas para quando o computador estiver ocioso.

Normalmente, o serviço de imagem nativa é iniciado pelo programa de instalação (instalador) para um aplicativo ou atualização. Para ações de prioridade 3, o serviço é executado durante o período ocioso do computador. O serviço salva seu estado e é capaz de continuar através de várias reinicializações, se for necessário. É possível enfileirar várias compilações de imagem.

O serviço também interage com o comando manual Ngen.exe. Comandos manuais têm precedência sobre a atividade em segundo plano.

> [!NOTE]
> No Windows Vista, o nome exibido para o serviço de imagem nativa é "Microsoft.NET Framework NGEN v2.0.50727_X86" ou "Microsoft.NET Framework NGEN v2.0.50727_X64". Em todas as versões anteriores do Microsoft Windows, o nome é ".NET Runtime Optimization Service v2.0.50727_X86" ou ".NET Runtime Optimization Service v2.0.50727_X64".

### <a name="launching-deferred-operations"></a>Iniciar operações adiadas

Antes de iniciar uma instalação ou atualização, recomendamos pausar o serviço. Isso garante que o serviço não seja executado enquanto o instalador está copiando os arquivos ou colocando os assemblies no cache de assembly global. Esta linha de comando do Ngen.exe pausa o serviço:

```console
ngen queue pause
```

Quando todas as operações adiadas tiverem sido enfileiradas, o comando a seguir permitirá que o serviço continue:

```console
ngen queue continue
```

Para adiar a geração de imagem nativa ao instalar um novo aplicativo ou ao atualizar um componente compartilhado, use a opção `/queue` com as ações `install` ou `update`. Estas linhas de comando do Ngen.exe instalam uma imagem nativa para um componente compartilhado e executam uma atualização de todas as raízes que foram afetadas:

```console
ngen install MyComponent /queue
ngen update /queue
```

A ação `update` gera novamente todas as imagens nativas que foram invalidadas, não apenas as que usam `MyComponent`.

Se o seu aplicativo for composto por várias raízes, você poderá controlar a prioridade das ações adiadas. Os comandos a seguir enfileiram a instalação de três raízes. O `Assembly1` é instalado primeiro, sem aguardar o tempo ocioso. O `Assembly2` também é instalado sem esperar o tempo ocioso, mas somente após a conclusão de todas as ações de prioridade 1. O `Assembly3` é instalado quando o serviço detecta que o computador está ocioso.

```console
ngen install Assembly1 /queue:1
ngen install Assembly2 /queue:2
ngen install Assembly3 /queue:3
```

Você pode forçar a ocorrência assíncrona de ações enfileiradas usando a ação `executeQueuedItems`. Se você fornecer a prioridade opcional, essa ação afetará somente as ações enfileiradas que têm prioridade igual ou menor. A prioridade padrão é a 3, portanto, o seguinte comando Ngen.exe processa todas as ações em fila imediatamente e não retorna até que todas sejam concluídas:

```console
ngen executeQueuedItems
```

Os comandos síncronos são executados pelo Ngen.exe e não usam o serviço de imagem nativa. Você pode executar ações usando Ngen.exe enquanto o serviço de imagem nativa estiver em execução.

### <a name="service-shutdown"></a>Desligamento do serviço

Depois de iniciado pela execução de um comando Ngen.exe que inclui a opção `/queue`, o serviço é executado em segundo plano até que todas as ações sejam concluídas. O serviço salva seu estado para que ele possa continuar por várias reinicializações, se for necessário. Quando o serviço detecta que há não mais ações na fila, ele redefine seu status para que não reinicie na próxima vez que o computador for inicializado e, depois, ele se desliga.

### <a name="service-interaction-with-clients"></a>Interação do serviço com clientes

No .NET Framework versão 2.0, a única interação com o serviço de imagem nativa ocorre por meio da ferramenta de linha de comando Ngen.exe. Use a ferramenta de linha de comando em scripts de instalação para enfileirar ações para o serviço de imagem nativa e para interagir com o serviço.

## <a name="see-also"></a>Consulte também

- [Ferramentas](index.md)
- [Processo de execução gerenciada](../../standard/managed-execution-process.md)
- [Como o runtime localiza assemblies](../deployment/how-the-runtime-locates-assemblies.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
