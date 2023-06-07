---
title: Armazenamentos isolado
description: Explore o armazenamento isolado, que é um mecanismo de armazenamento de dados que fornece isolamento & segurança definindo maneiras padronizadas de associar código a dados salvos.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- data storage using isolated storage
- stores
- storing data using isolated storage
- isolated storage
- location of isolated storage in file system
- standardizing storage systems
- storing data using isolated storage, when not to use
- code, isolated storage
- isolated storage, options
- data storage using isolated storage, when not to use
- storing data using isolated storage, options
- isolated storage, when not to use
- data storage using isolated storage, options
- isolation
ms.assetid: aff939d7-9e49-46f2-a8cd-938d3020e94e
ms.openlocfilehash: 0de0c7e9843ca8a97392733a68367b1dae8de232
ms.sourcegitcommit: 3492dafceb5d4183b6b0d2f3bdf4a1abc4d5ed8c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2020
ms.locfileid: "86416385"
---
# <a name="isolated-storage"></a>Armazenamento isolado

 Para aplicativos desktop, o armazenamento isolado é um mecanismo de armazenamento de dados que proporciona isolamento e segurança ao definir formas padronizadas de associar código a dados salvos. A padronização também fornece outros benefícios. Os administradores podem usar as ferramentas desenvolvidas para manipular armazenamentos isolados para configurar espaço de armazenamento de arquivos, definir políticas de segurança e excluir dados não utilizados. Com armazenamentos isolados, seu código não precisa mais de caminhos exclusivos para especificar locais seguros na sistema de arquivos e os dados são protegidos de outros aplicativos que só têm acesso a armazenamentos isolados. Informações embutidas em código que indicam onde a área de armazenamento de um aplicativo se encontra são desnecessárias.

> [!IMPORTANT]
> O armazenamento isolado não está disponível para aplicativos da loja do Windows 8. x. Em vez disso, use as classes de dados de aplicativos nos namespaces `Windows.Storage` incluídos na API do Windows Runtime para armazenar dados e arquivos locais. Para saber mais, confira [Dados de aplicativo](https://docs.microsoft.com/previous-versions/windows/apps/hh464917(v=win.10)) no Centro de Desenvolvimento do Windows.

<a name="data_compartments_and_stores"></a>

## <a name="data-compartments-and-stores"></a>Compartimentos e Repositórios de Dados

Quando um aplicativo armazena dados em um arquivo, o nome do arquivo e o local de armazenamento devem ser escolhidos cuidadosamente para minimizar a possibilidade de o local de armazenamento ser conhecido por outro aplicativo e, portanto, tornar-se vulnerável a corrompimento. Sem um sistema padrão para gerenciar esses problemas, o desenvolvimento de técnicas ad hoc que minimizam conflitos de armazenamento pode ser complexo e os resultados podem não ser confiáveis.

Com armazenamento isolado, os dados são sempre isolados pelo usuário e pelo assembly. Credenciais como a origem ou o nome forte do assembly determinam a identidade do assembly. Os dados também podem ser isolados por domínio de aplicativo, usando credenciais semelhantes.

Quando você usa armazenamento isolado, os aplicativos salvam dados em um compartimento de dados específico que está associado a algum aspecto da identidade do código, como seu site, o editor ou a assinatura. O compartimento de dados é uma abstração, não um local específico de armazenamento; ele consiste em um ou mais arquivos de armazenamento isolados, chamados de repositórios, que contêm os locais reais dos diretórios em que os dados estão armazenados. Por exemplo, um aplicativo pode ter um compartimento de dados associado a ele, e um diretório de sistema de arquivos implementaria o repositório que de fato preserva os dados para o aplicativo. Os dados salvos no repositório podem ser qualquer tipo de dados, desde informações de preferências do usuário até o estado do aplicativo. Para o desenvolvedor, o local do compartimento de dados é transparente. Os repositórios normalmente residem no cliente, mas um aplicativo de servidor pode usar repositórios isolados para armazenar informações que personificam o usuário em cujo nome ele está funcionando. Armazenamentos isolados também podem armazenar informações em um servidor com um perfil móvel de usuário para que as informações viajem com o usuário móvel.

<a name="quotas"></a>

## <a name="quotas-for-isolated-storage"></a>Cotas para armazenamento isolado

Uma cota é um limite na quantidade de armazenamento isolado que pode ser usada. A cota inclui bytes de espaço de arquivo, bem como a sobrecarga associada ao diretório e outras informações no repositório. O armazenamento isolado usa cotas de permissão, que são limites de armazenamento definidos com o uso de objetos <xref:System.Security.Permissions.IsolatedStoragePermission>. Se você tentar gravar os dados que excedam a cota, uma exceção <xref:System.IO.IsolatedStorage.IsolatedStorageException> será gerada.  Uma política de segurança, a qual pode ser modificada com a .NET Framework Configuration Tool (Mscorcfg.msc), determina quais permissões são concedidas ao código. O código que recebeu <xref:System.Security.Permissions.IsolatedStoragePermission> é restrito a não usar mais armazenamento do que o permitido pela propriedade <xref:System.Security.Permissions.IsolatedStoragePermission.UserQuota%2A>. No entanto, porque o código pode ignorar cotas de permissão ao apresentar identidades de usuário diferentes, as permissões de cotas servem mais como diretrizes de como o código deve se comportar em vez de atuar como um limite firme no comportamento do código.

As cotas não são aplicadas em repositórios móveis. Devido a isso, um nível um pouco mais alto de permissão é necessário para que o código as use. Os valores de enumeração <xref:System.Security.Permissions.IsolatedStorageContainment.AssemblyIsolationByRoamingUser> e <xref:System.Security.Permissions.IsolatedStorageContainment.DomainIsolationByRoamingUser> especificam uma permissão para usar o armazenamento isolado para um usuário móvel.

<a name="secure_access"></a>

## <a name="secure-access"></a>Acesso Seguro

Usar o armazenamento isolado permite que aplicativos parcialmente confiáveis armazenem dados de uma maneira que é controlada pela política de segurança do computador. Isto é especialmente útil para componentes baixados que um usuário talvez queira executar com cautela. Uma política de segurança raramente concede esse tipo de permissão de código quando você acessa o sistema de arquivos usando mecanismos padrão de E/S. No entanto, por padrão, código executado do computador local, de uma rede local ou da Internet recebe a permissão para usar o armazenamento isolado.

Os administradores podem limitar a quantidade de armazenamento isolado que um aplicativo ou usuário tem disponível com base em um nível de confiança apropriado. Além disso, os administradores podem remover todos os dados persistentes de um usuário. Para criar ou acessar um armazenamento isolado, o código deve receber a permissão <xref:System.Security.Permissions.IsolatedStorageFilePermission> apropriada.

Para acessar um armazenamento isolado, o código deve ter todos os direitos do sistema operacional da plataforma nativa necessários. As listas de controle de acesso (ACLs) que controlam quais usuários têm os direitos necessários para usar o sistema de arquivos devem ser satisfeitas. Os aplicativos .NET Framework já possuem direitos do sistema operacional para acessar armazenamentos isolados, a menos que eles executem a personificação (referente à plataforma). Nesse caso, o aplicativo é responsável por garantir que a identidade do usuário personificado tenha os direitos adequados do sistema operacional para acessar o armazenamento isolado. Esse acesso oferece uma maneira conveniente para que códigos que são executados ou baixados da Web possam ler e gravar em uma área de armazenamento relacionada a um usuário específico.

Para controlar o acesso ao armazenamento isolado, o Common Language Runtime utiliza objetos <xref:System.Security.Permissions.IsolatedStorageFilePermission>. Cada objeto tem propriedades que especificam os seguintes valores:

- Utilização permitida, que indica o tipo de acesso que é permitido. Os valores são membros da enumeração <xref:System.Security.Permissions.IsolatedStorageContainment>. Para obter mais informações sobre esses valores, consulte a tabela na próxima seção.

- Cota de armazenamento, conforme descrito na seção anterior.

O runtime exige a permissão <xref:System.Security.Permissions.IsolatedStorageFilePermission> quando o código tenta abrir um repositório pela primeira vez. Ele decide se essa permissão será concedida com base em quanto do código é confiável. Se a permissão é concedida, os valores de cota de armazenamento e de utilização permitida são determinados por política de segurança e pela solicitação do código para <xref:System.Security.Permissions.IsolatedStorageFilePermission>. A política de segurança é definida com o auxílio da .NET Framework Configuration Tool (Mscorcfg.msc). Todos os chamadores na pilha de chamadas são verificados para garantir que cada chamador tenha pelo menos a utilização permitida apropriada. O runtime também verifica a cota imposta no código que abriu ou criou o depósito no qual o arquivo será salvo. Se essas condições forem atendidas, a permissão será concedida. A cota é verificada novamente sempre que um arquivo é gravado no repositório.

O código do aplicativo não é obrigado a solicitar permissão porque o Common Language Runtime concederá qualquer <xref:System.Security.Permissions.IsolatedStorageFilePermission> apropriada com base na política de segurança. No entanto, existem bons motivos para solicitar as permissões específicas de que seu aplicativo necessita, incluindo <xref:System.Security.Permissions.IsolatedStorageFilePermission>.

<a name="allowed_usage"></a>

## <a name="allowed-usage-and-security-risks"></a>Uso permitido e riscos de segurança

O uso permitido especificado por <xref:System.Security.Permissions.IsolatedStorageFilePermission> determina o grau ao qual o código terá permissão para criar e usar o armazenamento isolado. A tabela a seguir mostra como o uso permitido especificado na permissão corresponde a tipos de isolamento e resume os riscos de segurança associados a cada uso permitido.

|Uso permitido|Tipos de isolamento|Impacto de segurança|
|-------------------|---------------------|---------------------|
|<xref:System.Security.Permissions.IsolatedStorageContainment.None>|Nenhum armazenamento isolado é permitido.|Não há impacto de segurança.|
|<xref:System.Security.Permissions.IsolatedStorageContainment.DomainIsolationByUser>|Isolamento por usuário, domínio e assembly Cada assembly possui um sub-repositório separado dentro do domínio. Repositórios que usam essa permissão também são implicitamente isolados por computador.|Esse nível de permissão deixa recursos abertos para superutilização não autorizada, embora cotas impostas tornem isso mais difícil. Isso é denominado um ataque de negação de serviço.|
|<xref:System.Security.Permissions.IsolatedStorageContainment.DomainIsolationByRoamingUser>|Igual a `DomainIsolationByUser`, mas o armazenamento é salvo em um local que irá transitar se os perfis de usuário móvel estiverem ativados e as cotas não forem aplicadas.|Como as cotas devem ser desativadas, os recursos de armazenamento são mais vulneráveis a um ataque de negação de serviço.|
|<xref:System.Security.Permissions.IsolatedStorageContainment.AssemblyIsolationByUser>|Isolamento por usuário e assembly Repositórios que usam essa permissão também são implicitamente isolados por computador.|As cotas serão aplicadas nesse nível para ajudar a evitar um ataque de negação de serviço. O mesmo assembly no outro domínio pode acessar esse repositório, abrindo a possibilidade de vazamento de informações entre aplicativos.|
|<xref:System.Security.Permissions.IsolatedStorageContainment.AssemblyIsolationByRoamingUser>|Igual a `AssemblyIsolationByUser`, mas o armazenamento é salvo em um local que irá transitar se os perfis de usuário móvel estiverem ativados e as cotas não forem aplicadas.|O mesmo que em `AssemblyIsolationByUser`, mas sem as cotas, o risco de um ataque de negação de serviço aumenta.|
|<xref:System.Security.Permissions.IsolatedStorageContainment.AdministerIsolatedStorageByUser>|Isolamento por usuário. Normalmente, apenas ferramentas administrativas ou de depuração usam esse nível de permissão.|O acesso com essa permissão permite que o código exiba ou exclua qualquer um dos arquivos ou diretórios de armazenamento isolados do usuário (independentemente do isolamento do assembly). Riscos incluem, mas não estão limitados a, vazamento de informações e perda de dados.|
|<xref:System.Security.Permissions.IsolatedStorageContainment.UnrestrictedIsolatedStorage>|Isolamento por todos os usuários, domínios e assemblies. Normalmente, apenas ferramentas administrativas ou de depuração usam esse nível de permissão.|Esta permissão cria o potencial de um comprometimento total de todos os repositórios isolados para todos os usuários.|

## <a name="safety-of-isolated-storage-components-with-regard-to-untrusted-data"></a>Segurança de componentes de armazenamento isolado com relação a dados não confiáveis

__Esta seção se aplica às seguintes estruturas:__

- .NET Framework (todas as versões)
- .NET Core 2.1 +
- .NET 5.0 +

O .NET Framework e o .NET Core oferecem armazenamento isolado como um mecanismo para manter dados para um usuário, um aplicativo ou um componente. Esse é um componente herdado projetado principalmente para cenários de segurança de acesso a código preteridos.

Várias APIs e ferramentas de armazenamento isolado podem ser usadas para ler dados entre limites de confiança. Por exemplo, a leitura de dados de um escopo em todo o computador pode agregar dados de outras contas de usuário possivelmente menos confiáveis no computador. Os componentes ou aplicativos que lêem os escopos de armazenamento isolado em toda a máquina devem estar cientes das consequências de ler esses dados.

### <a name="security-sensitive-apis-that-can-read-from-the-machine-wide-scope"></a>APIs sensíveis à segurança que podem ser lidas no escopo de todo o computador

Componentes ou aplicativos que chamam qualquer uma das seguintes APIs lidas do escopo de todo o computador:

* [IsolatedStorageFile. GetEnumerator](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.getenumerator), passando um escopo que inclui o sinalizador IsolatedStorageScope. Machine
* [IsolatedStorageFile. GetMachineStoreForApplication](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.getmachinestoreforapplication)
* [IsolatedStorageFile. GetMachineStoreForAssembly](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.getmachinestoreforassembly)
* [IsolatedStorageFile. GetMachineStoreForDomain](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.getmachinestorefordomain)
* [IsolatedStorageFile. GetStore](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.getstore), passando um escopo que inclui o sinalizador IsolatedStorageScope. Machine
* [IsolatedStorageFile. Remove](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.remove), passando um escopo que inclui o `IsolatedStorageScope.Machine` sinalizador

A [ferramenta de armazenamento isolado](../../framework/tools/storeadm-exe-isolated-storage-tool.md) `storeadm.exe` será afetada se for chamada com a `/machine` opção, conforme mostrado no código a seguir:

```txt
storeadm.exe /machine [any-other-switches]
```

A ferramenta de armazenamento isolado é fornecida como parte do Visual Studio e do SDK do .NET Framework.

Se o aplicativo não envolver chamadas para as APIs anteriores, ou se o fluxo de trabalho não envolve a chamada `storeadm.exe` dessa maneira, este documento não se aplica.

### <a name="impact-in-multi-user-environments"></a>Impacto em ambientes de vários usuários

Conforme mencionado anteriormente, o impacto de segurança dessas APIs resulta de dados gravados de um ambiente de confiança é lido de um ambiente de confiança diferente. O armazenamento isolado geralmente usa um dos três locais para ler e gravar dados:

1. `%LOCALAPPDATA%\IsolatedStorage\`: Por exemplo, `C:\Users\<username>\AppData\Local\IsolatedStorage\` para `User` escopo.
2. `%APPDATA%\IsolatedStorage\`: Por exemplo, `C:\Users\<username>\AppData\Roaming\IsolatedStorage\` para `User|Roaming` escopo.
3. `%PROGRAMDATA%\IsolatedStorage\`: Por exemplo, `C:\ProgramData\IsolatedStorage\` para `Machine` escopo.

Os dois primeiros locais são isolados por usuário. O Windows garante que diferentes contas de usuário no mesmo computador não possam acessar as pastas de perfil de usuário. Duas contas de usuário diferentes que usam `User` os `User|Roaming` repositórios ou não verão os dados uns dos outros e não poderão interferir nos dados uns dos outros.

O terceiro local é compartilhado entre todas as contas de usuário no computador. Contas diferentes podem ler e gravar nesse local e são capazes de ver os dados uns dos outros.

Os caminhos anteriores podem ser diferentes com base na versão do Windows em uso.

Agora, considere um sistema de vários usuários com dois usuários registrados _Mallory_ e _Bob_. O Mallory tem a capacidade de acessar seu diretório de perfil de usuário `C:\Users\Mallory\` e pode acessar o local de armazenamento em todo o computador compartilhado `C:\ProgramData\IsolatedStorage\` . Ela não pode acessar o diretório de perfil de usuário de Bob `C:\Users\Bob\` .

Se Mallory quiser atacar Bob, ela poderá gravar dados no local de armazenamento em todo o computador e, em seguida, tentar influenciar Bob na leitura do repositório de todo o computador. Quando Bob executa um aplicativo que lê esse armazenamento, esse aplicativo funcionará nos dados Mallory colocados ali, mas de dentro do contexto da conta de usuário de Bob. O restante deste documento contemplates vários vetores de ataque e quais etapas os aplicativos podem fazer para minimizar seus riscos a esses ataques.

__Observação:__ Para que tal ataque ocorra, o Mallory requer:

* Uma conta de usuário no computador.
* A capacidade de inserir um arquivo em um local conhecido no sistema de arquivos.
* O conhecimento que Bob irá, em algum momento, executar um aplicativo que tenta ler esses dados.

Esses não são vetores de ameaça que se aplicam a ambientes de desktop de usuário único padrão como PCs domésticos ou estações de trabalho de um funcionário corporativo.

#### <a name="elevation-of-privilege"></a>Elevação de privilégio

Um ataque __de elevação de privilégio__ ocorre quando o aplicativo de Bob lê o arquivo de Mallory e tenta automaticamente executar alguma ação com base no conteúdo dessa carga. Considere um aplicativo que leia o conteúdo de um script de inicialização do repositório de todo o computador e passe esse conteúdo para `Process.Start` . Se o Mallory puder fazer um script mal-intencionado dentro da loja de todo o computador, quando Bob iniciar seu aplicativo:

* Seu aplicativo analisa e inicia o script mal-intencionado do Mallory _no contexto do perfil de usuário de Bob_.
* Mallory acessa a conta do Bob no computador local.

#### <a name="denial-of-service"></a>Negação de serviço

Um ataque __de negação de serviço__ ocorre quando o aplicativo de Bob lê o arquivo do Mallory e falha ou para de funcionar corretamente. Considere novamente o aplicativo mencionado anteriormente, que tenta analisar um script de inicialização do repositório de todo o computador. Se Mallory puder posicionar um arquivo com conteúdo malformado dentro da loja de todo o computador, ela poderá:

* Fazer com que o aplicativo de Bob lance uma exceção no início do caminho de inicialização.
* Impedir que o aplicativo seja iniciado com êxito devido à exceção.

Ela negou, em seguida, Bob a capacidade de iniciar o aplicativo em sua própria conta de usuário.

#### <a name="information-disclosure"></a>Divulgação de informações confidenciais

Um ataque de __divulgação de informações__ ocorre quando o Mallory pode enganar Bob em revelar o conteúdo de um arquivo ao qual o Mallory normalmente não tem acesso. Considere que Bob tem um arquivo secreto *C:\Users\Bob\secret.txt* que o Mallory deseja ler. Ela sabe o caminho para esse arquivo, mas ela não pode lê-lo porque o Windows proíbe a obtenção de acesso ao diretório de perfil do usuário de Bob.

Em vez disso, o Mallory coloca um link físico no repositório de todo o computador. Esse é um tipo especial de arquivo que não contém nenhum conteúdo, em vez disso, aponta para outro arquivo no disco. A tentativa de ler o arquivo de link físico lerá o conteúdo do arquivo de destino pelo link. Depois de criar o link físico, o Mallory ainda não pode ler o conteúdo do arquivo porque ele não tem acesso ao destino ( `C:\Users\Bob\secret.txt` ) do link. No entanto _, Bob tem_ acesso a esse arquivo.

Quando o aplicativo de Bob lê da loja em todo o computador, ele agora lê inadvertidamente o conteúdo de seu `secret.txt` arquivo, como se o próprio arquivo estivesse presente no repositório de todo o computador. Quando o aplicativo de Bob é encerrado, se tentar salvar o arquivo novamente no repositório de todo o computador, ele acabará colocando uma cópia real do arquivo no diretório * C:\ProgramData\IsolatedStorage \* . Como esse diretório é legível por qualquer usuário no computador, o Mallory agora pode ler o conteúdo do arquivo.

### <a name="best-practices-to-defend-against-these-attacks"></a>Práticas recomendadas para a proteção contra esses ataques

__Importante:__ Se o seu ambiente tiver vários usuários mutuamente não confiáveis, __não__ chame a API `IsolatedStorageFile.GetEnumerator(IsolatedStorageScope.Machine)` nem invoque a ferramenta `storeadm.exe /machine /list` . Ambos pressupõem que eles estão operando em dados confiáveis. Se um invasor puder propagar uma carga mal-intencionada na loja de todo o computador, essa carga poderá levar a uma elevação de ataque de privilégio sob o contexto do usuário que executa esses comandos.

Se estiver operando em um ambiente de vários usuários, reconsidere o uso de recursos de armazenamento isolado que se destinam ao escopo da _máquina_ . Se um aplicativo precisar ler dados de um local em todo o computador, prefira ler os dados de um local que são graváveis somente por contas de administrador. O `%PROGRAMFILES%` diretório e o `HKLM` hive do registro são exemplos de locais que são graváveis somente por administradores e legíveis por todos. Portanto, os dados lidos desses locais são considerados confiáveis.

Se um aplicativo precisar usar o escopo da _máquina_ em um ambiente de vários usuários, valide o conteúdo de qualquer arquivo que você ler do repositório de todo o computador. Se o aplicativo desserializar grafos de objeto desses arquivos, considere usar serializadores mais seguros como `XmlSerializer` em vez de serializadores perigosos como `BinaryFormatter` ou `NetDataContractSerializer` . Tenha cuidado com grafos de objeto profundamente aninhados ou grafos de objetos que executam a alocação de recursos com base no conteúdo do arquivo.

<a name="isolated_storage_locations"></a>

## <a name="isolated-storage-locations"></a>Locais de Armazenamento Isolados

Às vezes é útil verificar uma alteração em um armazenamento usando o sistema de arquivos do sistema operacional. Talvez você também queira saber o local dos arquivos de armazenamento isolados. Esse local é diferente dependendo do sistema operacional. A tabela a seguir mostra os locais raiz em que um armazenamento isolado é criado em alguns sistemas operacionais comuns. Procure diretórios Microsoft\IsolatedStorage neste local raiz. Você deve alterar as configurações de pasta para mostrar arquivos e pastas ocultos para ver um armazenamento isolado no sistema de arquivos.

|Sistema operacional|Localização no sistema de arquivos|
|----------------------|-----------------------------|
|Windows 2000, Windows XP, Windows Server 2003 (atualização do Windows NT 4.0)|Repositórios com suporte a uso móvel =<br /><br /> \<SYSTEMROOT>\Profiles \\<o usuário \> \Application Data<br /><br /> Repositórios não móveis =<br /><br /> \<SYSTEMROOT>\Profiles \\<usuário \> \Local Settings\Application Data|
|Windows 2000 – Instalação limpa (e atualizações do Windows 98 e Windows NT 3.51)|Repositórios com suporte a uso móvel =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings \\<o usuário \> \Application Data<br /><br /> Repositórios não móveis =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings \\<usuário \> \Local Settings\Application Data|
|Windows XP, Windows Server 2003 – Instalação limpa (e atualizações do Windows 2000 e Windows 98)|Repositórios com suporte a uso móvel =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings \\<o usuário \> \Application Data<br /><br /> Repositórios não móveis =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings \\<usuário \> \Local Settings\Application Data|
|Windows 8, Windows 7, Windows Server 2008, Windows Vista|Repositórios com suporte a uso móvel =<br /><br /> \<SYSTEMDRIVE>\Users \\<usuário \> \AppData\Roaming<br /><br /> Repositórios não móveis =<br /><br /> \<SYSTEMDRIVE>\Users \\<usuário \> \AppData\Local|

<a name="isolated_storage_tasks"></a>

## <a name="creating-enumerating-and-deleting-isolated-storage"></a>Criando, Enumerando e Excluindo Armazenamento Isolado

O .NET Framework fornece três classes no namespace <xref:System.IO.IsolatedStorage> para ajudar a executar as tarefas que envolvem o armazenamento isolado:

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>, que é derivado de <xref:System.IO.IsolatedStorage.IsolatedStorage?displayProperty=nameWithType> e fornece gerenciamento básico de assemblies armazenados e arquivos de aplicativos. Uma instância da classe <xref:System.IO.IsolatedStorage.IsolatedStorageFile> representa um único repositório localizado no sistema de arquivos.

- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream> deriva de <xref:System.IO.FileStream?displayProperty=nameWithType> e fornece acesso aos arquivos em um repositório.

- <xref:System.IO.IsolatedStorage.IsolatedStorageScope> é uma enumeração que permite que você crie e selecione um repositório com o tipo de isolamento apropriado.

As classes de armazenamento isolado permitem que você crie, enumere e exclua o armazenamento isolado. Os métodos para a execução dessas tarefas estão disponíveis através do objeto <xref:System.IO.IsolatedStorage.IsolatedStorageFile>. Algumas operações exigem que você tenha a permissão <xref:System.Security.Permissions.IsolatedStorageFilePermission> que representa o direito para administrar o armazenamento isolado; você também pode precisar ter direitos do sistema operacional para acessar o arquivo ou diretório.

Para uma série de exemplos que demonstram tarefas de armazenamento isoladas comuns, confira os tópicos listados em [Tópicos relacionados](#related_topics).

<a name="scenarios_for_isolated_storage"></a>

## <a name="scenarios-for-isolated-storage"></a>Cenários para armazenamento isolado

O armazenamento isolado é útil em muitas situações, inclusive nestes quatro cenários:

- Controles baixados. Controles de código gerenciado baixados da Internet não têm permissão para escrever no disco rígido por meio de classes normais de E/S, mas eles podem usar o armazenamento isolado para persistir as configurações do usuário e o estado dos aplicativos.

- Armazenamento de componentes compartilhado. Componentes que são compartilhados entre aplicativos podem usar o armazenamento isolado para fornecer acesso controlado a repositórios de dados.

- Armazenamento de servidor. Aplicativos de servidor podem usar o armazenamento isolado para fornecer armazenamento individual para um grande número de usuários que estão realizando solicitações ao aplicativo. Pelo fato do armazenamento isolado ser sempre segredado por usuário, o servidor deve personificar o usuário que fez a solicitação. Nesse caso, os dados são isolados com base na identidade do principal, que é a mesma identidade que o aplicativo usa para fazer a distinção entre os seus usuários.

- Roaming. Os aplicativos também podem usar o armazenamento isolado com perfis de usuários móveis. Isso permite que repositórios isolados de um usuário façam roaming com o perfil.

Você não deve usar um armazenamento isolado nas seguintes situações:

- Para armazenar segredos de alto valor, como chaves sem criptografia ou senhas, pois o armazenamento isolado não é protegido contra código altamente confiável, código não gerenciado ou usuários confiáveis do computador.

- Para armazenar código.

- Para armazenar configurações e opções de implantação, as quais são controladas pelos administradores. (As preferências do usuário não são consideradas como definições de configuração, pois os administradores não as controlam.)

Muitos aplicativos usam bancos de dados para armazenar e isolar os dados. Nesse caso uma ou mais linhas em um banco de dados podem representar o armazenamento para um usuário específico. Você pode optar por usar armazenamento isolado em vez de um banco de dados quando o número de usuários é pequeno, quando as despesas no uso de um banco de dados é significativa ou quando não existe nenhum recurso de banco de dados. Além disso, quando o aplicativo requer um armazenamento que seja mais flexível e complexo do que aquele fornecido por uma linha em um banco de dados, o armazenamento isolado pode fornecer uma alternativa viável.

<a name="related_topics"></a>

## <a name="related-articles"></a>Artigos relacionados

|Título|Descrição|
|-----------|-----------------|
|[Tipos de isolamento](types-of-isolation.md)|Descreve os diferentes tipos de isolamento.|
|[Como: Obter repositórios para o armazenamento isolado](how-to-obtain-stores-for-isolated-storage.md)|Fornece um exemplo de uso da classe <xref:System.IO.IsolatedStorage.IsolatedStorageFile> para obter um armazenamento isolado por usuário e assembly.|
|[Como: Enumerar repositórios para o armazenamento isolado](how-to-enumerate-stores-for-isolated-storage.md)|Mostra como usar o método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetEnumerator%2A?displayProperty=nameWithType> para calcular o tamanho de todo o armazenamento isolado para o usuário.|
|[Como: Excluir repositórios no armazenamento isolado](how-to-delete-stores-in-isolated-storage.md)|Mostra como usar o método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.Remove%2A?displayProperty=nameWithType> de duas maneiras diferentes para excluir repositórios isolados.|
|[Como: Prever condições de espaço insuficiente com o armazenamento isolado](how-to-anticipate-out-of-space-conditions-with-isolated-storage.md)|Mostra como a medir o espaço restante em um armazenamento isolado.|
|[Como: Criar arquivos e diretórios no armazenamento isolado](how-to-create-files-and-directories-in-isolated-storage.md)|Fornece alguns exemplos de criação de arquivos e diretórios em um repositório isolado.|
|[Como: Localizar arquivos e diretórios existentes no armazenamento isolado](how-to-find-existing-files-and-directories-in-isolated-storage.md)|Demonstra como ler a estrutura de diretórios e arquivos no armazenamento isolado.|
|[Como: Ler e gravar em arquivos no armazenamento isolado](how-to-read-and-write-to-files-in-isolated-storage.md)|Fornece um exemplo de gravação de uma cadeia de caracteres em um arquivo de armazenamento isolado, seguida por sua leitura de volta.|
|[Como: Excluir arquivos e diretórios no armazenamento isolado](how-to-delete-files-and-directories-in-isolated-storage.md)|Demonstra como excluir arquivos e diretórios isolados.|
|[Arquivo e e/s de fluxo](index.md)|Explica como você pode executar acesso síncrono e assíncrono a fluxos de dados e arquivos.|

<a name="reference"></a>

## <a name="reference"></a>Referência

- <xref:System.IO.IsolatedStorage.IsolatedStorage?displayProperty=nameWithType>

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile?displayProperty=nameWithType>

- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream?displayProperty=nameWithType>

- <xref:System.IO.IsolatedStorage.IsolatedStorageScope?displayProperty=nameWithType>
