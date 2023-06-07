---
title: 'Como: Remover um assembly do cache de assembly global'
description: Saiba como remover um assembly do cache de assembly global no .NET, usando a ferramenta global assembly cache (Gacutil.exe) ou Windows Installer.
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- Gacutil.exe
- global assembly cache, removing assemblies
- strong-named assemblies, global assembly cache
- removing assemblies from global assembly cache
- deleting assemblies in global assembly cache
- Global Assembly Cache tool
- GAC (global assembly cache), removing assemblies
ms.assetid: acdcc588-b458-436d-876c-726de68244c1
ms.openlocfilehash: e3a596ea6029ded190c33032e96b601de9d4012d
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104770"
---
# <a name="how-to-remove-an-assembly-from-the-global-assembly-cache"></a>Como: Remover um assembly do cache de assembly global

Há duas maneiras de remover um assembly do GAC (cache de assemblies global):

- Usando a [Ferramenta Cache de Assembly Global (Gacutil.exe)](../tools/gacutil-exe-gac-tool.md). Use essa opção para desinstalar assemblies que você colocou no GAC durante o desenvolvimento e teste.

- Usando o [Windows Installer](/windows/desktop/Msi/windows-installer-portal). Use essa opção para desinstalar assemblies ao testar pacotes de instalação e para sistemas de produção.

## <a name="removing-an-assembly-with-gacutilexe"></a>Remover um assembly com o Gacutil.exe

No prompt de comando, digite o comando a seguir:

**Gacutil – u**\<*assembly name*>

Neste comando, *nome do assembly* é o nome do assembly a ser removido do cache de assembly global.

> [!WARNING]
> Não use Gacutil.exe para remover assemblies em sistemas de produção devido à possibilidade de que o assembly ainda possa ser necessário para algum aplicativo. Em vez disso, use o Windows Installer, que mantém uma contagem de referência para cada assembly que ele instala no GAC.

O exemplo a seguir remove um assembly chamado `hello.dll` do cache de assembly global:

```console
gacutil -u hello
```

## <a name="removing-an-assembly-with-windows-installer"></a>Remover um assembly com o Windows Installer

No aplicativo **Programas e Recursos**, em **Painel de Controle**, selecione o aplicativo que você deseja desinstalar. Se o pacote de instalação tiver colocado assemblies no GAC, o Windows Installer os removerá se não forem usados por outro aplicativo.

> [!NOTE]
> O Windows Installer mantém uma contagem de referência para os assemblies instalados no GAC. Um assembly será removido do GAC apenas quando sua contagem de referência atingir zero, o que indica que ele não é usado por qualquer aplicativo instalado por um pacote do Windows Installer.

## <a name="see-also"></a>Veja também

- [Trabalhando com assemblies e o cache de assemblies global](working-with-assemblies-and-the-gac.md)
- [Como instalar um assembly no cache de assembly global](install-assembly-into-gac.md)
- [Gacutil.exe (Ferramenta de Cache de Assembly Global)](../tools/gacutil-exe-gac-tool.md)
