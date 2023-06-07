---
title: Contadores de desempenho no .NET Framework
description: Leia sobre contadores de desempenho no .NET. Há contadores de desempenho para exceções, interoperabilidade, compiladores JIT, carregamento, memória, rede, segurança e muito mais.
ms.date: 03/30/2017
helpviewer_keywords:
- performance, .NET Framework applications
- performance counters
- performance monitoring, counters
ms.assetid: 06a4ae8c-eeb2-4d5a-817e-b1b95c0653e1
ms.openlocfilehash: 3702e9d2e0a369f5391c16088202caf5d7ced7ea
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803697"
---
# <a name="performance-counters-in-the-net-framework"></a>Contadores de desempenho no .NET Framework

Este tópico fornece uma lista de contadores de desempenho que você pode encontrar no [Monitor de desempenho do Windows](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749249%28v=ws.11%29).  

## <a name="exception-performance-counters"></a>Contadores de desempenho de exceção  
 A categoria Exceções do .NET CLR do Console de desempenho inclui contadores que fornecem informações sobre as exceções geradas por um aplicativo. A tabela a seguir descreve esses contadores de desempenho.  
  
|Contador de desempenho|Descrição|  
|-------------------------|-----------------|  
|**N. de exceções geradas**|Exibe o número total de exceções iniciadas desde que o aplicativo foi iniciado. Isso inclui exceções do .NET e exceções não gerenciadas que são convertidas em exceções do .NET. Por exemplo, um HRESULT retornado do código não gerenciado é convertido em uma exceção no código gerenciado.<br /><br /> Esse contador inclui tanto exceções manipuladas quanto não manipuladas. Exceções geradas novamente são contadas outra vez.|  
|**N. de exceções geradas/s**|Exibe o número de exceções geradas por segundo. Isso inclui exceções do .NET e exceções não gerenciadas que são convertidas em exceções do .NET. Por exemplo, um HRESULT retornado do código não gerenciado é convertido em uma exceção no código gerenciado.<br /><br /> Esse contador inclui tanto exceções manipuladas quanto não manipuladas. Ele não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem. Esse contador é um indicador de possíveis problemas de desempenho se um grande (>100) número de exceções são geradas.|  
|**N. de filtros/s**|Exibe o número de filtros de exceção do .NET executados por segundo. Um filtro de exceção avalia independentemente de uma exceção ser manipulada ou não.<br /><br /> Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**N. de Finallys/s**|Exibe o número de blocos finally executados por segundo. É certo que um bloco Finally será executado, independentemente de como o bloco try tiver sido encerrado.  Somente os blocos finally executados para uma exceção são contados; blocos finally em caminhos de código normais não são contabilizados por esse contador.<br /><br /> Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**Extensão do início ao tratamento/s**|Exibe o número de registros ativação desviados, do quadro que gerou a exceção até o quadro que tratou a exceção, por segundo. Esse contador é reiniciado para zero quando um manipulador de exceção é inserido, de modo que as exceções aninhadas mostram a profundidade da pilha de manipulador a manipulador.<br /><br /> Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  

## <a name="interop-performance-counters"></a>Contadores de desempenho de interoperabilidade  
 A categoria de interoperabilidade do .NET CLR do Console de desempenho inclui contadores que fornecem informações sobre a interação de um aplicativo com componentes COM, serviços COM+ e bibliotecas de tipos externo. A tabela a seguir descreve esses contadores de desempenho.  
  
|Contador de desempenho|Descrição|  
|-------------------------|-----------------|  
|**N. de CCWs**|Exibe o número atual de CCWs (COM Callable Wrappers). Um CCW é um proxy para um objeto gerenciado que está sendo referenciado de um cliente COM não gerenciado. Este contador indica o número de objetos gerenciados referenciados pelo código COM não gerenciado.|  
|**N. de marshaling**|Exibe o número total de vezes que argumentos e valores retornados sofreram marshaling de código gerenciado para não gerenciado e vice-versa, desde que o aplicativo foi iniciado. Não há incremento nesse contador se os stubs são embutidos. (Stubs são responsáveis por argumentos de marshaling e valores retornados). Se a sobrecarga de marshaling é pequena, os stubs geralmente são embutidos.|  
|**N. de Stubs**|Exibe o número atual de stubs criado pelo Common Language Runtime. Os stubs são responsáveis pelo marshaling de argumentos e valores retornados de código gerenciado para não gerenciado e vice-versa; durante uma chamada de interoperabilidade COM ou uma chamada de invocação de plataforma.|  
|**N. de exportações de TLB/s**|Reservado para uso futuro.|  
|**N. de importações de TLB/s**|Reservado para uso futuro.|  

## <a name="jit-performance-counters"></a>contadores de desempenho JIT  
 A categoria JIT do .NET CLR do Console de desempenho inclui contadores que fornecem informações sobre o código com compilação JIT. A tabela a seguir descreve esses contadores de desempenho.  
  
|Contador de desempenho|Descrição|  
|-------------------------|-----------------|  
|**N. de Bytes de IL com compilação JIT**|Exibe o número total de bytes do MSIL (Microsoft Intermediate Language) compilados pelo compilador JIT just-in-time (JIT) desde que o aplicativo foi iniciado. Este contador é equivalente ao contador **N. total de bytes de IL com compilação JIT**.|  
|**N. de métodos com compilação JIT**|Exibe o número total de métodos com compilação JIT desde que o aplicativo foi iniciado. Esse contador não inclui métodos com pré-compilação JIT.|  
|**% de tempo gasto em JIT**|Exibe o percentual de tempo decorrido gasto na compilação JIT desde a última fase da compilação JIT. Esse contador é atualizado no final de cada fase da compilação JIT. Uma fase da compilação JIT ocorre quando um método e suas dependências são compilados.|  
|**N. de Bytes de IL com compilação JIT/s**|Exibe o número de bytes MSIL com compilação JIT por segundo. Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**Falhas de JIT padrão**|Exibe o número máximo de métodos que o compilador JIT falhou ao compilar desde que o aplicativo foi iniciado. Essa falha pode ocorrer se o MSIL não pode ser verificado ou se há um erro interno no compilador JIT.|  
|**N. de bytes de IL com compilação JIT total**|Exibe o total de bytes MSIL com compilação JIT desde que o aplicativo foi iniciado. Este contador é equivalente ao contador **N. de bytes de IL com compilação JIT**.|  

## <a name="loading-performance-counters"></a>Carregando contadores de desempenho  
 A categoria Carregamento do .NET CLR do Console de desempenho inclui contadores que fornecem informações sobre os assemblies, as classes e os domínios de aplicativos que são carregados. A tabela a seguir descreve esses contadores de desempenho.  
  
|Contador de desempenho|Descrição|  
|-------------------------|-----------------|  
|**% do tempo de carregamento**|Reservado para uso futuro.|  
|**Duração da pesquisa de assembly**|Reservado para uso futuro.|  
|**Bytes no heap do carregador**|Exibe o tamanho atual, em bytes, da memória confirmada pelo carregador de classes entre todos os domínios de aplicativo. A memória confirmada é o espaço físico reservado no arquivo de paginação de disco.|  
|**Appdomains atuais**|Exibe o número atual de domínios de aplicativo carregados neste aplicativo.|  
|**Assemblies Atuais**|Exibe o número atual de assemblies carregados entre todos os domínios de aplicativo no aplicativo em execução no momento. Se o assembly for carregado como um domínio neutro de vários domínios de aplicativo, esse contador será incrementado apenas uma vez.|  
|**Classes carregadas atualmente**|Exibe o número actual de classes carregadas em todos os assemblies.|  
|**Taxa de appdomains**|Exibe o número de domínios do aplicativo carregados por segundo. Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**Taxa de AppDomains descarregados**|Exibe o número de domínios do aplicativo descarregados por segundo. Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**Taxa de assemblies**|Exibe o número de assemblies carregados por segundo entre todos os domínios de aplicativo. Se o assembly for carregado como um domínio neutro de vários domínios de aplicativo, esse contador será incrementado apenas uma vez.<br /><br /> Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**Taxa de Classes carregadas**|Exibe o número de classes carregadas por segundo em todos os assemblies. Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**Taxa de falhas de carregamento**|Exibe o número de classes que falharam ao carregar por segundo. Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.<br /><br /> Falhas de carregamento podem ocorrer por vários motivos, como segurança inadequada ou formato inválido. Para obter detalhes, consulte a que ajuda dos serviços de criação de perfil.|  
|**N. total de falhas de carregamento**|Exibe o número máximo de classes que falharam ao carregar desde que o aplicativo foi iniciado.<br /><br /> Falhas de carregamento podem ocorrer por vários motivos, como segurança inadequada ou formato inválido. Para obter detalhes, consulte a que ajuda dos serviços de criação de perfil.|  
|**Total de AppDomains**|Exibe o número máximo de domínios do aplicativo carregados desde que o aplicativo foi iniciado.|  
|**Total de AppDomains descarregados**|Exibe o número total de domínios do aplicativo descarregados desde que o aplicativo foi iniciado. Se um domínio de aplicativo for carregado e descarregado várias vezes, esse contador será incrementado sempre que o domínio de aplicativo for descarregado.|  
|**Total de Assemblies**|Exibe o número total de assemblies carregados desde que o aplicativo foi iniciado. Se o assembly for carregado como um domínio neutro de vários domínios de aplicativo, esse contador será incrementado apenas uma vez.|  
|**Total de Classes carregadas**|Exibe o número cumulativo de classes carregadas em todos os assemblies desde que o aplicativo foi iniciado.|  

## <a name="lock-and-thread-performance-counters"></a>Contadores de desempenho de bloqueio e thread  
 A categoria LocksAndThreads do .NET CLR do Console de desempenho inclui contadores que fornecem informações sobre bloqueios e threads gerenciados e usos de aplicativo. A tabela a seguir descreve esses contadores de desempenho.  
  
|Contador de desempenho|Descrição|  
|-------------------------|-----------------|  
|**N. atual de threads lógicos**|Exibe o número atual de objetos de thread gerenciados no aplicativo. Esse contador mantém a contagem tanto nos threads em execução quanto nos parados. Esse contador não é uma média ao longo do tempo; ele exibe apenas o último valor observado.|  
|**N. atual de threads físicos**|Exibe o número de threads do sistema operacional nativo criados e pertencentes ao Common Language Runtime para agir como threads subjacentes para os objetos de thread gerenciado. O valor desse contador não inclui os threads usados pelo runtime em suas operações internas; ele é um subconjunto dos threads no processo do sistema operacional.|  
|**N. atual de threads reconhecidos**|Exibe o número de threads que são reconhecidos atualmente pelo runtime. Esses threads são associados um objeto de thread gerenciado correspondente. O runtime não cria esses threads, mas eles foram executados dentro do runtime pelo menos uma vez.<br /><br /> Apenas os threads exclusivos são acompanhados; threads com a mesma ID de thread que adentram novamente o runtime ou são recriados depois da saída do thread não são contados duas vezes.|  
|**N. total de threads reconhecidos**|Exibe o número total de threads reconhecidos pelo runtime desde que o aplicativo foi iniciado. Esses threads são associados um objeto de thread gerenciado correspondente. O runtime não cria esses threads, mas eles foram executados dentro do runtime pelo menos uma vez.<br /><br /> Apenas os threads exclusivos são acompanhados; threads com a mesma ID de thread que adentram novamente o runtime ou são recriados depois da saída do thread não são contados duas vezes.|  
|**Taxa de contenção/s**|Exibe a taxa na qual threads no runtime que tentam, sem êxito, adquirir um bloqueio gerenciado.|  
|**Comprimento atual da fila**|Exibe o número total de threads que estão aguardando para adquirir um bloqueio gerenciado no aplicativo. Esse contador não é uma média ao longo do tempo; ele exibe o último valor observado.|  
|**Comprimento da fila/s**|Exibe o número de threads por segundo que estão aguardando para adquirir um bloqueio no aplicativo. Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**Tamanho máximo da fila**|Exibe o número total de threads que aguardaram para adquirir um bloqueio gerenciado desde que o aplicativo foi iniciado.|  
|**taxa de threads reconhecidos/s**|Exibe o número de threads por segundo que foram reconhecidos atualmente pelo runtime. Esses threads são associados um objeto de thread gerenciado correspondente. O runtime não cria esses threads, mas eles foram executados dentro do runtime pelo menos uma vez.<br /><br /> Apenas os threads exclusivos são acompanhados; threads com a mesma ID de thread que adentram novamente o runtime ou são recriados depois da saída do thread não são contados duas vezes.<br /><br /> Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**N. total de contenções**|Exibe o número total de vezes que os threads em runtime tentaram adquirir um bloqueio gerenciado sem êxito.|  

## <a name="memory-performance-counters"></a>Contadores de desempenho da memória  
 A categoria Memória do .NET CLR do Console de desempenho inclui contadores que fornecem informações sobre o coletor de lixo. A tabela a seguir descreve esses contadores de desempenho.  
  
|Contador de desempenho|Descrição|  
|-------------------------|-----------------|  
|**N º de Bytes em todos os Heaps**|Exibe a soma dos contadores **Tamanho do heap de geração 1**, **Tamanho do heap de geração 2** e **Tamanho de heap de Objeto Grande**. Este contador indica a memória atual alocada em bytes nos heaps de coleta de lixo.|  
|**# Identificadores de GC**|Exibe o número atual de identificadores de coleta de lixo em uso. Os identificadores de coleta de lixo são identificadores de recursos externos para o Common Language Runtime e o ambiente gerenciado.|  
|**Nº de Coleções Geração 0**|Exibe o número de vezes que os objetos da geração 0 (ou seja, a mais novos, objetos alocados mais recentemente) sofreram coleta de lixo desde que o aplicativo foi iniciado.<br /><br /> A coleta de lixo de geração 0 ocorre quando a memória disponível na geração 0 não é suficiente para atender a uma solicitação de alocação. Esse contador é incrementado no final de uma coleta de lixo da geração 0. Coletas de lixo de gerações mais elevadas incluem todas as coletas de gerações mais baixas. Esse contador é incrementado explicitamente quando ocorre uma coleta de lixo de geração mais elevada (geração 1 ou 2).<br /><br /> Esse contador exibe o último valor observado. O valor do contador **_Global\_** não é preciso e deve ser ignorado.|  
|**Nº de Coleções Geração 1**|Exibe o número de vezes que os objetos da geração 1 sofreram coleta de lixo desde que o aplicativo foi iniciado.<br /><br /> O contador é incrementado no final de uma coleta de lixo da geração 1. Coletas de lixo de gerações mais elevadas incluem todas as coletas de gerações mais baixas. Esse contador é incrementado explicitamente quando ocorre uma coleta de lixo de geração mais elevada (geração 2).<br /><br /> Esse contador exibe o último valor observado. O valor do contador **_Global\_** não é preciso e deve ser ignorado.|  
|**Nº de Coleções Geração 2**|Exibe o número de vezes que os objetos da geração 2 sofreram coleta de lixo desde que o aplicativo foi iniciado. O contador é incrementado no final de uma coleta de lixo da geração 2 (também conhecida como uma coleta de lixo completa).<br /><br /> Esse contador exibe o último valor observado. O valor do contador **_Global\_** não é preciso e deve ser ignorado.|  
|**N. de GCs induzidas**|Exibe o número máximo de vezes que a coleta de lixo foi executada devido a uma chamada explícita para <xref:System.GC.Collect%2A?displayProperty=nameWithType>. É recomendável para permitir que o coletor de lixo ajuste a frequência das coletas dele.|  
|**N. de objetos fixados**|Exibe o número de objetos fixados encontrados na última coleta de lixo. Um objeto fixado é um objeto que o coletor de lixo não consegue mover na memória. Esse contador controla os objetos fixados apenas nos heaps que sofrem coleta de lixo. Por exemplo, uma coleta de lixo de geração 0 causa a enumeração dos objetos fixados apenas no heap de geração 0.|  
|**N. de blocos de sincronização em uso**|Exibe o número atual de blocos de sincronização em uso. Blocos de sincronização são estruturas de dados por objeto alocadas para armazenar informações de sincronização. Eles mantêm referências fracas a objetos gerenciados e devem ser verificados pelo coletor de lixo. Blocos de sincronização não se limitam a armazenar informações de sincronização, eles também podem armazenar metadados de interoperabilidade COM. Este contador indica problemas de desempenho com uso intensivo dos primitivos de sincronização.|  
|**Nº Total de Bytes confirmados**|Exibe a quantidade de memória virtual, em bytes, confirmada atualmente pelo coletor de lixo. A memória confirmada é a memória física para a qual o espaço foi reservado no arquivo de paginação de disco.|  
|**N º Total de Bytes reservados**|Exibe a quantidade de memória virtual, em bytes, reservada atualmente pelo coletor de lixo. Memória reservada é o espaço de memória virtual reservado para o aplicativo quando nenhuma página de memória principal ou de disco foi usada.|  
|**% de Tempo na GC**|Exibe o percentual de tempo decorrido que foi gasto na execução de uma coleta de lixo desde o último ciclo de coleta de lixo. Esse contador geralmente indica que o trabalho feito pelo coletor de lixo para coletar e compactar memória em nome do aplicativo. Esse contador é atualizado somente no final de toda coleta de lixo. Esse contador não é uma média; o valor dele exibe o último valor observado.|  
|**Bytes alocados/segundo**|Exibe o número de bytes por segundo alocados no heap de coleta de lixo. Esse contador é atualizado no final de toda coleta de lixo e não em cada alocação. Esse contador não consiste numa média temporal; ele apresenta a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**Sobreviventes da finalização**|Exibe o número de objetos de coletor de lixo que sobrevivem a uma coleta porque estão aguardando finalização. Se esses objetos mantêm referências a outros objetos, esses últimos também sobreviverão, mas não serão contados por esse contador. O contador **Finalização promovida – memória da Geração 0** representa toda a memória que sobreviveu devido à finalização.<br /><br /> Esse contador não é cumulativo. ele é atualizado no final de cada coleta de lixo com a contagem de sobreviventes durante essa determinada coleta. Este contador indica a sobrecarga extra à qual o aplicativo pode estar sujeito a devido a finalização.|  
|**Tamanho do heap de geração 0**|Exibe o máximo de bytes que podem ser alocados na geração 0; ele não indica o número atual de bytes alocados na geração 0.<br /><br /> Uma coleta de lixo de geração 0 ocorre quando as alocações desde a última coleta excedem esse tamanho. O tamanho de geração 0 é ajustado para o coletor de lixo e pode ser alterado durante a execução do aplicativo. No final de uma coleção de geração 0 o tamanho do heap de geração 0 é 0 bytes. Esse contador exibe o tamanho, em bytes, de alocações, que invoca a próxima coleta de lixo de geração 0.<br /><br /> Esse contador é atualizado no final de uma coleta de lixo e não em cada alocação.|  
|**Bytes promovidos da geração 0/s**|Exibe os bytes por segundo que são promovidos da geração 0 para a geração 1. A memória é promovida quando sobrevive a uma coleta de lixo. Esse contador é um indicador dos objetos vida relativamente longa criados por segundo.<br /><br /> Esse contador mostra a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**Tamanho do heap de geração 1**|Exibe o número atual de bytes na geração 1; esse contador não exibe o tamanho máximo da geração 1. Os objetos não são diretamente alocados nessa geração; eles são promovidos de coletas de lixo de geração 0 anteriores. Esse contador é atualizado no final de uma coleta de lixo e não em cada alocação.|  
|**Bytes promovidos da geração 1/s**|Exibe os bytes por segundo que são promovidos da geração 1 para a geração 2. Objetos que são promovidos apenas porque estão aguardando a finalização não são incluídos nesse contador.<br /><br /> A memória é promovida quando sobrevive a uma coleta de lixo. Nenhum item é promovida da geração 2 porque ela é a geração mais antiga. Esse contador é um indicador dos objetos de vida muito longa criados por segundo.<br /><br /> Esse contador mostra a diferença entre os valores observados nos dois últimos exemplos divididos pela duração do intervalo de amostragem.|  
|**Tamanho do heap de geração 2**|Exibe o número atual de bytes na geração 2. Os objetos não são diretamente alocados nessa geração; eles são promovidos da geração 1 durante coletas de lixo de geração 1 anteriores. Esse contador é atualizado no final de uma coleta de lixo e não em cada alocação.|  
|**Tamanho de Heap de objeto grande**|Exibe o tamanho atual, em bytes, do heap de objeto grande. Objetos maiores que aproximadamente 85.000 bytes são tratados como objetos grandes pelo coletor de lixo e são diretamente alocados em um heap especial. Eles não são promovidos por meio das gerações. Esse contador é atualizado no final de uma coleta de lixo e não em cada alocação.|  
|**ID do Processo**|Exibe a ID do processo da instância do processo CLR que está sendo monitorada.|  
|**Finalização Promovida – Memória da Geração 0**|Exibe os bytes de memória que são promovidos da geração 0 para a geração 1 apenas porque estão aguardando a finalização. Esse contador não é cumulativo. ele exibe o valor observado no fim da última coleta de lixo.|  
|**Memória Promovida da Geração 0**|Exibe os bytes de memória que sobrevivem à coleta de lixo e são promovidos da geração 0 para a geração 1. Objetos que são promovidos apenas porque estão aguardando a finalização não são incluídos nesse contador. Esse contador não é cumulativo. ele exibe o valor observado no fim da última coleta de lixo.|  
|**Memória Promovida da Geração 1**|Exibe os bytes de memória que sobrevivem à coleta de lixo e são promovidos da geração 1 para a geração 2. Objetos que são promovidos apenas porque estão aguardando a finalização não são incluídos nesse contador. Esse contador não é cumulativo. ele exibe o valor observado no fim da última coleta de lixo. Esse contador é redefinido como 0 se a última coleta de lixo foi apenas de geração 0.|  

## <a name="networking-performance-counters"></a>Contadores de desempenho de rede  

A categoria Rede do .NET CLR do Console de desempenho inclui contadores que fornecem informações sobre dados que um aplicativo envia e recebe pela rede. A tabela a seguir descreve esses contadores de desempenho.  
  
|Contador de desempenho|Descrição|  
|-------------------------|-----------------|  
|**Bytes Recebidos**|Número total cumulativo de bytes recebidos por todos os objetos <xref:System.Net.Sockets.Socket> dentro do <xref:System.AppDomain> desde o início do processo. Esse número inclui dados e informações de protocolo que não são definidos por TCP/IP.|  
|**Bytes Enviados**|O número cumulativo de bytes enviados por todos os objetos <xref:System.Net.Sockets.Socket> dentro do <xref:System.AppDomain> desde o início do processo. Esse número inclui dados e informações de protocolo que não são definidos por TCP/IP.|  
|**Conexões Estabelecidas**|O número total cumulativo de objetos <xref:System.Net.Sockets.Socket> para soquetes de fluxo que foram conectados dentro do <xref:System.AppDomain> desde o início do processo.|  
|**Datagramas recebidos**|Número total cumulativo de pacotes de datagrama recebidos por todos os objetos <xref:System.Net.Sockets.Socket> dentro do <xref:System.AppDomain> desde o início do processo.|  
|**Datagramas enviados**|O número total cumulativo de pacotes de datagrama enviados por todos os objetos <xref:System.Net.Sockets.Socket> dentro do <xref:System.AppDomain> desde o início do processo.|  
|**Tempo de vida médio de HttpWebRequest**|O tempo médio até a conclusão de todos os objetos <xref:System.Net.HttpWebRequest> que foram finalizados no último intervalo dentro do <xref:System.AppDomain> desde o início do processo.|  
|**Tempo de espera médio de HttpWebRequest**|O tempo de espera médio de todos os objetos <xref:System.Net.HttpWebRequest> que deixaram a fila no último intervalo dentro do <xref:System.AppDomain> desde o início do processo.|  
|**HttpWebRequests criadas/s**|O número de objetos <xref:System.Net.HttpWebRequest> criados por segundo dentro do <xref:System.AppDomain>.|  
|**HttpWebRequests colocadas na fila/s**|O número de objetos <xref:System.Net.HttpWebRequest> que foram adicionados à fila por segundo dentro do <xref:System.AppDomain>.|  
|**HttpWebRequests anuladas/s**|O número de objetos <xref:System.Net.HttpWebRequest> em que o aplicativo chamou o método <xref:System.Net.HttpWebRequest.Abort%2A> por segundo dentro do <xref:System.AppDomain>.|  
|**HttpWebRequests com falha/s**|O número de objetos <xref:System.Net.HttpWebRequest> que receberam um código de status de falha do servidor por segundo dentro do <xref:System.AppDomain>.|  
  
 Existem várias classes de contadores de desempenho de rede com suporte:  
  
- Contadores de eventos que medem o número de vezes em que um determinado evento ocorreu.  
  
- Contadores de dados que medem a quantidade de dados enviados ou recebidos.  
  
- Contadores de duração que medem quanto tempo os diferentes processos levam. Os tempos são medidos nos objetos a cada intervalo (normalmente em segundos) após eles saírem de estados diferentes.  
  
- Contadores por intervalo que medem o número de objetos que estão fazendo uma determinada transição por intervalo (normalmente por segundo).  
  
Os contadores de desempenho de rede para eventos incluem o seguinte:  
  
- **Conexões Estabelecidas**  
  
- **Datagramas recebidos**  
  
- **Datagramas enviados**  
  
 Esses contadores de desempenho fornecem contagens desde o início do processo. As contagens de conexões <xref:System.Net.Sockets.Socket> estabelecidas incluem chamadas de método <xref:System.Net.Sockets.Socket> explícitas por um aplicativo uma conexão de soquete de fluxo que foi estabelecida, bem como chamadas internas feitas por outras classes (<xref:System.Net.HttpWebRequest>, <xref:System.Net.FtpWebRequest>, <xref:System.Net.WebClient> e <xref:System.Net.Sockets.TcpClient>, por exemplo) para a classe <xref:System.Net.Sockets.Socket>  
  
 As contagens de **Datagramas recebidos** e **Datagramas enviados** incluem pacotes de datagramas enviados ou recebidos usando chamadas de método <xref:System.Net.Sockets.Socket> explícitas por um aplicativo interno, bem como chamadas feitas por outras classes (por exemplo, <xref:System.Net.Sockets.UdpClient>) para a classe <xref:System.Net.Sockets.Socket>. . As contagens de **Datagramas recebidos** e **Datagramas enviados** também pode ser usada para fornecer uma medida bastante imprecisa de quantos bytes foram enviados ou recebidos usando datagramas, pressupondo um tamanho médio para um determinado datagrama.  
  
 Os contadores de desempenho de rede para dados incluem o seguinte:  
  
- **Bytes Recebidos**  
  
- **Bytes Enviados**  
  
 Os contadores acima fornecem contagens de bytes desde o início do processo.  
  
 Há dois contadores de duração que medem quanto tempo demorou parte ou todo o ciclo de vida de objetos <xref:System.Net.HttpWebRequest>:  
  
- **Tempo de vida médio de HttpWebRequest**  
  
- **Tempo de espera médio de HttpWebRequest**  
  
 Para o contador **Tempo de vida médio de HttpWebRequest**, o tempo de vida da maioria dos objetos <xref:System.Net.HttpWebRequest> sempre começa com a hora em que o objeto é criado até o momento em que o fluxo de resposta é fechado pelo aplicativo. Há dois casos incomuns:  
  
- Se o aplicativo nunca chama os métodos <xref:System.Net.HttpWebRequest.GetResponse%2A> ou <xref:System.Net.HttpWebRequest.BeginGetResponse%2A>, o tempo de vida do objeto <xref:System.Net.HttpWebRequest> é ignorado.  
  
- Se o objeto <xref:System.Net.HttpWebRequest> gera uma <xref:System.Net.WebException> ao chamar os métodos <xref:System.Net.HttpWebRequest.GetResponse%2A> ou <xref:System.Net.HttpWebRequest.EndGetResponse%2A>, o tempo de vida termina quando a exceção é gerada. Tecnicamente, o fluxo de resposta subjacente também está fechado nesse momento (o fluxo de resposta retornado ao usuário é realmente um fluxo de memória contendo uma cópia do fluxo de resposta).  
  
 Há quatro contadores que acompanham certas emissões de objetos <xref:System.Net.HttpWebRequest> por intervalo. Esses contadores de desempenho podem ajudar os desenvolvedores de aplicativos, os administradores e a equipe de suporte a compreender melhor o que os objetos <xref:System.Net.HttpWebRequest> estão fazendo. Os contadores incluem o seguinte:  
  
- **HttpWebRequests criadas/s**  
  
- **HttpWebRequests colocadas na fila/s**  
  
- **HttpWebRequests anuladas/s**  
  
- **HttpWebRequests com falha/s**  
  
 Para o contador **HttpWebRequests anuladas/s**, chamadas internas para <xref:System.Net.HttpWebRequest.Abort%2A> também são contadas. Essas chamadas internas são geralmente causadas por tempos limite que um aplicativo pode desejar medir.  
  
 O contador **HttpWebRequests com falha/s** contém o número de objetos <xref:System.Net.HttpWebRequest> que receberam um código de status de falha do servidor por segundo. Isso significa que o código de status recebido do servidor no final da solicitação Http não estava no intervalo entre 200 e 299. Códigos de status que são manipulados e resultam em uma nova solicitação (muitos dos códigos de status 401 – Não autorizado, por exemplo) falham ou não falham dependendo do resultado da repetição. Se o aplicativo ver um erro com base na nova tentativa, esse contador será incrementado.  
  
 Contadores de desempenho de rede podem ser acessados e gerenciados usando a <xref:System.Diagnostics.PerformanceCounter> e classes relacionadas no namespace <xref:System.Diagnostics>. Contadores de desempenho de rede também podem ser exibidos com o console do Monitor de Desempenho do Windows.  
  
 Contadores de desempenho de rede precisam ser habilitados no arquivo de configuração a ser usado. Todos os contadores de desempenho de rede são habilitados ou desabilitados com uma única configuração no arquivo de configuração. Contadores de desempenho de rede individuais não podem ser habilitados nem desabilitados. Para obter mais informações, consulte [ \<performanceCounter> elemento (configurações de rede)](../configure-apps/file-schema/network/performancecounter-element-network-settings.md).  
  
 Se os contadores de rede estiverem habilitados, isso criará e atualizará contadores de desempenho globais e por AppDomain. Se desabilitados, o aplicativo não fornecerá nenhum dado de contador de desempenho de rede.  
  
 Os contadores de desempenho são agrupados em categorias. Um aplicativo pode listar todas as categorias com o código de exemplo a seguir:  
  
```csharp
PerformanceCounterCategory[] Array = PerformanceCounterCategory.GetCategories();  
for (int i = 0; i < Array.Length; i++)  
{  
    Console.Out.WriteLine("{0}. Name={1} Help={2}", i, Array[i].CategoryName, Array[i].CategoryHelp);  
}  
```  
  
 Os contadores de desempenho de rede são listados em duas categorias:  
  
- "Rede do .NET CLR" – os contadores de desempenho originais introduzidos no .NET Framework Versão 2 e com suporte no .NET Framework Versão 2 e versões posteriores.  
  
- "Rede do .NET CLR 4.0.0.0" – todos os contadores de soquete acima mais os novos contadores de desempenho com suporte no .NET Framework Versão 4 e posteriores. Esses novos contadores fornecem informações de desempenho sobre objetos <xref:System.Net.HttpWebRequest>.  
  
 Para obter mais informações sobre como acessar e gerenciar os contadores de desempenho em um aplicativo, consulte [Contadores de desempenho](performance-counters.md).  

## <a name="security-performance-counters"></a>Contadores de desempenho de segurança  
 A categoria de Segurança do .NET CLR do Console de desempenho inclui contadores que fornecem informações sobre as verificações de segurança que o Common Language Runtime executa para um aplicativo. A tabela a seguir descreve esses contadores de desempenho.  
  
|Contador de desempenho|Descrição|  
|-------------------------|-----------------|  
|**N. de verificações de tempo de vinculação**|Exibe o número total de verificações de segurança de acesso do código de tempo de vinculação desde que o aplicativo foi iniciado. Verificações de segurança de acesso de código de tempo de vinculação são realizadas quando um chamador requer uma permissão específica no tempo de compilação JIT (Just-In-Time). Uma verificação de tempo de vinculação é executada uma vez por chamador. Essa contagem não é um indicativo de problemas de desempenho sérios, mas meramente de atividade do sistema de segurança.|  
|**% de tempo de verificações de RT**|Exibe o percentual do tempo decorrido que foi utilizado executando verificações de segurança de acesso do código de runtime desde a última amostra. Esse contador é atualizado no final de uma verificação de segurança do .NET Framework. Ele não é uma média; ele representa o último valor observado.|  
|**% de tempo em autenticação de assinatura**|Reservado para uso futuro.|  
|**Extensão do exame da pilha**|Exibe a profundidade da pilha durante essa última verificação de segurança de acesso do código de runtime. Verificações de segurança de acesso do código de runtime são realizadas movimentando a pilha. Esse contador não é uma média, ele exibe apenas o último valor observado.|  
|**Verificações de runtime total**|Exibe o número total de verificações de segurança de acesso do código de runtime realizadas desde que o aplicativo foi iniciado. Verificações de segurança de acesso de código de runtime são realizadas quando um chamador requer uma permissão específica. A verificação de runtime é feita em cada chamada pelo chamador e examina a pilha do thread atual do chamador. Quando usado com o contador **Extensão do exame da pilha**, esse contador indica a penalidade de desempenho que ocorre para verificações de segurança.|  
  
## <a name="see-also"></a>Consulte também

- [Contadores de desempenho](performance-counters.md)
- [Criação de perfil de tempo de execução](runtime-profiling.md)
