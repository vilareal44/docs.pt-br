---
title: Metadados e componentes autodescritivos
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- runtime, metadata
- languages, interoperability
- portable executable files, metadata
- self-describing files
- metadata, about metadata
- common language runtime, metadata
- PE files, metadata
- components [.NET Framework], metadata
ms.assetid: 3dd13c5d-a508-455b-8dce-0a852882a5a7
ms.openlocfilehash: 5327bd70b05bac8970fa9802fb15e94ba5f686c8
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290052"
---
# <a name="metadata-and-self-describing-components"></a>Metadados e componentes autodescritivos

No passado, um componente de software (.exe ou .dll) escrito em uma linguagem não podia usar facilmente um componente de software escrito em outra linguagem. COM foi um passo para a solução desse problema. O .NET Framework facilita ainda mais a interoperação entre componentes permitindo que compiladores emitam informações declarativas adicionais sobre todos os módulos e assemblies. Essas informações, chamadas de metadados, ajudam os componentes a interagirem perfeitamente.

 Metadados são informações binárias que descrevem o seu programa, armazenadas em um arquivo PE (Portable Executable) do Common Language Runtime ou na memória. Quando você compila seu código em um arquivo PE, os metadados são inseridos em uma parte do arquivo, e o código é convertido em MSIL (Microsoft Intermediate Language) e inserido em outra parte do arquivo. Cada tipo e membro definido e referenciado em um módulo ou assembly é descrito em metadados. Quando o código é executado, o runtime carrega os metadados na memória e os referencia para descobrir informações sobre suas classes de código, membros, herança etc.

 Os metadados descrevem cada tipo e membro definido no seu código de maneira neutra em relação à linguagem. Os metadados armazenam as seguintes informações:

- Descrição do assembly.

  - Identidade (nome, versão, cultura, chave pública).

  - Os tipos que são exportados.

  - Outros assemblies dos quais esse assembly dependa.

  - Permissões de segurança necessárias à execução.

- Descrição dos tipos.

  - Nome, visibilidade, classe base e interfaces implementadas.

  - Membros (métodos, campos, propriedades, eventos, tipos aninhados).

- Atributos.

  - Elementos descritivos adicionais que modificam tipos e membros.

## <a name="benefits-of-metadata"></a>Benefícios dos metadados

Os metadados são a chave para um modelo de programação mais simples e eliminam a necessidade de arquivos IDL (Interface Definition Language), arquivos de cabeçalho ou qualquer método externo de referência a componente. Os metadados permitem que linguagens .NET Framework se descrevam automaticamente de maneira neutra em relação à linguagem, sem que o desenvolvedor e o usuário vejam. Além disso, metadados são extensíveis pelo uso de atributos. Os metadados oferecem os seguintes benefícios principais:

- Arquivos autodescritivos.

  Módulos e assemblies do Common Language Runtime são autodescritivos. Os metadados de um módulo contêm tudo de que ele precisa para interagir com outro módulo. Como os metadados fornecem automaticamente a funcionalidade de IDL em COM, você pode usar um arquivo para definição e implementação. Módulos e assemblies do runtime sequer exigem o registro no sistema operacional. Como resultado, as descrições usadas pelo runtime sempre refletem o código real no arquivo compilado, o que aumenta a confiabilidade do aplicativo.

- Interoperabilidade de linguagem e facilidade de design com base em componentes.

  Os metadados fornecem todas as informações necessárias sobre código compilado para você herdar uma classe de um arquivo PE escrita em uma linguagem diferente. Você pode criar uma instância de qualquer classe escrita em qualquer linguagem gerenciada (qualquer linguagem que segmente o Common Language Runtime), sem se preocupar com o marshaling explícito ou com o uso de código de interoperabilidade personalizado.

- Atributos.

  O .NET Framework permite declarar tipos específicos de metadados, chamados atributos, no seu arquivo compilado. Os atributos podem ser encontrados em todo o .NET Framework e são usados para controlar mais detalhadamente como o seu programa se comporta no tempo de execução. Além disso, você pode emitir seus próprios metadados personalizados em arquivos do .NET Framework por meio de atributos definidos pelo usuário. Para obter mais informações, consulte [Atributos](attributes/index.md).

## <a name="metadata-and-the-pe-file-structure"></a>Metadados e a estrutura de arquivos PE

Os metadados são armazenados em uma seção de um arquivo PE (Portable Executable) do .NET Framework, e o MSIL (Microsoft Intermediate Language) é armazenado em outra seção do arquivo PE. A parte de metadados do arquivo contém uma série de estruturas de tabela e de dados do heap. A parte MSIL contém tokens MSIL e de metadados que referenciam a parte de metadados do arquivo PE. Você pode encontrar tokens de metadados ao usar ferramentas como o [MSIL Disassembler (Ildasm.exe)](../framework/tools/ildasm-exe-il-disassembler.md) para exibir o MSIL de seu código, por exemplo.

### <a name="metadata-tables-and-heaps"></a>Tabelas e heaps de metadados

Cada tabela de metadados contém informações sobre os elementos do seu programa. Por exemplo, uma tabela de metadados descreve as classes em seu código, outra descreve os campos e assim por diante. Se existirem dez classes em seu código, a tabela de classes terá dez linhas, uma para cada classe. Tabelas de metadados referenciam outras tabelas e heaps. Por exemplo, a tabela de metadados de classes referencia a tabela de métodos.

Metadados também armazenam informações em quatro estruturas de heap: cadeia de caracteres, blob, cadeia de caracteres de usuário e GUID. Todas as cadeias de caracteres usadas para nomear tipos e membros são armazenadas no heap da cadeia de caracteres. Por exemplo, uma tabela de métodos não armazena diretamente o nome de um método específico, mas aponta para o nome do método armazenado no heap da cadeia de caracteres.

### <a name="metadata-tokens"></a>Tokens de metadados

Cada linha de cada tabela de metadados é identificada com exclusividade na parte MSIL do arquivo PE por um token de metadados. Tokens de metadados são conceitualmente semelhantes a ponteiros, persistentes no MSIL, e referenciam uma tabela de metadados específica.

Um token de metadados é um número de quatro bytes. O byte superior denota a tabela de metadados a que um token específico se refere (método, tipo etc.). Os três bytes restantes especificam a linha na tabela de metadados que corresponde ao elemento de programação descrito. Se você definir um método em C# e compilá-lo em um arquivo PE, o seguinte token de metadados poderá existir na parte MSIL do arquivo PE:

`0x06000004`

O byte superior (`0x06`) indica que esse é um token **MethodDef**. Os três bytes inferiores (`000004`) pedem para o Common Language Runtime examinar na quarta linha da tabela **MethodDef** em busca das informações que descrevem essa definição de método.

### <a name="metadata-within-a-pe-file"></a>Metadados em um arquivo PE

Quando um programa é compilado para o Common Language Runtime, ele é convertido em um arquivo PE que consiste em três partes. A tabela a seguir descreve o conteúdo de cada parte.

|Seção PE|Conteúdo da seção PE|
|----------------|----------------------------|
|Cabeçalho PE|O índice das seções principais do arquivo PE e o endereço do ponto de entrada.<br /><br /> O ambiente de runtime usa essas informações para identificar o arquivo como um arquivo PE e determinar onde a runtime começa ao carregar o programa na memória.|
|Instruções MSIL|As instruções MSIL que compõem o seu código. Muitas instruções MSIL são acompanhadas de tokens de metadados.|
|Metadados|Tabelas e heaps de metadados. O runtime usa esta seção para registrar informações todos os tipos e membros em seu código. Esta seção também inclui atributos personalizados e informações de segurança.|

## <a name="run-time-use-of-metadata"></a>Uso de metadados em tempo de execução

Para compreender melhor os metadados e sua função no Common Language Runtime, pode ser útil criar um programa simples e ilustrar como os metadados afetam sua própria vida de tempo de execução. O exemplo de código a seguir mostra dois métodos dentro de uma classe chamada `MyApp`. O método `Main` é o ponto de entrada do programa, e o método `Add` apenas retorna a soma dos dois argumentos inteiros.

```vb
Public Class MyApp
   Public Shared Sub Main()
      Dim ValueOne As Integer = 10
      Dim ValueTwo As Integer = 20
      Console.WriteLine("The Value is: {0}", Add(ValueOne, ValueTwo))
   End Sub

   Public Shared Function Add(One As Integer, Two As Integer) As Integer
      Return (One + Two)
   End Function
End Class
```

```csharp
using System;
public class MyApp
{
   public static int Main()
   {
      int ValueOne = 10;
      int ValueTwo = 20;
      Console.WriteLine("The Value is: {0}", Add(ValueOne, ValueTwo));
      return 0;
   }
   public static int Add(int One, int Two)
   {
      return (One + Two);
   }
}
```

Quando o código é executado, o runtime carrega o módulo na memória e consulta os metadados dessa classe. Quando carregado, o runtime executa uma análise abrangente do fluxo MSIL (Microsoft Intermediate Language) do método para convertê-lo em instruções de máquina rápidas nativas. O runtime usa um compilador JIT (just-in-time) para converter as instruções MSIL em código de máquina nativo, um método por vez, conforme necessário.

O exemplo a seguir mostra parte do MSIL produzido a partir da função `Main` do código anterior. Você pode exibir o MSIL e os metadados de qualquer aplicativo .NET Framework usando o [MSIL Disassembler (Ildasm.exe)](../framework/tools/ildasm-exe-il-disassembler.md).

```console
.entrypoint
.maxstack  3
.locals ([0] int32 ValueOne,
         [1] int32 ValueTwo,
         [2] int32 V_2,
         [3] int32 V_3)
IL_0000:  ldc.i4.s   10
IL_0002:  stloc.0
IL_0003:  ldc.i4.s   20
IL_0005:  stloc.1
IL_0006:  ldstr      "The Value is: {0}"
IL_000b:  ldloc.0
IL_000c:  ldloc.1
IL_000d:  call int32 ConsoleApplication.MyApp::Add(int32,int32) /* 06000003 */
```

O compilador JIT lê o MSIL para o método inteiro o analisa-o por completo, além de gerar instruções nativas eficientes para o método. No `IL_000d`, um token de metadados do método `Add` ( `/*` `06000003 */`) é encontrado e o runtime usa o token para consultar a terceira linha da tabela **MethodDef**.

A tabela a seguir mostra parte da tabela **MethodDef** referenciada pelo token de metadados que descreve o método `Add`. Embora haja outras tabelas de metadados nesse assembly e elas tenham seus próprios valores exclusivos, somente essa tabela é examinada.

|Linha|RVA (endereço virtual relativo)|ImplFlags|Flags|Nome<br /><br /> (Aponta para o heap da cadeia de caracteres.)|Assinatura (Aponta para o heap de blob.)|
|---------|--------------------------------------|---------------|-----------|-----------------------------------------|----------------------------------------|
|1|0x00002050|IL<br /><br /> Gerenciados|Público<br /><br /> ReuseSlot<br /><br /> SpecialName<br /><br /> RTSpecialName<br /><br /> .ctor|.ctor (construtor)||
|2|0x00002058|IL<br /><br /> Gerenciados|Público<br /><br /> Estático<br /><br /> ReuseSlot|Principal|String|
|3|0x0000208c|IL<br /><br /> Gerenciados|Público<br /><br /> Estático<br /><br /> ReuseSlot|Adicionar|int, int, int|

Cada coluna da tabela contém informações importantes sobre seu código. A coluna **RVA** permite que o runtime calcule o endereço de memória inicial do MSIL que define esse método. As colunas **ImplFlags** e **Flags** contêm bitmasks que descrevem o método (por exemplo, se o método é público ou particular). A coluna **Nome** indexa o nome do método com base no heap da cadeia de caracteres. A coluna **Assinatura** indexa a definição da assinatura do método no heap de blob.

O runtime calcula o endereço de deslocamento desejado com base na coluna **RVA** na terceira linha e retorna esse endereço para o compilador JIT, que depois passa para o novo endereço. O compilador JIT continua processando o MSIL no novo endereço até encontrar outro token de metadados, e o processo é repetido.

Usando metadados, o ambiente de runtime tem acesso a todas as informações necessárias para carregar seu código e processá-lo em instruções de máquina nativas. Dessa maneira, os metadados permitem arquivos autodescritivos e, com o CTS, a herança entre linguagens.

## <a name="related-topics"></a>Tópicos relacionados

|Title|Descrição|
|-----------|-----------------|
|[Atributos](attributes/index.md)|Descreve como aplicar atributos, escrever atributos personalizados e recuperar informações armazenadas em atributos.|
