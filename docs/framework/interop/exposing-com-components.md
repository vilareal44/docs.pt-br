---
title: Expondo componentes do COM para o .NET Framework
description: Conheça o processo de exposição de componentes COM ao .NET. Os componentes COM são valiosos em código gerenciado como aplicativos de negócios de camada intermediária ou funcionalidade isolada.
ms.date: 03/30/2017
helpviewer_keywords:
- exposing COM components to .NET Framework
- interoperation with unmanaged code, exposing COM components
- COM interop, exposing COM components
ms.assetid: e78b14f1-e487-43cd-9c6d-1a07483f1730
ms.openlocfilehash: 459ba7ffed2e4f6c458f89a63b2baa37180d270d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85620840"
---
# <a name="exposing-com-components-to-the-net-framework"></a>Expondo componentes do COM para o .NET Framework
Esta seção resume o processo necessário para expor um componente COM existente ao código gerenciado. Para obter detalhes sobre como escrever servidores COM que são totalmente integrados ao .NET Framework, consulte [Considerações sobre design para interoperação](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/61aax4kh(v=vs.100)).
  
 Os componentes COM existentes são recursos valiosos no código gerenciado, como aplicativos de negócios de camada intermediária ou como uma funcionalidade isolada. Um componente ideal tem um assembly de interoperabilidade primário e está em conformidade total com os padrões de programação impostos pelo COM.  
  
#### <a name="to-expose-com-components-to-the-net-framework"></a>Para expor componentes COM ao .NET Framework  
  
1. [Importe uma biblioteca de tipos como um assembly](importing-a-type-library-as-an-assembly.md).  
  
     O Common Language Runtime exige metadados para todos os tipos, incluindo tipos COM. Há várias maneiras de obter um assembly que contém tipos COM importados como metadados.  
  
2. [Use tipos COM em código gerenciado](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100)).  
  
     É possível inspecionar tipos COM, ativar instâncias e invocar métodos no objeto COM da mesma maneira que você faria com qualquer tipo gerenciado.  
  
3. [Compile um projeto de interoperabilidade](compiling-an-interop-project.md).  
  
     O SDK do Windows fornece compiladores para várias linguagens em conformidade com CLS (Common Language Specification), incluindo Visual Basic, C# e C++.  
  
4. [Implante um aplicativo de interoperabilidade](deploying-an-interop-application.md).  
  
     Os aplicativos de interoperabilidade são mais bem implantados como assemblies assinados [de nome forte](../../standard/assembly/strong-named.md) no cache de assembly global.  
  
## <a name="see-also"></a>Consulte também

- [Interoperação com código não gerenciado](index.md)
- [Considerações sobre design para interoperação](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/61aax4kh(v=vs.100))
- [Exemplo de interoperabilidade COM: cliente .NET e servidor COM](com-interop-sample-net-client-and-com-server.md)
- [Componentes de independência de linguagem e componentes independentes da linguagem](../../standard/language-independence-and-language-independent-components.md)
- [Gacutil.exe (Ferramenta de Cache de Assembly Global)](../tools/gacutil-exe-gac-tool.md)
