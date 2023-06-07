---
title: Protocolo de mensagem confiável versão 1.1
ms.date: 03/30/2017
ms.assetid: 0da47b82-f8eb-42da-8bfe-e56ce7ba6f59
ms.openlocfilehash: ad0a77842c10965749eab4e76bb123938e07e9d5
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144715"
---
# <a name="reliable-messaging-protocol-version-11"></a>Protocolo de mensagem confiável versão 1.1

Este tópico aborda os detalhes de implementação do Windows Communication Foundation (WCF) para o protocolo WS-ReliableMessaging de fevereiro de 2007 (versão 1,1) necessário para interoperação usando o transporte HTTP. O WCF segue a especificação WS-ReliableMessaging com as restrições e esclarecimentos explicados neste tópico. Observe que o protocolo WS-ReliableMessaging versão 1,1 é implementado a partir do .NET Framework 3,5.

O protocolo WS-ReliableMessaging de fevereiro de 2007 é implementado no WCF pelo <xref:System.ServiceModel.Channels.ReliableSessionBindingElement> .

Para sua conveniência, o tópico usa as seguintes funções:

- Iniciador: o cliente que inicia a criação da sequência de mensagens WS-Reliable.

- Respondente: o serviço que recebe as solicitações do iniciador.

 Este documento usa os prefixos e namespaces na tabela a seguir.

|Prefixo|Namespace|
|-|-|
|WSRM|`http://docs.oasis-open.org/ws-rx/wsrm/200702`|
|netrm|`http://schemas.microsoft.com/ws/2006/05/rm`|
|s|`http://www.w3.org/2003/05/soap-envelope`|
|WSA|`http://schemas.xmlsoap.org/ws/2005/08/addressing`|
|wsse|`http://docs.oasis-open.org/wss/2004/01/oasis-200401-wssecurity-secext-1.0.xsd`|
|wsrmp|`http://docs.oasis-open.org/ws-rx/wsrmp/200702`|
|netrmp|`http://schemas.microsoft.com/ws-rx/wsrmp/200702`|
|WSP|(WS-Policy 1,2 ou WS-Policy 1,5)|

## <a name="messaging"></a>Mensagens

### <a name="sequence-creation"></a>Criação de sequência

O WCF implementa `CreateSequence` e `CreateSequenceResponse` mensagens para estabelecer uma sequência de mensagens confiável. As seguintes restrições se aplicam:

- B1101: o iniciador do WCF usa a mesma referência de ponto de extremidade que a `CreateSequence` mensagem `ReplyTo` `AcksTo` e `Offer/Endpoint` .

- R1102: as `AcksTo` `ReplyTo` referências do e do ponto de `Offer/Endpoint` extremidade na `CreateSequence` mensagem devem ter valores de endereço com representações de cadeia de caracteres idênticas, de modo que correspondam ao octeto.

  - O respondente do WCF verifica se a parte do URI do `AcksTo` `ReplyTo` e as `Endpoint` referências do ponto de extremidade são idênticas antes de criar uma sequência.

- R1103: as `AcksTo` `ReplyTo` referências do e do ponto de `Offer/Endpoint` extremidade na `CreateSequence` mensagem devem ter o mesmo conjunto de parâmetros de referência.

  - O WCF não se aplica, mas pressupõe que os parâmetros de referência das `AcksTo` `ReplyTo` referências de ponto de extremidade e, em que `Offer/Endpoint` `CreateSequence` são idênticos, e usa parâmetros de referência da `ReplyTo` referência do ponto de extremidade para confirmações e mensagens de sequência de inverso.

- B1104: o iniciador do WCF não gera o `Expires` elemento opcional ou `Offer/Expires` na `CreateSequence` mensagem.

- B1105: ao acessar a `CreateSequence` mensagem, o respondente do WCF usa o `Expires` valor no `CreateSequence` elemento como o `Expires` valor no `CreateSequenceResponse` elemento. Caso contrário, o respondente do WCF lê e ignora `Expires` os `Offer/Expires` valores e.

- B1106: ao acessar a `CreateSequenceResponse` mensagem, o iniciador do WCF lê o `Expires` valor opcional, mas não o usa.

- B1107: o iniciador e respondente do WCF sempre geram o `IncompleteSequenceBehavior` elemento opcional nos `CreateSequence/Offer` `CreateSequenceResponse` elementos e.

- B1108: o WCF usa apenas `DiscardFollowingFirstGap` os `NoDiscard` valores e no `IncompleteSequenceBehavior` elemento.

  - O WS-ReliableMessaging utiliza o `Offer` mecanismo para estabelecer as duas sequências correlacionadas que formam uma sessão.

- B1109: se `CreateSequence` contiver um `Offer` elemento, o respondente do WCF unidirecional rejeita a sequência oferecida respondendo com um `CreateSequenceResponse` sem `Accept` elemento.

- B1110: se um Respondente de mensagens confiável rejeitar a sequência oferecida, o iniciador do WCF falhará na sequência estabelecida recentemente.

- B1111: se não `CreateSequence` contiver um `Offer` elemento, o respondente do WCF bidirecional rejeitará a sequência oferecida respondendo com uma `CreateSequenceRefused` falha.

- R1112: quando duas sequências de conversos são estabelecidas usando o `Offer` mecanismo, a `[address]` propriedade da `CreateSequenceResponse/Accept/AcksTo` referência do ponto de extremidade deve corresponder ao URI de destino do `CreateSequence` byte de mensagem para byte.

- R1113: quando duas seqüências de converso são estabelecidas usando o `Offer` mecanismo, todas as mensagens em ambas as sequências que fluem do iniciador para o respondente devem ser enviadas para a mesma referência de ponto de extremidade.

O WCF usa o WS-ReliableMessaging para estabelecer sessões confiáveis entre o iniciador e o respondente. A implementação do WS-ReliableMessaging do WCF fornece uma sessão confiável para os padrões unidirecional, resposta de solicitação e mensagens em full duplex. O mecanismo WS-ReliableMessaging `Offer` no `CreateSequence` e `CreateSequenceResponse` permite que você estabeleça duas sequências de contraverse correlacionadas e fornece um protocolo de sessão que é adequado para todos os pontos de extremidade da mensagem. Como o WCF fornece uma garantia de segurança para tal sessão, incluindo proteção de ponta a ponta para integridade da sessão, é prático garantir que as mensagens destinadas à mesma parte cheguem ao mesmo destino. Isso também permite "transportado" de confirmações de sequência em mensagens de aplicativo. Portanto, as restrições R1102, R1112 e R1113 se aplicam ao WCF.

Um exemplo de uma `CreateSequence` mensagem.

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CreateSequence</wsa:Action>
    <wsa:MessageID>urn:uuid:949cca61-8813-42ff-ab33-18d9e3fa82fa</wsa:MessageID>
    <wsa:ReplyTo>
        <wsa:Address>http://Business456.com/clientA</wsa:Address>
    </wsa:ReplyTo>
    <wsa:To s:mustUnderstand="1">http://BusinessABC.com/serviceA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CreateSequence>
      <wsrm:AcksTo>
        <wsa:Address>http://Business456.com/clientA</wsa:Address>
      </wsrm:AcksTo>
      <wsrm:Offer>
        <wsrm:Identifier>urn:uuid:066b4730-fc82-458a-a5c1-210be4fb4e4e</wsrm:Identifier>
        <wsrm:Endpoint>
          <wsa:Address>http://Business456.com/clientA</wsa:Address>
        </wsrm:Endpoint>
        <wsrm:IncompleteSequenceBehavior>DiscardFollowingFirstGap</wsrm:IncompleteSequenceBehavior>
      </wsrm:Offer>
    </wsrm:CreateSequence>
  </s:Body>
</s:Envelope>
```

Um exemplo de uma `CreateSequenceResponse` mensagem.

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CreateSequenceResponse</wsa:Action>
    <wsa:RelatesTo>urn:uuid:949cca61-8813-42ff-ab33-18d9e3fa82fa</wsa:RelatesTo>
    <wsa:To s:mustUnderstand="1">http://Business456.com/clientA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CreateSequenceResponse>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:IncompleteSequenceBehavior>DiscardFollowingFirstGap</wsrm:IncompleteSequenceBehavior>
      <wsrm:Accept>
        <wsrm:AcksTo>
          <wsa:Address>http://BusinessABC.com/serviceA</wsa:Address>
        </wsrm:AcksTo>
      </wsrm:Accept>
    </wsrm:CreateSequenceResponse>
  </s:Body>
</s:Envelope>
```

### <a name="closing-a-sequence"></a>Fechando uma sequência

O WCF usa `CloseSequence` as `CloseSequenceResponse` mensagens e para um desligamento iniciado pela fonte de mensagens confiáveis. O destino de mensagens confiáveis do WCF não inicia o desligamento e a fonte de mensagens confiáveis do WCF não dá suporte a um desligamento iniciado por destino de mensagens confiáveis. As seguintes restrições se aplicam:

- B1201: a fonte de mensagens confiáveis do WCF sempre envia uma `CloseSequence` mensagem para desligar a sequência.

- B1202: a origem da mensagem confiável aguarda a confirmação do intervalo completo de mensagens de sequência antes de enviar a `CloseSequence` mensagem.

- B1203: a fonte de mensagens confiáveis sempre inclui o `LastMsgNumber` elemento opcional, a menos que a sequência não contenha mensagens.

- R1204: o destino do sistema de mensagens confiável não deve iniciar o desligamento enviando uma `CloseSequence` mensagem.

- B1205: ao receber uma `CloseSequence` mensagem, a origem de mensagens confiáveis do WCF considera a sequência incompleta e envia uma falha.

 Um exemplo de uma `CloseSequence` mensagem.

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CloseSequence</wsa:Action>
    <wsa:MessageID>urn:uuid:6ce1d4c3-e1c1-474f-a8c9-4210e37f7877</wsa:MessageID>
    <wsa:ReplyTo>
      <wsa:Address>http://Business456.com/clientA</wsa:Address>
    </wsa:ReplyTo>
    <wsa:To s:mustUnderstand="1">http://BusinessABC.com/serviceA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CloseSequence>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:LastMsgNumber>30</wsrm:LastMsgNumber>
    </wsrm:CloseSequence>
  </s:Body>
</s:Envelope>
```

Mensagem de exemplo `CloseSequenceResponse` :

```xml
<s:Envelope>
  <s:Header>
    <wsrm:SequenceAcknowledgement>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:AcknowledgementRange Lower="1" Upper="30"></wsrm:AcknowledgementRange>
      <wsrm:Final></wsrm:Final>
      <netrm:BufferRemaining>8</netrm:BufferRemaining>
    </wsrm:SequenceAcknowledgement>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CloseSequenceResponse</wsa:Action>
    <wsa:RelatesTo>urn:uuid:6ce1d4c3-e1c1-474f-a8c9-4210e37f7877</wsa:RelatesTo>
    <wsa:To s:mustUnderstand="1">http://Business456.com/clientA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CloseSequenceResponse>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
    </wsrm:CloseSequenceResponse>
  </s:Body>
</s:Envelope>
```

### <a name="sequence-termination"></a>Terminação de sequência

O WCF usa principalmente o `TerminateSequence/TerminateSequenceResponse` handshake depois de concluir o `CloseSequence/CloseSequenceResponse` handshake. O destino de mensagens confiáveis do WCF não inicia o encerramento e a fonte de mensagens confiáveis não dá suporte a um encerramento iniciado pelo destino de mensagens confiáveis. As seguintes restrições se aplicam:

- B1301: o iniciador do WCF envia apenas a `TerminateSequence` mensagem após a conclusão bem-sucedida do `CloseSequence/CloseSequenceResponse` handshake.

- R1302: o WCF valida que o `LastMsgNumber` elemento é consistente entre todas `CloseSequence` as `TerminateSequence` mensagens e para uma determinada sequência. Isso significa que o `LastMsgNumber` não está presente em todas `CloseSequence` as `TerminateSequence` mensagens, ou está presente e é idêntico em todas `CloseSequence` as `TerminateSequence` mensagens e.

- B1303: ao receber uma `TerminateSequence` mensagem após o `CloseSequence/CloseSequenceResponse` Handshake, o destino do sistema de mensagens confiável responde com uma `TerminateSequenceResponse` mensagem. Como a fonte de mensagens confiáveis tem a confirmação final antes de enviar a `TerminateSequence` mensagem, o destino da mensagem confiável sabe sem dúvida que a sequência termina e recupera recursos imediatamente.

- B1304: ao receber uma `TerminateSequence` mensagem antes do `CloseSequence/CloseSequenceResponse` Handshake, o destino do sistema de mensagens confiável do WCF responde com uma `TerminateSequenceResponse` mensagem. Se o destino da mensagem confiável determinar que não há inconsistências na sequência, o destino da mensagem confiável aguardará um tempo especificado de destino do aplicativo antes de recuperar recursos, para permitir que o cliente receba a confirmação final. Caso contrário, o destino da mensagem confiável recupera recursos imediatamente e indica ao destino do aplicativo que a sequência termina com a dúvida ao gerar o `Faulted` evento.

Um exemplo de uma `TerminateSequence` mensagem.

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/TerminateSequence</wsa:Action>
    <wsa:MessageID>urn:uuid:3597a398-4f3c-40f4-9335-8f1515572fdf</wsa:MessageID>
    <wsa:ReplyTo>
      <wsa:Address>http://Business456.com/clientA</wsa:Address>
    </wsa:ReplyTo>
    <wsa:To s:mustUnderstand="1">http://BusinessABC.com/serviceA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:TerminateSequence>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:LastMsgNumber>30</wsrm:LastMsgNumber>
      </wsrm:TerminateSequence>
  </s:Body>
</s:Envelope>
```

Mensagem de exemplo `TerminateSequenceResponse` :

```xml
<s:Envelope>
  <s:Header>
    <wsrm:SequenceAcknowledgement>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:AcknowledgementRange Lower="1" Upper="30"></wsrm:AcknowledgementRange>
      <wsrm:Final></wsrm:Final>
      <netrm:BufferRemaining>8</netrm:BufferRemaining>
    </wsrm:SequenceAcknowledgement>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/TerminateSequenceResponse</wsa:Action>
    <wsa:RelatesTo>urn:uuid:3597a398-4f3c-40f4-9335-8f1515572fdf</wsa:RelatesTo>
    <wsa:To s:mustUnderstand="1">://Business456.com/clientA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:TerminateSequenceResponse>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
    </wsrm:TerminateSequenceResponse>
  </s:Body>
</s:Envelope>
```

### <a name="sequences"></a>Sequências

Veja a seguir uma lista de restrições que se aplicam a sequências:

- B1401: o WCF gera e acessa números de sequência que não `xs:long` são maiores do que o valor inclusivo máximo, 9223372036854775807.

Um exemplo de um `Sequence` cabeçalho.

```xml
<wsrm:Sequence s:mustUnderstand="1">
  <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
  <wsrm:MessageNumber>1</wsrm:MessageNumber>
</wsrm:Sequence>
```

### <a name="request-acknowledgement"></a>Confirmação de solicitação

O WCF usa o `AckRequested` cabeçalho como um mecanismo Keep-Alive.

Um exemplo de um `AckRequested` cabeçalho.

```xml
<wsrm:AckRequested>
  <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
</wsrm:AckRequested>
```

### <a name="sequenceacknowledgement"></a>SequenceAcknowledgement

O WCF usa um mecanismo de "transportado" para confirmações de sequência fornecidas em mensagens WS-Reliable. As seguintes restrições se aplicam:

- R1601: quando duas sequências de conversos são estabelecidas usando o `Offer` mecanismo, o `SequenceAcknowledgement` cabeçalho pode ser incluído em qualquer mensagem de aplicativo transmitida para o destinatário pretendido. O ponto de extremidade remoto deve ser capaz de acessar um `SequenceAcknowledgement` cabeçalho acumulado.

- B1602: o WCF não gera `SequenceAcknowledgement` cabeçalhos que contêm `Nack` elementos. O WCF valida que cada `Nack` elemento contém um número de sequência, mas, caso contrário, ignora o `Nack` elemento e o valor.

 Um exemplo de um `SequenceAcknowledgement` cabeçalho.

```xml
<wsrm:SequenceAcknowledgement>
  <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
  <wsrm:AcknowledgementRange Lower="1" Upper="1"></wsrm:AcknowledgementRange>
</wsrm:SequenceAcknowledgement>
```

### <a name="ws-reliablemessaging-faults"></a>Falhas de WS-ReliableMessaging

Veja a seguir uma lista de restrições que se aplicam à implementação do WCF de falhas WS-ReliableMessaging. As seguintes restrições se aplicam:

- B1701: o WCF não gera `MessageNumberRollover` falhas.

- B1702: sobre SOAP 1,2, quando o ponto de extremidade de serviço atinge seu limite de conexão e não pode processar novas conexões, o WCF gera um `CreateSequenceRefused` subcódigo de falha aninhado, `netrm:ConnectionLimitReached` , conforme mostrado no exemplo a seguir.

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action>http://docs.oasis-open.org/ws-rx/wsrm/200702/fault</wsa:Action>
  </s:Header>
  <s:Body>
    <s:Fault>
      <s:Code>
        <s:Value>s:Receiver</s:Value>
        <s:Subcode>
          <s:Value>wsrm:CreateSequenceRefused</s:Value>
          <s:Subcode>
            <s:Value>netrm:ConnectionLimitReached</s:Value>
          </s:Subcode>
        </s:Subcode>
      </s:Code>
      <s:Reason>
        <s:Text xml:lang="en">Server 'http://BusinessABC.com/serviceA' is too busy to process this request. Try again later.</s:Text>
      </s:Reason>
    </s:Fault>
  </s:Body>
</s:Envelope>
```

### <a name="ws-addressing-faults"></a>Falhas no WS-Addressing

Como o WS-ReliableMessaging usa o WS-Addressing, a implementação do WS-ReliableMessaging do WCF pode gerar e transmitir falhas de WS-Addressing. Esta seção aborda as falhas do WS-Addressing que o WCF gera e transmite explicitamente na camada WS-ReliableMessaging:

- B1801: o WCF gera e transmite a `Message Addressing Header Required` falha quando uma das seguintes opções é verdadeira:

  - `CreateSequence` `CloseSequence` `TerminateSequence` Falta um cabeçalho ou uma mensagem `MessageId` .

  - `CreateSequence` `CloseSequence` `TerminateSequence` Falta um cabeçalho ou uma mensagem `ReplyTo` .

  - Uma `CreateSequenceResponse` `CloseSequenceResponse` mensagem, ou `TerminateSequenceResponse` está sem um `RelatesTo` cabeçalho.

- B1802: o WCF gera e transmite a `Endpoint Unavailable` falha para indicar que não há nenhum ponto de extremidade ouvindo que possa processar a sequência com base no exame dos cabeçalhos de endereçamento na `CreateSequence` mensagem.

## <a name="protocol-composition"></a>Composição de protocolo

### <a name="composition-with-ws-addressing"></a>Composição com WS-Addressing

O WCF dá suporte a duas versões do WS-Addressing: WS-Addressing 2004/08 [WS-ADDR] e W3C WS-Addressing 1,0 Recommendations [WS-ADDR-CORE] e [WS-ADDR-SOAP].

Embora a especificação WS-ReliableMessaging mencione apenas o WS-Addressing 2004/08, ela não restringe a versão do WS-Addressing a ser usada. Veja a seguir uma lista de restrições que se aplicam ao WCF:

- R2101: o WS-Addressing 2004/08 e o WS-Addressing 1,0 podem ser usados com o sistema de mensagens WS-Reliable.

- R2102: uma única versão do WS-Addressing deve ser usada em uma determinada sequência WS-ReliableMessaging ou um par de sequências inversas correlacionadas usando o `Offer` mecanismo.

### <a name="composition-with-soap"></a>Composição com SOAP

O WCF dá suporte ao uso de SOAP 1,1 e SOAP 1,2 com o sistema de mensagens WS-Reliable.

### <a name="composition-with-ws-security-and-ws-secureconversation"></a>Composição com WS-Security e WS-SecureConversation

O WCF fornece proteção para sequências WS-ReliableMessaging usando transporte seguro (HTTPS), composição com WS-Security e composição com a conversa WS-Secure. O protocolo WS-ReliableMessaging 1,1, o WS-Security 1,1 e o WS-Secure Conversation protocolo 1,3 devem ser usados juntos. Veja a seguir uma lista de restrições que se aplicam ao WCF:

- R2301: para proteger a integridade de uma sequência WS-ReliableMessaging, além da integridade e da confidencialidade de mensagens individuais, o WCF requer que a conversa WS-Secure deva ser usada.

- R2302: AWS-a sessão de conversa segura deve ser estabelecida antes de estabelecer a (s) sequência (ões) WS-ReliableMessaging.

- R2303: se o tempo de vida da sequência WS-ReliableMessaging exceder o tempo de vida da sessão de conversa do WS-Secure, o `SecurityContextToken` estabelecido usando a conversa do WS-Secure deverá ser renovado usando a associação de renovação de conversa WS-Secure correspondente.

- B2304: a sequência WS-ReliableMessaging ou um par de sequências correlacionadas correlatas são sempre associados a uma única sessão WS-SecureConversation.

- R2305: quando composto com a conversa WS-Secure, o respondente do WCF requer que a `CreateSequence` mensagem contenha o `wsse:SecurityTokenReference` elemento e o `wsrm:UsesSequenceSTR` cabeçalho.

 Um exemplo de um `UsesSequenceSTR` cabeçalho.

```xml
<wsrm:UsesSequenceSTR></wsrm:UsesSequenceSTR>
```

### <a name="composition-with-ssltls-sessions"></a>Composição com sessões SSL/TLS

O WCF não dá suporte à composição com sessões SSL/TLS:

- B2401: o WCF não gera o `wsrm:UsesSequenceSSL` cabeçalho.

- R2402: um iniciador de mensagens confiável não deve enviar uma `CreateSequence` mensagem com um `wsrm:UsesSequenceSSL` cabeçalho para um respondente do WCF.

### <a name="composition-with-ws-policy"></a>Composição com WS-Policy

O WCF dá suporte a duas versões de WS-Policy: WS-Policy 1,2 e WS-Policy 1,5.

## <a name="ws-reliablemessaging-ws-policy-assertion"></a>Declaração de WS-Policy de WS-ReliableMessaging

O WCF usa o WS-ReliableMessaging asserção WS-Policy `wsrm:RMAssertion` para descrever os recursos de pontos de extremidade. Veja a seguir uma lista de restrições que se aplicam ao WCF:

- B3001: o WCF anexa `wsrmn:RMAssertion` a asserção de WS-Policy a `wsdl:binding` elementos. O WCF dá suporte tanto a anexos `wsdl:binding` quanto a `wsdl:port` elementos.

- B3002: o WCF nunca gera a `wsp:Optional` marca.

- B3003: ao acessar a `wsrmp:RMAssertion` declaração de WS-Policy, o WCF ignora a `wsp:Optional` marca e trata a política WS-RM como obrigatória.

- R3004: como o WCF não compõe as sessões SSL/TLS, o WCF não aceita a política que especifica `wsrmp:SequenceTransportSecurity` .

- B3005: o WCF sempre gera o `wsrmp:DeliveryAssurance` elemento.

- B3006: o WCF sempre especifica a `wsrmp:ExactlyOnce` garantia de entrega.

- B3007: o WCF gera e lê as seguintes propriedades da declaração WS-ReliableMessaging e fornece controle sobre elas no WCF `ReliableSessionBindingElement` :

  - `netrmp:InactivityTimeout`

  - `netrmp:AcknowledgementInterval`

  Um exemplo de um `RMAssertion` .

  ```xml
  <wsrmp:RMAssertion>
    <wsp:Policy>
      <wsrmp:SequenceSTR/>
      <wsrmp:DeliveryAssurance>
        <wsp:Policy>
          <wsrmp:ExactlyOnce/>
          <wsrmp:InOrder/>
        </wsp:Policy>
      </wsrmp:DeliveryAssurance>
    </wsp:Policy>
    <netrmp:InactivityTimeout Milliseconds="600000"/>
    <netrmp:AcknowledgementInterval Milliseconds="200"/>
  </wsrmp:RMAssertion>
  ```

## <a name="flow-control-ws-reliablemessaging-extension"></a>Extensão WS-ReliableMessaging de controle de fluxo

O WCF usa a extensibilidade WS-ReliableMessaging para fornecer um controle mais rígido adicional opcional sobre o fluxo de mensagens de sequência.

O controle de fluxo é habilitado definindo a <xref:System.ServiceModel.Channels.ReliableSessionBindingElement.FlowControlEnabled?displayProperty=nameWithType> propriedade como `true` . Veja a seguir uma lista de restrições que se aplicam ao WCF:

- B4001: quando o controle de fluxo de mensagens confiável está habilitado, o WCF gera um `netrm:BufferRemaining` elemento na extensibilidade do elemento do `SequenceAcknowledgement` cabeçalho, conforme mostrado no exemplo a seguir.

  ```xml
  <wsrm:SequenceAcknowledgement>
    <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
    <wsrm:AcknowledgementRange Upper="1" Lower="1"/>
    <netrm:BufferRemaining>8</netrm:BufferRemaining>
  </wsrm:SequenceAcknowledgement>
  ```

- B4002: mesmo quando o controle de fluxo de mensagens confiável está habilitado, o WCF não requer um `netrm:BufferRemaining` elemento no `SequenceAcknowledgement` cabeçalho.

- B4003: o destino do sistema de mensagens confiável do WCF usa `netrm:BufferRemaining` para indicar quantas novas mensagens podem ser armazenadas em buffer.

- B4004: quando o controle de fluxo de mensagens confiável está habilitado, a fonte de mensagens confiáveis do WCF usa o valor de `netrm:BufferRemaining` para limitar a transmissão de mensagens.

- B4005: o WCF gera `netrm:BufferRemaining` valores inteiros entre 0 e 4096, inclusive, e lê os valores inteiros entre 0 e o `xs:int` `maxInclusive` valor 214748364, inclusive.

## <a name="message-exchange-patterns"></a>Padrões de troca de mensagens

Esta seção descreve o comportamento do WCF quando o WS-ReliableMessaging é usado para diferentes padrões de troca de mensagens. Para cada padrão de troca de mensagens, os seguintes dois cenários de implantações são considerados:

- Iniciador não endereçável: o iniciador está protegido por um firewall; O respondente pode entregar mensagens ao iniciador somente em respostas HTTP.

- Iniciador endereçável: o iniciador e o respondente podem ser enviados solicitações HTTP; em outras palavras, duas conexões HTTP de converso podem ser estabelecidas.

### <a name="one-way-non-addressable-initiator"></a>Iniciador unidirecional e não endereçável

#### <a name="binding"></a>Associação

O WCF fornece um padrão de troca de mensagens unidirecional usando uma sequência em um canal HTTP. O WCF usa solicitações HTTP para transmitir todas as mensagens do iniciador para o respondente e respostas HTTP para transmitir todas as mensagens do Respondente para o iniciador.

#### <a name="createsequence-exchange"></a>Exchange CreateSequence

O iniciador do WCF transmite uma `CreateSequence` mensagem sem `Offer` elemento em uma solicitação HTTP e espera a `CreateSequenceResponse` mensagem na resposta http. O respondente do WCF cria uma sequência e transmite a `CreateSequenceResponse` mensagem sem `Accept` elemento na resposta http.

#### <a name="sequenceacknowledgement"></a>SequenceAcknowledgement

O iniciador do WCF processa confirmações na resposta de todas as mensagens, exceto nas `CreateSequence` mensagens de mensagem e de falha. O respondente do WCF sempre transmite uma confirmação autônoma na resposta HTTP para todas as sequências e `AckRequested` mensagens.

#### <a name="closesequence-exchange"></a>CloseSequence Exchange

O iniciador do WCF transmite uma `CloseSequence` mensagem em uma solicitação HTTP e espera a `CreateSequenceResponse` mensagem na resposta http. O respondente do WCF transmite a `CloseSequenceResponse` mensagem na resposta http.

#### <a name="terminatesequence-exchange"></a>Troca de TerminateSequence

O iniciador do WCF transmite uma `TerminateSequence` mensagem em uma solicitação HTTP e espera a `TerminateSequenceResponse` mensagem na resposta http. O respondente do WCF transmite a `TerminateSequenceResponse` mensagem na resposta http.

### <a name="one-way-addressable-initiator"></a>Iniciador endereçável unidirecional

#### <a name="binding"></a>Associação

O WCF fornece um padrão de troca de mensagens unidirecional usando uma sequência em um canal HTTP de entrada e um de saída. O WCF usa as solicitações HTTP para transmitir todas as mensagens. Todas as respostas HTTP têm um corpo vazio e um código de status HTTP 202.

#### <a name="createsequence-exchange"></a>Exchange CreateSequence

O iniciador do WCF transmite uma `CreateSequence` mensagem sem `Offer` elemento em uma solicitação HTTP. O respondente do WCF cria uma sequência e transmite a `CreateSequenceResponse` mensagem sem nenhum `Accept` elemento em uma solicitação HTTP.

### <a name="duplex-addressable-initiator"></a>Iniciador duplex, endereçável

#### <a name="binding"></a>Associação

O WCF fornece um padrão de troca de mensagens bidirecional, totalmente assíncrono, usando duas sequências em um canal HTTP de entrada e um de saída. Esse padrão de troca de mensagens pode ser misturado com o `Request/Reply` `Addressable` padrão de troca de mensagens do iniciador de forma limitada. O WCF usa solicitações HTTP para transmitir todas as mensagens. Todas as respostas HTTP têm um corpo vazio e um código de status HTTP 202.

#### <a name="createsequence-exchange"></a>Exchange CreateSequence

O iniciador do WCF transmite uma `CreateSequence` mensagem com um `Offer` elemento em uma solicitação HTTP. O respondente do WCF garante que o `CreateSequence` tenha um `Offer` elemento e, em seguida, cria uma sequência e transmite a `CreateSequenceResponse` mensagem com um `Accept` elemento.

#### <a name="sequence-lifetime"></a>Tempo de vida da sequência

O WCF trata as duas sequências como uma sessão totalmente duplex.

Ao gerar uma falha que apresenta uma sequência, o WCF espera que o ponto de extremidade remoto tenha uma falha nas duas sequências. Após a leitura de uma falha que falha em uma sequência, o WCF falha em ambas as sequências.

O WCF pode fechar sua sequência de saída e continuar a processar mensagens em sua sequência de entrada. Por outro lado, o WCF pode processar o fechamento da sequência de entrada e continuar a enviar mensagens em sua sequência de saída.

### <a name="request-reply-and-one-way-non-addressable-initiator"></a>Solicitação-resposta e iniciador unidirecional, não endereçável

#### <a name="binding"></a>Associação

O WCF fornece um padrão de troca de mensagens unidirecional e de solicitação-resposta usando duas sequências em um canal HTTP. O WCF usa solicitações HTTP para transmitir todas as mensagens do iniciador para o respondente e respostas HTTP para transmitir todas as mensagens do Respondente para o iniciador.

#### <a name="createsequence-exchange"></a>Exchange CreateSequence

O iniciador do WCF transmite uma `CreateSequence` mensagem com um `Offer` elemento em uma solicitação HTTP e espera a `CreateSequenceResponse` mensagem na resposta http. O respondente do WCF cria uma sequência e transmite a `CreateSequenceResponse` mensagem com um `Accept` elemento na resposta http.

#### <a name="one-way-message"></a>Mensagem unidirecional

Para concluir uma troca de mensagens unidirecional com êxito, o iniciador do WCF transmite uma mensagem de sequência de solicitação na solicitação HTTP e recebe uma `SequenceAcknowledgement` mensagem autônoma na resposta http. O `SequenceAcknowledgement` deve reconhecer a mensagem transmitida.

O respondente do WCF pode responder à solicitação com uma confirmação, uma falha ou uma resposta com um corpo vazio e um código de status HTTP 202.

#### <a name="two-way-messages"></a>Mensagens de duas vias

Para concluir com êxito um protocolo de troca de mensagens de duas vias, o iniciador do WCF transmite uma mensagem de sequência de solicitação na solicitação HTTP e recebe uma mensagem de sequência de resposta na resposta HTTP. A resposta deve receber uma `SequenceAcknowledgement` confirmação da mensagem de sequência de solicitação transmitida.

O respondente do WCF pode responder à solicitação com uma resposta do aplicativo, uma falha ou uma resposta com um corpo vazio e um código de status HTTP 202.

Devido à presença de mensagens unidirecionais e do tempo das respostas do aplicativo, o número de sequência da mensagem da sequência de solicitação e o número de sequência da mensagem de resposta não têm nenhuma correlação.

#### <a name="retrying-replies"></a>Repetindo respostas

O WCF conta com correlação de solicitação-resposta HTTP para correlação de protocolo de troca de mensagens bidirecional. Por isso, o iniciador do WCF não interrompe a repetição de uma mensagem de sequência de solicitação quando a mensagem da sequência de solicitação é confirmada, mas sim quando a resposta HTTP carrega uma `SequenceAcknowledgement` , uma resposta do aplicativo ou uma falha. O respondente do WCF tenta respostas novamente na resposta HTTP da solicitação à qual a resposta está correlacionada.

#### <a name="closesequence-exchange"></a>CloseSequence Exchange

Depois de receber todas as mensagens de sequência de resposta e confirmações de todas as mensagens de sequência de solicitação unidirecionais, o iniciador do WCF transmite uma `CloseSequence` mensagem para a sequência de solicitação em uma solicitação HTTP e espera o `CloseSequenceResponse` na resposta http.

Fechar a sequência de solicitação fecha implicitamente a sequência de resposta. Isso significa que o iniciador do WCF inclui o final da sequência de resposta `SequenceAcknowledgement` na `CloseSequence` mensagem e a sequência de resposta não tem uma `CloseSequence` troca.

O respondente do WCF garante que todas as respostas sejam confirmadas e transmita a `CloseSequenceResponse` mensagem na resposta http.

#### <a name="terminatesequence-exchange"></a>Troca de TerminateSequence

Depois de receber a `CloseSequenceResponse` mensagem, o iniciador do WCF transmite uma `TerminateSequence` mensagem para a sequência de solicitação em uma solicitação HTTP e espera o `TerminateSequenceResponse` na resposta http.

Assim como a `CloseSequence` troca, encerrar a sequência de solicitação encerra implicitamente a sequência de resposta. Isso significa que o iniciador do WCF inclui o final da sequência de resposta `SequenceAcknowledgement` na `TerminateSequence` mensagem e a sequência de resposta não tem uma `TerminateSequence` troca.

O respondente do WCF transmite a `TerminateSequenceResponse` mensagem na resposta http.

### <a name="requestreply-addressable-initiator"></a>Solicitação/resposta, iniciador endereçável

#### <a name="binding"></a>Associação

O WCF fornece um padrão de troca de mensagens de solicitação-resposta usando duas sequências em um canal HTTP de entrada e um de saída. Esse padrão de troca de mensagens pode ser misturado com o `Duplex, Addressable` padrão de troca de mensagens do iniciador de uma maneira limitada. O WCF usa as solicitações HTTP para transmitir todas as mensagens. Todas as respostas HTTP têm um corpo vazio e um código de status HTTP 202.

#### <a name="createsequence-exchange"></a>Exchange CreateSequence

O iniciador do WCF transmite uma `CreateSequence` mensagem com um `Offer` elemento em uma solicitação HTTP. O respondente do WCF garante que o `CreateSequence` tem um `Offer` elemento e, em seguida, cria uma sequência e transmite a `CreateSequenceResponse` mensagem com um `Accept` elemento.

#### <a name="requestreply-correlation"></a>Correlação de solicitação/resposta

O seguinte se aplica a todas as solicitações e respostas correlacionadas:

- O WCF garante que todas as mensagens de solicitação de aplicativo tenham uma `ReplyTo` referência de ponto de extremidade e um `MessageId` .

- O WCF aplica a referência de ponto de extremidade local como cada mensagem de solicitação de aplicativo `ReplyTo` . A referência do ponto de extremidade local é a `CreateSequence` mensagem do `ReplyTo` iniciador e a `CreateSequence` mensagem do `To` respondente.

- O WCF garante que as mensagens de solicitação de entrada tenham um `MessageId` e um `ReplyTo` .

- O WCF garante que o `ReplyTo` URI da referência de ponto de extremidade de todas as mensagens de solicitação de aplicativo corresponda à referência de ponto de extremidade local conforme definido anteriormente.

- O WCF garante que todas as respostas retenham os cabeçalhos corretos `RelatesTo` e as `To` regras de correlação de `wsa` solicitação/resposta a seguir.
