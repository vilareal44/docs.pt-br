---
title: Usando a representação com segurança de transporte
ms.date: 03/30/2017
ms.assetid: 426df8cb-6337-4262-b2c0-b96c2edf21a9
ms.openlocfilehash: 1d33bfbbb74266aefa538166b4e1aca7d7e315ef
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594966"
---
# <a name="using-impersonation-with-transport-security"></a>Usando a representação com segurança de transporte
*Representação* é a capacidade de um aplicativo de servidor assumir a identidade do cliente. É comum que os serviços usem a representação ao validar o acesso aos recursos. O aplicativo de servidor é executado usando uma conta de serviço, mas quando o servidor aceita uma conexão de cliente, ele representa o cliente para que as verificações de acesso sejam executadas usando as credenciais do cliente. A segurança de transporte é um mecanismo para transmitir credenciais e proteger a comunicação usando essas credenciais. Este tópico descreve como usar a segurança de transporte no Windows Communication Foundation (WCF) com o recurso de representação. Para obter mais informações sobre a representação usando a segurança da mensagem, consulte [delegação e representação](delegation-and-impersonation-with-wcf.md).  
  
## <a name="five-impersonation-levels"></a>Cinco níveis de representação  
 A segurança de transporte usa cinco níveis de representação, conforme descrito na tabela a seguir.  
  
|Nível de representação|Descrição|  
|-------------------------|-----------------|  
|Nenhum|O aplicativo de servidor não tenta representar o cliente.|  
|Anônima|O aplicativo de servidor pode executar verificações de acesso nas credenciais do cliente, mas não recebe nenhuma informação sobre a identidade do cliente. Esse nível de representação é significativo apenas para comunicação no computador, como pipes nomeados. O uso de `Anonymous` com uma conexão remota promove o nível de representação para identificar.|  
|Identificar|O aplicativo de servidor conhece a identidade do cliente e pode executar a validação de acesso em relação às credenciais do cliente, mas não pode representar o cliente. Identificar é o nível de representação padrão usado com credenciais SSPI no WCF, a menos que o provedor de token forneça um nível de representação diferente.|  
|Impersonate|O aplicativo de servidor pode acessar recursos no computador do servidor como o cliente, além de executar verificações de acesso. O aplicativo de servidor não pode acessar recursos em computadores remotos usando a identidade do cliente porque o token representado não tem credenciais de rede|  
|Delegar|Além de ter os mesmos recursos que `Impersonate` o, o nível de representação de delegado também permite que o aplicativo de servidor acesse recursos em computadores remotos usando a identidade do cliente e passe a identidade para outros aplicativos.<br /><br /> **Importante** A conta de domínio do servidor deve ser marcada como confiável para delegação no controlador de domínio para usar esses recursos adicionais. Esse nível de representação não pode ser usado com contas de domínio de cliente marcadas como confidenciais.|  
  
 Os níveis mais comumente usados com segurança de transporte são `Identify` e `Impersonate` . Os níveis `None` e `Anonymous` não são recomendados para uso típico, e muitos transportes não dão suporte ao uso desses níveis com autenticação. O `Delegate` nível é um recurso poderoso que deve ser usado com cuidado. Somente aplicativos de servidor confiáveis devem receber a permissão para delegar credenciais.  
  
 Usar a representação nos `Impersonate` `Delegate` níveis ou exige que o aplicativo de servidor tenha o `SeImpersonatePrivilege` privilégio. Por padrão, um aplicativo tem esse privilégio se estiver sendo executado em uma conta no grupo Administradores ou em uma conta com um SID de serviço (serviço de rede, serviço local ou sistema local). A representação não requer autenticação mútua do cliente e do servidor. Alguns esquemas de autenticação que dão suporte à representação, como NTLM, não podem ser usados com autenticação mútua.  
  
## <a name="transport-specific-issues-with-impersonation"></a>Problemas específicos de transporte com representação  
 A escolha de um transporte no WCF afeta as possíveis opções de representação. Esta seção descreve os problemas que afetam os transportes padrão HTTP e pipe nomeado no WCF. Os transportes personalizados têm suas próprias restrições quanto ao suporte para representação.  
  
### <a name="named-pipe-transport"></a>Transporte de pipe nomeado  
 Os seguintes itens são usados com o transporte de pipe nomeado:  
  
- O transporte de pipe nomeado destina-se somente ao uso no computador local. O transporte de pipe nomeado no WCF explicitamente não permite conexões entre computadores.  
  
- Pipes nomeados não podem ser usados com o `Impersonate` `Delegate` nível de representação ou. O pipe nomeado não pode impor a garantia no computador nesses níveis de representação.  
  
 Para obter mais informações sobre pipes nomeados, consulte [escolhendo um transporte](choosing-a-transport.md).  
  
### <a name="http-transport"></a>Transporte HTTP  
 As associações que usam o transporte HTTP ( <xref:System.ServiceModel.WSHttpBinding> e <xref:System.ServiceModel.BasicHttpBinding> ) dão suporte a vários esquemas de autenticação, conforme explicado em [noções básicas sobre a autenticação http](understanding-http-authentication.md). O nível de representação com suporte depende do esquema de autenticação. Os seguintes itens são usados com o transporte HTTP:  
  
- O `Anonymous` esquema de autenticação ignora a representação.  
  
- O `Basic` esquema de autenticação dá suporte apenas ao `Delegate` nível. Todos os níveis de representação inferiores são atualizados.  
  
- O `Digest` esquema de autenticação dá suporte apenas aos `Impersonate` `Delegate` níveis e.  
  
- O `NTLM` esquema de autenticação, selecionável diretamente ou por meio da negociação, dá suporte apenas ao `Delegate` nível no computador local.  
  
- O esquema de autenticação Kerberos, que só pode ser selecionado por meio de negociação, pode ser usado com qualquer nível de representação com suporte.  
  
 Para obter mais informações sobre o transporte HTTP, consulte [escolhendo um transporte](choosing-a-transport.md).  
  
## <a name="see-also"></a>Confira também

- [Delegação e representação](delegation-and-impersonation-with-wcf.md)
- [Autorização](authorization-in-wcf.md)
- [Como personificar um cliente em um serviço](../how-to-impersonate-a-client-on-a-service.md)
- [Entendendo a autenticação HTTP](understanding-http-authentication.md)
