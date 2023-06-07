---
title: 'Como: Registrar assemblies de interoperabilidade primários'
description: Registre assemblies de interoperabilidade primária usando a ferramenta de registro de assembly (Regasm.exe) e leia sobre outros problemas relacionados a assemblies de interoperabilidade.
ms.date: 03/30/2017
helpviewer_keywords:
- registering primary interop assemblies
- primary interop assemblies, registering
ms.assetid: 4b2fcf8a-429d-43ce-8334-e026040be8bb
ms.openlocfilehash: a15bda7b40f160b31028c62cf7c73bdedd9541fa
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622738"
---
# <a name="how-to-register-primary-interop-assemblies"></a>Como: Registrar assemblies de interoperabilidade primários

As classes podem ter o marshaling realizado somente pela interoperabilidade COM e sempre têm o marshaling realizado como interfaces. Em alguns casos, a interface usada para realizar marshaling da classe é conhecida como a interface de classe. Para obter informações sobre como substituir a interface de classe por uma interface de sua preferência, consulte [COM Callable Wrapper](../../standard/native-interop/com-callable-wrapper.md).

 Embora qualquer desenvolvedor que deseje usar tipos COM de um aplicativo do .NET Framework possa gerar um assembly de interoperabilidade, fazer isso cria um problema. Cada vez que um desenvolvedor importa e assina uma biblioteca de tipos COM, o desenvolvedor cria um conjunto de tipos exclusivos que são incompatíveis com aqueles importados e assinados por outro desenvolvedor. A solução para esse problema de incompatibilidade de tipo é que cada desenvolvedor obtenha o assembly de interoperabilidade primário assinado e fornecido pelo fornecedor.

 Se você pretende expor os tipos COM terceiros para outros aplicativos, use sempre o assembly de interoperabilidade primário fornecido pelo mesmo editor da biblioteca de tipos que esse assembly define. Além de fornecer compatibilidade garantida, os assemblies de interoperabilidade primários geralmente são personalizados pelo fornecedor para aprimorar a interoperabilidade.

 Mesmo se você não planeja expor os tipos COM terceiros, usar o assembly de interoperabilidade primário pode facilitar a tarefa de interoperar com componentes COM. No entanto, essa estratégia não fornece nenhum isolamento de alterações que um fornecedor possa fazer a tipos definidos em um assembly de interoperabilidade primário. Quando seu aplicativo requer um isolamento desse tipo, gere seu próprio assembly de interoperabilidade em vez de usar o assembly de interoperabilidade primário.

 Você deve registrar todos os assemblies de interoperabilidade primários adquiridos no computador de desenvolvimento para que você referenciá-los com o Visual Studio. O Visual Studio procura e usa um assembly de interoperabilidade primário na primeira vez que você referenciar um tipo de uma biblioteca de tipos COM. Se o Visual Studio não puder localizar o assembly de interoperabilidade primário associado à biblioteca de tipos, ele solicitará que você o adquira ou oferecerá a possibilidade de criar um assembly de interoperabilidade em vez disso. Da mesma forma, o [Importador de Biblioteca de Tipos (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md) também usa o Registro para localizar assemblies de interoperabilidade primários.

 Embora não seja necessário registrar assemblies de interoperabilidade primários a menos que você planeje usar o Visual Studio, o registro fornece duas vantagens:

- Um assembly de interoperabilidade primário registrado é claramente marcado na chave do Registro da biblioteca de tipos original. O registro é a melhor maneira de localizar um assembly de interoperabilidade primário em seu computador.

- Você pode evitar gerar acidentalmente e usar um novo assembly de interoperabilidade se, em algum momento no futuro, você usar o Visual Studio para fazer referência a um tipo para o qual você tem um assembly de interoperabilidade primário não registrado.

Use a [Ferramenta de Registro do Assembly (Regasm.exe)](../tools/regasm-exe-assembly-registration-tool.md) para registrar um assembly de interoperabilidade primário.

## <a name="to-register-a-primary-interop-assembly"></a>Para registrar um assembly de interoperabilidade primário

1. No prompt de comando, digite:

     **regasm** *nomedoassembly*

     Nesse comando, *nomedoassembly* é o nome do arquivo do assembly que é registrado. Regasm.exe adiciona uma entrada para o assembly de interoperabilidade primário sob a mesma chave do Registro da biblioteca de tipos original.

## <a name="example"></a>Exemplo
 O exemplo a seguir registra o assembly de interoperabilidade primário `CompanyA.UtilLib.dll`.

```console
regasm CompanyA.UtilLib.dll
```

## <a name="see-also"></a>Consulte também

- [Programando com assemblies de interoperabilidade primários](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/baxfadst(v=vs.100))
- [Localizando assemblies de interoperabilidade primários](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/y06sxw56(v=vs.100))
- [Redistribuindo assemblies de interoperabilidade primários](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/w0dt2w20(v=vs.100))
