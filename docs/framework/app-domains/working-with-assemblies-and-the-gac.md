---
title: Trabalhando com assemblies e o cache de assemblies global
description: Trabalhe com assemblies e o GAC (cache de assembly global) no .NET. Examine os motivos pelos quais você pode querer instalar um assembly no GAC.
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- global assembly cache, benefits
- ACLs [.NET Framework]
- GAC (global assembly cache), benefits
- access control lists [.NET Framework]
ms.assetid: 8a18e5c2-d41d-49ef-abcb-7c27e2469433
ms.openlocfilehash: 16cfd9faf02d5b58acad1cc0cf19be61c9814d35
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105152"
---
# <a name="working-with-assemblies-and-the-global-assembly-cache"></a>Trabalhando com assemblies e o cache de assemblies global

Se você pretender compartilhar um assembly com vários aplicativos, será necessário instalá-lo no cache de assembly global. Cada computador em que o Common Language Runtime está instalado tem um cache de código em todo o computador. O cache de assembly global armazena assemblies projetados especificamente para serem compartilhados por vários aplicativos no computador. Um assembly deve ter um nome forte para ser instalado no cache de assembly global.  
  
> [!NOTE]
> Assemblies colocados no cache de assembly global devem ter o mesmo nome de assembly e de arquivo (não incluindo a extensão de nome de arquivo). Por exemplo, um assembly com o nome de myAssembly deve ter um nome de arquivo de myAssembly.exe ou myAssembly.dll.  
  
Você deve compartilhar assemblies instalando-os no cache de assembly global somente quando necessário. Como diretriz geral, mantenha as dependências de um assembly particulares e localize os assemblies no diretório de aplicativo, a menos que o compartilhamento de um assembly seja explicitamente obrigatório. Além disso, você não precisa instalar assemblies no cache de assembly global a fim de torná-los acessíveis para interoperabilidade COM ou código não gerenciado.  
  
Há diversas razões para instalar um assembly no cache de assembly global:  
  
- Local compartilhado.  
  
     Assemblies que devem ser usados por aplicativos podem ser colocados no cache de assembly global. Por exemplo, se todos os aplicativos devem usar um assembly localizado no cache de assembly global, uma declaração de política de versão pode adicionada ao arquivo Machine.config que redireciona referências para o assembly.  
  
- Segurança do arquivo.  
  
     Os administradores geralmente protegem o diretório systemroot usando uma ACL (Lista de Controle de Acesso) para controlar os acesso de gravação e de execução. Como o cache de assembly global é instalado em um diretório do systemroot, ele herda a ACL desse diretório. É recomendável que apenas usuários com privilégios Administrador tenham permissão para excluir arquivos do cache de assembly global.  
  
- Controle de versão lado a lado.  
  
     Várias cópias de assemblies com o mesmo nome, mas com informações diferentes de versão, podem ser mantidas no cache de assembly global.  
  
- Local adicional de pesquisa.  
  
     O Common Language Runtime verifica o cache de assembly global para um assembly que corresponde à solicitação de assembly antes de investigar ou usar as informações da base de código em um arquivo de configuração.  
  
 Observe que há situações em que explicitamente não é recomendado instalar um assembly no cache de assembly global. Se colocar um dos assemblies que compõem um aplicativo no cache de assembly global, você não poderá mais replicar nem instalar o aplicativo usando XCOPY para copiar o diretório do aplicativo. Nesse caso, você também deve mover o assembly para o cache de assembly global.  
  
## <a name="in-this-section"></a>Nesta seção  
[Como instalar um assembly no cache de assembly global](install-assembly-into-gac.md)  
Descreve as maneiras de instalar um assembly no cache de assembly global.  
  
[Como: Exibir o conteúdo do cache de assembly global](how-to-view-the-contents-of-the-gac.md)  
Explica como usar a [Gacutil.exe (Ferramenta de Cache de Assembly Global)](../tools/gacutil-exe-gac-tool.md) para exibir o conteúdo do cache de assembly global.  
  
[Como: Remover um assembly do cache de assembly global](how-to-remove-an-assembly-from-the-gac.md)  
Explica como usar a [Gacutil.exe (Ferramenta de Cache de Assembly Global)](../tools/gacutil-exe-gac-tool.md) para remover um assembly do cache de assembly global.  
  
[Usando componentes atendidos com o cache de assemblies global](use-serviced-components-with-the-gac.md)  
Explica por que componentes de atendidos (componentes COM+ gerenciados) devem ser colocados no cache de assembly global.  
  
## <a name="related-sections"></a>Seções relacionadas  

[Criando assemblies](../../standard/assembly/create.md)  
Apresenta uma visão geral da criação de assemblies.  
  
[Cache de assemblies global](gac.md)  
Descreve o cache de assembly global.  
  
[Como exibir o conteúdo de um assembly](../../standard/assembly/view-contents.md)  
Explica como usar o [Ildasm.exe (IL Disassembler)](../tools/ildasm-exe-il-disassembler.md) para exibir informações MSIL (Microsoft Intermediate Language) em um assembly.  
  
[Como o runtime localiza assemblies](../deployment/how-the-runtime-locates-assemblies.md)  
Descreve como o Common Language Runtime localiza e carrega os assemblies que compõem seu aplicativo.  
  
[Programação com assemblies](../../standard/assembly/index.md)  
Descreve assemblies, os blocos de construção de aplicativos gerenciados.
