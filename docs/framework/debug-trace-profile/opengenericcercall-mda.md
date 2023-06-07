---
title: MDA openGenericCERCall
description: Consulte o assistente de depuração gerenciada openGenericCERCall, que pode ser ativado se o código CER não for executado quando um thread for anulado ou quando um domínio de aplicativo for descarregado.
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), CER calls
- open generic CER calls
- constrained execution regions
- openGenericCERCall MDA
- CER calls
- managed debugging assistants (MDAs), CER calls
- generics [.NET Framework], open generic CER calls
ms.assetid: da3e4ff3-2e67-4668-9720-fa776c97407e
ms.openlocfilehash: 4df33b0cdf9759edec47f02b3feb671d03284ec8
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803931"
---
# <a name="opengenericcercall-mda"></a>MDA openGenericCERCall

O Assistente de Depuração Gerenciado de `openGenericCERCall` é ativado para avisar que um gráfico de CER (região de execução restrita) com variáveis de tipo genérico no método raiz está sendo processado em tempo de compilação JIT ou tempo de geração de imagem nativa e pelo menos uma das variáveis de tipo genérico é um tipo de referência de objeto.

## <a name="symptoms"></a>Sintomas

O código de CER não é executado quando um thread é anulado ou um domínio do aplicativo é descarregado.

## <a name="cause"></a>Causa

Em tempo de compilação JIT, uma instanciação que contém um tipo de referência de objeto é apenas representativo porque o código resultante é compartilhado e cada uma das variáveis de tipo de referência de objeto pode ser qualquer tipo de referência de objeto. Isso pode impedir a preparação antecipada de alguns recursos de tempo de execução.

Em particular, os métodos com variáveis de tipo genérico podem lentamente alocar recursos em segundo plano. Essas são chamadas de entradas de dicionário genéricas. Por exemplo, para a instrução `List<T> list = new List<T>();` em que `T` é uma variável de tipo genérico, o tempo de execução deve pesquisar e, possivelmente, criar a instanciação exata em tempo de execução, por exemplo, `List<Object>, List<String>` e assim por diante. Isso pode falhar por vários motivos fora do controle do desenvolvedor, tais como falta de memória.

Esse MDA só deve ser ativado em tempo de compilação JIT e não quando há uma instanciação exata.

Quando esse MDA é ativado, os sintomas prováveis são que as CERs não estejam funcionando para as instanciações incorretas. Na verdade, o runtime não tentou implementar uma CER sob as circunstâncias que causaram a ativação do MDA. Portanto, se o desenvolvedor usa uma instanciação compartilhada do CER, então os erros de compilação JIT, erros de carregamento de tipo genéricos ou anulações do thread dentro da região da CER pretendida não são detectados.

## <a name="resolution"></a>Resolução

Não use variáveis de tipo genérico do tipo de referência de objeto para métodos que podem conter uma CER.

## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime

Esse MDA não tem efeito sobre o CLR.

## <a name="output"></a>Saída

Veja a seguir um exemplo de saída deste MDA:
  
 ```output
 Method 'GenericMethodWithCer', which contains at least one constrained execution region, cannot be prepared automatically since it has one or more unbound generic type parameters.
 The caller must ensure this method is prepared explicitly at run time prior to execution.
 method name="GenericMethodWithCer"
 declaringType name="OpenGenericCERCall"
 ```

## <a name="configuration"></a>Configuração

```xml
<mdaConfig>
  <assistants>
    <openGenericCERCall/>
  </assistants>
</mdaConfig>
```  

## <a name="example"></a>Exemplo

O código CER não é executado.

```csharp
using System;
using System.Collections.Generic;
using System.Runtime.CompilerServices;

class Program
{
    static void Main(string[] args)
    {
        CallGenericMethods();
    }
    static void CallGenericMethods()
    {
        // This call is correct. The instantiation of the method
        // contains only nonreference types.
        MyClass.GenericMethodWithCer<int>();

        // This call is incorrect. A shared version of the method that
        // cannot be completely analyzed will be JIT-compiled. The
        // MDA will be activated at JIT-compile time, not at run time.
        MyClass.GenericMethodWithCer<String>();
    }
}

class MyClass
{
    public static void GenericMethodWithCer<T>()
    {
        RuntimeHelpers.PrepareConstrainedRegions();
        try
        {

        }
        finally
        {
            // This is the start of the CER.
            Console.WriteLine("In finally block.");
        }
    }
}
```

## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A>
- <xref:System.Runtime.ConstrainedExecution>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
