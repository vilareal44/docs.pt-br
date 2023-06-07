---
title: .NET Framework e versões fora de banda
description: Saiba mais sobre o .NET e as versões fora de banda. Novos recursos são lançados fora de banda (OOB) para melhorar o desenvolvimento de plataforma cruzada ou para introduzir novas funcionalidades.
ms.date: 10/10/2018
ms.assetid: 721f10fa-3189-4124-a00d-56ddabd889b3
ms.openlocfilehash: 9653696f46279e0c23418f92030d64839cc20518
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618760"
---
# <a name="net-framework-and-out-of-band-releases"></a>.NET Framework e lançamentos fora de banda

A .NET Framework evoluiu para acomodar diferentes plataformas, como aplicativos UWP e aplicativos Web e de área de trabalho tradicionais, e para maximizar a reutilização de código. Além das versões .NET Framework regulares, novos recursos são lançados fora de banda (OOB) para melhorar o desenvolvimento de várias plataformas ou para introduzir novas funcionalidades.

## <a name="advantages-of-oob-releases"></a>Vantagens das versões OOB

O envio de novos componentes ou atualizações para componentes fora da banda permite que a Microsoft forneça atualizações mais frequentes para .NET Framework. Além disso, podemos coletar e responder aos comentários dos clientes de forma mais rápida.

Quando você usa um recurso OOB em seu aplicativo, os usuários não precisam instalar a versão mais recente do .NET Framework para executar seu aplicativo, pois os assemblies OOB são implantados com o pacote do aplicativo.

## <a name="how-oob-packages-are-distributed"></a>Como os pacotes OOB são distribuídos

As versões OOB dos componentes de Common Language Runtime de núcleo (CLR) são entregues por meio do [NuGet](https://www.nuget.org/), que é o Gerenciador de pacotes do .net. O NuGet permite que você navegue e adicione bibliotecas a seus projetos de .NET Framework facilmente de dentro do Visual Studio. O Gerenciador de pacotes NuGet está incluído em todas as edições do Visual Studio a partir do Visual Studio 2012. Procure **Gerenciador de pacotes NuGet** no menu **ferramentas** no Visual Studio. Se ele não estiver instalado, siga as instruções em [instalando o NuGet](/nuget/install-nuget-client-tools). Para obter mais informações sobre o NuGet, consulte os [documentos do NuGet](/nuget).

## <a name="use-a-nuget-oob-package"></a>Usar um pacote OOB do NuGet

Se o Gerenciador de pacotes NuGet estiver instalado, você poderá procurar e adicionar referências a pacotes NuGet usando Gerenciador de Soluções no Visual Studio:

1. Abra o menu de atalho para seu projeto no Visual Studio e, em seguida, escolha **Gerenciar Pacotes NuGet**. (Esta opção também está disponível no menu **Projeto**.)

2. No painel esquerdo, escolha **Online**.

3. Se você deseja usar pacotes de pré-lançamento, na caixa de lista suspensa no painel intermediário, escolha **Incluir Pré-lançamento** em vez de **Somente Estável**.

4. No painel direito, use a caixa **Pesquisar** para localizar o pacote que deseja usar. Alguns pacotes da Microsoft são identificados pelo logotipo do Microsoft .NET Framework. Todos eles identificam Microsoft como o editor.

![O Gerenciador de pacotes NuGet.](./media/the-net-framework-and-out-of-band-releases/nuget-package-manager-dialog.png)

Conforme mencionado anteriormente, quando você implanta um aplicativo que usa um pacote OOB, os assemblies OOB são fornecidos com o pacote do aplicativo.

## <a name="types-of-oob-releases"></a>Tipos de versão OOB

Normalmente, um conjunto OOB possui uma ou várias versões de pré-lançamento e uma versão estável. A licença que acompanha uma versão pré-lançamento normalmente não permite a redistribuição, mas permite que você teste um pacote e forneça comentários. Os comentários são incorporados em quaisquer atualizações feitas no pacote. Um versão final é distribuída na forma de pacote estável com o NuGet e inclui uma licença que permite redistribuir o pacote Nuget com seu aplicativo. Pacotes estáveis têm suprote pela Microsoft. A Microsoft fornece suporte para IntelliSense, bem como para outros tipos de documentação, como postagens em blogs e respostas em fóruns para todos os pacotes. Além disso, o código-fonte pode ser disponibilizado em alguns pacotes, mas não em todos. Para consultar anúncios em relação a novos pacotes e atualizações, inscreva-se no [Blog do .NET Framework](https://devblogs.microsoft.com/dotnet/).

Para localizar pacotes de pré-lançamento e estáveis, escolha **incluir pré-lançamento** no Gerenciador de pacotes NuGet.

## <a name="see-also"></a>Consulte também

- [Guia de Introdução](index.md)
