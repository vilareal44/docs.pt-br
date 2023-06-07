---
title: Endereçamento IPv6
description: Saiba mais sobre o IPv6 (Internet Protocol versão 6), endereços, incluindo representação de texto e tipos de endereço.
ms.date: 03/30/2017
helpviewer_keywords:
- Internet Protocol version 6, addresses in
- Neighbor Discovery
- IPv6, enabling
- IPv6, mobility and
- Internet Protocol version 6, anycast addresses
- IPv6, Neighbor Discovery
- Internet Protocol version 6, auto-configuring
- Internet Protocol version 6, enabling
- IPv6, unicast addresses
- IPv6, auto-configuring
- Internet Protocol version 6, routing
- IPv6, RFC documents
- IPv6, routing
- Internet Protocol version 6, disabling
- Internet Protocol version 6, unicast addresses
- IPv6, multicast addresses
- Internet Protocol version 6, mobility and
- Internet Protocol version 6, RFC documents
- Internet Protocol version 6, Neighbor Discovery
- IPv6, anycast addresses
- Internet Protocol version 6, multicast addresses
- IPv6, addresses in
- IPv6, disabling
ms.assetid: 20a104ae-1649-4649-a005-531a5cf74c93
ms.openlocfilehash: fbf68cb5f40450c2f9ecf4900801ee55e326fcb4
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502334"
---
# <a name="ipv6-addressing"></a>Endereçamento IPv6

No protocolo IP versão 6 (IPv6), os endereços têm tamanho de 128 bits. Um motivo para um espaço de endereço tão grande é subdividir os endereços disponíveis em uma hierarquia de domínios de roteamento que reflitam a topologia da Internet. Outro motivo é mapear os endereços de adaptadores de rede (ou interfaces) que conectam dispositivos à rede. O IPv6 tem uma capacidade inerente de resolver endereços no nível mais baixo deles, que é o nível de adaptador de rede e também tem capacidades de configuração automática.

## <a name="text-representation"></a>Representação de texto

A seguir estão as três formas convencionais usadas para representar os endereços IPv6 como cadeias de caracteres de texto:

- **Forma hexadecimal com dois-pontos**. Esta é a forma preferencial: n:n:n:n:n:n:n:n. Cada n representa o valor hexadecimal de um dos oito elementos de 16 bits do endereço. Por exemplo: `3FFE:FFFF:7654:FEDA:1245:BA98:3210:4562`.

- **Forma compactada**. Devido ao tamanho de endereço, é comum ter endereços que contêm uma cadeia de caracteres longa de zeros. Para simplificar a gravar esses endereços, use o formato compactado, em que uma única sequência contígua de blocos de 0 é representada por um símbolo de dois-pontos duplos (::). Este símbolo pode aparecer apenas uma vez em um endereço. Por exemplo, o endereço multicast `FFED:0:0:0:0:BA98:3210:4562` torna-se `FFED::BA98:3210:4562` no formato compactado. O endereço unicast `3FFE:FFFF:0:0:8:800:20C4:0` em formato compactado é `3FFE:FFFF::8:800:20C4:0`. O endereço de loopback `0:0:0:0:0:0:0:1` em formato compactado é `::`1. O endereço não especificado `0:0:0:0:0:0:0:0` em formato compactado é `::`.

- **Formato misto**. Esse formato combina os endereços IPv4 e IPv6. Nesse caso, o formato de endereço é n:n:n:n:n:n:d.d.d.d, em que cada n representa os valores hexadecimais dos seis elementos do endereço de 16 bits superiores de IPv6 e cada d representa o valor decimal de um endereço IPv4.

## <a name="address-types"></a>Tipos de endereço

Os bits à esquerda do endereço definem o tipo específico de endereço IPv6. O campo de comprimento variável que contém esses bits à esquerda é chamado de um FP (prefixo de formato).

Um endereço unicast IPv6 é dividido em duas partes. A primeira parte contém o prefixo de endereço e a segunda parte contém o identificador de interface. Uma maneira concisa de expressar uma combinação de endereço IPv6/prefixo é a seguinte: endereço ipv6/comprimento do prefixo.

A seguir, um exemplo de um endereço com um prefixo de 64 bits.

`3FFE:FFFF:0:CD30:0:0:0:0/64`.

O prefixo neste exemplo é `3FFE:FFFF:0:CD30`. O endereço também pode ser gravado em um formato compactado, como `3FFE:FFFF:0:CD30::/64`.

O IPv6 define os seguintes tipos de endereço:

- **Endereço unicast**. Um identificador para uma única interface. Um pacote enviado para esse endereço é entregue para a interface identificada. Os endereços unicast são diferenciados dos endereços multicast pelo valor do octeto superior. O octeto superior dos endereços multicast tem o valor hexadecimal FF. Qualquer outro valor para esse octeto identifica um endereço unicast. Estes são os diferentes tipos de endereço unicast:

  - **Endereços de link local**. Esses endereços são usados em um único link e têm o seguinte formato: FE80::*InterfaceID*. Endereços de conexões locais são usados entre os nós em um link para a configuração automática de endereços, descoberta de vizinhos ou ainda quando nenhum dos roteadores está presente. Um endereço de link local é usado principalmente na inicialização e quando o sistema ainda não adquiriu endereços de escopo mais amplo.

  - **Endereços de site local**. Esses endereços são usados em um único site e têm o seguinte formato: FEC0::*SubnetID*:*InterfaceID*. Os endereços de sites locais são usados para endereçamento dentro de um site sem a necessidade de um prefixo global.

  - **Endereços unicast IPv6 globais**. Esses endereços podem ser usados pela Internet e têm o seguinte formato: 010(FP, 3 bits) ID de TLA (13 bits) Reserved (8 bits) ID de NLA (24 bits) ID de SLA (16 bits) *InterfaceID* (64 bits).

- **Endereço multicast**. Um identificador para um conjunto de interfaces (normalmente pertencentes a nós diferentes). Um pacote enviado para esse endereço será enviado para todas as interfaces identificadas pelo endereço. Os tipos de endereço multicast substituem os endereços difundidos por IPv4.

- **Endereço anycast**. Um identificador para um conjunto de interfaces (normalmente pertencentes a nós diferentes). Um pacote enviado para esse endereço será enviado para apenas uma interface identificada pelo endereço. Essa é a interface mais próxima conforme identificado pela métrica de roteamento. Endereços anycast são obtidos do espaço de endereço unicast e não é possível diferenciá-los sintaticamente. A interface endereçada faz a distinção entre endereços unicast e anycast como uma função de sua configuração.

Em geral, um nó sempre tem um endereço de link local. Ele pode ter um endereço de site local e um ou mais endereços globais.

## <a name="see-also"></a>Confira também

- [Protocolo IP versão 6](internet-protocol-version-6.md)
- [Soquetes](sockets.md)
