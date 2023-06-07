---
title: Segurança de transporte de HTTP
ms.date: 03/30/2017
ms.assetid: d3439262-c58e-4d30-9f2b-a160170582bb
ms.openlocfilehash: 28d0ac164022f585f25b44b16c68994b592ef041
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84592717"
---
# <a name="http-transport-security"></a>Segurança de transporte de HTTP
Ao usar HTTP como transporte, a segurança é fornecida por uma implementação de protocolo SSL (SSL). O SSL é amplamente usado na Internet para autenticar um serviço para um cliente e, em seguida, para fornecer confidencialidade (criptografia) ao canal. Este tópico explica como o SSL funciona e como ele é implementado no Windows Communication Foundation (WCF).  
  
## <a name="basic-ssl"></a>SSL básico  
 O modo como o SSL funciona é mais bem explicado por meio de um cenário típico, nesse caso, um site do banco. O site permite que um cliente faça logon com um nome de usuário e senha. Depois de ser autenticado, o usuário pode executar transações, como exibir saldos de conta, pagar faturas e mover dinheiro de uma conta para outra.  
  
 Quando um usuário visita o site pela primeira vez, o mecanismo de SSL inicia uma série de negociações, chamada de *handshake*, com o cliente do usuário (neste caso, o Internet Explorer). O SSL autentica primeiro o site bancário para o cliente. Essa é uma etapa essencial, pois os clientes devem primeiro saber que estão se comunicando com o site real, e não uma falsificação que tenta atraí-los digitando seu nome de usuário e senha. O SSL faz essa autenticação usando um certificado SSL fornecido por uma autoridade confiável, como a VeriSign. A lógica vai assim: o VeriSign garante a identidade do site bancário. Como o Internet Explorer confia na VeriSign, o site é confiável. Se você quiser verificar com a VeriSign, poderá fazer isso também clicando no logotipo da VeriSign. Isso apresenta uma declaração de autenticidade com sua data de expiração e a quem ele é emitido (o site do banco).  
  
 Para iniciar uma sessão segura, o cliente envia o equivalente de uma "saudação" para o servidor junto com uma lista de algoritmos criptográficos que ele pode usar para assinar, gerar hashes e criptografar e descriptografar com o. Em resposta, o site envia de volta uma confirmação e sua escolha de um dos conjuntos de algoritmos. Durante esse handshake inicial, ambas as partes enviam e recebem nonces. Um *nonce* é um trecho de dados gerado aleatoriamente que é usado, em combinação com a chave pública do site, para criar um hash. Um *hash* é um novo número que é derivado dos dois números usando um algoritmo padrão, como SHA1. (O cliente e o site também trocam mensagens para concordar com o algoritmo de hash a ser usado.) O hash é exclusivo e é usado somente para a sessão entre o cliente e o site para criptografar e descriptografar mensagens. Tanto o cliente quanto o serviço têm o nonce original e a chave pública do certificado, de modo que ambos os lados possam gerar o mesmo hash. Portanto, o cliente valida o hash enviado pelo serviço (a) usando o algoritmo acordado para calcular o hash dos dados e (b) comparando-o com o hash enviado pelo serviço; Se as duas corresponderem, o cliente terá garantia de que o hash não foi violado. O cliente pode usar esse hash como uma chave para criptografar uma mensagem que contém ainda outro novo hash. O serviço pode descriptografar a mensagem usando o hash e recuperar esse hash segundo a final. As informações acumuladas (nonces, chave pública e outros dados) agora são conhecidas por ambos os lados, e um hash final (ou chave mestra) pode ser criado. Essa chave final é enviada criptografada usando o hash de próximo a último. A chave mestra é usada para criptografar e descriptografar mensagens para o restante da sessão. Como o cliente e o serviço usam a mesma chave, isso também é chamado de *chave de sessão*.  
  
 A chave de sessão também é caracterizada como uma chave simétrica ou um "segredo compartilhado". Ter uma chave simétrica é importante porque reduz a computação exigida por ambos os lados da transação. Se todas as mensagens exigiram uma nova troca de nonces e hashes, o desempenho seria deteriorado. Portanto, o objetivo final do SSL é usar uma chave simétrica que permita que as mensagens fluam livremente entre os dois lados com um nível maior de segurança e eficiência.  
  
 A descrição anterior é uma versão simplificada do que acontece, pois o protocolo pode variar de site para site. Também é possível que tanto o cliente quanto o site gerem nonces que são forma algorítmica combinados durante o handshake para adicionar mais complexidade e, portanto, proteção, ao processo de troca de dados.  
  
### <a name="certificates-and-public-key-infrastructure"></a>Certificados e infraestrutura de chave pública  
 Durante o handshake, o serviço também envia seu certificado SSL para o cliente. O certificado contém informações, como a data de expiração, a autoridade emissora e a Uniform Resource Identifier do site (URI). O cliente compara o URI com o URI que tinha sido contatado originalmente para garantir uma correspondência e também verifica a data e a autoridade emissora.  
  
 Cada certificado tem duas chaves, uma chave privada e uma chave pública, e as duas são conhecidas como um *par de chaves do Exchange*. Em resumo, a chave privada é conhecida somente pelo proprietário do certificado, enquanto a chave pública é legível a partir do certificado. Qualquer chave pode ser usada para criptografar ou descriptografar um resumo, hash ou outra chave, mas apenas como operações contrárias. Por exemplo, se o cliente criptografar com a chave pública, somente o site poderá descriptografar a mensagem usando a chave privada. Da mesma forma, se o site criptografar com a chave privada, o cliente poderá descriptografá-la com a chave pública. Isso fornece garantia ao cliente de que as mensagens estão sendo trocadas somente com o possessor da chave privada, pois somente as mensagens criptografadas com a chave privada podem ser descriptografadas com a chave pública. O site tem certeza de que ele está trocando mensagens com um cliente que criptografou usando a chave pública. No entanto, essa troca é segura apenas para um handshake inicial, e é por isso que muito mais é feito para criar a chave simétrica real. No entanto, todas as comunicações dependem do serviço que tem um certificado SSL válido.  
  
## <a name="implementing-ssl-with-wcf"></a>Implementando o SSL com o WCF  
 A segurança de transporte HTTP (ou SSL) é fornecida externamente ao WCF. Você pode implementar o SSL de uma das duas maneiras; o fator decisivo é como seu aplicativo é hospedado:  
  
- Se você estiver usando Serviços de Informações da Internet (IIS) como seu host do WCF, use a infraestrutura do IIS para configurar um serviço SSL.  
  
- Se você estiver criando um aplicativo WCF auto-hospedado, poderá associar um certificado SSL ao endereço usando a ferramenta HttpCfg. exe.  
  
### <a name="using-iis-for-transport-security"></a>Usando o IIS para segurança de transporte  
  
#### <a name="iis-70"></a>IIS 7.0  
 Para configurar o IIS 7,0 como um host seguro (usando SSL), consulte [Configurando protocolo SSL no IIS 7,0](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771438(v=ws.10)).  
  
Para configurar certificados para uso com o IIS 7,0, consulte [Configurando certificados de servidor no iis 7,0](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732230(v=ws.10)).  
  
#### <a name="iis-60"></a>IIS 6,0  
 Para configurar o IIS 6,0 como um host seguro (usando SSL), consulte [Configurando protocolo SSL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc736992(v=ws.10)).  
  
 Para configurar certificados para uso com o IIS 6,0, consulte [Certificates_IIS_SP1_Ops](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc757474(v=ws.10)).  
  
### <a name="using-httpcfg-for-ssl"></a>Usando HttpCfg para SSL  

 Se você estiver criando um aplicativo WCF auto-hospedado, use a ferramenta [Httpcfg. exe](/windows/win32/http/httpcfg-exe) .
  
 Para obter mais informações sobre como usar a ferramenta HttpCfg. exe para configurar uma porta com um certificado X. 509, consulte [como: configurar uma porta com um certificado SSL](how-to-configure-a-port-with-an-ssl-certificate.md).  
  
## <a name="see-also"></a>Confira também

- [Segurança de transporte](transport-security.md)
- [Segurança de mensagem](message-security-in-wcf.md)
