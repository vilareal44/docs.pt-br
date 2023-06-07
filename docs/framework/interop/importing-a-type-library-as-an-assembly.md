---
title: Importando uma biblioteca de tipos como um assembly
description: Importar uma biblioteca de tipos, que contém definições de tipo COM, como um assembly. Aprenda maneiras de criar metadados de uma biblioteca de tipos, resultando em um assembly de interoperabilidade.
ms.date: 03/30/2017
helpviewer_keywords:
- importing type library
- type metadata
- custom wrappers
- metadata
- exposing COM components to .NET Framework
- Type Library Importer
- interoperation with unmanaged code, importing type library
- interoperation with unmanaged code, exposing COM components
- type libraries
- TypeLibConverter class, importing type library as assembly
- COM interop, importing type library
- COM interop, exposing COM components
ms.assetid: d1898229-cd40-426e-a275-f3eb65fbc79f
ms.openlocfilehash: e5187e3c2ce533f25a38e93bc3715dd3e2e47c11
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622712"
---
# <a name="importing-a-type-library-as-an-assembly"></a>Importando uma biblioteca de tipos como um assembly

Definições de tipo COM geralmente residem em uma biblioteca de tipos. Por outro lado, os compiladores em conformidade com CLS produzem metadados de tipo em um assembly. As duas fontes de informações de tipo são muito diferentes. Este tópico descreve técnicas para a geração de metadados de uma biblioteca de tipos. O assembly resultante é chamado de um assembly de interoperabilidade e as informações de tipo contidas nele permitem que aplicativos do .NET Framework usem tipos COM.

Há duas maneiras de disponibilizar essas informações de tipo para seu aplicativo:

- Usando assemblies de interoperabilidade somente de tempo de design: começando com o .NET Framework 4, você pode instruir o compilador a inserir informações de tipo do assembly de interoperabilidade em seu executável. O compilador insere apenas as informações de tipo usadas pelo aplicativo. Não é necessário implantar o assembly de interoperabilidade com o aplicativo. Essa é a técnica recomendada.

- Implantando assemblies de interoperabilidade: é possível criar uma referência padrão ao assembly de interoperabilidade. Nesse caso, o assembly de interoperabilidade deve ser implantado com o aplicativo. Se você usar essa técnica e não estiver usando um componente COM particular, sempre referencie o PIA (assembly de interoperabilidade primário) publicado pelo autor do componente COM que você pretende incorporar no código gerenciado. Para obter mais informações sobre como produzir e usar assemblies de interoperabilidade primários, consulte [Assemblies de interoperabilidade primários](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/aax7sdch(v=vs.100)).

Quando você usar assemblies de interoperabilidade somente em tempo de design, você pode inserir informações de tipo do assembly de interoperabilidade primário publicado pelo autor do componente COM. No entanto, não é necessário implantar o assembly de interoperabilidade primário com o aplicativo.

Usar assemblies de interoperabilidade somente em tempo de design reduz o tamanho do seu aplicativo, porque a maioria dos aplicativos não usam todos os recursos de um componente COM. O compilador é muito eficiente quando ele insere informações de tipo. Se seu aplicativo usa apenas alguns dos métodos em uma interface COM, o compilador não insere os métodos não utilizados. Quando um aplicativo que inseriu informações de tipo interage com outro aplicativo desse tipo ou interage com um aplicativo que usa um assembly de interoperabilidade primário, o Common Language Runtime usa regras de equivalência de tipo para determinar se dois tipos com o mesmo nome representam o mesmo tipo COM. Você não precisa conhecer essas regras para usar objetos COM. No entanto, se você estiver interessado nas regras, consulte [Equivalência de tipos e tipos de interoperabilidade inseridos](type-equivalence-and-embedded-interop-types.md).

## <a name="generating-metadata"></a>Geração de metadados

Bibliotecas de tipos COM podem ser arquivos autônomos que têm uma extensão .tlb, tais como Loanlib.tlb. Algumas bibliotecas de tipo são inseridas na seção de recursos de um arquivo .dll ou .exe. Outras fontes de informações da biblioteca de tipos são arquivos .olb e .ocx.

Após você localizar a biblioteca de tipos que contém a implementação do seu tipo COM de destino, você tem as seguintes opções para a geração de um assembly de interoperabilidade contendo os metadados de tipo:

- Visual Studio

  O Visual Studio converte automaticamente os tipos COM em uma biblioteca de tipos para metadados em um assembly. Para obter instruções, consulte [como: adicionar referências a bibliotecas de tipos](how-to-add-references-to-type-libraries.md).

- [Importador de Biblioteca de Tipos (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md)

  O importador da biblioteca de tipos fornece opções de linha de comando para ajustar os metadados no arquivo de interoperabilidade resultante, importa tipos de uma biblioteca de tipos existente e gera um assembly de interoperabilidade e um namespace. Para obter instruções, consulte [Como gerar assemblies de interoperabilidade com base em bibliotecas de tipos](how-to-generate-interop-assemblies-from-type-libraries.md).

- Classe <xref:System.Runtime.InteropServices.TypeLibConverter?displayProperty=nameWithType>

  Essa classe fornece métodos para converter coclasses e interfaces em uma biblioteca de tipos para metadados em um assembly. Ela produz a mesma saída de metadados que Tlbimp.exe. No entanto, ao contrário de Tlbimp.exe, a classe <xref:System.Runtime.InteropServices.TypeLibConverter> pode converter uma biblioteca de tipos na memória em metadados.

- Wrappers personalizados

  Quando uma biblioteca de tipos não está disponível ou é incorreta, uma opção é criar uma definição duplicada da classe ou interface em código-fonte gerenciado. Em seguida, você compilar o código-fonte com um compilador que tem como alvo o runtime para produzir os metadados em um assembly.

  Para definir tipos COM manualmente, você deve ter acesso aos seguintes itens:

  - Descrições precisas das coclasses e interfaces que estão sendo definidas.

  - Um compilador, assim como o compilador C#, que pode gerar as definições de classe do .NET Framework apropriadas.

  - Conhecimento das regras de conversão de biblioteca de tipos para assembly.

  Escrever um wrapper personalizado é uma técnica avançada. Para obter informações adicionais sobre como gerar um wrapper personalizado, consulte [Personalizando wrappers padrão](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/h7hx9abd(v=vs.100)).

 Para obter mais informações sobre o processo de importação de interoperabilidade COM, consulte [Resumo da conversão de bibliotecas de tipos em assemblies](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100)).

## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.TypeLibConverter>
- [Expondo componentes do COM para o .NET Framework](exposing-com-components.md)
- [Resumo da conversão de bibliotecas de tipos em assemblies](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))
- [Tlbimp.exe (tipo de importador de biblioteca de tipos)](../tools/tlbimp-exe-type-library-importer.md)
- [Personalizando wrappers padrão](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/h7hx9abd(v=vs.100))
- [Usando tipos COM no código gerenciado](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100))
- [Compilando um projeto de interoperabilidade](compiling-an-interop-project.md)
- [Implantando um aplicativo de interoperabilidade](deploying-an-interop-application.md)
- [Como: Adicionar referências a bibliotecas de tipos](how-to-add-references-to-type-libraries.md)
- [Como: Gerar assemblies de interoperabilidade com base em bibliotecas de tipos](how-to-generate-interop-assemblies-from-type-libraries.md)
