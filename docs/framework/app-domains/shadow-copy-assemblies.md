---
title: Criando cópias de sombra de assemblies
description: Explore a cópia de sombra de assemblies no .NET, de modo que os que são usados em um domínio de aplicativo possam ser atualizados sem descarregar o domínio do aplicativo.
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], shadow copying
- application domains, shadow copying assemblies
- shadow copying assemblies
ms.assetid: de8b8759-fca7-4260-896b-5a4973157672
ms.openlocfilehash: a7ff72763dd26dbc50cd37e070c2d25ababa00f3
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104563"
---
# <a name="shadow-copying-assemblies"></a>Criando cópias de sombra de assemblies

A criação de cópias de sombra permite que os assemblies usados em um domínio de aplicativo sejam atualizados sem descarregar o domínio de aplicativo. Isso é particularmente útil para aplicativos que devem estar disponíveis continuamente, como sites do ASP.NET.

> [!IMPORTANT]
> Não há suporte para cópia de sombra em aplicativos da loja do Windows 8. x.

O common language runtime bloqueia um arquivo do assembly quando o assembly é carregado para que o arquivo não possa ser atualizado até que o assembly seja descarregado. A única maneira de descarregar um assembly de um domínio de aplicativo é descarregar o domínio de aplicativo. Portanto, em circunstâncias normais, um assembly não pode ser atualizado no disco até que todos os domínios de aplicativo que o estão usando sejam descarregados.

Quando um domínio de aplicativo é configurado para cópia de sombra de arquivos, os assemblies do caminho do aplicativo são copiados em outro local e carregados a partir desse local. A cópia é bloqueada, mas o arquivo do assembly original é desbloqueado e pode ser atualizado.

> [!IMPORTANT]
> Os únicos assemblies dos quais é possível realizar uma cópia de sombra são aqueles armazenados no diretório do aplicativo ou em seus subdiretórios, especificados pelas propriedades <xref:System.AppDomainSetup.ApplicationBase%2A> e <xref:System.AppDomainSetup.PrivateBinPath%2A> quando o domínio de aplicativo é configurado. Os assemblies armazenados no cache de assembly global não passam por cópias de sombra.

Este artigo inclui as seções a seguir:

- [Habilitar e usar cópias de sombra](#EnablingAndUsing) descreve o uso básico e as opções disponíveis para cópia de sombra.

- [Desempenho de inicialização](#StartupPerformance) descreve as alterações feitas na cópia de sombra no .NET Framework 4 para melhorar o desempenho de inicialização, e como reverter para o comportamento de versões anteriores.

- [Métodos obsoletos](#ObsoleteMethods) descreve as alterações feitas nas propriedades e nos métodos que controlam a cópia de sombra no .NET Framework 2.0.

<a name="EnablingAndUsing"></a>

## <a name="enabling-and-using-shadow-copying"></a>Habilitando e Usando Cópias de Sombra

Você pode usar as propriedades da classe <xref:System.AppDomainSetup> da seguinte forma para configurar um domínio do aplicativo para a cópia de sombra:

- Habilite a cópia de sombra definindo a propriedade <xref:System.AppDomainSetup.ShadowCopyFiles%2A> com o valor de cadeia de caracteres `"true"`.

  Por padrão, essa configuração faz com que todos os assemblies no caminho do aplicativo sejam copiados para um cache de download antes de serem carregados. Esse é o mesmo cache mantido pelo common language runtime para armazenar os arquivos baixados de outros computadores, e o common language runtime exclui automaticamente os arquivos quando eles não são mais necessários.

- Opcionalmente, defina um local personalizado para os arquivos da cópia de sombra usando as propriedades <xref:System.AppDomainSetup.CachePath%2A> e <xref:System.AppDomainSetup.ApplicationName%2A>.

  O caminho base para o local é formado pela concatenação da propriedade <xref:System.AppDomainSetup.ApplicationName%2A> para a propriedade <xref:System.AppDomainSetup.CachePath%2A> como um subdiretório. Assemblies passam por cópia de sombra para subpastas nesse caminho, e não para o caminho base em si.

  > [!NOTE]
  > Se a propriedade <xref:System.AppDomainSetup.ApplicationName%2A> não for definida, a propriedade <xref:System.AppDomainSetup.CachePath%2A> será ignorada e o cache de download será usado. Nenhuma exceção é lançada.

  Se especificar um local personalizado, você será responsável pela limpeza de diretórios e arquivos copiados quando eles não forem mais necessários. Eles não são excluídos automaticamente.

  Há alguns motivos para você definir um local personalizado para arquivos de cópia de sombra. Talvez você queira definir um local personalizado para os arquivos de cópia de sombra se o aplicativo gerar uma grande quantidade de cópias. O cache de download tem um limite de tamanho, não de tempo de vida, portanto, é possível que o common language runtime tente excluir um arquivo que ainda está em uso. Outro motivo para definir um local personalizado é quando os usuários que estão executando seu aplicativo não tiverem acesso de gravação ao local do diretório usado pelo common language runtime para o cache de download.

- Opcionalmente, limite os assemblies dos quais é realizada a cópia de sombra usando a propriedade <xref:System.AppDomainSetup.ShadowCopyDirectories%2A>.

  Quando você habilita a cópia de sombra para um domínio do aplicativo, o padrão é copiar todos os assemblies no caminho do aplicativo — ou seja, nos diretórios especificados pelas propriedades <xref:System.AppDomainSetup.ApplicationBase%2A> e <xref:System.AppDomainSetup.PrivateBinPath%2A>. Você pode limitar a cópia aos diretórios selecionados criando uma cadeia de caracteres que contém apenas os diretórios dos quais deseja realizar a cópia de sombra e atribuindo a cadeia de caracteres à propriedade <xref:System.AppDomainSetup.ShadowCopyDirectories%2A>. Separe os diretórios com ponto e vírgula. Os únicos assemblies dos quais são feitas cópias de sombra são aqueles nas pastas selecionadas.

  > [!NOTE]
  > Se você não atribuir uma cadeia de caracteres à propriedade <xref:System.AppDomainSetup.ShadowCopyDirectories%2A> ou se definir essa propriedade como `null`, será feita a cópia de sombra de todos os assemblies nos diretórios especificados pelas propriedades <xref:System.AppDomainSetup.ApplicationBase%2A> e <xref:System.AppDomainSetup.PrivateBinPath%2A>.

  > [!IMPORTANT]
  > Os caminhos de diretório não devem conter ponto e vírgula, pois esse é o caractere delimitador. Não há nenhum caractere de escape para ponto e vírgula.

<a name="StartupPerformance"></a>

## <a name="startup-performance"></a>Desempenho da Inicialização

Quando um domínio de aplicativo que usa a cópia de sombra for iniciado, haverá um atraso durante a cópia dos assemblies do diretório do aplicativo para o diretório de cópia de sombra, ou durante a verificação se eles já estão no local. Antes do .NET Framework 4, todos os assemblies eram copiados para um diretório temporário. Cada assembly era aberto para verificar o nome do assembly, e o nome forte era validado. Cada assembly era analisado para ver se havia sido atualizado mais recentemente do que a cópia no diretório de cópia de sombra. Nesse caso, ele foi copiado para o diretório de cópia de sombra. Por fim, as cópias temporárias eram descartadas.

Do .NET Framework 4 em diante, o comportamento de inicialização padrão é comparar diretamente a data e hora do arquivo de cada assembly no diretório de aplicativo com a data e hora do arquivo da cópia no diretório de cópia de sombra. Se o assembly tiver sido atualizado, ele será copiado usando o mesmo procedimento das versões anteriores do .NET Framework; caso contrário, a cópia no diretório de cópia de sombra será carregada.

A melhoria no desempenho resultante é maior para aplicativos nos quais os assemblies não são alterados com frequência, e as alterações normalmente ocorrem em um pequeno subconjunto dos assemblies. Se a maioria dos assemblies em um aplicativo mudar com frequência, o novo comportamento padrão poderá causar uma regressão do desempenho. Você pode restaurar o comportamento de inicialização de versões anteriores do .NET Framework adicionando o [ \<shadowCopyVerifyByTimestamp> elemento](../configure-apps/file-schema/runtime/shadowcopyverifybytimestamp-element.md) ao arquivo de configuração, com `enabled="false"` .

<a name="ObsoleteMethods"></a>

## <a name="obsolete-methods"></a>Métodos obsoletos

A classe <xref:System.AppDomain> tem vários métodos, como <xref:System.AppDomain.SetShadowCopyFiles%2A> e <xref:System.AppDomain.ClearShadowCopyPath%2A>, que podem ser usados para controlar a cópia de sombra em um domínio do aplicativo, mas eles foram marcados como obsoletos no .NET Framework versão 2.0. A maneira recomendada de configurar um domínio do aplicativo para a cópia de sombra é usar as propriedades da classe <xref:System.AppDomainSetup>.

## <a name="see-also"></a>Veja também

- <xref:System.AppDomainSetup.ShadowCopyFiles%2A?displayProperty=nameWithType>
- <xref:System.AppDomainSetup.CachePath%2A?displayProperty=nameWithType>
- <xref:System.AppDomainSetup.ApplicationName%2A?displayProperty=nameWithType>
- <xref:System.AppDomainSetup.ShadowCopyDirectories%2A?displayProperty=nameWithType>
- [\<shadowCopyVerifyByTimestamp>Elementos](../configure-apps/file-schema/runtime/shadowcopyverifybytimestamp-element.md)
