---
title: Práticas recomendadas para o desenvolvimento de aplicativos prontos para internacionalização
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- global applications, best practices
- world-ready applications, best practices
- globalization [.NET Framework], best practices
- international applications [.NET Framework], best practices
ms.assetid: f08169c7-aad8-4ec3-9a21-9ebd3b89986c
ms.openlocfilehash: f0e5ccf999b6aa96b6317b88e25f3cd9d9fbc899
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84279874"
---
# <a name="best-practices-for-developing-world-ready-applications"></a>Práticas recomendadas para o desenvolvimento de aplicativos preparados para globalização

Essa seção descreve as práticas recomendadas a serem seguidas no desenvolvimento de aplicativos prontos para o mundo.

## <a name="globalization-best-practices"></a>Práticas recomendadas de globalização

1. Crie o aplicativo Unicode internamente.

2. Use as classes com reconhecimento de cultura fornecidas pelo namespace <xref:System.Globalization> para manipular e formatar dados.

    - Para classificar, use a classe <xref:System.Globalization.SortKey> e a classe <xref:System.Globalization.CompareInfo>.

    - Para comparações de cadeia de caracteres, use a classe <xref:System.Globalization.CompareInfo>.

    - Para formatação de data e hora, use a classe <xref:System.Globalization.DateTimeFormatInfo>.

    - Para formatação numérica, use a classe <xref:System.Globalization.NumberFormatInfo>.

    - Para calendários gregoriano e não gregorianos, use a classe <xref:System.Globalization.Calendar> ou uma das implementações de calendário específicas.

3. Em determinadas situações, use as configurações das propriedades de cultura fornecidas pela classe <xref:System.Globalization.CultureInfo?displayProperty=nameWithType>. Use a propriedade <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> para formatar tarefas, como a data e a hora, ou a formatação numérica. Use a propriedade <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> para recuperar recursos. Observe que as propriedades `CurrentCulture` e `CurrentUICulture` podem ser definidas por threads.

4. Habilite seu aplicativo para ler e gravar dados em várias codificações usando as classes de codificação no namespace <xref:System.Text>. Não aceite dados ASCII. Suponha que caracteres internacionais sejam fornecidos em todos os lugares que um usuário possa inserir texto. Por exemplo, o aplicativo deve aceitar caracteres internacionais em nomes de servidor, diretórios, nomes de arquivos, nomes de usuário e URLs.

5. Ao usar a classe <xref:System.Text.UTF8Encoding>, por motivos de segurança, use o recurso de detecção de erros oferecido pela classe. Para ativar o recurso de detecção de erro, crie uma instância da classe usando o construtor que usa um parâmetro `throwOnInvalidBytes` e defina o valor desse parâmetro como `true`.

6. Sempre que possível, trate as cadeias de caracteres como cadeias de caracteres inteiras em vez de tratá-las como uma série de caracteres individuais. Isso é especialmente importante durante a classificação ou a pesquisa de subcadeias de caracteres. Isso evitará problemas associados à análise de caracteres combinados. Você também pode trabalhar com unidades de texto em vez de com caracteres únicos usando a classe <xref:System.Globalization.StringInfo?displayProperty=nameWithType>.

7. Exiba o texto usando as classes fornecidas pelo namespace <xref:System.Drawing>.

8. Para ser consistente entre sistemas operacionais, não permita que as configurações de usuário substituam <xref:System.Globalization.CultureInfo>. Use o constructo `CultureInfo` que aceita um parâmetro `useUserOverride` e define-o como `false`.

9. Teste a funcionalidade de seu aplicativo em versões de sistemas operacionais internacionais usando dados internacionais.

10. Se uma decisão de segurança for baseada no resultado de uma comparação de cadeia de caracteres ou de uma operação de alteração entre maiúsculas e minúsculas, use uma operação de cadeia de caracteres sem detecção de cultura. Essa prática garante que o resultado não seja afetado pelo valor de `CultureInfo.CurrentCulture`. Confira a seção ["Comparações de cadeia de caracteres que usam a cultura atual"](../base-types/best-practices-strings.md#string-comparisons-that-use-the-current-culture) das [Práticas recomendadas para usar cadeias de caracteres](../base-types/best-practices-strings.md) para obter um exemplo que demonstra como as comparações de cadeias de caracteres com detecção de cultura podem produzir resultados inconsistentes.

## <a name="localization-best-practices"></a>Práticas recomendadas de localização

1. Mova todos os recursos localizáveis para separar DLLs somente de recursos. Os recursos localizáveis incluem elementos da interface do usuário, como cadeias de caracteres, mensagens de erro, caixas de diálogo, menus e recursos do objeto inserido.

2. Não codifique cadeias de caracteres ou recursos da interface do usuário.

3. Não coloque os recursos não localizáveis em DLLs somente de recursos. Isso confundirá os tradutores.

4. Não use cadeias de caracteres compostas compiladas em tempo de execução de frases concatenadas. As cadeias de caracteres compostas são difíceis de localizar já que elas muitas vezes assumem uma ordem gramatical em inglês que não se aplica a todos os idiomas.

5. Evite constructos ambíguos, como "Empty Folder" (Pasta vazia), em que as cadeias de caracteres podem ser traduzidas de forma diferente dependendo das funções gramaticais dos componentes da cadeia de caracteres. Por exemplo, "empty", em inglês, pode ser um verbo ou um adjetivo (esvaziar/vazio) e isso pode levar a diferentes traduções em idiomas como italiano ou francês.

6. No aplicativo, evite usar imagens e ícones que contenham texto. Fica caro localizá-los.

7. Dê espaço suficiente ao comprimento das cadeias de caracteres para que elas possam se expandir na interface do usuário. Em alguns idiomas, as frases podem exigir de 50 a 75% mais espaço do que em outros idiomas.

8. Use a classe <xref:System.Resources.ResourceManager?displayProperty=nameWithType> para recuperar os recursos com base na cultura.

9. Use o [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) para criar caixas de diálogo do Windows Forms para que elas possam ser localizadas usando o [Editor de Recursos do Windows Forms (Winres.exe)](../../framework/tools/winres-exe-windows-forms-resource-editor.md). Não codifique manualmente as caixas de diálogo do Windows Forms.

10. Procure um profissional de localização (tradução).

11. Para obter uma descrição completa sobre a criação e localização de recursos, confira [Recursos em aplicativos](../../framework/resources/index.md).

## <a name="globalization-best-practices-for-aspnet-applications"></a>Práticas recomendadas de globalização para aplicativos ASP.NET

1. Defina explicitamente as propriedades <xref:System.Globalization.CultureInfo.CurrentUICulture%2A> e <xref:System.Globalization.CultureInfo.CurrentCulture%2A> em seu aplicativo. Não confie em padrões.

2. Observe que os aplicativos ASP.NET são aplicativos gerenciados e, portanto, podem usar as mesmas classes como outros aplicativos gerenciados para recuperar, exibir e manipular informações com base na cultura.

3. Lembre-se de que você pode especificar os seguintes três tipos de codificações no ASP.NET:

    - requestEncoding especifica a codificação recebida do navegador do cliente.

    - responseEncoding especifica a codificação a ser enviada ao navegador do cliente. Na maioria das situações, essa codificação deve ser a mesma codificação especificada para requestEncoding.

    - fileEncoding especifica a codificação padrão da análise de arquivos .aspx, .asmx e .asax.

4. Especifique os valores dos atributos requestEncoding, responseEncoding, fileEncoding, culture e uiCulture nos três locais a seguir em um aplicativo ASP.NET:

    - Na seção de globalização de um arquivo Web.config. Este arquivo é externo no aplicativo ASP.NET. Para obter mais informações, consulte [ \<globalization> elemento](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/hy4kkhe0(v=vs.100)).

    - Em uma diretiva de página. Observe que, quando um aplicativo está em uma página, isso significa que o arquivo já foi lido. Portanto, já não é possível especificar fileEncoding e requestEncoding. Somente uiCulture, Culture e responseEncoding podem ser especificados em uma diretiva de página.

    - No código do aplicativo de forma programática. Essa configuração pode variar se houver uma solicitação. Com uma diretiva de página, quando o código do aplicativo for atingido, já terá passado da hora de especificar fileEncoding e requestEncoding. Somente uiCulture, Culture e responseEncoding podem ser especificados no código do aplicativo.

5. Observe que o valor de uiCulture pode ser definido para o navegador do idioma aceito.

## <a name="see-also"></a>Veja também

- [Globalização e localização](index.md)
- [Recursos em aplicativos da área de trabalho](../../framework/resources/index.md)
