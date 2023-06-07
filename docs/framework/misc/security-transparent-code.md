---
title: Código transparente de segurança
description: Entenda a finalidade do modelo de código Transparent, como especificar o nível de transparência e a imposição de transparência em segurança.
ms.date: 03/30/2017
helpviewer_keywords:
- transparent code
- security-transparent code
ms.assetid: 4f3dd841-82f7-4659-aab0-6d2db2166c65
ms.openlocfilehash: a167efe12b88f796fba4abc6d60ebffe4693709a
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309840"
---
# <a name="security-transparent-code"></a>Código transparente de segurança

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

A segurança envolve três peças de interação: área restrita, permissões e imposição. A área restrita refere-se à prática de criar domínios isolados em que algum código é tratado como totalmente confiável e outro código é restrito às permissões no conjunto de concessão para a área restrita. O código do aplicativo que é executado dentro do conjunto de concessão da área restrita é considerado transparente; ou seja, ele não pode executar nenhuma operação que possa afetar a segurança. A concessão definida para a área restrita é determinada por evidência ( <xref:System.Security.Policy.Evidence> classe). A evidência identifica quais permissões específicas são exigidas pelas áreas restritas e quais tipos de áreas restritas podem ser criadas. Imposição refere-se a permitir que o código Transparent seja executado somente dentro de seu conjunto de concessão.

> [!IMPORTANT]
> A política de segurança era um elemento fundamental nas versões anteriores do .NET Framework. A partir do .NET Framework 4, a política de segurança é obsoleta. A eliminação da política de segurança é separada da transparência de segurança. Para obter informações sobre os efeitos dessa alteração, consulte [compatibilidade e migração da política de segurança de acesso ao código](code-access-security-policy-compatibility-and-migration.md).

## <a name="purpose-of-the-transparency-model"></a>Finalidade do Modelo de Transparência

Transparência é um mecanismo de imposição que separa o código que é executado como parte do aplicativo do código que é executado como parte da infraestrutura. A transparência desenha uma barreira entre o código que pode fazer coisas privilegiadas (código crítico), como chamar código nativo e código que não pode (código Transparent). O código Transparent pode executar comandos dentro dos limites do conjunto de permissões em que está operando, mas não pode executar, derivar de ou conter código crítico.

O objetivo principal da imposição de transparência é fornecer um mecanismo simples e eficaz para isolar diferentes grupos de código com base no privilégio. No contexto do modelo de área restrita, esses grupos de privilégios são totalmente confiáveis (ou seja, não restritos) ou parcialmente confiáveis (ou seja, restritos ao conjunto de permissões concedidos à área restrita).

> [!IMPORTANT]
> O modelo de transparência transcende a segurança de acesso ao código. A transparência é imposta pelo compilador just-in-time e permanece em vigor, independentemente do conjunto de concessão para um assembly, incluindo confiança total.

A transparência foi introduzida na versão 2,0 do .NET Framework para simplificar o modelo de segurança e para facilitar a gravação e a implantação de bibliotecas e aplicativos seguros. O código Transparent também é usado no Microsoft Silverlight, para simplificar o desenvolvimento de aplicativos parcialmente confiáveis.

> [!NOTE]
> Ao desenvolver um aplicativo parcialmente confiável, você deve estar ciente dos requisitos de permissão para seus hosts de destino. Você pode desenvolver um aplicativo que usa recursos que não são permitidos por alguns hosts. Este aplicativo será compilado sem erros, mas falhará quando for carregado no ambiente hospedado. Se você tiver desenvolvido seu aplicativo usando o Visual Studio, poderá habilitar a depuração em confiança parcial ou em um conjunto de permissões restritos do ambiente de desenvolvimento. Para obter mais informações, consulte [como: Depurar um aplicativo ClickOnce com permissões restritas](/visualstudio/deployment/how-to-debug-a-clickonce-application-with-restricted-permissions). O recurso Calculate Permissions fornecido para aplicativos ClickOnce também está disponível para qualquer aplicativo parcialmente confiável.

## <a name="specifying-the-transparency-level"></a>Especificando o Nível de Transparência

O atributo de nível de assembly <xref:System.Security.SecurityRulesAttribute> seleciona explicitamente as <xref:System.Security.SecurityRuleSet> regras que o assembly irá seguir. As regras são organizadas em um sistema de nível numérico, onde níveis mais altos significam uma imposição mais rígida de regras de segurança.

Os níveis são os seguintes:

- Nível 2 ( <xref:System.Security.SecurityRuleSet.Level2> ) – as regras de transparência .NET Framework 4.

- Nível 1 ( <xref:System.Security.SecurityRuleSet.Level1> ) – as regras de transparência .NET Framework 2,0.

A principal diferença entre os dois níveis de transparência é que o nível 1 não impõe regras de transparência para chamadas de fora do assembly e destina-se apenas à compatibilidade.

> [!IMPORTANT]
> Você deve especificar a transparência de nível 1 somente para fins de compatibilidade; ou seja, especifique o nível 1 somente para o código que foi desenvolvido com o .NET Framework 3,5 ou anterior que usa o <xref:System.Security.AllowPartiallyTrustedCallersAttribute> atributo ou não usa o modelo de transparência. Por exemplo, use a transparência de nível 1 para assemblies .NET Framework 2,0 que permitem chamadas de chamadores parcialmente confiáveis (APTCA). Para o código que é desenvolvido para o .NET Framework 4, sempre use a transparência nível 2.

### <a name="level-2-transparency"></a>Transparência de nível 2

A transparência de nível 2 foi introduzida no .NET Framework 4. As três filosofias desse modelo são código Transparent, segurança – código crítico e com segurança crítica.

- Código Transparent, independentemente das permissões que ele recebe (incluindo confiança total), pode chamar apenas outros códigos transparentes ou código de segurança crítica. Se o código for parcialmente confiável, ele só poderá executar ações permitidas pelo conjunto de permissões do domínio. O código transparent não pode fazer o seguinte:

  - Execute uma <xref:System.Security.CodeAccessPermission.Assert%2A> operação ou elevação de privilégio.

  - Conter código não seguro ou não verificável.

  - Chame diretamente o código crítico.

  - Chame código nativo ou código que tenha o <xref:System.Security.SuppressUnmanagedCodeSecurityAttribute> atributo.

  - Chamar um membro que é protegido por um <xref:System.Security.Permissions.SecurityAction.LinkDemand> .

  - Herdar de tipos críticos.

    Além disso, os métodos Transparent não podem substituir métodos virtuais críticos nem implementar métodos de interface críticos.

- Segurança-o código crítico é totalmente confiável, mas é possível chamá-lo por código Transparent. Ele expõe uma área de superfície limitada de código de confiança total. A correção e as verificações de segurança ocorrem em código crítico seguro.

- O código de segurança crítica pode chamar qualquer código e é totalmente confiável, mas não pode ser chamado pelo código Transparent.

### <a name="level-1-transparency"></a>Transparência de Nível 1

O modelo transparência de nível 1 foi introduzido na versão 2,0 do .NET Framework para permitir que os desenvolvedores reduzam a quantidade de código que está sujeita a uma auditoria de segurança. Embora a transparência de nível 1 tenha sido disponibilizada publicamente na versão 2,0, ela foi usada principalmente apenas na Microsoft para fins de auditoria de segurança. Por meio de anotações, os desenvolvedores são capazes de declarar quais tipos e membros podem executar elevações de segurança e outras ações confiáveis (segurança crítica) e quais não podem (segurança transparente). O código que é identificado como transparente não requer um alto grau de auditoria de segurança. A transparência de nível 1 indica que a imposição de transparência está limitada a dentro do assembly. Em outras palavras, quaisquer tipos públicos ou membros identificados como segurança crítica são de segurança crítica somente no assembly. Se você deseja impor a segurança para esses tipos e membros quando eles são chamados de fora do assembly, você deve usar as demandas de link para confiança total. Se você não fizer isso, os tipos e membros de segurança visíveis publicamente serão tratados como de segurança crítica e podem ser chamados por código parcialmente confiável fora do assembly.

O modelo transparência nível 1 tem as seguintes limitações:

- Os tipos de segurança crítica e os membros públicos são acessíveis a partir do código de segurança transparente.

- As anotações de transparência são impostas somente dentro de um assembly.

- Os tipos de segurança crítica e os membros devem usar demandas de link para impor a segurança para chamadas de fora do assembly.

- As regras de herança não são impostas.

- O potencial existe para que o código Transparent faça coisas prejudiciais quando executado em confiança total.

## <a name="transparency-enforcement"></a>Imposição de Transparência

As regras de transparência não são impostas até que a transparência seja calculada. Nesse momento, um <xref:System.InvalidOperationException> será gerado se uma regra de transparência for violada. A hora em que a transparência é calculada depende de vários fatores e não pode ser prevista. Ele é calculado o mais tarde possível. No .NET Framework 4, o cálculo de transparência no nível do assembly ocorre antes do que acontece no .NET Framework 2,0. A única garantia é que o cálculo de transparência ocorrerá no momento em que for necessário. Isso é semelhante a como o compilador JIT (just-in-time) pode alterar o ponto quando um método é compilado e quaisquer erros nesse método são detectados. O cálculo de transparência será invisível se o seu código não tiver erros de transparência.

## <a name="see-also"></a>Confira também

- [Segurança-código Transparent, nível 1](security-transparent-code-level-1.md)
- [Código transparente de segurança, nível 2](security-transparent-code-level-2.md)
