---
title: Nomes de Assemblies e DLLs
description: Conheça as diretrizes para nomear assemblies e DLLs (bibliotecas de vínculo dinâmico). Um assembly pode abranger um ou mais arquivos, mas geralmente mapeia um para um com uma DLL.
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- names [.NET Framework], DLLs
- names [.NET Framework], assemblies
- assemblies [.NET Framework], names
- DLLs, names
ms.assetid: e800b610-31b4-4949-9c14-cb60e9f254be
ms.openlocfilehash: de7ce3ee774d4598521d7156d0d660c3fe30154c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594472"
---
# <a name="names-of-assemblies-and-dlls"></a>Nomes de Assemblies e DLLs
Um assembly é a unidade de implantação e identidade para programas de código gerenciado. Embora assemblies possam abranger um ou mais arquivos, normalmente um assembly mapeia um para um com uma DLL. Portanto, esta seção descreve apenas as convenções de nomenclatura de DLL, que podem ser mapeadas para convenções de nomenclatura de assembly.

 ✔️ escolher nomes para suas DLLs de assembly que sugerem grandes partes de funcionalidade, como System. Data.

 Nomes de assembly e DLL não precisam corresponder a nomes de namespace, mas é razoável seguir o nome do namespace ao nomear assemblies. Uma boa regra prática é nomear a DLL com base no prefixo comum dos namespaces contidos no assembly. Por exemplo, um assembly com dois namespaces, `MyCompany.MyTechnology.FirstFeature` e `MyCompany.MyTechnology.SecondFeature` , pode ser chamado `MyCompany.MyTechnology.dll` .

 ✔️ Considere nomear DLLs de acordo com o seguinte padrão:

 `<Company>.<Component>.dll`

 onde `<Component>` contém uma ou mais cláusulas separadas por ponto. Por exemplo:

 `Litware.Controls.dll`.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Confira também

- [Diretrizes de design de estrutura](index.md)
- [Diretrizes de nomenclatura](naming-guidelines.md)
