---
title: Protegendo código de wrapper
description: Examine como proteger o código wrapper, que pode abrir um conjunto exclusivo de pontos fracos de segurança, especialmente se o wrapper tiver maior confiança do que o código que o utiliza.
ms.date: 03/30/2017
helpviewer_keywords:
- security [.NET Framework], wrapper code
- wrapper code, securing
- secure coding, wrapper code
- code security, wrapper code
ms.assetid: 1df6c516-5bba-48bd-b450-1070e04b7389
ms.openlocfilehash: f448cbf55f3ad992ba9dcc53d5be70b364038744
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855745"
---
# <a name="securing-wrapper-code"></a>Protegendo código de wrapper

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 Código do wrapper, especialmente onde o wrapper tem confiança maior do que o código que o utiliza, pode abrir um conjunto exclusivo de pontos fracos de segurança. Qualquer coisa feita em nome de um chamador, em que as permissões limitadas do chamador não estão incluídas na verificação de segurança apropriada, é uma possível vulnerabilidade a ser explorada.  
  
 Nunca habilite algo por meio do wrapper que o chamador não podia fazer em si mesmo. Esse é um perigo especial ao fazer algo que envolve uma verificação de segurança limitada, em oposição a uma demanda completa de movimentação de pilha. Quando as verificações de nível único estão envolvidas, a interposição do código do wrapper entre o chamador real e o elemento de API em questão pode fazer com que a verificação de segurança tenha sucesso quando não deveria, diminuindo assim a segurança.  
  
## <a name="delegates"></a>Delegados  
 A segurança de delegado difere entre as versões do .NET Framework.  Esta seção descreve os diferentes comportamentos de delegado e as considerações de segurança associadas.  
  
### <a name="in-version-10-and-11-of-the-net-framework"></a>Na versão 1,0 e 1,1 do .NET Framework  
 A versão 1,0 e 1,1 do .NET Framework executar as seguintes ações de segurança em relação a um criador delegado e um chamador delegado.  
  
- Quando um delegado é criado, as demandas de link de segurança no método de destino delegado são executadas em relação ao conjunto de concessão do criador delegado.  Falha ao satisfazer os resultados da ação de segurança em um <xref:System.Security.SecurityException> .  
  
- Quando o delegado é invocado, todas as demandas de segurança existentes no chamador delegado são executadas.  
  
 Sempre que seu código usa um <xref:System.Delegate> código de menos confiável que pode chamá-lo, certifique-se de que você não está habilitando o código menos confiável para escalonar suas permissões. Se você pegar um delegado e usá-lo mais tarde, o código que criou o delegado não estará na pilha de chamadas e suas permissões não serão testadas se o código em ou sob o delegado tentar uma operação protegida. Se o código e o código do chamador tiverem privilégios mais altos do que o criador, o criador poderá orquestrar o caminho de chamada sem fazer parte da pilha de chamadas.  
  
### <a name="in-version-20-and-later-versions-of-the-net-framework"></a>Na versão 2,0 e versões posteriores do .NET Framework  
 Diferentemente das versões anteriores, a versão 2,0 e versões posteriores do .NET Framework executa a ação de segurança no criador de delegado quando o delegado é criado e chamado.  
  
- Quando um delegado é criado, as demandas de link de segurança no método de destino delegado são executadas em relação ao conjunto de concessão do criador delegado.  Falha ao satisfazer os resultados da ação de segurança em um <xref:System.Security.SecurityException> .  
  
- O conjunto de concessão do criador delegado também é capturado durante a criação de delegado e armazenado com o delegado.  
  
- Quando o delegado é invocado, o conjunto de concessão capturado do criador delegado é avaliado primeiro em relação a quaisquer demandas no contexto atual se o criador delegado e o chamador pertencerem a diferentes assemblies.  Em seguida, todas as demandas de segurança existentes no chamador delegado são executadas.  
  
## <a name="link-demands-and-wrappers"></a>Demandas de link e wrappers  
 Um caso de proteção especial com demandas de link foi reforçado na infraestrutura de segurança, mas ainda é uma fonte de possível fraqueza em seu código.  
  
 Se o código totalmente confiável chamar uma propriedade, evento ou método protegido por um [LinkDemand](link-demands.md), a chamada terá sucesso se a verificação de permissão de **LinkDemand** para o chamador for satisfeita. Além disso, se o código totalmente confiável expõe uma classe que usa o nome de uma propriedade e chama seu acessador **Get** usando reflexão, essa chamada para o acessador **Get** é bem sucedido, embora o código do usuário não tenha o direito de acessar essa propriedade. Isso ocorre porque o **LinkDemand** verifica apenas o chamador imediato, que é o código totalmente confiável. Em essência, o código totalmente confiável é fazer uma chamada privilegiada em nome do código do usuário sem ter certeza de que o código do usuário tem o direito de fazer essa chamada.  
  
 Para ajudar a evitar tais brechas de segurança, a Common Language Runtime estende o check-in para uma demanda completa de movimentação de pilha em qualquer chamada indireta para um método, Construtor, propriedade ou evento protegido por um **LinkDemand**. Essa proteção incorre em alguns custos de desempenho e altera a semântica da verificação de segurança; a demanda completa de movimentação de pilha pode falhar onde a verificação mais rápida e de um nível teria passado.  
  
## <a name="assembly-loading-wrappers"></a>Wrappers de carregamento de assembly  
 Vários métodos usados para carregar código gerenciado, incluindo <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> , carregam assemblies com a evidência do chamador. Se você encapsular qualquer um desses métodos, o sistema de segurança poderá usar a concessão de permissão do código, em vez das permissões do chamador para o seu wrapper, para carregar os assemblies. Não permita código menos confiável para carregar código que receba permissões mais altas do que aquelas do chamador para o seu wrapper.  
  
 Qualquer código que tenha confiança total ou confiança significativamente maior do que um chamador potencial (incluindo um chamador no nível de permissões da Internet) poderia enfraquecer a segurança dessa maneira. Se o seu código tiver um método público que usa uma matriz de bytes e o passará para **assembly. Load**, criando, assim, um assembly em nome do chamador, ele poderá interromper a segurança.  
  
 Esse problema se aplica aos seguintes elementos de API:  
  
- <xref:System.AppDomain.DefineDynamicAssembly%2A?displayProperty=nameWithType>  
  
- <xref:System.AppDomain.Load%2A?displayProperty=nameWithType>  
  
- <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>  
  
- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>  
  
## <a name="demand-vs-linkdemand"></a>Demand versus LinkDemand  
 A segurança declarativa oferece dois tipos de verificações de segurança que são semelhantes, mas executam verificações diferentes. É bom entender os dois formulários, pois a escolha errada pode resultar em perda de segurança ou de desempenho fraca.  
  
 A segurança declarativa oferece as seguintes verificações de segurança:  
  
- <xref:System.Security.Permissions.SecurityAction.Demand>Especifica a movimentação da pilha de segurança de acesso ao código. Todos os chamadores na pilha devem ter a permissão ou a identidade especificada para passar. A **demanda** ocorre em cada chamada porque a pilha pode conter chamadores diferentes. Se você chamar um método repetidamente, essa verificação de segurança ocorrerá a cada vez. A **demanda** é uma boa proteção contra ataques chamariz; o código não autorizado que está tentando passar por ele será detectado.  
  
- [LinkDemand](link-demands.md) ocorre no tempo de compilação JIT (just-in-time) e verifica apenas o chamador imediato. Essa verificação de segurança não verifica o chamador do chamador. Quando essa verificação é aprovada, não há nenhuma sobrecarga de segurança adicional, não importa quantas vezes o chamador pode chamar. No entanto, também não há nenhuma proteção contra ataques chamariz. Com **LinkDemand**, qualquer código que passa pelo teste e pode fazer referência a seu código pode potencialmente interromper a segurança, permitindo que o código mal-intencionado chame usando o código autorizado. Portanto, não use **LinkDemand** , a menos que todos os possíveis pontos fracos possam ser totalmente evitados.  
  
    > [!NOTE]
    > No .NET Framework 4, as demandas de link foram substituídas pelo <xref:System.Security.SecurityCriticalAttribute> atributo em <xref:System.Security.SecurityRuleSet.Level2> assemblies. O <xref:System.Security.SecurityCriticalAttribute> é equivalente a uma demanda de link para confiança total; no entanto, ele também afeta as regras de herança. Para obter mais informações sobre essa alteração, consulte [código de segurança transparente, nível 2](security-transparent-code-level-2.md).  
  
 As precauções extras necessárias ao usar **LinkDemand** devem ser programadas individualmente; o sistema de segurança pode ajudar na imposição. Qualquer erro abre uma vulnerabilidade de segurança. Todo o código autorizado que usa seu código deve ser responsável por implementar segurança adicional fazendo o seguinte:  
  
- Restringir o acesso do código de chamada à classe ou ao assembly.  
  
- Colocar as mesmas verificações de segurança no código de chamada que aparecem no código que está sendo chamado e obligating seus chamadores para fazer isso. Por exemplo, se você escrever código que chama um método que é protegido com um **LinkDemand** para o <xref:System.Security.Permissions.SecurityPermission> com o <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode> sinalizador especificado, o método também deve fazer um **LinkDemand** (ou **demanda**, que é mais forte) para essa permissão. A exceção é se o seu código usa o método protegido por **LinkDemand**de uma maneira limitada que você decide que é seguro, dado a outros mecanismos de proteção de segurança (como demandas) em seu código. Nesse caso excepcional, o chamador assume a responsabilidade de enfraquecer a proteção de segurança no código subjacente.  
  
- Garantir que os chamadores de seu código não possam enganar seu código para chamar o código protegido em seu nome. Em outras palavras, os chamadores não podem forçar o código autorizado a passar parâmetros específicos para o código protegido ou para obter os resultados de volta dele.  
  
### <a name="interfaces-and-link-demands"></a>Interfaces e demandas de link  
 Se um método virtual, uma propriedade ou um evento com **LinkDemand** substituir um método de classe base, o método de classe base também deverá ter o mesmo **LinkDemand** para o método substituído a fim de entrar em vigor. É possível que o código mal-intencionado seja convertido no tipo base e chame o método da classe base. Observe também que as demandas de link podem ser adicionadas implicitamente a assemblies que não têm o <xref:System.Security.AllowPartiallyTrustedCallersAttribute> atributo de nível de assembly.  
  
 É uma boa prática proteger as implementações de método com demandas de link quando métodos de interface também têm demandas de link. Observe o seguinte sobre o uso de demandas de link com interfaces:  
  
- Se você posicionar um **LinkDemand** em um método público de uma classe que implementa um método de interface, o **LinkDemand** não será imposto se você converter para a interface e chamar o método. Nesse caso, como você se vinculou à interface, somente o **LinkDemand** na interface é respeitado.  
  
 Examine os seguintes itens para problemas de segurança:  
  
- Demandas de link explícitas em métodos de interface. Verifique se essas demandas de link oferecem a proteção esperada. Determine se o código mal-intencionado pode usar uma conversão para contornar as demandas de link conforme descrito anteriormente.  
  
- Métodos virtuais com demandas de link aplicadas.  
  
- Tipos e as interfaces que eles implementam. Eles devem usar as demandas de link de forma consistente.  
  
## <a name="see-also"></a>Veja também

- [Diretrizes de codificação segura](../../standard/security/secure-coding-guidelines.md)
