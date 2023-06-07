---
title: Como verificar se cadeias de caracteres estão em um formato de email válido
description: Leia um exemplo de como uma expressão regular verifica se as cadeias de caracteres estão em um formato de email válido no .NET.
ms.date: 06/30/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, examples
- user input, examples
- Regex.IsMatch method
- regular expressions [.NET Framework], examples
- examples [Visual Basic], strings
- IsValidEmail
- validation, email strings
- input, checking
- strings [.NET Framework], examples [Visual Basic]
- email [.NET Framework], validating
- IsMatch method
ms.assetid: 7536af08-4e86-4953-98a1-a8298623df92
ms.openlocfilehash: d303c13dead6b4ba29cb7476c2a9b382a9395aff
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803190"
---
# <a name="how-to-verify-that-strings-are-in-valid-email-format"></a>Como verificar se cadeias de caracteres estão em um formato de email válido

O exemplo a seguir usa uma expressão regular para verificar se uma cadeia de caracteres está no formato de email válido.

[!INCLUDE [regex](../../../includes/regex.md)]

## <a name="example"></a>Exemplo

O exemplo define um método `IsValidEmail`, que retornará `true` se a cadeia de caracteres contiver um endereço de email válido e `false` se não contiver, mas não realiza outra ação.

Para verificar se o endereço de email é válido, o método `IsValidEmail` chama o método <xref:System.Text.RegularExpressions.Regex.Replace%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.MatchEvaluator%29?displayProperty=nameWithType> com o padrão da expressão regular `(@)(.+)$` para separar o nome de domínio do endereço de email. O terceiro parâmetro é um representante <xref:System.Text.RegularExpressions.MatchEvaluator>, que representa o método que processa e substitui o texto correspondente. O padrão da expressão regular é interpretado da seguinte maneira.

|Padrão|Descrição|
|-------------|-----------------|
|`(@)`|Coincida o caractere @. Este é o primeiro grupo de captura.|
|`(.+)`|Coincida uma ou mais ocorrências de qualquer caractere. Este é o segundo grupo de captura.|
|`$`|Encerrar a correspondência ao final da cadeia de caracteres.|

O nome de domínio com o caractere @ é passado para o método `DomainMapper`, que usa a classe <xref:System.Globalization.IdnMapping> para converter caracteres Unicode fora do intervalo de caracteres US-ASCII em Punycode. O método também define o sinalizador `invalid` como `True`, caso o método <xref:System.Globalization.IdnMapping.GetAscii%2A?displayProperty=nameWithType> detecte algum caractere inválido no nome do domínio. O método retorna o nome de domínio Punycode precedido pelo símbolo @ para o método `IsValidEmail`.

Em seguida, o método `IsValidEmail` chama o método <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%29?displayProperty=nameWithType> para verificar se o endereço está adequado para um padrão de expressão regular.

O método `IsValidEmail` não realiza a autenticação para validar o endereço de email. Ele simplesmente determina se o formato é válido para um endereço de email. Além disso, o método `IsValidEmail` não verifica se o nome de domínio primário é um nome de domínio válido listado no [IANA Root Zone Database](https://www.iana.org/domains/root/db) (Banco de dados de zona raiz da IANA), o que exigiria uma operação de pesquisa. Em vez disso, a expressão regular apenas verifica se o nome de domínio primário consiste entre dois e vinte e quatro caracteres ASCII, com caracteres alfanuméricos sendo os primeiros e os últimos e o restante dos caracteres sendo alfanuméricos ou um hífen (-).

[!code-csharp[RegularExpressions.Examples.Email#7](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.Email/cs/example4.cs#7)]
[!code-vb[RegularExpressions.Examples.Email#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.Email/vb/example4.vb#7)]

Neste exemplo, o padrão de expressão regular ``^(?(")(".+?(?<!\\)"@)|(([0-9a-z]((\.(?!\.))|[-!#\$%&'\*\+/=\?\^`\{\}\|~\w])*)(?<=[0-9a-z])@))(?(\[)(\[(\d{1,3}\.){3}\d{1,3}\])|(([0-9a-z][-0-9a-z]*[0-9a-z]*\.)+[a-z0-9][\-a-z0-9]{0,22}[a-z0-9]))$`` é interpretado conforme mostrado na legenda a seguir. A expressão regular é compilada usando o <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> sinalizador.

Padrão `^` : Inicie a correspondência no início da cadeia de caracteres.

Padrão `(?(")` : Determine se o primeiro caractere é uma aspa. `(?(")` é o início de um construtor de alternância.

Padrão `(?(")(".+?(?<!\\)"@)` : se o primeiro caractere for uma aspa, corresponda a uma aspa de início seguida por pelo menos uma ocorrência de qualquer caractere, seguida por uma aspa de término. As aspas finais não devem ser precedidas por um caractere de barra invertida (\\). `(?<!` é o início de uma asserção lookbehind negativa de largura zero. A cadeia de caracteres deve terminar com um sinal de arroba (@).

Padrão `|(([0-9a-z]` : se o primeiro caractere não for uma aspa, corresponda a qualquer caractere alfabético de a a z ou a a z (a comparação não diferencia maiúsculas de minúsculas) ou qualquer caractere numérico de 0 a 9.

Padrão `(\.(?!\.))` : se o próximo caractere for um ponto, combine-o. Se ele não for um ponto final, observe o próximo caractere e continue a correspondência. `(?!\.)` é uma asserção lookahead negativa de largura zero que impede dois pontos finais consecutivos de serem exibidos na parte local de um endereço de email.

Padrão ``|[-!#\$%&'\*\+/=\?\^`\{\}\|~\w]`` : se o próximo caractere não for um ponto, corresponda a qualquer caractere de palavra ou um dos seguintes caracteres:-! # $% & ' \* +/=? ^ \` {} | ~

Padrão ``((\.(?!\.))|[-!#\$%&'\*\+/=\?\^`\{\}\|~\w])*`` : corresponder ao padrão de alternância (um ponto seguido por um período que não seja um ponto ou um de vários caracteres) zero ou mais vezes.

Padrão `@` : corresponder ao caractere @.

Padrão `(?<=[0-9a-z])` : Continue a correspondência se o caractere que precede o caractere @ for de a a z, a a z ou 0 a 9. Esse padrão define uma asserção lookbehind positiva de largura zero.

Padrão `(?(\[)` : Verifique se o caractere que segue @ é um colchete de abertura.

Padrão `(\[(\d{1,3}\.){3}\d{1,3}\])` : se for um colchete de abertura, corresponda ao colchete de abertura seguido por um endereço IP (quatro conjuntos de um a três dígitos, com cada conjunto separado por um ponto) e um colchete de fechamento.

Padrão `|(([0-9a-z][-0-9a-z]*[0-9a-z]*\.)+` : se o caractere que segue @ não for um colchete de abertura, corresponda a um caractere alfanumérico com um valor de a-z, a-z ou 0-9, seguido por zero ou mais ocorrências de um hífen, seguido por zero ou um caractere alfanumérico com um valor de a-z, a-z ou 0-9, seguido de um ponto. Esse padrão pode ser repetido uma ou mais vezes e deve ser seguido pelo nome de domínio primário.

Padrão `[a-z0-9][\-a-z0-9]{0,22}[a-z0-9]))` : o nome de domínio de nível superior deve começar e terminar com um caractere alfanumérico (a-z, a-z e 0-9). Ele também pode incluir de zero a 22 caracteres ASCII que são alfanuméricos ou hifens.

Padrão `$` : Finalize a correspondência no final da cadeia de caracteres.

## <a name="compile-the-code"></a>Compilar o código

Os métodos `IsValidEmail` e `DomainMapper` podem ser incluídos em uma biblioteca de métodos de utilitário de expressão regular ou como métodos estáticos privados ou de instância na classe do aplicativo.

Também é possível usar o método <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType> para incluir essa expressão regular em uma biblioteca de expressões regulares.

Se eles forem usados em uma biblioteca de expressões regulares, você poderá informá-los usando um código como o seguinte:

[!code-csharp[RegularExpressions.Examples.Email#8](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.Email/cs/example4.cs#8)]
[!code-vb[RegularExpressions.Examples.Email#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.Email/vb/example4.vb#8)]

## <a name="see-also"></a>Consulte também

- [Expressões regulares do .NET Framework](regular-expressions.md)
