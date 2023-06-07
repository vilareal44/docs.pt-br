---
title: Limpando recursos não gerenciados
description: Veja como limpar recursos não gerenciados não tratados pelo coletor de lixo do .NET, como arquivos, Windows, & conexões de rede ou de banco de dados.
ms.date: 05/13/2020
ms.technology: dotnet-standard
helpviewer_keywords:
- Close method
- Dispose method
- garbage collector
- garbage collection, unmanaged resources
- cleanup operations
- explicit cleanups
- unmanaged resource cleanup
- Finalize method
ms.assetid: a17b0066-71c2-4ba4-9822-8e19332fc213
ms.openlocfilehash: 07a8d754f1fc2612ae53407fa1b12a1eab7e38f2
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599822"
---
# <a name="cleaning-up-unmanaged-resources"></a>Limpando recursos não gerenciados

Para a maioria dos objetos que seu aplicativo cria, você pode contar com o [coletor de lixo do .net](index.md) para lidar com o gerenciamento de memória. No entanto, ao criar objetos que incluem recursos não gerenciados, você deve liberar explicitamente esses recursos quando terminar de usá-los. Os tipos mais comuns de recursos não gerenciados são objetos que encapsulam recursos do sistema operacional, como arquivos, Windows, conexões de rede ou conexões de banco de dados. Embora o coletor de lixo consiga controlar o tempo de vida de um objeto que encapsula um recurso não gerenciado, ele não sabe como liberar e limpar o recurso não gerenciado.

Se seus tipos usam recursos não gerenciados, você deve fazer o seguinte:

- Implementar o [padrão de descarte](implementing-dispose.md). Isso exige que você forneça uma <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> implementação para habilitar a versão determinística de recursos não gerenciados. Um consumidor de seu tipo chama <xref:System.IDisposable.Dispose%2A> quando o objeto (e os recursos que ele usa) não são mais necessários. O método <xref:System.IDisposable.Dispose%2A> libera imediatamente os recursos não gerenciados.

- Caso um consumidor de seu tipo se esqueça de chamar <xref:System.IDisposable.Dispose%2A> , forneça uma maneira para que seus recursos não gerenciados sejam liberados. Existem duas maneiras de fazer isso:

  - Usar um identificador seguro para encapsular o recurso não gerenciado. Essa é a técnica recomendada. Os identificadores seguros são derivados da <xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType> classe abstrata e incluem um <xref:System.Object.Finalize%2A> método robusto. Ao usar um identificador seguro, você simplesmente implementa a interface <xref:System.IDisposable> e chama o método <xref:System.Runtime.InteropServices.SafeHandle.Dispose%2A> do seu identificador seguro em sua implementação do <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType>. O finalizador do identificador seguro é chamado automaticamente pelo coletor de lixo quando seu método <xref:System.IDisposable.Dispose%2A> não é chamado.

    —**ou**–

  - Substituir o método <xref:System.Object.Finalize%2A?displayProperty=nameWithType>. A finalização habilita a liberação não determinística de recursos não gerenciados quando o consumidor de um tipo não chama <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> para fazer o descarte de forma determinística. Defina um [finalizador](../../csharp/programming-guide/classes-and-structs/destructors.md) substituindo o <xref:System.Object.Finalize%2A?displayProperty=nameWithType> método.

  > [!WARNING]
  > No entanto, como a finalização de objeto pode ser uma operação complexa e propensa a erros, recomendamos que você use um identificador seguro em vez de fornecer seu próprio finalizador.

Os consumidores do seu tipo poderão então chamar sua implementação de <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> diretamente para liberar a memória usada pelos recursos não gerenciados. Quando você implementa corretamente um método <xref:System.IDisposable.Dispose%2A>, o método <xref:System.Object.Finalize%2A> do seu identificador seguro ou sua própria substituição do método <xref:System.Object.Finalize%2A?displayProperty=nameWithType> torna-se uma proteção para limpar recursos para o caso do método <xref:System.IDisposable.Dispose%2A> não ser chamado.

## <a name="in-this-section"></a>Nesta seção

A [implementação de um método Dispose](implementing-dispose.md) descreve como implementar o padrão Dispose para liberar recursos não gerenciados.

[Usando objetos que implementam `IDisposable` ](using-objects.md) Descreve como os consumidores de um tipo garantem que sua <xref:System.IDisposable.Dispose%2A> implementação seja chamada. É recomendável usar a `using` instrução C# (ou Visual Basic `Using` ) para fazer isso.

## <a name="reference"></a>Referência

| Tipo/membro | Descrição |
|--|--|
| <xref:System.IDisposable?displayProperty=nameWithType> | Define o método <xref:System.IDisposable.Dispose%2A> para liberar recursos não gerenciados. |
| <xref:System.Object.Finalize%2A?displayProperty=nameWithType> | Cuida da finalização de objetos quando os recursos não gerenciados não são liberados pelo método <xref:System.IDisposable.Dispose%2A>. |
| <xref:System.GC.SuppressFinalize%2A?displayProperty=nameWithType> | Suprime a finalização. Este método geralmente é chamado de um método `Dispose` para impedir a execução de um finalizador. |
