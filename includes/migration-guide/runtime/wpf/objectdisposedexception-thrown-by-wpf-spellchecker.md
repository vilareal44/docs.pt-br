---
ms.openlocfilehash: b836b26f3f52e9d0cc78feb764629bd2fa306657
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621767"
---
### <a name="objectdisposedexception-thrown-by-wpf-spellchecker"></a>ObjectDisposedException gerada pelo verificador ortográfico do WPF

#### <a name="details"></a>Detalhes

Ocasionalmente, os aplicativos WPF falham durante o desligamento com uma <xref:System.ObjectDisposedException?displayProperty=fullName> gerada pelo verificador ortográfico. Isso foi corrigido no WPF do .NET Framework 4.7 por meio do tratamento normal da exceção, garantindo que os aplicativos não sejam mais prejudicados. Observe que exceções de primeira chance ocasionais continuariam a ser observadas em aplicativos em execução em um depurador.

#### <a name="suggestion"></a>Sugestão

Atualizar para o .NET Framework 4.7

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.6.1|
|Type|Runtime|
