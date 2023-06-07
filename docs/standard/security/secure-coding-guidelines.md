---
title: Diretrizes de codificação segura para .NET
description: Código de design com o qual trabalhar. As permissões de rede imposta e outras imposição para ajudar a impedir que códigos mal-intencionados acessem dados ou executem outras ações.
ms.date: 07/15/2020
helpviewer_keywords:
- managed wrapper to native code implementation
- secure coding
- reusable components
- library code that exposes protected resources
- code, security
- code security
- secure coding, options
- components [.NET], security
- code security, options
- security-neutral code
- security [.NET], coding guidelines
ms.assetid: 4f882d94-262b-4494-b0a6-ba9ba1f5f177
ms.openlocfilehash: a05e0cec2814be88ac835d05601d5cf5f66383c3
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555950"
---
# <a name="secure-coding-guidelines"></a>Diretrizes de codificação segura

A maioria dos códigos de aplicativo pode simplesmente usar a infraestrutura implementada pelo .NET. Em alguns casos, é necessária uma segurança adicional específica do aplicativo, criada por meio da extensão do sistema de segurança ou usando novos métodos ad hoc.

Usando as permissões impostas do .NET e outras imposição em seu código, você deve colocar barreiras para impedir que um código mal-intencionado acesse informações que você não deseja que ele tenha ou execute outras ações indesejadas. Além disso, você deve ter um equilíbrio entre segurança e usabilidade em todos os cenários esperados usando código confiável.

Esta visão geral descreve as diferentes maneiras como o código pode ser projetado para funcionar com o sistema de segurança.

## <a name="securing-resource-access"></a>Protegendo o acesso aos recursos

Ao projetar e escrever seu código, você precisa proteger e limitar o acesso que o código tem aos recursos, especialmente ao usar ou invocar o código de origem desconhecida. Portanto, tenha em mente as seguintes técnicas para garantir que seu código seja seguro:

- Não use a CAS (segurança de acesso do código).

- Não use código parcialmente confiável.

- Não use o atributo [AllowPartiallyTrustedCaller](xref:System.Security.AllowPartiallyTrustedCallersAttribute) (APTCA).

- Não use a comunicação remota do .NET.

- Não use Component Object Model distribuída (DCOM).

- Não use formatadores binários.

Segurança de acesso a código e segurança-o código transparent não tem suporte como um limite de segurança com código parcialmente confiável. Não aconselhamos carregar e executar códigos de origens desconhecidas sem a adoção de medidas de segurança alternativas no local. As medidas de segurança alternativas são:

- Virtualização

- AppContainers

- Usuários e permissões do sistema operacional (SO)

- Contêineres do Hyper-V

## <a name="security-neutral-code"></a>Segurança-código neutro

O código de segurança neutra não faz nada de explícito com o sistema de segurança. Ele é executado com qualquer permissão que receber. Embora os aplicativos que não consigam capturar as exceções de segurança associadas a operações protegidas (como usar arquivos, redes e assim por diante) possam resultar em uma exceção sem tratamento, o código de segurança neutra ainda aproveita as tecnologias de segurança no .NET.

Uma biblioteca com segurança neutra tem características especiais que você deve entender. Suponha que sua biblioteca forneça elementos de API que usam arquivos ou chamam código não gerenciado. Se o seu código não tiver a permissão correspondente, ele não será executado conforme descrito. No entanto, mesmo que o código tenha a permissão, qualquer código de aplicativo que o chame deve ter a mesma permissão para funcionar. Se o código de chamada não tiver a permissão correta, um <xref:System.Security.SecurityException> será exibido como resultado da movimentação da pilha de segurança de acesso ao código.

## <a name="application-code-that-isnt-a-reusable-component"></a>Código do aplicativo que não é um componente reutilizável

Se seu código fizer parte de um aplicativo que não será chamado por outro código, a segurança será simples e a codificação especial poderá não ser necessária. No entanto, lembre-se de que o código mal-intencionado pode chamar seu código. Embora a segurança de acesso ao código possa impedir que o código mal-intencionado acesse recursos, esse código ainda pode ler valores de seus campos ou propriedades que podem conter informações confidenciais.

Além disso, se o seu código aceitar entrada do usuário da Internet ou de outras fontes não confiáveis, você deverá ter cuidado com a entrada mal-intencionada.

## <a name="managed-wrapper-to-native-code-implementation"></a>Wrapper gerenciado para implementação de código nativo

Normalmente, nesse cenário, algumas funcionalidades úteis são implementadas no código nativo que você deseja disponibilizar para o código gerenciado. Wrappers gerenciados são fáceis de escrever usando a invocação de plataforma ou a interoperabilidade COM. No entanto, se você fizer isso, os chamadores de seus wrappers deverão ter direitos de código não gerenciados para serem bem-sucedidos. Na política padrão, isso significa que o código baixado de uma intranet ou da Internet não funcionará com os wrappers.

Em vez de fornecer direitos de código não gerenciado a todos os aplicativos que usam esses wrappers, é melhor fornecer esses direitos somente ao código do wrapper. Se a funcionalidade subjacente não expõe recursos e a implementação é segura, o wrapper só precisa declarar seus direitos, o que permite que qualquer código chame através dele. Quando os recursos estão envolvidos, a codificação de segurança deve ser a mesma do caso de código de biblioteca descrito na próxima seção. Como o wrapper está potencialmente expondo chamadores a esses recursos, a verificação cuidadosa da segurança do código nativo é necessária e é responsabilidade do wrapper.

## <a name="library-code-that-exposes-protected-resources"></a>Código de biblioteca que expõe recursos protegidos

A abordagem a seguir é a mais poderosa e, portanto, potencialmente perigosa (se tiver sido feita incorretamente) para codificação de segurança: sua biblioteca serve como uma interface para que outro código acesse determinados recursos que não estão disponíveis de outra forma, assim como as classes .NET impõem permissões para os recursos que usam. Sempre que você expor um recurso, seu código deve primeiro solicitar a permissão apropriada para o recurso (ou seja, ele deve executar uma verificação de segurança) e, em seguida, declarar seus direitos para executar a operação real.

## <a name="see-also"></a>Confira também

- [Protegendo dados de estado](securing-state-data.md)
- [Segurança e entrada do usuário](security-and-user-input.md)
- [Segurança e condições de corrida](security-and-race-conditions.md)
- [Segurança e geração de código durante a execução](security-and-on-the-fly-code-generation.md)
- [Segurança baseada em função](role-based-security.md)
- [Segurança de ASP.NET Core](/aspnet/core/security/)
