---
title: Serialização binária
description: Este artigo descreve a serialização binária e os tipos para os quais o .NET Core dá suporte. Lembre-se dos perigos de serialização binária e alternativas para ele.
ms.date: 01/02/2018
helpviewer_keywords:
- binary serialization
- serialization, about serialization
- deserialization
- binary serialization, about serialization
- binary serialization, .net core serialization
- serialization, cross-framework
ms.assetid: 2b1ea3be-1152-4032-b2b3-07794054c405
author: ViktorHofer
ms.openlocfilehash: c735d30920fd3c8cd13243b4a5a29489ce05b262
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84289688"
---
# <a name="binary-serialization"></a>Serialização binária

A serialização pode ser definida como o processo de armazenar o estado de um objeto em uma mídia de armazenamento. Durante esse processo, os campos públicos e privados do objeto e o nome da classe, incluindo o assembly que contém a classe, são convertidos em um fluxo de bytes, que é em seguida escrito em um fluxo de dados. Quando o objeto é desserializado posteriormente, um clone exato do objeto original é criado.

Ao implementar um mecanismo de serialização em um ambiente orientado a objeto, você precisará fazer algumas trocas entre a facilidade de uso e a flexibilidade. O processo pode ser automatizado em grande parte, contanto que você tenha controle suficiente sobre o processo. Por exemplo, em algumas situações, a serialização binária simples pode não ser suficiente, ou pode haver um motivo específico para decidir quais campos em uma classe precisam ser serializados. As seções a seguir examinam o mecanismo de serialização robusto fornecido com o .NET e destacam vários recursos importantes que permitem personalizar o processo para atender às suas necessidades.

> [!NOTE]
> O estado de um objeto codificado UTF-8 ou UTF-7 não é preservado se o objeto é serializado e desserializado usando versões diferentes do .NET Framework.

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

A serialização binária permite modificar membros privados dentro de um objeto e, portanto, alterar o estado dele. Por isso, são recomendadas outras estruturas de serialização, como <xref:System.Text.Json?displayProperty=fullName> , que operam na superfície da API pública.

## <a name="net-core"></a>.NET Core

O .NET Core dá suporte à serialização binária para um subconjunto de tipos. Você pode ver a lista de tipos com suporte na seção [tipos serializáveis](#serializable-types) a seguir. Os tipos listados têm a garantia de serem serializáveis entre .NET Framework 4.5.1 e versões posteriores e entre o .NET Core 2,0 e versões posteriores. Outras implementações do .NET, como mono, não são oficialmente suportadas, mas também devem funcionar.

### <a name="serializable-types"></a>Tipos serializáveis

> [!div class="mx-tdCol2BreakAll"]
> | Type | Observações |
> | - | - |
> | <xref:Microsoft.CSharp.RuntimeBinder.RuntimeBinderException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:Microsoft.CSharp.RuntimeBinder.RuntimeBinderInternalCompilerException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.AccessViolationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.AggregateException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.AppDomainUnloadedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ApplicationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ArgumentException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ArgumentNullException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ArgumentOutOfRangeException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ArithmeticException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Array?displayProperty=nameWithType> | |
> | <xref:System.ArraySegment%601?displayProperty=nameWithType> | |
> | <xref:System.ArrayTypeMismatchException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Attribute?displayProperty=nameWithType> | |
> | <xref:System.BadImageFormatException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Boolean?displayProperty=nameWithType> | |
> | <xref:System.Byte?displayProperty=nameWithType> | |
> | <xref:System.CannotUnloadAppDomainException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Char?displayProperty=nameWithType> | |
> | <xref:System.Collections.ArrayList?displayProperty=nameWithType> | |
> | <xref:System.Collections.BitArray?displayProperty=nameWithType> | |
> | <xref:System.Collections.Comparer?displayProperty=nameWithType> | |
> | <xref:System.Collections.DictionaryEntry?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.Comparer%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.EqualityComparer%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.HashSet%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.KeyNotFoundException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Collections.Generic.KeyValuePair%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.LinkedList%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.Queue%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.SortedDictionary%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.SortedList%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.SortedSet%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.Stack%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Hashtable?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.Collection%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.KeyedCollection%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.ObservableCollection%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.ReadOnlyCollection%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.ReadOnlyDictionary%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.ReadOnlyObservableCollection%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Queue?displayProperty=nameWithType> | |
> | <xref:System.Collections.SortedList?displayProperty=nameWithType> | |
> | <xref:System.Collections.Specialized.HybridDictionary?displayProperty=nameWithType> | |
> | <xref:System.Collections.Specialized.ListDictionary?displayProperty=nameWithType> | |
> | <xref:System.Collections.Specialized.OrderedDictionary?displayProperty=nameWithType> | |
> | <xref:System.Collections.Specialized.StringCollection?displayProperty=nameWithType> | |
> | <xref:System.Collections.Specialized.StringDictionary?displayProperty=nameWithType> | |
> | <xref:System.Collections.Stack?displayProperty=nameWithType> | |
> | `System.Collections.Generic.NonRandomizedStringEqualityComparer` | A partir do .NET Core 2.0.4. |
> | <xref:System.ComponentModel.BindingList%601?displayProperty=nameWithType> | |
> | <xref:System.ComponentModel.DataAnnotations.ValidationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ComponentModel.Design.CheckoutException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ComponentModel.InvalidAsynchronousStateException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ComponentModel.InvalidEnumArgumentException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ComponentModel.LicenseException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4.<br/>Não há suporte para a serialização de .NET Framework para o .NET Core. |
> | <xref:System.ComponentModel.WarningException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ComponentModel.Win32Exception?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Configuration.ConfigurationErrorsException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Configuration.ConfigurationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Configuration.Provider.ProviderException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Configuration.SettingsPropertyIsReadOnlyException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Configuration.SettingsPropertyNotFoundException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Configuration.SettingsPropertyWrongTypeException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ContextMarshalException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DBNull?displayProperty=nameWithType> | A partir do .NET Core 2.0.2 e versões posteriores. |
> | <xref:System.Data.Common.DbException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.ConstraintException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.DBConcurrencyException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.DataException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.DataSet?displayProperty=nameWithType> | |
> | <xref:System.Data.DataTable?displayProperty=nameWithType> | Se você definir `RemotingFormat` como `SerializationFormat.Binary` , ele só poderá ser trocado com o .net Core 2,1 e versões posteriores. |
> | <xref:System.Data.DeletedRowInaccessibleException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.DuplicateNameException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.EvaluateException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.InRowChangingEventException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.InvalidConstraintException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.InvalidExpressionException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.MissingPrimaryKeyException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.NoNullAllowedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.Odbc.OdbcException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.OperationAbortedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.PropertyCollection?displayProperty=nameWithType> | |
> | <xref:System.Data.ReadOnlyException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.RowNotInTableException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.SqlClient.SqlException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4.<br/>Não há suporte para a serialização de .NET Framework para o .NET Core |
> | <xref:System.Data.SqlTypes.SqlAlreadyFilledException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.SqlTypes.SqlBoolean?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlByte?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlDateTime?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlDouble?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlGuid?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlInt16?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlInt32?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlInt64?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlNotFilledException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.SqlTypes.SqlNullValueException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.SqlTypes.SqlString?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlTruncateException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.SqlTypes.SqlTypeException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.StrongTypingException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.SyntaxErrorException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Data.VersionNotFoundException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DataMisalignedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DateTime?displayProperty=nameWithType> | |
> | <xref:System.DateTimeOffset?displayProperty=nameWithType> | |
> | <xref:System.Decimal?displayProperty=nameWithType> | |
> | `System.Diagnostics.Contracts.ContractException` | A partir do .NET Core 2.0.4. |
> | <xref:System.Diagnostics.Tracing.EventSourceException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.IO.DirectoryNotFoundException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.AccountManagement.MultipleMatchesException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.AccountManagement.NoMatchingPrincipalException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.AccountManagement.PasswordException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.AccountManagement.PrincipalException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.AccountManagement.PrincipalExistsException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.AccountManagement.PrincipalOperationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.AccountManagement.PrincipalServerDownException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryObjectExistsException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryObjectNotFoundException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryOperationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryServerDownException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.ActiveDirectory.ForestTrustCollisionException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.ActiveDirectory.SyncFromAllServersOperationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.DirectoryServicesCOMException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.Protocols.BerConversionException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.Protocols.DirectoryException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.Protocols.DirectoryOperationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.Protocols.LdapException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DirectoryServices.Protocols.TlsOperationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DivideByZeroException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.DllNotFoundException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Double?displayProperty=nameWithType> | |
> | <xref:System.Drawing.Color?displayProperty=nameWithType> | |
> | <xref:System.Drawing.Point?displayProperty=nameWithType> | |
> | <xref:System.Drawing.PointF?displayProperty=nameWithType> | |
> | <xref:System.Drawing.Rectangle?displayProperty=nameWithType> | |
> | <xref:System.Drawing.RectangleF?displayProperty=nameWithType> | |
> | <xref:System.Drawing.Size?displayProperty=nameWithType> | |
> | <xref:System.Drawing.SizeF?displayProperty=nameWithType> | |
> | <xref:System.DuplicateWaitObjectException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.EntryPointNotFoundException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Enum?displayProperty=nameWithType> | |
> | <xref:System.EventArgs?displayProperty=nameWithType> | A partir do .NET Core 2.0.6. |
> | <xref:System.Exception?displayProperty=nameWithType> | |
> | <xref:System.ExecutionEngineException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.FieldAccessException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.FormatException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Globalization.CompareInfo?displayProperty=nameWithType> | |
> | <xref:System.Globalization.CultureNotFoundException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Globalization.SortVersion?displayProperty=nameWithType> | |
> | <xref:System.Guid?displayProperty=nameWithType> | |
> | `System.IO.Compression.ZLibException` | A partir do .NET Core 2.0.4. |
> | <xref:System.IO.DriveNotFoundException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.IO.EndOfStreamException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.IO.FileFormatException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.IO.FileLoadException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.IO.FileNotFoundException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.IO.IOException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.IO.InternalBufferOverflowException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.IO.InvalidDataException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.IO.IsolatedStorage.IsolatedStorageException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.IO.PathTooLongException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.IndexOutOfRangeException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.InsufficientExecutionStackException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.InsufficientMemoryException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Int16?displayProperty=nameWithType> | |
> | <xref:System.Int32?displayProperty=nameWithType> | |
> | <xref:System.Int64?displayProperty=nameWithType> | |
> | <xref:System.IntPtr?displayProperty=nameWithType> | |
> | <xref:System.InvalidCastException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.InvalidOperationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.InvalidProgramException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.InvalidTimeZoneException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.MemberAccessException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.MethodAccessException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.MissingFieldException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.MissingMemberException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.MissingMethodException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.MulticastNotSupportedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Net.Cookie?displayProperty=nameWithType> | |
> | <xref:System.Net.CookieCollection?displayProperty=nameWithType> | |
> | <xref:System.Net.CookieContainer?displayProperty=nameWithType> | |
> | <xref:System.Net.CookieException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Net.HttpListenerException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Net.Mail.SmtpException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Net.Mail.SmtpFailedRecipientException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Net.Mail.SmtpFailedRecipientsException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Net.NetworkInformation.NetworkInformationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Net.NetworkInformation.PingException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Net.ProtocolViolationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Net.Sockets.SocketException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Net.WebException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Net.WebSockets.WebSocketException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.NotFiniteNumberException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.NotImplementedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.NotSupportedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.NullReferenceException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Nullable%601?displayProperty=nameWithType> | |
> | <xref:System.Numerics.BigInteger?displayProperty=nameWithType> | |
> | <xref:System.Numerics.Complex?displayProperty=nameWithType> | |
> | <xref:System.Object?displayProperty=nameWithType> | |
> | <xref:System.ObjectDisposedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.OperationCanceledException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.OutOfMemoryException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.OverflowException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.PlatformNotSupportedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.RankException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Reflection.AmbiguousMatchException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Reflection.CustomAttributeFormatException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Reflection.InvalidFilterCriteriaException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Reflection.ReflectionTypeLoadException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4.<br/>Não há suporte para a serialização de .NET Framework para o .NET Core. |
> | <xref:System.Reflection.TargetException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Reflection.TargetInvocationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Reflection.TargetParameterCountException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Resources.MissingManifestResourceException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Resources.MissingSatelliteAssemblyException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Runtime.CompilerServices.RuntimeWrappedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Runtime.InteropServices.COMException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Runtime.InteropServices.ExternalException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Runtime.InteropServices.InvalidComObjectException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Runtime.InteropServices.InvalidOleVariantTypeException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Runtime.InteropServices.MarshalDirectiveException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Runtime.InteropServices.SEHException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Runtime.InteropServices.SafeArrayRankMismatchException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Runtime.InteropServices.SafeArrayTypeMismatchException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Runtime.Serialization.InvalidDataContractException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Runtime.Serialization.SerializationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.SByte?displayProperty=nameWithType> | |
> | <xref:System.Security.AccessControl.PrivilegeNotHeldException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Security.Authentication.AuthenticationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Security.Authentication.InvalidCredentialException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Security.Cryptography.CryptographicException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Security.Cryptography.CryptographicUnexpectedOperationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | `System.Security.Cryptography.Xml.CryptoSignedXmlRecursionException` | A partir do .NET Core 2.0.4. |
> | <xref:System.Security.HostProtectionException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Security.Policy.PolicyException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Security.Principal.IdentityNotMappedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Security.SecurityException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4.<br/>Dados de serialização limitados. |
> | <xref:System.Security.VerificationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Security.XmlSyntaxException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ServiceProcess.TimeoutException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Single?displayProperty=nameWithType> | |
> | <xref:System.StackOverflowException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.String?displayProperty=nameWithType> | |
> | <xref:System.StringComparer?displayProperty=nameWithType> | |
> | <xref:System.SystemException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Text.DecoderFallbackException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Text.EncoderFallbackException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Text.RegularExpressions.RegexMatchTimeoutException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Text.StringBuilder?displayProperty=nameWithType> | |
> | <xref:System.Threading.AbandonedMutexException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Threading.BarrierPostPhaseException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Threading.LockRecursionException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Threading.SemaphoreFullException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Threading.SynchronizationLockException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Threading.Tasks.TaskCanceledException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Threading.Tasks.TaskSchedulerException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Threading.ThreadAbortException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Threading.ThreadInterruptedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Threading.ThreadStartException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Threading.ThreadStateException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Threading.WaitHandleCannotBeOpenedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.TimeSpan?displayProperty=nameWithType> | |
> | <xref:System.TimeZoneInfo.AdjustmentRule?displayProperty=nameWithType> | |
> | <xref:System.TimeZoneInfo?displayProperty=nameWithType> | |
> | <xref:System.TimeZoneNotFoundException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.TimeoutException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Transactions.TransactionAbortedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Transactions.TransactionException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Transactions.TransactionInDoubtException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Transactions.TransactionManagerCommunicationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Transactions.TransactionPromotionException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Tuple?displayProperty=nameWithType> | |
> | <xref:System.TypeAccessException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.TypeInitializationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.TypeLoadException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.TypeUnloadedException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.UInt16?displayProperty=nameWithType> | |
> | <xref:System.UInt32?displayProperty=nameWithType> | |
> | <xref:System.UInt64?displayProperty=nameWithType> | |
> | <xref:System.UIntPtr?displayProperty=nameWithType> | |
> | <xref:System.UnauthorizedAccessException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Uri?displayProperty=nameWithType> | |
> | <xref:System.UriFormatException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.ValueTuple?displayProperty=nameWithType> | Não serializável no .NET Framework 4,7 e em versões anteriores. |
> | <xref:System.ValueType?displayProperty=nameWithType> | |
> | <xref:System.Version?displayProperty=nameWithType> | |
> | <xref:System.WeakReference%601?displayProperty=nameWithType> | |
> | <xref:System.WeakReference?displayProperty=nameWithType> | |
> | <xref:System.Xml.Schema.XmlSchemaException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Xml.Schema.XmlSchemaInferenceException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Xml.Schema.XmlSchemaValidationException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Xml.XPath.XPathException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Xml.XmlException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Xml.Xsl.XsltCompileException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |
> | <xref:System.Xml.Xsl.XsltException?displayProperty=nameWithType> | A partir do .NET Core 2.0.4. |

## <a name="see-also"></a>Confira também

- <xref:System.Runtime.Serialization>\
Contém classes que podem ser usadas para serialização e desserialização de objetos.

- [Serialização de XML e SOAP](xml-and-soap-serialization.md)\
Descreve o mecanismo de serialização de XML que está incluído com o Common Language Runtime.

- [Segurança e serialização](../../framework/misc/security-and-serialization.md)\
Descreve as diretrizes para codificação segura para seguir ao escrever o código que executa a serialização.

- [Comunicação remota do .NET](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/72x4h507(v=vs.100))\
Descreve os vários métodos que começam no .NET Framework para comunicação remota.

- [Serviços Web XML criados usando ASP.NET e clientes de serviço Web XML](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/7bkzywba(v=vs.100))\
Artigos que descrevem e explicam como programar serviços Web XML criados usando o ASP.NET.
