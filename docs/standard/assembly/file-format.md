---
title: Formato de arquivo do assembly .NET
description: Saiba mais sobre o formato de arquivo do assembly do .NET, que é usado para descrever e conter aplicativos e bibliotecas do .NET.
author: richlander
ms.date: 08/20/2019
ms.technology: dotnet-standard
ms.assetid: 6520323e-ff28-4c8a-ba80-e64a413199e6
ms.openlocfilehash: b4aa961c3a6f2d4fa1580ff608aaf2a40d462fa0
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288505"
---
# <a name="net-assembly-file-format"></a>Formato de arquivo do assembly .NET

O .NET define um formato de arquivo binário, *assembly*, que é usado para descrever totalmente e conter programas .net. Os assemblies são usados para os programas em si, bem como todas as bibliotecas dependentes. Um programa .NET pode ser executado como um ou mais assemblies, sem a necessidade de nenhum outro artefato, além da implementação do .NET apropriado. As dependências nativas, incluindo as APIs do sistema operacional, são uma preocupação separada e não estão contidas no formato do assembly .NET, embora elas às vezes sejam descritas com esse formato (por exemplo, WinRT).

> Cada componente de CLI carrega os metadados para declarações, implementações e referências específicas para esse componente. Portanto, os metadados específicos do componente são conhecidos como metadados de componente e o componente resultante deve ser autodescritivo – do ECMA 335 I.9.1, Componentes e assemblies.

O formato é totalmente especificado e padronizado como [ECMA 335](https://www.ecma-international.org/publications/standards/Ecma-335.htm). Todos os runtimes e compiladores .NET usam esse formato. A presença de um formato binário documentado e raramente atualizado foi um grande benefício (sem dúvida, um requisito) da interoperabilidade. O formato foi atualizado de forma substancial pela última vez em 2005 (.NET 2.0) para acomodar os genéricos e a arquitetura do processador.

O formato é independente de CPU e sistema operacional. Ele tem sido usado como parte de implementações do .NET que se destinam a muitos chips e CPUs. Embora o formato em si tenha patrimônio do Windows, é implementável em qualquer sistema operacional. Sua escolha indiscutivelmente mais significativa para a interoperabilidade do sistema operacional é que a maioria dos valores é armazenada no formato little endian. Ele não tem uma afinidade específica para o tamanho do ponteiro de computador (por exemplo, 32 bits, 64 bits).

O formato do assembly .NET também é muito descritivo sobre a estrutura de um determinado programa ou biblioteca. Ele descreve os componentes internos de um assembly, especificamente referências de assembly e tipos definidos e sua estrutura interna. APIs ou ferramentas podem ler e processar essas informações para exibição ou para tomar decisões de programação.

## <a name="format"></a>Formatar

O formato binário do .NET se baseia no formato de [arquivo PE](https://en.wikipedia.org/wiki/Portable_Executable) do Windows. Na verdade, as bibliotecas de classes do .NET são compatíveis com PEs do Windows e, a primeira vista, parecem ser DLLs (bibliotecas de vínculo dinâmico) do Windows ou EXEs (executáveis) de aplicativo. Essa é uma característica muito útil no Windows, no qual elas podem se mascarar como binários executáveis nativos e obter parte do mesmo tratamento (por exemplo, o carregamento do sistema operacional, ferramentas PE).

![Cabeçalhos de assembly](../media/assembly-format/assembly-headers.png)

Cabeçalhos de assembly do ECMA 335 II.25.1, Estrutura do formato de arquivo do runtime.

## <a name="process-the-assemblies"></a>Processar os assemblies

É possível escrever APIs ou ferramentas para processar assemblies. As informações do assembly permitem fazer decisões programáticas em tempo de execução, reescrever assemblies, fornecer IntelliSense de API em um editor e gerar documentação. <xref:System.Reflection?displayProperty=nameWithType>, <xref:System.Reflection.MetadataLoadContext?displayProperty=nameWithType> e [Mono.Cecil](https://www.mono-project.com/docs/tools+libraries/libraries/Mono.Cecil/) são bons exemplos de ferramentas que frequentemente são usadas para essa finalidade.
