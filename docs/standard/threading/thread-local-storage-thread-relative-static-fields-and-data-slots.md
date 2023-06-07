---
title: 'Armazenamento local de thread: campos estáticos relativos a thread e slots de dados'
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- threading [.NET Framework], local storage
- threading [.NET Framework], thread-relative static fields
- local thread storage
- TLS
ms.assetid: c633a4dc-a790-4ed1-96b5-f72bd968b284
ms.openlocfilehash: adeeb6c95769d8e1ac120d4fb26d8aaedf7a1d4d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291078"
---
# <a name="thread-local-storage-thread-relative-static-fields-and-data-slots"></a>Armazenamento local de thread: campos estáticos relativos a thread e slots de dados
Você pode usar o armazenamento local de thread (TLS) gerenciado para armazenar dados que são exclusivos de um domínio de aplicativo e thread. O .NET Framework oferece duas maneiras de usar o TLS gerenciado: campos estáticos relativos a thread e slots de dados.  
  
- Use campos estáticos relativos a thread (campos `Shared` relativos a thread no Visual Basic) se você puder prever suas necessidades exatas em tempo de compilação. Campos estáticos relativos a thread fornecem o melhor desempenho. Eles também oferecem os benefícios da verificação do tipo de tempo de compilação.  
  
- Use slots de dados quando seus requisitos reais podem ser descobertos apenas em tempo de execução. Os slots de dados são mais lentos e mais difíceis de usar do que os campos estáticos relativos a thread, e os dados são armazenados como tipo <xref:System.Object>, então você deve lançá-los no tipo correto antes de usá-los.  
  
 No C ++ não gerenciado, você usa `TlsAlloc` para alocar slots dinamicamente e `__declspec(thread)` para declarar que uma variável deve ser alocada no armazenamento relativo ao thread. Os campos estáticos relativos a thread e os slots de dados fornecem a versão gerenciada desse comportamento.  
  
 No .NET Framework 4, você pode usar a classe <xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType> para criar objetos locais de thread que são inicializados lentamente quando o objeto é consumido pela primeira vez. Para obter mais informações, veja [Inicialização lenta](../../framework/performance/lazy-initialization.md).  
  
## <a name="uniqueness-of-data-in-managed-tls"></a>Exclusividade de dados em TLS gerenciado  
 Não importa se você usa campos estáticos relativos a thread ou slots de dados, os dados em TLS gerenciado são exclusivos da combinação de thread e domínio de aplicativos.  
  
- Dentro de um domínio de aplicativo, um thread não pode modificar dados de outro thread, mesmo quando ambos threads usam o mesmo campo ou slot.  
  
- Quando um thread acessa o mesmo campo ou slot de vários domínios de aplicativos, um valor separado é mantido em cada domínio de aplicativo.  
  
 Por exemplo, se um thread define o valor de um campo estático relativo a thread, entra em outro domínio do aplicativo e, em seguida, recupera o valor do campo, o valor recuperado no segundo domínio do aplicativo difere do valor no primeiro domínio do aplicativo. Definir um novo valor para o campo no segundo domínio do aplicativo não afeta o valor do campo no primeiro domínio do aplicativo.  
  
 Da mesma forma, quando um segmento obtém o mesmo slot de dados nomeado em dois domínios de aplicativo diferentes, os dados no primeiro domínio do aplicativo permanecem independentes dos dados no segundo domínio do aplicativo.  
  
## <a name="thread-relative-static-fields"></a>Campos estáticos relativos a thread  
 Se você sabe que um dado é sempre exclusivo a uma combinação de thread e domínio de aplicativo, aplique o atributo <xref:System.ThreadStaticAttribute> ao campo estático. Use o campo como você usaria qualquer outro campo estático. Os dados no campo são exclusivos para cada thread que os usa.  
  
 Campos estáticos relativos a thread fornecem melhor desempenho do que os slots de dados e têm o benefício da verificação do tipo de tempo de compilação.  
  
 Lembre-se de que qualquer código de construtor de classe será executado no primeiro thread no primeiro contexto que acessa o campo. Em todos os outros threads ou contextos no mesmo domínio de aplicativo, os campos serão inicializados para `null` (`Nothing` no Visual Basic) se forem tipos de referência, ou para seus valores padrão se forem tipos de valor. Portanto, você não deve confiar em construtores de classe para inicializar campos estáticos relacionados a thread. Em vez disso, evite a inicialização de campos estáticos relacionados a thread e suponha que eles sejam inicializados para `null` (`Nothing`) ou para seus valores padrão.  
  
## <a name="data-slots"></a>Slots de dados  
 O .NET Framework fornece slots de dados dinâmicos que são exclusivos a uma combinação de thread e domínio de aplicativo. Existem dois tipos de slots de dados: slots nomeados e slots sem nome. Ambos são implementados usando a estrutura <xref:System.LocalDataStoreSlot>.  
  
- Para criar um slot de dados nomeado, use o método <xref:System.Threading.Thread.AllocateNamedDataSlot%2A?displayProperty=nameWithType> ou <xref:System.Threading.Thread.GetNamedDataSlot%2A?displayProperty=nameWithType>. Para obter uma referência a um slot nomeado existente, passe seu nome para o método <xref:System.Threading.Thread.GetNamedDataSlot%2A>.  
  
- Para criar um slot de dados sem nome, use o método <xref:System.Threading.Thread.AllocateDataSlot%2A?displayProperty=nameWithType>.  
  
 Para slots nomeados e sem nome, use os métodos <xref:System.Threading.Thread.SetData%2A?displayProperty=nameWithType> e <xref:System.Threading.Thread.GetData%2A?displayProperty=nameWithType> para definir e recuperar as informações no slot. Estes são métodos estáticos que sempre atuam sobre os dados do thread que os está executando atualmente.  
  
 Os slots nomeados podem ser convenientes, pois você pode recuperar o slot quando necessário, passando seu nome para o método <xref:System.Threading.Thread.GetNamedDataSlot%2A>, em vez de manter uma referência a um slot sem nome. No entanto, se outro componente usa o mesmo nome para seu armazenamento relativo ao thread e um thread executa o código do seu componente e do outro componente, os dois componentes podem corromper os dados uns dos outros. (Este cenário pressupõe que ambos os componentes estão sendo executados no mesmo domínio de aplicativo e que eles não foram projetados para compartilhar os mesmos dados).  
  
## <a name="see-also"></a>Veja também

- <xref:System.ContextStaticAttribute>
- <xref:System.Threading.Thread.GetNamedDataSlot%2A?displayProperty=nameWithType>
- <xref:System.ThreadStaticAttribute>
- <xref:System.Runtime.Remoting.Messaging.CallContext>
- [Threading](index.md)
