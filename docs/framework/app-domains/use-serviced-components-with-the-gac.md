---
title: Usando componentes atendidos com o cache de assemblies global
description: Use componentes de serviço (componentes COM+ de código gerenciado) com o cache de assembly global no .NET. Veja se os serviços CLR e COM+ podem manipular componentes não GAC.
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- GAC (global assembly cache), serviced components
- serviced components, global assembly cache
- global assembly cache, serviced components
ms.assetid: 3423e5d9-234c-4571-8161-e35f6d130128
ms.openlocfilehash: 6b7371865b7b1cedda0ee03b2cc28c74b5c3da0b
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104474"
---
# <a name="using-serviced-components-with-the-global-assembly-cache"></a>Usando componentes atendidos com o cache de assemblies global
Componentes atendidos (componentes COM+ com código gerenciado) devem ser colocados no Cache de Assembly Global. Em alguns cenários, o Common Language Runtime e os Serviços COM+ podem manipular componentes atendidos que não estão no Cache de Assembly Global; em outros cenários, eles não podem. Os seguintes cenários ilustram isso:  
  
- Para componentes atendidos em um aplicativo COM+ para Servidor, o assembly que contém os componentes deve estar no Cache de Assembly Global, pois Dllhost.exe não é executado no mesmo diretório que contém os componentes atendidos.  
  
- Para componentes atendidos em um aplicativo de COM+ Library, o runtime e COM+ Services podem resolver as referências ao assembly que contém os componentes pesquisando o diretório atual. Nesse caso, o assembly não precisa estar no cache de assembly global.  
  
- Para componentes atendidos em um aplicativo ASP.NET, a situação é diferente. Se você colocar o assembly que contém os componentes atendidos no diretório bin da base de aplicativo e usar o registro sob demanda, o assembly será copiado por sombra para o cache de download, pois o ASP.NET aproveita os recursos de sombra do runtime.  
  
## <a name="see-also"></a>Veja também

- [Trabalhando com assemblies e o cache de assemblies global](working-with-assemblies-and-the-gac.md)
- [Gacutil.exe (Ferramenta de Cache de Assembly Global)](../tools/gacutil-exe-gac-tool.md)
