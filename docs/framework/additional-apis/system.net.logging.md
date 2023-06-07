---
title: Classe Logging (System.Net)
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.Logging
- System.Net.Logging.Associate
- System.Net.Logging.Enter
- System.Net.Logging.Exception
- System.Net.Logging.Exit
- System.Net.Logging.get_Http
- System.Net.Logging.get_On
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: ad85fdd41252ed147eb5fe1a9db029db046d720c
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990302"
---
# <a name="logging-class"></a>Classe Logging

Fornece a funcionalidade de log de rastreamento.

```csharp
internal class Logging
```

> [!WARNING]
> Essa classe é interna e não deve ser usada diretamente no seu código.
>
> A Microsoft não oferece suporte ao uso dessa classe em um aplicativo de produção em nenhuma circunstância.

## <a name="associate-method"></a>Associar método

Registra as informações de que dois objetos estão associados entre si.

```csharp
internal static void Associate(TraceSource traceSource, object objA, object objB)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `objA` <xref:System.Object>

  O objeto ao qual associar `objB` .

- `objB` <xref:System.Object>

  O objeto ao qual associar `objA` .

## <a name="entertracesource-object-string-string-method"></a>Inserir o método (Tracename, Object, String, String)

Registra a entrada em um método.

```csharp
internal static void Enter(TraceSource traceSource, object obj, string method, string param)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `obj` <xref:System.Object>

  O objeto no qual o método foi chamado.

- `method` <xref:System.String>

  O método que está sendo inserido.

- `param` <xref:System.String>

  Os parâmetros que foram passados para o método.

## <a name="entertracesource-object-string-object-method"></a>Inserir o método (Tracename, Object, String, Object)

Registra a entrada em um método.

```csharp
internal static void Enter(TraceSource traceSource, object obj, string method, object paramObject)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `obj` <xref:System.Object>

  O objeto no qual o método foi chamado.

- `method` <xref:System.String>

  O método que está sendo inserido.

- `paramObject` <xref:System.Object>

  Os parâmetros que foram passados para o método.

## <a name="entertracesource-string-string-string-method"></a>Inserir o método (Tracename, String, String, String)

Registra a entrada em um método.

```csharp
internal static void Enter(TraceSource traceSource, string obj, string method, string param)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `obj` <xref:System.String>

  O objeto no qual o método foi chamado.

- `method` <xref:System.String>

  O método que está sendo inserido.

- `param` <xref:System.String>

  Os parâmetros que foram passados para o método.

## <a name="entertracesource-string-string-object-method"></a>Inserir o método (Tracename, String, String, Object)

Registra a entrada em um método.

```csharp
internal static void Enter(TraceSource traceSource, string obj, string method, object paramObject)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `obj` <xref:System.String>

  O objeto no qual o método foi chamado.

- `method` <xref:System.String>

  O método que está sendo inserido.

- `paramObject` <xref:System.Object>

  Os parâmetros que foram passados para o método.

## <a name="entertracesource-string-string-method"></a>Insira o método (Tracename, String, String)

Registra a entrada em um método.

```csharp
internal static void Enter(TraceSource traceSource, string method, string parameters)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `method` <xref:System.String>

  O método que está sendo inserido.

- `parameters` <xref:System.String>

  Os parâmetros que foram passados para o método.

## <a name="entertracesource-string-method"></a>Insira o método (Tracename, String)

Registra a entrada em um método.

```csharp
internal static void Enter(TraceSource traceSource, string msg)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `msg` <xref:System.String>

  A mensagem de entrada para fazer logon na origem do rastreamento.

## <a name="exception-method"></a>Método Exception

Registra uma exceção e restaura o recuo.

```csharp
internal static void Exception(TraceSource traceSource, object obj, string method, Exception e)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `obj` <xref:System.Object>

  O objeto no qual o método que gerou uma exceção foi chamado.

- `method` <xref:System.String>

  O método que gerou a exceção.

- `e` <xref:System.Exception>

  A exceção que foi gerada.

## <a name="exittracesource-object-string-object-method"></a>Método Exit (Tracename, Object, String, Object)

Os logs saem de uma função.

```csharp
internal static void Exit(TraceSource traceSource, object obj, string method, object retObject)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `obj` <xref:System.Object>

  O objeto no qual o método foi chamado.

- `method` <xref:System.String>

  O método que está sendo encerrado.

- `retObject` <xref:System.Object>

  O valor que está sendo retornado pelo método.

## <a name="exittracesource-string-string-object-method"></a>Método Exit (Tracename, String, String, Object)

Os logs saem de uma função.

```csharp
internal static void Exit(TraceSource traceSource, string obj, string method, object retObject)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `obj` <xref:System.String>

  O objeto no qual o método foi chamado.

- `method` <xref:System.String>

  O método que está sendo encerrado.

- `retObject` <xref:System.Object>

  O valor que está sendo retornado pelo método.

## <a name="exittracesource-object-string-string-method"></a>Exit (Tracename, Object, String, String) do método

Os logs saem de uma função.

```csharp
internal static void Exit(TraceSource traceSource, object obj, string method, string retValue)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `obj` <xref:System.Object>

  O objeto no qual o método foi chamado.

- `method` <xref:System.String>

  O método que está sendo encerrado.

- `retValue` <xref:System.String>

  O valor que está sendo retornado pelo método.

## <a name="exittracesource-string-string-string-method"></a>Exit (Tracename, String, String, String) do método

Os logs saem de uma função.

```csharp
internal static void Exit(TraceSource traceSource, string obj, string method, string retValue)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `obj` <xref:System.String>

  O objeto no qual o método foi chamado.

- `method` <xref:System.String>

  O método que está sendo encerrado.

- `retValue` <xref:System.String>

  O valor que está sendo retornado pelo método.

## <a name="exittracesource-string-string-method"></a>Método Exit (Tracename, String, String)

Os logs saem de uma função.

```csharp
internal static void Exit(TraceSource traceSource, string method, string parameters)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `method` <xref:System.String>

  O método que está sendo encerrado.

- `parameters` <xref:System.String>

  Os parâmetros que foram passados para o método que está sendo encerrado.

## <a name="exittracesource-string-method"></a>Método Exit (Tracename, String)

Os logs saem de uma função.

```csharp
internal static void Exit(TraceSource traceSource, string msg)
```

### <a name="parameters"></a>Parâmetros

- `traceSource` <xref:System.Diagnostics.TraceSource>

  A origem do rastreamento no qual registrar o evento.

- `msg` <xref:System.String>

  A mensagem de saída a ser registrada na origem do rastreamento.

## <a name="http-property"></a>Propriedade http

Obtém a origem do rastreamento para "System .net. http".

```csharp
internal static TraceSource Http { get; }
```

### <a name="property-value"></a>Valor da propriedade

<xref:System.Diagnostics.TraceSource>\
A origem do rastreamento para "System .net. http" ou `null` se o registro em log não estiver habilitado.

## <a name="on-property"></a>Na propriedade

Obtém um valor que indica se o registro em log está habilitado.

```csharp
internal static bool On { get; }
```

### <a name="property-value"></a>Valor da propriedade

<xref:System.Boolean>\
O valor será `true` se o registro em log estiver habilitado; caso contrário, o valor será `false`.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)
