---
title: Compatibilidade de versão no .NET Framework
description: Saiba mais sobre a compatibilidade entre as versões do .NET Framework, incluindo compatibilidade com versões anteriores e execução lado a lado.
ms.date: 04/02/2019
helpviewer_keywords:
- .NET Framework, version compatibility
- .NET Framework, compatibility with earlier versions
- .NET Framework versions, compatibility
ms.assetid: 2f25e522-456a-48c3-8a53-e5f39275649f
ms.openlocfilehash: 92cfdc1a2a530f9790a693d0aa1ca5f65ff1af9f
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558758"
---
# <a name="version-compatibility"></a>Compatibilidade de versões

Compatibilidade com versões anteriores significa que um aplicativo desenvolvido para uma versão específica de uma plataforma será executado em versões posteriores dessa plataforma. .NET Framework tenta maximizar a compatibilidade com versões anteriores: o código-fonte escrito para uma versão do .NET Framework deve ser compilado em versões posteriores do .NET Framework, e os binários executados em uma versão do .NET Framework devem se comportar de forma idêntica em versões posteriores do .NET Framework.

## <a name="version-compatibility-for-apps"></a><a name="Apps"></a> Compatibilidade de versão para aplicativos

Por padrão, um aplicativo é executado na versão do .NET Framework para o qual foi criado. Se essa versão não estiver presente e o arquivo de configuração de aplicativos não definir versões compatíveis, um erro de inicialização do .NET Framework poderá ocorrer. Nesse caso, a tentativa de executar o aplicativo falhará.

Para definir as versões específicas nas quais seu aplicativo é executado, adicione um ou mais [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) elementos ao arquivo de configuração do aplicativo. Cada elemento `<supportedRuntime>` lista uma versão compatível do runtime, com o primeiro especificando a versão mais preferida e o último especificando a versão menos preferida.

```xml
<configuration>
   <startup>
      <supportedRuntime version="v2.0.50727" />
      <supportedRuntime version="v4.0" />
   </startup>
</configuration>
```

Para saber mais, confira [How to: Configure an App to Support .NET Framework 4 or 4.x](how-to-configure-an-app-to-support-net-framework-4-or-4-5.md) (Como configurar um aplicativo para dar suporte ao .NET Framework 4 ou 4.x).

## <a name="version-compatibility-for-components"></a>Compatibilidade de versão para componentes

Um aplicativo pode controlar a versão do .NET Framework no qual é executado, mas um componente não pode. Os componentes e as bibliotecas de classe são carregados no contexto de um aplicativo específico e é por isso que são executados automaticamente na versão do .NET Framework em que o aplicativo é executado.

Por conta dessa restrição, as garantias de compatibilidade são especialmente importante para componentes. Desde o .NET Framework 4, você pode especificar o grau em que um componente deve permanecer compatível em várias versões aplicando o atributo <xref:System.Runtime.Versioning.ComponentGuaranteesAttribute?displayProperty=nameWithType> ao componente. As ferramentas podem usar esse atributo para detectar potenciais violações da garantia de compatibilidade em futuras versões de um componente.

## <a name="backward-compatibility"></a>Compatibilidade com versões anteriores

O .NET Framework 4.5 e as versões posteriores são compatíveis com versões anteriores de aplicativos que foram compilados com versões anteriores do .NET Framework. Em outras palavras, aplicativos e componentes compilados com versões anteriores funcionarão sem modificação no .NET Framework 4.5 e versões posteriores. No entanto, por padrão, como os aplicativos são executados na versão do Common Language Runtime para a qual foram desenvolvidos, talvez você precise fornecer um arquivo de configuração para permitir que seu aplicativo seja executado no .NET Framework 4.5 ou versões posteriores. Para saber mais, confira a seção [Compatibilidade de versão para aplicativos](#Apps), anteriormente neste artigo.

Na prática, essa compatibilidade pode ser desfeita por alterações aparentemente inconsequentes feitas no .NET Framework e por alterações em técnicas de programação. Por exemplo, as melhorias de desempenho no .NET Framework 4.5 podem expor uma condição de corrida que não ocorria em versões anteriores. Da mesma forma, o uso de um caminho codificado para assemblies do .NET Framework, a execução de uma comparação de igualdade com uma versão específica do .NET Framework e a obtenção do valor de um campo particular usando-se reflexão não são práticas compatíveis com versões anteriores. Além disso, cada versão do .NET Framework inclui correções de bug e mudanças relacionadas à segurança que podem afetar a compatibilidade de alguns aplicativos e componentes.

Se o aplicativo ou o componente não funcionar conforme esperado no .NET Framework 4.5 (incluindo suas versões de ponto, o .NET Framework 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2, 4.7, 4.7.1, 4.7.2 ou 4.8), use as seguintes listas de verificação:

- Se seu aplicativo foi desenvolvido para ser executado em qualquer versão do .NET Framework a partir do .NET Framework 4,0, consulte [compatibilidade de aplicativos](application-compatibility.md) para gerar listas de alterações entre sua versão de .NET Framework de destino e a versão na qual seu aplicativo está sendo executado.

- Se você tiver um aplicativo .NET Framework 3.5, consulte também [Problemas de migração do .NET Framework 4](net-framework-4-migration-issues.md).

- Se você tiver um aplicativo .NET Framework 2.0, consulte também [Alterações no .NET Framework 3.5 SP1](https://docs.microsoft.com/previous-versions/dotnet/articles/dd310284(v=msdn.10)).

- Se você tiver um aplicativo .NET Framework 1.1, consulte também [Alterações no .NET Framework 2.0](https://docs.microsoft.com/previous-versions/aa570326(v=msdn.10)).

- Caso esteja recompilando o código-fonte existente para ser executado no .NET Framework 4.5 ou suas versões de ponto ou desenvolvendo uma nova versão de um aplicativo ou componente direcionado ao .NET Framework 4.5 ou suas versões de ponto de uma base de código-fonte existente, confira [O que está obsoleto na biblioteca de classes](../whats-new/whats-obsolete.md) para obter tipos e membros obsoletos e aplicar a solução alternativa descrita. (O código compilado anteriormente continuará sendo executado em tipos e membros marcados como obsoletos.)

- Se você determinar que uma alteração no .NET Framework 4,5 desrompeu o aplicativo, verifique o [esquema de configurações de tempo de execução](../configure-apps/file-schema/runtime/index.md)e, particularmente, o [ \<AppContextSwitchOverrides> elemento](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md), para determinar se você pode usar uma configuração de tempo de execução no arquivo de configuração do aplicativo para restaurar o comportamento anterior.

- Se você encontrar um problema que não está documentado, abra um problema no [site da Comunidade de Desenvolvedores do .NET](https://developercommunity.visualstudio.com/spaces/61/index.html) ou no [repositório GitHub do Microsoft/dotnet](https://github.com/microsoft/dotnet/issues).

## <a name="side-by-side-execution"></a>Execução lado a lado

Se você não encontrar uma solução alternativa adequada para seu problema, lembre-se de que o .NET Framework 4,5 (ou uma de suas versões de ponto) é executado lado a lado com as versões 1,1, 2,0 e 3,5, e é uma atualização in-loco que substitui a versão 4. Para aplicativos direcionados às versões 1,1, 2,0 e 3,5, você pode instalar a versão apropriada do .NET Framework no computador de destino para executar o aplicativo em seu melhor ambiente. Para saber mais sobre a execução lado a lado, confira [Side-by-Side Execution](../deployment/side-by-side-execution.md) (Execução lado a lado).

## <a name="see-also"></a>Confira também

- [Novidades](../whats-new/index.md)
- [O que está obsoleto na biblioteca de classes](../whats-new/whats-obsolete.md)
- [Compatibilidade de aplicativos](application-compatibility.md)
- [.NET Framework a política de suporte oficial](https://dotnet.microsoft.com/platform/support/policy/dotnet-framework)
- [.NET Framework 4 problemas de migração](net-framework-4-migration-issues.md)
