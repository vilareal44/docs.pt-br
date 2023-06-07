---
title: Caspol.exe (Ferramenta de Política de Segurança de Acesso de Código)
description: Examine a Caspol.exe, a ferramenta de política CAS (segurança de acesso ao código). Essa ferramenta permite que os usuários e administradores modifiquem a política de segurança para diferentes níveis de política.
ms.date: 03/30/2017
helpviewer_keywords:
- permission sets, modifying security policy
- security policy [.NET Framework], Code Access Security Policy tool
- enterprise security policy
- referencing code groups and permission sets
- user security policy
- Caspol.exe
- Code Access Security Policy tool
- code groups, modifying security policy
- application development [.NET Framework], security
- machine security policy
- security policy [.NET Framework], modifying
- manually editing security configuration files
ms.assetid: d2bf6123-7b0c-4e60-87ad-a39a1c3eb2e0
ms.openlocfilehash: 4d29c22c09e42be5596d860d90b182e512ad3b45
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167335"
---
# <a name="caspolexe-code-access-security-policy-tool"></a>Caspol.exe (Ferramenta de Política de Segurança de Acesso de Código)
A ferramenta de política (Caspol.exe) CAS (Code Access Security) permite que usuários e administradores modifiquem a política de segurança para o nível de política do computador, o nível de política do usuário e o nível de política da empresa.  
  
> [!IMPORTANT]
> A partir do .NET Framework 4, Caspol.exe não afeta a política de CAS, a menos que o [ \<legacyCasPolicy> elemento](../configure-apps/file-schema/runtime/netfx40-legacysecuritypolicy-element.md) seja definido como `true` . Todas as configurações mostradas ou modificadas por CasPol.exe só afetarão aplicativos que optarem por usar a política de CAS. Para saber mais, confira [Alterações de segurança](https://docs.microsoft.com/previous-versions/dotnet/framework/security/security-changes).  
  
> [!NOTE]
> Os computadores 64 bits incluem versões 64 e 32 bits da política de segurança. Para verificar se as alterações na política se aplicam a aplicativos 32 e 64 bits, execute as versões 32 e 64 bits de Caspol.exe.  
  
 A ferramenta Política de Segurança de Acesso a Códigos é instalada automaticamente com o .NET Framework e o Visual Studio. É possível encontrar o Caspol.exe em %windir%\Microsoft.NET\Framework\\*version* em sistemas 32 bits ou em %windir%\Microsoft.NET\Framework64\\*version* em sistemas 64 bits. (Por exemplo, o local é% windir% \Microsoft.NET\Framework64\v4.030319\caspol.exe para o .NET Framework 4 em um sistema de 64 bits.) Várias versões da ferramenta poderão ser instaladas se o computador estiver executando várias versões do .NET Framework lado a lado. É possível executar a ferramenta no diretório de instalação. Entretanto, recomendamos que você use os [Prompts de comando](developer-command-prompt-for-vs.md), que não exigem que você navegue até a pasta de instalação.  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console
caspol [options]  
```  
  
## <a name="parameters"></a>parâmetros  
  
|Opção|Descrição|  
|------------|-----------------|  
|**-addfulltrust** *assembly_file*<br /><br /> ou<br /><br /> **-af** *assembly_file*|Adiciona um assembly que implementa um objeto de segurança personalizado (como uma permissão personalizada ou uma condição de associação personalizada) à lista de assemblies de confiança total para um nível de política específico. O argumento *assembly_file* especifica o assembly a ser adicionado. Esse arquivo deve ser assinado com um [nome forte](../../standard/assembly/strong-named.md). Você pode assinar um assembly com um nome forte usando a [Ferramenta Nome Forte (Sn.exe)](sn-exe-strong-name-tool.md).<br /><br /> Sempre que um conjunto de permissões contendo uma permissão personalizada for adicionado à política, o assembly que implementa a permissão personalizada deverá ser adicionado à lista de confiança total desse nível de política. Assemblies que implementam objetos de segurança personalizados (como grupos de códigos ou condições de associação) usados em uma política de segurança (como a política do computador) devem ser adicionados à lista de assemblies de confiança total. **Cuidado:** se o assembly que implementa o objeto de segurança personalizado fizer referência a outros assemblies, você deverá primeiro adicionar os assemblies referenciados à lista de assemblies de confiança total. Objetos de segurança personalizados criados usando-se Visual Basic, do C++ e JScript fazem referência a Microsoft.VisualBasic.dll, Microsoft.VisualC.dll ou Microsoft.JScript.dll, respectivamente. Esses assemblies não estão na lista de assemblies de confiança total por padrão. Você deverá adicionar a lista de assemblies apropriada à lista de confiança total antes de adicionar um objeto de segurança personalizado. Deixar de fazer isso interromperá o sistema de segurança, causando falha no carregamento dos assemblies. Nessa situação, a opção **-all -reset** de Caspol.exe não reparará a segurança. Para reparar segurança, você deve editar manualmente os arquivos de segurança a fim de remover o objeto de segurança personalizado.|  
|**-addgroup** {*parent_label &#124; parent_name*} *mship pset_name* [*flags*]<br /><br /> ou<br /><br /> **-ag** {*parent_label &#124; parent_name*} *mship pset_name* [*flags*]|Adiciona um novo grupo de códigos à hierarquia do grupo de códigos. É possível especificar o *parent_label* ou o *parent_name*. O argumento *parent_label* especifica o rótulo (como 1. ou 1.1) o nome do grupo de códigos que é o pai do grupo de códigos sendo adicionado. O argumento *parent_name* especifica o nome do grupo de códigos que é o pai do grupo de códigos sendo adicionado. Como *parent_label* e *parent_name* podem ser usados alternadamente, o Caspol.exe deve ser capaz de diferenciá-los. Por isso, *parent_name* não pode começar com um número. Além disso, *parent_name* só pode conter A-Z, 0-9 e o caractere de sublinhado.<br /><br /> O argumento *mship* especifica a condição de associação para o novo grupo de códigos. Para obter mais informações, consulte a tabela de argumentos *mship* posteriormente nesta seção.<br /><br /> O argumento *pset_name* é o nome do conjunto de permissões que será associado ao novo grupo de códigos. Também é possível definir um ou mais *flags* para o novo grupo. Para obter mais informações, consulte a tabela de argumentos *flags* posteriormente nesta seção.|  
|**-addpset** {*psfile* &#124; *psfile* p*set_name*}<br /><br /> ou<br /><br /> **-ap** {*named*_*psfile* &#124; *psfile* *pset_name*}|Adicione um novo conjunto de permissões nomeadas à política. O conjunto de permissões deve ser criado em XML e armazenado em um arquivo .xml. Se o arquivo XML contiver o nome do conjunto de permissões, apenas esse arquivo (*psfile*) será especificado. Se o arquivo XML não contiver o nome do conjunto de permissões, você deverá especificar o nome do arquivo XML (*psfile*) e o nome do conjunto de permissões (*pset_name*).<br /><br /> Observe que todas as permissões usadas em um conjunto de permissões devem ser definidas em assemblies contidos no cache de assembly global.|  
|**-a**[**ll**]|Indica que todas as opções depois dessa se aplicam a esse computador, ao usuário e a políticas da empresa. A opção **-all** sempre referencia a política do usuário conectado no momento. Veja a opção **-customall** para consultar a política de um usuário diferente do usuário atual.|  
|**-chggroup** {*label &#124;name*} {*mship* &#124; *pset_name* &#124;<br /><br /> *sinalizadores* de`}`<br /><br /> ou<br /><br /> **-cg** {*label &#124;name*} {*mship* &#124; *pset_name* &#124;<br /><br /> *sinalizadores* de`}`|Altera uma condição de associação do grupo de códigos, o conjunto de permissões ou as configurações dos sinalizadores **exclusive**, **levelfinal**, **name** ou **description**. É possível especificar o *label* ou o *name*. O argumento *label* especifica o rótulo (como 1. ou 1.1.) do grupo de códigos. O argumento *name* especifica o nome do grupo de códigos a ser alterado. Como *label* e *name* podem ser usados alternadamente, o Caspol.exe deve ser capaz de diferenciá-los. Por isso, *name* não pode começar com um número. Além disso, *name* só pode conter A-Z, 0-9 e o caractere de sublinhado.<br /><br /> O argumento *pset_name* especifica o nome do conjunto de permissões a ser associado ao grupo de códigos. Consulte as tabelas posteriormente nesta seção para obter informações sobre os argumentos *mship* e *flags*.|  
|**-chgpset**  *psfile pset_name*<br /><br /> ou<br /><br /> **-cp** *psfile pset_name*|Altera um conjunto de permissões nomeadas. O argumento *psfile* fornece a nova definição do conjunto de permissões. Ele é um arquivo do conjunto de permissões serializado no formato XML. O argumento *pset_name* especifica o nome do conjunto de permissões que você deseja alterar.|  
|**-customall**  *path*<br /><br /> ou<br /><br /> **-ca**  *path*|Indica que todas as opções depois dessa se aplicam a esse computador, empresa e políticas de usuário personalizadas especificadas. Você deve especificar o local do arquivo de configuração de segurança do usuário personalizado com o argumento *path*.|  
|**-cu**[**stomuser**] *path*|Permite a administração de uma política de usuário personalizada que não pertence ao usuário em nome do qual Caspol.exe está em execução no momento. Você deve especificar o local do arquivo de configuração de segurança do usuário personalizado com o argumento *path*.|  
|**-Enterprise**<br /><br /> ou<br /><br /> **-EN**|Indica que todas as opções depois dessa se aplicam a essa política de nível empresarial. Usuários que não são administradores de empresa não têm direitos suficientes para modificar a política da empresa, embora possam exibi-la. Em cenários não empresariais, essa política, por padrão, não interfere na política de computador e usuário.|  
|**-e**[**xecution**] {**on** &#124; **off**}|Ativa ou desativa o mecanismo que verifica a permissão para execução antes do código iniciar a execução. **Observação:**  Essa opção é removida no .NET Framework 4 e em versões posteriores. Para saber mais, confira [Alterações de segurança](https://docs.microsoft.com/previous-versions/dotnet/framework/security/security-changes).|  
|**-f**[**orçar**]|Suprime o teste de autodestruição da ferramenta e altera a política conforme especificado pelo usuário. Normalmente, Caspol.exe verifica se alguma alteração da política impediria que Caspol.exe fosse executado corretamente; assim, Caspol.exe não salva a alteração da política e imprime uma mensagem de erro. Para forçar Caspol.exe a alterar a política, mesmo que isso evite a execução de Caspol.exe, use a opção **–force**.|  
|**-h**[**elp**]|Exibe sintaxe de comando e opções de Caspol.exe.|  
|**-l**[**ist**]|Lista a hierarquia do grupo de códigos e os conjuntos de permissões para o computador especificado, o usuário, a empresa ou todos os níveis de política. Caspol.exe exibe o rótulo do grupo de códigos primeiro, seguido do nome, se não for nulo.|  
|**-listdescription**<br /><br /> ou<br /><br /> **-ld**|Lista todas as descrições do grupo de códigos para o nível de política especificado.|  
|**-listfulltrust**<br /><br /> ou<br /><br /> **-LF**|Lista o conteúdo da lista de assemblies de confiança total para o nível de política especificado.|  
|**-listgroups**<br /><br /> ou<br /><br /> **-LG**|Exibe os grupos de códigos do nível de política especificado ou todos os níveis de política. Caspol.exe exibe o rótulo do grupo de códigos primeiro, seguido do nome, se não for nulo.|  
|**-listpset** ou **-lp**|Exibe os conjuntos de permissões para o nível de política especificado ou todos os níveis de política.|  
|**-m**[**achine**]|Indica que todas as opções depois dessa se aplicam a essa política de nível do computador. Usuários que não são administradores não têm direitos suficientes para modificar a política do computador, embora possam exibi-la. Para administradores, **-machine** é o padrão.|  
|**-polchgprompt** {**on** &#124; **off**}<br /><br /> ou<br /><br /> **-pp** {**on** &#124; **off**}|Habilita ou desabilita o prompt exibido sempre que Caspol.exe for executado usando-se uma opção que causaria alterações na política.|  
|**-Quiet**<br /><br /> ou<br /><br /> **-q**|Desabilita temporariamente o prompt exibido normalmente para uma opção que faz alterações na política. A configuração do prompt de alteração global não muda. Só use a opção com base em um único comando para evitar desabilitar o prompt de todos os comandos de Caspol.exe.|  
|**-r**[**eCover**]|Recupera a política de um arquivo de backup. Sempre que for feita uma alteração na política, Caspol.exe armazenará a política anterior em um arquivo de backup.|  
|**-remfulltrust** *assembly_file*<br /><br /> ou<br /><br /> **-rf**  *assembly_file*|Remove um assembly da lista de confiança total de um nível de política. Essa operação deverá ser realizada se um conjunto de permissões que contém uma permissão personalizada não for mais usado pela política. Porém, você só deverá remover um assembly que implementa uma permissão personalizada da lista de confiança total se o assembly não implementar nenhuma outra permissão personalizada que ainda está sendo usada. Ao remover um assembly da lista, você também deve remover todos os outros assemblies dependentes.|  
|**-remgroup** {*label &#124;name*}<br /><br /> ou<br /><br /> **-rg** {l*abel &#124; name*}|Remove o grupo de códigos especificado por rótulo ou nome. Se o grupo de códigos especificado tiver grupos de códigos filho, Caspol.exe também removerá todos os grupos de códigos filho.|  
|**-rempset** *pset_name*<br /><br /> ou<br /><br /> **-rp** *pset_name*|Remove o conjunto de permissões especificado da política. O argumento *pset_name* indica o conjunto de permissões a ser removido. Caspol.exe só removerá o conjunto de permissões se não estiver associado a um grupo de códigos. Os conjuntos de permissões (internos) padrão não podem ser removidos.|  
|**-redefinir**<br /><br /> ou<br /><br /> **-RS**|Restaura o estado padrão da política e o mantém no disco. Isso será útil sempre que uma política alterada estiver aparentemente além do reparo e você quiser recomeçar usando os padrões da instalação. A redefinição também pode ser prática quando você deseja usar a política padrão como ponto de partida para modificações feitas em arquivos de configuração de segurança específicos. Para obter mais informações, consulte [Editando manualmente os arquivos de configuração da segurança](#cpgrfcodeaccesssecuritypolicyutilitycaspolexeanchor1).|  
|**-resetlockdown**<br /><br /> ou<br /><br /> **-rsld**|Restaura uma versão mais restritiva do estado padrão e a mantém no disco; cria um backup da política do computador anterior e o mantém em um arquivo chamado `security.config.bac`.  A política bloqueada é semelhante à política padrão, exceto pela política não conceder permissões ao código nas zonas `Local Intranet`, `Trusted Sites` e `Internet` e os grupos de códigos correspondentes não terem grupos de códigos filho.|  
|**-resolvegroup** *assembly_file*<br /><br /> ou<br /><br /> **-rsg**  *assembly_file*|Mostra os grupos de códigos a que um assembly específico (*assembly_file*) pertence. Por padrão, essa opção exibe o computador, o usuário e os níveis de política empresarial a que o assembly pertence. Para exibir apenas um nível de política, use essa opção com a opção **-machine**, **-user** ou **-enterprise**.|  
|**-resolveperm** *assembly_file*<br /><br /> ou<br /><br /> **-rsp** *assembly_file*|Exibe todas as permissões que o nível especificado (ou padrão) da política de segurança concederia ao assembly se o assembly tivesse permissão para executar. O argumento *assembly_file* especifica o assembly. Se você especificar a opção **-all**, Caspol.exe calculará as permissões do assembly com base no usuário, no computador e na política corporativa, do contrário, as regras de comportamento padrão se aplicam.|  
|**-s**[**ecurity**] {**on** &#124; **off**}|Ativa ou desativa a segurança de acesso ao código. A especificação da opção **-s off** não desabilita a segurança com base na função. **Observação:**  Essa opção é removida no .NET Framework 4 e em versões posteriores. Para saber mais, confira [Alterações de segurança](https://docs.microsoft.com/previous-versions/dotnet/framework/security/security-changes). **Cuidado:** quando a segurança de acesso do código é desabilitada, todas as demandas de acesso ao código são bem-sucedidas. A desabilitação da segurança de acesso ao código deixa o sistema vulnerável a ataques pelo código mal-intencionado, como vírus e worms. A desativação da segurança oferece alguns ganhos de desempenho extra, mas só deve ser feita quando outras medidas de segurança foram tomadas para ajudar a garantir que não haja violação da segurança do sistema em geral. Entre os exemplos de outras precauções de segurança estão a desconexão de redes públicas, protegendo fisicamente computadores e assim por diante.|  
|**-u**[**ser**]|Indica que todas as opções depois dessa se aplicam à política no nível do usuário em cujo nome Caspol.exe está em execução. Para usuários não administrativos, o padrão é **-user**.|  
|**-?**|Exibe sintaxe de comando e opções de Caspol.exe.|  
  
 O argumento *mship*, que especifica a condição de associação para um grupo de códigos, pode ser usado com as opções **-addgroup** e **-chggroup**. Cada argumento *mship* é implementado como uma classe do .NET Framework. Para especificar *mship,* use uma das opções a seguir.  
  
|Argumento|Descrição|  
|--------------|-----------------|  
|**-allcode**|Especifica qualquer código. Para obter mais informações sobre essa condição de associação, consulte <xref:System.Security.Policy.AllMembershipCondition?displayProperty=nameWithType>.|  
|**-appdir**|Especifica o diretório do aplicativo. Se você especificar **–appdir** como a condição de associação, a evidência de URL do código será comparada com a evidência do diretório de aplicativo do código. Se ambos os valores de evidência forem iguais, essa condição de associação será atendida. Para obter mais informações sobre essa condição de associação, consulte <xref:System.Security.Policy.ApplicationDirectoryMembershipCondition?displayProperty=nameWithType>.|  
|**-custom**  *xmlfile*|Adiciona uma condição de associação personalizada. O argumento *xmlfile* obrigatório especifica o arquivo .xml que contém a serialização XML da condição de associação personalizada.|  
|**-hash** *hashAlg* {**-hex** *hashValue* &#124; **-file** *assembly_file* }|Especifica o código que tem o hash de assembly indicado. Para usar um hash como uma condição de associação do grupo de códigos, você deve especificar o valor de hash ou o arquivo de assembly. Para obter mais informações sobre essa condição de associação, consulte <xref:System.Security.Policy.HashMembershipCondition?displayProperty=nameWithType>.|  
|**-pub** { **-cert** *cert_file_name* &#124;<br /><br /> **-file** *signed_file_name* &#124; **-hex**  *hex_string* }|Especifica o código que tem o editor de software indicado, conforme mostrado por um arquivo de certificado, uma assinatura em um arquivo ou pela representação hexadecimal de um certificado X509. Para obter mais informações sobre essa condição de associação, consulte <xref:System.Security.Policy.PublisherMembershipCondition?displayProperty=nameWithType>.|  
|**-site** *website*|Especifica o código que tem o site de origem indicado. Por exemplo:<br /><br /> `-site** www.proseware.com`<br /><br /> Para obter mais informações sobre essa condição de associação, consulte <xref:System.Security.Policy.SiteMembershipCondition?displayProperty=nameWithType>.|  
|**-strong -file** *file_name* {*name* &#124; **-noname**} {*version* &#124; **-noversion**}|Especifica o código que tem um nome forte específico, conforme designado pelo nome do arquivo, o nome do assembly como uma cadeia de caracteres e a versão do assembly no formato *major*.*minor*.*build*.*revision*. Por exemplo:<br /><br /> **-strong -file** myAssembly.exe myAssembly 1.2.3.4<br /><br /> Para obter mais informações sobre essa condição de associação, consulte <xref:System.Security.Policy.StrongNameMembershipCondition?displayProperty=nameWithType>.|  
|**-url** *URL*|Especifica o código originado da URL indicada. A URL deve incluir um protocolo, como `http://` ou `ftp://`. Além disso, um caractere curinga (\*) pode ser usado para especificar vários assemblies de uma URL específica. **Observação:** como uma URL pode ser identificada usando vários nomes, o uso de uma URL como uma condição de associação não é uma maneira segura de verificar a identidade do código. Sempre que possível, use uma condição de associação de nome forte, uma condição de associação de publicador ou a condição de associação de hash. <br /><br /> Para obter mais informações sobre essa condição de associação, consulte <xref:System.Security.Policy.UrlMembershipCondition?displayProperty=nameWithType>.|  
|**-zone** *zonename*|Especifica o código com a zona de origem indicada. O argumento *zonename* pode ter um dos seguintes valores: **MyComputer**, **Intranet**, **Trusted**, **Internet** ou **Untrusted**. Para obter mais informações sobre essa condição de associação, consulte <xref:System.Security.Policy.ZoneMembershipCondition> Classe.|  
  
 O argumento *flags*, que pode ser usado com as opções **–addgroup** e **–chggroup**, é especificado usando um dos valores a seguir.  
  
|Argumento|Descrição|  
|--------------|-----------------|  
|**-description** "*descrição*"|Se usado com a opção **–addgroup**, especifica a descrição de um grupo de códigos a ser adicionado. Se usado com a opção **–chggroup**, especifica a descrição de um grupo de códigos a ser editado. O argumento *description* deve ser colocado entre aspas duplas.|  
|**-exclusive** {**on**&#124;**off**}|Quando definido como **on**, indica que apenas o conjunto de permissões associado ao grupo de códigos que você está adicionando ou modificando é levado em consideração quando algum código se ajusta à condição de associação do grupo de códigos. Quando essa opção é definida como **off**, Caspol.exe leva em consideração os conjuntos de permissões de todos os grupos de códigos compatíveis no nível da política.|  
|**-levelfinal** {**on**&#124;**off**}|Quando definido como **on**, indica que nenhum nível de política abaixo do nível no qual o ocorre grupo de códigos adicionado ou modificado é considerado. Essa opção costuma ser usada no nível de política do computador. Por exemplo, se você definir esse sinalizador para um grupo de códigos no nível do computador e algum código corresponder a essa condição de associação do grupo de códigos, Caspol.exe não calculará ou não aplicará a política de nível do usuário desse código.|  
|**-name** "*nome*"|Se usado com a opção **–addgroup**, especifica o nome de script de um grupo de códigos a ser adicionado. Se usado com a opção **-chggroup**, especifica o nome de script de um grupo de códigos a ser editado. O argumento *name* deve ser colocado entre aspas duplas. O argumento *name* não pode começar com um número e só pode conter A-Z, 0-9 e o caractere de sublinhado. Os grupos de códigos podem ser referenciados por esse *name* em vez de seu rótulo numérico. O *name* também é muito útil para fins de script.|  
  
## <a name="remarks"></a>Comentários  
 A política de segurança é expressada usando-se três níveis de política: política de computador, política de usuário e política de empresa. O conjunto de permissões recebido por um assembly é determinado pela interseção dos conjuntos de permissões permitidos por esses três níveis de política. Cada nível de política é representado por uma estrutura hierárquica de grupos de códigos. Cada grupo de códigos tem uma condição de associação que determina qual código é um membro desse grupo. Um conjunto de permissões nomeado também está associado a cada grupo de códigos. Esse conjunto de permissões especifica as permissões permitidas pelo runtime que a condição de associação deve ter. Uma hierarquia do grupo de códigos, além de seus conjuntos de permissões nomeados associados, define e mantém cada nível de política de segurança. É possível usar as opções **–user**, **-customuser**, **–machine** e **-enterprise** para definir o nível de política de segurança.  
  
 Para obter mais informações sobre a política de segurança e como o runtime determina quais permissões devem ser concedidas ao código, consulte [Gerenciamento de Políticas de Segurança](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/c1k0eed6(v=vs.100)).  
  
## <a name="referencing-code-groups-and-permission-sets"></a>Fazendo Referência a Grupos de Códigos e Conjuntos de Permissões  
 Para facilitar referências a grupos de códigos em uma hierarquia, a opção **-list** exibe uma lista recuada de grupos de códigos com seus rótulos numéricos (1, 1.1, 1.1.1 e assim por diante). As outras operações de linha de comando com grupos de códigos como destino também usam os rótulos numéricos para fazer referência a grupos de códigos específicos.  
  
 Os conjuntos de permissões nomeados são referenciados por seus nomes. A opção **–list** exibe a lista de grupos de códigos seguida de uma lista de conjuntos de permissões nomeados disponíveis nessa política.  
  
## <a name="caspolexe-behavior"></a>Comportamento de Caspol.exe  
 Todas as opções exceto **-s**[**ecurity**] {**on** &#124; **off**} usam a versão do .NET Framework com que Caspol.exe foi instalado. Se você executar o Caspol.exe instalado com a versão *X* do runtime, as alterações só se aplicarão a essa versão. Outras instalações lado a lado do runtime, se houver, não serão afetadas. Se você executar Caspol.exe na linha de comando sem estar em um diretório para uma versão específica do runtime, a ferramenta será executada no primeiro diretório da versão do runtime no caminho (normalmente, a versão mais recente do runtime instalada).  
  
 A opção **-s**[**ecurity**] {**on** &#124; **off**} é uma operação para todo o computador. A desativação da segurança de acesso ao código encerra verificações de segurança para todo o código gerenciado e para todos os usuários no computador. Se versões lado a lado do .NET Framework forem instaladas, esse comando desativará a segurança para toda versão instalada no computador. Embora a opção **-list** mostre que a segurança está desativada, nada mais indica claramente para outros usuários que a segurança foi desativada.  
  
 Quando um usuário sem direitos administrativos executa Caspol.exe, todas as opções referenciam a política de nível do usuário, a menos que a opção **–machine** esteja especificada. Quando um administrador executa Caspol.exe, todas as opções referenciam a política do computador, a menos que a opção **–user** esteja especificada.  
  
 Caspol.exe deve receber o equivalente ao conjunto de permissões **Everything** para funcionar. A ferramenta tem um mecanismo protetor que impede que a política seja modificada de maneira que impediria Caspol.exe de receber as permissões que precisa executar. Se você tentar fazer essas alterações, Caspol.exe notificará que a alteração da política solicitada interromperá a ferramenta, e a alteração da política será descartada. É possível desativar esse mecanismo protetor para um determinado comando usando a opção **–force**.  
  
<a name="cpgrfcodeaccesssecuritypolicyutilitycaspolexeanchor1"></a>
## <a name="manually-editing-the-security-configuration-files"></a>Editando Manualmente os Arquivos de Configuração da Segurança  
 Três arquivos de configuração de segurança correspondem aos três níveis de política compatíveis com Caspol.exe: um para a política do computador, um para a política de um determinado usuário e um para a política da empresa. Esses arquivos só são criados em disco quando a política do computador, do usuário ou da empresa é alterada usando-se Caspol.exe. É possível usar a opção **–reset** em Caspol.exe para salvar a política de segurança padrão em disco, se necessário.  
  
 Na maioria dos casos, a edição manual dos arquivos de configuração de segurança não é recomendada. Mas talvez haja cenários nos quais a modificação desses arquivos se torne necessária como quando um administrador deseja editar as configurações de segurança de um usuário específico.  
  
## <a name="examples"></a>Exemplos  
 **-addfulltrust**  
  
 Suponhamos que um conjunto de permissões contendo uma permissão personalizada tenha sido adicionado à política do computador. Essa permissão personalizada é implementada em `MyPerm.exe` e faz referência a classes `MyPerm.exe` em `MyOther.exe`. Ambos os assemblies devem ser adicionados à lista de assemblies de confiança total. O comando a seguir adiciona o assembly `MyPerm.exe` à lista de confiança total da política do computador.  
  
```console  
caspol -machine -addfulltrust MyPerm.exe  
```  
  
 O comando a seguir adiciona o assembly `MyOther.exe` à lista de confiança total da política do computador.  
  
```console  
caspol -machine -addfulltrust MyOther.exe  
```  
  
 **-addgroup**  
  
 O comando a seguir adiciona um grupo de códigos filho à raiz da hierarquia do grupo de códigos da política do computador. O novo grupo de códigos é um membro da zona **Internet** e é associado ao conjunto de permissões **Execução**.  
  
```console  
caspol -machine -addgroup 1.  -zone Internet Execution  
```  
  
 O comando a seguir adiciona um grupo de códigos filho que concede ao compartilhamento \\\netserver\netshare permissões de intranet local.  
  
```console  
caspol -machine -addgroup 1. -url \\netserver\netshare\* LocalIntranet  
```  
  
 **-addpset**  
  
 O comando a seguir adiciona o conjunto de permissões `Mypset` à política do usuário.  
  
```console  
caspol -user -addpset Mypset.xml Mypset  
```  
  
 **-chggroup**  
  
 O comando a seguir altera o conjunto de permissões definido na política do usuário do grupo de códigos identificado como 1.2. para o conjunto de permissões **Execução**.  
  
```console  
caspol -user -chggroup 1.2. Execution  
```  
  
 O comando a seguir altera a condição de associação na política padrão do grupo de códigos identificado como 1.2.1. e altera a configuração do sinalizador **exclusive**. A condição de associação é definida para ser o código originado na zona **Internet** e o sinalizador **exclusive** é ativado.  
  
```console  
caspol -chggroup 1.2.1. -zone Internet -exclusive on  
```  
  
 **-chgpset**  
  
 O comando a seguir altera o conjunto de permissões com o nome `Mypset` para o conjunto de permissões contido em `newpset.xml`. Observe que a versão atual não dá suporte à alteração dos conjuntos de permissões que estão sendo usados pela hierarquia do grupo de códigos.  
  
```console  
caspol -chgpset Mypset newpset.xml  
```  
  
 **-Force**  
  
 O comando a seguir associa o grupo de códigos raiz da política do usuário (identificada como 1) ao conjunto de permissões nomeado como **Nothing**. Isso impede a execução de Caspol.exe.  
  
```console  
caspol -force -user -chggroup 1 Nothing  
```  
  
 **-recuperação**  
  
 O comando a seguir recupera a política do computador salva mais recentemente.  
  
```console  
caspol -machine -recover  
```  
  
 **-remgroup**  
  
 O comando a seguir remove o grupo de códigos identificado como 1.1. Se esse grupo de códigos tiver algum grupo de códigos filho, esses grupos também serão excluídos.  
  
```console  
caspol -remgroup 1.1.  
```  
  
 **-rempset**  
  
 O comando a seguir remove o conjunto de permissões **Execution** da política do usuário.  
  
```console  
caspol -user -rempset Execution  
```  
  
 O comando a seguir remove `Mypset` do nível de política do usuário.  
  
```console  
caspol -rempset MyPset  
```  
  
 **-resolvegroup**  
  
 O comando a seguir mostra todos os grupos de códigos da política do computador aos quais `myassembly` pertence.  
  
```console  
caspol -machine -resolvegroup myassembly  
```  
  
 O comando a seguir mostra todos os grupos de códigos das políticas do computador, da empresa e do usuário personalizado especificado a que `myassembly` pertence.  
  
```console  
caspol -customall "c:\config_test\security.config" -resolvegroup myassembly  
```  
  
 **-resolveperm**  
  
 O comando a seguir calcula as permissões de `testassembly` com base nos níveis de política do computador e do usuário.  
  
```console  
caspol -all -resolveperm testassembly  
```  
  
## <a name="see-also"></a>Confira também

- [Ferramentas](index.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
