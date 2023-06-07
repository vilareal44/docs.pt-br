---
title: Cache de assemblies global
description: Entenda o cache de assembly global, que é um cache de código de todo o computador em que o Common Language Runtime para .NET está instalado.
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- GAC (global assembly cache)
- ACLs [.NET Framework]
- global assembly cache
- cache [.NET Framework], global assembly cache
- global assembly cache, about
- access control lists [.NET Framework]
ms.assetid: cf5eacd0-d3ec-4879-b6da-5fd5e4372202
ms.openlocfilehash: 7f08bb4cf279924b12432f259dae8ce5a8474285
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104910"
---
# <a name="global-assembly-cache"></a>Cache de assemblies global
Cada computador em que o Common Language Runtime está instalado tem um cache de código em todo o computador chamado Cache de Assembly Global. O Cache de Assembly Global armazena assemblies projetados especificamente para serem compartilhados por vários aplicativos no computador.  
  
 Você deverá compartilhar assemblies instalando-os no Cache de Assembly Global somente quando precisar. Como diretriz geral, mantenha as dependências de um assembly privadas e localize os assemblies no diretório de aplicativo, a menos que o compartilhamento de um assembly seja explicitamente obrigatório. Além disso, não é necessário instalar assemblies no Cache de Assembly Global a fim de torná-los acessíveis para interoperabilidade COM ou código não gerenciado.  
  
> [!NOTE]
> Há cenários em que você explicitamente não deseja instalar um assembly no Cache de Assembly Global. Se você colocar um dos assemblies que compõem um aplicativo no Cache de Assembly Global, não poderá mais replicar nem instalar o aplicativo usando o comando **xcopy** para copiar o diretório de aplicativo. Você também deve mover o assembly no Cache de Assembly Global.  
  
 Há duas maneiras de implantar um assembly no Cache de Assembly Global:  
  
- Usar um instalador projetado para funcionar com o Cache de Assembly Global. Essa é a opção preferencial para instalar assemblies no Cache de Assembly Global.  
  
- Use uma ferramenta de desenvolvedor chamada [Cache de Assembly Global (Gacutil.exe)](../tools/gacutil-exe-gac-tool.md), fornecida pelo SDK do Windows.  
  
    > [!NOTE]
    > Em cenários de implantação, use o Windows Installer para instalar assemblies no Cache de Assembly Global. Só use a ferramenta Global Assembly Cache em cenários de desenvolvimento, porque ela não fornece contagem de referência de assembly e outros recursos fornecidos durante o uso do Windows Installer.  
  
 A partir do .NET Framework 4, a localização padrão do Cache de Assembly Global é **%windir%\Microsoft.NET\assembly**. Em versões anteriores do .NET Framework, o local padrão era **%windir%\assembly**.  
  
 Os administradores geralmente protegem o diretório systemroot usando uma ACL (lista de controle de acesso) para controlar acesso de escrita e execução. Como o Cache de Assembly Global é instalado em um subdiretório do diretório systemroot, ele herda a ACL desse diretório. É recomendável que apenas usuários com privilégios de Administrador tenham permissão para excluir arquivos do Cache de Assembly Global.  
  
 Assemblies implantados no Cache de Assembly Global devem ter um nome forte. Quando um assembly é adicionado ao Cache de Assembly Global, são executadas verificações de integridade em todos os arquivos que compõem o assembly. O cache executa essas verificações de integridade para garantir que um assembly não tenha sido adulterado, por exemplo, quando um arquivo é alterado, mas o manifesto não reflete a alteração.  
  
## <a name="see-also"></a>Veja também

- [Assemblies no .NET](../../standard/assembly/index.md)
- [Trabalhando com assemblies e o cache de assemblies global](working-with-assemblies-and-the-gac.md)
- [Assemblies de nome forte](../../standard/assembly/strong-named.md)
