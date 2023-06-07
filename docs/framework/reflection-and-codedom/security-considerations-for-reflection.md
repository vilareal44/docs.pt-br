---
title: Considerações sobre segurança relacionadas à reflexão
description: Saiba mais sobre as considerações de segurança para reflexão no .NET. Obter informações sobre tipos e membros é permitido, mas acessar membros tem restrições.
ms.date: 03/30/2017
helpviewer_keywords:
- permissions [.NET Framework], reflection
- MethodInfo parameters
- reflection, security
- partial trust,reflection
- nonpublic members
- reflection,partial trust
- link demands
ms.assetid: 42d9dc2a-8fcc-4ff3-b002-4ff260ef3dc5
ms.openlocfilehash: 0465cbd5ceb7d4f44bb6d10865fcbd17b8ed7af6
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865249"
---
# <a name="security-considerations-for-reflection"></a>Considerações sobre segurança relacionadas à reflexão

A reflexão fornece a capacidade de obter informações sobre tipos e membros e de acessar membros (ou seja, chamar métodos e construtores, obter e definir valores de propriedade, adicionar e remover manipuladores de eventos e assim por diante). O uso da reflexão para obter informações sobre tipos e membros não é restrito. Todo o código pode usar reflexões para realizar as seguintes tarefas:

- Enumere os tipos e membros, e examine seus metadados.

- Enumerar e examine os módulos e assemblies.

Usar reflexão para acessar membros está, por outro lado, sujeitos a restrições. A partir do .NET Framework 4, somente código confiável pode usar reflexão para acessar membros críticos para segurança. Além disso, somente o confiável pode usar reflexão para acessar membros não públicos que não estariam diretamente acessíveis em código compilado. Por fim, o código que usa reflexão para acessar um membro crítico para segurança deve ter as devidas permissões exigidas pelo membro crítico para segurança, assim como com código compilado.

O código pode usar reflexão para realizar os seguintes tipos de acesso, sujeito às permissões necessárias:

- Acesse os membros públicos que não são críticos para segurança.

- Acessar membros não públicos que estariam acessíveis ao código compilado, se tais membros não fossem críticos para segurança. Exemplos de tais membros não públicos:

  - Membros protegidos de classes base do código de chamada. (Na reflexão, isso é chamado de acesso de nível familiar.)

  - Membros `internal` (membros `Friend` no Visual Basic) no assembly do código de chamada. (Na reflexão, isso é chamado de acesso no nível do assembly.)

  - Membros privados de outras instâncias da classe que contém o código de chamada.

Por exemplo, o código que é executado em um domínio do aplicativo em área restrita é limitado ao acesso descrito nesta lista, a menos que o domínio do aplicativo conceda permissões adicionais.

A partir do .NET Framework 2.0 Service Pack 1, tentar acessar membros que normalmente não podem ser acessados gera uma demanda para o conjunto de concessões do objeto de destino mais <xref:System.Security.Permissions.ReflectionPermission> com o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType>. Código que está sendo executado com confiança total (por exemplo, o código em um aplicativo que é iniciado da linha de comando) sempre pode atender a essas permissões. (Isso é sujeito às limitações de acesso a membros de críticos para segurança, conforme descrito mais adiante neste artigo.)

Como opção, um domínio do aplicativo em área restrita pode conceder <xref:System.Security.Permissions.ReflectionPermission> com o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType>, conforme descrito na seção [Acessando membros que são normalmente inacessíveis](#accessingNormallyInaccessible), mais adiante neste artigo.

<a name="accessingSecurityCritical"></a>

## <a name="accessing-security-critical-members"></a>Acessando membros críticos para segurança

Um membro é crítico para segurança se ele tiver o <xref:System.Security.SecurityCriticalAttribute>, se ele pertencer a um tipo que tem o <xref:System.Security.SecurityCriticalAttribute> ou se ele está em um assembly crítico para segurança. A partir do .NET Framework 4, as regras para acessar membros críticos para segurança são as seguintes:

- O código transparente não pode usar reflexão para acessar membros críticos para segurança, mesmo que o código seja totalmente confiável. Um <xref:System.MethodAccessException>, <xref:System.FieldAccessException> ou <xref:System.TypeAccessException> é gerado.

- Código em execução com confiança parcial é tratado como transparente.

Essas regras são as mesmas se um membro crítico para segurança for acessado diretamente pelo código compilado ou por meio de reflexão.

Código de aplicativo que é executado da linha de comando é executado com confiança total. Desde que não esteja marcado como transparente, ele pode usar reflexão para acessar membros críticos para segurança. Quando o mesmo código é executado com confiança parcial (por exemplo, em um domínio do aplicativo em área restrita), o nível de confiança do assembly determina se ele pode acessar o código crítico para segurança: se o assembly tiver um nome forte e estiver instalado no cache de assembly global, ele é um assembly confiável e pode chamar membros crítico de segurança. Se não for confiável, ele se tornará transparente mesmo se não for marcado como transparente e não poderá acessar membros críticos para segurança.

Para obter mais informações sobre o modelo de segurança no .NET Framework 4, consulte [Alterações de Segurança](https://docs.microsoft.com/previous-versions/dotnet/framework/security/security-changes).

## <a name="reflection-and-transparency"></a>Reflexão e transparência

Do .NET Framework 4 em diante, o Common Language Runtime determina o nível de transparência de um tipo ou membro de vários fatores, incluindo o nível de confiança do assembly e o nível de confiança do domínio do aplicativo. A reflexão fornece as propriedades <xref:System.Type.IsSecurityCritical%2A>, <xref:System.Type.IsSecuritySafeCritical%2A> e <xref:System.Type.IsSecurityTransparent%2A> para que você possa descobrir o nível de transparência de um tipo. A tabela a seguir mostra as combinações válidas dessas propriedades.

|Nível de segurança|IsSecurityCritical|IsSecuritySafeCritical|IsSecurityTransparent|
|--------------------|------------------------|----------------------------|---------------------------|
|Crítico|`true`|`false`|`false`|
|Crítico para segurança|`true`|`true`|`false`|
|Transparente|`false`|`false`|`true`|

Usar essas propriedade é muito mais simples que examinar as anotações de segurança de um assembly e seus tipos, verificando o nível de confiança atual e tentando duplicar as regras do runtime. Por exemplo, o mesmo tipo pode ser crítico para segurança quando é executado da linha de comando ou transparente de segurança quando é executado em um domínio do aplicativo em área restrita.

Há propriedades semelhantes nas classes <xref:System.Reflection.MethodBase>, <xref:System.Reflection.FieldInfo>, <xref:System.Reflection.Emit.TypeBuilder>, <xref:System.Reflection.Emit.MethodBuilder> e <xref:System.Reflection.Emit.DynamicMethod>. (Para outras reflexão e abstrações de emissão de reflexão, os atributos de segurança são aplicados aos métodos associados; por exemplo, no caso de propriedades que são aplicadas aos acessadores de propriedade).

<a name="accessingNormallyInaccessible"></a>

## <a name="accessing-members-that-are-normally-inaccessible"></a>Acessando membros que são normalmente inacessíveis

Para usar a reflexão para invocar os membros que podem ser acessados de acordo com as regras de acessibilidade do Common Language Runtime, o código deve receber uma das duas permissões:

- Para permitir que o código invoque qualquer membro não público: seu código deve receber a concessão de <xref:System.Security.Permissions.ReflectionPermission> com o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType>.

  > [!NOTE]
  > Por padrão, a política de segurança nega essa permissão para código originado da Internet. Essa permissão nunca deve ser concedida o código originado da Internet.

- Para permitir que o código invoque qualquer membro não público, desde que o conjunto de concessões do assembly que contém o membro chamado seja o mesmo ou um subconjunto do conjunto de concessões do assembly que contém a invocação de código: seu código deve receber a concessão de <xref:System.Security.Permissions.ReflectionPermission> com o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType>.

Por exemplo, suponha que você concedeu permissões da Internet a um domínio do aplicativo mais <xref:System.Security.Permissions.ReflectionPermission> com o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> e, em seguida, executou um aplicativo da Internet com dois assemblies, A e B.

- O assembly A pode usar reflexão para acessar membros particulares do assembly B, pois o conjunto de concessões do assembly B não inclui permissões que não foram concedidas a A.

- O assembly A não pode usar a reflexão para acessar membros particulares de assemblies do .NET Framework como mscorlib.dll, pois mscorlib.dll é totalmente confiável e, portanto, tem permissões que não foram concedidas ao assembly A. Um <xref:System.MemberAccessException> é gerado quando a segurança de acesso do código percorre a pilha no tempo de execução.

## <a name="serialization"></a>Serialização

Para serialização, <xref:System.Security.Permissions.SecurityPermission> com o sinalizador <xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter%2A?displayProperty=nameWithType> fornece a capacidade de obter e definir os membros de tipos serializáveis, independentemente da acessibilidade. Essa permissão permite que o código descubra e altere o estado particular de uma instância. (Além de receber a concessão das permissões apropriadas, o tipo deve ser [marcado](../../standard/attributes/applying-attributes.md) como serializável nos metadados.)

## <a name="parameters-of-type-methodinfo"></a>Parâmetros do tipo MethodInfo

Evite escrever membros públicos que utilizam parâmetros <xref:System.Reflection.MethodInfo>, especialmente para código confiável. Esses membros podem ser mais vulneráveis a códigos mal-intencionados. Por exemplo, considere um membro público no código altamente confiável que utiliza um parâmetro <xref:System.Reflection.MethodInfo>. Suponha que o membro público chama indiretamente o método <xref:System.Reflection.MethodBase.Invoke%2A> no parâmetro fornecido. Se o membro público não executa as verificações de permissão necessária, a chamada para o método <xref:System.Reflection.MethodBase.Invoke%2A> sempre terá êxito, pois o sistema de segurança determina se o chamador é altamente confiável. Mesmo que o código mal-intencionado não tenha permissão para invocar o método indiretamente, ele ainda poderá fazer isso indiretamente chamando o membro público.

## <a name="version-information"></a>Informações sobre versão

- A partir do .NET Framework 4, código transparente não pode usar reflexão para acessar membros críticos para segurança.

- O sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> foi introduzido no .NET Framework 2.0 Service Pack 1. Versões anteriores do .NET Framework exigem o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> para o código que usa a reflexão para acessar membros não públicos. Esta é uma permissão que nunca deve ser concedida a código parcialmente confiável.

- A partir do .NET Framework 2.0, usar reflexão para obter informações sobre tipos e membros não públicos não requer permissões. Em versões anteriores, o <xref:System.Security.Permissions.ReflectionPermission> com o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.TypeInformation?displayProperty=nameWithType> é obrigatório.

## <a name="see-also"></a>Veja também

- <xref:System.Security.Permissions.ReflectionPermissionFlag>
- <xref:System.Security.Permissions.ReflectionPermission>
- <xref:System.Security.Permissions.SecurityPermission>
- [Alterações de segurança](https://docs.microsoft.com/previous-versions/dotnet/framework/security/security-changes)
- [Segurança de acesso do código](../misc/code-access-security.md)
- [Problemas de segurança na emissão de reflexão](security-issues-in-reflection-emit.md)
- [Exibindo informações de tipo](viewing-type-information.md)
- [Aplicando atributos](../../standard/attributes/applying-attributes.md)
- [Acessando atributos personalizados](accessing-custom-attributes.md)
