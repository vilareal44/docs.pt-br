---
title: Tipos .NET qualificados para interoperação COM
description: Este artigo fornece diretrizes para ajudá-lo a expor tipos em um assembly .NET para aplicativos COM para interoperabilidade COM.
ms.date: 03/30/2017
helpviewer_keywords:
- exposing .NET Framework components to COM
- COM interop, qualifying .NET types
- qualifying .NET types for interoperation
- interoperation with unmanaged code, qualifying .NET types
- interoperation with unmanaged code, exposing .NET Framework components
- COM interop, exposing COM components
ms.assetid: 4b8afb52-fb8d-4e65-b47c-fd82956a3cdd
ms.openlocfilehash: 5e8d604c8152d37475bf93e3b5687f24cfebfa02
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84285957"
---
# <a name="qualifying-net-types-for-com-interoperation"></a>Tipos .NET qualificados para interoperação COM
Se você pretende expor os tipos em um assembly para aplicativos COM, considere os requisitos de interoperabilidade COM em tempo de design. Tipos gerenciados (classe, interface, estrutura e enumeração) se integram perfeitamente com tipos COM quando você obedece às seguintes diretrizes:  
  
- As classes devem implementar interfaces explicitamente.  
  
     Embora a interoperabilidade COM forneça um mecanismo para gerar automaticamente uma interface contendo todos os membros da classe e os membros da respectiva classe base, é muito melhor fornecer interfaces explícitas. A interface gerada automaticamente é chamada de interface de classe. Para obter diretrizes, consulte [introdução à interface de classe](com-callable-wrapper.md#introducing-the-class-interface).  
  
     Você pode usar Visual Basic, C# e C++ para incorporar as definições de interface no código, em vez de usar a linguagem IDL ou uma equivalente. Para obter detalhes da sintaxe, consulte a documentação da linguagem.  
  
- Os tipos gerenciados devem ser públicos.  
  
     Somente os tipos públicos em um assembly são registrados e exportados para a biblioteca de tipos. Como resultado, somente os tipos públicos são visíveis para COM.  
  
     Tipos gerenciados expõem recursos para outro código gerenciado que pode não ser exposto a COM. Por exemplo, campos de constantes, construtores com parâmetros e métodos estáticos não são expostos a clientes COM. Além disso, como o runtime realiza marshaling de dados dentro e fora de um tipo, os dados podem ser copiados ou transformados.  
  
- Propriedades, métodos, campos e eventos devem ser públicos.  
  
     Membros de tipos públicos também devem ser públicos para ser visíveis para COM. Você pode restringir a visibilidade de um assembly, um tipo público ou membros públicos de um tipo público aplicando o <xref:System.Runtime.InteropServices.ComVisibleAttribute>. Por padrão, todos os membros e tipos públicos são visíveis.  
  
- Os tipos devem ter um construtor sem parâmetros público para ser ativado no COM.  
  
     Tipos públicos gerenciados são visíveis para o COM. No entanto, sem um construtor sem parâmetros público (um construtor sem argumentos), clientes COM não podem criar o tipo. Clientes COM ainda podem usar o tipo se ele é ativado por outros meios.  
  
- Os tipos não podem ser abstratos.  
  
     Nem clientes COM, tampouco clientes .NET podem criar tipos abstratos.  
  
 Quando exportados para COM, a hierarquia de herança de um tipo gerenciado é nivelada. O controle de versão também difere entre ambientes gerenciados e não gerenciados. Os tipos expostos ao COM não têm as mesmas características de controle de versão de outros tipos gerenciados.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Runtime.InteropServices.ComVisibleAttribute>
- [Expondo componentes do .NET Framework para COM](../../framework/interop/exposing-dotnet-components-to-com.md)
- [Introdução à interface de classe](com-callable-wrapper.md#introducing-the-class-interface)
- [Aplicando atributos de interoperabilidade](apply-interop-attributes.md)
- [Como empacotar um assembly .NET Framework para o COM](../../framework/interop/packaging-an-assembly-for-com.md)
