---
title: 'Erros de inicialização de .NET Framework: Gerenciando a experiência do usuário'
description: Controle a experiência do usuário quando ocorre um erro de inicialização do .NET, como quando o sistema de ativação não consegue encontrar a versão correta do CLR para carregar.
ms.date: 03/30/2017
helpviewer_keywords:
- no framework found experience
- initialization errors [.NET Framework]
- .NET Framework, initialization errors
ms.assetid: 680a7382-957f-4f6e-b178-4e866004a07e
ms.openlocfilehash: 6db68b43381dfe275c93cae5610386e10a6f09ae
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619683"
---
# <a name="net-framework-initialization-errors-managing-the-user-experience"></a>Erros de inicialização de .NET Framework: Gerenciando a experiência do usuário

O sistema de ativação do CLR (Common Language Runtime) determina a versão do CLR que será usada para executar o código do aplicativo gerenciado. Em alguns casos, o sistema de ativação pode não ser capaz de encontrar uma versão do CLR para carregar. Essa situação normalmente ocorre quando um aplicativo requer uma versão do CLR inválido ou que não está instalada em um determinado computador. Se a versão solicitada não for encontrada, o sistema de ativação do CLR retornará um código de erro HRESULT da função ou interface que foi chamada e poderá exibir uma mensagem de erro para o usuário que está executando o aplicativo. Este artigo fornece uma lista de códigos HRESULT e explica como você pode impedir que a mensagem de erro seja exibida.

O CLR fornece infraestrutura de log para ajudar a depurar problemas de ativação do CLR, conforme descrito em [Como depurar problemas de ativação do CLR](how-to-debug-clr-activation-issues.md). Essa infraestrutura não deve ser confundida com os [logs de associação do assembly](../tools/fuslogvw-exe-assembly-binding-log-viewer.md), que são totalmente diferentes.

## <a name="clr-activation-hresult-codes"></a>Códigos HRESULT de ativação do CLR

As APIs de ativação do CLR retornam códigos HRESULT para relatar o resultado da operação de ativação para um host. Os hosts CLR devem sempre consultar esses valores retornados antes de prosseguir com operações adicionais.

- CLR_E_SHIM_RUNTIMELOAD

- CLR_E_SHIM_RUNTIMEEXPORT

- CLR_E_SHIM_INSTALLROOT

- CLR_E_SHIM_INSTALLCOMP

- CLR_E_SHIM_LEGACYRUNTIMEALREADYBOUND

- CLR_E_SHIM_SHUTDOWNINPROGRESS

## <a name="ui-for-initialization-errors"></a>Interface do usuário para erros de inicialização

Se o sistema de ativação do CLR não puder carregar a versão correta de um runtime exigido por um aplicativo, ele exibirá uma mensagem de erro para os usuários para informá-los de que seu computador não está configurado adequadamente para executar o aplicativo e fornece a eles a oportunidade de corrigir a situação. Normalmente, a seguinte mensagem de erro é apresentada nesta situação. O usuário pode escolher **Sim** para ir para um site da Microsoft em que pode baixar a versão correta do .NET Framework para o aplicativo.

![Caixa de diálogo Erro de Inicialização do .NET Framework](./media/initialization-errors-managing-the-user-experience/initialization-error-dialog.png "Mensagem de erro típica para erros de inicialização")

## <a name="resolving-the-initialization-error"></a>Resolvendo o erro de inicialização

Como desenvolvedor, você tem uma variedade de opções para controlar a mensagem de erro de inicialização do .NET Framework. Por exemplo, você pode usar um sinalizador de API para impedir que a mensagem seja exibida, conforme discutido na próxima seção. No entanto, você ainda precisa resolver o problema que impediu que o aplicativo carregasse o runtime solicitado. Caso contrário, seu aplicativo pode não ser executado de forma alguma ou alguma funcionalidade pode não estar disponível.

Para resolver os problemas subjacentes e fornecer a melhor experiência do usuário (menos mensagens de erro), recomendamos o seguinte:

- Para aplicativos .NET Framework 3,5 (e versões anteriores): configure seu aplicativo para dar suporte a .NET Framework 4 ou versões posteriores (consulte [as instruções](../migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)).

- Para aplicativos do .NET Framework 4: instale o pacote redistribuível do .NET Framework 4 como parte da instalação de seu aplicativo. Consulte [Guia de implantação para desenvolvedores](deployment-guide-for-developers.md).

## <a name="controlling-the-error-message"></a>Controlando a mensagem de erro

Exibir uma mensagem de erro para comunicar que uma versão do .NET Framework solicitada não foi encontrada pode ser visto como um serviço útil ou uma pequena perturbação para os usuários. Em ambos os casos, você pode controlar essa interface do usuário passando sinalizadores para as APIs de ativação.

O método [ICLRMetaHostPolicy::GetRequestedRuntime](../unmanaged-api/hosting/iclrmetahostpolicy-getrequestedruntime-method.md) aceita um membro de enumeração [METAHOST_POLICY_FLAGS](../unmanaged-api/hosting/metahost-policy-flags-enumeration.md) como entrada. Você pode incluir o sinalizador METAHOST_POLICY_SHOW_ERROR_DIALOG para solicitar uma mensagem de erro se a versão do CLR solicitada não for encontrada. Por padrão, a mensagem de erro não é exibida. (O método [ICLRMetaHost::GetRuntime](../unmanaged-api/hosting/iclrmetahost-getruntime-method.md) não aceita esse sinalizador e não fornece nenhuma outra maneira de exibir a mensagem de erro.)

O Windows fornece uma função [SetErrorMode](/windows/win32/api/errhandlingapi/nf-errhandlingapi-seterrormode) que pode ser usada para declarar se você deseja que as mensagens de erro sejam mostradas como resultado do código executado em seu processo. Você pode especificar o sinalizador SEM_FAILCRITICALERRORS para impedir que a mensagem de erro seja exibida.

No entanto, em alguns cenários, é importante substituir a configuração de SEM_FAILCRITICALERRORS definida por um processo do aplicativo. Por exemplo, se você tiver um componente COM nativo que hospeda o CLR e que é hospedado em um processo em que SEM_FAILCRITICALERRORS está definido, talvez queira substituir o sinalizador, dependendo do impacto de exibir mensagens de erro naquele processo de aplicativo específico. Nesse caso, você pode usar um dos sinalizadores a seguir para substituir SEM_FAILCRITICALERRORS:

- Use METAHOST_POLICY_IGNORE_ERROR_MODE com o método [ICLRMetaHostPolicy::GetRequestedRuntime](../unmanaged-api/hosting/iclrmetahostpolicy-getrequestedruntime-method.md).

- Use RUNTIME_INFO_IGNORE_ERROR_MODE com a função [GetRequestedRuntimeInfo](../unmanaged-api/hosting/getrequestedruntimeinfo-function.md).

## <a name="ui-policy-for-clr-provided-hosts"></a>Política de interface do usuário para hosts fornecidos pelo CLR

O CLR inclui um conjunto de hosts para uma variedade de cenários e esses hosts exibem uma mensagem de erro ao encontrar problemas ao carregar a versão necessária do runtime. A tabela a seguir fornece uma lista de hosts e suas políticas de mensagem de erro.

|Host do CLR|Descrição|Política de mensagem de erro|A mensagem de erro pode ser desabilitada?|
|--------------|-----------------|--------------------------|------------------------------------|
|Host EXE gerenciado|Inicia EXEs gerenciados.|É mostrada no caso de uma versão do .NET Framework ausente|Não|
|Host COM gerenciado|Carrega componentes COM gerenciados em um processo.|É mostrada no caso de uma versão do .NET Framework ausente|Sim, definindo o sinalizador SEM_FAILCRITICALERRORS|
|Host ClickOnce|Inicia os aplicativos ClickOnce.|É mostrada no caso de uma versão do .NET Framework ausente, do .NET Framework 4.5 em diante|Não|
|Host XBAP|Inicia os aplicativos XBAP WPF.|É mostrada no caso de uma versão do .NET Framework ausente, do .NET Framework 4.5 em diante|Não|

## <a name="windows-8-behavior-and-ui"></a>Interface do usuário e comportamento do Windows 8

O sistema de ativação do CLR fornece o mesmo comportamento e interface do usuário no Windows 8, como acontece em outras versões do sistema operacional Windows, exceto quando encontra problemas ao carregar o CLR 2,0. O Windows 8 inclui o .NET Framework 4,5, que usa o CLR 4,5. No entanto, o Windows 8 não inclui o .NET Framework 2,0, 3,0 ou 3,5, que usam o CLR 2,0. Como resultado, os aplicativos que dependem do CLR 2,0 não são executados no Windows 8 por padrão. Em vez disso, eles exibem a seguinte caixa de diálogo para permitir que os usuários instalem o .NET Framework 3.5. Os usuários também podem habilitar o .NET Framework 3.5 no Painel de Controle. Ambas as opções são discutidas no artigo [Install the .NET Framework 3.5 on Windows 10, Windows 8.1, and Windows 8](../install/dotnet-35-windows-10.md) (Instalar o .NET Framework 3.5 no Windows 10, no Windows 8.1 e no Windows 8).

![Caixa de diálogo para a instalação do 3.5 no Windows 8](./media/initialization-errors-managing-the-user-experience/install-framework-on-demand-dialog.png "Solicitar a instalação do .NET Framework 3.5 sob demanda")

> [!NOTE]
> O .NET Framework 4.5 substitui o .NET Framework 4 (CLR 4) no computador do usuário. Portanto, .NET Framework 4 aplicativos são executados diretamente, sem exibir essa caixa de diálogo no Windows 8.

Quando o .NET Framework 3,5 é instalado, os usuários podem executar aplicativos que dependem do .NET Framework 2,0, 3,0 ou 3,5 em seus computadores com Windows 8. Eles também podem executar aplicativos do .NET Framework 1.0 e 1.1, desde que esses aplicativos não estejam explicitamente configurados para serem executados apenas no .NET Framework 1.0 e 1.1. Consulte [Migrating from the .NET Framework 1.1](../migration-guide/migrating-from-the-net-framework-1-1.md) (Migrando do .NET Framework 1.1).

Do .NET Framework 4.5 em diante, o registro em log de ativação do CLR foi aprimorado para incluir entradas de log que registram quando e por que a mensagem de erro de inicialização é exibida. Para obter mais informações, consulte [Como depurar problemas de ativação do CLR](how-to-debug-clr-activation-issues.md).

## <a name="see-also"></a>Consulte também

- [Guia de implantação para desenvolvedores](deployment-guide-for-developers.md)
- [Como configurar um aplicativo para dar suporte a .NET Framework 4 ou versões posteriores](../migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)
- [Como depurar problemas de ativação do CLR](how-to-debug-clr-activation-issues.md)
- [Instalar o .NET Framework 3.5 no Windows 10, Windows 8.1 e Windows 8](../install/dotnet-35-windows-10.md)
