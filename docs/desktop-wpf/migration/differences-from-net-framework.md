---
title: Diferenças entre o .NET Framework e o .NET Core
description: Descreve as diferenças entre a implementação de .NET Framework do Windows Presentation Foundation (WPF) e do WPF do .NET Core. Ao migrar seu aplicativo, você deve considerar essas incompatibilidades.
author: adegeo
ms.date: 09/21/2019
ms.author: adegeo
ms.openlocfilehash: 3bedc30046c36d4c5430feedf5854276ebaef8aa
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325692"
---
# <a name="differences-in-wpf"></a>Diferenças no WPF

Este artigo descreve as diferenças entre Windows Presentation Foundation (WPF) no .NET Core e .NET Framework. O WPF para .NET Core é uma [estrutura de código-fonte aberto](https://github.com/dotnet/wpf) bifurcada do WPF original para .NET Framework código-fonte.

Há alguns recursos de .NET Framework para os quais o .NET Core não dá suporte. Para obter mais informações sobre tecnologias sem suporte, consulte [.NET Framework Technologies indisponíveis no .NET Core](../../core/porting/net-framework-tech-unavailable.md).

[!INCLUDE [desktop guide under construction](../../../includes/desktop-guide-preview-note.md)]

## <a name="sdk-style-projects"></a>Projetos no estilo SDK

O .NET Core usa arquivos de projeto no estilo SDK. Esses arquivos de projeto são diferentes dos arquivos de projeto .NET Framework tradicionais gerenciados pelo Visual Studio. Para migrar seus aplicativos .NET Framework WPF para o .NET Core, você deve converter seus projetos. Para obter mais informações, consulte [migrar aplicativos do WPF para o .NET Core 3,0](convert-project-from-net-framework.md).

## <a name="nuget-package-references"></a>Referências do pacote NuGet

Se seu aplicativo .NET Framework listar suas dependências do NuGet em um arquivo *packages.config* , migre para o [`<PackageReference>`](/nuget/consume-packages/package-references-in-project-files) formato:

1. No Visual Studio, abra o painel **Gerenciador de soluções** .
1. Em seu projeto do WPF, clique com o botão direito do mouse em **packages.config**  >  **migrar packages.config para PackageReference**.

![Atualizando para o PackageReference](media/differences-from-net-framework/package-reference-migration.png)

Uma caixa de diálogo será exibida mostrando as dependências do NuGet de nível superior calculadas e perguntando quais outros pacotes NuGet devem ser promovidos para o nível superior. Selecione **OK** e o arquivo de *packages.config* será removido do projeto e os `<PackageReference>` elementos serão adicionados ao arquivo de projeto.

Quando seu projeto usa `<PackageReference>` , os pacotes não são armazenados localmente em uma pasta de *pacotes* , eles são armazenados globalmente. Abra o arquivo de projeto e remova todos os `<Analyzer>` elementos que referenciaram a pasta *pacotes* . Esses analisadores são incluídos automaticamente com as referências do pacote NuGet.

## <a name="code-access-security"></a>Segurança de Acesso do Código

A CAS (segurança de acesso ao código) não tem suporte do .NET Core ou do WPF para .NET Core. Todas as funcionalidades relacionadas ao CAS são tratadas sob a suposição de confiança total. O WPF para .NET Core remove o código relacionado ao CAS. A superfície da API pública desses tipos ainda existe para garantir que as chamadas nesses tipos tenham sucesso.

Os tipos relacionados ao CAS definidos publicamente foram movidos para fora dos assemblies do WPF e para os assemblies da biblioteca .NET principal. Os assemblies do WPF têm o encaminhamento de tipo definido para o novo local dos tipos movidos.

| Assembly de origem | Assembly de destino | Tipo                |
| --------------- | --------------- | ------------------- |
| *WindowsBase.dll* | *System.Security.Permissions.dll* | <xref:System.Security.Permissions.MediaPermission> <br /> <xref:System.Security.Permissions.MediaPermissionAttribute> <br /> <xref:System.Security.Permissions.MediaPermissionAudio> <br /> <xref:System.Security.Permissions.MediaPermissionImage> <br /> <xref:System.Security.Permissions.MediaPermissionVideo> <br /> <xref:System.Security.Permissions.WebBrowserPermission> <br /> <xref:System.Security.Permissions.WebBrowserPermissionAttribute> <br /> <xref:System.Security.Permissions.WebBrowserPermissionLevel> |
| *System.Xaml.dll* | *System.Security.Permissions.dll* | <xref:System.Xaml.Permissions.XamlLoadPermission> |
| *System.Xaml.dll* | *System.Windows.Extension.dll*    | <xref:System.Xaml.Permissions.XamlAccessLevel><br/> |

> [!NOTE]
> Para minimizar o conflito de portamento, a funcionalidade para armazenar e recuperar informações relacionadas às propriedades a seguir foi mantida no `XamlAccessLevel` tipo.
>
> - `PrivateAccessToTypeName`
> - `AssemblyNameString`

## <a name="next-steps"></a>Próximas etapas

- [Saiba como portar um aplicativo .NET Framework WPF para o .NET Core.](convert-project-from-net-framework.md)
