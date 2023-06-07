---
title: 'Como: Obter repositórios para o armazenamento isolado'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- stores, obtaining
- storing data using isolated storage, obtaining stores
- isolated storage, obtaining stores
- data stores, obtaining
- data storage using isolated storage, obtaining stores
ms.assetid: fcb6b178-d526-47c4-b029-e946f880f9db
ms.openlocfilehash: a08563b67239c679e3bc88876781508fd78bea75
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291832"
---
# <a name="how-to-obtain-stores-for-isolated-storage"></a>Como: Obter repositórios para o armazenamento isolado
Um repositório isolado expõe um sistema de arquivos virtual dentro de um compartimento de dados. A classe <xref:System.IO.IsolatedStorage.IsolatedStorageFile> fornece vários métodos para interagir com um repositório isolado. Para criar e recuperar repositórios, o <xref:System.IO.IsolatedStorage.IsolatedStorageFile> fornece três métodos estáticos:  
  
- <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A> retorna o armazenamento que é isolado pelo usuário e pelo assembly.  
  
- <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A> retorna o armazenamento que é isolado pelo domínio e pelo assembly.  
  
     Os dois métodos recuperam um repositório que pertence ao código do qual eles são chamados.  
  
- O método estático <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> retorna um repositório isolado que é especificado passando uma combinação de parâmetros de escopo.  
  
 O exemplo de código a seguir retorna um repositório que é isolado pelo usuário, pelo assembly e pelo domínio.  
  
 [!code-cpp[Conceptual.IsolatedStorage#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source6.cpp#6)]
 [!code-csharp[Conceptual.IsolatedStorage#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source6.cs#6)]
 [!code-vb[Conceptual.IsolatedStorage#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source6.vb#6)]  
  
 Você pode usar o método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> para especificar se um repositório deve ser usado com um perfil de usuário móvel. Para obter detalhes sobre como configurar isso, confira [Tipos de isolamento](types-of-isolation.md).  
  
 Os repositórios isolados obtidos em diferentes assemblies são, por padrão, os repositórios diferentes. Você pode acessar o repositório de um assembly ou de um domínio diferente passando a evidência de assembly ou domínio nos parâmetros do método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A>. Isso exige uma permissão para acessar o armazenamento isolado pela identidade do domínio do aplicativo. Para obter mais informações, confira as sobrecargas do método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A>.  
  
 Os métodos <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A>, <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A> e <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> retornam um objeto <xref:System.IO.IsolatedStorage.IsolatedStorageFile>. Para ajudá-lo a decidir qual tipo de isolamento é mais apropriado para sua situação, confira [Tipos de isolamento](types-of-isolation.md). Quando você tem um objeto de arquivo de armazenamento isolado, você pode usar os métodos de armazenamento isolado para ler, gravar, criar e excluir arquivos e diretórios.  
  
 Não há mecanismos que impeçam o código de passar um objeto <xref:System.IO.IsolatedStorage.IsolatedStorageFile> para o código que não tem acesso suficiente para obter o próprio repositório. As identidades de domínio e de assembly e as permissões de armazenamento isolado são verificadas apenas quando uma referência a um objeto <xref:System.IO.IsolatedStorage.IsolatedStorage> é obtida, normalmente no método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A>, <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A> ou <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A>. Proteger referências a objetos <xref:System.IO.IsolatedStorage.IsolatedStorageFile> é, portanto, a responsabilidade do código que usa essas referências.  
  
## <a name="example"></a>Exemplo  
 O código a seguir fornece um exemplo simples de uma classe obtendo um repositório que é isolado pelo usuário e pelo assembly. O código pode ser alterado para recuperar um repositório que é isolado pelo usuário, domínio e assembly adicionando <xref:System.IO.IsolatedStorage.IsolatedStorageScope.Domain?displayProperty=nameWithType> aos argumentos que o método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> passa.  
  
 Depois de executar o código, você pode confirmar se um repositório foi criado digitando **StoreAdm /LIST** na linha de comando. Essa ação executa a [Ferramenta de armazenamento isolado (Storeadm.exe)](../../framework/tools/storeadm-exe-isolated-storage-tool.md) e lista todos os atuais repositórios isolados para o usuário.  
  
 [!code-cpp[Conceptual.IsolatedStorage#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source6.cpp#7)]
 [!code-csharp[Conceptual.IsolatedStorage#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source6.cs#7)]
 [!code-vb[Conceptual.IsolatedStorage#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source6.vb#7)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- <xref:System.IO.IsolatedStorage.IsolatedStorageScope>
- [Armazenamento isolado](isolated-storage.md)
- [Tipos de isolamento](types-of-isolation.md)
- [Assemblies no .NET](../assembly/index.md)
