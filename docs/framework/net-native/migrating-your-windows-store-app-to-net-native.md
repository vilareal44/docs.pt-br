---
title: Migrando seu aplicativo da Windows Store para .NET Nativo
ms.date: 03/30/2017
ms.assetid: 4153aa18-6f56-4a0a-865b-d3da743a1d05
ms.openlocfilehash: 5e5c655d0e8d6f1730f27d35525692e110b3c80c
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309190"
---
# <a name="migrate-your-windows-store-app-to-net-native"></a>Migre seu aplicativo da Windows Store para .NET Native

.NET Native fornece compilação estática de aplicativos na Windows Store ou no computador do desenvolvedor. Isso difere da compilação dinâmica realizada para Aplicativos da Windows Store pelo compilador JIT (just-in-time) ou o [Gerador de Imagem Nativa (Ngen.exe)](../tools/ngen-exe-native-image-generator.md) no dispositivo. Apesar das diferenças, .NET Native tenta manter a compatibilidade com o [.net para aplicativos da Windows Store](https://docs.microsoft.com/previous-versions/windows/apps/br230302%28v=vs.140%29). Para a maior parte, as coisas que funcionam no .NET para aplicativos da Windows Store também funcionam com .NET Native.  No entanto, em alguns casos, você pode encontrar alterações de comportamento. Este documento discute essas diferenças entre o .NET Standard para aplicativos da Windows Store e .NET Native nas seguintes áreas:

- [Diferenças de runtime geral](#Runtime)

- [Diferenças de programação dinâmica](#Dynamic)

- [Outras diferenças relacionadas à reflexão](#Reflection)

- [Cenários e APIs sem suporte](#Unsupported)

- [Diferenças do Visual Studio](#VS)

<a name="Runtime"></a>

## <a name="general-runtime-differences"></a>Diferenças de runtime geral

- Exceções, como <xref:System.TypeLoadException> , que são geradas pelo compilador JIT quando um aplicativo é executado no Common Language Runtime (CLR) geralmente resultam em erros de tempo de compilação quando processados pelo .net Native.

- Não chame o método <xref:System.GC.WaitForPendingFinalizers%2A?displayProperty=nameWithType> de um thread de interface do usuário do aplicativo. Isso pode resultar em um deadlock no .NET Native.

- Não conte com ordenação de invocação do construtor de classe estática. Em .NET Native, a ordem de invocação é diferente da ordem no tempo de execução padrão. (Mesmo com o runtime padrão, você não deve confiar na ordem de execução de construtores de classe estáticos.)

- Loops infinitos sem fazer uma chamada (por exemplo, `while(true);`) em qualquer thread interromper o aplicativo. Da mesma forma, esperas grandes ou infinitas podem interromper o aplicativo.

- Determinados ciclos de inicialização genéricos não geram exceções no .NET Native. Por exemplo, o código a seguir gera uma exceção <xref:System.TypeLoadException> no CLR padrão. Em .NET Native, isso não é verdade.

  [!code-csharp[ProjectN#8](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/compat1.cs#8)]

- Em alguns casos, .NET Native fornece diferentes implementações de .NET Framework bibliotecas de classes. Um objeto retornado de um método sempre implementará os membros do tipo retornado. No entanto, desde que sua implementação de backup seja diferente, você não poderá convertê-lo para o mesmo conjunto de tipos como realizado em outras plataformas .NET Framework. Por exemplo, em alguns casos, você não poderá converter o objeto de interface <xref:System.Collections.Generic.IEnumerable%601> retornado por métodos como <xref:System.Reflection.TypeInfo.DeclaredMembers%2A?displayProperty=nameWithType> ou <xref:System.Reflection.TypeInfo.DeclaredProperties%2A?displayProperty=nameWithType> para `T[]`.

- O cache do WinInet não está habilitado por padrão no .NET para aplicativos da Windows Store, mas está em .NET Native. Isso melhora o desempenho, mas tem implicações de conjunto de trabalho. Nenhuma ação do desenvolvedor é necessária.

<a name="Dynamic"></a>

## <a name="dynamic-programming-differences"></a>Diferenças de programação dinâmica

.NET Native links estaticamente no código da .NET Framework para tornar o aplicativo de código local para o desempenho máximo. No entanto, os tamanhos binários precisam permanecer pequenos para que todo o .NET Framework não seja trazido. O compilador .NET Native resolve essa limitação usando um redutor de dependência que remove referências a código não utilizado. No entanto, .NET Native pode não manter ou gerar algumas informações de tipo e código quando essas informações não podem ser inferidas estaticamente no tempo de compilação, mas são recuperadas dinamicamente no tempo de execução.

.NET Native habilita a reflexão e a programação dinâmica. No entanto, nem todos os tipos podem ser marcados para reflexão, porque isso tornaria o tamanho do código gerado grande demais (especialmente porque há suporte para reflexão nas APIs públicas do .NET Framework). O compilador de .NET Native faz escolhas inteligentes sobre quais tipos devem dar suporte à reflexão e mantém os metadados e gera código apenas para esses tipos.

Por exemplo, a associação de dados necessita de um aplicativo para conseguir mapear nomes de propriedade às funções. Nos aplicativos .NET para Windows Store, o Common Language Runtime usa reflexão automaticamente para fornecer esse recurso para tipos nativos gerenciados e disponíveis publicamente. No .NET Native, o compilador inclui automaticamente metadados para os tipos aos quais você associa dados.

O compilador de .NET Native também pode manipular tipos genéricos comumente usados, como <xref:System.Collections.Generic.List%601> e <xref:System.Collections.Generic.Dictionary%602> , que funcionam sem a necessidade de nenhuma dica ou diretivas. A palavra-chave [dynamic](../../csharp/language-reference/builtin-types/reference-types.md#the-dynamic-type) também tem suporte dentro de certos limites.

> [!NOTE]
> Você deve testar completamente todos os caminhos de código dinâmico ao portar seu aplicativo para .NET Native.

A configuração padrão para .NET Native é suficiente para a maioria dos desenvolvedores, mas alguns desenvolvedores podem querer ajustar suas configurações usando um arquivo de diretivas de tempo de execução (.rd.xml). Além disso, em alguns casos, o compilador .NET Native não é capaz de determinar quais metadados devem estar disponíveis para reflexão e se baseia em dicas, especialmente nos seguintes casos:

- Algumas construções <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> e <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A?displayProperty=nameWithType> não podem ser determinadas estaticamente.

- Como o compilador não pode determinar as instanciações, um tipo genérico sobre o qual você deseja refletir deverá ser especificado por diretivas de runtime. Isso não é apenas porque todo o código deve ser incluído, mas sim porque a reflexão em tipos genéricos pode formar um ciclo infinito (por exemplo, quando um método genérico é invocado em um tipo genérico).

> [!NOTE]
> Diretivas de runtime são definidas em um arquivo de diretivas de runtime (.rd.xml). Para obter mais informações sobre como usar essa arquivo, consulte [Introdução](getting-started-with-net-native.md). Para obter informações sobre as diretivas de runtime, consulte [Referência do arquivo de configuração das diretivas de runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md).

.NET Native também inclui ferramentas de criação de perfil que ajudam o desenvolvedor a determinar quais tipos fora do conjunto padrão devem dar suporte à reflexão.

<a name="Reflection"></a>

## <a name="other-reflection-related-differences"></a>Outras diferenças relacionadas à reflexão

Há uma série de outras diferenças individuais relacionadas à reflexão em comportamento entre o .NET para aplicativos da Windows Store e .NET Native.

Em .NET Native:

- Não há suporte para reflexão privada sobre tipos e membros da biblioteca de classes do .NET Framework. No entanto, é possível refletir sobre seus próprios tipos e membros privados, bem como tipos e membros em bibliotecas de terceiros.

- A propriedade <xref:System.Reflection.ParameterInfo.HasDefaultValue%2A?displayProperty=nameWithType> retorna `false` corretamente para um objeto <xref:System.Reflection.ParameterInfo> que representa um valor de retorno. Nos aplicativos .NET para Windows Store, ele retorna `true`. A IL (linguagem intermediária) não dá suporte a isso diretamente, e a interpretação é deixada para o idioma.

- Membros públicos nas estruturas <xref:System.RuntimeFieldHandle> e <xref:System.RuntimeMethodHandle> não são suportadas. Esses tipos são suportados apenas para LINQ, árvores de expressão e inicialização de matriz estática.

- <xref:System.Reflection.RuntimeReflectionExtensions.GetRuntimeProperties%2A?displayProperty=nameWithType> e <xref:System.Reflection.RuntimeReflectionExtensions.GetRuntimeEvents%2A?displayProperty=nameWithType> incluem membros ocultos em classes base e, portanto, podem ser substituídos sem substituições explícitas. Isso também é verdadeiro para outros métodos [RuntimeReflectionExtensions.GetRuntime*](xref:System.Reflection.RuntimeReflectionExtensions).

- <xref:System.Type.MakeArrayType%2A?displayProperty=nameWithType>e <xref:System.Type.MakeByRefType%2A?displayProperty=nameWithType> não falham quando você tenta criar determinadas combinações (por exemplo, uma matriz de `byref` objetos).

- Você não pode usar reflexão para invocar os membros que têm parâmetros de ponteiro.

- Você não pode usar reflexão para obter ou definir um campo de ponteiro.

- Quando a contagem de argumentos está errada e o tipo de um dos argumentos está incorreto, .NET Native gera um <xref:System.ArgumentException> em vez de um <xref:System.Reflection.TargetParameterCountException> .

- A serialização binária de exceções geralmente não é suportada. Como resultado, objetos não serializáveis não podem ser adicionados ao dicionário <xref:System.Exception.Data%2A?displayProperty=nameWithType>.

<a name="Unsupported"></a>

## <a name="unsupported-scenarios-and-apis"></a>Cenários e APIs sem suporte

As seções a seguir listam cenários não suportados e APIs de desenvolvimento geral, interoperabilidade e tecnologias como HTTPClient e Windows Communication Foundation (WCF):

- [Desenvolvimento geral](#General)

- [HttpClient](#HttpClient)

- [Interoperabilidade](#Interop)

- [APIs sem suporte](#APIs)

<a name="General"></a>

### <a name="general-development-differences"></a>Diferenças de desenvolvimento geral

**Tipos de valor**

- Se você substituir os métodos <xref:System.ValueType.Equals%2A?displayProperty=nameWithType> e <xref:System.ValueType.GetHashCode%2A?displayProperty=nameWithType> para um tipo de valor, não chame as implementações de classe base. Em aplicativos .NET para Windows Store, esses métodos dependem de reflexão. No momento da compilação, .NET Native gera uma implementação que não depende da reflexão de tempo de execução. Isso significa que, se você não substituir esses dois métodos, eles funcionarão conforme o esperado, porque .NET Native gera a implementação no momento da compilação. No entanto, substituir esses métodos, mas chamar a implementação de classe base, resulta em uma exceção.

- Não há suporte para tipos de valor maiores que 1 megabyte.

- Tipos de valor não podem ter um construtor sem parâmetros em .NET Native. (C# e Visual Basic proíbem construtores sem parâmetros em tipos de valor. No entanto, eles podem ser criados em IL.)

**matrizes**

- Não há suporte para matrizes com um limite inferior diferente de zero. Normalmente, esses conjuntos são criados ao chamar a sobrecarga <xref:System.Array.CreateInstance%28System.Type%2CSystem.Int32%5B%5D%2CSystem.Int32%5B%5D%29?displayProperty=nameWithType>.

- A criação dinâmica de matrizes multidimensionais não tem suporte. Essas matrizes geralmente são criados chamando uma sobrecarga do método <xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> que inclui um parâmetro `lengths` ou chamando o método <xref:System.Type.MakeArrayType%28System.Int32%29?displayProperty=nameWithType>.

- Não há suporte para matrizes multidimensionais com quatro ou mais dimensões, ou seja, com valor da propriedade <xref:System.Array.Rank%2A?displayProperty=nameWithType> quatro ou superior. Use [matrizes denteadas](../../csharp/programming-guide/arrays/jagged-arrays.md) (uma matriz de matrizes) em vez disso. Por exemplo, `array[x,y,z]` é inválido, mas `array[x][y][z]` não é.

- Variação de matrizes multidimensionais não é suportada e causa uma exceção <xref:System.InvalidCastException> no tempo de execução.

**Genéricos**

- A expansão do tipo genérico infinito resulta em um erro do compilador. Por exemplo, esse código falha ao compilar:

  [!code-csharp[ProjectN#9](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/compat2.cs#9)]

**Ponteiros**

- Não há suporte para matrizes de ponteiros.

- Você não pode usar reflexão para obter ou definir um campo de ponteiro.

**Serialização**

O atributo <xref:System.Runtime.Serialization.KnownTypeAttribute.%23ctor%28System.String%29> não é suportado. Use o atributo <xref:System.Runtime.Serialization.KnownTypeAttribute.%23ctor%28System.Type%29> em vez disso.

**Recursos**

O uso de recursos localizados com a classe <xref:System.Diagnostics.Tracing.EventSource> não é suportado. A propriedade <xref:System.Diagnostics.Tracing.EventSourceAttribute.LocalizationResources%2A?displayProperty=nameWithType> não define os recursos localizados.

**Delegados**

`Delegate.BeginInvoke` e `Delegate.EndInvoke` não são suportados.

**APIs diversas**

- A propriedade [typeInfo. GUID](xref:System.Type.GUID) gera uma <xref:System.PlatformNotSupportedException> exceção se um <xref:System.Runtime.InteropServices.GuidAttribute> atributo não é aplicado ao tipo. O GUID é usado principalmente para suporte a COM.

- O <xref:System.DateTime.Parse%2A?displayProperty=nameWithType> método analisa corretamente as cadeias de caracteres que contêm datas curtas em .net Native. Contudo, ele não mantém a compatibilidade com as alterações na análise de data e hora descritas no artigos da Base de Dados de Conhecimento Microsoft [KB2803771](https://support.microsoft.com/kb/2803771) e [KB2803755](https://support.microsoft.com/kb/2803755).

- <xref:System.Numerics.BigInteger.ToString%2A?displayProperty=nameWithType>`("E")`é arredondado corretamente em .net Native. Em algumas versões do CLR, a cadeia de caracteres de resultado é truncada em vez de arredondada.

<a name="HttpClient"></a>

### <a name="httpclient-differences"></a>Diferenças do HttpClient

Em .NET Native, a <xref:System.Net.Http.HttpClientHandler> classe internamente usa o WinInet (por meio da <xref:Windows.Web.Http.Filters.HttpBaseProtocolFilter> classe) em vez das <xref:System.Net.WebRequest> <xref:System.Net.WebResponse> classes e usadas no .NET padrão para aplicativos da Windows Store.  O WinINet não oferece suporte a todas as opções de configuração que a classe <xref:System.Net.Http.HttpClientHandler> suporta.  Como resultado:

- Algumas das propriedades de funcionalidade no <xref:System.Net.Http.HttpClientHandler> retorno `false` no .net Native, enquanto elas retornam `true` no .NET padrão para aplicativos da Windows Store.

- Alguns acessadores de propriedade de configuração `get` sempre retornam um valor fixo em .net Native que é diferente do valor padrão configurável no .net para aplicativos da Windows Store.

Algumas diferenças comportamentais adicionais são abrangidas nas subseções a seguir.

**Proxy**

A <xref:Windows.Web.Http.Filters.HttpBaseProtocolFilter> classe não dá suporte à configuração ou substituição do proxy por solicitação.  Isso significa que todas as solicitações em .NET Native usam o servidor proxy configurado pelo sistema ou nenhum servidor proxy, dependendo do valor da <xref:System.Net.Http.HttpClientHandler.UseProxy%2A?displayProperty=nameWithType> propriedade.  Em aplicativos .NET para Windows Store, o servidor proxy é definido pela propriedade <xref:System.Net.Http.HttpClientHandler.Proxy%2A?displayProperty=nameWithType>.  Em .NET Native, a configuração de <xref:System.Net.Http.HttpClientHandler.Proxy%2A?displayProperty=nameWithType> para um valor diferente de `null` gera uma <xref:System.PlatformNotSupportedException> exceção.  A <xref:System.Net.Http.HttpClientHandler.SupportsProxy%2A?displayProperty=nameWithType> propriedade retorna `false` em .net Native, enquanto retorna `true` na .NET Framework padrão para aplicativos da Windows Store.

**Redirecionamento automático**

A <xref:Windows.Web.Http.Filters.HttpBaseProtocolFilter> classe não permite que o número máximo de redirecionamentos automáticos seja configurado.  O valor da propriedade <xref:System.Net.Http.HttpClientHandler.MaxAutomaticRedirections%2A?displayProperty=nameWithType> é 50 por padrão nos aplicativos .NET para Windows Store e pode ser modificada. Em .NET Native, o valor dessa propriedade é 10 e tentar modificá-la gera uma <xref:System.PlatformNotSupportedException> exceção.  A <xref:System.Net.Http.HttpClientHandler.SupportsRedirectConfiguration%2A?displayProperty=nameWithType> propriedade retorna `false` em .net Native, enquanto ela retorna `true` no .net para aplicativos da Windows Store.

**Descompactação automática**

Aplicativos .NET para Windows Store permitem definir a propriedade <xref:System.Net.Http.HttpClientHandler.AutomaticDecompression%2A?displayProperty=nameWithType> para <xref:System.Net.DecompressionMethods.Deflate>, <xref:System.Net.DecompressionMethods.GZip>, ambas <xref:System.Net.DecompressionMethods.Deflate> e <xref:System.Net.DecompressionMethods.GZip>, ou <xref:System.Net.DecompressionMethods.None>.  .NET Native só dá suporte <xref:System.Net.DecompressionMethods.Deflate> ao <xref:System.Net.DecompressionMethods.GZip> , ou <xref:System.Net.DecompressionMethods.None> .  Tentar definir a propriedade <xref:System.Net.Http.HttpClientHandler.AutomaticDecompression%2A> para <xref:System.Net.DecompressionMethods.Deflate> ou <xref:System.Net.DecompressionMethods.GZip> sozinho silenciosamente define-a para <xref:System.Net.DecompressionMethods.Deflate> e <xref:System.Net.DecompressionMethods.GZip>.

**Arar**

A manipulação de cookies é executada simultaneamente por <xref:System.Net.Http.HttpClient> e WinINet.  Os cookies do <xref:System.Net.CookieContainer> são combinados com cookies no cache de cookies do WinINet.  Remover um cookie do <xref:System.Net.CookieContainer> impede que <xref:System.Net.Http.HttpClient> envie o cookie, mas se o cookie já foi visto pelo WinINet e os cookies não foram excluídos pelo usuário, o WinINet o envia.  Não é possível programar a remoção automática de um cookie do WinINet usando a API <xref:System.Net.Http.HttpClient>, <xref:System.Net.Http.HttpClientHandler> ou <xref:System.Net.CookieContainer>.  Definir a propriedade <xref:System.Net.Http.HttpClientHandler.UseCookies%2A?displayProperty=nameWithType> para `false` faz com que apenas <xref:System.Net.Http.HttpClient> pare de enviar cookies; o WinINet ainda pode incluir os cookies na solicitação.

**Credenciais**

Em aplicativos .NET para Windows Store, as propriedades <xref:System.Net.Http.HttpClientHandler.UseDefaultCredentials%2A?displayProperty=nameWithType> e <xref:System.Net.Http.HttpClientHandler.Credentials%2A?displayProperty=nameWithType> funcionam de forma independente.  Além disso, a propriedade <xref:System.Net.Http.HttpClientHandler.Credentials%2A> aceita qualquer objeto que implemente a interface <xref:System.Net.ICredentials>.  Em .NET Native, definir a <xref:System.Net.Http.HttpClientHandler.UseDefaultCredentials%2A> propriedade como `true` faz com que a <xref:System.Net.Http.HttpClientHandler.Credentials%2A> propriedade se torne `null` .  Além disso, a propriedade <xref:System.Net.Http.HttpClientHandler.Credentials%2A> pode ser definida somente para `null`, <xref:System.Net.CredentialCache.DefaultCredentials%2A> ou um objeto do tipo <xref:System.Net.NetworkCredential>.  Atribuir de qualquer outro objeto <xref:System.Net.ICredentials>, sendo o mais popular o <xref:System.Net.CredentialCache>, para a propriedade <xref:System.Net.Http.HttpClientHandler.Credentials%2A>, gera uma <xref:System.PlatformNotSupportedException>.

**Outros recursos sem suporte ou não configuráveis**

Em .NET Native:

- O valor da propriedade <xref:System.Net.Http.HttpClientHandler.ClientCertificateOptions%2A?displayProperty=nameWithType> sempre é <xref:System.Net.Http.ClientCertificateOption.Automatic>.  Em aplicativos .NET para Windows Store, o padrão é <xref:System.Net.Http.ClientCertificateOption.Manual>.

- A propriedade <xref:System.Net.Http.HttpClientHandler.MaxRequestContentBufferSize%2A?displayProperty=nameWithType> não é configurável.

- A propriedade <xref:System.Net.Http.HttpClientHandler.PreAuthenticate%2A?displayProperty=nameWithType> sempre é `true`.  Em aplicativos .NET para Windows Store, o padrão é `false`.

- O cabeçalho `SetCookie2` em respostas será ignorado como obsoleto.

<a name="Interop"></a>
### <a name="interop-differences"></a>Diferenças de interoperabilidade
 **APIs obsoletas**

 Várias APIs pouco usadas para a interoperabilidade com código gerenciado foram preteridas. Quando usado com .NET Native, essas APIs podem gerar uma <xref:System.NotImplementedException> <xref:System.PlatformNotSupportedException> exceção ou, ou resultar em um erro do compilador. Em aplicativos .NET para Windows Store, essas APIs são marcadas como obsoletas, embora chamá-las gere um aviso do compilador em vez de um erro do compilador.

 As APIs preteridas para o `VARIANT` marshaling incluem:

- <xref:System.Runtime.InteropServices.BStrWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.CurrencyWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.DispatchWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.ErrorWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.UnknownWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.VariantWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.UnmanagedType.IDispatch?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.VarEnum?displayProperty=nameWithType>

 <xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=nameWithType>tem suporte, mas gera uma exceção em alguns cenários, como quando ele é usado com [IDispatch](https://docs.microsoft.com/previous-versions/windows/desktop/api/oaidl/nn-oaidl-idispatch) ou `byref` variantes.

 As APIs preteridas para suporte a [IDispatch](https://docs.microsoft.com/previous-versions/windows/desktop/api/oaidl/nn-oaidl-idispatch) incluem:

- <xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDispatch?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDual?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.ComDefaultInterfaceAttribute?displayProperty=nameWithType>

APIs preteridas para eventos COM clássicos incluem:

- <xref:System.Runtime.InteropServices.ComEventsHelper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.ComSourceInterfacesAttribute>

APIs preteridas na <xref:System.Runtime.InteropServices.ICustomQueryInterface?displayProperty=nameWithType> interface, que não têm suporte no .net Native, incluem:

- <xref:System.Runtime.InteropServices.ICustomQueryInterface?displayProperty=nameWithType>(todos os membros)
- <xref:System.Runtime.InteropServices.CustomQueryInterfaceMode?displayProperty=nameWithType>(todos os membros)
- <xref:System.Runtime.InteropServices.CustomQueryInterfaceResult?displayProperty=nameWithType>(todos os membros)
- <xref:System.Runtime.InteropServices.Marshal.GetComInterfaceForObject%28System.Object%2CSystem.Type%2CSystem.Runtime.InteropServices.CustomQueryInterfaceMode%29?displayProperty=fullName>

Outros recursos de interoperabilidade sem suporte incluem:

- <xref:System.Runtime.InteropServices.ICustomAdapter?displayProperty=nameWithType>(todos os membros)
- <xref:System.Runtime.InteropServices.SafeBuffer?displayProperty=nameWithType>(todos os membros)
- <xref:System.Runtime.InteropServices.UnmanagedType.Currency?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.UnmanagedType.VBByRefStr?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.UnmanagedType.AnsiBStr?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.UnmanagedType.AsAny?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.UnmanagedType.CustomMarshaler?displayProperty=fullName>

 APIs de marshaling raramente usadas:

- <xref:System.Runtime.InteropServices.Marshal.ReadByte%28System.Object%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.ReadInt16%28System.Object%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.ReadInt32%28System.Object%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.ReadInt64%28System.Object%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.ReadIntPtr%28System.Object%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.WriteByte%28System.Object%2CSystem.Int32%2CSystem.Byte%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.WriteInt16%28System.Object%2CSystem.Int32%2CSystem.Int16%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.WriteInt32%28System.Object%2CSystem.Int32%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.WriteInt64%28System.Object%2CSystem.Int32%2CSystem.Int64%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.WriteIntPtr%28System.Object%2CSystem.Int32%2CSystem.IntPtr%29?displayProperty=fullName>

 **Invocação de plataforma e compatibilidade de interoperabilidade COM**

 A maioria dos cenários de invocação de plataforma e de interoperabilidade COM ainda tem suporte no .NET Native. Em especial, toda a interoperabilidade com APIs do Windows Runtime (WinRT) APIs e todo o marshaling necessário para o Windows Runtime são suportados. Isso inclui suporte a marshaling para:

- Matrizes (incluindo <xref:System.Runtime.InteropServices.UnmanagedType.ByValArray?displayProperty=nameWithType>)

- `BStr`

- Delegados

- Cadeias de caracteres (Unicode, ANSI e HSTRING)

- Estruturas (`byref` e `byval`)

- Uniões

- Identificadores do Win32

- Todos os constructos do WinRT

- Suporte parcial aos tipos variantes de marshaling. Há suporte para o seguinte:

  - <xref:System.Boolean>

  - <xref:System.Byte>

  - <xref:System.Decimal>

  - <xref:System.Double>

  - <xref:System.Int16>

  - <xref:System.Int32>

  - <xref:System.Int64>

  - <xref:System.SByte>

  - <xref:System.Single>

  - <xref:System.UInt16>

  - <xref:System.UInt32>

  - <xref:System.UInt64>

  - `BStr`

  - [IUnknown](/windows/desktop/api/unknwn/nn-unknwn-iunknown)

No entanto, o .NET Native não dá suporte ao seguinte:

- Usando os eventos clássicos do COM

- Implementando a interface <xref:System.Runtime.InteropServices.ICustomQueryInterface?displayProperty=nameWithType> em um tipo gerenciado

- Implementando a interface [IDispatch](https://docs.microsoft.com/previous-versions/windows/desktop/api/oaidl/nn-oaidl-idispatch) em um tipo gerenciado por meio do atributo <xref:System.Runtime.InteropServices.ComDefaultInterfaceAttribute?displayProperty=nameWithType>. No entanto, você não pode chamar objetos COM por meio `IDispatch` do e o objeto gerenciado não pode implementar `IDispatch` .

Usar reflexão para invocar um método de invocação de plataforma não é suportado. Você pode contornar essa limitação envolvendo a chamada de método em outro método e usando reflexão para chamar o wrapper em vez disso.

<a name="APIs"></a>

### <a name="other-differences-from-net-apis-for-windows-store-apps"></a>Outras diferenças das APIs .NET para Windows Store

Esta seção lista as APIs restantes que não têm suporte no .NET Native. O maior conjunto de APIs sem suporte é a Windows Communication Foundation APIs (WCF).

**DataAnnotations (System.ComponentModel.DataAnnotations)**

Os tipos nos <xref:System.ComponentModel.DataAnnotations> <xref:System.ComponentModel.DataAnnotations.Schema> namespaces e não têm suporte no .net Native. Isso inclui os seguintes tipos que estão presentes no .NET para aplicativos da Windows Store para Windows 8:

- <xref:System.ComponentModel.DataAnnotations.AssociationAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.ConcurrencyCheckAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.CustomValidationAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.DataType?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.DataTypeAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.DisplayAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.DisplayColumnAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.DisplayFormatAttribute>
- <xref:System.ComponentModel.DataAnnotations.EditableAttribute>
- <xref:System.ComponentModel.DataAnnotations.EnumDataTypeAttribute>
- <xref:System.ComponentModel.DataAnnotations.FilterUIHintAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.KeyAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.RangeAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.RequiredAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.StringLengthAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.TimestampAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.UIHintAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.ValidationAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.ValidationContext?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.ValidationException?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.ValidationResult?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.Validator?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedOption?displayProperty=nameWithType>

 **Visual Basic**

Atualmente, não há suporte para Visual Basic no .NET Native. Os seguintes tipos nos <xref:Microsoft.VisualBasic> <xref:Microsoft.VisualBasic.CompilerServices> namespaces e não estão disponíveis no .net Native:

- <xref:Microsoft.VisualBasic.CallType?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Constants?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.HideModuleNameAttribute?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Strings?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.Conversions?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.IncompleteInitialization?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.NewLateBinding?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.ObjectFlowControl?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.ObjectFlowControl.ForLoopControl?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.Operators?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.OptionTextAttribute?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.ProjectData?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.StandardModuleAttribute?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.StaticLocalInitFlag?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.Utils?displayProperty=nameWithType>

**Contexto de reflexão (namespace System.Reflection.Context)**

A <xref:System.Reflection.Context.CustomReflectionContext?displayProperty=nameWithType> classe não tem suporte no .net Native.

**RTC (System.Net.Http.Rtc)**

A `System.Net.Http.RtcRequestFactory` classe não tem suporte no .net Native.

**Windows Communication Foundation (WCF) (System.ServiceModel.\*)**

Os tipos nos [namespaces System. ServiceModel. *](xref:System.ServiceModel) não têm suporte no .net Native. Eles incluem os seguintes tipos:

- <xref:System.ServiceModel.ActionNotSupportedException?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpBinding?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpMessageCredentialType?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpSecurity?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpSecurityMode?displayProperty=nameWithType>
- <xref:System.ServiceModel.CallbackBehaviorAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.ChannelFactory?displayProperty=nameWithType>
- <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601.BeginOperationDelegate?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601.ChannelBase%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601.EndOperationDelegate?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601.InvokeAsyncCompletedEventArgs?displayProperty=nameWithType>
- <xref:System.ServiceModel.CommunicationException?displayProperty=nameWithType>
- <xref:System.ServiceModel.CommunicationObjectAbortedException?displayProperty=nameWithType>
- <xref:System.ServiceModel.CommunicationObjectFaultedException?displayProperty=nameWithType>
- <xref:System.ServiceModel.CommunicationState?displayProperty=nameWithType>
- <xref:System.ServiceModel.DataContractFormatAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.DnsEndpointIdentity?displayProperty=nameWithType>
- <xref:System.ServiceModel.DuplexChannelFactory%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.DuplexClientBase%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.EndpointAddress?displayProperty=nameWithType>
- <xref:System.ServiceModel.EndpointAddressBuilder?displayProperty=nameWithType>
- <xref:System.ServiceModel.EndpointIdentity?displayProperty=nameWithType>
- <xref:System.ServiceModel.EndpointNotFoundException?displayProperty=nameWithType>
- <xref:System.ServiceModel.EnvelopeVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.ExceptionDetail?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultCode?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultContractAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultException?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultException%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultReason?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultReasonText?displayProperty=nameWithType>
- <xref:System.ServiceModel.HttpBindingBase?displayProperty=nameWithType>
- <xref:System.ServiceModel.HttpClientCredentialType?displayProperty=nameWithType>
- <xref:System.ServiceModel.HttpTransportSecurity?displayProperty=nameWithType>
- <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.ICommunicationObject?displayProperty=nameWithType>
- <xref:System.ServiceModel.IContextChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.IDefaultCommunicationTimeouts?displayProperty=nameWithType>
- <xref:System.ServiceModel.IExtensibleObject%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.IExtension%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.IExtensionCollection%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.InstanceContext?displayProperty=nameWithType>
- <xref:System.ServiceModel.InvalidMessageContractException?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageBodyMemberAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageContractAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageContractMemberAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageCredentialType?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageHeader%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageHeaderException?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageParameterAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageSecurityOverTcp?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageSecurityVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.NetHttpBinding?displayProperty=nameWithType>
- <xref:System.ServiceModel.NetHttpMessageEncoding?displayProperty=nameWithType>
- <xref:System.ServiceModel.NetTcpBinding?displayProperty=nameWithType>
- <xref:System.ServiceModel.NetTcpSecurity?displayProperty=nameWithType>
- <xref:System.ServiceModel.OperationContext?displayProperty=nameWithType>
- <xref:System.ServiceModel.OperationContextScope?displayProperty=nameWithType>
- <xref:System.ServiceModel.OperationContractAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.OperationFormatStyle?displayProperty=nameWithType>
- <xref:System.ServiceModel.ProtocolException?displayProperty=nameWithType>
- <xref:System.ServiceModel.QuotaExceededException?displayProperty=nameWithType>
- <xref:System.ServiceModel.SecurityMode?displayProperty=nameWithType>
- <xref:System.ServiceModel.ServerTooBusyException?displayProperty=nameWithType>
- <xref:System.ServiceModel.ServiceActivationException?displayProperty=nameWithType>
- <xref:System.ServiceModel.ServiceContractAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.ServiceKnownTypeAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.SpnEndpointIdentity?displayProperty=nameWithType>
- <xref:System.ServiceModel.TcpClientCredentialType?displayProperty=nameWithType>
- <xref:System.ServiceModel.TcpTransportSecurity?displayProperty=nameWithType>
- <xref:System.ServiceModel.TransferMode?displayProperty=nameWithType>
- <xref:System.ServiceModel.UnknownMessageReceivedEventArgs?displayProperty=nameWithType>
- <xref:System.ServiceModel.UpnEndpointIdentity?displayProperty=nameWithType>
- <xref:System.ServiceModel.XmlSerializerFormatAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.AddressHeader?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.AddressHeaderCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.AddressingVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.ChannelManagerBase?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.ChannelParameterCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.CommunicationObject?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.CompressionFormat?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.CustomBinding?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.FaultConverter?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.HttpRequestMessageProperty?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.HttpResponseMessageProperty?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.HttpsTransportBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.HttpTransportBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IChannelFactory?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IChannelFactory%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IDuplexChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IDuplexSession?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IDuplexSessionChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IHttpCookieContainerManager?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IInputChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IInputSession?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IInputSessionChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IMessageProperty?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IOutputChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IOutputSession?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IOutputSessionChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IRequestChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IRequestSessionChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.ISession?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.ISessionChannel%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.LocalClientSecuritySettings?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.Message?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageBuffer?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageEncoder?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageEncoderFactory?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageFault?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageHeader?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageHeaderInfo?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageHeaders?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageProperties?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageState?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.RequestContext?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.SecurityBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.SecurityHeaderLayout?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.TcpConnectionPoolSettings?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.TcpTransportBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.TransportBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.TransportSecurityBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.WebSocketTransportSettings?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.WebSocketTransportUsage?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.ClientCredentials?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.ContractDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.FaultDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.FaultDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.IEndpointBehavior?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.IOperationBehavior?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageBodyDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageDirection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageHeaderDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageHeaderDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessagePartDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessagePartDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessagePropertyDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessagePropertyDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.OperationDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.OperationDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.ServiceEndpoint?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.ClientOperation?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.ClientRuntime?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.DispatchOperation?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.DispatchRuntime?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.EndpointDispatcher?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.IClientMessageFormatter?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.IClientMessageInspector?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.IClientOperationSelector?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.IParameterInspector?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.BasicSecurityProfileVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.HttpDigestClientCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.MessageSecurityException?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.SecureConversationVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.SecurityAccessDeniedException?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.SecurityPolicyVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.SecurityVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.TrustVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.UserNamePasswordClientCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.WindowsClientCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.Tokens.SupportingTokenParameters?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.Tokens.UserNameSecurityTokenParameters?displayProperty=nameWithType>

### <a name="differences-in-serializers"></a>Diferenças nos serializadores

As seguintes diferenças envolvem serialização e desserialização com e classes <xref:System.Runtime.Serialization.DataContractSerializer>, <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> e <xref:System.Xml.Serialization.XmlSerializer>:

- Em .NET Native, <xref:System.Runtime.Serialization.DataContractSerializer> e <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> falha ao serializar ou desserializar uma classe derivada que tem um membro de classe base cujo tipo não é um tipo de serialização raiz. Por exemplo, no código a seguir, tentar serializar ou desserializar `Y` resulta em um erro:

  [!code-csharp[ProjectN#10](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/compat3.cs#10)]

  O tipo `InnerType` não é conhecido pelo serializador, pois os membros da classe base não são percorridos durante a serialização.

- <xref:System.Runtime.Serialization.DataContractSerializer> e <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> não conseguem serializar uma classe ou estrutura que implementa a interface <xref:System.Collections.Generic.IEnumerable%601>. Por exemplo, os seguintes tipos falham ao serializar ou desserializar:

- <xref:System.Xml.Serialization.XmlSerializer> falha ao serializar o valor de objeto a seguir, porque não sabe o tipo exato de objeto a ser serializado:

- <xref:System.Xml.Serialization.XmlSerializer> falha ao serializar ou desserializar se o tipo do objeto serializado é <xref:System.Xml.XmlQualifiedName>.

- Todos os serializadores (<xref:System.Runtime.Serialization.DataContractSerializer>, <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> e <xref:System.Xml.Serialization.XmlSerializer>) falham ao gerar o código de serialização para tipo <xref:System.Xml.Linq.XElement?displayProperty=nameWithType> ou para um tipo que contém <xref:System.Xml.Linq.XElement>. Eles exibem erros de tempo de compilação em vez disso.

- Não há garantia que os seguintes construtores dos tipos de serialização funcionem conforme o esperado:

  - <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29>

  - <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.Runtime.Serialization.DataContractSerializerSettings%29>

  - <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.String%2CSystem.String%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29>

  - <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.Xml.XmlDictionaryString%2CSystem.Xml.XmlDictionaryString%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29>

  - <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.%23ctor%28System.Type%2CSystem.Runtime.Serialization.Json.DataContractJsonSerializerSettings%29>

  - <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.%23ctor%28System.Type%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29>

  - <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.String%29>

  - <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Type%5B%5D%29>

  - <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Xml.Serialization.XmlAttributeOverrides%29>

  - <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Xml.Serialization.XmlRootAttribute%29>

  - <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Xml.Serialization.XmlAttributeOverrides%2CSystem.Type%5B%5D%2CSystem.Xml.Serialization.XmlRootAttribute%2CSystem.String%29>

- O <xref:System.Xml.Serialization.XmlSerializer> falha ao gerar o código para um tipo que tem métodos atribuídos com qualquer um dos seguintes atributos:

  - <xref:System.Runtime.Serialization.OnSerializingAttribute>

  - <xref:System.Runtime.Serialization.OnSerializedAttribute>

  - <xref:System.Runtime.Serialization.OnDeserializingAttribute>

  - <xref:System.Runtime.Serialization.OnDeserializedAttribute>

- O <xref:System.Xml.Serialization.XmlSerializer> não aceita a interface de serialização personalizada <xref:System.Xml.Serialization.IXmlSerializable>. Se você tiver uma classe que implementa essa interface, <xref:System.Xml.Serialization.XmlSerializer> considera o tipo como um objeto CLR (POCO) antigo simples, serializando apenas suas propriedades públicas.

- A serialização de um objeto sem formatação <xref:System.Exception> não funciona bem com o <xref:System.Runtime.Serialization.DataContractSerializer> e o <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> .

<a name="VS"></a>

## <a name="visual-studio-differences"></a>Diferenças do Visual Studio

**Exceções e depuração**

Quando você estiver executando aplicativos compilados usando .NET Native no depurador, as exceções de primeira chance serão habilitadas para os seguintes tipos de exceção:

- <xref:System.MemberAccessException>

- <xref:System.TypeAccessException>

**Criando aplicativos**

Use as ferramentas de criação x86 usadas por padrão pelo Visual Studio. Não recomendamos usar as ferramentas AMD64 MSBuild, que se encontram em C:\Program Files (x86)\MSBuild\12.0\bin\amd64, pois podem criar problemas de compilação.

**Criadores de perfil**

- O criador de perfil de CPU do Visual Studio e o criador de perfil de memória XAML não exibem o Just-My-Code corretamente.

- O gerador de perfil de memória XAML não exibe corretamente os dados da pilha gerenciada.

- O gerador de perfil de CPU não identifica corretamente módulos e exibe nomes de função de prefixo.

**Projetos da biblioteca de teste de unidade**

A habilitação de .NET Native em uma biblioteca de teste de unidade para um projeto de aplicativos da Windows Store não tem suporte e faz com que o projeto falhe ao ser compilado.

## <a name="see-also"></a>Confira também

- [Introdução](getting-started-with-net-native.md)
- [Referência do arquivo de configuração de diretivas do runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md)
- [Visão geral dos aplicativos .NET para Windows Store](https://docs.microsoft.com/previous-versions/windows/apps/br230302%28v=vs.140%29)
- [Suporte do .NET Framework para Aplicativos da Windows Store e Windows Runtime](../../standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md)
