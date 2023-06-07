---
ms.openlocfilehash: a5c6dda0c1d68468cd95f67716709dd059948c80
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621038"
---
### <a name="product-versioning-changes-in-the-net-framework-46-and-later-versions"></a>Alterações de controle de versão de produto no .NET Framework 4.6 e versões posteriores

#### <a name="details"></a>Detalhes

O controle de versão do produto foi alterado nas versões anteriores do .NET Framework, especialmente do .NET Framework 4, 4.5, 4.5.1 e 4.5.2. Veja a seguir as alterações em detalhes:<ul><li>O valor da entrada <code>Version</code> na chave <code>HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full</code> foi alterado para <code>4.6.xxxxx</code> para o .NET Framework 4.6 e suas versões pontuais, e para <code>4.7.xxxxx</code> para o .NET Framework 4.7 e 4.7.1. No .NET Framework 4.5, 4.5.1 e 4.5.2, ele tinha o formato <code>4.5.xxxxx</code>.</li><li>O controle de versão de arquivo e produto para arquivos do .NET Framework foi alterado do esquema de controle de versão anterior de 4.0.30319.x para 4.6.X.0 para o .NET Framework 4.6 e suas versões pontuais, e para 4.7.X.0 para o .NET Framework 4.7 e 4.7.1. Você pode ver esses novos valores quando exibe as Propriedades do arquivo depois de clicar com o botão direito do mouse em um arquivo.</li><li>Os atributos <xref:System.Reflection.AssemblyFileVersionAttribute> e <xref:System.Reflection.AssemblyInformationalVersionAttribute> para assemblies gerenciados têm valores de Versão no formato 4.6.X.0 para o .NET Framework 4.6 e suas versões pontuais, e 4.7.X.0 para o .NET Framework 4.7 e 4.7.1.</li><li>No .NET Framework 4.6, 4.6.1, 4.6.2, 4.7 e 4.7.1, a propriedade <xref:System.Environment.Version?displayProperty=nameWithType> retorna a cadeia de caracteres de versão fixa <code>4.0.30319.42000</code>. No .NET Framework 4, 4.5, 4.5.1 e 4.5.2, ela retorna as cadeias de caracteres de versão no formato <code>4.0.30319.xxxxx</code> (por exemplo, &quot;4.0.30319.18010&quot;). Não é recomendável criar nova dependência de código de aplicativo na propriedade Environment.Version.</li></ul>Para obter mais informações, consulte [Como determinar quais versões do .NET Framework estão instaladas](~/docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md).

#### <a name="suggestion"></a>Sugestão

Em geral, os aplicativos devem depender das técnicas recomendadas para detecção de itens como a versão de runtime do .NET Framework e o diretório de instalação:<ul><li>Para detectar a versão de runtime do .NET Framework, confira [How to: Determine Which .NET Framework Versions Are Installed](~/docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md) (Como determinar quais versões do .NET Framework estão instaladas).</li><li>Para determinar o caminho de instalação do .NET Framework, use o valor da entrada <code>InstallPath</code> na chave <code>HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full</code>.</li></ul> <blockquote> [!IMPORTANT] O nome da subchave é <code>NET Framework Setup</code>, e não <code>.NET Framework Setup</code>.</blockquote> <ul><li>Para determinar o caminho do diretório do Common Language Runtime do .NET Framework, chame o método <xref:System.Runtime.InteropServices.RuntimeEnvironment.GetRuntimeDirectory?displayProperty=nameWithType>.</li><li>Para obter a versão do CLR, chame o método <xref:System.Runtime.InteropServices.RuntimeEnvironment.GetSystemVersion?displayProperty=nameWithType>. Para o .NET Framework 4 e suas versões pontuais (o .NET Framework 4.5, 4.5.1, 4.5.2 e o .NET Framework 4.6, 4.6.1, 4.6.2, 4.7 e 4.7.1), ele retorna a cadeia de caracteres v4.0.30319.</li></ul>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.6|
|Type|Runtime|
