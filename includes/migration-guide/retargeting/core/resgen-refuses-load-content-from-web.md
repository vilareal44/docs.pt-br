---
ms.openlocfilehash: f980c8029375074889505a8eb7e8a65aaa8d74e4
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614292"
---
### <a name="resgen-refuses-to-load-content-from-the-web"></a>O Resgen recusa a carregar conteúdo da Web

#### <a name="details"></a>Detalhes

arquivos. resx podem conter entrada binária formatada. Se você tentar usar Resgen para carregar um arquivo que foi baixado de um local não confiável, ele falhará ao carregar a entrada por padrão.

#### <a name="suggestion"></a>Sugestão

Os usuários ResGen que exigem o carregamento de entradas formatadas binárias de locais não confiáveis podem remover a marca da Web do arquivo de entrada ou aplicar a sutileza da recusa. Adicione a seguinte configuração do registro para aplicar a peculiarização de aceitação por todo o computador: [HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft.netframework\sdk] &quot; AllowProcessOfUntrustedResourceFiles &quot; = &quot; true&quot;

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7.2       |
| Type    | Redirecionando |
