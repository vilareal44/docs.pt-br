---
title: Escrevendo aplicativos .NET Framework grandes e dinâmicos
description: Grave aplicativos .NET responsivos ou grandes, ou aplicativos que processam uma grande quantidade de dados, como arquivos ou bancos de dado.
ms.date: 03/30/2017
ms.assetid: 123457ac-4223-4273-bb58-3bc0e4957e9d
author: BillWagner
ms.author: wiwagn
ms.openlocfilehash: 4a9f5d50ad78b2b0bef0ece3c4fce47d2925aca5
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063751"
---
# <a name="writing-large-responsive-net-framework-apps"></a>Escrevendo aplicativos .NET Framework grandes e dinâmicos

Este artigo apresenta dicas para melhorar o desempenho de grandes aplicativos do .NET Framework ou aplicativos que processam um grande volume de dados, como arquivos ou bancos de dados. Essas dicas vêm da nova gravação de compiladores do C# e do Visual Basic em código gerenciado, e este artigo inclui diversos exemplos reais do compilador do C#.
  
O .NET Framework é altamente produtivo para compilar aplicativos. Linguagens eficientes e seguras e uma coleção sofisticada de bibliotecas tornam a compilação de aplicativos altamente produtiva. Porém, grande produtividade traz muita responsabilidade. Você deve usar toda a potência do .NET Framework, mas esteja preparado para ajustar o desempenho do código quando necessário.
  
## <a name="why-the-new-compiler-performance-applies-to-your-app"></a>Por que o desempenho do novo compilador se aplica ao seu aplicativo  
 A equipe do .NET Compiler Platform ("Roslyn") reescreveu os compiladores do C# e do Visual Basic em código gerenciado para fornecer novas APIs para modelar e analisar códigos, compilar ferramentas e possibilitar experiências de código mais sofisticadas no Visual Studio. A nova gravação de compiladores e a criação de experiências do Visual Studio para os novos compiladores revelaram informações úteis sobre o desempenho, aplicáveis a qualquer aplicativo grande do .NET Framework ou qualquer aplicativo que processe muitos dados. Não é preciso ter conhecimento de compiladores para aproveitar as informações e os exemplos do compilador do C#.
  
 O Visual Studio usa as APIs do compilador para compilar todos os recursos do IntelliSense adorados pelos usuários, como colorização de identificadores e palavras-chave, listas de conclusão de sintaxe, soluções para erros, dicas de parâmetro, problemas de código e ações de código. O Visual Studio oferece essa ajuda enquanto os desenvolvedores estão digitando e alterando seus códigos e ele deve continuar respondendo enquanto o compilador modela continuamente a edição do desenvolvedor do código.
  
 Ao interagir com o aplicativo, os usuários finais esperam que ele seja ágil na resposta. A digitação ou a manipulação de comandos jamais deve ser bloqueada. A Ajuda deverá ser exibida rapidamente ou fechada se o usuário continuar digitando. O aplicativo deve evitar o bloqueio do thread da interface do usuário com computações longas, que aparentemente deixam o aplicativo lento.
  
 Para obter mais informações sobre compiladores do Roslyn, consulte [o SDK do .net Compiler Platform](../../csharp/roslyn-sdk/index.md).
  
## <a name="just-the-facts"></a>Aos fatos  
 Considere estes fatos ao ajustar o desempenho e criar aplicativos do .NET Framework ágeis na resposta.
  
### <a name="fact-1-dont-prematurely-optimize"></a>Fato 1: não otimize de forma prematura  
 Gravar um código mais complexo do que o necessário acarreta custos de manutenção, depuração e acabamento. Os programadores experientes têm uma compreensão intuitiva de como resolver problemas de codificação e gravar um código mais eficiente. Porém, às vezes, eles otimizam o código antes. Por exemplo, eles usam uma tabela hash quando uma simples matriz bastaria ou usam um cache complicado que pode causar perda de memória, em vez de simplesmente recalcular os valores. Mesmo que não seja um programador experiente, você deve testar o desempenho e analisar o código quando encontrar problemas.
  
### <a name="fact-2-if-youre-not-measuring-youre-guessing"></a>Fato 2: se você não está medindo, está adivinhando  
 Os perfis e as medidas não mentem. Os perfis mostram se a CPU está totalmente carregada ou se há um bloqueio na E/S do disco. Os perfis informam o tipo e a quantidade de memória que está sendo alocada e se a CPU está gastando muito tempo no [GC](../../standard/garbage-collection/index.md) (coleta de lixo).
  
 Estabeleça metas de desempenho para experiências ou cenários importantes do cliente no aplicativo e gravar testes para avaliar o desempenho. Investigue testes com falha aplicando o método científico: use perfis para orientá-lo, crie hipóteses sobre qual seria o problema e teste as hipóteses com um experimento ou uma alteração feita no código. Estabeleça medidas de desempenho de linha de base com o passar do tempo, usando testes regulares para que seja possível isolar as alterações que causam regressões no desempenho. Abordando o trabalho de desempenho de maneira rigorosa, você evitará a perda de tempo com atualizações desnecessárias de código.
  
### <a name="fact-3-good-tools-make-all-the-difference"></a>Fato 3: boas ferramentas fazem toda a diferença  
 As boas ferramentas permitem chegar rapidamente aos maiores problemas de desempenho (CPU, memória ou disco) e ajudam a alocar o código que causa esses gargalos. A Microsoft fornece uma variedade de ferramentas de desempenho, como o [Visual Studio Profiler](/visualstudio/profiling/beginners-guide-to-performance-profiling) e o [Perfview](https://www.microsoft.com/download/details.aspx?id=28567).
  
 PerfView é uma ferramenta gratuita e incrivelmente eficiente que ajuda você a se concentrar em problemas intensos, como E/S de disco, eventos de GC e memória. Capture eventos [ETW](../wcf/samples/etw-tracing.md) (Rastreamento de Eventos para Windows) relacionados ao desempenho e exiba informações por aplicativo, processo, pilha e thread com facilidade. O PerfView mostra quanto e que tipo de memória o aplicativo aloca, além de quais funções ou pilhas de chamadas contribuem para a quantidade de alocações da memória. Para obter detalhes, consulte os tópicos avançados da ajuda, as demonstrações e os vídeos incluídos com a ferramenta (como os [tutoriais do PerfView](https://channel9.msdn.com/Series/PerfView-Tutorial) no Channel 9).
  
### <a name="fact-4-its-all-about-allocations"></a>Fato 4: é tudo uma questão de alocação  
 Convém pensar que compilar um aplicativo do .NET Framework ágil na resposta é uma questão de algoritmos, como usar a classificação rápida em vez da classificação de bolhas, mas não é esse o caso. O maior fator na compilação de um aplicativo ágil na resposta é alocar memória, especialmente quando o aplicativo é muito grande ou processa grandes volumes de dados.
  
 Praticamente todo o trabalho de compilação de experiências IDE ágeis na resposta com as APIs do novo compilador envolveu evitar alocações e gerenciar estratégias de cache. Os rastreamentos do PerfView mostram que o desempenho dos novos compiladores do C# e do Visual Basic raramente está associado à CPU. Os compiladores podem estar associados à E/S na leitura de milhares ou milhões de linhas de código, de metadados ou na emissão de código gerenciado. Os atrasos do thread da interface do usuário são praticamente todos por conta da coleta de lixo. A GC do .NET Framework está totalmente ajustada para o desempenho e faz boa parte de seu trabalho junto com a execução do código do aplicativo. Porém, uma única alocação pode disparar uma coleta [gen2](../../standard/garbage-collection/fundamentals.md) cara, interrompendo todos os threads.
  
## <a name="common-allocations-and-examples"></a>Alocações e exemplos comuns  
 As expressões de exemplo nesta seção têm alocações ocultas aparentemente pequenas. Porém, se um aplicativo grande executar as expressões o número de vezes suficiente, elas poderão causar centenas de megabytes, até mesmo gigabytes, de alocações. Por exemplo, testes de um minuto que simulavam a digitação de um desenvolvedor no editor alocaram gigabytes de memória e permitiram que a equipe de desenvolvimento se concentrasse nos cenários de digitação.
  
### <a name="boxing"></a>Conversão boxing  
 A [conversão boxing](../../csharp/programming-guide/types/boxing-and-unboxing.md) ocorre quando tipos de valor, que normalmente residem na pilha ou nas estruturas de dados, são encapsulados em um objeto. Ou seja, você aloca um objeto para manter os dados e retorna um ponteiro para o objeto. Às vezes, o .NET Framework realiza a conversão boxing de valores por conta da assinatura de um método ou do tipo de local de armazenamento. A disposição de um tipo de valor em um objeto causa alocação da memória. Muitas operações de conversão boxing podem proporcionar megabytes ou gigabytes de alocações no aplicativo, o que significa que o aplicativo causará mais GCs. O .NET Framework e os compiladores de linguagem evitam a conversão boxing sempre que possível, mas às vezes ela acontece quando você menos espera.
  
 Para ver a conversão boxing no PerfView, abra um rastreamento e observe GC Heap Alloc Stacks abaixo do nome de processo do aplicativo (lembre-se de que o PerfView relata todos os processos). Caso veja tipos como <xref:System.Int32?displayProperty=nameWithType> e <xref:System.Char?displayProperty=nameWithType> sob as alocações, você está realizando a conversão boxing dos tipos de valor. A escolha de um desses tipos mostrará as pilhas e as funções nas quais a conversão boxing é realizada.
  
 **Exemplo 1: métodos de cadeia de caracteres e argumentos de tipo de valor**  
  
 Este código de exemplo ilustra a conversão boxing excessiva e possivelmente desnecessária:  
  
```csharp  
public class Logger  
{  
    public static void WriteLine(string s) { /*...*/ }  
}  
  
public class BoxingExample  
{  
    public void Log(int id, int size)  
    {  
        var s = string.Format("{0}:{1}", id, size);  
        Logger.WriteLine(s);  
    }  
}  
```  
  
 Como esse código oferece funcionalidade de registro em log, um aplicativo pode chamar a função `Log` com frequência, talvez milhões de vezes. O problema é que a chamada para o `string.Format` é resolvida para a sobrecarga <xref:System.String.Format%28System.String%2CSystem.Object%2CSystem.Object%29>.
  
 Essa sobrecarga exige que o .NET Framework realize a conversão boxing dos valores `int` em objetos a fim de passá-los para essa chamada de método. Uma correção parcial é chamar `id.ToString()` e `size.ToString()` e passar todas as cadeias de caracteres (que são objetos) para a chamada `string.Format`. A chamada de `ToString()` não aloca uma cadeia de caracteres, mas essa alocação ainda acontecerá em `string.Format`.
  
 Talvez você ache que essa chamada básica para `string.Format` seja apenas uma concatenação da cadeia de caracteres, dessa forma, será possível gravar esse código em seu lugar:  
  
```csharp  
var s = id.ToString() + ':' + size.ToString();  
```  
  
 Porém, essa linha de código introduz uma alocação de conversão boxing porque ela é compilada para <xref:System.String.Concat%28System.Object%2CSystem.Object%2CSystem.Object%29>. O .NET Framework deve realizar a conversão boxing do literal de caractere para invocar `Concat`  
  
 **Correção para o exemplo 1**  
  
 A correção completa é simples. Basta substituir o literal de caractere por um literal da cadeia de caracteres, que não acarreta conversão boxing porque as cadeias de caracteres já são objetos:  
  
```csharp  
var s = id.ToString() + ":" + size.ToString();  
```  
  
 **Exemplo 2: conversão boxing de enum**  
  
 Esse exemplo foi responsável por um grande volume de alocação nos novos compiladores do C# e do Visual Basic por conta do uso frequente de tipos de enumeração, especialmente em operações de pesquisa de dicionário.
  
```csharp  
public enum Color  
{  
    Red, Green, Blue  
}  
  
public class BoxingExample  
{  
    private string name;  
    private Color color;  
    public override int GetHashCode()  
    {  
        return name.GetHashCode() ^ color.GetHashCode();  
    }  
}  
```  
  
 Esse problema é muito sutil. O PerfView relataria isso como conversão boxing de <xref:System.Enum.GetHashCode> porque o método realiza a conversão boxing da representação subjacente do tipo de enumeração por motivos de implementação. Se observar atentamente o PerfView, você poderá ver duas alocações de conversão boxing para cada chamada para <xref:System.Enum.GetHashCode>. O compilador insere uma e o .NET Framework insere a outra.
  
 **Correção para o exemplo 2**  
  
 É possível evitar facilmente as duas alocações com a transmissão da representação subjacente antes de chamar <xref:System.Enum.GetHashCode>:  
  
```csharp  
((int)color).GetHashCode()  
```  
  
 Outra fonte comum de conversão boxing em tipos de enumeração é o método <xref:System.Enum.HasFlag%28System.Enum%29?displayProperty=nameWithType>. O argumento passado para <xref:System.Enum.HasFlag%28System.Enum%29> precisa passar pela conversão boxing. Na maioria dos casos, a substituição das chamadas <xref:System.Enum.HasFlag%28System.Enum%29?displayProperty=nameWithType> por um teste bit a bit é mais simples e sem alocação.
  
 Mantenha o fator do primeiro desempenho em mente (ou seja, não otimize antes) e não comece a gravar novamente todo o código dessa forma. Esteja atento aos custos da conversão boxing, mas só altere o código depois de criar o perfil do aplicativo e encontrar os pontos de acesso.
  
### <a name="strings"></a>Cadeias de caracteres  
 As manipulações da cadeia de caracteres são as maiores responsáveis pelas alocações e costumam aparecer no PerfView entre as cinco primeiras alocações. Os programas usam cadeias de caracteres na serialização, em JSON e nas APIs REST. É possível usar cadeias de caracteres como constantes programáticas na interoperação com sistemas quando não é possível usar tipos de enumeração. Quando a criação de perfis mostrar que as cadeias de caracteres estão afetando muito o desempenho, procure chamadas para métodos <xref:System.String> como <xref:System.String.Format%2A>, <xref:System.String.Concat%2A>, <xref:System.String.Split%2A>, <xref:System.String.Join%2A>, <xref:System.String.Substring%2A> e assim por diante. O uso de <xref:System.Text.StringBuilder> para evitar o custo com a criação de uma cadeia de caracteres com base em muitas peças ajuda, mas mesmo a alocação do objeto <xref:System.Text.StringBuilder> pode se tornar um gargalo que precisa ser gerenciado.
  
 **Exemplo 3: operações da cadeia de caracteres**  
  
 O compilador do C# tinha este código que grava o texto de um comentário em documento formatado em XML:  
  
```csharp  
public void WriteFormattedDocComment(string text)  
{  
    string[] lines = text.Split(new[] { "\r\n", "\r", "\n" },  
                                StringSplitOptions.None);  
    int numLines = lines.Length;  
    bool skipSpace = true;  
    if (lines[0].TrimStart().StartsWith("///"))  
    {  
        for (int i = 0; i < numLines; i++)  
        {  
            string trimmed = lines[i].TrimStart();  
            if (trimmed.Length < 4 || !char.IsWhiteSpace(trimmed[3]))  
            {  
                skipSpace = false;  
                break;  
            }  
        }  
        int substringStart = skipSpace ? 4 : 3;  
        for (int i = 0; i < numLines; i++)  
            WriteLine(lines[i].TrimStart().Substring(substringStart));  
    }  
    else { /* ... */ }  
```  
  
 É possível ver que esse código faz muita manipulação de cadeia de caracteres. O código usa métodos de biblioteca para dividir linhas em cadeias de caracteres separadas, cortar espaços em branco, verificar se o argumento `text` é um comentário de documentação em XML e extrair subcadeias de caracteres das linhas.
  
 Na primeira linha do `WriteFormattedDocComment`, sempre que ocorre a chamada `text.Split`, ela aloca uma nova matriz de três elementos como o argumento. O compilador sempre precisa emitir o código para alocar essa matriz. Isso porque o compilador não sabe se <xref:System.String.Split%2A> armazena a matriz em um lugar onde a matriz pode ser modificada por outro código, o que afetaria chamadas posteriores para `WriteFormattedDocComment`. A chamada para <xref:System.String.Split%2A> também aloca uma cadeia de caracteres para cada linha em `text` e aloca outra memória para realizar a operação.
  
 `WriteFormattedDocComment` tem três chamadas ao método <xref:System.String.TrimStart%2A>. Duas estão em loops internos que duplicam o trabalho e as alocações. Para piorar as coisas, a chamada do método <xref:System.String.TrimStart%2A> sem argumentos aloca uma matriz vazia (para o parâmetro `params`) além do resultado da cadeia de caracteres.
  
 Por fim, existe uma chamada para o método <xref:System.String.Substring%2A>, que normalmente aloca uma nova cadeia de caracteres.
  
 **Correção para o exemplo 3**  
  
 Diferentemente dos exemplos anteriores, pequenas edições não podem corrigir essas alocações. É preciso voltar, observar o problema e abordá-lo de maneira diferente. Por exemplo, você verá que o argumento para `WriteFormattedDocComment()` é uma cadeia de caracteres com todas as informações de que o método precisa, para que o código pudesse fazer mais indexação em vez de alocar muitas cadeias de caracteres parciais.
  
 A equipe de desempenho do compilador resolveu todas essas alocações com códigos como este:  
  
```csharp  
private int IndexOfFirstNonWhiteSpaceChar(string text, int start) {  
    while (start < text.Length && char.IsWhiteSpace(text[start])) start++;  
    return start;  
}  
  
private bool TrimmedStringStartsWith(string text, int start, string prefix) {  
    start = IndexOfFirstNonWhiteSpaceChar(text, start);  
    int len = text.Length - start;  
    if (len < prefix.Length) return false;  
    for (int i = 0; i < len; i++)  
    {  
        if (prefix[i] != text[start + i]) return false;  
    }  
    return true;  
}  
  
// etc...
```  
  
 A primeira versão de `WriteFormattedDocComment()` alocava uma matriz, diversas subcadeias de caracteres e uma subcadeia de caracteres cortada com uma matriz `params` vazia. Ele também verificou "///". O código revisado usa apenas a indexação e não aloca nada. Ele localiza o primeiro caractere que não é espaço em branco e, em seguida, verifica o caractere por caractere para ver se a cadeia de caracteres começa com "///". O novo código usa `IndexOfFirstNonWhiteSpaceChar` em vez de <xref:System.String.TrimStart%2A> para retornar o primeiro índice (após um índice inicial especificado) em que um caractere que não seja espaço em branco ocorre. A correção não está completa, mas é possível ver como aplicar correções semelhantes para uma solução completa. Aplicando essa abordagem em todo o código, é possível remover todas as alocações em `WriteFormattedDocComment()`.
  
 **Exemplo 4: StringBuilder**  
  
 Este exemplo usa um objeto <xref:System.Text.StringBuilder>. A função a seguir gera um nome de tipo completo para tipos genéricos:  
  
```csharp  
public class Example  
{  
    // Constructs a name like "SomeType<T1, T2, T3>"  
    public string GenerateFullTypeName(string name, int arity)  
    {  
        StringBuilder sb = new StringBuilder();  
  
        sb.Append(name);  
        if (arity != 0)  
        {  
            sb.Append("<");  
            for (int i = 1; i < arity; i++)  
            {  
                sb.Append("T"); sb.Append(i.ToString()); sb.Append(", ");  
            }  
            sb.Append("T"); sb.Append(i.ToString()); sb.Append(">");  
        }  
  
        return sb.ToString();  
    }  
}  
```  
  
 O foco está na linha que cria uma nova instância <xref:System.Text.StringBuilder>. O código causa uma alocação para `sb.ToString()` e alocações internas dentro da implementação <xref:System.Text.StringBuilder>, mas não será possível controlar essas alocações se você quiser o resultado da cadeia de caracteres.
  
 **Correção para o exemplo 4**  
  
 Para corrigir a alocação do objeto `StringBuilder`, armazene o objeto em cache. Mesmo o armazenamento em cache de uma única instância que pode ser descartada pode melhorar significativamente o desempenho. Essa é a nova implementação da função, omitindo todo o código, exceto as primeiras e as últimas linhas novas:  
  
```csharp  
// Constructs a name like "MyType<T1, T2, T3>"  
public string GenerateFullTypeName(string name, int arity)  
{  
    StringBuilder sb = AcquireBuilder();  
    /* Use sb as before */  
    return GetStringAndReleaseBuilder(sb);  
}  
```  
  
 As partes-chave são as novas funções `AcquireBuilder()` e `GetStringAndReleaseBuilder()`:  
  
```csharp  
[ThreadStatic]  
private static StringBuilder cachedStringBuilder;  
  
private static StringBuilder AcquireBuilder()  
{  
    StringBuilder result = cachedStringBuilder;  
    if (result == null)  
    {  
        return new StringBuilder();  
    }  
    result.Clear();  
    cachedStringBuilder = null;  
    return result;  
}  
  
private static string GetStringAndReleaseBuilder(StringBuilder sb)  
{  
    string result = sb.ToString();  
    cachedStringBuilder = sb;  
    return result;  
}  
```  
  
 Como os novos compiladores usam o threading, essas implementações usam um campo com thread estático (atributo <xref:System.ThreadStaticAttribute>) para armazenar em cache o <xref:System.Text.StringBuilder> e você certamente pode esquecer a declaração `ThreadStatic`. O campo com thread estático mantém um valor exclusivo para cada thread que executa esse código.
  
 `AcquireBuilder()` retornará a instância de <xref:System.Text.StringBuilder> armazenada em cache se houver uma, depois de limpá-la e definir o campo ou o cache como nulo. Do contrário, `AcquireBuilder()` cria uma nova instância e a retorna, deixando o campo ou o cache definido com nulo.
  
 Quando concluir o <xref:System.Text.StringBuilder>, você poderá chamar `GetStringAndReleaseBuilder()` para obter o resultado da cadeia de caracteres, salvar a instância de <xref:System.Text.StringBuilder> no campo ou no cache e retornar o resultado. Para a execução, é possível reinserir esse código e criar vários objetos <xref:System.Text.StringBuilder> (embora isso raramente aconteça). O código salva apenas a última instância de <xref:System.Text.StringBuilder> lançada para uso posterior. Essa estratégia de cache simples reduziu significativamente as alocações nos novos compiladores. Partes do .NET Framework e do MSBuild (“MSBuild”) usam uma técnica semelhante para melhorar o desempenho.
  
 Essa estratégia de cache simples respeita o bom design de cache porque tem um limite de tamanho. Porém, há mais código agora do que havia originalmente, o que significa mais custos com manutenção. Você só deverá adotar a estratégia de cache se tiver encontrado um problema de desempenho e o PerfView tiver mostrado que as alocações de <xref:System.Text.StringBuilder> são um fator significativo.
  
### <a name="linq-and-lambdas"></a>LINQ e lambdas  
A consulta integrada à linguagem (LINQ), em conjunto com expressões lambda, é um exemplo de um recurso de produtividade. No entanto, seu uso pode ter um impacto significativo no desempenho ao longo do tempo e talvez você ache necessário reescrever seu código.
  
 **Exemplo 5: lambdas, lista \<T> e IEnumerable\<T>**  
  
 Esse exemplo usa [o LINQ e um código de estilo funcional](https://docs.microsoft.com/archive/blogs/charlie/anders-hejlsberg-on-linq-and-functional-programming) para localizar um símbolo no modelo do compilador, considerando uma cadeia de caracteres de nome:  
  
```csharp  
class Symbol {  
    public string Name { get; private set; }  
    /*...*/  
}  
  
class Compiler {  
    private List<Symbol> symbols;  
    public Symbol FindMatchingSymbol(string name)  
    {  
        return symbols.FirstOrDefault(s => s.Name == name);  
    }  
}  
```  
  
 O novo compilador e as experiências de IDE com base nele chamam `FindMatchingSymbol()` com muita frequência, além de haver diversas alocações ocultas nessa única linha de código da função. Para examinar essas alocações, primeiro divida a única linha de código da função em duas linhas:  
  
```csharp  
Func<Symbol, bool> predicate = s => s.Name == name;  
     return symbols.FirstOrDefault(predicate);  
```  
  
 Na primeira linha, a [expressão lambda](../../csharp/language-reference/operators/lambda-expressions.md) `s => s.Name == name` [fecha](https://docs.microsoft.com/archive/blogs/ericlippert/what-are-closures) a variável local `name`. Isso significa que, além de alocar um objeto para o [representante](../../csharp/language-reference/builtin-types/reference-types.md#the-delegate-type) que `predicate` mantém, o código aloca uma classe estática para manter o ambiente que captura o valor `name`. O compilador gera um código semelhante ao seguinte:  
  
```csharp  
// Compiler-generated class to hold environment state for lambda  
private class Lambda1Environment  
{  
    public string capturedName;  
    public bool Evaluate(Symbol s)  
    {  
        return s.Name == this.capturedName;  
    }  
}  
  
// Expanded Func<Symbol, bool> predicate = s => s.Name == name;  
Lambda1Environment l = new Lambda1Environment() { capturedName = name };  
var predicate = new Func<Symbol, bool>(l.Evaluate);  
```  
  
 Agora, as duas alocações `new` (uma para a classe de ambiente e uma para o representante) estão explícitas.
  
 Agora observe a chamada para `FirstOrDefault`. Esse método de extensão no tipo <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> também acarreta uma alocação. Como `FirstOrDefault` utiliza um objeto <xref:System.Collections.Generic.IEnumerable%601> como seu primeiro argumento, é possível expandir a chamada para o seguinte código (um pouco simplificado para discussão):  
  
```csharp  
// Expanded return symbols.FirstOrDefault(predicate) ...
     IEnumerable<Symbol> enumerable = symbols;  
     IEnumerator<Symbol> enumerator = enumerable.GetEnumerator();  
     while(enumerator.MoveNext())  
     {  
         if (predicate(enumerator.Current))  
             return enumerator.Current;  
     }  
     return default(Symbol);  
```  
  
 A variável `symbols` tem o tipo <xref:System.Collections.Generic.List%601>. O tipo de coleção <xref:System.Collections.Generic.List%601> implementa <xref:System.Collections.Generic.IEnumerable%601> e define de maneira inteligente um enumerador (interface <xref:System.Collections.Generic.IEnumerator%601>) que <xref:System.Collections.Generic.List%601> implementa com um `struct`. O uso de uma estrutura em vez de uma classe significa que você normalmente evita alocações de heap, o que, por sua vez, pode afetar o desempenho da coleta de lixo. Os enumeradores costumam ser usados com o loop `foreach` da linguagem, que usa a estrutura do enumerador conforme ela retorna à pilha de chamadas. O incremento do ponteiro do heap de chamadas para abrir espaço a um objeto não afeta a GC, como faz uma alocação de heap.
  
 No caso da chamada `FirstOrDefault` expandida, o código precisa chamar `GetEnumerator()` em um <xref:System.Collections.Generic.IEnumerable%601>. A atribuição de `symbols` à variável `enumerable` do tipo `IEnumerable<Symbol>` perde as informações de que o objeto real é um <xref:System.Collections.Generic.List%601>. Isso significa que, quando o código busca o enumerador com `enumerable.GetEnumerator()`, o .NET Framework precisa realizar a conversão boxing da estrutura retornada para atribuí-la à variável `enumerator`.
  
 **Correção para o exemplo 5**  
  
 A correção é para gravar novamente `FindMatchingSymbol` da seguinte forma, substituindo sua linha de código única por seis linhas de código que continuam concisas, fáceis de ler, compreender e manter:  
  
```csharp  
public Symbol FindMatchingSymbol(string name)  
    {  
        foreach (Symbol s in symbols)  
        {  
            if (s.Name == name)  
                return s;  
        }  
        return null;  
    }  
```  
  
 Esse código não usa métodos de extensão LINQ, lambdas ou enumeradores, e não acarreta alocações. Não há alocações porque o compilador pode ver que a coleção `symbols` é um <xref:System.Collections.Generic.List%601> e pode associar o enumerador resultante (uma estrutura) a uma variável local com o tipo certo para evitar a conversão boxing. A versão original dessa função era um ótimo exemplo da potência expressiva do C# e da produtividade do .NET Framework. Essa versão nova e mais eficiente preserva essas qualidades sem adicionar nenhum código complexo de manutenção.
  
### <a name="async-method-caching"></a>Cache de método assíncrono  

O próximo exemplo mostra um problema comum quando você tenta usar os resultados armazenados em cache em um método [async](../../csharp/programming-guide/concepts/async/index.md).
  
 **Exemplo 6: cache em métodos assíncronos**  
  
 Os recursos de IDE do Visual Studio internos nos novos compiladores do C# e do Visual Basic normalmente buscam árvores de sintaxe, e os compiladores usam métodos assíncronos para isso, a fim de manter o Visual Studio ágil na resposta. Aqui está a primeira versão do código que pode ser gravado para obter uma árvore de sintaxe:  
  
```csharp  
class SyntaxTree { /*...*/ }  
  
class Parser { /*...*/  
    public SyntaxTree Syntax { get; }  
    public Task ParseSourceCode() { /*...*/ }  
}  
  
class Compilation { /*...*/  
    public async Task<SyntaxTree> GetSyntaxTreeAsync()  
    {  
        var parser = new Parser(); // allocation  
        await parser.ParseSourceCode(); // expensive  
        return parser.Syntax;  
    }  
}  
```  
  
 É possível ver que a chamada de `GetSyntaxTreeAsync()` cria uma instância de `Parser`, analisa o código e retorna um objeto <xref:System.Threading.Tasks.Task>, `Task<SyntaxTree>`. A parte adversa é alocar a instância de `Parser` e analisar o código. A função retorna um <xref:System.Threading.Tasks.Task>, para que os chamadores possam aguardar o trabalho de análise e liberar o thread da interface do usuário para responder à entrada do usuário.
  
 Como diversos recursos do Visual Studio podem tentar obter a mesma árvore de sintaxe, convém gravar o código a seguir para armazenar em cache o resultado da análise a fim de economizar tempo e alocações. Porém, esse código acarreta uma alocação:  
  
```csharp  
class Compilation { /*...*/  
  
    private SyntaxTree cachedResult;  
  
    public async Task<SyntaxTree> GetSyntaxTreeAsync()  
    {  
        if (this.cachedResult == null)  
        {  
            var parser = new Parser(); // allocation  
            await parser.ParseSourceCode(); // expensive  
            this.cachedResult = parser.Syntax;  
        }  
        return this.cachedResult;  
    }  
}  
```  
  
 Você vê que o novo código com cache tem um arquivo `SyntaxTree` chamado `cachedResult`. Quando esse campo é nulo, `GetSyntaxTreeAsync()` faz o trabalho e salva o resultado no cache. `GetSyntaxTreeAsync()` retorna o objeto `SyntaxTree`. O problema é que quando você tem uma função `async` do tipo `Task<SyntaxTree>` e retorna um valor do tipo `SyntaxTree`, o compilador emite um código para alocar uma Tarefa e manter o resultado (usando `Task<SyntaxTree>.FromResult()`). A Tarefa é marcada como concluída, e o resultado é disponibilizado imediatamente. No código dos novos compiladores, os objetos <xref:System.Threading.Tasks.Task> que já estavam concluídos ocorriam com tanta frequência que a correção dessas alocações melhorou claramente a capacidade de resposta.
  
 **Correção para o exemplo 6**  
  
 Para remover a <xref:System.Threading.Tasks.Task> alocação concluída, você pode armazenar em cache o objeto de tarefa com o resultado concluído:  
  
```csharp  
class Compilation { /*...*/  
  
    private Task<SyntaxTree> cachedResult;  
  
    public Task<SyntaxTree> GetSyntaxTreeAsync()  
    {  
        return this.cachedResult ??
               (this.cachedResult = GetSyntaxTreeUncachedAsync());  
    }  
  
    private async Task<SyntaxTree> GetSyntaxTreeUncachedAsync()  
    {  
        var parser = new Parser(); // allocation  
        await parser.ParseSourceCode(); // expensive  
        return parser.Syntax;  
    }  
}  
```  
  
 Esse código altera o tipo de `cachedResult` para `Task<SyntaxTree>` e emprega uma função auxiliar `async` que mantém o código original de `GetSyntaxTreeAsync()`. `GetSyntaxTreeAsync()` agora usa o [operador de união nula](../../csharp/language-reference/operators/null-coalescing-operator.md) para retornar `cachedResult`, caso ele não seja nulo. Se `cachedResult` for nulo, `GetSyntaxTreeAsync()` chamará `GetSyntaxTreeUncachedAsync()` e armazenará o resultado em cache. `GetSyntaxTreeAsync()` não aguarda a chamada para `GetSyntaxTreeUncachedAsync()` como o código faria normalmente. Não usar a espera significa que, quando `GetSyntaxTreeUncachedAsync()` retorna seu objeto <xref:System.Threading.Tasks.Task>, `GetSyntaxTreeAsync()` retorna imediatamente o <xref:System.Threading.Tasks.Task>. Agora, o resultado armazenado em cache é um <xref:System.Threading.Tasks.Task>. Assim, não há alocações para retornar o resultado armazenado em cache.
  
### <a name="additional-considerations"></a>Considerações adicionais  
 Aqui estão mais alguns pontos sobre possíveis problemas em aplicativos grandes ou em aplicativos que processam muitos dados.
  
 **OldValues**  
  
 Os dicionários são amplamente usados em muitos programas, e bons dicionários são muito práticos e naturalmente eficientes. Porém, eles costumam ser usados incorretamente. No Visual Studio e nos novos compiladores, a análise mostra que muitos dos dicionários apresentavam um único elemento ou estavam vazios. Um <xref:System.Collections.Generic.Dictionary%602> vazio tem dez campos e ocupa 48 bytes no heap em um computador x86. Os dicionários são ótimos quando você precisa de um mapeamento ou de uma estrutura de dados associativa com pesquisa constante. No entanto, quando tem apenas alguns elementos, você perde muito espaço usando um dicionário. Em vez disso, por exemplo, você pode analisar iterativamente um `List<KeyValuePair\<K,V>>` com a mesma rapidez. Se você usa um dicionário apenas para carregá-lo com dados e, em seguida, lê-lo (um padrão muito comum), o uso de uma matriz classificada com uma pesquisa N(log(N)) pode ter praticamente a mesma velocidade, dependendo do número de elementos que você estiver usando.
  
 **Classes vs. estruturas**  
  
 De certa forma, classes e estruturas oferecem uma contrapartida de espaço/tempo clássica para ajustar os aplicativos. As classes acarretam 12 bytes de sobrecarga em um computador x86, mesmo que não tenham campos, mas são fáceis de passar porque só utilizam um ponteiro para consultar uma instância de classe. As estruturas não acarretam alocações de heap em caso de não conversão boxing, mas, quando você passa estruturas grandes como argumentos de função ou valores de retorno, elas utilizam tempo de CPU para copiar atomicamente todos os membros de dados das estruturas. Cuidado com chamadas repetidas para propriedades que retornem estruturas, e armazene em cache o valor da propriedade em uma variável local para evitar a cópia excessiva de dados.
  
 **Caches**  
  
 Um truque de desempenho comum é armazenar resultados em cache. Porém, um cache sem um limite de tamanho ou uma política de alienação pode causar perda de memória. Ao processar grandes volumes de dados, se mantiver muita memória em caches, você poderá fazer a coleta de lixo substituir os benefícios das pesquisas armazenadas em cache.
  
 Neste artigo, abordamos como você deve dar atenção a sintomas de gargalo de desempenho que possam afetar a capacidade de resposta do aplicativo, especialmente para sistemas grandes ou sistemas que processem um grande volume de dados. Entre os responsáveis mais comuns estão conversão boxing, manipulações da cadeia de caracteres, LINQ e lambda, cache em métodos assíncronos, cache sem um limite de tamanho ou uma política de alienação, uso incorreto de dicionários e passagem de estruturas. Lembre-se dos quatro fatos para ajustar os aplicativos:  
  
- Não otimize antes – seja produtivo e ajuste o aplicativo quando identificar problemas.
  
- Os perfis não mentem – você está adivinhando se não está medindo.
  
- As boas ferramentas fazem toda a diferença – baixe o PerfView e faça um teste.
  
- É tudo uma questão de alocação – é onde a equipe da plataforma do compilador passa boa parte do tempo melhorando o desempenho dos novos compiladores.
  
## <a name="see-also"></a>Consulte também

- [Vídeo de apresentação deste tópico](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B333)
- [Guia do iniciante à criação de perfil do desempenho](/visualstudio/profiling/beginners-guide-to-performance-profiling)
- [Desempenho](index.md)
- [Dicas de desempenho do .NET](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973839(v%3dmsdn.10))
- [Tutoriais do PerfView no Channel 9](https://channel9.msdn.com/Series/PerfView-Tutorial)
- [O SDK do .NET Compiler Platform](../../csharp/roslyn-sdk/index.md)
- [repositório dotnet/Roslyn no GitHub](https://github.com/dotnet/roslyn)
