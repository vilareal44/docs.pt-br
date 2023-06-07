---
ms.openlocfilehash: 54ef49755dc0b9d1b821ae7999ab218626d455e1
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556314"
---
### <a name="custom-encoderfallbackbuffer-instances-cannot-fall-back-recursively"></a>Instâncias EncoderFallbackBuffer personalizadas não podem retornar recursivamente

As <xref:System.Text.EncoderFallbackBuffer> instâncias personalizadas não podem fazer fallback recursivamente. A implementação de <xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> deve resultar em uma sequência de caracteres que é conversível para a codificação de destino. Caso contrário, ocorrerá uma exceção.

#### <a name="change-description"></a>Descrição da alteração

Durante uma operação de transcodificação de caractere para byte, o tempo de execução detecta sequências UTF-16 mal formadas ou não conversíveis e fornece esses caracteres para o <xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> método. O `Fallback` método determina quais caracteres devem ser substituídos pelos dados originais não conversíveis e esses caracteres são drenados chamando <xref:System.Text.EncoderFallbackBuffer.GetNextChar%2A?displayProperty=nameWithType> em um loop.

Em seguida, o tempo de execução tenta transcodificar esses caracteres de substituição para a codificação de destino. Se essa operação for realizada com sucesso, o tempo de execução continuará transcodificando de onde parou na cadeia de caracteres de entrada original.

Anteriormente, as implementações personalizadas do <xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> podem retornar sequências de caracteres que não são conversíveis para a codificação de destino. Se os caracteres substituídos não puderem ser transcodificados para a codificação de destino, o tempo de execução invocará o <xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> método novamente com os caracteres de substituição, esperando que o <xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> método retorne uma nova sequência de substituição. Esse processo continua até que o tempo de execução eventualmente Veja uma substituição bem formada, conversível ou até que uma contagem de recursão máxima seja atingida.

A partir do .NET Core 3,0, implementações personalizadas do <xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> devem retornar sequências de caracteres que são conversíveis para a codificação de destino. Se os caracteres substituídos não puderem ser transcodificados para a codificação de destino, um <xref:System.ArgumentException> será lançado. O tempo de execução não fará mais chamadas recursivas na <xref:System.Text.EncoderFallbackBuffer> instância.

Esse comportamento se aplica somente quando todas as três condições a seguir são atendidas:

- O tempo de execução detecta uma sequência UTF-16 mal formada ou uma sequência UTF-16 que não pode ser convertida para a codificação de destino.
- Um personalizado foi <xref:System.Text.EncoderFallback> especificado.
- As tentativas personalizadas de <xref:System.Text.EncoderFallback> substituir uma nova sequência UTF-16 mal formada ou não conversível.

#### <a name="version-introduced"></a>Versão introduzida

3.0

#### <a name="recommended-action"></a>Ação recomendada

A maioria dos desenvolvedores não precisam realizar nenhuma ação.

Se um aplicativo usar uma <xref:System.Text.EncoderFallback> classe e personalizada <xref:System.Text.EncoderFallbackBuffer> , verifique se a implementação de <xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> popula o buffer de FALLBACK com dados UTF-16 bem formados que são conversíveis diretamente para a codificação de destino quando o <xref:System.Text.EncoderFallbackBuffer.Fallback%2A> método é invocado pela primeira vez pelo tempo de execução.

#### <a name="category"></a>Categoria

Bibliotecas principais do .NET

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType>
- <xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:System.Text.EncoderFallbackBuffer.Fallback`
- `M:System.Text.EncoderFallbackBuffer.GetNextChar`

-->
