---
title: E/S de arquivo e de fluxo – .NET
description: Aprenda as noções básicas de e/s de arquivo e fluxo, que é a transferência de dados de ou para um meio de armazenamento, no .NET.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- IO namespace
- files, I/O
- System.IO namespace
- I/O [.NET Framework]
- streams, I/O
- data streams, I/O
ms.assetid: 4f4a33a9-66b7-4cd7-a285-4ad3e4276cd2
ms.openlocfilehash: 2761d17846009ba06a2ffb1fc58b430f3ec9a949
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662713"
---
# <a name="file-and-stream-io"></a>E/S de arquivo e de fluxo

E/S (entrada/saída) de arquivos e fluxos refere-se à transferência de dados de ou para uma mídia de armazenamento. No .NET Framework, os namespaces `System.IO` contêm tipos que permitem a leitura e a gravação, de forma síncrona e assíncrona, em fluxos de dados e arquivos. Esses namespaces também contêm tipos que executam compactação e descompactação em arquivos e tipos que possibilitam a comunicação por meio de pipes e portas seriais.

Um arquivo é uma coleção ordenada e nomeada de bytes com armazenamento persistente. Ao trabalhar com arquivos, você trabalha com caminhos de diretórios, armazenamento em disco e nomes de arquivos e diretórios. Por outro lado, um fluxo é uma sequência de bytes que você pode usar para ler e gravar em um repositório, o qual pode ser uma entre vários tipos de mídia de armazenamento (por exemplo, discos ou memória). Assim como há vários repositórios diferentes de discos, há vários tipos diferentes de fluxos diferentes de fluxos de arquivos, como os fluxos de rede, memória e pipes.

## <a name="files-and-directories"></a>Arquivos e diretórios

Você pode usar os tipos no namespace <xref:System.IO?displayProperty=nameWithType> para interagir com arquivos e diretórios. Por exemplo, você pode obter e definir propriedades para arquivos e diretórios e recuperar coleções de arquivos e diretórios com base em critérios de pesquisa.

Para convenções de nomenclatura de caminhos e os modos de expressar um caminho de arquivo para sistemas Windows, incluindo a sintaxe de dispositivo DOS compatível com o .NET Core 1.1 e posterior e o .NET Framework 4.6.2 e posterior, veja [Formatos de caminho de arquivo em sistemas Windows](file-path-formats.md).

Aqui estão algumas classes de arquivos e diretórios comumente usadas:

- <xref:System.IO.File> – Fornece métodos estáticos para criar, copiar, excluir, mover e abrir arquivos, além de ajudar na criação de um objeto <xref:System.IO.FileStream>.

- <xref:System.IO.FileInfo> – Fornece métodos de instâncias para criar, copiar, excluir, mover e abrir arquivos, além de ajudar na criação de um objeto <xref:System.IO.FileStream>.

- <xref:System.IO.Directory> – Fornece métodos estáticos para criar, mover e enumerar ao longo de diretórios e subdiretórios.

- <xref:System.IO.DirectoryInfo> – Fornece métodos de instância para criar, mover e enumerar ao longo de diretórios e subdiretórios.

- <xref:System.IO.Path> – Fornece métodos e propriedades para processar cadeias de caracteres de diretório de uma maneira compatível com várias plataformas.

Você sempre deve fornecer tratamento de exceção robusto ao chamar métodos de sistema de arquivos. Para obter mais informações, veja [Tratamento de erros de E/S](handling-io-errors.md).

Além de usar essas classes, os usuários do Visual Basic podem usar os métodos e as propriedades fornecidas pela classe <xref:Microsoft.VisualBasic.FileIO.FileSystem?displayProperty=nameWithType> para E/S de arquivo.

Confira [Como copiar diretórios](how-to-copy-directories.md), [Como criar uma listagem de diretórios](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/5cf8zcfh(v=vs.100)) e [Como enumerar diretórios e arquivos](how-to-enumerate-directories-and-files.md).

## <a name="streams"></a>Fluxos

A classe base abstrata <xref:System.IO.Stream> oferece suporte a leitura e gravação de bytes. Todas as classes que representam fluxos herdam da classe <xref:System.IO.Stream>. A classe <xref:System.IO.Stream> e suas classes derivadas fornecem uma visão comum de fontes e repositórios de dados, isolando o programador de detalhes específicos do sistema operacional e dispositivos subjacentes.

Fluxos envolvem estas três operações fundamentais:

- Leitura – Transferência de dados de um fluxo para uma estrutura de dados, como uma matriz de bytes.

- Gravação – Transferência de dados para um fluxo a partir de uma fonte de dados.

- Busca – Consulta e modificação da posição atual em um fluxo.

Dependendo do repositório ou da fonte de dados subjacente, os fluxos podem oferecer suporte somente algumas dessas capacidades. Por exemplo, a classe <xref:System.IO.Pipes.PipeStream> não oferece suporte à operação de busca. As propriedades <xref:System.IO.Stream.CanRead%2A>, <xref:System.IO.Stream.CanWrite%2A> e <xref:System.IO.Stream.CanSeek%2A> de um fluxo especificam as operações às quais o fluxo oferece suporte.

Algumas classes de fluxo comumente usadas são:

- <xref:System.IO.FileStream> – Para leitura e gravação em um arquivo.

- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream> – Para leitura e gravação em um arquivo no armazenamento isolado.

- <xref:System.IO.MemoryStream> – Para leitura e gravação na memória como o repositório de backup.

- <xref:System.IO.BufferedStream> – Para melhorar o desempenho das operações de leitura e gravação.

- <xref:System.Net.Sockets.NetworkStream> – Para leitura e gravação via soquetes de rede.

- <xref:System.IO.Pipes.PipeStream> – Para leitura e gravação sobre pipes anônimos e nomeados.

- <xref:System.Security.Cryptography.CryptoStream> – Para vincular fluxos de dados a transformações criptográficas.

Para um exemplo de como trabalhar com fluxos de forma assíncrona, confira [E/S de arquivo assíncrona](asynchronous-file-i-o.md).

## <a name="readers-and-writers"></a>Leitores e gravadores

O namespace <xref:System.IO?displayProperty=nameWithType> também fornece tipos usados para ler caracteres codificados de fluxos e gravá-los em fluxos. Normalmente, os fluxos são criados para a entrada e a saída de bytes. Os tipos de leitor e de gravador tratam a conversão dos caracteres codificados de/para bytes para que o fluxo possa concluir a operação. Cada classe de leitor e gravador é associada a um fluxo, o qual pode ser recuperado pela propriedade `BaseStream` da classe.

Algumas classes de leitores e gravadores comumente usadas são:

- <xref:System.IO.BinaryReader> e <xref:System.IO.BinaryWriter> – Para leitura e gravação de tipos de dados primitivos como valores binários.

- <xref:System.IO.StreamReader> e <xref:System.IO.StreamWriter> – Para leitura e gravação de caracteres usando um valor de codificação para converter os caracteres para/de bytes.

- <xref:System.IO.StringReader> e <xref:System.IO.StringWriter> – Para leitura e gravação de caracteres e cadeias de caracteres.

- <xref:System.IO.TextReader> e <xref:System.IO.TextWriter> – Funcionam como as classes base abstratas para outros leitores e gravadores que leem e gravam caracteres e cadeias de caracteres, mas não dados binários.

Confira [Como ler texto de um arquivo](how-to-read-text-from-a-file.md), [Como gravar texto em um arquivo](how-to-write-text-to-a-file.md), [Como ler caracteres em uma cadeia de caracteres](how-to-read-characters-from-a-string.md) e [Como gravar caracteres em uma cadeia de caracteres](how-to-write-characters-to-a-string.md).

## <a name="asynchronous-io-operations"></a>Operações de E/S assíncronas

A leitura ou gravação de uma grande quantidade de dados pode consumir muitos recursos. Você deve executar essas tarefas de forma assíncrona se seu aplicativo precisar continuar respondendo ao usuário. Com as operações de E/S síncronas, o thread de interface do usuário é bloqueado até que a operação de uso intensivo seja concluída.  Use operações de e/s assíncronas ao desenvolver aplicativos da loja do Windows 8. x para evitar a criação da impressão de que seu aplicativo parou de funcionar.

Os membros assíncronas contêm `Async` em seus nomes, como os métodos <xref:System.IO.Stream.CopyToAsync%2A>, <xref:System.IO.Stream.FlushAsync%2A>, <xref:System.IO.Stream.ReadAsync%2A> e <xref:System.IO.Stream.WriteAsync%2A>. Você usa esses métodos com as palavras-chave `async` e `await`.

Para saber mais, confira [E/S de arquivo assíncrona](asynchronous-file-i-o.md).

## <a name="compression"></a>Compactação

A compactação refere-se ao processo de reduzir o tamanho de um arquivo para fins de armazenamento. A descompactação é o processo de extrair o conteúdo de um arquivo compactado para um formato utilizável. O namespace <xref:System.IO.Compression?displayProperty=nameWithType> contém tipos para compactar e descompactar arquivos e fluxos.

As classes a seguir são frequentemente usadas para compactar e descompactar arquivos e fluxos:

- <xref:System.IO.Compression.ZipArchive> – Para criar e recuperar entradas em arquivos ZIP.

- <xref:System.IO.Compression.ZipArchiveEntry> – Para representar um arquivo compactado.

- <xref:System.IO.Compression.ZipFile> – Para criar, extrair e abrir um pacote compactado.

- <xref:System.IO.Compression.ZipFileExtensions> – Para criar e extrair entradas em um pacote compactado.

- <xref:System.IO.Compression.DeflateStream> – Para compactar e descompactar fluxos usando o algoritmo de Deflate.

- <xref:System.IO.Compression.GZipStream> – Para compactar e descompactar fluxos no formato de dados GZIP.

Confira [How to: Compress and Extract Files](how-to-compress-and-extract-files.md) (Como compactar e extrair arquivos).

## <a name="isolated-storage"></a>Armazenamento isolado

Um armazenamento isolado é um mecanismo de armazenamento de dados que fornece isolamento e segurança ao definir maneiras padronizadas de associar códigos a dados salvos. O armazenamento fornece um sistema de arquivos virtual que é isolado por usuário, assembly e (opcionalmente) domínio. O armazenamento isolado é particularmente útil quando o aplicativo não tem permissão para acessar arquivos de usuários. Você pode salvar configurações ou arquivos para seu aplicativo de modo que ele seja controlado pela política de segurança do computador.

O armazenamento isolado não está disponível para aplicativos da loja do Windows 8. x; em vez disso, use classes de dados de aplicativo no <xref:Windows.Storage?displayProperty=nameWithType> namespace. Para saber mais, veja [Dados de aplicativo](https://docs.microsoft.com/previous-versions/windows/apps/hh464917%28v=win.10%29).

As classes a seguir são usadas com frequência na implementação do armazenamento isolado:

- <xref:System.IO.IsolatedStorage.IsolatedStorage> – Fornece a classe base para implementações de armazenamento isolado.

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile> – Fornece uma área de armazenamento isolado que contém arquivos e diretórios.

- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream> – Expõe um arquivo no armazenamento isolado.

Confira [Armazenamentos isolado](isolated-storage.md).

## <a name="io-operations-in-windows-store-apps"></a>Operações de E/S em aplicativos da Windows Store

O .NET para aplicativos da loja do Windows 8. x contém muitos dos tipos de leitura e gravação em fluxos; no entanto, esse conjunto não inclui todos os tipos de e/s de .NET Framework.

Algumas diferenças importantes a serem observadas ao usar operações de e/s em aplicativos da loja do Windows 8. x:

- Tipos especificamente relacionados a operações de arquivo, como <xref:System.IO.File> , <xref:System.IO.FileInfo> <xref:System.IO.Directory> e <xref:System.IO.DirectoryInfo> , não estão incluídos no .net para aplicativos da loja do Windows 8. x. Em vez disso, use os tipos no namespace <xref:Windows.Storage?displayProperty=nameWithType> do Windows Runtime, como <xref:Windows.Storage.StorageFile> e <xref:Windows.Storage.StorageFolder>.

- O armazenamento isolado não está disponível. Use [dados de aplicativo](https://docs.microsoft.com/previous-versions/windows/apps/hh464917(v=win.10)).

- Use métodos assíncronos, como <xref:System.IO.Stream.ReadAsync%2A> e <xref:System.IO.Stream.WriteAsync%2A>, para evitar o bloqueio do thread da interface do usuário.

- Os tipos de compactação com base em caminhos <xref:System.IO.Compression.ZipFile> e <xref:System.IO.Compression.ZipFileExtensions> não estão disponíveis. Em vez disso, use os tipos no namespace <xref:Windows.Storage.Compression?displayProperty=nameWithType>.

É possível converter entre fluxos do .NET Framework e fluxos do Windows Runtime, se necessário. Para obter mais informações, consulte [como converter entre fluxos de .NET Framework e fluxos de Windows Runtime](how-to-convert-between-dotnet-streams-and-winrt-streams.md) ou <xref:System.IO.WindowsRuntimeStreamExtensions> .

Para obter mais informações sobre operações de e/s em um aplicativo da loja do Windows 8. x, consulte [início rápido: lendo e gravando arquivos](https://docs.microsoft.com/previous-versions/windows/apps/hh758325(v=win.10)).

## <a name="io-and-security"></a>E/S e segurança

Ao usar as classes no namespace <xref:System.IO?displayProperty=nameWithType>, você deve atender aos requisitos de segurança do sistema operacional, como ACLs (listas de controle de acesso) para controlar o acesso a arquivos e diretórios. Esse é um requisito adicional aos requisitos de <xref:System.Security.Permissions.FileIOPermission>. As ACLs podem ser gerenciadas por meio de programação. Para saber mais, confira [Como adicionar ou remover entradas da lista de controle de acesso](how-to-add-or-remove-access-control-list-entries.md).

As políticas de segurança padrão impedem que aplicativos da Internet ou intranet acessem arquivos no computador do usuário. Consequentemente, não use classes de E/S que exijam um caminho para um arquivo físico ao escrever código que será baixado via Internet ou intranet. Em vez disso, use o [armazenamento isolado](isolated-storage.md) para aplicativos .NET Framework tradicionais ou use [dados de aplicativos](https://docs.microsoft.com/previous-versions/windows/apps/hh464917(v=win.10)) para aplicativos da loja do Windows 8. x.

Uma verificação de segurança é executada somente quando o fluxo é construído. Consequentemente, não abra um fluxo para depois passá-lo para código ou domínios de aplicativos menos confiáveis.

## <a name="related-topics"></a>Tópicos relacionados

- [Tarefas comuns de e/s](common-i-o-tasks.md)\
Fornece uma lista das tarefas de E/S associadas a arquivos, diretórios e fluxos, além de links para conteúdo e exemplos relevantes para cada tarefa.

- [E/s de arquivo assíncrono](asynchronous-file-i-o.md)\
Descreve as vantagens de desempenho e a operação básica da E/S assíncrona.

- [Armazenamento isolado](isolated-storage.md)\
Descreve um mecanismo de armazenamento isolado que fornece isolamento e segurança ao definir maneiras padronizadas de associar códigos aos dados salvos.

- [Especificadas](pipe-operations.md)\
Descreve operações de pipes anônimos e nomeados no .NET Framework.

- [Arquivos mapeados para memória](memory-mapped-files.md)\
Descreve arquivos mapeados na memória, os quais armazenam o conteúdo de arquivos do disco na memória virtual. Você pode usar arquivos mapeados na memória para editar arquivos muito grandes e para criar memória compartilhada para a comunicação entre processos.
