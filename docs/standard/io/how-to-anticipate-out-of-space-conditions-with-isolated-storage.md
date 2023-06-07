---
title: 'Como: Prever condições de espaço insuficiente com o armazenamento isolado'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data stores, quotas
- isolated storage, quotas
- quantity of isolated storage used
- limit on isolated storage used
- stores, quotas
- stores, out of space conditions
- data storage using isolated storage, quotas
- storing data using isolated storage, quotas
- space remaining in isolated storage
- data stores, out of space conditions
- storing data using isolated storage, out of space conditions
- quotas for isolated storage
- isolated storage, out of space conditions
- data storage using isolated storage, out of space conditions
ms.assetid: e35d4535-3732-421e-b1a3-37412e036145
ms.openlocfilehash: bdc2cee343e9d9be44230e84ff45d6fa54901f48
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288583"
---
# <a name="how-to-anticipate-out-of-space-conditions-with-isolated-storage"></a>Como: Prever condições de espaço insuficiente com o armazenamento isolado

O código que usa armazenamento isolado é restrito por uma [cota](isolated-storage.md#quotas) que especifica o tamanho máximo do compartimento de dados no qual arquivos de armazenamento e diretórios isolados existem. A cota é definida pela política de segurança e é configurável por administradores. Se o tamanho máximo permitido for ultrapassado ao tentar gravar dados, uma exceção <xref:System.IO.IsolatedStorage.IsolatedStorageException> será lançada e a operação falhará. Isso ajuda a evitar ataques de negação de serviço que podem fazer com que o aplicativo recuse solicitações porque o armazenamento de dados está cheio.

Para ajudar você a determinar se uma determinada tentativa de gravação provavelmente falhará por esse motivo, a classe <xref:System.IO.IsolatedStorage.IsolatedStorage> fornece três propriedades somente leitura: <xref:System.IO.IsolatedStorage.IsolatedStorage.AvailableFreeSpace%2A>, <xref:System.IO.IsolatedStorage.IsolatedStorage.UsedSize%2A> e <xref:System.IO.IsolatedStorage.IsolatedStorage.Quota%2A>. Você pode usar essas propriedades para determinar se a gravação no armazenamento fará com que o tamanho máximo permitido de armazenamento seja ultrapassado. Lembre-se de que o armazenamento isolado pode ser acessado simultaneamente; portanto, ao calcular a quantidade de armazenamento restante, o espaço de armazenamento poderá ser consumido até você tentar gravar no armazenamento. No entanto, você pode usar o tamanho máximo do armazenamento para ajudar a determinar se o limite superior no armazenamento disponível está prestes a ser alcançado.

A propriedade <xref:System.IO.IsolatedStorage.IsolatedStorage.Quota%2A> depende da evidência do funcionamento correto do assembly. Por esse motivo, você deve recuperar essa propriedade somente em objetos <xref:System.IO.IsolatedStorage.IsolatedStorageFile> que foram criados usando o método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A>, <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A> ou <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A>. Os objetos <xref:System.IO.IsolatedStorage.IsolatedStorageFile> que foram criados de qualquer outra forma (por exemplo, objetos retornados do método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetEnumerator%2A>) não retornarão um tamanho máximo preciso.

## <a name="example"></a>Exemplo

O exemplo de código a seguir obtém um armazenamento isolado, cria alguns arquivos e recupera a propriedade <xref:System.IO.IsolatedStorage.IsolatedStorage.AvailableFreeSpace%2A>. O espaço restante é relatado em bytes.

[!code-cpp[Conceptual.IsolatedStorage#8](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source7.cpp#8)]
[!code-csharp[Conceptual.IsolatedStorage#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source7.cs#8)]
[!code-vb[Conceptual.IsolatedStorage#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source7.vb#8)]

## <a name="see-also"></a>Veja também

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- [Armazenamento isolado](isolated-storage.md)
- [Como: Obter repositórios para o armazenamento isolado](how-to-obtain-stores-for-isolated-storage.md)
