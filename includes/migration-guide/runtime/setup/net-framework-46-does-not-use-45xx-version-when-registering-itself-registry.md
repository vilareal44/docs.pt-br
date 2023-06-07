---
ms.openlocfilehash: 09fb7a54fccd5cf37800483c64e2fa6a54681f11
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621036"
---
### <a name="the-net-framework-46-does-not-use-a-45xx-version-when-registering-itself-in-the-registry"></a>.NET Framework 4.6 não usa uma versão 4.5.x.x ao se registrar no registro

#### <a name="details"></a>Detalhes

Como é de se esperar, a chave de versão definida no registro (em <code>HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\NET Framework Setup\NDP\v4\Full</code>) do .NET Framework 4.6 começa com "4.6", e não "4.5". Os aplicativos que dependem dessas chaves do Registro para saber quais versões do .NET Framework estão instaladas em um computador precisam ser atualizados para compreenderem que 4.6 é uma nova versão possível, compatível com as versões 4.5.x anteriores.

#### <a name="suggestion"></a>Sugestão

Atualize os aplicativos que estão investigando uma instalação do .NET Framework 4.5 procurando por chaves do Registro 4.5 que também aceitam 4.6.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.6|
|Type|Runtime|
