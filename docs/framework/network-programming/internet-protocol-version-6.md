---
title: Protocolo IP versão 6
description: Saiba mais sobre o protocolo IPv6 e como ele difere do IPv4. .NET Framework aplicativos dão suporte ao IPv6, mas podem exigir configuração.
ms.date: 03/30/2017
helpviewer_keywords:
- IPv6, improvements
- IPv4
- IPv6
- Internet Protocol version 6, improvements
- Internet Protocol version 6
ms.assetid: e6fa8ebd-010a-4c48-a5ec-a5102c53c06f
ms.openlocfilehash: dd8b0b26e449fba7479d2678aff25ed49e721a90
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502386"
---
# <a name="internet-protocol-version-6"></a>Protocolo IP versão 6
O protocolo IP versão 6 (IPv6) é um novo pacote de protocolos padrão para a camada de rede da Internet. O IPv6 foi projetado para resolver muitos dos problemas da versão atual do pacote de protocolos IP (conhecido como IPv4) relacionados ao o esgotamento de endereços, a segurança, a configuração automática, a necessidade de extensibilidade e outros. O IPv6 expande os recursos da Internet para habilitar novos tipos de aplicativos, inclusive aplicativos móveis e de ponto a ponto. Estes são os principais problemas do protocolo IPv4 atual:  
  
- Rápido esgotamento do espaço de endereço.  
  
     Isso levou ao uso de conversores de endereço de rede (NATs) que mapeiam vários endereços particulares para um único endereço IP público. Os principais problemas criados por esse mecanismo são a sobrecarga de processamento e a falta de conectividade de ponta a ponta.  
  
- Falta de suporte a hierarquia.  
  
     Por causa de sua organização de classe predefinida inerente, IPv4 não tem um verdadeiro suporte hierárquico. É impossível estruturar os endereços IP de uma forma que realmente mapeie a topologia de rede. Essa falha de design crucial cria a necessidade de grandes tabelas de roteamento para entregar pacotes IPv4 em qualquer local na Internet.  
  
- Configuração de rede complexa.  
  
     Com o IPv4, os endereços devem ser atribuídos estaticamente ou usando um protocolo de configuração, como o DHCP. Em uma situação ideal, hosts não precisariam depender da administração de uma infraestrutura DHCP. Em vez disso, eles poderiam configurar a si mesmos com base no segmento de rede no qual estivessem localizados.  
  
- Falta de autenticação interna e de confidencialidade.  
  
     O IPv4 não requer suporte para nenhum outro mecanismo que fornece autenticação ou criptografia dos dados transmitidos. Isso muda com o IPv6. O protocolo IPsec é um requisito de suporte a IPv6.  
  
 Um novo pacote de protocolos deve atender aos seguintes requisitos básicos:  
  
- Roteamento em larga escala e endereçamento com pouca sobrecarga.  
  
- Configuração automática para situações de conexão diversas.  
  
- Autenticação interna e confidencialidade.  
  
 Para obter mais informações, consulte [Endereçamento IPv6](ipv6-addressing.md), [Roteamento IPv6](ipv6-routing.md), [Configuração automática de IPv6](ipv6-auto-configuration.md), [Habilitando e desabilitando o IPv6](enabling-and-disabling-ipv6.md) e [Como modificar o arquivo de configuração do computador para habilitar o suporte a IPv6](how-to-modify-the-computer-configuration-file-to-enable-ipv6-support.md).  
  
## <a name="references"></a>Referências  
 Veja a seguir documentos do RFC selecionados que podem ser encontrados no site da [IETF (Internet Engineering Task Force)](https://www.ietf.org/):  
  
- RFC 1287, voltado a arquitetura de Internet futura.  
  
- RFC 1454, comparação de propostas para a próxima versão do IP.  
  
- RFC 2373, arquitetura de endereçamento do IP versão 6.  
  
- RFC 2374, um formato de endereço unicast global agregável IPv6.  
  
 Você também pode encontrar informações relacionadas ao IPv6 em [Versão 6 do IP (IPv6)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379498%28v=ws.10%29).  
  
## <a name="see-also"></a>Confira também

- [Amostra de soquetes IPv6](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.0/ms180981%28v=vs.85%29)
- [Amostras de programação de rede](network-programming-samples.md)
- [Soquetes](sockets.md)
