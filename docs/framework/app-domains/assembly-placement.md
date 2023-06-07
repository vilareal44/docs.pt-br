---
title: Posicionamento dos assemblies
description: Examine as diretrizes para o posicionamento do assembly .NET nos diretórios (por exemplo, no cache de assembly global ou no diretório ou subdiretório do aplicativo).
ms.date: 03/30/2017
helpviewer_keywords:
- <codeBase> element
- locating assemblies
- assemblies [.NET Framework], placement
- assemblies [.NET Framework], location
ms.assetid: ff8d48bc-f606-484f-9fe1-d0af264269fb
ms.openlocfilehash: 3106f6f01229057725cbc2e8e689a4e2247f95e6
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104959"
---
# <a name="assembly-placement"></a>Posicionamento dos assemblies
Para a maioria dos aplicativos .NET Framework, você localiza assemblies que compõem um aplicativo no diretório do aplicativo, em um subdiretório do diretório do aplicativo ou no cache de assembly global (se o assembly for compartilhado). Você pode substituir onde o Common Language Runtime procura um assembly usando o [ \<codeBase> elemento](../configure-apps/file-schema/runtime/codebase-element.md) em um arquivo de configuração. Se o assembly não tiver um nome forte, o local especificado usando o [ \<codeBase> elemento](../configure-apps/file-schema/runtime/codebase-element.md) será restrito ao diretório do aplicativo ou a um subdiretório. Se o assembly tiver um nome forte, o [ \<codeBase> elemento](../configure-apps/file-schema/runtime/codebase-element.md) poderá especificar qualquer local no computador ou em uma rede.  
  
 Regras similares se aplicam à localização de assemblies durante o trabalho com código não gerenciado ou aplicativos interop COM: se o assembly for compartilhado por vários aplicativos, ele deverá ser instalado no cache de assembly global. Assemblies usados com códigos não gerenciados devem ser exportados como uma biblioteca de tipos e registrados. Assemblies usados pelo interop COM devem ser registrados no catálogo, embora em alguns casos, esse registro ocorra automaticamente.  
  
## <a name="see-also"></a>Veja também

- [Como o runtime localiza assemblies](../deployment/how-the-runtime-locates-assemblies.md)
- [Configuração de aplicativos](../configure-apps/index.md)
- [Interoperação com código não gerenciado](../interop/index.md)
- [Assemblies no .NET](index.md)
