---
title: Segurança de Acesso do Código
description: Saiba mais sobre o mecanismo de segurança de acesso ao código no .NET, que ajuda a proteger sistemas de computador contra código móvel mal-intencionado.
ms.date: 03/30/2017
helpviewer_keywords:
- named permission sets, code access security
- call stacks
- malicious code
- stack walk
- security [.NET Framework], code access security
- permissions [.NET Framework], code access security
- trusted code
- mobile code security
- unmanaged code, code access security
- granting permissions, code access security
- user authentication, code access security
- code access security
ms.assetid: 859af632-c80d-4736-8d6f-1e01b09ce127
ms.openlocfilehash: 49d55ffde3dcb88720f47af6f9702013d8a7f1ee
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855862"
---
# <a name="code-access-security"></a>Segurança de Acesso do Código

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 Os sistemas de computador altamente conectados de hoje são frequentemente expostos ao código proveniente de várias fontes possivelmente desconhecidas. O código pode ser anexado a email, contido em documentos ou baixado pela Internet. Infelizmente, muitos usuários de computadores experimentaram em primeira mão os efeitos do código móvel mal-intencionado, incluindo vírus e worms, que podem danificar ou destruir dados e o tempo de custo e dinheiro.  
  
 Os mecanismos de segurança mais comuns concedem direitos aos usuários com base em suas credenciais (geralmente uma senha) e restringem os recursos (geralmente diretórios e arquivos) que o usuário tem permissão para acessar. No entanto, essa abordagem não resolve vários problemas: os usuários obtêm código de várias fontes, alguns dos quais podem não ser confiáveis; o código pode conter bugs ou vulnerabilidades que permitem que ele seja explorado por código mal-intencionado; e o código às vezes faz coisas que o usuário não sabe que ele fará. Como resultado, os sistemas de computador podem ser danificados e os dados privados podem ser vazados quando usuários cuidadosos e confiáveis executam software mal-intencionado ou preenchido por erros. A maioria dos mecanismos de segurança do sistema operacional exige que cada parte do código seja confiável para ser executada, exceto talvez para scripts em uma página da Web. Portanto, ainda há uma necessidade de um mecanismo de segurança amplamente aplicável que permite que o código proveniente de um sistema de computador seja executado com proteção em outro sistema, mesmo quando não há nenhuma relação de confiança entre os sistemas.  
  
 O .NET Framework fornece um mecanismo de segurança chamado segurança de acesso ao código para ajudar a proteger sistemas de computador contra código móvel mal-intencionado, para permitir que o código de origens desconhecidas seja executado com proteção e para ajudar a impedir que um código confiável seja comprometido intencionalmente ou acidentalmente a comprometer a segurança. A segurança de acesso ao código permite que o código seja confiável para graus variados, dependendo de onde o código se origina e em outros aspectos da identidade do código. A segurança de acesso ao código também impõe os diversos níveis de confiança no código, o que minimiza a quantidade de código que deve ser totalmente confiável para ser executado. O uso da segurança de acesso ao código pode reduzir a probabilidade de que seu código seja usado por código mal-intencionado ou com preenchimento de erro. Ele pode reduzir sua responsabilidade, pois você pode especificar o conjunto de operações que seu código deve ter permissão para executar. A segurança de acesso ao código também pode ajudar a minimizar os danos que podem resultar de vulnerabilidades de segurança em seu código.  
  
> [!NOTE]
> Alterações importantes foram feitas na segurança de acesso ao código no .NET Framework 4. A alteração mais notável foi a [transparência da segurança](security-transparent-code.md), mas também há outras alterações significativas que afetam a segurança de acesso do código. Para obter informações sobre essas alterações, consulte [Security Changes](https://docs.microsoft.com/previous-versions/dotnet/framework/security/security-changes).  
  
 A segurança de acesso ao código afeta principalmente o código da biblioteca e os aplicativos parcialmente confiáveis. Os desenvolvedores de biblioteca devem proteger seu código contra o acesso não autorizado de aplicativos parcialmente confiáveis. Aplicativos parcialmente confiáveis são aplicativos que são carregados de fontes externas, como a Internet. Os aplicativos instalados em sua área de trabalho ou na intranet local são executados com confiança total. Os aplicativos de confiança total não são afetados pela segurança de acesso ao código, a menos que estejam marcados como de [segurança transparente](security-transparent-code.md), pois são totalmente confiáveis. A única limitação para aplicativos de confiança total é que os aplicativos marcados com o <xref:System.Security.SecurityTransparentAttribute> atributo não podem chamar o código marcado com o <xref:System.Security.SecurityCriticalAttribute> atributo. Aplicativos parcialmente confiáveis devem ser executados em uma área restrita (por exemplo, no Internet Explorer) para que a segurança de acesso ao código possa ser aplicada. Se você baixar um aplicativo da Internet e tentar executá-lo a partir de sua área de trabalho, receberá uma <xref:System.NotSupportedException> com a mensagem: "foi feita uma tentativa de carregar um assembly de um local de rede, o que faria com que o assembly estivesse na área restrita em versões anteriores do .NET Framework. Essa versão do .NET Framework não habilita a política de CAS por padrão, portanto, essa carga pode ser perigosa. " Se você tiver certeza de que o aplicativo pode ser confiável, poderá habilitá-lo para ser executado como confiança total usando o [ \<loadFromRemoteSources> elemento](../configure-apps/file-schema/runtime/loadfromremotesources-element.md). Para obter informações sobre como executar um aplicativo em uma área restrita, consulte [como: executar código parcialmente confiável em uma área restrita](how-to-run-partially-trusted-code-in-a-sandbox.md).  
  
 Todo o código gerenciado que tem como alvo o Common Language Runtime recebe os benefícios da segurança de acesso ao código, mesmo que esse código não faça uma chamada de segurança de acesso de código única. Para obter mais informações, consulte [Noções Básicas da Segurança de Acesso do Código](code-access-security-basics.md).  
  
<a name="key_functions"></a>
## <a name="key-functions-of-code-access-security"></a>Principais funções de segurança de acesso a código  
 A segurança de acesso ao código ajuda a limitar o acesso que o código tem a recursos e operações protegidos. No .NET Framework, a segurança de acesso ao código executa as seguintes funções:  
  
- Define permissões e conjuntos de permissões que representam o direito de acessar vários recursos do sistema.  
  
- Permite que o código exija que seus chamadores tenham permissões específicas.  
  
- Permite que o código exija que seus chamadores possuam uma assinatura digital, permitindo que somente os chamadores de uma determinada organização ou site chamem o código protegido.  
  
- Impõe restrições no código em tempo de execução, comparando as permissões concedidas de cada chamador na pilha de chamadas para as permissões que os chamadores devem ter.  
  
<a name="walking_the_call_stack"></a>
## <a name="walking-the-call-stack"></a>Percorrendo a pilha de chamadas  
 Para determinar se o código está autorizado a acessar um recurso ou executar uma operação, o sistema de segurança do tempo de execução percorre a pilha de chamadas, comparando as permissões concedidas de cada chamador à permissão que está sendo solicitada. Se qualquer chamador na pilha de chamadas não tiver a permissão exigida, uma exceção de segurança será gerada e o acesso será recusado. A movimentação de pilha foi projetada para ajudar a evitar ataques chamariz, em que código menos confiável chama código altamente confiável e o utiliza para executar ações não autorizadas. As permissões exigentes de todos os chamadores em tempo de execução afetam o desempenho, mas é essencial ajudar a proteger o código contra ataques chamariz por código menos confiável. Para otimizar o desempenho, você pode fazer com que seu código execute menos movimentações de pilha. No entanto, certifique-se de não expor um ponto fraco de segurança sempre que fizer isso.  
  
 A ilustração a seguir mostra a movimentação de pilha que resulta quando um método no assembly A4 exige que seus chamadores tenham a permissão P.  
  
 ![Movimentação de pilha de segurança de acesso ao código](media/slide-10a.gif "slide_10a")
  
<a name="related_topics"></a>
## <a name="related-articles"></a>Artigos relacionados
  
|Título|Descrição|  
|-----------|-----------------|  
|[Noções básicas da segurança de acesso do código](code-access-security-basics.md)|Descreve a segurança de acesso do código e seus usos mais comuns.|  
|[Código transparente de segurança, nível 2](security-transparent-code-level-2.md)|Descreve o modelo de transparência de segurança no .NET Framework 4.|  
|[Usando bibliotecas de código parcialmente confiável](using-libraries-from-partially-trusted-code.md)|Descreve como habilitar bibliotecas para uso com código não gerenciado e como usar bibliotecas de código não gerenciado.|  
|[Conceitos principais de segurança](../../standard/security/key-security-concepts.md)|Fornece uma visão geral de muitos dos principais termos e conceitos usados no sistema de segurança .NET Framework.|  
|[Segurança baseada em função](../../standard/security/role-based-security.md)|Descreve como incorporar a segurança com base em funções.|  
|[Serviços criptográficos](../../standard/security/cryptographic-services.md)|Descreve como incorporar a criptografia em seus aplicativos.|
