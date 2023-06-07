---
title: Segurança de mensagem no WCF
description: Saiba mais sobre o TransportWithMessageCredential, um tipo de segurança de mensagem do WCF que usa uma combinação de modos de segurança de transporte e mensagem.
ms.date: 03/30/2017
ms.assetid: a80efb59-591a-4a37-bb3c-8fffa6ca0b7d
ms.openlocfilehash: 315a12c73929bfe71340e42f122ae542d4fddc07
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245018"
---
# <a name="message-security-in-wcf"></a>Segurança de mensagem no WCF

O Windows Communication Foundation (WCF) tem dois modos principais para fornecer segurança ( `Transport` e `Message` ) e um terceiro modo ( `TransportWithMessageCredential` ) que combina os dois. Este tópico discute a segurança da mensagem e os motivos para usá-la.

## <a name="what-is-message-security"></a>O que é segurança de mensagem?

A segurança da mensagem usa a especificação WS-Security para proteger mensagens. A especificação WS-Security descreve aprimoramentos em mensagens SOAP para garantir a confidencialidade, a integridade e a autenticação no nível de mensagem SOAP (em vez do nível de transporte).

Em resumo, a segurança da mensagem difere da segurança de transporte encapsulando as credenciais de segurança e as declarações com cada mensagem junto com qualquer proteção de mensagem (assinatura ou criptografia). Aplicar a segurança diretamente à mensagem modificando seu conteúdo permite que a mensagem protegida seja autocontido em relação aos aspectos de segurança. Isso habilita alguns cenários que não são possíveis quando a segurança de transporte é usada.

## <a name="reasons-to-use-message-security"></a>Motivos para usar a segurança da mensagem

Na segurança em nível de mensagem, todas as informações de segurança são encapsuladas na mensagem. Proteger a mensagem com segurança em nível de mensagem em vez de segurança em nível de transporte tem as seguintes vantagens:

- Segurança de ponta a ponta. A segurança de transporte, como protocolo SSL (SSL) só protege mensagens quando a comunicação é ponto a ponto. Se a mensagem for roteada para um ou mais intermediários SOAP (por exemplo, um roteador) antes de alcançar o receptor final, a mensagem em si não será protegida quando um intermediário a ler da conexão. Além disso, as informações de autenticação de cliente estão disponíveis apenas para o primeiro intermediário e devem ser retransmitidas para o receptor final fora de banda, se necessário. Isso se aplica mesmo se a rota inteira usar a segurança SSL entre saltos individuais. Como a segurança da mensagem funciona diretamente com a mensagem e protege o XML nela, a segurança permanece com a mensagem, independentemente de quantos intermediários estão envolvidos antes de atingir o receptor final. Isso permite um cenário de segurança verdadeiro de ponta a ponta.

- Maior flexibilidade. Partes da mensagem, em vez de toda a mensagem, podem ser assinadas ou criptografadas. Isso significa que os intermediários podem exibir as partes da mensagem que são destinadas a eles. Se o remetente precisar fazer parte das informações na mensagem visível para os intermediários, mas desejar garantir que ela não seja violada, ela poderá apenas conectá-la, mas deixá-la descriptografada. Como a assinatura faz parte da mensagem, o receptor final pode verificar se as informações na mensagem foram recebidas intactas. Um cenário pode ter um serviço intermediário de SOAP que roteia a mensagem de acordo com o valor do cabeçalho de ação. Por padrão, o WCF não criptografa o valor da ação, mas o assina se a segurança da mensagem for usada. Portanto, essas informações estão disponíveis para todos os intermediários, mas ninguém pode alterá-la.

- Suporte para vários transportes. Você pode enviar mensagens seguras por vários transportes diferentes, como pipes nomeados e TCP, sem precisar contar com o protocolo de segurança. Com a segurança em nível de transporte, todas as informações de segurança são delimitadas a uma única conexão de transporte específica e não estão disponíveis no próprio conteúdo da mensagem. A segurança da mensagem torna a mensagem segura, independentemente de qual transporte você usa para transmitir a mensagem, e o contexto de segurança é diretamente inserido dentro da mensagem.

- Suporte para um amplo conjunto de credenciais e declarações. A segurança da mensagem é baseada na especificação WS-Security, que fornece uma estrutura extensível capaz de transmitir qualquer tipo de declaração dentro da mensagem SOAP. Diferentemente da segurança de transporte, o conjunto de mecanismos de autenticação, ou declarações, que você pode usar não é limitado pelos recursos de transporte. A segurança de mensagens do WCF inclui vários tipos de transmissão de solicitação e autenticação e pode ser estendida para dar suporte a tipos adicionais, conforme necessário. Por esses motivos, por exemplo, um cenário de credenciais federadas não é possível sem a segurança da mensagem. Para obter mais informações sobre cenários de Federação compatíveis com o WCF, consulte [Federação e tokens emitidos](federation-and-issued-tokens.md).

## <a name="how-message-and-transport-security-compare"></a>Como a segurança de mensagens e transporte é comparada

### <a name="pros-and-cons-of-transport-level-security"></a>Prós e contras da segurança em nível de transporte

A segurança de transporte tem as seguintes vantagens:

- Não exige que as partes de comunicação compreendam os conceitos de segurança em nível de XML. Isso pode melhorar a interoperabilidade, por exemplo, quando o HTTPS é usado para proteger a comunicação.

- Desempenho aprimorado em geral.

- Os aceleradores de hardware estão disponíveis.

- O streaming é possível.

 A segurança de transporte tem as seguintes desvantagens:

- Somente salto a salto.

- Conjunto de credenciais limitado e extensível.

- Dependente de transporte.

### <a name="disadvantages-of-message-level-security"></a>Desvantagens da segurança em nível de mensagem

A segurança da mensagem tem as seguintes desvantagens:

- Desempenho

- Não é possível usar o streaming de mensagens.

- Requer a implementação de mecanismos de segurança em nível XML e suporte para especificação WS-Security. Isso pode afetar a interoperabilidade.

## <a name="see-also"></a>Veja também

- [Protegendo serviços e clientes](securing-services-and-clients.md)
- [Segurança de transporte](transport-security.md)
- [Como utilizar credenciais de mensagem e segurança de transporte](how-to-use-transport-security-and-message-credentials.md)
- [Práticas e padrões da Microsoft, capítulo 3: Implementando o transporte e a segurança da camada de mensagens](https://docs.microsoft.com/previous-versions/msp-n-p/ff647370(v=pandp.10))
