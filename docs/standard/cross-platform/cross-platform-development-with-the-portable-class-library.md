---
title: Desenvolvimento entre plataformas com a Biblioteca de Classes Portátil
description: Crie aplicativos e bibliotecas de plataforma cruzada para plataformas da Microsoft de forma rápida e fácil usando o tipo de projeto de biblioteca de classes portátil no .NET.
ms.date: 09/17/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- Portable Class Library [.NET Framework]
- targeting multiple platforms
- multiple platforms, targeting
ms.assetid: c31e1663-c164-4e65-b66d-d3aa8750a154
ms.openlocfilehash: be1a49f7da7ce98f9e5e3ff8d927ce5230bfa8d8
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769133"
---
# <a name="cross-platform-development-with-the-portable-class-library"></a>Desenvolvimento de plataforma cruzada com a biblioteca de classes portátil

O tipo de projeto de biblioteca de classes portátil no Visual Studio ajuda a criar aplicativos e bibliotecas de plataforma cruzada para plataformas da Microsoft de forma rápida e fácil.

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]

Bibliotecas de classes portáteis podem ajudar a reduzir o tempo e os custos com desenvolvimento e testes de código. Use este tipo de projeto para gravar e criar assemblies de .NET Framework portáteis e, em seguida, fazer referência a esses assemblies de aplicativos direcionados a várias plataformas, como o .NET Framework, iOS ou Mac.

Mesmo depois de criar um projeto de Biblioteca de Classes Portátil no Visual Studio e começar a desenvolvê-lo, é possível alterar as plataformas de destino. O Visual Studio compila sua biblioteca com os novos assemblies, o que ajuda a identificar as alterações que você precisa fazer em seu código.

## <a name="create-a-portable-class-library-project"></a>Criar um projeto de biblioteca de classes portátil

Para criar uma biblioteca de classes portátil, use o modelo fornecido no Visual Studio. Crie um novo projeto (**arquivo**  >  **novo projeto**) e, na caixa de diálogo **novo projeto** , selecione sua linguagem de programação (Visual C# ou Visual Basic). Em seguida, selecione o modelo **biblioteca de classes (portátil herdado)** . Insira um nome para seu projeto e escolha **OK**.

A caixa de diálogo **Adicionar biblioteca de classes portátil** é exibida. Escolha dois ou mais destinos e, em seguida, escolha **OK**.

![Adicionar destinos de biblioteca de classes portáteis no Visual Studio](media/add-portable-class-library.png)

## <a name="change-targets"></a>Alterar destinos

Você pode alterar as plataformas de destino de um projeto de biblioteca de classes portátil ao criá-lo ou depois de iniciar o desenvolvimento. Se você quiser alterar os destinos depois de criar seu projeto, no **Gerenciador de soluções**, abra o menu de atalho do seu projeto de biblioteca de classes portátil (não a solução) e escolha **Propriedades**. Na página Propriedades do projeto, a guia **biblioteca** mostra as plataformas que o seu projeto tem como destino no momento.

![Propriedades do projeto para a biblioteca de classes portátil no Visual Studio](media/pcl-project-properties.png)

Para adicionar ou remover destinos, escolha o botão **alterar** e, em seguida, marque e desmarque as caixas de seleção apropriadas.

Ao alterar os destinos, as APIs disponíveis para desenvolver o projeto mudarão conforme a sua seleção. O Visual Studio relata os erros e avisos que podem ocorrer como resultado de alterações nos destinos.

Se você quiser avaliar a portabilidade de seus assemblies antes de fazer alterações no Visual Studio, poderá usar o [analisador de portabilidade do .net](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer).

## <a name="supported-types-and-members"></a>Tipos e membros com suporte

Os tipos e membros disponíveis em projetos da Biblioteca de Classes Portátil são restritos por vários fatores de compatibilidade:

- Eles devem ser compartilhados entre os destinos selecionados.

- Eles devem se comportar de forma semelhante nesses destinos.

- Eles não devem ser candidatos à substituição.

- Eles devem fazer sentido em um ambiente portátil, especialmente quando os membros de suporte não são portáveis.

Se houver suporte a um membro na Biblioteca de Classes Portátil e aos destinos selecionados, isso aparecerá no projeto no IntelliSense. Porém, lembre-se de que uma API pode ter suporte na Biblioteca de Classes Portátil, mas sua capacidade de usá-la dependerá dos destinos selecionados.

## <a name="api-differences-in-the-portable-class-library"></a>Diferenças de API na Biblioteca de Classes Portátil

Para tornar os assemblies da Biblioteca de Classes Portátil compatíveis entre todas as plataformas com suporte, alguns membros foram ligeiramente alterados na Biblioteca de Classes Portátil.

## <a name="use-the-portable-class-library"></a>Usar a biblioteca de classes portátil

Depois de criar o projeto da Biblioteca de Classes Portátil, você simplesmente faz uma referência a ele em outros projetos. Você pode fazer referência ao projeto ou assemblies específicos que contêm as classes que deseja acessar.

Para executar um aplicativo que faça referência a um assembly da Biblioteca de Classes Portátil, deve ser instalada a versão necessária (ou uma versão posterior) das plataformas de destino no computador. O Visual Studio contém todas as estruturas necessárias, assim, você pode executar aplicativos sem alteração adicional no computador usado para desenvolver o aplicativo.

### <a name="deploy-a-universal-windows-app"></a>Implantar um aplicativo universal do Windows

Quando você cria um aplicativo universal do Windows que faz referência a um assembly de biblioteca de classes portátil, tudo o que você precisa para implantar o aplicativo é incluído no pacote do aplicativo e nenhuma etapa adicional é necessária.

### <a name="deploy-a-net-framework-app"></a>Implantar um aplicativo .NET Framework

Ao implantar o aplicativo do .NET Framework que faça referência a um assembly da Biblioteca de Classes Portátil, você deve especificar uma dependência na versão correta do .NET Framework. Ao especificar essa dependência, você garante que a versão requisitada seja instalada com seu aplicativo.

- Para criar uma dependência com a implantação do ClickOnce: em **Gerenciador de soluções**, escolha o nó do projeto que você deseja publicar. (Esse é o projeto que faz referência ao projeto de biblioteca de classes portátil.) Na barra de menus, escolha **Project**  >  **Propriedades**do projeto e, em seguida, escolha a guia **publicar** . Na página **publicar** , escolha **pré-requisitos**. Selecione a versão necessária do .NET Framework como um pré-requisito.

- Para criar uma dependência com um projeto de instalação: em **Gerenciador de soluções**, escolha o projeto de instalação. Na barra de menus, escolha **projeto**  >  **Propriedades**  >  **pré-requisitos**. Selecione a versão necessária do .NET Framework como um pré-requisito.

Para obter mais informações sobre a implantação de aplicativos .NET Framework, consulte [Guia de implantação para desenvolvedores](../../framework/deployment/deployment-guide-for-developers.md).

## <a name="see-also"></a>Veja também

- [Usando a Biblioteca de Classes Portátil com o Modelo MVVM](using-portable-class-library-with-model-view-view-model.md)
- [Recursos do aplicativo para bibliotecas direcionadas a várias plataformas](app-resources-for-libraries-that-target-multiple-platforms.md)
- [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)
- [Suporte do .NET Framework para Aplicativos da Windows Store e Windows Runtime](support-for-windows-store-apps-and-windows-runtime.md)
- [Implantação](../../framework/deployment/net-framework-applications.md)
