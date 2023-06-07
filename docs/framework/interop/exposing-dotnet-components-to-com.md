---
title: Como expor componentes .NET ao COM
description: Expor componentes .NET ao COM. Qualificar tipos .NET para interoperação. Aplicar atributos de interoperabilidade. Empacotar um assembly para COM. Consumir um tipo gerenciado do COM.
ms.date: 03/30/2017
helpviewer_keywords:
- exposing .NET Framework components to COM
- interoperation with unmanaged code, exposing .NET Framework components
- COM interop, exposing COM components
ms.assetid: e42a65f7-1e61-411f-b09a-aca1bbce24c6
ms.openlocfilehash: 918c90f6741047f7d3cdf89a9b182700ecb2ed93
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617451"
---
# <a name="exposing-net-components-to-com"></a>Como expor componentes .NET ao COM

A escrita de um tipo .NET e o consumo desse tipo em um código não gerenciado são atividades distintas para desenvolvedores. Esta seção descreve várias dicas para escrever um código gerenciado que interopera com clientes COM:

- [Qualificando tipos .net para interoperação](../../standard/native-interop/qualify-net-types-for-interoperation.md).

     Todos os tipos gerenciados, métodos, propriedades, campos e eventos que você deseja expor ao COM devem ser públicos. Os tipos devem ter um construtor sem parâmetros público, que é o único construtor que pode ser invocado por meio do COM.

- [Aplicando atributos de interoperabilidade](../../standard/native-interop/apply-interop-attributes.md).

     Atributos personalizados no código gerenciado podem melhorar a interoperabilidade de um componente.

- [Empacotando um assembly para com](packaging-an-assembly-for-com.md).

     Os desenvolvedores do COM podem precisar que você resuma as etapas envolvidas na referência e implantação dos assemblies.

 Além disso, esta seção identifica as tarefas relacionadas ao consumo de um tipo gerenciado em um cliente COM.

## <a name="to-consume-a-managed-type-from-com"></a>Para consumir um tipo gerenciado por meio do COM

1. [Registrar assemblies com o COM](registering-assemblies-with-com.md).

     Os tipos em um assembly (e as bibliotecas de tipos) devem ser registrados em tempo de design. Se um instalador não registrar o assembly, instrua os desenvolvedores do COM a usar Regasm.exe.

2. [Referencie tipos .net de com](how-to-reference-net-types-from-com.md).

     Os desenvolvedores do COM podem referenciar tipos em um assembly usando as mesmas ferramentas e técnicas que usam hoje.

3. [Chamar um objeto .NET](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/8hw8h46b(v=vs.100)).

     Os desenvolvedores do COM podem chamar métodos no objeto .NET da mesma forma que chamam métodos em qualquer tipo não gerenciado. Por exemplo, a API **CoCreateInstance** do COM ativa objetos .NET.

4. [Implante um aplicativo para o acesso COM](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/c2850st8(v=vs.100)).

     Um assembly de nome forte pode ser instalado no cache de assembly global e exige uma assinatura de seu fornecedor. Os assemblies que não têm nome forte devem ser instalados no diretório do aplicativo do cliente.

## <a name="see-also"></a>Consulte também

- [Interoperação com código não gerenciado](index.md)
- [Amostra de interoperabilidade COM: Cliente COM e servidor .NET](com-interop-sample-com-client-and-net-server.md)
