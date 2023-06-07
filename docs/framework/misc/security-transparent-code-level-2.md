---
title: Código transparente de segurança, nível 2
description: Entenda o código transparente de nível 2. Consulte exemplos de uso e comportamentos, padrões de substituição, regras de herança e muito mais.
ms.date: 03/30/2017
helpviewer_keywords:
- transparency
- level 2 transparency
- security-transparent code
- security-critical code
ms.assetid: 4d05610a-0da6-4f08-acea-d54c9d6143c0
ms.openlocfilehash: 3b87a48ac3f9925fd868be9e58d5904014ca6c09
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309203"
---
# <a name="security-transparent-code-level-2"></a>Código transparente de segurança, nível 2

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

A transparência de nível 2 foi introduzida no .NET Framework 4. As três filosofias desse modelo são código Transparent, segurança – código crítico e com segurança crítica.

- O código Transparent, incluindo o código que está sendo executado como confiança total, pode chamar outros códigos transparentes ou somente com segurança crítica. Ele só pode executar ações permitidas pelo conjunto de permissões de confiança parcial do domínio (se houver). O código transparent não pode fazer o seguinte:

  - Execute uma <xref:System.Security.CodeAccessPermission.Assert%2A> elevação de privilégio.

  - Conter código não seguro ou não verificável.

  - Chame diretamente o código crítico.

  - Chame código nativo ou código com o <xref:System.Security.SuppressUnmanagedCodeSecurityAttribute> atributo.

  - Chamar um membro que é protegido por um <xref:System.Security.Permissions.SecurityAction.LinkDemand> .

  - Herdar de tipos críticos.

  Além disso, os métodos Transparent não podem substituir métodos virtuais críticos nem implementar métodos de interface críticos.

- O código crítico seguro é totalmente confiável, mas é possível chamá-lo por código Transparent. Ele expõe uma área de superfície limitada de código de confiança total; a correção e as verificações de segurança ocorrem em código crítico seguro.

- O código de segurança crítica pode chamar qualquer código e é totalmente confiável, mas não pode ser chamado pelo código Transparent.

## <a name="usage-examples-and-behaviors"></a>Exemplos de Uso e Comportamentos

Para especificar .NET Framework regras de 4 (transparência de nível 2), use a seguinte anotação para um assembly:

```csharp
[assembly: SecurityRules(SecurityRuleSet.Level2)]
```

Para bloquear as regras do .NET Framework 2,0 (transparência de nível 1), use a seguinte anotação:

```csharp
[assembly: SecurityRules(SecurityRuleSet.Level1)]
```

Se você não anotar um assembly, as regras .NET Framework 4 serão usadas por padrão. No entanto, a prática recomendada é usar o <xref:System.Security.SecurityRulesAttribute> atributo em vez de dependendo do padrão.

### <a name="assembly-wide-annotation"></a>Anotação em todo o assembly

As regras a seguir se aplicam ao uso de atributos no nível do assembly:

- Nenhum atributo: se você não especificar nenhum atributo, o tempo de execução interpretará todo o código como segurança crítica, exceto onde a segurança crítica viola uma regra de herança (por exemplo, ao substituir ou implementar um método de interface ou virtual transparente). Nesses casos, os métodos são de segurança crítica. Especificar nenhum atributo faz com que o Common Language Runtime determine as regras de transparência para você.

- `SecurityTransparent`: Todo o código é transparente; o assembly inteiro não fará nada privilegiado nem seguro.

- `SecurityCritical`: Todo o código introduzido por tipos neste assembly é crítico; todo o outro código é transparente. Esse cenário é semelhante a não especificar nenhum atributo; no entanto, o Common Language Runtime não determina automaticamente as regras de transparência. Por exemplo, se você substituir um método virtual ou abstract ou implementar um método de interface, por padrão, esse método será transparente. Você deve anotar explicitamente o método como `SecurityCritical` ou `SecuritySafeCritical` ; caso contrário, um <xref:System.TypeLoadException> será lançado em tempo de carregamento. Essa regra também se aplica quando a classe base e a classe derivada estão no mesmo assembly.

- `AllowPartiallyTrustedCallers`(somente nível 2): todos os padrões de código são transparentes. No entanto, tipos individuais e membros podem ter outros atributos.

A tabela a seguir compara o comportamento de nível de assembly para o nível 2 com o nível 1.

|Atributo de assembly|Nível 2|Nível 1|
|------------------------|-------------|-------------|
|Nenhum atributo em um assembly parcialmente confiável|Os tipos e membros são, por padrão, transparentes, mas podem ser críticos para segurança ou com segurança crítica.|Todos os tipos e membros são transparentes.|
|Nenhum atributo|Especificar nenhum atributo faz com que o Common Language Runtime determine as regras de transparência para você. Todos os tipos e membros são críticos à segurança, exceto onde a segurança crítica viola uma regra de herança.|Em um assembly totalmente confiável (no cache de assembly global ou identificado como confiança total no `AppDomain` ), todos os tipos são transparentes e todos os membros são críticos à segurança.|
|`SecurityTransparent`|Todos os tipos e membros são transparentes.|Todos os tipos e membros são transparentes.|
|`SecurityCritical(SecurityCriticalScope.Everything)`|Não aplicável.|Todos os tipos e membros são críticos para a segurança.|
|`SecurityCritical`|Todo o código introduzido por tipos neste assembly é crítico; todo o outro código é transparente. Se você substituir um método virtual ou abstract ou implementar um método de interface, deverá anotar explicitamente o método como `SecurityCritical` ou `SecuritySafeCritical` .|Todos os padrões de código são transparentes. No entanto, tipos individuais e membros podem ter outros atributos.|

### <a name="type-and-member-annotation"></a>Tipo e Anotação do Membro

Os atributos de segurança que são aplicados a um tipo também se aplicam aos membros que são introduzidos pelo tipo. No entanto, eles não se aplicam a substituições virtuais ou abstratas da classe base ou implementações de interface. As regras a seguir se aplicam ao uso de atributos no nível de tipo e de membro:

- `SecurityCritical`: O tipo ou membro é crítico e pode ser chamado somente pelo código de confiança total. Os métodos que são introduzidos em um tipo de segurança crítica são críticos.

    > [!IMPORTANT]
    > Os métodos virtuais e abstratos que são introduzidos em classes base ou interfaces, e substituídos ou implementados em uma classe de segurança crítica são transparentes por padrão. Eles devem ser identificados como `SecuritySafeCritical` ou `SecurityCritical` .

- `SecuritySafeCritical`: O tipo ou o membro é de segurança crítica. No entanto, o tipo ou membro pode ser chamado de código transparente (parcialmente confiável) e é compatível como qualquer outro código crítico. O código deve ser auditado quanto à segurança.

## <a name="override-patterns"></a>Substituir Padrões

A tabela a seguir mostra as substituições de método permitidas para transparência de nível 2.

|Membro de interface/virtual base|Substituição/interface|
|------------------------------------|-------------------------|
|`Transparent`|`Transparent`|
|`Transparent`|`SafeCritical`|
|`SafeCritical`|`Transparent`|
|`SafeCritical`|`SafeCritical`|
|`Critical`|`Critical`|

## <a name="inheritance-rules"></a>Regras de Herança

Nesta seção, a seguinte ordem é atribuída a `Transparent` , `Critical` e a `SafeCritical` código com base no acesso e nos recursos:

`Transparent` < `SafeCritical` < `Critical`

- Regras para tipos: indo da esquerda para a direita, o acesso se torna mais restritivo. Os tipos derivados devem ser pelo menos tão restritivos quanto o tipo base.

- Regras para métodos: métodos derivados não podem alterar a acessibilidade do método base. Para o comportamento padrão, todos os métodos derivados que não são anotados são `Transparent` . Derivações de tipos críticos causam uma exceção a ser gerada se o método substituído não for anotado explicitamente como `SecurityCritical` .

A tabela a seguir mostra os padrões de herança de tipo permitidos.

|Classe base|A classe derivada pode ser|
|----------------|--------------------------|
|`Transparent`|`Transparent`|
|`Transparent`|`SafeCritical`|
|`Transparent`|`Critical`|
|`SafeCritical`|`SafeCritical`|
|`SafeCritical`|`Critical`|
|`Critical`|`Critical`|

A tabela a seguir mostra os padrões de herança de tipo não permitido.

|Classe base|A classe derivada não pode ser|
|----------------|-----------------------------|
|`SafeCritical`|`Transparent`|
|`Critical`|`Transparent`|
|`Critical`|`SafeCritical`|

A tabela a seguir mostra os padrões de herança de método permitidos.

|Método base|O método derivado pode ser|
|-----------------|---------------------------|
|`Transparent`|`Transparent`|
|`Transparent`|`SafeCritical`|
|`SafeCritical`|`Transparent`|
|`SafeCritical`|`SafeCritical`|
|`Critical`|`Critical`|

A tabela a seguir mostra os padrões de herança de método não permitido.

|Método base|O método derivado não pode ser|
|-----------------|------------------------------|
|`Transparent`|`Critical`|
|`SafeCritical`|`Critical`|
|`Critical`|`Transparent`|
|`Critical`|`SafeCritical`|

> [!NOTE]
> Essas regras de herança se aplicam a tipos e membros de nível 2. Os tipos em assemblies de nível 1 podem herdar de membros e tipos de segurança de nível 2-críticos. Portanto, os tipos de nível 2 e os membros devem ter demandas de herança separadas para herdeiros de nível 1.

## <a name="additional-information-and-rules"></a>Informações Adicionais e Regras

### <a name="linkdemand-support"></a>Suporte LinkDemand

O modelo de transparência nível 2 substitui o <xref:System.Security.Permissions.SecurityAction.LinkDemand> pelo <xref:System.Security.SecurityCriticalAttribute> atributo. No código herdado (nível 1), um <xref:System.Security.Permissions.SecurityAction.LinkDemand> é automaticamente tratado como um <xref:System.Security.Permissions.SecurityAction.Demand> .

### <a name="reflection"></a>Reflexão

Invocar um método crítico ou ler um campo crítico dispara uma demanda de confiança total (assim como se você estivesse invocando um método ou um campo particular). Portanto, o código de confiança total pode invocar um método crítico, enquanto o código de confiança parcial não pode.

As propriedades a seguir foram adicionadas ao <xref:System.Reflection> namespace para determinar se o tipo, o método ou o campo é `SecurityCritical` , `SecuritySafeCritical` , ou `SecurityTransparent` : <xref:System.Type.IsSecurityCritical%2A> , <xref:System.Reflection.MethodBase.IsSecuritySafeCritical%2A> e <xref:System.Reflection.MethodBase.IsSecurityTransparent%2A> . Use essas propriedades para determinar a transparência usando reflexão em vez de verificar a presença do atributo. As regras de transparência são complexas e a verificação do atributo pode não ser suficiente.

> [!NOTE]
> Um `SafeCritical` método retorna `true` para <xref:System.Type.IsSecurityCritical%2A> and <xref:System.Reflection.MethodBase.IsSecuritySafeCritical%2A> , porque `SafeCritical` é realmente crítico (ele tem os mesmos recursos que o código crítico, mas pode ser chamado a partir do código Transparent).

Métodos dinâmicos herdam a transparência dos módulos aos quais eles estão anexados; Eles não herdam a transparência do tipo (se estiverem anexados a um tipo).

### <a name="skip-verification-in-full-trust"></a>Ignorar Verificação em Confiança Total

Você pode ignorar a verificação de assemblies transparentes totalmente confiáveis definindo a <xref:System.Security.SecurityRulesAttribute.SkipVerificationInFullTrust%2A> propriedade como `true` no <xref:System.Security.SecurityRulesAttribute> atributo:

`[assembly: SecurityRules(SecurityRuleSet.Level2, SkipVerificationInFullTrust = true)]`

A <xref:System.Security.SecurityRulesAttribute.SkipVerificationInFullTrust%2A> propriedade é `false` por padrão, portanto, a propriedade deve ser definida como `true` para ignorar a verificação. Isso deve ser feito apenas para fins de otimização. Você deve garantir que o código transparent no assembly seja verificável usando a `transparent` opção na [ferramenta PEVerify](../tools/peverify-exe-peverify-tool.md).

## <a name="see-also"></a>Confira também

- [Segurança-código Transparent, nível 1](security-transparent-code-level-1.md)
- [Alterações de segurança](https://docs.microsoft.com/previous-versions/dotnet/framework/security/security-changes)
