---
title: Classe MissingRuntimeArtifactException (.NET Nativo)
ms.date: 03/30/2017
ms.assetid: d5b3d13e-689f-4584-8ba6-44f5167a8590
ms.openlocfilehash: 7a69add45202b3ad838de592fadc82a84fa0ba5d
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79180975"
---
# <a name="missingruntimeartifactexception-class-net-native"></a>Classe MissingRuntimeArtifactException (.NET Nativo)
**.NET para aplicativos do Windows para Windows 10, somente .NET Native**  
  
 A exceção que é acionada quando metadados de um tipo ou um membro de tipo estão disponíveis, mas sua implementação foi removida.  
  
 **Namespace:** System.Reflection  
  
> [!IMPORTANT]
> A `MissingRuntimeArtifactException` classe é destinada exclusivamente ao uso interno da cadeia de ferramentas .net Native. Ela não é destinado para uso em código de terceiros e você também não deve tratar a exceção no seu código do aplicativo. Em vez disso, elimine a exceção adicionando entradas ao seu [arquivo de diretivas de runtime](runtime-directives-rd-xml-configuration-file-reference.md). Para obter mais informações, consulte a seção Comentários.  
  
## <a name="syntax"></a>Sintaxe  
 [!code-csharp[ProjectN#22](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/missingruntimeartifactexception_syntax1.cs#22)]  
  
 Observe que a `MissingRuntimeArtifactException` classe deriva de <xref:System.MemberAccessException>.  
  
 A classe `MissingRuntimeArtifactException` tem os seguintes membros:  
  
## <a name="constructors"></a>Construtores  
  
|Construtor|Descrição|  
|-----------------|-----------------|  
|`public MissingRuntimeArtifactException()`|Inicializa uma nova instância da classe `MissingRuntimeArtifactException` usando uma mensagem fornecida pelo sistema que descreve o erro.<br /><br /> Este construtor é para uso interno somente pela cadeia de ferramentas .NET Native.|  
|`public MissingRuntimeArtifactException(String message)`|Inicializa uma nova instância da classe `MissingRuntimeArtifactException` com uma mensagem de erro especificada.<br /><br /> Este construtor é para uso interno somente pela cadeia de ferramentas .NET Native.|  
  
## <a name="properties"></a>Propriedades  
  
|Propriedade|Descrição|  
|--------------|-----------------|  
|`public IDictionary Data { get; }`|Obtém uma coleção de pares de chave/valor que fornecem informações definidas pelo usuário adicionais sobre a exceção. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
|`public string HelpLink { get; set; }`|Obtém ou define um link para o arquivo de ajuda associado a essa exceção. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
|`public int HResult { get; protected set; }`|Obtém ou define o `HRESULT`, um valor numérico codificado que é atribuído a uma exceção específica. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
|`public Exception InnerException { get; }`|Obtém a exceção que causou a exceção atual. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
|`public string Message { get; }`|Obtém uma mensagem que descreve a exceção atual. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
|`public string Source { get; set; }`|Obtém ou define o nome do aplicativo ou objeto que causou o erro. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
|`public string StackTrace { get; }`|Obtém uma representação de cadeia de caracteres de quadros imediatos na pilha de chamadas. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
|`public MethodBase TargetSite { get; }`|Obtém o método que acionou a exceção atual. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|`public bool Equals(Object obj)`|Determina se o objeto especificado é igual ao objeto atual.  (Herdado de <xref:System.Object>.)|  
|`protected void Finalize()`|Permite que um objeto tente liberar recursos e executar outras operações de limpeza antes de ser recuperado pela coleta de lixo. (Herdado de <xref:System.Object>.)|  
|`public Exception GetBaseException()`|Retorna a exceção é a causa raiz de uma ou mais exceções subsequentes. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
|`public int GetHashCode()`|Retorna um código de hash para uma instância `MissingRuntimeArtifactException`.   (Herdado de <xref:System.Object>.)|  
|`public void GetObjectData(SerializationInfo info, StreamingContext context)`|Define um objeto <xref:System.Runtime.Serialization.SerializationInfo> com informações sobre a exceção.  (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
|`public Type GetType()`|Obtém o tipo de runtime da instância atual. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
|`protected Object MemberwiseClone()`|Cria uma cópia superficial do objeto atual. (Herdado de <xref:System.Object>.)|  
|`public string ToString()`|Retorna a representação de cadeia de caracteres de exceção atual. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
  
## <a name="events"></a>Eventos  
  
|Evento|Descrição|  
|-----------|-----------------|  
|`protected event EventHandler<SafeSerializationEventArgs> SerializeObjectState`|Ocorre quando uma exceção é serializada para criar um objeto de estado de exceção que contém dados serializados sobre a exceção. (Herdado de <xref:System.Exception?displayProperty=nameWithType>.)|  
  
## <a name="usage-details"></a>Detalhes de uso  
 A exceção `MissingRuntimeArtifactException` é gerada quando ocorre uma tentativa de instanciar um tipo ou invocar um membro de tipo e, embora o tipo ou metadados do membro estejam presentes, sua implementação foi removida.  
  
 Se os metadados e o código de implementação para executar dinamicamente um método estão disponíveis para um aplicativo em tempo de execução é definido pelo arquivo de diretivas de tempo de execução (configuração XML), \* . Rd. xml. Para impedir que o seu aplicativo gere esta exceção, você deve modificar o \*.rd.xml para garantir que os metadados necessários para um tipo ou membro de tipo estejam presentes no tempo de execução. Para obter informações sobre o formato do arquivo \*.rd.xml, consulte [Referência do arquivo de configuração das diretivas de runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md).  
  
> [!IMPORTANT]
> Como essa exceção indica que o código de implementação exigido pelo seu aplicativo não está disponível no tempo de execução, você não deve tratar essa exceção em um bloco `try`/`catch`. Em vez disso, você deve diagnosticar a causa da exceção e eliminá-la usando um arquivo de diretivas de runtime. Normalmente, você elimina essa exceção especificando a política apropriada `Activate` `Dynamic` para um elemento de programa no arquivo de diretivas de tempo de execução ( \* arquivo. Rd. xml). Para obter a entrada que você pode adicionar ao seu arquivo de diretivas de runtime e que elimina a exceção, você pode usar uma das duas soluções de problemas:  
>
> - A [solução de problemas MissingMetadataException](https://dotnet.github.io/native/troubleshooter/type.html) para tipos.  
> - A [solução de problemas MissingMetadataException](https://dotnet.github.io/native/troubleshooter/method.html) para métodos.  
  
 A classe `MissingRuntimeArtifactException` não contém membros exclusivos. Todos os seus membros são herdados de sua classe base, <xref:System.MemberAccessException>.  
  
## <a name="see-also"></a>Confira também

- [Referência do arquivo de configuração de diretivas do runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md)
- [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md)
