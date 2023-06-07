---
title: Rastreamentos de diagnóstico
description: Saiba mais sobre os rastreamentos de diagnóstico no .NET. Os rastreamentos são a publicação de mensagens específicas que são gerados durante a execução do aplicativo.
ms.date: 03/30/2017
ms.assetid: 28e77a63-d20d-4b6a-9caf-ddad86550427
ms.openlocfilehash: 5de8fdf7b95cf01b119118dac75d373c32949dcd
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141805"
---
# <a name="diagnostic-traces"></a>Rastreamentos de diagnóstico
Os rastreamentos são a publicação de mensagens específicas que são gerados durante a execução do aplicativo. Ao usar o rastreamento, você deve ter um mecanismo para coletar e registrar as mensagens são enviadas. Mensagens de rastreamento são recebidas por ouvintes. A finalidade de um ouvinte é coletar, armazenar e rotear mensagens de rastreamento. Os ouvintes direcionam a saída de rastreamento para um destino apropriado, como um log, uma janela ou um arquivo de texto.  
  
 Um ouvinte, o <xref:System.Diagnostics.DefaultTraceListener>, é criado e inicializado automaticamente quando o rastreamento está habilitado. Se desejar que a saída de rastreamento sejam direcionados para quaisquer outras fontes, você deve criar e inicializar ouvintes de rastreamento adicionais. Os ouvintes que você criar devem refletir suas necessidades individuais. Por exemplo, convém um registro de texto de toda a saída de rastreamento. Nesse caso, você criaria um ouvinte que escrevi todas as saídas para um novo arquivo de texto quando habilitado. Por outro lado, você talvez só queira exibir saída durante a execução do aplicativo. Nesse caso, você pode criar um ouvinte que direcionadas toda a saída para uma janela do console. O <xref:System.Diagnostics.EventLogTraceListener> pode direcionar a saída de rastreamento para um log de eventos e o <xref:System.Diagnostics.TextWriterTraceListener> pode gravar a saída de rastreamento em um fluxo.  
  
## <a name="enabling-tracing"></a>Habilitar o rastreamento  
 Para habilitar rastreamentos durante o processamento de transações, você deve editar o arquivo de configuração do aplicativo. A seguir, é mostrado um exemplo.  
  
```xml  
<configuration>  
<system.diagnostics>  
     <sources>  
          <source name="System.Transactions" switchValue="Warning">  
               <listeners>  
                    <add name="tx"
                     type="System.Diagnostics.XmlWriterTraceListener"
                     initializeData= "tx.log" />  
               </listeners>  
          </source>  
     </sources>  
</system.diagnostics>  
</configuration>  
```  
  
 <xref:System.Transactions>os rastreamentos são gravados na fonte chamada "System. Transactions". Você pode usar `add` para especificar o nome e o tipo do ouvinte de rastreamento que deseja usar. Em nosso exemplo de configuração, é chamado o ouvinte "tx" e adicionado o ouvinte de rastreamento padrão do .NET Framework (<xref:System.Diagnostics.XmlWriterTraceListener>) como o tipo que deseja usar. Use `initializeData` para definir o nome do arquivo de log para esse ouvinte. Além disso, você pode substituir um caminho totalmente qualificado para um nome de arquivo simples.  
  
 Cada tipo de mensagem de rastreamento é atribuído um nível para indicar o grau de importância. Se o nível de rastreamento do aplicativo-domínio for igual ou menor do que o nível de um tipo de evento, essa mensagem é gerada. O nível de rastreamento é controlado pelo `switchValue` configuração no arquivo de configuração. Os níveis que estão associados com as mensagens de rastreamento de diagnóstico são definidos na tabela a seguir.  
  
|Nível de rastreamento:|Description|  
|-----------------|-----------------|  
|Crítico|Ocorreram falhas graves, como o seguinte:<br /><br /> -Um erro que pode causar uma perda imediata na funcionalidade do usuário.<br />-Um evento que exige que um administrador tome medidas para evitar perda de funcionalidade.<br />-O código trava.<br />-Esse nível de rastreamento também pode fornecer contexto suficiente para interpretar outros rastreamentos críticos. Isso pode ajudar a identificar a seqüência de operações que leva a uma falha grave.|  
|Erro|Ocorreu um erro (por exemplo, inválido configuração ou da rede comportamento) que pode resultar em uma perda de funcionalidade do usuário.|  
|Aviso|Existe uma condição que pode resultar em um erro ou uma falha crítica (por exemplo, alocação de falha ou alcançando o limite). O processamento normal de erros de código do usuário (por exemplo, transação anulada, tempos limite de falha de autenticação) também pode gerar um aviso.|  
|Informações|São geradas Mensagens úteis para monitorar e diagnosticar o status do sistema, medir o desempenho ou criar perfis. Eles podem incluir eventos de tempo de vida de transação e a inscrição, como uma transação que está sendo criado ou confirmadas, a interseção de um limite significativa ou a alocação de recursos significativos. Um desenvolvedor pode utilizar essas informações para o gerenciamento de desempenho e planejamento de capacidade.|  
  
## <a name="trace-codes"></a>Códigos de rastreamento  
 A tabela a seguir lista os códigos de rastreamento gerados pelo <xref:System.Transactions> infra-estrutura. Estão incluídos na tabela o identificador de código de rastreamento, o <xref:System.Diagnostics.EventTypeFilter.EventType%2A> nível de enumeração para o rastreamento e os dados adicionais contidos no **TraceRecord** para o rastreamento. Além disso, o nível de rastreamento correspondente do rastreamento também é armazenado no **TraceRecord**.  
  
|TraceCode|EventType|Dados extras em TraceRecord|  
|---------------|---------------|-------------------------------|  
|TransactionCreated|Informações|TransactionTraceId|  
|TransactionPromoted|Informações|TransactionTraceId local, TransactionTraceId distribuída|  
|EnlistmentCreated|Informações|TransactionTraceId, EnlistmentTraceId, EnlistmentType (duráveis voláteis), EnlistmentOptions|  
|EnlistmentCallbackNegative|Aviso|TransactionTraceId, EnlistmentTraceId,<br /><br /> Retorno de chamada (forcerollback/anulado/incertas)|  
|TransactionRollbackCalled|Aviso|TransactionTraceId|  
|TransactionAborted|Aviso|TransactionTraceId|  
|TransactionInDoubt|Aviso|TransactionTraceId|  
|TransactionScopeCreated|Informações|TransactionScopeResult, que pode ser o seguinte:<br /><br /> -Nova transação.<br />-Transação aprovada.<br />Transação dependente de aprovado.<br />-Usando a transação atual.<br />-Nenhuma transação.<br /><br /> novo TransactionTraceId atual|  
|TransactionScopeDisposed|Informações|TransactionTraceId da transação atual "esperada" do escopo.|  
|TransactionScopeIncomplete|Aviso|TransactionTraceId da transação atual "esperada" do escopo.|  
|TransactionScopeNestedIncorrectly|Aviso|TransactionTraceId da transação atual "esperada" do escopo.|  
|TransactionScopeCurrentTransactionChanged|Aviso|Antigo TransactionTraceId atual, outro TransactionTraceId|  
|TransactionScopeTimeout|Aviso|TransactionTraceId da transação atual "esperada" do escopo.|  
|DependentCloneCreated|Informações|TransactionTraceId, tipo de transação dependente criado (RollbackIfNotComplete/BlockCommitUntilComplete)|  
|DependentCloneComplete|Informações|TransactionTraceId|  
|RecoveryComplete|Informações|Gerenciador de recursos de GUID (de base)|  
|Reenlist|Informações|Gerenciador de recursos de GUID (de base)|  
|TransactionSerialized|Informações|TransactionTraceId.|  
|TransactionException|Erro|Mensagem de exceção|  
|InvalidOperationException|Erro|Mensagem de exceção|  
|InternalError|Crítico|Mensagem de exceção|  
|TransferEvent||Quando uma transação é desserializada ou promovida de um <xref:System.Transactions> transação para um distribuído, o ActivityID atual de ExecutionContext e a ID de transação distribuída são gravados.<br /><br /> Quando o DTC chama de volta em código gerenciado, a ID de transação distribuída é definida como o ActivityID em ExecutionContext durante o retorno de chamada.|  
|ConfiguredDefaultTimeoutAdjusted|Aviso|Nenhum dado extra|  
|TransactionTimeout|Aviso|TransactionTraceId da transação que está sendo atingiu o tempo limite.|  
  
 O esquema XML para cada um dos itens de dados extras anterior tem o seguinte formato.  
  
### <a name="transactiontraceidentifier"></a>TransactionTraceIdentifier  
 `<TransactionTraceIdentifier>`  
  
 `<TransactionIdentifier >`  
  
 `string representation of transaction id`  
  
 `</TransactionIdentifier>`  
  
 `< CloneIdentifier >`  
  
 `the clone id number`  
  
 `</CloneIdentifier>`  
  
 `</TransactionTraceIdentifier>`  
  
### <a name="enlistmenttraceidentifier"></a>EnlistmentTraceIdentifier  
 `<EnlistmentTraceIdentifier>`  
  
 `<ResourceManagerId>`  
  
 `string form of guid`  
  
 `</ResourceManagerId>`  
  
 `<TransactionTraceIdentifier>`  
  
 `<TransactionIdentifier >`  
  
 `string representation of transaction id`  
  
 `</TransactionIdentifier>`  
  
 `<CloneIdentifier >`  
  
 `the clone id number`  
  
 `</CloneIdentifier>`  
  
 `<TransactionTraceIdentifier>`  
  
 `<EnlistmentIdentifier>`  
  
 `the enlistment id number`  
  
 `</EnlistmentIdentifier>`  
  
 `</EnlistmentTraceIdentifier>`  
  
### <a name="resource-manager-identifier"></a>Identificador do Gerenciador de recursos  
 `<ResourceManagerId>`  
  
 `string form of guid`  
  
 `</ResourceManagerId>`  
  
## <a name="security-issues-for-tracing"></a>Problemas de segurança para rastreamento  
 Quando você como administrador ative o rastreamento, informações confidenciais podem gravadas em um log de rastreamento que podem ser exibido publicamente por padrão. Para minimizar qualquer possível ameaça à segurança, considere armazenar o log de rastreamento em um local seguro controlado por permissões de acesso de compartilhamento e arquivo de sistema.
