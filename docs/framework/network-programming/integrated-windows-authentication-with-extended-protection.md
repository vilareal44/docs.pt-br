---
title: Autenticação Integrada do Windows com Proteção Estendida
ms.date: 03/30/2017
ms.assetid: 81731998-d5e7-49e4-ad38-c8e6d01689d0
ms.openlocfilehash: d69471f4be0f102381dee4fc5037e8f8b0c625c3
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144845"
---
# <a name="integrated-windows-authentication-with-extended-protection"></a>Autenticação Integrada do Windows com Proteção Estendida
Foram feitas melhorias que afetam a maneira como a autenticação integrada do Windows é controlada por <xref:System.Net.HttpWebRequest>, <xref:System.Net.HttpListener>, <xref:System.Net.Mail.SmtpClient>, <xref:System.Net.Security.SslStream>, <xref:System.Net.Security.NegotiateStream> e por classes relacionadas no <xref:System.Net> e por namespaces relacionados. Foi adicionado suporte à proteção estendida a fim de aprimorar a segurança.  
  
 Essas alterações podem afetar os aplicativos que usam essas classes para fazer solicitações da Web e receber respostas em que a autenticação integrada do Windows é usada. Essa alteração também pode afetar os servidores Web e aplicativos cliente configurados para usar a autenticação integrada do Windows.  
  
 Essas alterações também podem afetar os aplicativos que usam essas classes para fazer outros tipos de solicitações e receber respostas em que a autenticação integrada do Windows é usada.  
  
 As alterações para dar suporte à proteção estendida estão disponíveis somente para aplicativos no Windows 7 e Windows Server 2008 R2. Os recursos de proteção estendida não estão disponíveis em versões anteriores do Windows.  
  
## <a name="overview"></a>Visão geral  
 O design da autenticação integrada do Windows permite que algumas respostas de desafio de credencial sejam universais, o que significa que elas podem ser reutilizadas ou encaminhadas. As respostas de desafio devem ser construídas no mínimo com informações específicas do destino e preferencialmente também com algumas informações específicas de canal. Os serviços, em seguida, podem fornecer proteção estendida para assegurar que as respostas de desafio de credencial contenham informações específicas do serviço, tal como um SPN (nome da entidade de serviço). Com essas informações nas trocas de credenciais, os serviços são capazes de proteger melhor contra o uso mal intencionado de respostas de desafio de credencial que podem ter sido usadas incorretamente.  
  
 O design de proteção estendida é uma melhoria projetada para minimizar ataques de retransmissão de autenticação de protocolos de autenticação. Ele gira em torno do conceito de informações de associação de serviço/canal.  
  
 Os objetivos gerais são os seguintes:  
  
1. Se o cliente é atualizado para dar suporte a proteção estendida, os aplicativos devem fornecer informações de associação de serviço e de associação de canal para todos os protocolos de autenticação com suporte. Informações de associação de canal só podem ser fornecidas quando há um canal (TLS) ao qual se associar. Informações de associação de serviço sempre devem ser fornecidas.  
  
2. Servidores atualizados que estão configurados corretamente poderão verificar as e informações de associação de serviço e de associação de canal quando elas estiverem presentes no token de autenticação de cliente e rejeitar a tentativa de autenticação se as associações de canal não corresponderem. Dependendo do cenário de implantação, os servidores podem verificar a associação de canal, a associação de serviço ou ambas.  
  
3. Servidores atualizados têm a capacidade de aceitar ou rejeitar solicitações de cliente de nível inferior que não contêm as informações de associação de canal com base em política.  
  
 Informações usadas pela proteção estendida consistem em uma ou ambas as duas partes a seguir:  
  
1. Um token de associação de canal ou CBT.  
  
2. Informações de associação de serviço na forma de um nome da entidade de serviço ou SPN.  
  
 Informações de associação de serviço são uma indicação da intenção do cliente para autenticar para um ponto de extremidade de serviço específico. Elas são comunicadas de cliente para servidor com as seguintes propriedades:  
  
- O valor de SPN deve estar disponível para o servidor que executa a autenticação de cliente em forma de texto não criptografado.  
  
- O valor do SPN é público.  
  
- O SPN deve ser protegido criptograficamente em trânsito, de modo que um ataque man-in-the-middle não possa inserir, remover nem modificar seu valor.  
  
 Um CBT é uma propriedade de externa do canal de segurança (como TLS) usada para ligá-lo (associá-lo) a uma conversa em um canal interno autenticado pelo cliente. O CBT deve ter as seguintes propriedades (também definidas pela RFC 5056 da IETF):  
  
- Quando um canal externo existe, o valor do CBT deve ser uma propriedade que identifica o canal externo ou o ponto de extremidade do servidor, no qual se chega independentemente por ambos o lado do cliente e o lado do servidor de uma conversa.  
  
- O valor do CBT enviado pelo cliente não deve ser algo que um invasor pode influenciar.  
  
- Não há garantias sobre sigilo do valor de CBT. Isso não significa no entanto que o valor da associação de serviço, bem como informações de associação de canal, sempre possam ser examinadas por qualquer outro que não o servidor que executa a autenticação, já que o protocolo portando o CBT pode estar criptografando-o.  
  
- O CBT deve ter sua integridade protegida criptograficamente em trânsito, de modo que um invasor não possa inserir, remover nem modificar seu valor.  
  
 A associação de canal é realizada pelo cliente transferindo o SPN e o CBT para o servidor de maneira inviolável. O servidor valida as informações de associação de canal de acordo com sua política e rejeita tentativas de autenticação cujo destino pretendido ele não acredita ter sido ele mesmo. Desse modo, os dois canais tornam-se criptograficamente associados juntos.  
  
 Para preservar a compatibilidade com os clientes e aplicativos existentes, um servidor pode ser configurado para permitir tentativas de autenticação por clientes que ainda não dão suporte à proteção estendida. Isso é chamado de uma configuração "parcialmente protegida", em contraste com uma configuração "totalmente protegida".  
  
 Vários componentes nos namespaces <xref:System.Net> e <xref:System.Net.Security> executam a autenticação integrada do Windows em nome de um aplicativo de chamada. Esta seção descreve as alterações nos componentes do System.Net para adicionar a proteção estendida no uso que eles fazem da autenticação integrada do Windows.  
  
 A proteção estendida tem suporte atualmente no Windows 7. Um mecanismo é fornecido para que um aplicativo possa determinar se o sistema operacional dá suporte à proteção estendida.  
  
## <a name="changes-to-support-extended-protection"></a>Alterações para dar suporte à proteção estendida  
 O processo de autenticação usado com autenticação integrada do Windows, dependendo do protocolo de autenticação usado, geralmente inclui um desafio emitido pelo computador de destino e enviado de volta para o computador cliente. A proteção estendida adiciona novos recursos para esse processo de autenticação  
  
 O namespace <xref:System.Security.Authentication.ExtendedProtection> dá suporte à autenticação usando proteção estendida para aplicativos. A classe <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding> nesse namespace representa uma associação de canal. A classe <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> nesse namespace representa a política de proteção estendida usada pelo servidor para validar as conexões de entrada do cliente. Outros membros de classe são usados com proteção estendida.  
  
 Para aplicativos para servidores, essas classes incluem o seguinte:  
  
 Um <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> que tem os seguintes elementos:  
  
- Uma propriedade <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy.OSSupportsExtendedProtection%2A> que indica se o sistema operacional dá suporte à autenticação integrada do Windows com proteção estendida.  
  
- Um valor <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement> que indica quando a política de proteção estendida deve ser imposta.  
  
- Um valor <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario> que indica o cenário de implantação. Isso influencia o modo como a proteção estendida é verificada.  
  
- Um <xref:System.Security.Authentication.ExtendedProtection.ServiceNameCollection> opcional que contém a lista de SPN personalizada que é usada para fazer a correspondência com o SPN fornecido pelo cliente como o destino pretendido da autenticação.  
  
- Um <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding> opcional que contém uma associação de canal personalizada a ser usada para validação. Esse cenário não é um caso comum  
  
 O namespace <xref:System.Security.Authentication.ExtendedProtection.Configuration> dá suporte à configuração da autenticação usando proteção estendida para aplicativos.  
  
 Um número de alterações de recursos foram feitas para dar suporte à proteção estendida no namespace <xref:System.Net> existente. Essas alterações incluem o seguinte:  
  
- Uma nova classe <xref:System.Net.TransportContext> adicionada ao namespace <xref:System.Net> que representa um contexto de transporte.  
  
- Novos métodos de sobrecarga <xref:System.Net.HttpWebRequest.EndGetRequestStream%2A> e <xref:System.Net.HttpWebRequest.GetRequestStream%2A> na classe <xref:System.Net.HttpWebRequest> que permitem recuperar o <xref:System.Net.TransportContext> para dar suporte à proteção estendida para aplicativos cliente.  
  
- Adições para as classes <xref:System.Net.HttpListener> e <xref:System.Net.HttpListenerRequest> para dar suporte a aplicativos para servidores.  
  
 Foi feita uma alteração de recurso para dar suporte à proteção estendida para aplicativos cliente SMTP existentes no namespace <xref:System.Net.Mail>:  
  
- Uma propriedade <xref:System.Net.Mail.SmtpClient.TargetName%2A> na classe <xref:System.Net.Mail.SmtpClient> que representa o SPN a ser usado para autenticação ao usar proteção estendida para aplicativos cliente SMTP.  
  
 Um número de alterações de recursos foram feitas para dar suporte à proteção estendida no namespace <xref:System.Net.Security> existente. Essas alterações incluem o seguinte:  
  
- Novos métodos de sobrecarga <xref:System.Net.Security.NegotiateStream.BeginAuthenticateAsClient%2A> e <xref:System.Net.Security.NegotiateStream.AuthenticateAsClient%2A> na classe <xref:System.Net.Security.NegotiateStream> que permitem passar um CBT para dar suporte à proteção estendida para aplicativos cliente.  
  
- Novos métodos de sobrecarga <xref:System.Net.Security.NegotiateStream.BeginAuthenticateAsServer%2A> e <xref:System.Net.Security.NegotiateStream.AuthenticateAsServer%2A> na classe <xref:System.Net.Security.NegotiateStream> que permitem passar um <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> para dar suporte à proteção estendida para aplicativos para servidores.  
  
- Uma nova propriedade <xref:System.Net.Security.SslStream.TransportContext%2A> na classe <xref:System.Net.Security.SslStream> para dar suporte à proteção estendida para aplicativos cliente e aplicativos para servidores.  
  
 Uma propriedade <xref:System.Net.Configuration.SmtpNetworkElement> foi adicionada para dar suporte à configuração da proteção estendida para clientes SMTP no namespace <xref:System.Net.Security>.  
  
## <a name="extended-protection-for-client-applications"></a>Proteção estendida para aplicativos cliente  
 O suporte à proteção estendida para a maioria dos aplicativos cliente ocorre automaticamente. As classes <xref:System.Net.HttpWebRequest> e <xref:System.Net.Mail.SmtpClient> dão suporte à proteção estendida sempre que a versão subjacente do Windows dá suporte à proteção estendida. Uma instância de <xref:System.Net.HttpWebRequest> envia um SPN construído com base em <xref:System.Uri>. Por padrão, uma instância de <xref:System.Net.Mail.SmtpClient> envia um SPN construído com base no nome do host do servidor de email SMTP.  
  
 Para autenticação personalizada, os aplicativos cliente podem usar os métodos <xref:System.Net.HttpWebRequest.EndGetRequestStream%28System.IAsyncResult%2CSystem.Net.TransportContext%40%29?displayProperty=nameWithType> ou <xref:System.Net.HttpWebRequest.GetRequestStream%28System.Net.TransportContext%40%29?displayProperty=nameWithType> na classe <xref:System.Net.HttpWebRequest>, que permitem recuperar o <xref:System.Net.TransportContext> e o CBT usando o método <xref:System.Net.TransportContext.GetChannelBinding%2A>.  
  
 O SPN a ser usado para autenticação integrada do Windows enviada por uma instância de <xref:System.Net.HttpWebRequest> para um determinado serviço pode ser substituída pela configuração da propriedade <xref:System.Net.AuthenticationManager.CustomTargetNameDictionary%2A>.  
  
 A propriedade <xref:System.Net.Mail.SmtpClient.TargetName%2A> pode ser usada para definir um SPN personalizado a ser usado para autenticação integrada do Windows para a conexão SMTP.  
  
## <a name="extended-protection-for-server-applications"></a>Proteção estendida para aplicativos para servidores  
 <xref:System.Net.HttpListener> fornece automaticamente mecanismos para validar as associações de serviço ao executar a autenticação HTTP.  
  
 O cenário mais seguro é habilitar a proteção estendida para `HTTPS://` prefixos. Nesse caso, defina <xref:System.Net.HttpListener.ExtendedProtectionPolicy%2A?displayProperty=nameWithType> para um <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> com <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement> definida para <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> ou <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> e <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario> definida para <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario.TransportSelected>. Um valor de <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> coloca <xref:System.Net.HttpListener> no modo parcialmente elevado, enquanto <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> corresponde ao modo totalmente elevado.  
  
 Nessa configuração, quando uma solicitação é feita ao servidor por meio de um canal seguro externo, o canal externo é consultado para uma associação de canal. A associação de canal é passada para chamadas SSPI de autenticação, que validam que a associação de canal no blob de autenticação é correspondente. Existem três desfechos possíveis:  
  
1. O sistema operacional subjacente do servidor não dá suporte à proteção estendida. A solicitação não será exposta para o aplicativo e uma resposta de não autorizado (401) será retornada ao cliente. Uma mensagem será registrada para a origem do rastreamento <xref:System.Net.HttpListener> especificando o motivo da falha.  
  
2. A chamada SSPI falha indicando que o cliente especificou uma associação de canal que não correspondeu ao valor esperado recuperado do canal externo ou então indicando que o cliente falhou ao fornecer uma associação de canal quando a política de proteção estendida do servidor foi configurada para <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always>. Em ambos os casos, a solicitação não será exposta para o aplicativo e uma resposta de não autorizado (401) será retornada ao cliente. Uma mensagem será registrada para a origem do rastreamento <xref:System.Net.HttpListener> especificando o motivo da falha.  
  
3. O cliente especifica a associação de canal correta ou tem permissão para se conectar sem especificar uma associação de canal, já que a política de proteção estendida do servidor é configurada com <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported>. A solicitação é retornada ao aplicativo para processamento. Nenhuma verificação de nome de serviço é executada automaticamente. Um aplicativo pode optar por executar sua própria validação de nome de serviço usando a propriedade <xref:System.Net.HttpListenerRequest.ServiceName%2A>, mas sob essas circunstâncias, isso é redundante.  
  
 Se um aplicativo faz suas próprias chamadas SSPI para realizar a autenticação com base em blobs passados bidirecionalmente dentro do corpo de uma solicitação HTTP e deseja dar suporte a associação de canal, ele precisa recuperar a associação de canal esperada do canal seguro externo usando <xref:System.Net.HttpListener> para passá-la para a função [AcceptSecurityContext](/windows/win32/api/sspi/nf-sspi-acceptsecuritycontext) de Win32 nativo. Para fazer isso, use a propriedade <xref:System.Net.HttpListenerRequest.TransportContext%2A> e chame o método <xref:System.Net.TransportContext.GetChannelBinding%2A> para recuperar o CBT. Há suporte apenas para associações de ponto de extremidade. Se qualquer coisa diferente de <xref:System.Security.Authentication.ExtendedProtection.ChannelBindingKind.Endpoint> for especificada, uma <xref:System.NotSupportedException> será gerada. Se o sistema operacional subjacente oferecer suporte à associação de canal, o <xref:System.Net.TransportContext.GetChannelBinding%2A> método retornará um <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding> <xref:System.Runtime.InteropServices.SafeHandle> ponteiro de encapsulamento para uma associação de canal adequada para passar para a função [AcceptSecurityContext](/windows/win32/api/sspi/nf-sspi-acceptsecuritycontext) como o membro pvBuffer de uma estrutura SecBuffer passada no `pInput` parâmetro. A propriedade <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding.Size%2A> contém o comprimento, em bytes, da associação de canal. Se o sistema operacional subjacente não dá suporte a associações de canal, a função retornará `null`.  
  
 Outro cenário possível é habilitar a proteção estendida para `HTTP://` prefixos quando proxies não são usados. Nesse caso, defina <xref:System.Net.HttpListener.ExtendedProtectionPolicy%2A?displayProperty=nameWithType> para um <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> com <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement> definida para <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> ou <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> e <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario> definida para <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario.TransportSelected>. Um valor de <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> coloca <xref:System.Net.HttpListener> no modo parcialmente elevado, enquanto <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> corresponde ao modo totalmente elevado.  
  
 Uma lista padrão de nomes de serviço permitidos é criada com base nos prefixos que foram registrados com o <xref:System.Net.HttpListener>. Essa lista padrão pode ser examinada por meio da propriedade <xref:System.Net.HttpListener.DefaultServiceNames%2A>. Se esta lista não é abrangente, um aplicativo pode especificar uma coleção de nomes de serviço personalizada no construtor para a classe <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy>, que será usada em vez da lista de nomes de serviço padrão.  
  
 Nessa configuração, quando uma solicitação é feita ao servidor sem um canal externo seguro, a autenticação continua normalmente sem uma verificação de associação de canal. Se a autenticação for bem-sucedida, o contexto será consultado para o nome do serviço que o cliente forneceu e validado mediante a lista de nomes de serviço aceitáveis. Existem quatro desfechos possíveis:  
  
1. O sistema operacional subjacente do servidor não dá suporte à proteção estendida. A solicitação não será exposta para o aplicativo e uma resposta de não autorizado (401) será retornada ao cliente. Uma mensagem será registrada para a origem do rastreamento <xref:System.Net.HttpListener> especificando o motivo da falha.  
  
2. O sistema operacional subjacente do cliente não dá suporte à proteção estendida. Na configuração <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported>, a tentativa de autenticação será bem-sucedida e a solicitação será retornada para o aplicativo. Na configuração <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always>, a tentativa de autenticação falhará. A solicitação não será exposta para o aplicativo e uma resposta de não autorizado (401) será retornada ao cliente. Uma mensagem será registrada para a origem do rastreamento <xref:System.Net.HttpListener> especificando o motivo da falha.  
  
3. O sistema operacional subjacente do cliente dá suporte à proteção estendida, mas o aplicativo não especificou uma associação de serviço. A solicitação não será exposta para o aplicativo e uma resposta de não autorizado (401) será retornada ao cliente. Uma mensagem será registrada para a origem do rastreamento <xref:System.Net.HttpListener> especificando o motivo da falha.  
  
4. O cliente especificou uma associação de serviço. A associação de serviço é comparada com a lista de associações de serviço permitidas. Se ela for correspondente, a solicitação será retornada ao aplicativo. Caso contrário, a solicitação não será exposta para o aplicativo e uma resposta de não autorizado (401) será retornada automaticamente ao cliente. Uma mensagem será registrada para a origem do rastreamento <xref:System.Net.HttpListener> especificando o motivo da falha.  
  
 Se essa abordagem simples usando uma lista de nomes de serviço aceitáveis com permissão for insuficiente, um aplicativo poderá fornecer sua própria validação de nome de serviço consultando a propriedade <xref:System.Net.HttpListenerRequest.ServiceName%2A>. Nos casos 1 e 2 acima, a propriedade retornará `null`. No caso 3, ela retornará uma cadeia de caracteres vazia. No caso 4, o nome do serviço especificado pelo cliente será retornado.  
  
 Esses recursos de proteção estendida também podem ser usados por aplicativos para servidores para autenticação com outros tipos de solicitações e quando proxies confiáveis são usados.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Security.Authentication.ExtendedProtection>
- <xref:System.Security.Authentication.ExtendedProtection.Configuration>
