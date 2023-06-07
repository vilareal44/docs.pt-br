---
title: Classes usadas em E/S de Arquivo do .NET Framework e o Sistema de Arquivos
ms.date: 07/20/2015
helpviewer_keywords:
- file I/O classes
ms.assetid: 4a5ca924-eea8-4a95-a5f0-6ac10de276a3
ms.openlocfilehash: 2c696d5b8c83ae55caf61e4710630ad1e34c088d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401765"
---
# <a name="classes-used-in-net-framework-file-io-and-the-file-system-visual-basic"></a>Classes usadas em E/S de arquivo do .NET Framework e o sistema de arquivos (Visual Basic)

As tabelas a seguir listam as classes usadas comumente para E/S de arquivos do .NET Framework, categorizadas em classes de E/S de arquivos, classes usadas para criar fluxos e classes usadas para ler e gravar em fluxos.  
  
Para obter uma listagem mais abrangente, consulte [Visão Geral da Biblioteca de Classes](../../../../standard/class-library-overview.md).  
  
## <a name="basic-io-classes-for-files-drives-and-directories"></a>Classes básicas de E/S para arquivos, unidades e pastas  

 A tabela a seguir lista e descreve as principais classes usadas para E/S de arquivos.  
  
|Classe|Descrição|  
|-----------|-----------------|  
|<xref:System.IO.Directory?displayProperty=nameWithType>|Fornece métodos estáticos para criar, mover e enumerar ao longo de diretórios e subdiretórios.|  
|<xref:System.IO.DirectoryInfo?displayProperty=nameWithType>|Fornece métodos de instância para criar, mover e enumerar ao longo de diretórios e subdiretórios.|  
|<xref:System.IO.DriveInfo?displayProperty=nameWithType>|Fornece métodos de instância para criar, mover e enumerar ao longo de unidades.|  
|<xref:System.IO.File?displayProperty=nameWithType>|Fornece métodos estáticos para criar, copiar, excluir, mover e abrir arquivos, além de ajudar na criação de um `FileStream`.|  
|<xref:System.IO.FileAccess?displayProperty=nameWithType>|Define constantes para acesso de leitura, gravação ou leitura/gravação para um arquivo.|  
|<xref:System.IO.FileAttributes?displayProperty=nameWithType>|Fornece atributos para arquivos e diretórios como `Archive`, `Hidden` e `ReadOnly`.|  
|<xref:System.IO.FileInfo?displayProperty=nameWithType>|Fornece métodos estáticos para criar, copiar, excluir, mover e abrir arquivos, além de ajudar na criação de um `FileStream`.|  
|<xref:System.IO.FileMode?displayProperty=nameWithType>|Controla como um arquivo é aberto. Este parâmetro é especificado em muitos dos construtores para `FileStream` e `IsolatedStorageFileStream`, também para os métodos `Open` de <xref:System.IO.File> e <xref:System.IO.FileInfo>.|  
|<xref:System.IO.FileShare?displayProperty=nameWithType>|Define constantes para controlar o tipo de acesso que outros fluxos de arquivos podem ter ao mesmo arquivo.|  
|<xref:System.IO.Path?displayProperty=nameWithType>|Fornece métodos e propriedades para processar cadeias de caracteres de diretório.|  
|<xref:System.Security.Permissions.FileIOPermission?displayProperty=nameWithType>|Controla o acesso de arquivos e pastas definindo permissões <xref:System.Security.Permissions.FileIOPermissionAttribute.Read%2A>, <xref:System.Security.Permissions.FileIOPermissionAttribute.Write%2A>, <xref:System.Security.Permissions.FileIOPermissionAttribute.Append%2A> e <xref:System.Security.Permissions.FileIOPermissionAttribute.PathDiscovery%2A>.|  
  
## <a name="classes-used-to-create-streams"></a>Classes usadas para criar fluxos  

 A tabela a seguir lista e descreve as principais classes usadas para criar fluxos.  
  
|Classe|Descrição|  
|-----------|-----------------|  
|<xref:System.IO.BufferedStream?displayProperty=nameWithType>|Adiciona uma camada de armazenamento em buffer para ler e gravar operações em outro fluxo.|  
|<xref:System.IO.FileStream?displayProperty=nameWithType>|Dá suporte ao acesso aleatório a arquivos por meio de seu método <xref:System.IO.FileStream.Seek%2A>. <xref:System.IO.FileStream> abre arquivos de forma síncrona por padrão, mas também dá suporte à operação assíncrona.|  
|<xref:System.IO.MemoryStream?displayProperty=nameWithType>|Cria um fluxo cujo repositório de backup é a memória, em vez de um arquivo.|  
|<xref:System.Net.Sockets.NetworkStream?displayProperty=nameWithType>|Fornece o fluxo de dados subjacente para acesso à rede.|  
|<xref:System.Security.Cryptography.CryptoStream?displayProperty=nameWithType>|Define uma transmissão que liga fluxos de dados a transformações criptográficas.|  
  
## <a name="classes-used-to-read-from-and-write-to-streams"></a>Classes usadas para ler e gravar em fluxos  

 A tabela a seguir mostra as classes específicas usadas para ler e gravar em arquivos com fluxos.  
  
|**Classe**|**Descrição**|  
|---------------|---------------------|  
|<xref:System.IO.BinaryReader?displayProperty=nameWithType>|Lê cadeias de caracteres codificadas e tipos de dados primitivos de um <xref:System.IO.FileStream>.|  
|<xref:System.IO.BinaryWriter?displayProperty=nameWithType>|Grava cadeias de caracteres codificadas e tipos de dados primitivos em um <xref:System.IO.FileStream>.|  
|<xref:System.IO.StreamReader?displayProperty=nameWithType>|Lê caracteres de um <xref:System.IO.FileStream>, usando <xref:System.IO.StreamReader.CurrentEncoding%2A> para converter caracteres em bytes e vice-versa. <xref:System.IO.StreamReader> tem um construtor que tenta determinar o <xref:System.IO.StreamReader.CurrentEncoding%2A> correto de determinado fluxo, com base na presença de um preâmbulo específico de <xref:System.IO.StreamReader.CurrentEncoding%2A>, como uma marca de ordem de byte.|  
|<xref:System.IO.StreamWriter?displayProperty=nameWithType>|Grava caracteres em um `FileStream`, usando <xref:System.IO.StreamWriter.Encoding%2A> para converter caracteres em bytes.|  
|<xref:System.IO.StringReader?displayProperty=nameWithType>|Lê caracteres de um `String`. A saída pode ser um fluxo em qualquer codificação ou um `String`.|  
|<xref:System.IO.StringWriter?displayProperty=nameWithType>|Grava caracteres em um `String`. A saída pode ser um fluxo em qualquer codificação ou um `String`.|  
  
## <a name="see-also"></a>Confira também

- [Compor fluxos](../../../../standard/io/composing-streams.md)
- [Arquivo e e/s de fluxo](../../../../standard/io/index.md)
- [E/s de arquivo assíncrono](../../../../standard/io/asynchronous-file-i-o.md)
- [Noções básicas de E/S de arquivo do .NET Framework e o sistema de arquivos (Visual Basic)](basics-of-net-framework-file-io-and-the-file-system.md)
