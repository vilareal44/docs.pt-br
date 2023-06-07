---
title: Contadores de desempenho
description: Use contadores de desempenho ADO.NET para monitorar o status do aplicativo e seus recursos de conexão usando o monitor de desempenho do Windows ou programaticamente.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 0b121b71-78f8-4ae2-9aa1-0b2e15778e57
ms.openlocfilehash: 9dde2d7305a1176dadba3802fc5335c0c95bfbbb
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286670"
---
# <a name="performance-counters-in-adonet"></a>Contadores de desempenho no ADO.NET
O ADO.NET 2,0 introduziu suporte expandido para contadores de desempenho que incluem suporte para o <xref:System.Data.SqlClient> e o <xref:System.Data.OracleClient> . Os <xref:System.Data.SqlClient> contadores de desempenho disponíveis nas versões anteriores do ADO.net foram preteridos e substituídos pelos novos contadores de desempenho discutidos neste tópico. Você pode usar contadores de desempenho ADO.NET para monitorar o status do seu aplicativo e os recursos de conexão que ele usa. Os contadores de desempenho podem ser monitorados usando o monitor de desempenho do Windows ou podem ser acessados programaticamente usando a <xref:System.Diagnostics.PerformanceCounter> classe no <xref:System.Diagnostics> namespace.  
  
## <a name="available-performance-counters"></a>Contadores de desempenho disponíveis  
 Atualmente, há 14 diferentes contadores de desempenho disponíveis <xref:System.Data.SqlClient> para <xref:System.Data.OracleClient> o e conforme descrito na tabela a seguir. Observe que os nomes dos contadores individuais não são localizados em versões regionais do Microsoft .NET Framework.  
  
|Contador de desempenho|Description|  
|-------------------------|-----------------|  
|`HardConnectsPerSecond`|O número de conexões por segundo que estão sendo feitas a um servidor de banco de dados.|  
|`HardDisconnectsPerSecond`|O número de desconexões por segundo que estão sendo feitas em um servidor de banco de dados.|  
|`NumberOfActiveConnectionPoolGroups`|O número de grupos de pools de conexão exclusivos que estão ativos. Esse contador é controlado pelo número de cadeias de conexão exclusivas encontradas no AppDomain.|  
|`NumberOfActiveConnectionPools`|O número total de pools de conexão.|  
|`NumberOfActiveConnections`|O número de conexões ativas que estão em uso no momento. **Observação:**  Esse contador de desempenho não está habilitado por padrão. Para habilitar esse contador de desempenho, consulte [ativando contadores desativados por padrão](#ActivatingOffByDefault).|  
|`NumberOfFreeConnections`|O número de conexões disponíveis para uso nos pools de conexão. **Observação:**  Esse contador de desempenho não está habilitado por padrão. Para habilitar esse contador de desempenho, consulte [ativando contadores desativados por padrão](#ActivatingOffByDefault).|  
|`NumberOfInactiveConnectionPoolGroups`|O número de grupos de pools de conexão exclusivos que estão marcados para remoção. Esse contador é controlado pelo número de cadeias de conexão exclusivas encontradas no AppDomain.|  
|`NumberOfInactiveConnectionPools`|O número de pools de conexão inativos que não tiveram nenhuma atividade recente e que estão aguardando para serem descartados.|  
|`NumberOfNonPooledConnections`|O número de conexões ativas que não estão em pool.|  
|`NumberOfPooledConnections`|O número de conexões ativas que estão sendo gerenciadas pela infraestrutura de pooling de conexão.|  
|`NumberOfReclaimedConnections`|O número de conexões que foram recuperadas por meio da coleta de lixo onde `Close` ou `Dispose` não foi chamado pelo aplicativo. Não fechar ou descartar explicitamente as conexões afeta o desempenho.|  
|`NumberOfStasisConnections`|O número de conexões que estão aguardando a conclusão de uma ação e que, portanto, não estão disponíveis para uso pelo seu aplicativo.|  
|`SoftConnectsPerSecond`|O número de conexões ativas sendo extraídas do pool de conexões. **Observação:**  Esse contador de desempenho não está habilitado por padrão. Para habilitar esse contador de desempenho, consulte [ativando contadores desativados por padrão](#ActivatingOffByDefault).|  
|`SoftDisconnectsPerSecond`|O número de conexões ativas que estão sendo retornadas para o pool de conexões. **Observação:**  Esse contador de desempenho não está habilitado por padrão. Para habilitar esse contador de desempenho, consulte [ativando contadores desativados por padrão](#ActivatingOffByDefault).|  
  
### <a name="connection-pool-groups-and-connection-pools"></a>Grupos de pools de conexão e pools de conexão  
 Ao usar a autenticação do Windows (segurança integrada), você deve monitorar `NumberOfActiveConnectionPoolGroups` os `NumberOfActiveConnectionPools` contadores de desempenho e. O motivo é que os grupos do pool de conexões são mapeados para cadeias de conexão exclusivas. Quando a segurança integrada é usada, os pools de conexão são mapeados para cadeias de conexão e também criam pools separados para identidades individuais do Windows. Por exemplo, se Fred e Julie, cada um dentro do mesmo AppDomain, ambos usam a cadeia de conexão `"Data Source=MySqlServer;Integrated Security=true"` , um grupo de pools de conexão é criado para a cadeia de conexão e dois pools adicionais são criados, um para Fred e outro para Julie. Se João e Martha usarem uma cadeia de conexão com um logon de SQL Server idêntico, `"Data Source=MySqlServer;User Id=lowPrivUser;Password=Strong?Password"` apenas um único pool será criado para a identidade **lowPrivUser** .  
  
<a name="ActivatingOffByDefault"></a>
### <a name="activating-off-by-default-counters"></a>Ativando contadores desativados por padrão  
 Os contadores de desempenho `NumberOfFreeConnections` , `NumberOfActiveConnections` , `SoftDisconnectsPerSecond` e `SoftConnectsPerSecond` estão desativados por padrão. Adicione as seguintes informações ao arquivo de configuração do aplicativo para habilitá-las:  
  
```xml  
<system.diagnostics>  
  <switches>  
    <add name="ConnectionPoolPerformanceCounterDetail"  
         value="4"/>  
  </switches>  
</system.diagnostics>  
```  
  
## <a name="retrieving-performance-counter-values"></a>Recuperando valores do contador de desempenho  
 O aplicativo de console a seguir mostra como recuperar valores de contador de desempenho em seu aplicativo. As conexões devem estar abertas e ativas para que as informações sejam retornadas para todos os contadores de desempenho ADO.NET.  
  
> [!NOTE]
> Este exemplo usa o banco de dados **AdventureWorks** de exemplo incluído com SQL Server. As cadeias de conexão fornecidas no código de exemplo pressupõem que o banco de dados está instalado e disponível no computador local com um nome de instância de SQLExpress e que você criou SQL Server logons que correspondem àqueles fornecidos nas cadeias de conexão. Talvez seja necessário habilitar os logons SQL Server se o servidor estiver configurado usando as configurações de segurança padrão que permitem apenas a autenticação do Windows. Modifique as cadeias de conexão conforme necessário para se adequar ao seu ambiente.  
  
### <a name="example"></a>Exemplo  
  
```vb  
Option Explicit On  
Option Strict On  
  
Imports System.Data.SqlClient  
Imports System.Diagnostics  
Imports System.Runtime.InteropServices  
  
Class Program  
  
    Private PerfCounters(9) As PerformanceCounter  
    Private connection As SqlConnection = New SqlConnection  
  
    Public Shared Sub Main()  
        Dim prog As Program = New Program  
        ' Open a connection and create the performance counters.
        prog.connection.ConnectionString = _  
           GetIntegratedSecurityConnectionString()  
        prog.SetUpPerformanceCounters()  
        Console.WriteLine("Available Performance Counters:")  
  
        ' Create the connections and display the results.  
        prog.CreateConnections()  
        Console.WriteLine("Press Enter to finish.")  
        Console.ReadLine()  
    End Sub  
  
    Private Sub CreateConnections()  
        ' List the Performance counters.  
        WritePerformanceCounters()  
  
        ' Create 4 connections and display counter information.  
        Dim connection1 As SqlConnection = New SqlConnection( _  
           GetIntegratedSecurityConnectionString)  
        connection1.Open()  
        Console.WriteLine("Opened the 1st Connection:")  
        WritePerformanceCounters()  
  
        Dim connection2 As SqlConnection = New SqlConnection( _  
           GetSqlConnectionStringDifferent)  
        connection2.Open()  
        Console.WriteLine("Opened the 2nd Connection:")  
        WritePerformanceCounters()  
  
        Console.WriteLine("Opened the 3rd Connection:")  
        Dim connection3 As SqlConnection = New SqlConnection( _  
           GetSqlConnectionString)  
        connection3.Open()  
        WritePerformanceCounters()  
  
        Dim connection4 As SqlConnection = New SqlConnection( _  
           GetSqlConnectionString)  
        connection4.Open()  
        Console.WriteLine("Opened the 4th Connection:")  
        WritePerformanceCounters()  
  
        connection1.Close()  
        Console.WriteLine("Closed the 1st Connection:")  
        WritePerformanceCounters()  
  
        connection2.Close()  
        Console.WriteLine("Closed the 2nd Connection:")  
        WritePerformanceCounters()  
  
        connection3.Close()  
        Console.WriteLine("Closed the 3rd Connection:")  
        WritePerformanceCounters()  
  
        connection4.Close()  
        Console.WriteLine("Closed the 4th Connection:")  
        WritePerformanceCounters()  
    End Sub  
  
    Private Enum ADO_Net_Performance_Counters  
        NumberOfActiveConnectionPools  
        NumberOfReclaimedConnections  
        HardConnectsPerSecond  
        HardDisconnectsPerSecond  
        NumberOfActiveConnectionPoolGroups  
        NumberOfInactiveConnectionPoolGroups  
        NumberOfInactiveConnectionPools  
        NumberOfNonPooledConnections  
        NumberOfPooledConnections  
        NumberOfStasisConnections  
        ' The following performance counters are more expensive to track.  
        ' Enable ConnectionPoolPerformanceCounterDetail in your config file.  
        '     SoftConnectsPerSecond  
        '     SoftDisconnectsPerSecond  
        '     NumberOfActiveConnections  
        '     NumberOfFreeConnections  
    End Enum  
  
    Private Sub SetUpPerformanceCounters()  
        connection.Close()  
        Me.PerfCounters(9) = New PerformanceCounter()  
  
        Dim instanceName As String = GetInstanceName()  
        Dim apc As Type = GetType(ADO_Net_Performance_Counters)  
        Dim i As Integer = 0  
        Dim s As String = ""  
        For Each s In [Enum].GetNames(apc)  
            Me.PerfCounters(i) = New PerformanceCounter()  
            Me.PerfCounters(i).CategoryName = ".NET Data Provider for SqlServer"  
            Me.PerfCounters(i).CounterName = s  
            Me.PerfCounters(i).InstanceName = instanceName  
            i = (i + 1)  
        Next  
    End Sub  
  
    Private Declare Function GetCurrentProcessId Lib "kernel32.dll" () As Integer  
  
    Private Function GetInstanceName() As String  
        'This works for Winforms apps.
        Dim instanceName As String = _  
           System.Reflection.Assembly.GetEntryAssembly.GetName.Name  
  
        ' Must replace special characters like (, ), #, /, \\
        Dim instanceName2 As String = _  
           AppDomain.CurrentDomain.FriendlyName.ToString.Replace("(", "[") _  
           .Replace(")", "]").Replace("#", "_").Replace("/", "_").Replace("\\", "_")  
  
        'For ASP.NET applications your instanceName will be your CurrentDomain's
        'FriendlyName. Replace the line above that sets the instanceName with this:
        'instanceName = AppDomain.CurrentDomain.FriendlyName.ToString.Replace("(", "[") _  
        '    .Replace(")", "]").Replace("#", "_").Replace("/", "_").Replace("\\", "_")  
  
        Dim pid As String = GetCurrentProcessId.ToString  
        instanceName = (instanceName + ("[" & (pid & "]")))  
        Console.WriteLine("Instance Name: {0}", instanceName)  
        Console.WriteLine("---------------------------")  
        Return instanceName  
    End Function  
  
    Private Sub WritePerformanceCounters()  
        Console.WriteLine("---------------------------")  
        For Each p As PerformanceCounter In Me.PerfCounters  
            Console.WriteLine("{0} = {1}", p.CounterName, p.NextValue)  
        Next  
        Console.WriteLine("---------------------------")  
    End Sub  
  
    Private Shared Function GetIntegratedSecurityConnectionString() As String  
        ' To avoid storing the connection string in your code,
        ' you can retrieve it from a configuration file.
        Return ("Data Source=.\SqlExpress;Integrated Security=True;" &
          "Initial Catalog=AdventureWorks")  
    End Function  
  
    Private Shared Function GetSqlConnectionString() As String  
        ' To avoid storing the connection string in your code,
        ' you can retrieve it from a configuration file.
        Return ("Data Source=.\SqlExpress;User Id=LowPriv;Password=Data!05;" &
          "Initial Catalog=AdventureWorks")  
    End Function  
  
    Private Shared Function GetSqlConnectionStringDifferent() As String  
        ' To avoid storing the connection string in your code,
        ' you can retrieve it from a configuration file.
        Return ("Initial Catalog=AdventureWorks;Data Source=.\SqlExpress;" & _  
          "User Id=LowPriv;Password=Data!05;")  
    End Function  
End Class  
```  

```csharp  
using System;  
using System.Data.SqlClient;  
using System.Diagnostics;  
using System.Runtime.InteropServices;  
  
class Program  
{  
    PerformanceCounter[] PerfCounters = new PerformanceCounter[10];  
    SqlConnection connection = new SqlConnection();  
  
    static void Main()  
    {  
        Program prog = new Program();  
        // Open a connection and create the performance counters.  
        prog.connection.ConnectionString =  
           GetIntegratedSecurityConnectionString();  
        prog.SetUpPerformanceCounters();  
        Console.WriteLine("Available Performance Counters:");  
  
        // Create the connections and display the results.  
        prog.CreateConnections();  
        Console.WriteLine("Press Enter to finish.");  
        Console.ReadLine();  
    }  
  
    private void CreateConnections()  
    {  
        // List the Performance counters.  
        WritePerformanceCounters();  
  
        // Create 4 connections and display counter information.  
        SqlConnection connection1 = new SqlConnection(  
              GetIntegratedSecurityConnectionString());  
        connection1.Open();  
        Console.WriteLine("Opened the 1st Connection:");  
        WritePerformanceCounters();  
  
        SqlConnection connection2 = new SqlConnection(  
              GetSqlConnectionStringDifferent());  
        connection2.Open();  
        Console.WriteLine("Opened the 2nd Connection:");  
        WritePerformanceCounters();  
  
        SqlConnection connection3 = new SqlConnection(  
              GetSqlConnectionString());  
        connection3.Open();  
        Console.WriteLine("Opened the 3rd Connection:");  
        WritePerformanceCounters();  
  
        SqlConnection connection4 = new SqlConnection(  
              GetSqlConnectionString());  
        connection4.Open();  
        Console.WriteLine("Opened the 4th Connection:");  
        WritePerformanceCounters();  
  
        connection1.Close();  
        Console.WriteLine("Closed the 1st Connection:");  
        WritePerformanceCounters();  
  
        connection2.Close();  
        Console.WriteLine("Closed the 2nd Connection:");  
        WritePerformanceCounters();  
  
        connection3.Close();  
        Console.WriteLine("Closed the 3rd Connection:");  
        WritePerformanceCounters();  
  
        connection4.Close();  
        Console.WriteLine("Closed the 4th Connection:");  
        WritePerformanceCounters();  
    }  
  
    private enum ADO_Net_Performance_Counters  
    {  
        NumberOfActiveConnectionPools,  
        NumberOfReclaimedConnections,  
        HardConnectsPerSecond,  
        HardDisconnectsPerSecond,  
        NumberOfActiveConnectionPoolGroups,  
        NumberOfInactiveConnectionPoolGroups,  
        NumberOfInactiveConnectionPools,  
        NumberOfNonPooledConnections,  
        NumberOfPooledConnections,  
        NumberOfStasisConnections  
        // The following performance counters are more expensive to track.  
        // Enable ConnectionPoolPerformanceCounterDetail in your config file.  
        //     SoftConnectsPerSecond  
        //     SoftDisconnectsPerSecond  
        //     NumberOfActiveConnections  
        //     NumberOfFreeConnections  
    }  
  
    private void SetUpPerformanceCounters()  
    {  
        connection.Close();  
        this.PerfCounters = new PerformanceCounter[10];  
        string instanceName = GetInstanceName();  
        Type apc = typeof(ADO_Net_Performance_Counters);  
        int i = 0;  
        foreach (string s in Enum.GetNames(apc))  
        {  
            this.PerfCounters[i] = new PerformanceCounter();  
            this.PerfCounters[i].CategoryName = ".NET Data Provider for SqlServer";  
            this.PerfCounters[i].CounterName = s;  
            this.PerfCounters[i].InstanceName = instanceName;  
            i++;  
        }  
    }  
  
    [DllImport("kernel32.dll", SetLastError = true)]  
    static extern int GetCurrentProcessId();  
  
    private string GetInstanceName()  
    {  
        //This works for Winforms apps.  
        string instanceName =  
            System.Reflection.Assembly.GetEntryAssembly().GetName().Name;  
  
        // Must replace special characters like (, ), #, /, \\  
        string instanceName2 =  
            AppDomain.CurrentDomain.FriendlyName.ToString().Replace('(', '[')  
            .Replace(')', ']').Replace('#', '_').Replace('/', '_').Replace('\\', '_');  
  
        // For ASP.NET applications your instanceName will be your CurrentDomain's
        // FriendlyName. Replace the line above that sets the instanceName with this:  
        // instanceName = AppDomain.CurrentDomain.FriendlyName.ToString().Replace('(','[')  
        // .Replace(')',']').Replace('#','_').Replace('/','_').Replace('\\','_');  
  
        string pid = GetCurrentProcessId().ToString();  
        instanceName = instanceName + "[" + pid + "]";  
        Console.WriteLine("Instance Name: {0}", instanceName);  
        Console.WriteLine("---------------------------");  
        return instanceName;  
    }  
  
    private void WritePerformanceCounters()  
    {  
        Console.WriteLine("---------------------------");  
        foreach (PerformanceCounter p in this.PerfCounters)  
        {  
            Console.WriteLine("{0} = {1}", p.CounterName, p.NextValue());  
        }  
        Console.WriteLine("---------------------------");  
    }  
  
    private static string GetIntegratedSecurityConnectionString()  
    {  
        // To avoid storing the connection string in your code,  
        // you can retrieve it from a configuration file.  
        return @"Data Source=.\SqlExpress;Integrated Security=True;" +  
          "Initial Catalog=AdventureWorks";  
    }  
    private static string GetSqlConnectionString()  
    {  
        // To avoid storing the connection string in your code,  
        // you can retrieve it from a configuration file.  
        return @"Data Source=.\SqlExpress;User Id=LowPriv;Password=Data!05;" +  
          "Initial Catalog=AdventureWorks";  
    }  
  
    private static string GetSqlConnectionStringDifferent()  
    {  
        // To avoid storing the connection string in your code,  
        // you can retrieve it from a configuration file.  
        return @"Initial Catalog=AdventureWorks;Data Source=.\SqlExpress;" +  
          "User Id=LowPriv;Password=Data!05;";  
    }  
}  
```  

## <a name="see-also"></a>Confira também

- [Conectando a uma Fonte de Dados](connecting-to-a-data-source.md)
- [Conexão do Oracle, ODBC e OLE DB Pooling](ole-db-odbc-and-oracle-connection-pooling.md)
- [Contadores de Desempenho do ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/fxk122b4(v=vs.100))
- [Criação de perfil de tempo de execução](../../debug-trace-profile/runtime-profiling.md)
- [Introdução ao monitoramento de limites de desempenho](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/bd20x32d(v=vs.90))
- [Visão geral do ADO.NET](ado-net-overview.md)
