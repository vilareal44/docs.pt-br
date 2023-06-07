---
title: Tipos de isolamento
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- storing data using isolated storage, accessing isolated storage
- storing data using isolated storage, isolation types
- authentication [.NET Framework], isolated storage
- assemblies [.NET Framework], identity
- isolated storage, accessing
- data storage using isolated storage, isolation types
- data storage using isolated storage, accessing isolated storage
- domain identity
- isolated storage, types
- user authentication, isolated storage
ms.assetid: 14812988-473f-44ae-b75f-fd5c2f21fb7b
ms.openlocfilehash: 7802e4bc27195d1c8ecaccbd64121fb24328a4d8
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288531"
---
# <a name="types-of-isolation"></a>Tipos de isolamento
O acesso ao armazenamento isolado é sempre restrito ao usuário que o criou. Para implementar esse tipo de isolamento, o common language runtime usa a mesma noção de identidade de usuário que o sistema operacional reconhece, que é a identidade associada ao processo no qual o código está em execução quando o armazenamento é aberto. Essa é uma identidade de usuário autenticado, mas a representação pode fazer com que a identidade do usuário atual seja alterada dinamicamente.  
  
 O acesso ao armazenamento isolado também é restrito de acordo com a identidade associada ao domínio do aplicativo e ao assembly ou apenas ao assembly. O runtime obtém essas identidades das seguintes maneiras:  
  
- A identidade de domínio representa a evidência do aplicativo, que, no caso de um aplicativo Web, pode ser a URL completa. Para código hospedado pelo shell, a identidade de domínio pode ser baseada no caminho de diretório de aplicativo. Por exemplo, se o executável é executado do caminho C:\Office\MyApp.exe, a identidade de domínio é C:\Office\MyApp.exe.  
  
- A identidade do assembly é a evidência do assembly. Pode vir de uma assinatura digital de criptografia, que pode ser o [nome forte](../assembly/strong-named.md) do assembly, o editor de software do assembly ou sua identidade de URL. Se um assembly tiver um nome forte e uma identidade do editor de software, a identidade do editor de software será usada. Se o assembly vier da Internet e não estiver assinado, a identidade de URL será usada. Para saber mais sobre assemblies e nomes fortes, confira [Programação com assemblies](../assembly/index.md).  
  
- Os armazenamentos móveis migram com um usuário que tem um perfil de usuário móvel. Os arquivos são gravados em um diretório de rede e são baixados em qualquer computador em que o usuário faz logon. Para saber mais sobre perfis de usuário móvel, confira <xref:System.IO.IsolatedStorage.IsolatedStorageScope.Roaming?displayProperty=nameWithType>.  
  
 Combinando os conceitos de usuário, domínio e identidade de assembly, o armazenamento isolado pode isolar dados das seguintes maneiras, cada uma das quais tem seus próprios cenários de uso:  
  
- [Isolamento por usuário e assembly](#UserAssembly)  
  
- [Isolamento por usuário, domínio e assembly](#UserDomainAssembly)  
  
 Um desses isolamentos pode ser combinado a um perfil de usuário móvel. Para saber mais, confira a seção [Armazenamento isolado e roaming](#Roaming).  
  
 A seguinte ilustração demonstra como os repositórios são isolados em escopos diferentes:  
  
 ![Diagrama que mostra o isolamento por usuário e assembly.](./media/types-of-isolation/isolated-storage-types.gif)  
  
 Exceto para armazenamentos de roaming, o armazenamento isolado é sempre isolado implicitamente pelo computador porque ele usa os recursos de armazenamento que são locais para um determinado computador.  
  
> [!IMPORTANT]
> O armazenamento isolado não está disponível para aplicativos da loja do Windows 8. x. Em vez disso, use as classes de dados de aplicativos nos namespaces `Windows.Storage` incluídos na API do Windows Runtime para armazenar dados e arquivos locais. Para saber mais, confira [Dados de aplicativo](https://docs.microsoft.com/previous-versions/windows/apps/hh464917(v=win.10)) no Centro de Desenvolvimento do Windows.  
  
<a name="UserAssembly"></a>
## <a name="isolation-by-user-and-assembly"></a>Isolamento por usuário e assembly  
 Quando o assembly que usa o repositório de dados precisa ser acessível de qualquer domínio de aplicativo, o isolamento por usuário e assembly é apropriado. Normalmente, nessa situação, o armazenamento isolado é usado para armazenar dados que se aplicam a vários aplicativos e não estão vinculados a um aplicativo específico, como o nome do usuário ou informações de licença. Para acessar o armazenamento isolado por usuário e assembly, o código deve ser confiável para transferir informações entre aplicativos. Normalmente, o isolamento pelo usuário e assembly é permitido em intranets, mas não na Internet. Chamar o método estático <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A?displayProperty=nameWithType> e passar um usuário e um assembly <xref:System.IO.IsolatedStorage.IsolatedStorageScope> retorna um armazenamento com esse tipo de isolamento.  
  
 O exemplo de código a seguir recupera um armazenamento que é isolado por usuário e assembly. O repositório pode ser acessado por meio do objeto `isoFile`.  
  
 [!code-cpp[Conceptual.IsolatedStorage#17](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source11.cpp#17)]
 [!code-csharp[Conceptual.IsolatedStorage#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source11.cs#17)]
 [!code-vb[Conceptual.IsolatedStorage#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source11.vb#17)]  
  
 Para obter um exemplo que usa os parâmetros de evidência, confira <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%28System.IO.IsolatedStorage.IsolatedStorageScope%2CSystem.Security.Policy.Evidence%2CSystem.Type%2CSystem.Security.Policy.Evidence%2CSystem.Type%29>.  
  
 O método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A> está disponível como um atalho, conforme mostrado no exemplo de código a seguir. Este atalho não pode ser usado para abrir repositórios compatíveis com roaming. Use <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> nesses casos.  
  
 [!code-cpp[Conceptual.IsolatedStorage#18](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source11.cpp#18)]
 [!code-csharp[Conceptual.IsolatedStorage#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source11.cs#18)]
 [!code-vb[Conceptual.IsolatedStorage#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source11.vb#18)]  
  
<a name="UserDomainAssembly"></a>
## <a name="isolation-by-user-domain-and-assembly"></a>Isolamento por usuário, domínio e assembly  
 Se o aplicativo usa um assembly de terceiros que exige um armazenamento de dados privados, você pode usar o armazenamento isolado para armazenar os dados privados. O isolamento por usuário, domínio e assembly garante que somente o código em determinado assembly possa acessar os dados e somente quando o assembly for usado pelo aplicativo que estava em execução quando o assembly criou o repositório e somente quando o usuário para quem o armazenamento foi criado executar o aplicativo. O isolamento por usuário, domínio e assembly impede que o assembly de terceiros vaze dados para outros aplicativos. Esse tipo de isolamento deverá ser a opção padrão, se você souber que deseja usar o armazenamento isolado e não tiver certeza de qual tipo de isolamento usar. Chamar o método estático <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> de <xref:System.IO.IsolatedStorage.IsolatedStorageFile> e passar um usuário, domínio e assembly <xref:System.IO.IsolatedStorage.IsolatedStorageScope> retorna um armazenamento com esse tipo de isolamento.  
  
 O exemplo de código a seguir recupera um armazenamento isolado por usuário, domínio e assembly. O repositório pode ser acessado por meio do objeto `isoFile`.  
  
 [!code-cpp[Conceptual.IsolatedStorage#14](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source10.cpp#14)]
 [!code-csharp[Conceptual.IsolatedStorage#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source10.cs#14)]
 [!code-vb[Conceptual.IsolatedStorage#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source10.vb#14)]  
  
 Outro método está disponível como um atalho, conforme mostrado no exemplo de código a seguir. Este atalho não pode ser usado para abrir repositórios compatíveis com roaming. Use <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> nesses casos.  
  
 [!code-cpp[Conceptual.IsolatedStorage#15](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source10.cpp#15)]
 [!code-csharp[Conceptual.IsolatedStorage#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source10.cs#15)]
 [!code-vb[Conceptual.IsolatedStorage#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source10.vb#15)]  
  
<a name="Roaming"></a>
## <a name="isolated-storage-and-roaming"></a>Armazenamento isolado e roaming  
 Os perfis de usuário móvel são um recurso do Windows que permite que um usuário configure uma identidade em uma rede e use essa identidade para fazer logon em qualquer computador da rede, mantendo todas as configurações personalizadas. Um assembly que usa o armazenamento isolado pode especificar que o armazenamento isolado do usuário deve migrar com o perfil de usuário móvel. O roaming pode ser usado em conjunto com o isolamento por usuário e assembly ou com o isolamento por usuário, domínio e assembly. Se um escopo móvel não for usado, os repositórios não entrarão em roaming, mesmo que um perfil de usuário móvel seja usado.  
  
 O exemplo de código a seguir recupera um armazenamento móvel isolado por usuário e assembly. O repositório pode ser acessado por meio do objeto `isoFile`.  
  
 [!code-cpp[Conceptual.IsolatedStorage#11](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source9.cpp#11)]
 [!code-csharp[Conceptual.IsolatedStorage#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source9.cs#11)]
 [!code-vb[Conceptual.IsolatedStorage#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source9.vb#11)]  
  
 Um escopo de domínio pode ser adicionado para criar um armazenamento móvel isolado por usuário, domínio e aplicativo. O código de exemplo a seguir demonstra isso.  
  
 [!code-cpp[Conceptual.IsolatedStorage#12](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source9.cpp#12)]
 [!code-csharp[Conceptual.IsolatedStorage#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source9.cs#12)]
 [!code-vb[Conceptual.IsolatedStorage#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source9.vb#12)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.IO.IsolatedStorage.IsolatedStorageScope>
- [Armazenamento isolado](isolated-storage.md)
