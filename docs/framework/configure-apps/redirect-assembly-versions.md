---
title: Redirecionando versões de assembly
description: Redirecione referências de associação de tempo de compilação para diferentes versões de assemblies .NET, assemblies de terceiros ou assemblies de seu próprio aplicativo.
ms.date: 03/30/2017
helpviewer_keywords:
- assembly binding, redirection
- redirecting assembly binding to earlier version
- configuration [.NET Framework], applications
- application configuration [.NET Framework]
- assemblies [.NET Framework], binding redirection
ms.assetid: 88fb1a17-6ac9-4b57-8028-193aec1f727c
ms.openlocfilehash: 4cfd4336fb9999c996bea28eb86f1143932d4c51
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141728"
---
# <a name="redirecting-assembly-versions"></a>Redirecionando versões de assembly

Você pode redirecionar referências de associação de tempo de compilação para .NET Framework assemblies, assemblies de terceiros ou assemblies de seu próprio aplicativo. Você pode redirecionar seu aplicativo para usar uma versão diferente de um assembly de várias maneiras: por meio da política de editor, por meio de um arquivo de configuração de aplicativo; ou por meio do arquivo de configuração do computador. Este artigo discute como a associação de assembly funciona no .NET Framework e como ela pode ser configurada.

<a name="BKMK_Assemblyunificationanddefaultbinding"></a>
## <a name="assembly-unification-and-default-binding"></a>Unificação de assembly e associação padrão
 Associações a .NET Framework assemblies são, às vezes, redirecionadas por meio de um processo chamado *Unificação de assembly*. O .NET Framework consiste em uma versão do Common Language Runtime e cerca de duas dúzias de .NET Framework assemblies que compõem a biblioteca de tipos. Esses .NET Framework assemblies são tratados pelo tempo de execução como uma única unidade. Por padrão, quando um aplicativo é iniciado, todas as referências a tipos no código são executadas pelo tempo de execução são direcionadas para .NET Framework assemblies que têm o mesmo número de versão que o tempo de execução que é carregado em um processo. Os redirecionamentos que ocorrem com esse modelo são o comportamento padrão para o tempo de execução.

 Por exemplo, se seu aplicativo referencia tipos no namespace System.XML e foi criado usando o .NET Framework 4,5, ele contém referências estáticas ao assembly System.XML que é fornecido com a versão de tempo de execução 4,5. Se você quiser redirecionar a referência de associação para apontar para o System.XML assembly que acompanha o .NET Framework 4, você pode colocar as informações de redirecionamento no arquivo de configuração do aplicativo. Um redirecionamento de associação em um arquivo de configuração para um assembly de .NET Framework unificado cancela a unificação desse assembly.

 Além disso, talvez você queira redirecionar manualmente a associação de assembly para assemblies de terceiros se houver várias versões disponíveis.

<a name="BKMK_Redirectingassemblyversionsbyusingpublisherpolicy"></a>
## <a name="redirecting-assembly-versions-by-using-publisher-policy"></a>Redirecionando versões de assembly usando a política do Publicador
 Os fornecedores de assemblies podem direcionar aplicativos para uma versão mais recente de um assembly, incluindo um arquivo de política do Publicador com o novo assembly. O arquivo de política do Publicador, localizado no cache de assembly global, contém configurações de redirecionamento de assembly.

 Cada uma das *principais*. a versão *secundária* de um assembly tem seu próprio arquivo de política de editor. Por exemplo, redirecionamentos da versão 2.0.2.222 para 2.0.3.000 e da versão 2.0.2.321 para a versão 2.0.3.000 vão para o mesmo arquivo, pois eles estão associados à versão 2,0. No entanto, um redirecionamento da versão 3.0.0.999 para a versão 4.0.0.000 entra no arquivo para a versão 3.0.999. Cada versão principal do .NET Framework tem seu próprio arquivo de política de editor.

 Se um arquivo de política do Publicador existir para um assembly, o tempo de execução verificará esse arquivo depois de verificar o manifesto do assembly e o arquivo de configuração do aplicativo. Os fornecedores devem usar arquivos de política do Publicador somente quando o novo assembly for compatível com versões anteriores com o assembly sendo redirecionado.

 Você pode ignorar a política do Publicador para seu aplicativo especificando as configurações no arquivo de configuração do aplicativo, conforme discutido na [seção ignorando a política do editor](#bypass_PP).

<a name="BKMK_Redirectingassemblyversionsattheapplevel"></a>
## <a name="redirecting-assembly-versions-at-the-app-level"></a>Redirecionando versões de assembly no nível do aplicativo
 Há algumas técnicas diferentes para alterar o comportamento de associação para seu aplicativo por meio do arquivo de configuração de aplicativo: você pode editar o arquivo manualmente, você pode contar com o redirecionamento de associação automática ou pode especificar o comportamento de associação ignorando a política do Publicador.

### <a name="manually-editing-the-app-config-file"></a>Editando manualmente o arquivo de configuração do aplicativo
 Você pode editar manualmente o arquivo de configuração de aplicativo para resolver problemas de assembly. Por exemplo, se um fornecedor puder liberar uma versão mais recente de um assembly que seu aplicativo usa sem fornecer uma política de Publicador, porque eles não garantem a compatibilidade com versões anteriores, você pode direcionar seu aplicativo para usar a versão mais recente do assembly, colocando informações de associação de assembly no arquivo de configuração do aplicativo da seguinte maneira.

```xml
<dependentAssembly>
  <assemblyIdentity name="someAssembly"
    publicKeyToken="32ab4ba45e0a69a1"
    culture="en-us" />
  <bindingRedirect oldVersion="7.0.0.0" newVersion="8.0.0.0" />
</dependentAssembly>
```

### <a name="relying-on-automatic-binding-redirection"></a>Contando com o redirecionamento de associação automática

Quando você cria um aplicativo de área de trabalho no Visual Studio que tem como alvo o .NET Framework 4.5.1 ou uma versão posterior, o aplicativo usa o redirecionamento de associação automática. Isso significa que, se dois componentes fizerem referência a versões diferentes do mesmo assembly de nome forte, o tempo de execução adicionará automaticamente um redirecionamento de associação à versão mais recente do assembly no arquivo de configuração do aplicativo de saída (app.config). Esse redirecionamento substitui a Unificação de assembly que, de outra forma, poderia ocorrer. O arquivo de origem app.config não é modificado. Por exemplo, digamos que seu aplicativo faça referência diretamente a um componente de .NET Framework fora de banda, mas usa uma biblioteca de terceiros que se destina a uma versão mais antiga do mesmo componente. Quando você compila o aplicativo, o arquivo de configuração do aplicativo de saída é modificado para conter um redirecionamento de associação para a versão mais recente do componente. Se você criar um aplicativo Web, receberá um aviso de compilação sobre o conflito de associação que, por sua vez, oferecerá a opção de adicionar o redirecionamento de associação necessário ao arquivo de configuração da Web de origem.

Se você adicionar manualmente os redirecionamentos de associação ao arquivo de app.config de origem, no momento da compilação, o Visual Studio tentará unificar os assemblies com base nos redirecionamentos de associação adicionados. Por exemplo, digamos que você insira o redirecionamento de associação a seguir para um assembly:

`<bindingRedirect oldVersion="3.0.0.0" newVersion="2.0.0.0" />`

Se outro projeto em seu aplicativo fizer referência à versão 1.0.0.0 do mesmo assembly, o redirecionamento de associação automática adicionará a seguinte entrada ao arquivo de app.config de saída para que o aplicativo seja unificado na versão 2.0.0.0 deste assembly:

`<bindingRedirect oldVersion="1.0.0.0" newVersion="2.0.0.0" />`

Você pode habilitar o redirecionamento de associação automática se seu aplicativo for destinado a versões mais antigas do .NET Framework. Você pode substituir esse comportamento padrão fornecendo informações de redirecionamento de associação no arquivo de app.config para qualquer assembly ou desativando o recurso de redirecionamento de associação. Para obter informações sobre como ativar ou desativar esse recurso, consulte [como habilitar e desabilitar o redirecionamento de associação automática](how-to-enable-and-disable-automatic-binding-redirection.md).

<a name="bypass_PP"></a>
### <a name="bypassing-publisher-policy"></a>Ignorando a política do Publicador
 Você pode substituir a política do Publicador no arquivo de configuração do aplicativo, se necessário. Por exemplo, novas versões de assemblies que alegam ser compatíveis com versões anteriores ainda podem interromper um aplicativo. Se você quiser ignorar a política do Publicador, adicione um [\<publisherPolicy>](./file-schema/runtime/publisherpolicy-element.md) elemento ao [\<dependentAssembly>](./file-schema/runtime/dependentassembly-element.md) elemento no arquivo de configuração do aplicativo e defina o atributo **Apply** como **não**, que substitui as configurações **Sim** anteriores.

 `<publisherPolicy apply="no" />`

 Ignore a política do editor para manter seu aplicativo em execução para seus usuários, mas certifique-se de relatar o problema para o fornecedor do assembly. Se um assembly tiver um arquivo de política do Publicador, o fornecedor deverá garantir que o assembly seja compatível com versões anteriores e que os clientes possam usar a nova versão o máximo possível.

<a name="BKMK_Redirectingassemblyversionsatthemachinelevel"></a>
## <a name="redirecting-assembly-versions-at-the-machine-level"></a>Redirecionando versões de assembly no nível do computador
 Pode haver casos raros em que um administrador de máquina deseja que todos os aplicativos em um computador usem uma versão específica de um assembly. Por exemplo, o administrador pode querer que todos os aplicativos usem uma versão de assembly específica, pois essa versão corrige uma brecha de segurança. Se um assembly for redirecionado no arquivo de configuração da máquina, todos os aplicativos nesse computador que usam a versão antiga serão direcionados para usar a nova versão. O arquivo de configuração do computador substitui o arquivo de configuração do aplicativo e o arquivo de política do Publicador. Esse arquivo está localizado no diretório %*runtime install path*%\Config. Normalmente, o .NET Framework é instalado no diretório%drive%\Windows\Microsoft.NET\Framework

<a name="BKMK_Specifyingassemblybindinginconfigurationfiles"></a>
## <a name="specifying-assembly-binding-in-configuration-files"></a>Especificando a associação de assembly em arquivos de configuração
 Você usa o mesmo formato XML para especificar redirecionamentos de associação, seja no arquivo de configuração do aplicativo, no arquivo de configuração do computador ou no arquivo de política do Publicador. Para redirecionar uma versão de assembly para outra, use o [\<bindingRedirect>](./file-schema/runtime/bindingredirect-element.md) elemento. O atributo **OldVersion** pode especificar uma única versão de assembly ou um intervalo de versões. O `newVersion` atributo deve especificar uma única versão.  Por exemplo, `<bindingRedirect oldVersion="1.1.0.0-1.2.0.0" newVersion="2.0.0.0"/>` especifica que o tempo de execução deve usar a versão 2.0.0.0 em vez das versões de assembly entre 1.1.0.0 e 1.2.0.0.

 O exemplo de código a seguir demonstra uma variedade de cenários de redirecionamento de associação. O exemplo especifica um redirecionamento para um intervalo de versões para `myAssembly` , e um único redirecionamento de associação para `mySecondAssembly` . O exemplo também especifica que o arquivo de política do Publicador não substituirá os redirecionamentos de associação para `myThirdAssembly` .

 Para associar um assembly, você deve especificar a cadeia de caracteres "urn: schemas-microsoft-com: asm. v1" com o atributo **xmlns** na [\<assemblyBinding>](./file-schema/runtime/assemblybinding-element-for-runtime.md) marca.

```xml
<configuration>
  <runtime>
    <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
      <dependentAssembly>
        <assemblyIdentity name="myAssembly"
          publicKeyToken="32ab4ba45e0a69a1"
          culture="en-us" />
        <!-- Assembly versions can be redirected in app,
          publisher policy, or machine configuration files. -->
        <bindingRedirect oldVersion="1.0.0.0-2.0.0.0" newVersion="3.0.0.0" />
      </dependentAssembly>
      <dependentAssembly>
        <assemblyIdentity name="mySecondAssembly"
          publicKeyToken="32ab4ba45e0a69a1"
          culture="en-us" />
             <bindingRedirect oldVersion="1.0.0.0" newVersion="2.0.0.0" />
      </dependentAssembly>
      <dependentAssembly>
      <assemblyIdentity name="myThirdAssembly"
        publicKeyToken="32ab4ba45e0a69a1"
        culture="en-us" />
        <!-- Publisher policy can be set only in the app
          configuration file. -->
        <publisherPolicy apply="no" />
      </dependentAssembly>
    </assemblyBinding>
  </runtime>
</configuration>
```

### <a name="limiting-assembly--bindings-to-a-specific-version"></a>Limitando associações de assembly a uma versão específica
 Você pode usar o atributo **AppliesTo** no [\<assemblyBinding>](./file-schema/runtime/assemblybinding-element-for-runtime.md) elemento em um arquivo de configuração de aplicativo para redirecionar referências de associação de assembly para uma versão específica do .NET Framework. Esse atributo opcional usa um número de versão .NET Framework para indicar a qual versão ele se aplica. Se nenhum atributo **AppliesTo** for especificado, o [\<assemblyBinding>](./file-schema/runtime/assemblybinding-element-for-runtime.md) elemento se aplicará a todas as versões do .NET Framework.

 Por exemplo, para redirecionar a associação de assembly para um assembly .NET Framework 3,5, você incluiria o seguinte código XML em seu arquivo de configuração de aplicativo.

```xml
<runtime>
  <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1"
    appliesTo="v3.5">
    <dependentAssembly>
      <!-- assembly information goes here -->
    </dependentAssembly>
  </assemblyBinding>
</runtime>
```

 Você deve inserir informações de redirecionamento na ordem de versão. Por exemplo, insira informações de redirecionamento de associação de assembly para assemblies .NET Framework 3,5 seguidos por assemblies de .NET Framework 4,5. Por fim, insira as informações de redirecionamento de associação de assembly para qualquer redirecionamento de assembly do .NET Framework que não use o atributo **appliesTo** e, portanto, se aplica a todas as versões do .NET Framework. Se houver um conflito no redirecionamento, a primeira instrução de redirecionamento correspondente no arquivo de configuração será usada.

 Por exemplo, para redirecionar uma referência a um assembly .NET Framework 3,5 e outra referência a um assembly .NET Framework 4, use o padrão mostrado no pseudocódigo a seguir.

```xml
<assemblyBinding xmlns="..." appliesTo="v3.5 ">
  <!--.NET Framework version 3.5 redirects here -->
</assemblyBinding>

<assemblyBinding xmlns="..." appliesTo="v4.0.30319">
  <!--.NET Framework version 4.0 redirects here -->
</assemblyBinding>

<assemblyBinding xmlns="...">
  <!-- redirects meant for all versions of the runtime -->
</assemblyBinding>
```

## <a name="see-also"></a>Veja também

- [Como: Habilitar e desabilitar o redirecionamento automático de associação](how-to-enable-and-disable-automatic-binding-redirection.md)
- [\<bindingRedirect>Elementos](./file-schema/runtime/bindingredirect-element.md)
- [Permissão de segurança para redirecionamento de associações de assemblies](assembly-binding-redirection-security-permission.md)
- [Assemblies no .NET](../../standard/assembly/index.md)
- [Programação com assemblies](../../standard/assembly/index.md)
- [Como o runtime localiza assemblies](../deployment/how-the-runtime-locates-assemblies.md)
- [Configuração de aplicativos](index.md)
- [Esquema de configurações do runtime](./file-schema/runtime/index.md)
- [Esquema de arquivos de configuração](./file-schema/index.md)
- [Como: Criar uma política de editor](how-to-create-a-publisher-policy.md)
