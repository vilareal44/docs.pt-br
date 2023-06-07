---
title: Noções básicas de autenticação HTTP
description: Examine esta introdução à autenticação HTTP no WCF, incluindo esquemas de autenticação HTTP e escolhendo um esquema de autenticação.
ms.date: 03/30/2017
ms.assetid: 9376309a-39e3-4819-b47b-a73982b57620
ms.openlocfilehash: 761ab7a92aa26ce1437eefa360e5b46df179e32d
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246513"
---
# <a name="understanding-http-authentication"></a>Noções básicas de autenticação HTTP
A autenticação é o processo de identificar se um cliente está qualificado para acessar um recurso. O protocolo HTTP dá suporte à autenticação como um meio de negociar o acesso a um recurso seguro.  
  
 A solicitação inicial de um cliente normalmente é uma solicitação anônima, que não contém nenhuma informação de autenticação. Os aplicativos de servidor HTTP podem negar a solicitação anônima enquanto indica que a autenticação é necessária. O aplicativo de servidor envia cabeçalhos WWW-Authentication para indicar os esquemas de autenticação com suporte. Este documento descreve vários esquemas de autenticação para HTTP e discute seu suporte no Windows Communication Foundation (WCF).  
  
## <a name="http-authentication-schemes"></a>Esquemas de autenticação HTTP  
 O servidor pode especificar vários esquemas de autenticação para o cliente escolher. A tabela a seguir descreve alguns dos esquemas de autenticação geralmente encontrados em aplicativos do Windows.  
  
|Esquema de Autenticação|Description|  
|---------------------------|-----------------|  
|Anônima|Uma solicitação anônima não contém nenhuma informação de autenticação. Isso é equivalente a conceder acesso de todos ao recurso a todos.|  
|Basic|A autenticação básica envia uma cadeia de caracteres codificada em base64 que contém um nome de usuário e senha para o cliente. Base64 não é uma forma de criptografia e deve ser considerado o mesmo que enviar o nome de usuário e a senha em texto não criptografado. Se um recurso precisar ser protegido, considere fortemente usar um esquema de autenticação diferente da autenticação básica.|  
|Digest|A autenticação Digest é um esquema de desafio/resposta que pretende substituir a autenticação básica. O servidor envia uma cadeia de caracteres de dados aleatórios chamada de *nonce* para o cliente como um desafio. O cliente responde com um hash que inclui o nome de usuário, a senha e o nonce, entre as informações adicionais. A complexidade que esse Exchange apresenta e o hash de dados torna mais difícil roubar e reutilizar as credenciais do usuário com esse esquema de autenticação.<br /><br /> A autenticação Digest requer o uso de contas de domínio do Windows. O *Realm* de resumo é o nome de domínio do Windows. Portanto, você não pode usar um servidor em execução em um sistema operacional que não ofereça suporte a domínios do Windows, como o Windows XP Home Edition, com autenticação Digest. Por outro lado, quando o cliente é executado em um sistema operacional que não dá suporte a domínios do Windows, uma conta de domínio deve ser especificada explicitamente durante a autenticação.|  
|NTLM|A autenticação NTLM (NT LAN Manager) é um esquema de desafio/resposta que é uma variação segura da autenticação Digest. O NTLM usa as credenciais do Windows para transformar os dados de desafio em vez do nome de usuário e da senha não codificados. A autenticação NTLM requer várias trocas entre o cliente e o servidor. O servidor e os proxies intermediários devem dar suporte a conexões persistentes para concluir a autenticação com êxito.|  
|Negotiate|Negociar a autenticação seleciona automaticamente entre o protocolo Kerberos e a autenticação NTLM, dependendo da disponibilidade. O protocolo Kerberos será usado se estiver disponível; caso contrário, o NTLM é tentado. A autenticação Kerberos melhora significativamente no NTLM. A autenticação Kerberos é mais rápida que a NTLM e permite o uso de autenticação mútua e delegação de credenciais para computadores remotos.|  
|Windows Live ID|O serviço HTTP do Windows subjacente inclui autenticação usando protocolos federados. No entanto, os transportes HTTP padrão no WCF não dão suporte ao uso de esquemas de autenticação federada, como o Microsoft Windows Live ID. O suporte para esse recurso está disponível no momento por meio do uso de segurança de mensagem. Para obter mais informações, consulte [Federação e tokens emitidos](federation-and-issued-tokens.md).|  
  
## <a name="choosing-an-authentication-scheme"></a>Escolhendo um esquema de autenticação  
 Ao selecionar os esquemas de autenticação potenciais para um servidor HTTP, alguns itens a serem considerados incluem o seguinte:  
  
- Considere se o recurso precisa ser protegido. Usar a autenticação HTTP requer a transmissão de mais dados e pode limitar a interoperabilidade com clientes. Permitir acesso anônimo a recursos que não precisam ser protegidos.  
  
- Se o recurso precisar ser protegido, considere quais esquemas de autenticação fornecem o nível de segurança necessário. O esquema de autenticação padrão mais fraco discutido aqui é a autenticação básica. A autenticação básica não protege as credenciais do usuário. O esquema de autenticação padrão mais forte é a autenticação de negociação, resultando no protocolo Kerberos.  
  
- Um servidor não deve apresentar (nos cabeçalhos WWW-Authentication) qualquer esquema que não esteja preparado para aceitar ou que não proteja adequadamente o recurso protegido. Os clientes são livres para escolher entre qualquer um dos esquemas de autenticação que o servidor apresenta. Alguns clientes assumem como padrão um esquema de autenticação fraco ou o primeiro esquema de autenticação na lista do servidor.  
  
## <a name="see-also"></a>Veja também

- [Visão geral de segurança de transporte](transport-security-overview.md)
- [Usando a representação com segurança de transporte](using-impersonation-with-transport-security.md)
- [Delegação e representação](delegation-and-impersonation-with-wcf.md)
