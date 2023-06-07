---
title: Noções Básicas de E/S de Arquivo do .NET Framework e o Sistema de Arquivos
ms.date: 07/20/2015
helpviewer_keywords:
- file access, file I/O in Visual Basic
- file attributes, determining
- streams, fundamental operations
- file permissions
- streams
- streams, definition
ms.assetid: 49d837c0-cf28-416f-8606-4d83d7b479ef
ms.openlocfilehash: 187a20617ec901e722a30ebfa571e4a55ed0b5c3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401791"
---
# <a name="basics-of-net-framework-file-io-and-the-file-system-visual-basic"></a>Noções básicas de E/S de arquivo do .NET Framework e o sistema de arquivos (Visual Basic)

As classes no namespace <xref:System.IO> são usadas para trabalhar com unidades, arquivos e diretórios.

O namespace <xref:System.IO> contém as classes <xref:System.IO.File> e <xref:System.IO.Directory>, que fornecem a funcionalidade do .NET Framework que manipula arquivos e diretórios. Como os métodos desses objetos são estáticos ou membros compartilhados, você pode usá-los diretamente sem antes criar uma instância da classe. Associadas a essas classes estão as classes <xref:System.IO.FileInfo> e <xref:System.IO.DirectoryInfo>, conhecidas pelos usuários do recurso `My`. Para usar essas classes, você deve qualificar totalmente os nomes ou importar os namespaces apropriados, incluindo as declarações `Imports` no início do código afetado. Para obter mais informações, consulte [Instrução Imports (tipo e namespace .NET)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md).

> [!NOTE]
> Os outros tópicos nesta seção usam o objeto `My.Computer.FileSystem`, em vez das classes `System.IO`, para trabalhar com unidades, arquivos e diretórios. O objeto `My.Computer.FileSystem` destina-se principalmente a ser usado em programas do Visual Basic. As classes `System.IO` destinam-se a serem usadas por qualquer linguagem compatível com o .NET Framework, incluindo o Visual Basic.

## <a name="definition-of-a-stream"></a>Definição de um fluxo

O .NET Framework usa fluxos para dar suporte à leitura e gravação em arquivos. Você pode pensar em um fluxo como um conjunto unidimensional de dados contíguos, que tem um início e um fim e um local em que o cursor indica a posição atual no fluxo.

![O cursor mostra a posição atual no fluxo de arquivos.](./media/basics-of-net-framework-file-io-and-the-file-system/filestream-cursor-position.gif)

## <a name="stream-operations"></a>Operações de fluxo

Os dados contidos no fluxo podem vir da memória, de um arquivo ou de um soquete TCP/IP. Os fluxos têm operações fundamentais que podem ser aplicadas a eles:

- **Lendo**. Você pode ler em um fluxo, transferindo dados do fluxo para uma estrutura de dados, como uma cadeia de caracteres ou uma matriz de bytes.

- **Gravando**. Você pode gravar em um fluxo, transferindo dados de uma fonte de dados para o fluxo.

- **Busca**. Você pode consultar e modificar a sua posição no fluxo.

Para obter mais informações, consulte [Compondo fluxos](../../../../standard/io/composing-streams.md).

## <a name="types-of-streams"></a>Tipos de fluxos

No .NET Framework, um fluxo é representado pela classe <xref:System.IO.Stream>, que forma a classe abstrata para todos os outros fluxos. Você não pode criar diretamente uma instância da classe <xref:System.IO.Stream>, mas deve usar uma das classes que ela implementa.

Há muitos tipos de fluxos, porém, com a finalidade de trabalhar com entrada/saída (E/S) de arquivos, os tipos mais importantes são a classe <xref:System.IO.FileStream>, que fornece uma maneira de ler e gravar arquivos, e a classe <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream>, que fornece uma maneira de criar arquivos e diretórios em armazenamento isolado. Outros fluxos que podem ser usados ao trabalhar com E/S de arquivo incluem:

- <xref:System.IO.BufferedStream>

- <xref:System.Security.Cryptography.CryptoStream>

- <xref:System.IO.MemoryStream>

- <xref:System.Net.Sockets.NetworkStream>.

A tabela a seguir lista as tarefas comumente realizadas com um fluxo:

|Para|Consulte|
|---|---|
|Ler e gravar em um arquivo de dados|[Como: Ler e gravar em um arquivo de dados recém-criado](../../../../standard/io/how-to-read-and-write-to-a-newly-created-data-file.md)|
|Ler texto de um arquivo|[Como: Ler texto de um arquivo](../../../../standard/io/how-to-read-text-from-a-file.md)|
|Gravar texto em um arquivo|[Como: gravar texto em um arquivo](../../../../standard/io/how-to-write-text-to-a-file.md)|
|Ler caracteres de uma cadeia de caracteres|[Como: Ler caracteres de uma cadeia de caracteres](../../../../standard/io/how-to-read-characters-from-a-string.md)|
|Gravar caracteres em uma cadeia de caracteres|[Como: Gravar caracteres em uma cadeia de caracteres](../../../../standard/io/how-to-write-characters-to-a-string.md)|
|Criptografar dados|[Criptografando dados](../../../../standard/security/encrypting-data.md)|
|Descriptografar dados|[Descriptografando dados](../../../../standard/security/decrypting-data.md)|

## <a name="file-access-and-attributes"></a>Acesso a arquivos e atributos

Você pode controlar como os arquivos são criados, abertos e compartilhados com as enumerações <xref:System.IO.FileAccess>, <xref:System.IO.FileMode> e <xref:System.IO.FileShare>, que contêm os sinalizadores usados pelos construtores da classe <xref:System.IO.FileStream>. Por exemplo, quando você abre ou cria uma nova <xref:System.IO.FileStream>, a enumeração <xref:System.IO.FileMode> permite especificar se o arquivo está aberto para acréscimos, se um novo arquivo será criado caso o arquivo especificado não exista, se o arquivo será substituído e assim por diante.

A enumeração <xref:System.IO.FileAttributes> habilita a coleta de informações específicas do arquivo. A enumeração <xref:System.IO.FileAttributes> retorna os atributos armazenados do arquivo, por exemplo, se ele é compactado, criptografado, oculto, somente leitura, um arquivo, um diretório, um arquivo de sistema ou um arquivo temporário.

A tabela a seguir lista as tarefas que envolvem o acesso a arquivos e atributos de arquivo:

|Para|Consulte|
|---|---|
|Abrir e acrescentar texto a um arquivo de log|[Como: Abrir um arquivo de log e fazer acréscimos a ele](../../../../standard/io/how-to-open-and-append-to-a-log-file.md)|
|Determinar os atributos de um arquivo|<xref:System.IO.FileAttributes>|

## <a name="file-permissions"></a>Permissões de arquivo

É possível controlar o acesso a arquivos e diretórios com a classe <xref:System.Security.Permissions.FileIOPermission>. Isso pode ser especialmente importante para desenvolvedores que trabalham com Web Forms, que, por padrão, são executados no contexto de uma conta de usuário local especial denominada ASPNET, que é criada como parte das instalações do ASP.NET e do .NET Framework. Quando esse tipo de aplicativo solicita acesso a um recurso, a conta de usuário ASPNET tem permissões limitadas, que podem impedir que o usuário realize ações como gravar em um arquivo de um aplicativo Web. Para obter mais informações, consulte <xref:System.Security.Permissions.FileIOPermission>.

## <a name="isolated-file-storage"></a>Armazenamento isolado de arquivo

O armazenamento isolado é uma tentativa de resolver problemas criados ao trabalhar com arquivos em que o usuário ou código pode não ter as permissões necessárias. O armazenamento isolado atribui a cada usuário um compartimento de dados, que pode conter uma ou mais repositórios. Os repositórios podem ser isolados uns dos outros, por usuário e por assembly. Somente o usuário e o assembly que criou um repositório tem acesso a ele. Um repositório atua como um sistema de arquivos virtual completo. Você pode criar e manipular pastas e arquivos em um repositório.

A tabela a seguir lista as tarefas comumente associadas ao armazenamento isolado de arquivo.

|Para|Consulte|
|---|---|
|Criar um repositório isolado|[Como: Obter repositórios para o armazenamento isolado](../../../../standard/io/how-to-obtain-stores-for-isolated-storage.md)|
|Enumerar repositórios isolados|[Como: Enumerar repositórios para o armazenamento isolado](../../../../standard/io/how-to-enumerate-stores-for-isolated-storage.md)|
|Excluir um repositório isolado|[Como: Excluir repositórios no armazenamento isolado](../../../../standard/io/how-to-delete-stores-in-isolated-storage.md)|
|Criar um arquivo ou diretório no armazenamento isolado|[Como: Criar arquivos e diretórios no armazenamento isolado](../../../../standard/io/how-to-create-files-and-directories-in-isolated-storage.md)|
|Localizar um arquivo no armazenamento isolado|[Como: Localizar arquivos e diretórios existentes no armazenamento isolado](../../../../standard/io/how-to-find-existing-files-and-directories-in-isolated-storage.md)|
|Ler ou gravar em um arquivo no armazenamento isolado|[Como: Ler e gravar em arquivos no armazenamento isolado](../../../../standard/io/how-to-read-and-write-to-files-in-isolated-storage.md)|
|Excluir um arquivo ou diretório no armazenamento isolado|[Como: Excluir arquivos e diretórios no armazenamento isolado](../../../../standard/io/how-to-delete-files-and-directories-in-isolated-storage.md)|

## <a name="file-events"></a>Eventos de arquivo

O componente <xref:System.IO.FileSystemWatcher> permite observar alterações em arquivos e diretórios em seu sistema ou em qualquer computador em que você tem acesso à rede. Por exemplo, se um arquivo é modificado, convém enviar a um usuário um alerta sobre a alteração ocorrida. Quando ocorrem alterações, um ou mais eventos são acionados, armazenados em um buffer e enviados ao componente <xref:System.IO.FileSystemWatcher> para processamento.

## <a name="see-also"></a>Confira também

- [Compor fluxos](../../../../standard/io/composing-streams.md)
- [Arquivo e e/s de fluxo](../../../../standard/io/index.md)
- [E/s de arquivo assíncrono](../../../../standard/io/asynchronous-file-i-o.md)
- [Classes usadas em E/S de arquivo do .NET Framework e o sistema de arquivos (Visual Basic)](classes-used-in-net-framework-file-io-and-the-file-system.md)
