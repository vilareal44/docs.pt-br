---
title: Utilizando contratos de mensagem
description: Saiba como usar os atributos de contrato de mensagem para criar um contrato de mensagem especificando a estrutura de uma mensagem SOAP em WFC.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- message contracts [WCF]
ms.assetid: 1e19c64a-ae84-4c2f-9155-91c54a77c249
ms.openlocfilehash: 0a75298b50df74ddf15904af43a0eb62c5ba8496
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244706"
---
# <a name="using-message-contracts"></a>Utilizando contratos de mensagem
Normalmente, ao criar aplicativos Windows Communication Foundation (WCF), os desenvolvedores pagam atentamente com as estruturas de dados e problemas de serialização e não precisam se preocupar com a estrutura das mensagens nas quais os dados são transmitidos. Para esses aplicativos, a criação de contratos de dados para os parâmetros ou valores de retorno é simples. (Para obter mais informações, consulte [especificando transferência de dados em contratos de serviço](specifying-data-transfer-in-service-contracts.md).)  
  
 No entanto, às vezes, o controle total sobre a estrutura de uma mensagem SOAP é tão importante quanto controlar seu conteúdo. Isso é especialmente verdadeiro quando a interoperabilidade é importante ou para controlar especificamente problemas de segurança no nível da mensagem ou da parte da mensagem. Nesses casos, você pode criar um *contrato de mensagem* que permite especificar a estrutura da mensagem SOAP precisa necessária.  
  
 Este tópico discute como usar os vários atributos de contrato de mensagem para criar um contrato de mensagem específico para a operação.  
  
## <a name="using-message-contracts-in-operations"></a>Usando contratos de mensagem em operações  
 O WCF dá suporte a operações modeladas no *estilo de RPC (chamada de procedimento remoto)* ou no *estilo de mensagens*. Em uma operação em estilo RPC, você pode usar qualquer tipo serializável e ter acesso aos recursos que estão disponíveis para chamadas locais, como vários parâmetros e `ref` `out` parâmetros. Nesse estilo, a forma de serialização escolhida controla a estrutura dos dados nas mensagens subjacentes e o tempo de execução do WCF cria as mensagens para dar suporte à operação. Isso permite que os desenvolvedores que não estão familiarizados com mensagens SOAP e SOAP criem e usem aplicativos de serviço de forma rápida e fácil.  
  
 O exemplo de código a seguir mostra uma operação de serviço modelada no estilo RPC.  
  
```csharp  
[OperationContract]  
public BankingTransactionResponse PostBankingTransaction(BankingTransaction bt);  
```  
  
 Normalmente, um contrato de dados é suficiente para definir o esquema para as mensagens. Por exemplo, no exemplo anterior, é suficiente para a maioria dos aplicativos se `BankingTransaction` e `BankingTransactionResponse` tiver contratos de dados para definir o conteúdo das mensagens SOAP subjacentes. Para obter mais informações sobre contratos de dados, consulte [usando contratos de dados](using-data-contracts.md).  
  
 No entanto, ocasionalmente, é necessário controlar precisamente como a estrutura da mensagem SOAP é transmitida pela rede. O cenário mais comum para isso é inserir cabeçalhos SOAP personalizados. Outro cenário comum é definir as propriedades de segurança para os cabeçalhos e o corpo da mensagem, ou seja, decidir se esses elementos são assinados digitalmente e criptografados. Por fim, algumas pilhas SOAP de terceiros exigem que as mensagens estejam em um formato específico. As operações de estilo de mensagens fornecem esse controle.  
  
 Uma operação no estilo de mensagens tem no máximo um parâmetro e um valor de retorno em que ambos os tipos são tipos de mensagem; ou seja, eles serializam diretamente em uma estrutura de mensagem SOAP especificada. Isso pode ser qualquer tipo marcado com o <xref:System.ServiceModel.MessageContractAttribute> ou o <xref:System.ServiceModel.Channels.Message> tipo. O exemplo de código a seguir mostra uma operação semelhante ao estilo de RCP anterior, mas que usa o estilo de mensagens.  
  
 Por exemplo, se `BankingTransaction` e `BankingTransactionResponse` forem os dois tipos que são contratos de mensagem, o código nas operações a seguir será válido.  
  
```csharp  
[OperationContract]  
BankingTransactionResponse Process(BankingTransaction bt);  
[OperationContract]  
void Store(BankingTransaction bt);  
[OperationContract]  
BankingTransactionResponse GetResponse();  
```  
  
 No entanto, o código a seguir é inválido.  
  
```csharp  
[OperationContract]  
bool Validate(BankingTransaction bt);  
// Invalid, the return type is not a message contract.  
[OperationContract]  
void Reconcile(BankingTransaction bt1, BankingTransaction bt2);  
// Invalid, there is more than one parameter.  
```  
  
 Uma exceção é lançada para qualquer operação que envolva um tipo de contrato de mensagem e que não siga um dos padrões válidos. É claro que as operações que não envolvem tipos de contrato de mensagem não estão sujeitas a essas restrições.  
  
 Se um tipo tiver um contrato de mensagem e um contrato de dados, somente seu contrato de mensagem será considerado quando o tipo for usado em uma operação.  
  
## <a name="defining-message-contracts"></a>Definindo contratos de mensagem  
 Para definir um contrato de mensagem para um tipo (ou seja, para definir o mapeamento entre o tipo e um envelope SOAP), aplique o <xref:System.ServiceModel.MessageContractAttribute> ao tipo. Em seguida, aplique o <xref:System.ServiceModel.MessageHeaderAttribute> aos membros do tipo que você deseja transformar em cabeçalhos SOAP e aplique o <xref:System.ServiceModel.MessageBodyMemberAttribute> aos membros que você deseja fazer em partes do corpo SOAP da mensagem.  
  
 O código a seguir fornece um exemplo de como usar um contrato de mensagem.  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public Operation operation;  
  [MessageHeader] public DateTime transactionDate;  
  [MessageBodyMember] private Account sourceAccount;  
  [MessageBodyMember] private Account targetAccount;  
  [MessageBodyMember] public int amount;  
}  
```  
  
 Ao usar esse tipo como um parâmetro de operação, o seguinte envelope SOAP é gerado:  
  
```xml  
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">  
  <s:Header>  
    <h:operation xmlns:h="http://tempuri.org/" xmlns="http://tempuri.org/">Deposit</h:operation>  
    <h:transactionDate xmlns:h="http://tempuri.org/" xmlns="http://tempuri.org/">2012-02-16T16:10:00</h:transactionDate>  
  </s:Header>  
  <s:Body xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <BankingTransaction xmlns="http://tempuri.org/">  
      <amount>0</amount>  
      <sourceAccount xsi:nil="true"/>  
      <targetAccount xsi:nil="true"/>  
    </BankingTransaction>  
  </s:Body>  
</s:Envelope>  
```  
  
 Observe que `operation` e `transactionDate` aparecem como cabeçalhos SOAP e o corpo SOAP consiste em um elemento wrapper `BankingTransaction` contendo `sourceAccount` , `targetAccount` e `amount` .  
  
 Você pode aplicar o <xref:System.ServiceModel.MessageHeaderAttribute> e <xref:System.ServiceModel.MessageBodyMemberAttribute> a todos os campos, propriedades e eventos, independentemente de eles serem públicos, privados, protegidos ou internos.  
  
 O <xref:System.ServiceModel.MessageContractAttribute> permite que você especifique os atributos WrapperName e WrapperNamespace que controlam o nome do elemento wrapper no corpo da mensagem SOAP. Por padrão, o nome do tipo de contrato de mensagem é usado para o wrapper e o namespace no qual o contrato de mensagem é definido `http://tempuri.org/` é usado como o namespace padrão.  
  
> [!NOTE]
> <xref:System.Runtime.Serialization.KnownTypeAttribute>os atributos são ignorados em contratos de mensagem. Se <xref:System.Runtime.Serialization.KnownTypeAttribute> for necessário, coloque-o na operação que está usando o contrato de mensagem em questão.  
  
## <a name="controlling-header-and-body-part-names-and-namespaces"></a>Controlando nomes e namespaces de partes de cabeçalho e corpo  
 Na representação SOAP de um contrato de mensagem, cada parte de cabeçalho e corpo é mapeada para um elemento XML que tem um nome e um namespace.  
  
 Por padrão, o namespace é o mesmo que o namespace do contrato de serviço em que a mensagem está participando e o nome é determinado pelo nome do membro ao qual os <xref:System.ServiceModel.MessageHeaderAttribute> atributos ou <xref:System.ServiceModel.MessageBodyMemberAttribute> são aplicados.  
  
 Você pode alterar esses padrões manipulando o <xref:System.ServiceModel.MessageContractMemberAttribute.Name%2A?displayProperty=nameWithType> e <xref:System.ServiceModel.MessageContractMemberAttribute.Namespace%2A?displayProperty=nameWithType> (na classe pai dos <xref:System.ServiceModel.MessageHeaderAttribute> <xref:System.ServiceModel.MessageBodyMemberAttribute> atributos e).  
  
 Considere a classe no exemplo de código a seguir.  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public Operation operation;  
  [MessageHeader(Namespace="http://schemas.contoso.com/auditing/2005")] public bool IsAudited;  
  [MessageBodyMember(Name="transactionData")] public BankingTransactionData theData;  
}  
```  
  
 Neste exemplo, o `IsAudited` cabeçalho está no namespace especificado no código e a parte do corpo que representa o `theData` membro é representada por um elemento XML com o nome `transactionData` . O seguinte mostra o XML gerado para este contrato de mensagem.  
  
```xml  
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">  
  <s:Header>  
    <h:IsAudited xmlns:h="http://schemas.contoso.com/auditing/2005" xmlns="http://schemas.contoso.com/auditing/2005">false</h:IsAudited>  
    <h:operation xmlns:h="http://tempuri.org/" xmlns="http://tempuri.org/">Deposit</h:operation>  
  </s:Header>  
  <s:Body xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <AuditedBankingTransaction xmlns="http://tempuri.org/">  
      <transactionData/>  
    </AuditedBankingTransaction>  
  </s:Body>  
</s:Envelope>  
```  
  
## <a name="controlling-whether-the-soap-body-parts-are-wrapped"></a>Controlando se as partes do corpo SOAP estão encapsuladas  
 Por padrão, as partes do corpo SOAP são serializadas dentro de um elemento encapsulado. Por exemplo, o código a seguir mostra o `HelloGreetingMessage` elemento wrapper gerado a partir do nome do <xref:System.ServiceModel.MessageContractAttribute> tipo no contrato de mensagem para a `HelloGreetingMessage` mensagem.  
  
[!code-csharp[MessageHeaderAttribute#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/messageheaderattribute/cs/services.cs#3)]
[!code-vb[MessageHeaderAttribute#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/messageheaderattribute/vb/services.vb#3)]  
  
 Para suprimir o elemento wrapper, defina a <xref:System.ServiceModel.MessageContractAttribute.IsWrapped%2A> propriedade como `false` . Para controlar o nome e o namespace do elemento wrapper, use as <xref:System.ServiceModel.MessageContractAttribute.WrapperName%2A> Propriedades e <xref:System.ServiceModel.MessageContractAttribute.WrapperNamespace%2A> .  
  
> [!NOTE]
> Ter mais de uma parte do corpo da mensagem em mensagens que não estão encapsuladas não é compatível com o WS-I Basic Profile 1,1 e não é recomendado ao criar novos contratos de mensagem. No entanto, pode ser necessário ter mais de uma parte do corpo da mensagem desencapsulada em determinados cenários de interoperabilidade específicos. Se você pretende transmitir mais de um dado em um corpo de mensagem, é recomendável usar o modo padrão (encapsulado). Ter mais de um cabeçalho de mensagem em mensagens não encapsuladas é completamente aceitável.  
  
## <a name="using-custom-types-inside-message-contracts"></a>Usando tipos personalizados dentro de contratos de mensagem  
 Cada cabeçalho de mensagem individual e parte do corpo da mensagem é serializado (transformado em XML) usando o mecanismo de serialização escolhido para o contrato de serviço em que a mensagem é usada. O mecanismo de serialização padrão, o `XmlFormatter` , pode lidar com qualquer tipo que tenha um contrato de dados, seja explicitamente (tendo o <xref:System.Runtime.Serialization.DataContractAttribute?displayProperty=nameWithType> ) ou implicitamente (sendo um tipo primitivo, tendo o <xref:System.SerializableAttribute?displayProperty=nameWithType> e assim por diante). Para obter mais informações, consulte [usando contratos de dados](using-data-contracts.md).  
  
 No exemplo anterior, os `Operation` tipos e `BankingTransactionData` devem ter um contrato de dados e `transactionDate` são serializáveis porque <xref:System.DateTime> é um primitivo (e, portanto, tem um contrato de dados implícito).  
  
 No entanto, é possível alternar para um mecanismo de serialização diferente, o `XmlSerializer` . Se você fizer essa opção, deverá garantir que todos os tipos usados para cabeçalhos de mensagens e partes de corpo sejam serializáveis usando o `XmlSerializer` .  
  
## <a name="using-arrays-inside-message-contracts"></a>Usando matrizes dentro de contratos de mensagem  
 Você pode usar matrizes de elementos repetitivos em contratos de mensagem de duas maneiras.  
  
 A primeira é usar um <xref:System.ServiceModel.MessageHeaderAttribute> ou um <xref:System.ServiceModel.MessageBodyMemberAttribute> diretamente na matriz. Nesse caso, toda a matriz é serializada como um elemento (ou seja, um cabeçalho ou uma parte do corpo) com vários elementos filho. Considere a classe no exemplo a seguir.  
  
```csharp  
[MessageContract]  
public class BankingDepositLog  
{  
  [MessageHeader] public int numRecords;  
  [MessageHeader] public DepositRecord[] records;  
  [MessageHeader] public int branchID;  
}  
```  
  
 Isso resulta em cabeçalhos SOAP semelhante ao seguinte.  
  
```xml  
<BankingDepositLog>  
<numRecords>3</numRecords>  
<records>  
  <DepositRecord>Record1</DepositRecord>  
  <DepositRecord>Record2</DepositRecord>  
  <DepositRecord>Record3</DepositRecord>  
</records>  
<branchID>20643</branchID>  
</BankingDepositLog>  
```  
  
 Uma alternativa para isso é usar o <xref:System.ServiceModel.MessageHeaderArrayAttribute> . Nesse caso, cada elemento da matriz é serializado de forma independente e, portanto, cada elemento da matriz tem um cabeçalho, semelhante ao seguinte.  
  
```xml  
<numRecords>3</numRecords>  
<records>Record1</records>  
<records>Record2</records>  
<records>Record3</records>  
<branchID>20643</branchID>  
```  
  
 O nome padrão para entradas de matriz é o nome do membro ao qual os <xref:System.ServiceModel.MessageHeaderArrayAttribute> atributos são aplicados.  
  
 O <xref:System.ServiceModel.MessageHeaderArrayAttribute> atributo é herdado do <xref:System.ServiceModel.MessageHeaderAttribute> . Ele tem o mesmo conjunto de recursos que os atributos que não são da matriz, por exemplo, é possível definir a ordem, o nome e o namespace para uma matriz de cabeçalhos da mesma maneira que você o define para um único cabeçalho. Quando você usa a `Order` propriedade em uma matriz, ela se aplica a toda a matriz.  
  
 Você pode aplicar o <xref:System.ServiceModel.MessageHeaderArrayAttribute> somente às matrizes, não às coleções.  
  
## <a name="using-byte-arrays-in-message-contracts"></a>Usando matrizes de bytes em contratos de mensagem  
 As matrizes de bytes, quando usadas com os atributos que não são da matriz ( <xref:System.ServiceModel.MessageBodyMemberAttribute> e <xref:System.ServiceModel.MessageHeaderAttribute> ), não são tratadas como matrizes, mas como um tipo primitivo especial representado como dados codificados em base64 no XML resultante.  
  
 Quando você usa matrizes de bytes com o atributo de matriz <xref:System.ServiceModel.MessageHeaderArrayAttribute> , os resultados dependem do serializador em uso. Com o serializador padrão, a matriz é representada como uma entrada individual para cada byte. No entanto, quando o `XmlSerializer` é selecionado, (usando o <xref:System.ServiceModel.XmlSerializerFormatAttribute> no contrato de serviço), as matrizes de bytes são tratadas como dados base64, independentemente de os atributos da matriz ou não da matriz serem usados.  
  
## <a name="signing-and-encrypting-parts-of-the-message"></a>Assinando e Criptografando partes da mensagem  
 Um contrato de mensagem pode indicar se os cabeçalhos e/ou o corpo da mensagem devem ser assinados digitalmente e criptografados.  
  
 Isso é feito definindo a <xref:System.ServiceModel.MessageContractMemberAttribute.ProtectionLevel%2A?displayProperty=nameWithType> Propriedade nos <xref:System.ServiceModel.MessageHeaderAttribute> <xref:System.ServiceModel.MessageBodyMemberAttribute> atributos e. A propriedade é uma enumeração do <xref:System.Net.Security.ProtectionLevel?displayProperty=nameWithType> tipo e pode ser definida como <xref:System.Net.Security.ProtectionLevel.None> (sem criptografia ou assinatura), <xref:System.Net.Security.ProtectionLevel.Sign> (somente assinatura digital) ou <xref:System.Net.Security.ProtectionLevel.EncryptAndSign> (criptografia e assinatura digital). O padrão é <xref:System.Net.Security.ProtectionLevel.EncryptAndSign>.  
  
 Para que esses recursos de segurança funcionem, você deve configurar corretamente a associação e os comportamentos. Se você usar esses recursos de segurança sem a configuração apropriada (por exemplo, tentar assinar uma mensagem sem fornecer suas credenciais), uma exceção será lançada no momento da validação.  
  
 Para cabeçalhos de mensagens, o nível de proteção é determinado individualmente para cada cabeçalho.  
  
 Para as partes do corpo da mensagem, o nível de proteção pode ser considerado como o "nível mínimo de proteção". O corpo tem apenas um nível de proteção, independentemente do número de partes do corpo. O nível de proteção do corpo é determinado pela <xref:System.ServiceModel.MessageContractMemberAttribute.ProtectionLevel%2A> configuração de propriedade mais alta de todas as partes do corpo. No entanto, você deve definir o nível de proteção de cada parte do corpo para o nível de proteção mínima real necessário.  
  
 Considere a classe no exemplo de código a seguir.  
  
```csharp  
[MessageContract]  
public class PatientRecord  
{  
   [MessageHeader(ProtectionLevel=None)] public int recordID;  
   [MessageHeader(ProtectionLevel=Sign)] public string patientName;  
   [MessageHeader(ProtectionLevel=EncryptAndSign)] public string SSN;  
   [MessageBodyMember(ProtectionLevel=None)] public string comments;  
   [MessageBodyMember(ProtectionLevel=Sign)] public string diagnosis;  
   [MessageBodyMember(ProtectionLevel=EncryptAndSign)] public string medicalHistory;  
}  
```  
  
 Neste exemplo, o `recordID` cabeçalho não está protegido, `patientName` é `signed` e `SSN` é criptografado e assinado. Pelo menos uma parte do corpo, `medicalHistory` , foi <xref:System.Net.Security.ProtectionLevel.EncryptAndSign> aplicada e, portanto, todo o corpo da mensagem é criptografado e assinado, embora os comentários e as partes do corpo do diagnóstico especifiquem níveis de proteção inferiores.  
  
## <a name="soap-action"></a>Ação SOAP  
 Os padrões do SOAP e dos serviços Web relacionados definem uma propriedade chamada `Action` que pode estar presente para cada mensagem SOAP enviada. As <xref:System.ServiceModel.OperationContractAttribute.Action%2A?displayProperty=nameWithType> Propriedades e da operação <xref:System.ServiceModel.OperationContractAttribute.ReplyAction%2A?displayProperty=nameWithType> controlam o valor dessa propriedade.  
  
## <a name="soap-header-attributes"></a>Atributos de cabeçalho SOAP  
 O padrão SOAP define os seguintes atributos que podem existir em um cabeçalho:  
  
- `Actor/Role`( `Actor` em soap 1,1, `Role` em SOAP 1,2)  
  
- `MustUnderstand`  
  
- `Relay`  
  
 O `Actor` `Role` atributo ou especifica o Uniform Resource Identifier (URI) do nó para o qual um determinado cabeçalho é pretendido. O `MustUnderstand` atributo especifica se o nó que está processando o cabeçalho deve ser compreendido. O `Relay` atributo especifica se o cabeçalho deve ser retransmitido para nós downstream. O WCF não executa nenhum processamento desses atributos em mensagens de entrada, exceto para o `MustUnderstand` atributo, conforme especificado na seção "controle de versão de contrato de mensagem" mais adiante neste tópico. No entanto, ele permite que você leia e grave esses atributos conforme necessário, como na descrição a seguir.  
  
 Ao enviar uma mensagem, esses atributos não são emitidos por padrão. Você pode alterar isso de duas maneiras. Primeiro, você pode definir estaticamente os atributos para os valores desejados alterando <xref:System.ServiceModel.MessageHeaderAttribute.Actor%2A?displayProperty=nameWithType> as <xref:System.ServiceModel.MessageHeaderAttribute.MustUnderstand%2A?displayProperty=nameWithType> Propriedades, e <xref:System.ServiceModel.MessageHeaderAttribute.Relay%2A?displayProperty=nameWithType> , conforme mostrado no exemplo de código a seguir. (Observe que não há nenhuma `Role` propriedade; definir a <xref:System.ServiceModel.MessageHeaderAttribute.Actor%2A> Propriedade emitirá o `Role` atributo se você estiver usando SOAP 1,2).  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader(Actor="http://auditingservice.contoso.com", MustUnderstand=true)] public bool IsAudited;  
  [MessageHeader] public Operation operation;  
  [MessageBodyMember] public BankingTransactionData theData;  
}  
```  
  
 A segunda maneira de controlar esses atributos é dinamicamente, por meio de código. Você pode conseguir isso encapsulando o tipo de cabeçalho desejado no <xref:System.ServiceModel.MessageHeader%601> tipo (não se esqueça de confundir esse tipo com a versão não genérica) e usando o tipo junto com o <xref:System.ServiceModel.MessageHeaderAttribute> . Em seguida, você pode usar propriedades no <xref:System.ServiceModel.MessageHeader%601> para definir os atributos SOAP, conforme mostrado no exemplo de código a seguir.  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public MessageHeader<bool> IsAudited;  
  [MessageHeader] public Operation operation;  
  [MessageBodyMember] public BankingTransactionData theData;  
}  
// application code:  
BankingTransaction bt = new BankingTransaction();  
bt.IsAudited = new MessageHeader<bool>();  
bt.IsAudited.Content = false; // Set IsAudited header value to "false"  
bt.IsAudited.Actor="http://auditingservice.contoso.com";  
bt.IsAudited.MustUnderstand=true;  
```  
  
 Se você usar os mecanismos de controle dinâmico e estático, as configurações estáticas serão usadas como padrão, mas, posteriormente, poderão ser substituídas usando o mecanismo dinâmico, conforme mostrado no código a seguir.  
  
```csharp  
[MessageHeader(MustUnderstand=true)] public MessageHeader<Person> documentApprover;  
// later on in the code:  
BankingTransaction bt = new BankingTransaction();  
bt.documentApprover = new MessageHeader<Person>();  
bt.documentApprover.MustUnderstand = false; // override the static default of 'true'  
```  
  
 A criação de cabeçalhos repetidos com o controle de atributo dinâmico é permitida, conforme mostrado no código a seguir.  
  
```csharp  
[MessageHeaderArray] public MessageHeader<Person> documentApprovers[];  
```  
  
 No lado do recebimento, a leitura desses atributos SOAP só poderá ser feita se a <xref:System.ServiceModel.MessageHeader%601> classe for usada para o cabeçalho no tipo. Examine as `Actor` `Relay` Propriedades,, ou `MustUnderstand` de um cabeçalho do <xref:System.ServiceModel.MessageHeader%601> tipo para descobrir as configurações de atributo na mensagem recebida.  
  
 Quando uma mensagem é recebida e enviada de volta, as configurações de atributo SOAP somente vão para os cabeçalhos do <xref:System.ServiceModel.MessageHeader%601> tipo.  
  
## <a name="order-of-soap-body-parts"></a>Ordem das partes do corpo SOAP  
 Em algumas circunstâncias, talvez seja necessário controlar a ordem das partes do corpo. A ordem dos elementos do corpo é alfabética por padrão, mas pode ser controlada pela <xref:System.ServiceModel.MessageBodyMemberAttribute.Order%2A?displayProperty=nameWithType> propriedade. Essa propriedade tem a mesma semântica que a <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A?displayProperty=nameWithType> propriedade, exceto pelo comportamento em cenários de herança (em contratos de mensagem, membros do corpo de tipo base não são classificados antes dos membros do corpo do tipo derivado). Para obter mais informações, consulte [ordem de membro de dados](data-member-order.md).  
  
 No exemplo a seguir, `amount` normalmente apareceria primeiro porque ele é primeiro em ordem alfabética. No entanto, a <xref:System.ServiceModel.MessageBodyMemberAttribute.Order%2A> Propriedade a coloca na terceira posição.  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public Operation operation;  
  [MessageBodyMember(Order=1)] public Account sourceAccount;  
  [MessageBodyMember(Order=2)] public Account targetAccount;  
  [MessageBodyMember(Order=3)] public int amount;  
}  
```  
  
## <a name="message-contract-versioning"></a>Controle de versão do contrato de mensagem  
 Ocasionalmente, talvez seja necessário alterar os contratos de mensagem. Por exemplo, uma nova versão do seu aplicativo pode adicionar um cabeçalho extra a uma mensagem. Em seguida, ao enviar da nova versão para a antiga, o sistema deve lidar com um cabeçalho extra, bem como um cabeçalho ausente ao entrar na outra direção.  
  
 As regras a seguir se aplicam a cabeçalhos de versão:  
  
- O WCF não faz o objeto para os cabeçalhos ausentes – os membros correspondentes são deixados com seus valores padrão.  
  
- O WCF também ignora cabeçalhos extras inesperados. A única exceção a essa regra é se o cabeçalho extra tiver um `MustUnderstand` atributo definido como `true` na mensagem SOAP de entrada — nesse caso, uma exceção será lançada porque um cabeçalho que deve ser compreendido não pode ser processado.  
  
 Os corpos de mensagens têm regras de controle de versão semelhantes – as partes de corpo de mensagem adicionais e ausentes são ignoradas.  
  
## <a name="inheritance-considerations"></a>Considerações sobre herança  
 Um tipo de contrato de mensagem pode herdar de outro tipo, desde que o tipo base também tenha um contrato de mensagem.  
  
 Ao criar ou acessar uma mensagem usando um tipo de contrato de mensagem que herda de outros tipos de contrato de mensagem, as seguintes regras se aplicam:  
  
- Todos os cabeçalhos de mensagem na hierarquia de herança são coletados juntos para formar o conjunto completo de cabeçalhos da mensagem.  
  
- Todas as partes do corpo da mensagem na hierarquia de herança são coletadas juntas para formar o corpo completo da mensagem. As partes do corpo são ordenadas de acordo com as regras de ordenação usuais (por <xref:System.ServiceModel.MessageBodyMemberAttribute.Order%2A?displayProperty=nameWithType> propriedade e, em seguida, em ordem alfabética), sem relevância para seu lugar na hierarquia de herança. O uso da herança de contrato de mensagem em que ocorrem as partes do corpo da mensagem em vários níveis da árvore de herança é altamente desencorajado. Se uma classe base e uma classe derivada definirem um cabeçalho ou uma parte de corpo com o mesmo nome, o membro da classe base-a mais será usado para armazenar o valor desse cabeçalho ou parte do corpo.  
  
 Considere as classes no exemplo de código a seguir.  
  
```csharp  
[MessageContract]  
public class PersonRecord  
{  
  [MessageHeader(Name="ID")] public int personID;  
  [MessageBodyMember] public string patientName;  
}  
  
[MessageContract]  
public class PatientRecord : PersonRecord  
{  
  [MessageHeader(Name="ID")] public int patientID;  
  [MessageBodyMember] public string diagnosis;  
}  
```  
  
 A `PatientRecord` classe descreve uma mensagem com um cabeçalho chamado `ID` . O cabeçalho corresponde ao `personID` e não ao `patientID` membro, pois o membro da maior base é escolhido. Portanto, o `patientID` campo é inútil nesse caso. O corpo da mensagem contém o `diagnosis` elemento seguido pelo `patientName` elemento, pois essa é a ordem alfabética. Observe que o exemplo mostra um padrão que é altamente desencorajado: os contratos de mensagem base e derivada têm partes do corpo da mensagem.  
  
## <a name="wsdl-considerations"></a>Considerações de WSDL  
 Ao gerar um contrato WSDL (Web Services Description Language) de um serviço que usa contratos de mensagem, é importante lembrar que nem todos os recursos de contrato de mensagem são refletidos no WSDL resultante. Considere os seguintes pontos:  
  
- O WSDL não pode expressar o conceito de uma matriz de cabeçalhos. Ao criar mensagens com uma matriz de cabeçalhos usando o <xref:System.ServiceModel.MessageHeaderArrayAttribute> , o WSDL resultante reflete apenas um cabeçalho em vez da matriz.  
  
- O documento WSDL resultante pode não refletir algumas informações no nível de proteção.  
  
- O tipo de mensagem gerado no WSDL tem o mesmo nome que o nome de classe do tipo de contrato de mensagem.  
  
- Ao usar o mesmo contrato de mensagem em várias operações, vários tipos de mensagens são gerados no documento WSDL. Os nomes são exclusivos, adicionando os números "2", "3" e assim por diante, para usos subsequentes. Ao importar de volta o WSDL, vários tipos de contrato de mensagem são criados e são idênticos, exceto para seus nomes.  
  
## <a name="soap-encoding-considerations"></a>Considerações sobre codificação SOAP  
 O WCF permite que você use o estilo de codificação SOAP herdado de XML. no entanto, seu uso não é recomendado. Ao usar esse estilo (definindo a `Use` propriedade como `Encoded` on <xref:System.ServiceModel.XmlSerializerFormatAttribute?displayProperty=nameWithType> aplicada ao contrato de serviço), as seguintes considerações adicionais se aplicam:  
  
- Não há suporte para os cabeçalhos de mensagem; Isso significa que o atributo <xref:System.ServiceModel.MessageHeaderAttribute> e o atributo de matriz <xref:System.ServiceModel.MessageHeaderArrayAttribute> são incompatíveis com a codificação SOAP.  
  
- Se o contrato de mensagem não estiver encapsulado, ou seja, se a propriedade <xref:System.ServiceModel.MessageContractAttribute.IsWrapped%2A> for definida como `false` , o contrato de mensagem poderá ter apenas uma parte do corpo.  
  
- O nome do elemento de wrapper para o contrato de mensagem de solicitação deve corresponder ao nome da operação. Use a `WrapperName` Propriedade do contrato de mensagem para isso.  
  
- O nome do elemento wrapper para o contrato da mensagem de resposta deve ser o mesmo que o nome da operação sufixada por ' Response '. Use a <xref:System.ServiceModel.MessageContractAttribute.WrapperName%2A> Propriedade do contrato de mensagem para isso.  
  
- A codificação SOAP preserva as referências de objeto. Por exemplo, considere o código a seguir.  
  
    ```csharp  
    [MessageContract(WrapperName="updateChangeRecord")]  
    public class ChangeRecordRequest  
    {  
      [MessageBodyMember] Person changedBy;  
      [MessageBodyMember] Person changedFrom;  
      [MessageBodyMember] Person changedTo;  
    }  
  
    [MessageContract(WrapperName="updateChangeRecordResponse")]  
    public class ChangeRecordResponse  
    {  
      [MessageBodyMember] Person changedBy;  
      [MessageBodyMember] Person changedFrom;  
      [MessageBodyMember] Person changedTo;  
    }  
  
    // application code:  
    ChangeRecordRequest cr = new ChangeRecordRequest();  
    Person p = new Person("John Doe");  
    cr.changedBy=p;  
    cr.changedFrom=p;  
    cr.changedTo=p;  
    ```  
  
 Depois de serializar a mensagem usando a codificação SOAP `changedFrom` e `changedTo` não conter suas próprias cópias de `p` , mas, em vez disso, apontar para a cópia dentro do `changedBy` elemento.  
  
## <a name="performance-considerations"></a>Considerações sobre desempenho  
 Cada cabeçalho de mensagem e parte do corpo da mensagem é serializado independentemente dos outros. Portanto, os mesmos namespaces podem ser declarados novamente para cada cabeçalho e parte do corpo. Para melhorar o desempenho, especialmente em termos do tamanho da mensagem na conexão, consolide vários cabeçalhos e partes do corpo em um único cabeçalho ou parte do corpo. Por exemplo, em vez do código a seguir:  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public Operation operation;  
  [MessageBodyMember] public Account sourceAccount;  
  [MessageBodyMember] public Account targetAccount;  
  [MessageBodyMember] public int amount;  
}  
```  
  
 Use este código.  
  
```csharp  
[MessageContract]  
public class BankingTransaction  
{  
  [MessageHeader] public Operation operation;  
  [MessageBodyMember] public OperationDetails details;  
}  
  
[DataContract]  
public class OperationDetails  
{  
  [DataMember] public Account sourceAccount;  
  [DataMember] public Account targetAccount;  
  [DataMember] public int amount;  
}  
```  
  
### <a name="event-based-asynchronous-and-message-contracts"></a>Contratos de mensagens e assíncronas baseados em eventos  
 As diretrizes de design do modelo assíncrono baseado em eventos declaram que, se mais de um valor for retornado, um valor será retornado como a propriedade `Result` e os outros serão retornados como propriedades no objeto <xref:System.EventArgs>. Um resultado é que, se um cliente importar metadados usando as opções de comando assíncrono com base em eventos, e a operação retornar mais de um valor, o objeto padrão <xref:System.EventArgs> retornará um valor como a propriedade `Result` e os restantes serão propriedades do objeto <xref:System.EventArgs>.  
  
 Se você desejar receber o objeto de mensagem como a propriedade `Result` e ter valores retornados como propriedades nesse objeto, use a opção de comando `/messageContract`. Isso gera uma assinatura que retorna a mensagem de resposta como a propriedade `Result` no objeto <xref:System.EventArgs>. Todos os valores de retorno internos são propriedades do objeto da mensagem de resposta.  
  
## <a name="see-also"></a>Veja também

- [Usando contratos de dados](using-data-contracts.md)
- [Serviços de design e implantação](../designing-and-implementing-services.md)
