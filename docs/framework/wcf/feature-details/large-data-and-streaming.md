---
title: Dados grandes e streaming
description: Saiba mais sobre as considerações sobre a comunicação baseada em XML do WCF, codificadores e dados de streaming, incluindo a transferência de dados binários.
ms.date: 03/30/2017
ms.assetid: ab2851f5-966b-4549-80ab-c94c5c0502d2
ms.openlocfilehash: 2eb57e2f57bebb2e765ea798b3dff27e0187e8c7
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246578"
---
# <a name="large-data-and-streaming"></a>Dados grandes e streaming

O Windows Communication Foundation (WCF) é uma infraestrutura de comunicação baseada em XML. Como os dados XML são comumente codificados no formato de texto padrão definido na [especificação XML 1,0](https://www.w3.org/TR/REC-xml/), os desenvolvedores e arquitetos de sistemas conectados geralmente se preocupam com a superfície de conexão (ou o tamanho) das mensagens enviadas pela rede, e a codificação baseada em texto do XML representa desafios especiais para a transferência eficiente de dados binários.  
  
## <a name="basic-considerations"></a>Considerações básicas  
 Para fornecer informações básicas sobre as informações a seguir para o WCF, esta seção destaca algumas questões gerais e considerações sobre codificações, dados binários e streaming que geralmente se aplicam a infraestruturas de sistemas conectados.  
  
### <a name="encoding-data-text-vs-binary"></a>Dados de codificação: Text vs. Binary  
 As preocupações geralmente expressas pelos desenvolvedores incluem a percepção de que o XML tem uma sobrecarga significativa quando comparado com formatos binários devido à natureza repetitiva das marcas de início e de fim, que a codificação de valores numéricos é considerada como sendo significativamente maior porque são expressos em valores de texto, e que os dados binários não podem ser expressos de modo eficiente porque devem ser especialmente codificados para inserção em um formato de texto.  
  
 Embora muitas dessas e de outras preocupações semelhantes sejam válidas, a diferença real entre as mensagens codificadas em texto XML em um ambiente de serviços Web XML e as mensagens codificadas em binário em um ambiente herdado de RPC (chamada de procedimento remoto) com frequência muito menos significativa do que a consideração inicial pode sugerir.  
  
 Embora as mensagens codificadas em texto XML sejam transparentes e “legíveis pelo usuário”, em comparação, as mensagens binárias geralmente são bastante obscuras e difíceis de serem decodificadas sem ferramentas. Essa diferença na legibilidade leva um usuário a ignorar que as mensagens binárias, com frequência, também carregam metadados embutidos em sua carga, o que adiciona um sobrecarga da mesma forma como ocorre em mensagens de texto XML. Isso é especialmente verdadeiro para formatos binários que pretendem fornecer acoplamento flexível e recursos de invocação dinâmica.  
  
 No entanto, os formatos binários normalmente carregam essas informações descritivas de metadados em um "cabeçalho", que também declara o layout dos dados para os registros de dados seguintes. A carga então segue essa declaração comum do bloco de metadados com sobrecarga adicional mínima. Por outro lado, o XML embute cada item de dados em um elemento ou atributo para que os metadados embutidos sejam incluídos de maneira repetitiva para cada objeto de carga serializado. Como resultado, o tamanho de um único objeto de carga serializado é semelhante na comparação de representação de texto e binárias, pois alguns metadados descritivos devem ser expressos para ambas, mas o formato binário se beneficia da descrição dos metadados compartilhados com cada objeto de carga adicional que é transferido devido a sobrecarga geral mais baixa.  
  
 Ainda assim, para determinados tipos de dados, como números, pode haver uma desvantagem em usar representações numéricas binárias de tamanho fixo, como um tipo decimal de 128 bits, em vez de texto sem formatação, uma vez que a representação de texto sem formatação pode ser menor em vários bytes. Os dados de texto também podem ter benefícios de tamanho com as opções geralmente mais flexíveis de codificação de texto XML, enquanto alguns formatos binários podem ter seu padrão definido como Unicode de 16 bits ou mesmo de 32 bits, o que não se aplica ao formato XML binário .NET.  
  
 Como resultado, decidir entre texto ou binário não é tão fácil quanto supor que mensagens binárias sempre são menores do que mensagens de texto XML.  
  
 Uma vantagem clara das mensagens de texto XML é que elas são baseadas em padrões e oferecem a escolha mais abrangente de opções de interoperabilidade e de suporte da plataforma. Para obter mais informações, consulte a seção "codificações" mais adiante neste tópico.  
  
### <a name="binary-content"></a>Conteúdo binário  
 Uma área onde as codificações binárias são superiores às codificações baseadas em texto em termos de tamanho da mensagem resultante são os grandes itens de dados binários, como imagens, vídeos, clipes de som ou qualquer outro formulário de dados binários opacos que devem ser trocados entre os serviços e seus usuários. Para ajustar esses tipos de dados em texto XML, a abordagem mais comum é codificá-los usando codificação Base64.  
  
 Em uma cadeia de caracteres codificados em Base64, cada caractere representa 6 bits dos dados originais de 8 bits, o que resulta em uma taxa de sobrecarga de codificação de 4:3 para Base64, sem contar os caracteres adicionais de formatação (retorno de carro/alimentação de linha) que normalmente são adicionados por convenção. Embora a importância das diferenças entre as codificações XML e binárias normalmente dependa do cenário, um ganho em tamanho de mais de 33% ao transmitir uma carga de 500 MB normalmente não é aceitável.  
  
 Para evitar essa sobrecarga de codificação, o padrão MTOM (Mecanismo de otimização de transmissão de mensagens) permite exteriorizar grandes elementos de dados contidos em uma mensagem e carregá-los com a mensagem como dados binários sem nenhuma codificação especial. Com o MTOM, as mensagens são trocadas de forma semelhante às mensagens de email do protocolo SMTP com anexos ou conteúdo inserido (imagens e outros conteúdos incorporados); As mensagens MTOM são empacotadas como sequências MIME de várias partes/relacionadas com a parte raiz sendo a mensagem SOAP real.  
  
 Uma mensagem SOAP MTOM é modificada de sua versão não codificada de forma que as marcas dos elementos especiais que se referem às respectivas partes MIME tomam o lugar dos elementos originais na mensagem que continha dados binários. Como resultado, a mensagem SOAP faz referência ao conteúdo binário apontando para as partes MIME enviadas com ela, mas por outro lado carrega apenas dados de texto XML. Como esse modelo está bastante alinhado com o bem-estabelecido modelo SMTP, há um amplo suporte de ferramentas para codificar e decodificar mensagens MTOM em muitas plataformas, o que o torna uma escolha extremamente interoperável.  
  
 Ainda assim, como com a Base64, o MTOM também vem com uma sobrecarga necessária para o formato MIME, de forma que as vantagens de se usar o MTOM são consideradas apenas quando o tamanho de um elemento de dados binários excede aproximadamente 1 KB. Devido à sobrecarga, as mensagens codificadas por MTOM podem ser maiores que as mensagens que usam a codificação de Base64 para dados binários, se a carga de binários permanecer abaixo desse limite. Para obter mais informações, consulte a seção "codificações" mais adiante neste tópico.  
  
### <a name="large-data-content"></a>Grande conteúdo de dados  
 Deixando a superfície eletrônica de lado, a carga de 500 MB mencionada anteriormente também representa um grande desafio local para o serviço e o cliente. Por padrão, o WCF processa mensagens no *modo de buffer*. Isso significa que todo o conteúdo de uma mensagem está presente na memória antes de ser enviado ou depois de ser recebido. Embora essa seja uma boa estratégia na maioria dos cenários e seja necessária para recursos de mensagens, como assinaturas digitais e entrega confiável, grandes mensagens podem esgotar os recursos do sistema.  
  
 A estratégia para manipular cargas grandes é o streaming. Embora as mensagens, especialmente as expressas em XML, normalmente sejam consideradas como pacotes de dados relativamente compactos, uma mensagem pode ter um tamanho de vários gigabytes e parecer mais um fluxo de dados contínuo do que um pacote de dados. Quando os dados são transferidos no modo de streaming em vez de no modo com buffer, o remetente torna o conteúdo do corpo da mensagem disponível para o destinatário na forma de um fluxo, e a infraestrutura da mensagem encaminha continuamente os dados do remetente para o destinatário conforme eles se tornam disponíveis.  
  
 O cenário mais comum onde ocorrem essas grandes transferências de conteúdo de dados são as transferências de objetos de dados binários que:  
  
- Não podem ser divididos facilmente em uma sequência de mensagens.  
  
- Devem ser entregues em tempo hábil.  
  
- Não estão totalmente disponíveis quando a transferência é iniciada.  
  
 Para dados que não têm essas restrições, normalmente é melhor enviar sequências de mensagens no escopo de uma sessão do que uma grande mensagem. Para obter mais informações, consulte a seção "dados de streaming" mais adiante neste tópico.  
  
 Ao enviar grandes quantidades de dados, você precisará definir a `maxAllowedContentLength` configuração do IIS (para obter mais informações, consulte [Configurando limites de solicitação do IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)) e a `maxReceivedMessageSize` configuração de associação (por exemplo, [System. ServiceModel. BasicHttpBinding. MaxReceivedMessageSize](xref:System.ServiceModel.HttpBindingBase.MaxReceivedMessageSize%2A) ou <xref:System.ServiceModel.NetTcpBinding.MaxReceivedMessageSize%2A> ). A `maxAllowedContentLength` propriedade assume o padrão de 28,6 MB e a `maxReceivedMessageSize` propriedade assume o padrão de 64 KB.  
  
## <a name="encodings"></a>Codificações  
 Uma *codificação* define um conjunto de regras sobre como apresentar mensagens na conexão. Um *codificador* implementa tal codificação e é responsável, no lado do remetente, para transformar uma memória em <xref:System.ServiceModel.Channels.Message> um fluxo de bytes ou um buffer de bytes que pode ser enviado pela rede. No lado do destinatário, o codificador transforma uma sequência de bytes em uma mensagem na memória.  
  
 O WCF inclui três codificadores e permite que você escreva e conecte seus próprios codificadores, se necessário.  
  
 Cada uma das associações padrão inclui um codificador pré-configurado, por meio do qual as associações com o prefixo Net* usam o codificador binário (incluindo a classe <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>) enquanto as classes <xref:System.ServiceModel.BasicHttpBinding> e <xref:System.ServiceModel.WSHttpBinding> usam o codificador de mensagem de texto (por meio da classe <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>) por padrão.  
  
|Elemento de associação do codificador|Description|  
|-----------------------------|-----------------|  
|<xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>|O codificador de mensagem de texto é o codificador padrão para todas as associações baseadas em HTTP e é a escolha apropriada para todas as associações personalizadas onde a interoperabilidade é a maior preocupação. Esse codificador lê e grava mensagens de texto SOAP 1.1/SOAP 1.2 padrão sem tratamento especial para dados binários. Se a <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType> propriedade de uma mensagem for definida como <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType> , o invólucro de envelope SOAP será omitido da saída e somente o conteúdo do corpo da mensagem será serializado.|  
|<xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>|O codificador de mensagem MTOM é um codificador de texto que implementa o tratamento especial de dados binários e, por padrão, não é usado em nenhuma associação padrão porque é estritamente um utilitário de otimização caso a caso. Se a mensagem contiver dados binários que excedam um limite onde a codificação MTOM gere um benefício, os dados serão exteriorizados em uma parte MIME seguindo o envelope de mensagem. Consulte Habilitando o MTOM mais adiante nesta seção.|  
|<xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>|O codificador de mensagem binária é o codificador padrão para as associações net * e a opção apropriada sempre que ambas as partes de comunicação são baseadas no WCF. O codificador de mensagem binária usa o formato XML binário .NET, uma representação binária específica da Microsoft para Infosets (Conjuntos de informações XML) que geralmente geram uma superfície menor do que a representação XML 1.0 equivalente e codifica dados binários como um fluxo de bytes.|  
  
 A codificação de mensagem de texto normalmente é a melhor escolha para qualquer caminho de comunicação que exija interoperabilidade, enquanto a codificação de mensagem binária é a melhor escolha para qualquer outro caminho de comunicação. A codificação de mensagem binária normalmente gera tamanhos menores de mensagens em comparação com texto para um única mensagem e progressivamente até tamanhos menores de mensagens durante uma sessão de comunicação. Ao contrário da codificação de texto, a codificação binária não precisa usar tratamento especial para dados binários, como o uso da Base64, mas representa bytes como bytes.  
  
 Se sua solução não exigir interoperabilidade, mas se você ainda desejar usar transporte HTTP, poderá compor o <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement> em uma associação personalizada que use a classe <xref:System.ServiceModel.Channels.HttpTransportBindingElement> para o transporte. Se vários clientes em seu serviço exigirem interoperabilidade, será recomendável expor os pontos de extremidade paralelos para que cada um tenha as opções apropriadas de transporte e de codificação para os respectivos clientes habilitados.  
  
### <a name="enabling-mtom"></a>Habilitando o MTOM  
 Quando a interoperabilidade for um requisito e dados binários grandes precisarem ser enviados, a codificação de mensagem MTOM será a estratégia de codificação alternativa que você pode habilitar nas associações padrão <xref:System.ServiceModel.BasicHttpBinding> ou <xref:System.ServiceModel.WSHttpBinding> definindo a respectiva propriedade `MessageEncoding` como <xref:System.ServiceModel.WSMessageEncoding.Mtom> ou compondo <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> em <xref:System.ServiceModel.Channels.CustomBinding>. O código de exemplo a seguir, extraído do exemplo de [codificação MTOM](../samples/mtom-encoding.md) , demonstra como habilitar o MTOM na configuração.  
  
```xml  
<system.serviceModel>  
     …  
    <bindings>  
      <wsHttpBinding>  
        <binding name="ExampleBinding" messageEncoding="Mtom"/>  
      </wsHttpBinding>  
    </bindings>  
     …  
</system.serviceModel>  
```  
  
 Conforme mencionado anteriormente, a decisão de usar a codificação MTOM depende do volume de dados que você está enviando. Além disso, como o MTOM é habilitado no nível da associação, a habilitação do MTOM afeta todas as operações em um determinado ponto de extremidade.  
  
 Como o codificador MTOM sempre emite uma mensagem MIME/de várias partes codificada por MTOM independentemente dos dados binários acabarem sendo exteriorizados, geralmente você deve habilitar o MTOM apenas para pontos de extremidade que trocam mensagens com mais de 1 KB de dados binários. Além disso, os contratos de serviço criados para uso com pontos de extremidade habilitados para MTOM devem, quando possível, ser restringidos para especificar essas operações de transferência de dados. A funcionalidade de controle relacionada deve residir em um contrato separado. Essa regra de "apenas MTOM" se aplica apenas a mensagens enviadas por meio de um ponto de extremidade habilitado para MTOM. O codificador MTOM também pode decodificar e analisar mensagens de entrada não MTOM.  
  
 O uso do codificador MTOM está em conformidade com todos os outros recursos do WCF. Observe que talvez não seja possível seguir essa regra em todos os casos, como quando o suporte à sessão for necessário.  
  
### <a name="programming-model"></a>Modelo de Programação  
 Independentemente de quais dos três codificadores internos você usar em seu aplicativo, a experiência de programação é idêntica no que diz respeito à transferência de dados binários. A diferença está em como o WCF lida com os dados com base em seus tipos de dados.  
  
```csharp
[DataContract]  
class MyData  
{  
    [DataMember]  
    byte[] binaryBuffer;  
    [DataMember]  
    string someStringData;  
}
```  
  
 Ao usar o MTOM, o contrato de dados anterior é serializado de acordo com as seguintes regras:  
  
- Se `binaryBuffer` não for `null` e individualmente contiver dados suficientes para justificar a sobrecarga de exteriorização do MTOM (cabeçalhos MIME etc.) quando comparado com a codificação Base64, os dados serão exteriorizados e transportados com a mensagem como uma parte MIME binária. Se o limite não for excedido, os dados serão codificados como Base64.  
  
- A cadeia de caracteres (e todos os outros tipos que não sejam binários) sempre é representada como uma cadeia de caracteres no corpo da mensagem, independentemente do tamanho.  
  
 O efeito na codificação MTOM será o mesmo se você usar um contrato de dados explícito, conforme mostrado no exemplo anterior, usar uma lista de parâmetros em uma operação, tiver contratos de dados aninhados ou transferir um objeto do contrato de dados em uma coleção. Matrizes de bytes são sempre candidatas à otimização e serão otimizadas se os limites de otimização estiverem sendo atendidos.  
  
> [!NOTE]
> Você não deve usar tipos derivados de <xref:System.IO.Stream?displayProperty=nameWithType> em contratos de dados. Os dados de fluxo devem ser comunicados usando o modelo de streaming, explicado na seção "Dados de streaming".  
  
## <a name="streaming-data"></a>Dados de streaming  
 Quando você tem uma grande quantidade de dados a serem transferidos, o modo de transferência de streaming no WCF é uma alternativa viável para o comportamento padrão de armazenamento em buffer e processamento de mensagens na memória em sua totalidade.  
  
 Conforme mencionado anteriormente, habilite o streaming apenas para mensagens grandes (com conteúdo de texto ou binário), se os dados não puderem ser segmentados, se a mensagem precisar ser entregue em tempo hábil ou se os dados ainda não estiverem totalmente disponíveis quando a transferência for iniciada.  
  
### <a name="restrictions"></a>Restrições  
 Você não pode usar um número significativo de recursos do WCF quando o streaming está habilitado:  
  
- As assinaturas digitais do corpo da mensagem não podem ser executadas porque exigem a computação de um hash sobre o conteúdo da mensagem inteira. No streaming, o conteúdo não está totalmente disponível quando os cabeçalhos da mensagem são construídos e enviados e, portanto, uma assinatura digital não pode ser computada.  
  
- A criptografia depende das assinaturas digitais para verificar se os dados foram reconstruídos corretamente.  
  
- As sessões confiáveis devem armazenar as mensagens enviadas em buffer no cliente para nova entrega no caso de uma mensagem ser perdida na transferência e devem manter as mensagens no serviço antes de entregá-las para a implementação do serviço para preservar a ordem das mensagens no caso de as mensagens serem recebidas fora da sequência.  
  
 Devido a essas restrições funcionais, você pode usar somente opções de segurança em nível de transporte para streaming e não pode ativar sessões confiáveis. O streaming está disponível somente com as seguintes associações definidas pelo sistema:  
  
- <xref:System.ServiceModel.BasicHttpBinding>  
  
- <xref:System.ServiceModel.NetTcpBinding>  
  
- <xref:System.ServiceModel.NetNamedPipeBinding>  
  
- <xref:System.ServiceModel.WebHttpBinding>  
  
 Como os transportes subjacentes de <xref:System.ServiceModel.NetTcpBinding> e <xref:System.ServiceModel.NetNamedPipeBinding> têm entrega confiável inerente e suporte à sessão com base na conexão, ao contrário do HTTP, na prática, essas duas associações são afetadas somente minimamente por essas restrições.  
  
 O streaming não está disponível com o transporte de MSMQ (enfileiramento de mensagens) e, portanto, não pode ser usado com a classe <xref:System.ServiceModel.NetMsmqBinding> ou <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>. O transporte de enfileiramento de mensagens dá suporte apenas a transferências de dados em buffer com um tamanho restrito de mensagem, enquanto todos os outros transportes não têm nenhum limite prático de tamanho de mensagem para a grande maioria dos cenários.  
  
 O streaming também não está disponível ao usar o transporte de Canal de mesmo nível, portanto, não está disponível com <xref:System.ServiceModel.NetPeerTcpBinding>.  
  
#### <a name="streaming-and-sessions"></a>Streaming e sessões  
 Você pode obter um comportamento inesperado durante o streaming de chamadas com uma associação baseada em sessão. Todas as chamadas de streaming são feitas por um único canal (o canal de datagrama) que não oferece suporte a sessões mesmo que a associação que está sendo usada esteja configurada para usar sessões. Se vários clientes fizerem chamadas de streaming para o mesmo objeto sobre uma associação baseada em sessão, e o modo de simultaneidade do serviço estiver definido como único, e o modo de contexto de sua instância estiver definido como PerSession, todas as chamadas deverão passar pelo canal de datagrama e, portanto, apenas uma chamada será processada de cada vez. Um ou mais clientes podem atingir o tempo limite. Você pode contornar esse problema definindo o modo de contexto da instância do objeto de serviço para executar a chamada ou a simultaneidade para várias.  
  
> [!NOTE]
> MaxConcurrentSessions não tem efeito nesse caso porque há apenas uma "sessão" disponível.  
  
### <a name="enabling-streaming"></a>Habilitando o streaming  
 Você pode habilitar o streaming das seguintes maneiras:  
  
- Enviar e aceitar solicitações no modo de streaming e aceitar e retornar respostas no modo com buffer (<xref:System.ServiceModel.TransferMode.StreamedRequest>).  
  
- Enviar e aceitar solicitações no modo com buffer e aceitar e retornar respostas no modo streaming (<xref:System.ServiceModel.TransferMode.StreamedResponse>).  
  
- Enviar e receber solicitações e respostas no modo streaming nas duas direções. (<xref:System.ServiceModel.TransferMode.Streamed>).  
  
 Você pode desabilitar o streaming definindo o modo de transferência como <xref:System.ServiceModel.TransferMode.Buffered>, que é a configuração padrão em todas as associações. O código a seguir mostra como definir o modo de transferência na configuração.  
  
```xml  
<system.serviceModel>  
     …  
    <bindings>  
      <basicHttpBinding>  
        <binding name="ExampleBinding" transferMode="Streamed"/>  
      </basicHttpBinding>  
    </bindings>  
     …  
</system.serviceModel>  
```  
  
 Ao criar uma instância de sua associação no código, você deve definir a respectiva propriedade `TransferMode` da associação (ou o elemento de associação de transporte se estiver compondo uma associação personalizada) para um dos valores mencionados anteriormente.  
  
 Você pode ativar o streaming para solicitações e respostas ou para as duas direções independentemente, em qualquer lado das partes da comunicação sem afetar a funcionalidade. No entanto, você deve sempre presumir que o tamanho dos dados transferidos é tão significativo que a habilitação de streaming é justificada nos dois pontos de extremidade de um link de comunicação. Para comunicação entre plataformas em que um dos pontos de extremidade não é implementado com o WCF, a capacidade de usar o streaming depende dos recursos de streaming da plataforma. Outra rara exceção pode ser um cenário orientado pelo consumo de memória, onde um cliente ou um serviço deve minimizar o conjunto de trabalho e possa suportar apenas tamanhos de buffer pequenos.  
  
### <a name="enabling-asynchronous-streaming"></a>Habilitando o streaming assíncrono  
 Para habilitar o streaming assíncrono, adicione o comportamento do ponto de extremidade <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior> ao host de serviço e defina sua propriedade <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior.AsynchronousSendEnabled%2A> como `true`. Também adicionamos o recurso de streaming assíncrono verdadeiro no lado do envio. Isso melhora a escalabilidade do serviço em cenários onde ele está executando o streaming de mensagens para vários clientes, alguns dos quais estão lendo lentamente, possivelmente devido a congestionamento da rede, ou não estão lendo nada. Nesses cenários, agora não bloqueamos threads individuais no serviço por cliente. Isso garante que o serviço possa processar muito mais clientes, melhorando dessa forma a escalabilidade do serviço.  
  
### <a name="programming-model-for-streamed-transfers"></a>Modelo de programação para transferências por streaming  
 O modelo de programação de streaming é simples. Para receber dados por streaming, especifique um contrato de operação que tenha um único parâmetro de entrada tipada <xref:System.IO.Stream>. Para retornar dados por streaming, retorne uma referência a <xref:System.IO.Stream>.  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IStreamedService  
{  
    [OperationContract]  
    Stream Echo(Stream data);  
    [OperationContract]  
    Stream RequestInfo(string query);  
    [OperationContract(OneWay=true)]  
    void ProvideInfo(Stream data);  
}  
```  
  
 A operação `Echo` no exemplo anterior recebe e retorna um fluxo e, portanto, deve ser usada em uma associação com <xref:System.ServiceModel.TransferMode.Streamed>. Para a operação `RequestInfo`, <xref:System.ServiceModel.TransferMode.StreamedResponse> é a melhor opção, porque retorna apenas <xref:System.IO.Stream>. A operação unidirecional é mais adequada para <xref:System.ServiceModel.TransferMode.StreamedRequest>.  
  
 Observe que a adição de um segundo parâmetro às seguintes operações de `Echo` ou `ProvideInfo` faz com que o modelo do serviço seja revertido para uma estratégia em buffer e use a representação da serialização do tempo de execução do fluxo. Apenas operações com um único parâmetro de fluxo de entrada são compatíveis com streaming de solicitação de ponta a ponta.  
  
 Essa regra se aplica da mesma forma a contratos de mensagens. Conforme mostrado no contrato de mensagem a seguir, você pode ter apenas um único membro de corpo em seu contrato de mensagem que seja um fluxo. Se você desejar comunicar informações adicionais com o fluxo, essas informações deverão ser transportadas em cabeçalhos de mensagem. O corpo da mensagem é exclusivamente reservado para o conteúdo do fluxo.  
  
```csharp
[MessageContract]  
public class UploadStreamMessage  
{  
   [MessageHeader]  
   public string appRef;  
   [MessageBodyMember]  
   public Stream data;  
}
```  
  
 Transferências em streaming são encerradas e a mensagem é fechada quando o fluxo atinge o EOF (final do arquivo). Ao enviar uma mensagem (retornando um valor ou invocando uma operação), você pode passar um <xref:System.IO.FileStream> e a infraestrutura do WCF subsequentemente efetua pull de todos os dados desse fluxo até que o fluxo tenha sido totalmente lido e atingido o EOF. Para transferir dados em streaming para uma origem onde não existe nenhuma classe derivada <xref:System.IO.Stream> pré-criada, construa essa classe, sobreponha-a sobre a origem do fluxo e use como o argumento ou o valor de retorno.  
  
 Ao receber uma mensagem, o WCF constrói um fluxo sobre o conteúdo do corpo da mensagem codificada em Base64 (ou a respectiva parte MIME se estiver usando MTOM) e o fluxo atingirá EOF quando o conteúdo tiver sido lido.  
  
 O streaming em nível de transporte também funciona com qualquer outro tipo de contrato de mensagem (listas de parâmetros, argumentos de contratos de dados e contrato de mensagem explícito), mas como a serialização e a desserialização dessas mensagens tipadas exigem buffer pelo serializador, não é aconselhável usar essas variantes de contrato.  
  
### <a name="special-security-considerations-for-large-data"></a>Considerações de segurança especial para grandes dados  
 Todas as associações permitem restringir o tamanho das mensagens de entrada para evitar ataques de negação de serviço. O <xref:System.ServiceModel.BasicHttpBinding> , por exemplo, expõe uma propriedade [System. ServiceModel. BasicHttpBinding. MaxReceivedMessageSize](xref:System.ServiceModel.HttpBindingBase.MaxReceivedMessageSize%2A) que limita o tamanho da mensagem de entrada e, portanto, também limita a quantidade máxima de memória que é acessada durante o processamento da mensagem. Essa unidade é definida em bytes com um valor padrão de 65.536 bytes.  
  
 Uma ameaça de segurança que é específica para o cenário de streaming de dados grandes provoca uma negação de serviço fazendo com que os dados sejam armazenados em buffer quando o receptor espera que os dados sejam enviados por streaming. Por exemplo, o WCF sempre armazena em buffer os cabeçalhos SOAP de uma mensagem e, portanto, um invasor pode construir uma mensagem mal-intencionada grande que consiste inteiramente em cabeçalhos para forçar os dados a serem armazenados em buffer. Quando o streaming está habilitado, `MaxReceivedMessageSize` pode ser definido como um valor extremamente grande, porque o receptor nunca espera que a mensagem inteira seja imediatamente armazenada em buffer na memória. Se o WCF for forçado a armazenar a mensagem em buffer, ocorrerá um estouro de memória.  
  
 Portanto, restringir o tamanho máximo de mensagens de entrada não é suficiente nesse caso. A `MaxBufferSize` propriedade é necessária para restringir a memória que o WCF armazena em buffers. É importante definir isso como um valor seguro (ou manter o valor padrão) ao usar streaming. Por exemplo, suponha que o serviço precise receber arquivos de até 4 GB de tamanho e armazená-los no disco local. Suponha também que sua memória esteja restrita de tal forma que você possa armazenar apenas 64 KB de dados em buffer de cada vez. Portanto, você deve definir `MaxReceivedMessageSize` como 4 GB e `MaxBufferSize` como 64 KB. Além disso, na implementação de seu serviço, garanta que você leia somente o fluxo de entrada em partes de 64 KB e não leia a próxima parte antes que a anterior seja gravada em disco e descartada da memória.  
  
 Também é importante entender que essa cota limita apenas o armazenamento em buffer feito pelo WCF e não pode protegê-lo contra qualquer buffer que você faça em seu próprio serviço ou implementação de cliente. Para obter mais informações sobre considerações de segurança adicionais, consulte [considerações de segurança para dados](security-considerations-for-data.md).  
  
> [!NOTE]
> A decisão de usar transferência em buffer ou em streaming é uma decisão local do ponto de extremidade. Para transportes HTTP, o modo de transferência não se propaga para uma conexão ou para servidores proxy e outros intermediários. A definição do modo de transferência não é refletida na descrição da interface de serviço. Depois de gerar um cliente WCF para um serviço, você deve editar o arquivo de configuração para serviços destinados a serem usados com transferências transmitidas para definir o modo. Para transportes TCP e pipe nomeado, o modo de transferência é propagado como uma declaração de política.  
  
## <a name="see-also"></a>Veja também

- [Como habilitar transmissão](how-to-enable-streaming.md)
