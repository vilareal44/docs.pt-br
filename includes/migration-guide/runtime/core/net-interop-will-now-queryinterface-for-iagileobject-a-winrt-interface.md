---
ms.openlocfilehash: 150b94b55fa8f2a29f1da7cf7ac7bf6dd06b9666
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621910"
---
### <a name="net-interop-will-now-queryinterface-for-iagileobject-a-winrt-interface"></a>A Interoperabilidade do .NET agora fará QueryInterface para IAgileObject (uma interface do WinRT)

#### <a name="details"></a>Detalhes

Ao usar um evento de WinRT com um delegado do .NET, Windows fará QI para IAgileObject começando com o .NET Framework 4.8.  Nas versões anteriores do .NET Framework, o runtime falharia nessa QI e o evento não poderia ser assinado.<ul><li>[x] Quirked</li></ul>

#### <a name="suggestion"></a>Sugestão

Se a habilitação de QI para IAgileObject interromper a execução, você poderá desabilitar esse código definindo a configuração a seguir. <h4>Método 1: variável de ambiente</h4> Defina a variável de ambiente a seguir:COMPLUS_DisableCCWSupportIAgileObject=1Esse método afeta qualquer ambiente que herda essa variável de ambiente. Isso pode ser apenas uma única sessão de console ou pode afetar o computador inteiro se você definir a variável de ambiente globalmente. O nome da variável de ambiente não diferencia maiúsculas de minúsculas. <h4>Método 2: registro</h4> Usando o editor do registro (regedit.exe), localize uma das seguintes subchaves: HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft.NETFramework HKEY_CURRENT_USER \SOFTWARE\Microsoft.NETFrameworkThen adicione o seguinte: nome do valor: DisableCCWSupportIAgileObject tipo: DWORD (32-bit) valor (também chamado de REG_WORD) valor: 1Você pode usar a ferramenta de REG.EXE do Windows para adicionar esse valor de um ambiente de linha de comando ou de script. Por exemplo:<pre><code class="lang-console">reg add HKLM\SOFTWARE\Microsoft\.NETFramework /v DisableCCWSupportIAgileObject /t REG_DWORD /d 1&#13;&#10;</code></pre>Nesse caso, <code>HKLM</code> é usado em vez de <code>HKEY_LOCAL_MACHINE</code>. Use <code>reg add /?</code> para ver a ajuda nessa sintaxe. O nome do valor do registro não diferencia maiúsculas de minúsculas.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.8|
|Type|Runtime|
