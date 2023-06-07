---
title: Compilação e reutilização em expressões regulares
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- parsing text with regular expressions, compilation
- searching with regular expressions, compilation
- .NET Framework regular expressions, engines
- .NET Framework regular expressions, compilation
- regular expressions, compilation
- compilation, regular expressions
- pattern-matching with regular expressions, compilation
- regular expressions, engines
ms.assetid: 182ec76d-5a01-4d73-996c-0b0d14fcea18
ms.openlocfilehash: 54f14a4f31bef00dd222686cc523935b2d9dd5fa
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84279032"
---
# <a name="compilation-and-reuse-in-regular-expressions"></a>Compilação e reutilização em expressões regulares
Você pode otimizar o desempenho de aplicativos que fazem uso intensivo de expressões regulares compreendendo como o mecanismo de expressões regulares compila expressões e como as expressões regulares são armazenadas em cache. Este tópico discute a compilação e o cache.  
  
## <a name="compiled-regular-expressions"></a>Expressões regulares compiladas  
 Por padrão, o mecanismo de expressões regulares compila uma expressão regular para uma sequência de instruções internas (esses são códigos de alto nível que são diferentes da Microsoft Intermediate Language ou MSIL). Quando o mecanismo executa uma expressão regular, ele interpreta os códigos internos.  
  
 Se um objeto <xref:System.Text.RegularExpressions.Regex> for construído com a opção <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType>, ele compilará a expressão regular para o código explícito em MSIL, em vez de instruções internas de expressões regulares de alto nível. Isso permite que o compilador JIT (just-in-time) do .NET converta a expressão para um código de computador nativo para um melhor desempenho.  O custo da construção do <xref:System.Text.RegularExpressions.Regex> objeto pode ser maior, mas o custo da execução de correspondências com ele provavelmente será muito menor.

 Uma alternativa é usar expressões regulares pré-compiladas. Você pode compilar todas as suas expressões em uma DLL reutilizável usando o método <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A>. Isso evita a necessidade de Compilar em tempo de execução enquanto ainda se beneficia da velocidade das expressões regulares compiladas.  
  
## <a name="the-regular-expressions-cache"></a>Cache de expressões regulares  
 Para melhorar o desempenho, o mecanismo de expressões regulares mantém um cache em todo o aplicativo com as expressões regulares compiladas. O cache armazena padrões de expressões regulares que são usados somente em chamadas de método estático. (Padrões de expressão regular fornecidos para métodos de instância não são armazenados em cache.) Isso evita a necessidade de analisar novamente uma expressão em um código de byte de alto nível sempre que ela for usada.  
  
 O número máximo de expressões regulares em cache é determinado pelo valor da propriedade `static` (`Shared` no Visual Basic) <xref:System.Text.RegularExpressions.Regex.CacheSize%2A?displayProperty=nameWithType>. Por padrão, o mecanismo de expressões regulares armazena em cache até 15 expressões regulares compiladas. Se o número de expressões regulares compiladas ultrapassar o tamanho do cache, a expressão regular menos utilizada recentemente será descartada e a nova expressão regular será armazenada em cache.  
  
 Seu aplicativo pode reutilizar expressões regulares de uma das duas maneiras a seguir:  
  
- Usando um método estático do objeto <xref:System.Text.RegularExpressions.Regex> para definir a expressão regular. Se você estiver usando um padrão de expressão regular que já foi definido por outra chamada de método estático, o mecanismo de expressão regular tentará recuperá-lo do cache. Se não estiver disponível no cache, o mecanismo compilará a expressão regular e a adicionará ao cache.
  
- Reutilizando um objeto <xref:System.Text.RegularExpressions.Regex> existente enquanto o padrão de expressão regular for necessário.  
  
 Devido à sobrecarga da instanciação de objetos e à compilação da expressão regular, criar e destruir rapidamente vários objetos <xref:System.Text.RegularExpressions.Regex> é um processo muito caro. Para aplicativos que usam um grande número de expressões regulares diferentes, você pode otimizar o desempenho usando chamadas para métodos `Regex` estáticos e, possivelmente, aumentando o tamanho do cache de expressão regular.  
  
## <a name="see-also"></a>Veja também

- [Expressões regulares do .NET](regular-expressions.md)
