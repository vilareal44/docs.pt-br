---
title: Tratamento de erros de E/S no .NET
description: Saiba como lidar com erros de e/s no .NET. Mapeie códigos de erro para exceções, manipule exceções em operações de e/s e manipule IOException.
ms.date: 08/27/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- I/O, exception handling
- I/O, errors
ms.workload:
- dotnet
- dotnetcore
ms.openlocfilehash: 45f3951b727d3b615d8384541ff169e8840acab0
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599796"
---
# <a name="handling-io-errors-in-net"></a>Tratamento de erros de E/S no .NET

Além das exceções que podem ser geradas em qualquer chamada de método (como um <xref:System.OutOfMemoryException> quando um sistema sofre estresse ou um <xref:System.NullReferenceException> devido a um erro do programador), os métodos do sistema de arquivos do .NET podem gerar as seguintes exceções:

- <xref:System.IO.IOException?displayProperty=nameWithType>, a classe base de todos os tipos de exceção <xref:System.IO>. Ela é gerada para erros cujos códigos retornados do sistema operacional não mapeiam diretamente para outro tipo de exceção.
- <xref:System.IO.FileNotFoundException?displayProperty=nameWithType>.
- <xref:System.IO.DirectoryNotFoundException?displayProperty=nameWithType>.
- <xref:System.IO.DriveNotFoundException??displayProperty=nameWithType>.
- <xref:System.IO.PathTooLongException?displayProperty=nameWithType>.
- <xref:System.OperationCanceledException?displayProperty=nameWithType>.
- <xref:System.UnauthorizedAccessException?displayProperty=nameWithType>.
- <xref:System.ArgumentException?displayProperty=nameWithType>, gerada para caracteres de caminho inválido no .NET Framework e no .NET Core 2.0 e versões anteriores.
- <xref:System.NotSupportedException?displayProperty=nameWithType>, gerada para dois-pontos inválidos no .NET Framework.
- <xref:System.Security.SecurityException?displayProperty=nameWithType>, gerada para aplicativos em execução em confiança limitada que não têm as permissões necessárias somente no .NET Framework. (Confiança total é o padrão no .NET Framework.)

## <a name="mapping-error-codes-to-exceptions"></a>Mapeando códigos de erro para exceções

Como o sistema de arquivos é um recurso de sistema operacional, os métodos de E/S no .NET Core e no .NET Framework encapsulam chamadas ao sistema operacional subjacente. Quando ocorre um erro de E/S no código executado pelo sistema operacional, ele retorna informações do erro para o método de E/S do .NET. Em seguida, o método converte as informações do erro, normalmente na forma de um código de erro, em um tipo de exceção do .NET. Na maioria dos casos, ele faz isso convertendo diretamente o código de erro em seu tipo de exceção correspondente; ele não realiza nenhum mapeamento especial do erro com base no contexto da chamada de método.

Por exemplo, no sistema operacional Windows, uma chamada de método que retorna um código de erro `ERROR_FILE_NOT_FOUND` (ou 0x02) é mapeada para um <xref:System.IO.FileNotFoundException> e um código de erro `ERROR_PATH_NOT_FOUND` (ou 0x03) é mapeado para um <xref:System.IO.DirectoryNotFoundException>.

No entanto, as condições precisas sob as quais o sistema operacional retorna códigos de erro específicos geralmente não são documentadas ou são mal documentadas. Em decorrência disso, podem ocorrer exceções inesperadas. Por exemplo, como você está trabalhando com um diretório em vez de um arquivo, você esperaria que fornecer um caminho de diretório inválido para o construtor <xref:System.IO.DirectoryInfo.%23ctor%2A> gerasse uma <xref:System.IO.DirectoryNotFoundException>. No entanto, isso também pode gerar uma <xref:System.IO.FileNotFoundException>.

## <a name="exception-handling-in-io-operations"></a>Tratamento de exceções em operações de E/S

Por causa dessa dependência do sistema operacional, condições de exceção idênticas (como o erro de diretório não encontrado em nosso exemplo) podem resultar na geração de toda uma classe de exceções de E/S pelo método de E/S. Isso significa que, ao chamar APIs de E/S, seu código deve estar preparado para lidar com a maioria ou com todas essas exceções, conforme mostrado na tabela a seguir:

| Tipo de exceção | .NET Core | .NET Framework |
|---|---|---|
| <xref:System.IO.IOException> | Sim | Sim |
| <xref:System.IO.FileNotFoundException> | Sim | Sim |
| <xref:System.IO.DirectoryNotFoundException> | Sim | Sim |
| <xref:System.IO.DriveNotFoundException?> | Sim | Sim |
| <xref:System.IO.PathTooLongException> | Sim | Sim |
| <xref:System.OperationCanceledException> | Sim | Sim |
| <xref:System.UnauthorizedAccessException> | Sim | Sim |
| <xref:System.ArgumentException> | .NET Core 2.0 e anterior| Sim |
| <xref:System.NotSupportedException> | Não | Sim |
| <xref:System.Security.SecurityException> | Não | Confiança limitada apenas |

## <a name="handling-ioexception"></a>Tratamento IOException

Como a classe base para exceções no namespace <xref:System.IO>, a <xref:System.IO.IOException> também é gerada para qualquer código de erro que não é mapeado para um tipo de exceção predefinido. Isso significa que ela pode ser gerada por uma operação de E/S.

> [!IMPORTANT]
> Como <xref:System.IO.IOException> é a classe base dos outros tipos de exceção no namespace <xref:System.IO>, você deve tratar em um bloco `catch` depois de lidar com as outras exceções relacionadas a E/S.

Além disso, do .NET Core 2.1 em diante, as verificações de validação da exatidão do caminho (por exemplo, para garantir que os caracteres inválidos não estejam presentes em um caminho) foram removidas, e o runtime gera uma exceção mapeada de um código de erro do sistema operacional, em vez do seu próprio código de validação. A exceção com maior probabilidade de ser gerada nesse caso é uma <xref:System.IO.IOException>, embora também possa ser gerado qualquer outro tipo de exceção.

Observe que, no seu código de tratamento de exceções, você deve sempre tratar a <xref:System.IO.IOException> por último. Caso contrário, como ela é a classe base de todas as outras exceções de E/S, os blocos catch de classes derivadas não serão avaliados.

No caso de um <xref:System.IO.IOException>, é possível obter outras informações de erro na propriedade [IOException.HResult](xref:System.Exception.HResult). Para converter o valor HResult em um código de erro do Win32, remova os 16 bits superiores do valor de 32 bits. A tabela a seguir lista os códigos de erro que podem ser encapsulados em um <xref:System.IO.IOException>.

| HResult | Constante | Descrição |
| --- | --- | --- |
| ERROR_SHARING_VIOLATION | 32 | O nome do arquivo está ausente ou o arquivo ou diretório está em uso. |
| ERROR_FILE_EXISTS | 80 | O arquivo já existe. |
| ERROR_INVALID_PARAMETER | 87 | Um argumento fornecido para o método é inválido. |
| ERROR_ALREADY_EXISTS | 183 | O arquivo ou diretório já existe. |

É possível lidar com eles usando uma cláusula `When` em uma instrução catch, como o exemplo a seguir mostra.

[!code-csharp[io-exception-handling](~/samples/snippets/standard/io/io-exceptions/cs/io-exceptions.cs)]
[!code-vb[io-exception-handling](~/samples/snippets/standard/io/io-exceptions/vb/io-exceptions.vb)]

## <a name="see-also"></a>Consulte também

- [Tratando e gerando exceções no .NET](../exceptions/index.md)
- [Tratamento de exceções (biblioteca de paralelismo de tarefas)](../parallel-programming/exception-handling-task-parallel-library.md)
- [Práticas recomendadas para exceções](../exceptions/best-practices-for-exceptions.md)
- [Como usar exceções específicas em um bloco catch](../exceptions/how-to-use-specific-exceptions-in-a-catch-block.md)
