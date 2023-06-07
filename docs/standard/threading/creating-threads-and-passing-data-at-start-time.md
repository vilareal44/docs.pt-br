---
title: Criando threads e passando dados na hora de início
description: Entenda como criar threads e passar dados na hora de início de um processo do sistema operacional no .NET.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- threading [.NET Framework], creating
- threading [.NET Framework], passing data to threads
- threading [.NET Framework], retrieving data from threads
ms.assetid: 52b32222-e185-4f42-91a7-eaca65c0ab6d
ms.openlocfilehash: 811028d3c853441ff3a61d3628a44e5c65ba7059
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84661908"
---
# <a name="creating-threads-and-passing-data-at-start-time"></a>Criando threads e passando dados na hora de início

Quando um processo do sistema operacional é criado, o sistema operacional injeta um thread para executar o código nesse processo, incluindo qualquer domínio de aplicativo original. Desse ponto em diante, os domínios de aplicativo podem ser criados e destruídos sem qualquer criação ou destruição de threads do sistema operacional. Se o código que estiver sendo executado for um código gerenciado, um objeto <xref:System.Threading.Thread> para o thread em execução no domínio do aplicativo atual poderá ser obtido recuperando a propriedade <xref:System.Threading.Thread.CurrentThread%2A> estática do tipo <xref:System.Threading.Thread>. Este tópico descreve a criação de thread e aborda alternativas para passar dados para o procedimento do thread.  
  
## <a name="creating-a-thread"></a>Criação de um thread

 A criação de um novo objeto <xref:System.Threading.Thread> cria um novo thread gerenciado. A classe <xref:System.Threading.Thread> tem construtores que usam um delegado <xref:System.Threading.ThreadStart> ou um delegado <xref:System.Threading.ParameterizedThreadStart>; o delegado encapsula o método invocado pelo novo thread quando você chama o método <xref:System.Threading.Thread.Start%2A>. Chamar <xref:System.Threading.Thread.Start%2A> mais de uma vez faz com que uma <xref:System.Threading.ThreadStateException> seja lançada.  
  
 O método <xref:System.Threading.Thread.Start%2A> retorna imediatamente, muitas vezes antes do novo thread realmente começar. Você pode usar as propriedades <xref:System.Threading.Thread.ThreadState%2A> e <xref:System.Threading.Thread.IsAlive%2A> para determinar o estado do thread a qualquer momento, mas essas propriedades nunca devem ser usadas para sincronizar as atividades dos threads.  
  
> [!NOTE]
> Após o início do thread, não é necessário manter uma referência ao objeto <xref:System.Threading.Thread>. O thread continua a executar até que o procedimento do thread termine.  
  
 O exemplo de código a seguir cria dois novos threads para chamar métodos de instância e estáticos em outro objeto.  
  
 [!code-cpp[System.Threading.ThreadStart2#2](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CPP/source2.cpp#2)]
 [!code-csharp[System.Threading.ThreadStart2#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CS/source2.cs#2)]
 [!code-vb[System.Threading.ThreadStart2#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Threading.ThreadStart2/VB/source2.vb#2)]  
  
## <a name="passing-data-to-threads"></a>Passando dados para threads

 No .NET Framework versão 2.0, o delegado <xref:System.Threading.ParameterizedThreadStart> fornece uma maneira fácil para passar um objeto que contém dados para um thread quando você chama a sobrecarga do método <xref:System.Threading.Thread.Start%2A?displayProperty=nameWithType>. Consulte <xref:System.Threading.ParameterizedThreadStart> para obter um exemplo de código.  
  
 Usar o delegado <xref:System.Threading.ParameterizedThreadStart> não é uma maneira fortemente tipada de passar dados, pois a sobrecarga do método <xref:System.Threading.Thread.Start%2A?displayProperty=nameWithType> aceita qualquer objeto. Uma alternativa é encapsular o procedimento do thread e os dados em uma classe auxiliar e usar o delegado <xref:System.Threading.ThreadStart> para executar o procedimento de thread. O exemplo a seguir demonstra essa técnica:

 [!code-cpp[System.Threading.ThreadStart2#3](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CPP/source3.cpp#3)]
 [!code-csharp[System.Threading.ThreadStart2#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CS/source3.cs#3)]
 [!code-vb[System.Threading.ThreadStart2#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Threading.ThreadStart2/VB/source3.vb#3)]  

Nenhum dos delegados <xref:System.Threading.ThreadStart> e <xref:System.Threading.ParameterizedThreadStart> tem um valor retornado, porque não há um local para retornar os dados de uma chamada assíncrona. Para recuperar os resultados de um método de thread, é possível usar um método de retorno de chamada, conforme mostrado na próxima seção.
  
## <a name="retrieving-data-from-threads-with-callback-methods"></a>Recuperação de dados de threads com métodos de retorno de chamada

 O exemplo a seguir demonstra um método de retorno de chamada que recupera dados de um thread. O construtor para a classe que contém os dados e o método de thread também aceita um delegado que representa o método de retorno de chamada; antes do método de thread terminar, ele invoca o delegado de retorno de chamada.  
  
 [!code-cpp[System.Threading.ThreadStart2#4](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CPP/source4.cpp#4)]
 [!code-csharp[System.Threading.ThreadStart2#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CS/source4.cs#4)]
 [!code-vb[System.Threading.ThreadStart2#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Threading.ThreadStart2/VB/source4.vb#4)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Threading.Thread>
- <xref:System.Threading.ThreadStart>
- <xref:System.Threading.ParameterizedThreadStart>
- <xref:System.Threading.Thread.Start%2A?displayProperty=nameWithType>
- [Threading](index.md)
- [Usando threads e threading](using-threads-and-threading.md)
