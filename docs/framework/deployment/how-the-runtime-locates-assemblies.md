---
title: Como o runtime localiza assemblies
description: Saiba como o Common Language Runtime (CLR) localiza e se associa aos assemblies que compõem seu aplicativo no .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- app.config files, assembly locations
- deploying applications [.NET Framework], assembly locations
- application configuration files, locating assemblies
- .NET Framework application deployment, locating assemblies
- locating assemblies
- assemblies [.NET Framework], location
ms.assetid: 772ac6f4-64d2-4cfb-92fd-58096dcd6c34
ms.openlocfilehash: 4cf1e5787fe2e430d20208d8e79b610e9126c67c
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622621"
---
# <a name="how-the-runtime-locates-assemblies"></a>Como o runtime localiza assemblies

Para implantar seu aplicativo .NET Framework com êxito, você deve entender como o Common Language Runtime localiza e associa aos assemblies que compõem seu aplicativo. Por padrão, o runtime tenta associar com a versão exata de um assembly com o qual o aplicativo foi criado. Esse comportamento padrão pode ser substituído pelas configurações do arquivo de configuração.

O Common Language Runtime executa uma série de etapas ao tentar localizar um assembly e resolver uma referência de assembly. Cada etapa é explicada nas seções a seguir. A investigação de termo normalmente é usada para descrever como o runtime localiza os assemblies, ela referencia o conjunto de heurística usado para localizar o assembly com base em seu nome e cultura.

> [!NOTE]
> Você pode exibir informações de associação no arquivo de log usando o [Visualizador de Log de Associação de Assembly (Fuslogvw.exe)](../tools/fuslogvw-exe-assembly-binding-log-viewer.md), incluído no SDK do Windows.

## <a name="initiating-the-bind"></a>Iniciando a associação

O processo de localização e associação a um assembly começa quando o runtime tenta resolver uma referência a outro assembly. Essa referência pode ser estática ou dinâmica. O compilador registra as referências estáticas nos metadados do manifesto do assembly no tempo de build. Referências dinâmicas são construídas em tempo real como resultado de chamar vários métodos, como <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>.

A melhor maneira de referenciar um assembly é usar uma referência completa, incluindo o nome do assembly, a versão, a cultura e o token de chave pública (se houver). O runtime usa essas informações para localizar o assembly, seguindo as etapas descritas posteriormente nesta seção. O runtime usa o mesmo processo de resolução, independentemente de se a referência é para um assembly estático ou dinâmico.

Você também pode fazer uma referência dinâmica a um assembly fornecendo o método de chamada apenas com informações parciais sobre o assembly, como especificando apenas o nome do assembly. Nesse caso, somente o diretório do aplicativo é pesquisado par ao assembly e não ocorre nenhuma outra verificação. Você faz uma referência parcial usando qualquer um dos vários métodos para carregar assemblies como <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> ou <xref:System.AppDomain.Load%2A?displayProperty=nameWithType>.

Por fim, você pode fazer uma referência dinâmica usando um método como <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> e fornecer apenas informações parciais; em seguida, você qualifica a referência usando o [\<qualifyAssembly>](../configure-apps/file-schema/runtime/qualifyassembly-element.md) elemento no arquivo de configuração do aplicativo. Esse elemento permite que você forneça as informações de referência completa (nome, versão, cultura e, se aplicável, o token de chave pública) em seu arquivo de configuração de aplicativo em vez de no seu código. Você usaria essa técnica se desejasse qualificar completamente uma referência a um assembly fora do diretório do aplicativo ou se desejasse referenciar um assembly no cache de assembly global, mas quisesse a conveniência de especificar a referência completa no arquivo de configuração em vez de no código.

> [!NOTE]
> Esse tipo de referência parcial não deve ser usado com assemblies que são compartilhados entre vários aplicativos. Como as definições de configuração são aplicadas por aplicativo e não por assembly, um assembly compartilhado usando esse tipo de referência parcial exigiria que cada aplicativo usando o assembly compartilhado tivesse as informações de qualificação no seu arquivo de configuração.

O runtime usa as seguintes etapas para resolver uma referência de assembly:

1. [Determina a versão do assembly correta](#step1) examinando arquivos de configuração aplicáveis, inclusive o arquivo de configuração de aplicativo, o arquivo de política de publicador e o arquivo de configuração de computador. Se o arquivo de configuração estiver localizado em um computador remoto, o runtime deverá localizar e baixar o arquivo de configuração de aplicativo primeiro.

2. [Verifica se o nome do assembly foi associado antes](#step2) e, em caso afirmativo, usa o assembly carregado anteriormente. Se uma solicitação anterior para carregar o assembly falhou, a solicitação falhará imediatamente sem tentar carregar o assembly.

    > [!NOTE]
    > O cache de falhas de associação de assembly é uma novidade no .NET Framework versão 2.0.

3. [Verifica o cache de assembly global](#step3). Se o assembly for encontrado lá, o runtime usará esse assembly.

4. [Investiga o assembly](#step4) usando as seguintes etapas:

    1. Se a política do publicador e a configuração não afetam a referência original e se a solicitação de associação foi criada usando o método <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>, o runtime procura dicas de localização.

    2. Se uma base de código for encontrada nos arquivos de configuração, o runtime verificará apenas esse local. Se essa investigação falhar, o runtime determinará que a solicitação de associação falhou e não ocorrem outras investigações.

    3. Investiga o assembly usando a heurística descrita na [seção de investigação](#step4). Se o assembly não for encontrado após a investigação, o runtime solicitará que o Windows Installer forneça o assembly. Isso funciona como um recurso de instalação sob demanda.

        > [!NOTE]
        > Não há nenhuma verificação de versão para os assemblies sem nomes fortes e o runtime não faz o check-in do cache de assembly global se há assemblies sem nomes fortes.

<a name="step1"></a>

## <a name="step-1-examining-the-configuration-files"></a>Etapa 1: Examinando os arquivos de configuração

O comportamento de associação do assembly pode ser configurado em diferentes níveis com base em três arquivos XML:

- Arquivo de configuração de aplicativo.

- Arquivo de política do publicador.

- Arquivo de configuração do computador.

Esses arquivos seguem a mesma sintaxe e fornecem informações como redirecionamentos de associação, o local do código e modos de associação para assemblies específicos. Cada arquivo de configuração pode conter um [ \<assemblyBinding> elemento](../configure-apps/file-schema/runtime/assemblybinding-element-for-runtime.md) que redireciona o processo de associação. Os elementos filho do [ \<assemblyBinding> elemento](../configure-apps/file-schema/runtime/assemblybinding-element-for-runtime.md) incluem o [ \<dependentAssembly> elemento](../configure-apps/file-schema/runtime/dependentassembly-element.md). Os filhos do [ \<dependentAssembly> elemento](../configure-apps/file-schema/runtime/dependentassembly-element.md) incluem o [ \<assemblyIdentity> elemento](/visualstudio/deployment/assemblyidentity-element-clickonce-deployment), o [ \<bindingRedirect> elemento](../configure-apps/file-schema/runtime/bindingredirect-element.md)e o [ \<codeBase> elemento](../configure-apps/file-schema/runtime/codebase-element.md).

> [!NOTE]
> As informações de configuração podem ser encontradas nos três arquivos de configuração, nem todos os elementos são válidos em todos os arquivos de configuração. Por exemplo, as informações de caminho particular e modo de associação podem estar somente no arquivo de configuração de aplicativo. Para obter uma lista completa das informações que estão contidas em cada arquivo, consulte [Configuring Apps by Using Configuration Files](../configure-apps/index.md) (Configurando aplicativos usando arquivos de configuração).

### <a name="application-configuration-file"></a>Arquivo de Configuração do Aplicativo

Primeiro, o Common Language Runtime verifica o arquivo de configuração de aplicativo quanto a informações que substituem as informações de versão armazenadas no manifesto do assembly de chamada. O arquivo de configuração de aplicativo pode ser implantado com um aplicativo, mas não é necessário para a execução do aplicativo. Geralmente, a recuperação desse arquivo é quase instantânea, mas em situações em que a base de aplicativo está em um computador remoto, como em um cenário baseado na Web do Internet Explorer, o arquivo de configuração deve ser baixado.

Para executáveis de cliente, o arquivo de configuração de aplicativo reside no mesmo diretório do executável do aplicativo e tem o mesmo nome base do executável com uma extensão .config. Por exemplo, o arquivo de configuração para C:\Program Files\Myapp\Myapp.exe é C:\Program Files\Myapp\Myapp.exe.config. Em um cenário baseado em navegador, o arquivo HTML deve usar o **\<link>** elemento para apontar explicitamente para o arquivo de configuração.

O código a seguir fornece um exemplo simples de um arquivo de configuração de aplicativo. Este exemplo adiciona um <xref:System.Diagnostics.TextWriterTraceListener> à coleção <xref:System.Diagnostics.Debug.Listeners%2A> para habilitar a gravação de informações de depuração em um arquivo.

```xml
<configuration>
   <system.diagnostics>
      <trace useGlobalLock="false" autoflush="true" indentsize="0">
         <listeners>
            <add name="myListener" type="System.Diagnostics.TextWriterTraceListener, system version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" initializeData="c:\myListener.log" />
         </listeners>
      </trace>
   </system.diagnostics>
</configuration>
```

### <a name="publisher-policy-file"></a>Arquivo de política de publicador

Em segundo lugar, o runtime examina o arquivo de política de publicador, se houver. Os arquivos de política de publicador são distribuídos por um publicador de componente como uma correção ou atualização para um componente compartilhado. Esses arquivos contêm informações de compatibilidade emitidas pelo publicador do componente compartilhado que direciona uma referência de assembly para uma nova versão. Ao contrário de arquivos de configuração de computador e de aplicativo, os arquivos de política de publicador estão contidos em seu próprio assembly que deve ser instalado no cache de assembly global.

A seguir está um exemplo de um arquivo de configuração de política de publicador:

```xml
<configuration>
    <runtime>
        <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">

            <dependentAssembly>
                <assemblyIdentity name="asm6" publicKeyToken="c0305c36380ba429" />
                <bindingRedirect oldVersion="3.0.0.0" newVersion="2.0.0.0"/>
            </dependentAssembly>

        </assemblyBinding>
    </runtime>
</configuration>
```

Para criar um assembly, você pode usar a ferramenta [Al.exe (Assembly Linker)](../tools/al-exe-assembly-linker.md) com um comando como o seguinte:

```console
Al.exe /link:asm6.exe.config /out:policy.3.0.asm6.dll /keyfile: compatkey.dat /v:3.0.0.0
```

`compatkey.dat` é um arquivo de chave de nome forte. Este comando cria um assembly com nome forte, que você pode colocar no cache de assembly global.

> [!NOTE]
> A política de publicador afeta todos os aplicativos que usam um componente compartilhado.

O arquivo de configuração de política de publicador substitui as informações de versão vem do aplicativo (ou seja, do manifesto do assembly ou do arquivo de configuração de aplicativo). Se não houver nenhuma instrução no arquivo de configuração de aplicativo para redirecionar a versão especificada no manifesto do assembly, o arquivo de política de publicador substituirá a versão especificada no manifesto do assembly. No entanto, se houver uma instrução de redirecionamento no arquivo de configuração de aplicativo, a política de publicador substituirá essa versão, em vez da especificada no manifesto.

Um arquivo de política de publicador é usado quando um componente compartilhado é atualizado e a nova versão do componente compartilhado deve ser selecionada por todos os aplicativos que usam esse componente. As configurações no arquivo de política de publicador substituem as configurações no arquivo de configuração de aplicativo, a menos que o arquivo de configuração de aplicativo imponha o modo de segurança.

#### <a name="safe-mode"></a>Modo de segurança
Os arquivos de política de publicador normalmente são explicitamente instalados como parte de uma atualização do programa ou service pack. Se houver algum problema com o componente compartilhado atualizado, você poderá ignorar as substituições no arquivo de política de publicador usando o modo de segurança. O modo de segurança é determinado pelo **\<publisherPolicy apply="yes**&#124;**no"/>** elemento, localizado somente no arquivo de configuração do aplicativo. Ele especifica se as informações de configuração de política do publicador devem ser removidas do processo de associação.

O modo de segurança pode ser definido para todo o aplicativo ou para os assemblies selecionados. Ou seja, você pode desligar a política para todos os assemblies que compõem o aplicativo ou ativá-lo para alguns assemblies, mas não para outros. Para aplicar seletivamente a política do Publicador a assemblies que compõem um aplicativo, defina **\<publisherPolicy apply\=no/>** e especifique quais assemblies você deseja que sejam afetados usando o \<**dependentAssembly**> elemento. Para aplicar a política do Publicador a todos os assemblies que compõem o aplicativo, defina sem **\<publisherPolicy apply\=no/>** elementos de assembly dependentes. Para obter mais informações sobre a configuração, consulte [Configuring Apps by Using Configuration Files](../configure-apps/index.md) (Configurando aplicativos usando arquivos de configuração).

### <a name="machine-configuration-file"></a>Arquivo de configuração do computador
Em terceiro lugar, o runtime examina o arquivo de configuração do computador. Esse arquivo, chamado Machine.config, reside no computador local no subdiretório Config do diretório raiz em que o runtime está instalado. Esse arquivo pode ser usado por administradores para especificar restrições de associação de assembly que são locais no computador. As configurações no arquivo de configuração do computador têm precedência sobre todas as outras definições de configuração. No entanto, isso não significa que todas as definições de configuração devem ser colocadas nesse arquivo. A versão de determinada pelo arquivo de política de administrador é final e não pode ser substituída. As substituições especificadas no arquivo Machine.config afetam todos os aplicativos. Para obter mais informações sobre os arquivos de configuração, consulte [Configuring Apps by Using Configuration Files](../configure-apps/index.md) (Configurando aplicativos usando arquivos de configuração).

<a name="step2"></a>

## <a name="step-2-checking-for-previously-referenced-assemblies"></a>Etapa 2: Verificando se há assemblies referenciados anteriormente

Se o assembly solicitado também foi solicitado em chamadas anteriores, o Common Language Runtime usa o assembly que já está carregado. Isso pode ter ramificações ao nomear os assemblies que compõem um aplicativo. Para obter mais informações sobre a nomeação de assemblies, consulte [Assembly Names](../../standard/assembly/names.md) (Nomes de assembly).

Se uma solicitação anterior para o assembly falhou, as solicitações subsequentes para o assembly falharão imediatamente sem tentar carregar o assembly. Desde o .NET Framework versão 2.0, as falhas de associação de assembly são armazenadas em cache e as informações em cache são usadas para determinar se deve-se tentar carregar o assembly.

> [!NOTE]
> Para reverter para o comportamento do .NET Framework versões 1,0 e 1,1, que não armazenavam em cache falhas de associação, inclua o [ \<disableCachingBindingFailures> elemento](../configure-apps/file-schema/runtime/disablecachingbindingfailures-element.md) no arquivo de configuração.

<a name="step3"></a>

## <a name="step-3-checking-the-global-assembly-cache"></a>Etapa 3: Verificando o cache de assemblies global

Para assemblies de nome forte, o processo de associação continua procurando no cache de assembly global. O cache de assembly global armazena os assemblies que podem ser usados por vários aplicativos em um computador. Todos os assemblies no cache de assembly global devem ter nomes fortes.

<a name="step4"></a>

## <a name="step-4-locating-the-assembly-through-codebases-or-probing"></a>Etapa 4: Localizando o assembly por meio de codebases ou sondagem

Depois de determinar a versão do assembly correta usando as informações na referência do assembly realizando a chamada e nos arquivos de configuração e após ela ter feito check-in do cache de assembly global (somente para assemblies de nome forte), o Common Language Runtime tenta localizar o assembly. O processo de localização de um assembly envolve as seguintes etapas:

1. Se um [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) elemento for encontrado no arquivo de configuração do aplicativo, o tempo de execução verificará o local especificado. Se uma correspondência for encontrada, esse assembly será usado e nenhuma investigação ocorrerá. Se o assembly não for encontrado lá, a solicitação de associação falhará.

2. O runtime investiga em busca do assembly referenciado usando as regras especificadas posteriormente nesta seção.

> [!NOTE]
> Se você tiver várias versões de um assembly em um diretório e desejar fazer referência a uma versão específica desse assembly, deverá usar o [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) elemento em vez do `privatePath` atributo do [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) elemento. Se você usar o [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) elemento, o tempo de execução interromperá a investigação pela primeira vez que encontrar um assembly que corresponda ao nome de assembly simples referenciado, seja uma correspondência correta ou não. Se for uma correspondência correta, esse assembly será usado. Se não for uma correspondência correta, a investigação parará e a associação falhará.

### <a name="locating-the-assembly-through-codebases"></a>Localizando o assembly por meio de bases de código

Informações de CodeBase podem ser fornecidas usando um [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) elemento em um arquivo de configuração. Essa base de código sempre é verificada antes de o runtime tentar investigar o assembly referenciado. Se um arquivo de política do Publicador que contém o redirecionamento da versão final também contiver um [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) elemento, esse [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) elemento será aquele usado. Por exemplo, se o arquivo de configuração do aplicativo especificar um [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) elemento e um arquivo de política do Publicador que está substituindo as informações do aplicativo também especificar um [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) elemento, o [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) elemento no arquivo de política do Publicador será usado.

Se nenhuma correspondência for encontrada no local especificado pelo [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) elemento, a solicitação de ligação falhará e nenhuma outra etapa será executada. Se o runtime determina que um assembly corresponde aos critérios do assembly de chamada, ele usa esse assembly. Quando o arquivo especificado pelo elemento fornecido [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) é carregado, o tempo de execução verifica se o nome, a versão, a cultura e a chave pública correspondem à referência do assembly de chamada.

> [!NOTE]
> Os assemblies referenciados fora do diretório raiz do aplicativo devem ter nomes fortes e devem ser instalados no cache de assembly global ou especificados com o uso do [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) elemento.

### <a name="locating-the-assembly-through-probing"></a>Localizando o assembly por meio de investigação

Se não houver nenhum [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) elemento no arquivo de configuração do aplicativo, as investigações de tempo de execução para o assembly usando quatro critérios:

- Base de aplicativo, que é o local da raiz em que o aplicativo está sendo executado.

- Cultura, que é o atributo de cultura do assembly que está sendo referenciado.

- Nome, que é o nome do assembly referenciado.

- O `privatePath` atributo do [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) elemento, que é a lista definida pelo usuário de subdiretórios no local raiz. Esse local pode ser especificado no arquivo de configuração de aplicativo e no código gerenciado usando a propriedade <xref:System.AppDomainSetup.PrivateBinPath?displayProperty=nameWithType> para um domínio do aplicativo. Quando especificado no código gerenciado, o código gerenciado `privatePath` é analisado primeiro, seguido pelo caminho especificado no arquivo de configuração de aplicativo.

#### <a name="probing-the-application-base-and-culture-directories"></a>Investigando a base de aplicativo e os diretórios de cultura

O runtime sempre começa investigando a base do aplicativo, que pode ser uma URL ou diretório da raiz do aplicativo em um computador. Se o assembly referenciado não for encontrado na base de aplicativo e nenhuma informação de cultura for fornecida, o runtime pesquisará todos os subdiretórios com o nome do assembly. Os diretórios investigados incluem:

- [base de aplicativo] / [nome do assembly].dll

- [base de aplicativo] / [nome do assembly] / [nome do assembly].dll

Se informações de cultura forem especificadas para o assembly referenciado, apenas os diretórios a seguir serão investigados:

- [base de aplicativo] / [cultura] / [nome do assembly].dll

- [base de aplicativo] / [cultura] / [nome do assembly] / [nome do assembly].dll

#### <a name="probing-with-the-privatepath-attribute"></a>Investigando com o atributo privatePath

Além dos subdiretórios de cultura e dos subdiretórios nomeados para o assembly referenciado, o tempo de execução também investiga os diretórios especificados usando o `privatePath` atributo do [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) elemento. Os diretórios especificados usando o atributo `privatePath` devem ser subdiretórios do diretório raiz do aplicativo. Os diretórios investigados variam dependendo de se as informações de cultura estão incluídas na solicitação do assembly referenciado.

O runtime interromperá a investigação na primeira vez que encontrar um assembly que corresponde ao nome de assembly simples referenciado, independentemente de ser uma correspondência correta ou não. Se for uma correspondência correta, esse assembly será usado. Se não for uma correspondência correta, a investigação parará e a associação falhará.

Se a cultura for incluída, os diretórios a seguir serão investigados:

- [base de aplicativo] / [binpath] / [cultura] / [nome do assembly].dll

- [base de aplicativo] / [binpath] / [cultura] / [nome do assembly] / [nome do assembly].dll

Se as informações de cultura não forem incluídas, os diretórios a seguir serão investigados:

- [base de aplicativo] / [binpath] / [nome do assembly].dll

- [base de aplicativo] / [binpath] / [nome do assembly] / [nome do assembly].dll

#### <a name="probing-examples"></a>Exemplos de investigação

Considerando as seguintes informações:

- Nome do assembly referenciado: myAssembly

- Diretório raiz do aplicativo: `http://www.code.microsoft.com`

- [\<probing>](../configure-apps/file-schema/runtime/probing-element.md)o elemento no arquivo de configuração especifica: bin

- Cultura: de

O runtime investiga as seguintes URLs:

- `http://www.code.microsoft.com/de/myAssembly.dll`

- `http://www.code.microsoft.com/de/myAssembly/myAssembly.dll`

- `http://www.code.microsoft.com/bin/de/myAssembly.dll`

- `http://www.code.microsoft.com/bin/de/myAssembly/myAssembly.dll`

##### <a name="multiple-assemblies-with-the-same-name"></a>Vários assemblies com o mesmo nome

O exemplo a seguir mostra como configurar vários assemblies com o mesmo nome.

```xml
<dependentAssembly>
   <assemblyIdentity name="Server" publicKeyToken="c0305c36380ba429" />
   <codeBase version="1.0.0.0" href="v1/Server.dll" />
   <codeBase version="2.0.0.0" href="v2/Server.dll" />
</dependentAssembly>
```

#### <a name="other-locations-probed"></a>Outros locais investigados

O local do assembly também pode ser determinado usando o contexto de associação atual. Isso normalmente ocorre quando o método <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> é usado e em cenários de interoperabilidade COM. Se um assembly usa o método <xref:System.Reflection.Assembly.LoadFrom%2A> para referenciar outro assembly, o local do assembly de chamada é considerado uma dica de onde encontrar o assembly referenciado. Se uma correspondência for encontrada, esse assembly será carregado. Se nenhuma correspondência for encontrada, o runtime continuará com sua semântica de pesquisa e, em seguida, consultará o Windows Installer para fornecer o assembly. Se não for fornecido nenhum assembly correspondente à solicitação de associação, uma exceção será gerada. Essa exceção será uma <xref:System.TypeLoadException> no código gerenciado se um tipo tiver sido referenciado ou uma <xref:System.IO.FileNotFoundException> se um assembly sendo carregado não tiver sido encontrado.

Por exemplo, se o Assembly1 fizer referência ao Assembly2 e o Assembly1 tiver sido baixado de `http://www.code.microsoft.com/utils`, esse local será considerado uma dica sobre onde encontrar o Assembly2.dll. Em seguida, o runtime procura pelo assembly em `http://www.code.microsoft.com/utils/Assembly2.dll` e `http://www.code.microsoft.com/utils/Assembly2/Assembly2.dll`. Se Assembly2 não for encontrado em um desses locais, o runtime de consultará o Windows Installer.

## <a name="see-also"></a>Consulte também

- [Práticas recomendadas para carregamento de assemblies](best-practices-for-assembly-loading.md)
- [Implantação](index.md)
