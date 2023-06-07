---
title: Usar tipos de exceção padrão
description: Leia sobre os tipos de exceção padrão no .NET. Saiba mais sobre o SystemException, o ApplicationException, o ArgumentException, o COMException e muito mais.
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- throwing exceptions, standard types
- catching exceptions
- exceptions, catching
- exceptions, throwing
ms.assetid: ab22ce03-78f9-4dca-8824-c7ed3bdccc27
ms.openlocfilehash: f95529eabd87d9ec7c379b9f790e039e1192ac53
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84663051"
---
# <a name="using-standard-exception-types"></a>Usar tipos de exceção padrão
Esta seção descreve as exceções padrão fornecidas pela estrutura e os detalhes de seu uso. A lista não é, de maneira alguma, completa. Consulte a documentação de referência do .NET Framework para uso de outros tipos de exceção de estrutura.

## <a name="exception-and-systemexception"></a>Exception e SystemException
 ❌Não lançar <xref:System.Exception?displayProperty=nameWithType> ou <xref:System.SystemException?displayProperty=nameWithType> .

 ❌Não pegue `System.Exception` nem `System.SystemException` no código de estrutura, a menos que você pretenda relançar.

 ❌Evite capturar `System.Exception` ou `System.SystemException` , exceto em manipuladores de exceção de nível superior.

## <a name="applicationexception"></a>ApplicationException
 ❌NÃO lançar ou derivar de <xref:System.ApplicationException> .

## <a name="invalidoperationexception"></a>InvalidOperationException
 ✔️ gerar um <xref:System.InvalidOperationException> se o objeto estiver em um estado inadequado.

## <a name="argumentexception-argumentnullexception-and-argumentoutofrangeexception"></a>ArgumentException, ArgumentNullException e ArgumentOutOfRangeException
 ✔️ gerar <xref:System.ArgumentException> ou um de seus subtipos se argumentos inválidos forem passados para um membro. Prefira o tipo de exceção mais derivada, se aplicável.

 ✔️ definir a `ParamName` propriedade ao lançar uma das subclasses de `ArgumentException` .

 Essa propriedade representa o nome do parâmetro que causou a geração da exceção. Observe que a propriedade pode ser definida usando uma das sobrecargas do construtor.

 ✔️ Use `value` para o nome do parâmetro de valor implícito dos setters de propriedade.

## <a name="nullreferenceexception-indexoutofrangeexception-and-accessviolationexception"></a>NullReferenceException, IndexOutOfRangeException e AccessViolationException
 ❌Não permita que as APIs que podem ser chamadas publicamente lancem <xref:System.NullReferenceException> , <xref:System.AccessViolationException> , ou <xref:System.IndexOutOfRangeException> . Essas exceções são reservadas e geradas pelo mecanismo de execução e, na maioria dos casos, indicam um bug.

 Faça a verificação de argumento para evitar lançar essas exceções. Lançar essas exceções expõe os detalhes de implementação do método que pode mudar ao longo do tempo.

## <a name="stackoverflowexception"></a>StackOverflowException
 ❌Não lance explicitamente <xref:System.StackOverflowException> . A exceção deve ser gerada explicitamente apenas pelo CLR.

 ❌Não Catch `StackOverflowException` .

 É quase impossível escrever código gerenciado que permaneça consistente na presença de estouros de pilha arbitrários. As partes não gerenciadas do CLR permanecem consistentes com o uso de investigações para mover estouros de pilha para locais bem definidos em vez de fazer backup de estouros de pilha arbitrários.

## <a name="outofmemoryexception"></a>OutOfMemoryException
 ❌Não lance explicitamente <xref:System.OutOfMemoryException> . Essa exceção deve ser lançada apenas pela infraestrutura do CLR.

## <a name="comexception-sehexception-and-executionengineexception"></a>COMException, SEHException e ExecutionEngineException
 ❌Não lance explicitamente <xref:System.Runtime.InteropServices.COMException> , <xref:System.ExecutionEngineException> , e <xref:System.Runtime.InteropServices.SEHException> . Essas exceções devem ser lançadas apenas pela infraestrutura do CLR.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Confira também

- [Diretrizes de design de estrutura](index.md)
- [Diretrizes de design para exceções](exceptions.md)
