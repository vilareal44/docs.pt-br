---
title: Arquivos mapeados na memória
description: Explore os arquivos mapeados na memória no .NET, que contêm o conteúdo do arquivo na memória virtual, e permita que os aplicativos modifiquem o arquivo gravando diretamente na memória.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- memory-mapped files
- inter-process communication
ms.assetid: a483d1b5-64aa-45b6-86ef-11b859f7f02e
ms.openlocfilehash: db63c15357b0670c55b1174b91b02e2f49a0c4c1
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84661973"
---
# <a name="memory-mapped-files"></a>Arquivos mapeados na memória
Um arquivo mapeado pela memória tem o conteúdo de um arquivo em memória virtual. Esse mapeamento entre um espaço de arquivo e a memória permite que um aplicativo, inclusive vários processos, modifique o arquivo ao ler e gravar diretamente na memória. A partir do .NET Framework 4, é possível usar o código gerenciado para acessar arquivos mapeados na memória da mesma maneira que funções nativas do Windows acessam arquivos mapeados na memória, conforme descrito em [Gerenciamento de arquivos mapeados na memória](https://docs.microsoft.com/previous-versions/ms810613(v=msdn.10)).  
  
 Há dois tipos de arquivos mapeados na memória:  
  
- Arquivos persistentes mapeados na memória  
  
     Arquivos persistentes são arquivos mapeados na memória associados a um arquivo de origem em um disco. Quando o último processo termina de trabalhar com o arquivo, os dados são salvos no arquivo de origem no disco. Esses arquivos mapeados na memória são adequados para trabalhar com arquivos de origem muito grandes.  
  
- Arquivos não persistentes mapeados na memória  
  
     Arquivos não persistentes são arquivos mapeados na memória não associados a um arquivo em disco. Quando o último processo termina de trabalhar com o arquivo, os dados são perdidos e o arquivo é solicitado pela coleta de lixo. Esses arquivos são adequados para a criação de memória compartilhada para comunicações entre processos (IPC).  
  
## <a name="processes-views-and-managing-memory"></a>Processos, exibição e gerenciamento de memória  
 Os arquivos mapeados na memória podem ser compartilhados entre vários processos. Os processos podem mapear para o mesmo arquivo mapeado na memória usando um nome comum, que é atribuído pelo processo que criou o arquivo.  
  
 Para trabalhar com um arquivo mapeado na memória, você deve criar uma exibição de todo o arquivo mapeado na memória ou de parte dele. Também é possível criar vários modos de exibição para a mesma parte do arquivo mapeado na memória, criando assim memórias simultâneas. Para que dois modos de exibição permaneçam simultâneos, precisam ser criados usando o mesmo arquivo de memória mapeada.  
  
 Vários modos de exibição também poderão ser necessários se o arquivo for maior que o tamanho do espaço de memória lógica do aplicativo disponível para o mapeamento de memória (2GB em um computador de 32 bits).  
  
 Há dois tipos de modos de exibição: modo de exibição de acesso por fluxo e modo de exibição de acesso aleatório. Use os modos de exibição de acesso por fluxo para acessos sequenciais a um arquivo. Isso é recomendado para arquivos não persistentes e IPCs. A preferência é para os modos de exibição de acessos aleatórios para trabalhos com arquivos persistentes.  
  
 Os arquivos mapeados na memória são acessados pelo gerenciador de memória do sistema operacional, então o arquivo é automaticamente particionado em um número de páginas e acessado conforme a necessidade. Não é preciso lidar com o gerenciamento de memória por conta própria.  
  
 A ilustração a seguir mostra como vários processos podem ter modos de exibição variados e sobrepostos para o mesmo arquivo mapeado na memória ao mesmo tempo.

 A seguinte imagem mostra várias exibições sobrepostas em um arquivo mapeado em memória:  
  
 ![Captura de tela que mostra as exibições em um arquivo mapeado em memória.](./media/memory-mapped-files/memory-map-persist-file.png)  
  
## <a name="programming-with-memory-mapped-files"></a>Programar com arquivos mapeados na memória  
 A tabela a seguir orienta como usar objetos de arquivos mapeados na memória e os membros deles.  
  
|Tarefa|Métodos ou propriedades a serem usados|  
|----------|----------------------------------|  
|Para obter um objeto <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> de um arquivo no disco que representa um arquivo persistente mapeado na memória.|Método <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateFromFile%2A?displayProperty=nameWithType>.|  
|Para obter um objeto <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> que representa um arquivo não persistente mapeado na memória (não associado a um arquivo no disco).|Método <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateNew%2A?displayProperty=nameWithType>.<br /><br /> - ou -<br /><br /> Método <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateOrOpen%2A?displayProperty=nameWithType>.|  
|Para obter um objeto <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> de um arquivo mapeado na memória existente (persistente ou não persistente).|Método <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.OpenExisting%2A?displayProperty=nameWithType>.|  
|Para obter um objeto <xref:System.IO.UnmanagedMemoryStream> para uma exibição de acesso em sequência para o arquivo mapeado na memória.|Método <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateViewStream%2A?displayProperty=nameWithType>.|  
|Para obter um objeto <xref:System.IO.UnmanagedMemoryAccessor> para uma exibição de acesso aleatório para um arquivo mapeado na memória.|Método <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateViewAccessor%2A?displayProperty=nameWithType>.|  
|Para obter um objeto <xref:Microsoft.Win32.SafeHandles.SafeMemoryMappedViewHandle> a ser usado com um código não gerenciado.|Propriedade <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.SafeMemoryMappedFileHandle%2A?displayProperty=nameWithType>.<br /><br /> - ou -<br /><br /> Propriedade <xref:System.IO.MemoryMappedFiles.MemoryMappedViewAccessor.SafeMemoryMappedViewHandle%2A?displayProperty=nameWithType>.<br /><br /> - ou -<br /><br /> Propriedade <xref:System.IO.MemoryMappedFiles.MemoryMappedViewStream.SafeMemoryMappedViewHandle%2A?displayProperty=nameWithType>.|  
|Para atrasar a alocação de memória até que uma exibição seja criada (somente arquivos não persistentes).<br /><br /> Para determinar o tamanho de página atual do sistema, use a propriedade <xref:System.Environment.SystemPageSize%2A?displayProperty=nameWithType>.|Método <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateNew%2A> com o valor <xref:System.IO.MemoryMappedFiles.MemoryMappedFileOptions.DelayAllocatePages?displayProperty=nameWithType>.<br /><br /> - ou -<br /><br /> Métodos <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateOrOpen%2A> que possuem uma enumeração <xref:System.IO.MemoryMappedFiles.MemoryMappedFileOptions> como parâmetro.|  
  
### <a name="security"></a>Segurança  
 É possível aplicar os direitos de acesso ao criar um arquivo mapeado na memória usando os seguintes métodos que usam uma enumeração <xref:System.IO.MemoryMappedFiles.MemoryMappedFileAccess> como parâmetro:  
  
- <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateFromFile%2A?displayProperty=nameWithType>  
  
- <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateNew%2A?displayProperty=nameWithType>  
  
- <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateOrOpen%2A?displayProperty=nameWithType>  
  
 É possível especificar os direitos de acesso para abrir um arquivo mapeado na memória existente usando os métodos <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.OpenExisting%2A> que usam <xref:System.IO.MemoryMappedFiles.MemoryMappedFileRights> como parâmetro.  
  
 Além disso, é possível incluir um objeto <xref:System.IO.MemoryMappedFiles.MemoryMappedFileSecurity> que contém regras de acesso predefinidas.  
  
 Para aplicar as regras de acesso novas ou modificadas a um arquivo mapeado na memória, use o método <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.SetAccessControl%2A>. Para recuperar as regras de acesso ou de auditoria de um arquivo existente, use o método <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.GetAccessControl%2A>.  
  
## <a name="examples"></a>Exemplos  
  
### <a name="persisted-memory-mapped-files"></a>Arquivos persistentes mapeados na memória  
 Os métodos <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateFromFile%2A> criam um arquivo mapeado na memória de um arquivo existente no disco.  
  
 O exemplo a seguir cria uma exibição de mapeamento na memória de parte de um arquivo muito grande e manipula uma porção dele.  
  
 [!code-csharp[MemoryMappedFiles.MemoryMappedFile.CreateFromFile#1](../../../samples/snippets/csharp/VS_Snippets_CLR/memorymappedfiles.memorymappedfile.createfromfile/cs/program.cs#1)]
 [!code-vb[MemoryMappedFiles.MemoryMappedFile.CreateFromFile#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/memorymappedfiles.memorymappedfile.createfromfile/vb/program.vb#1)]  

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

 O exemplo a seguir abre o mesmo arquivo mapeado na memória para outro processo.  
  
 [!code-csharp[MemoryMappedFiles.MemoryMappedFile.OpenExisting#1](../../../samples/snippets/csharp/VS_Snippets_CLR/memorymappedfiles.memorymappedfile.openexisting/cs/program.cs#1)]
 [!code-vb[MemoryMappedFiles.MemoryMappedFile.OpenExisting#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/memorymappedfiles.memorymappedfile.openexisting/vb/program.vb#1)]  
  
### <a name="non-persisted-memory-mapped-files"></a>Arquivos não persistentes mapeados na memória  
 Os métodos <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateNew%2A> e <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateOrOpen%2A> criam um arquivo mapeado na memória que não está mapeado para um arquivo existente no disco.  
  
 O exemplo a seguir consiste de três processos separados (aplicativos de console) que gravam valores booleanos em um arquivo mapeado na memória. Ocorre a seguinte sequência de ações:  
  
1. O `Process A` cria o arquivo mapeado na memória e grava um valor para ele.  
  
2. O `Process B` abre o arquivo mapeado na memória e grava um valor para ele.  
  
3. O `Process C` abre o arquivo mapeado na memória e grava um valor para ele.  
  
4. O `Process A` lê e exibe os valores do arquivo mapeado na memória.  
  
5. Após o `Process A` concluir com o arquivo mapeado na memória, o arquivo é imediatamente recuperado pela coleta de lixo.  
  
 Para executar este exemplo, faça o seguinte:  
  
1. Compile os aplicativos e abra três janelas de Prompt de comando.  
  
2. Na primeira janela do prompt, execute o `Process A`.  
  
3. Na segunda janela, execute o `Process B`.  
  
4. Retorne ao `Process A` e pressione ENTER.  
  
5. Na terceira janela do prompt, execute o `Process C`.  
  
6. Retorne ao `Process A` e pressione ENTER.  
  
 A saída do `Process A` é a seguinte:  
  
```console  
Start Process B and press ENTER to continue.  
Start Process C and press ENTER to continue.  
Process A says: True  
Process B says: False  
Process C says: True  
```  
  
 **Processo A**  
  
 [!code-csharp[System.IO.MemoryMappedFiles_IPC_X#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_x/cs/program.cs#1)]
 [!code-vb[System.IO.MemoryMappedFiles_IPC_X#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_x/vb/program.vb#1)]  
  
 **Processo B**  
  
 [!code-csharp[System.IO.MemoryMappedFiles_IPC_A#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_a/cs/program.cs#1)]
 [!code-vb[System.IO.MemoryMappedFiles_IPC_A#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_a/vb/program.vb#1)]  
  
 **Processo C**  
  
 [!code-csharp[System.IO.MemoryMappedFiles_IPC_B#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_b/cs/program.cs#1)]
 [!code-vb[System.IO.MemoryMappedFiles_IPC_B#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_b/vb/program.vb#1)]  
  
## <a name="see-also"></a>Confira também

- [Arquivo e e/s de fluxo](index.md)
