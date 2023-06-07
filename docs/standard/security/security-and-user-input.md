---
title: Segurança e entrada do usuário
description: Seu código pode passar dados inseridos pelo usuário como parâmetros para outro código, o que pode afetar a segurança. Você pode fazer a verificação de intervalo para rejeitar a entrada problemática.
ms.date: 07/15/2020
ms.technology: dotnet-standard
helpviewer_keywords:
- security [.NET], user input
- user input, security
- secure coding, user input
- code security, user input
ms.assetid: 9141076a-96c9-4b01-93de-366bb1d858bc
ms.openlocfilehash: e46bf8e653567637b4e6236849981fdb32df447c
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555936"
---
# <a name="security-and-user-input"></a>Segurança e entrada do usuário

Os dados do usuário, que são qualquer tipo de entrada (dados de uma solicitação da Web ou URL, entrada para controles de um aplicativo Microsoft Windows Forms e assim por diante) podem influenciar o código de forma adversa, pois geralmente esses dados são usados diretamente como parâmetros para chamar outro código. Essa situação é análoga ao código mal-intencionado que chama seu código com parâmetros estranhos e as mesmas precauções devem ser tomadas. A entrada do usuário é realmente mais difícil de se tornar segura, pois não há um quadro de pilha para rastrear a presença dos dados potencialmente não confiáveis.

Esses são os bugs de segurança mais sutis e difíceis de serem encontrados porque, embora possam existir em código que pareça não estar relacionado à segurança, eles são um gateway para passar dados inválidos para outro código. Para procurar esses bugs, siga qualquer tipo de dado de entrada, imagine o que é o intervalo de possíveis valores e considere se o código que vê esses dados pode lidar com todos esses casos. Você pode corrigir esses bugs por meio da verificação de intervalo e rejeitar qualquer entrada que o código não possa manipular.

Algumas considerações importantes envolvendo os dados do usuário incluem o seguinte:

- Qualquer dado de usuário em uma resposta de servidor é executado no contexto do site do servidor no cliente. Se o seu servidor Web usa dados de usuário e os insere na página da Web retornada, ele pode, por exemplo, incluir uma **\<script>** marca e executar como se fosse do servidor.

- Lembre-se de que o cliente pode solicitar qualquer URL.

- Considere caminhos complicados ou inválidos:

  - .. \, caminhos extremamente longos.

  - Uso de caracteres curinga (*).

  - Expansão de token (% token%).

  - Formas estranhas de caminhos com significado especial.

  - Nomes de fluxo do sistema de arquivos alternativos, como `filename::$DATA` .

  - Versões curtas de nomes de arquivo, como `longfi~1` para `longfilename` .

- Lembre-se de que Eval (UserData) pode fazer qualquer coisa.

- Tenha cuidado com a associação tardia a um nome que inclua alguns dados do usuário.

- Se você estiver lidando com dados da Web, considere as várias formas de escapes que são permitidas, incluindo:

  - Escapes hexadecimais (% nn).

  - Escapes Unicode (% nnn).

  - Escapes UTF-8 longos (% nn% NN).

  - Escapes duplos (% nn torna-se% mmnn, em que% mm é o escape para '% ').

- Tenha cuidado com nomes de usuário que possam ter mais de um formato canônico. Por exemplo, geralmente você pode usar o formulário de nome de usuário mydomain \\ *username* ou o *nome de usuário* @mydomain.example.com .

## <a name="see-also"></a>Confira também

- [Diretrizes de codificação segura](secure-coding-guidelines.md)
- [Segurança de ASP.NET Core](/aspnet/core/security/)
