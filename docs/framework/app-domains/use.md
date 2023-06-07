---
title: Usando domínios do aplicativo
description: Use domínios de aplicativo, que fornecem uma unidade de isolamento para o Common Language Runtime (CLR). Os domínios de aplicativo são criados e executados dentro de um processo.
ms.date: 03/30/2017
helpviewer_keywords:
- application domains, about
- common language runtime, application domains
- runtime, application domains
ms.assetid: c6d99815-e022-4d2c-9420-1d7ab5b9d504
ms.openlocfilehash: df2a63716904ebfc6ee163121a1f07e53aa07514
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105181"
---
# <a name="using-application-domains"></a>Usando domínios do aplicativo

Os domínios do aplicativos fornecem uma unidade de isolamento para o Common Language Runtime. Eles são criados e executados dentro de um processo. Domínios do aplicativo geralmente são criados por um host de runtime, que é um aplicativo responsável por carregar o runtime para um processo e executar o código do usuário em um domínio do aplicativo. O host de runtime cria um processo e um domínio de aplicativo padrão e executa o código gerenciado dentro dele. Hosts de runtime incluem ASP.NET, Microsoft Internet Explorer e o shell do Windows.  
  
Para a maioria dos aplicativos, você não precisará criar seu próprio domínio do aplicativo; o host de runtime cria os domínios do aplicativo necessários para você. No entanto, você pode criar e configurar domínios do aplicativo adicionais se seu aplicativo precisar isolar o código ou usar e descarregar DLLs.  
  
## <a name="in-this-section"></a>Nesta seção  

[Como: Criar um domínio do aplicativo](how-to-create-an-application-domain.md)  
Descreve como criar um domínio do aplicativo com programação.  
  
[Como: Descarregar um domínio do aplicativo](how-to-unload-an-application-domain.md)  
Descreve como descarregar um domínio do aplicativo com programação.  
  
[Como: Configurar um domínio do aplicativo](how-to-configure-an-application-domain.md)  
Fornece uma introdução à configuração de um domínio do aplicativo.  
  
[Recuperando informações de instalação de um domínio de aplicativo](retrieve-setup-information.md)  
Descreve como recuperar informações de configuração de um domínio do aplicativo.  
  
[Como: Carregar assemblies em um domínio do aplicativo](how-to-load-assemblies-into-an-application-domain.md)  
Descreve como carregar um assembly em um domínio do aplicativo.  
  
[Como obter as informações de tipo e membro de um assembly](../reflection-and-codedom/get-type-member-information.md)  
Descreve como recuperar informações sobre um assembly.  
  
[Criando cópias de sombra de assemblies](shadow-copy-assemblies.md)  
Descreve como a cópia de sombra permite realizar atualizações nos assemblies enquanto eles estão em uso e como configurar cópias de sombra.  
  
[Como: Receber notificações de exceção de primeira tentativa](how-to-receive-first-chance-exception-notifications.md)  
Explica como você pode receber uma notificação de que uma exceção foi gerada, antes do Common Language Runtime começar a procurar por manipuladores de exceção.  
  
[Como resolver carregamentos de assembly](../../standard/assembly/resolve-loads.md)  
Fornece diretrizes sobre como usar o evento <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> para resolver falhas de carregamento de assembly.  
  
## <a name="reference"></a>Referência  

<xref:System.AppDomain>  
Representa um domínio do aplicativo. Fornece métodos para criar e controlar domínios de aplicativo.  
  
## <a name="related-sections"></a>Seções relacionadas  
[Assemblies no .NET](../../standard/assembly/index.md)  
Fornece uma visão geral das funções executadas por assemblies.  
  
[Programação com assemblies](../../standard/assembly/index.md)  
Descreve como criar, assinar e definir atributos em assemblies.  
  
[Emissão de métodos e assemblies dinâmicos](../reflection-and-codedom/emitting-dynamic-methods-and-assemblies.md)  
Descreve como criar assemblies dinâmicos.  
  
[Domínios do aplicativo](application-domains.md)  
Fornece uma visão geral conceitual de domínios de aplicativos.  
  
[Visão geral da reflexão](../reflection-and-codedom/reflection.md)  
Descreve como usar a classe **Reflection** para obter informações sobre um assembly.
