---
title: UriTemplate and UriTemplateTable
ms.date: 03/30/2017
ms.assetid: 5cbbe03f-4a9e-4d44-9e02-c5773239cf52
ms.openlocfilehash: 106ba21b58dabab96afbc8fb6db5cb305386f2fe
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595070"
---
# <a name="uritemplate-and-uritemplatetable"></a>UriTemplate and UriTemplateTable
Os desenvolvedores da Web exigem a capacidade de descrever a forma e o layout dos URIs aos quais seus serviços respondem. Windows Communication Foundation (WCF) adicionou duas novas classes para dar aos desenvolvedores controle sobre seus URIs. <xref:System.UriTemplate>e <xref:System.UriTemplateTable> forma a base do mecanismo de expedição baseado em URI no WCF. Essas classes também podem ser usadas por conta própria, permitindo que os desenvolvedores tirem proveito dos modelos e do mecanismo de mapeamento de URI sem implementar um serviço WCF.  
  
## <a name="templates"></a>Modelos  
 Um modelo é uma maneira de descrever um conjunto de URIs relativos. O conjunto de modelos de URI na tabela a seguir mostra como um sistema que recupera vários tipos de informações de clima pode ser definido.  
  
|Dados|Modelo|  
|----------|--------------|  
|Previsão nacional|clima/nacional|  
|Previsão de estado|clima/{estado}|  
|Previsão de cidade|clima/{estado}/{cidade}|  
|Previsão de atividade|clima/{estado}/{cidade}/{atividade}|  
  
 Esta tabela descreve um conjunto de URIs estruturalmente semelhantes. Cada entrada é um modelo de URI. Os segmentos entre chaves descrevem as variáveis. Os segmentos que não estão entre chaves descrevem cadeias de caracteres literais. As classes de modelo do WCF permitem que um desenvolvedor Use um URI de entrada, por exemplo, "/Weather/wa/Seattle/Cycling" e faça a correspondência com um modelo que o descreva, "/Weather/{State}/{City}/{Activity}".  
  
## <a name="uritemplate"></a>UriTemplate  
 <xref:System.UriTemplate>é uma classe que encapsula um modelo de URI. O construtor usa um parâmetro de cadeia de caracteres que define o modelo. Essa cadeia de caracteres contém o modelo no formato descrito na próxima seção. A <xref:System.UriTemplate> classe fornece métodos que permitem que você corresponda a um URI de entrada para um modelo, gere um URI a partir de um modelo, recupere uma coleção de nomes de variáveis usados no modelo, determine se dois modelos são equivalentes e retorna a cadeia de caracteres do modelo.  
  
 <xref:System.UriTemplate.Match%28System.Uri%2CSystem.Uri%29>usa um endereço base e um URI candidato e tenta corresponder o URI ao modelo. Se a correspondência for bem-sucedida, uma <xref:System.UriTemplateMatch> instância será retornada. O <xref:System.UriTemplateMatch> objeto contém um URI de base, o URI candidato, uma coleção de nome/valor dos parâmetros de consulta, uma matriz dos segmentos de caminho relativo, uma coleção de nome/valor de variáveis que foram correspondidas, a <xref:System.UriTemplate> instância usada para executar a correspondência, uma cadeia de caracteres que contém qualquer parte não correspondente do URI candidato (usada quando o modelo tem um curinga) e um objeto associado ao modelo.  
  
> [!NOTE]
> A <xref:System.UriTemplate> classe ignora o esquema e o número da porta ao corresponder um URI candidato a um modelo.  
  
 Há dois métodos que permitem gerar um URI a partir de um modelo <xref:System.UriTemplate.BindByName%28System.Uri%2CSystem.Collections.Specialized.NameValueCollection%29> e <xref:System.UriTemplate.BindByPosition%28System.Uri%2CSystem.String%5B%5D%29> . <xref:System.UriTemplate.BindByName%28System.Uri%2CSystem.Collections.Specialized.NameValueCollection%29>usa um endereço base e uma coleção de nome/valor de parâmetros. Esses parâmetros são substituídos por variáveis quando o modelo é associado. <xref:System.UriTemplate.BindByPosition%28System.Uri%2CSystem.String%5B%5D%29>usa os pares de nome/valor e os substitui da esquerda para a direita.  
  
 <xref:System.UriTemplate.ToString>Retorna a cadeia de caracteres do modelo.  
  
 A <xref:System.UriTemplate.PathSegmentVariableNames%2A> propriedade contém uma coleção dos nomes das variáveis usadas em segmentos de caminho na cadeia de caracteres do modelo.  
  
 <xref:System.UriTemplate.IsEquivalentTo%28System.UriTemplate%29>usa um <xref:System.UriTemplate> como parâmetro e retorna um valor booliano que especifica se os dois modelos são equivalentes. Para obter mais informações, consulte a seção equivalência de modelo mais adiante neste tópico.  
  
 <xref:System.UriTemplate>foi projetado para funcionar com qualquer esquema URI que esteja de acordo com a gramática de HTTP URI. Veja a seguir exemplos de esquemas de URI com suporte.  
  
- `http://`  
  
- `https://`  
  
- `net.tcp://`  
  
- `net.pipe://`  
  
- `sb://`  
  
 Esquemas como file://e urn://não estão em conformidade com a gramática de HTTP URI e causam resultados imprevisíveis quando usados com modelos de URI.  
  
### <a name="template-string-syntax"></a>Sintaxe de cadeia de caracteres de modelo  
 Um modelo tem três partes: um caminho, uma consulta opcional e um fragmento opcional. Para obter um exemplo, consulte o seguinte modelo:  
  
`"/weather/{state}/{city}?forecast={length)#frag1`  
  
 O caminho consiste em "/Weather/{State}/{City}", a consulta consiste em "? previsão = {Length} e o fragmento consiste em" #frag1 ".  
  
 As barras à esquerda e à direita são opcionais na expressão de caminho. Tanto as expressões de consulta quanto de fragmento podem ser omitidas inteiramente. Um caminho consiste em uma série de segmentos delimitados por '/', cada segmento pode ter um valor literal, um nome de variável (escrito em {keycolchetes}) ou um curinga (gravado como ' \* '). No modelo anterior, o "segmento \weather\ é um valor literal enquanto" {State} "e" {City} "são variáveis. As variáveis recebem seu nome do conteúdo de suas chaves e elas podem ser substituídas posteriormente por um valor concreto para criar um *URI fechado*. O curinga é opcional, mas só pode aparecer no final do URI, onde ele corresponde logicamente a "o restante do caminho".  
  
 A expressão de consulta, se presente, especifica uma série de pares de nome/valor não ordenados delimitadas por ' & '. Os elementos da expressão de consulta podem ser pares literais (x = 2) ou um par de variáveis (x = {var}). Somente o lado direito da consulta pode ter uma expressão variável. ({somename} = {someValue} não é permitido. Valores não emparelhados (? x) não são permitidos. Não há nenhuma diferença entre uma expressão de consulta vazia e uma expressão de consulta consistindo apenas em um único '? ' (ambos significam "qualquer consulta").  
  
 A expressão de fragmento pode consistir em um valor literal, nenhuma variável é permitida.  
  
 Todos os nomes de variáveis de modelo dentro de uma cadeia de caracteres de modelo devem ser exclusivos. Os nomes de variáveis de modelo não diferenciam maiúsculas de minúsculas.  
  
 Exemplos de cadeias de caracteres de modelo válidas:  
  
- ""  
  
- "/shoe"  
  
- "/Shoe/ \* "  
  
- "{sapato}/Boat"  
  
- "{sapato}/{Boat}/Bed/{Quilt}"  
  
- "sapato/{barco}"  
  
- "sapato/{barco}/ \* "  
  
- "sapato/barco? x = 2"  
  
- "sapato/{barco}? x = {cama}"  
  
- "sapato/{barco}? x = {cama} &y = banda"  
  
- "? x = {sapato}"  
  
- "sapato? x = 3&y = {var}  
  
 Exemplos de cadeias de caracteres de modelo inválidas:  
  
- "{sapato}/{SHOE}/x = 2" – nomes de variáveis duplicados.  
  
- "{sapato}/Boat/? cama = {sapato}" – nomes de variáveis duplicados.  
  
- "? x = 2&x = 3" – os pares de nome/valor dentro de uma cadeia de caracteres de consulta devem ser exclusivos, mesmo que sejam literais.  
  
- "? x = 2&" – a cadeia de caracteres de consulta está malformada.  
  
- "? 2&x = {sapato}" – a cadeia de caracteres de consulta deve ser pares de nome/valor.  
  
- "? y = 2&&X = 3" – a cadeia de caracteres de consulta deve ser pares de valor de nome, os nomes não podem começar com ' & '.  
  
### <a name="compound-path-segments"></a>Segmentos de caminho composto  
 Segmentos de caminho composto permitem que um único segmento de caminho de URI contenha várias variáveis, bem como variáveis combinadas com literais. Veja a seguir exemplos de segmentos de caminho composto válidos.  
  
- /filename. {ext}/  
  
- /{filename}.jpg/  
  
- /{filename}. {ext}/  
  
- um. {b} someLiteral {c} ({d})/  
  
 Veja a seguir exemplos de segmentos de caminho inválidos.  
  
- As {} variáveis/devem ser nomeadas.  
  
- /{Shoe}{Boat}-as variáveis devem ser separadas por um literal.  
  
### <a name="matching-and-compound-path-segments"></a>Correspondência e segmentos de caminho compostos  
 Segmentos de caminho composto permitem que você defina um UriTemplate que tenha várias variáveis dentro de um único segmento de caminho. Por exemplo, na seguinte cadeia de caracteres de modelo: "Addresses/{State}. {City} "duas variáveis (State e City) são definidas dentro do mesmo segmento. Esse modelo corresponderia a uma URL como `http://example.com/Washington.Redmond` , mas também corresponderá a uma URL como `http://example.com/Washington.Redmond.Microsoft` . No último caso, a variável de estado conterá "Washington" e a variável City conterá "Redmond. Microsoft". Nesse caso, qualquer texto (exceto '/') corresponderá à variável {City}. Se você quiser um modelo que não corresponda ao texto "extra", coloque a variável em um segmento de modelo separado, por exemplo: "Addresses/{State}/{City}.  
  
### <a name="named-wildcard-segments"></a>Segmentos curinga nomeados  
 Um segmento curinga nomeado é qualquer segmento de variável de caminho cujo nome da variável comece com o caractere curinga ' \* '. A cadeia de caracteres de modelo a seguir contém um segmento curinga nomeado chamado "sapato".  
  
`"literal/{*shoe}"`  
  
 Os segmentos curinga devem seguir as seguintes regras:  
  
- Pode haver no máximo um segmento curinga nomeado para cada cadeia de caracteres de modelo.  
  
- Um segmento curinga nomeado deve aparecer no segmento mais à direita no caminho.  
  
- Um segmento curinga nomeado não pode coexistir com um segmento curinga anônimo dentro da mesma cadeia de caracteres de modelo.  
  
- O nome de um segmento curinga nomeado deve ser exclusivo.  
  
- Segmentos curinga nomeados não podem ter valores padrão.  
  
- Segmentos curinga nomeados não podem terminar com "/".  
  
### <a name="default-variable-values"></a>Valores de variáveis padrão  
 Valores de variáveis padrão permitem que você especifique valores padrão para variáveis dentro de um modelo. As variáveis padrão podem ser especificadas com as chaves que declaram a variável ou como uma coleção passada para o construtor de UriTemplate. O modelo a seguir mostra duas maneiras de especificar um <xref:System.UriTemplate> com variáveis com valores padrão.  
  
```csharp
UriTemplate t = new UriTemplate("/test/{a=1}/{b=5}");  
```  
  
 Este modelo declara uma variável chamada `a` com um valor padrão de `1` e uma variável chamada `b` com um valor padrão de `5` .  
  
> [!NOTE]
> Somente variáveis de segmento de caminho têm permissão para ter valores padrão. Variáveis de cadeia de caracteres de consulta, variáveis de segmento compostas e variáveis curinga nomeadas não têm permissão para ter valores padrão.  
  
 O código a seguir mostra como os valores de variáveis padrão são tratados ao corresponder a um URI candidato.  
  
```csharp
Uri baseAddress = new Uri("http://localhost:8000/");

UriTemplate t = new UriTemplate("/{state=WA}/{city=Redmond}/", true);
Uri candidate = new Uri("http://localhost:8000/OR");

UriTemplateMatch m1 = t.Match(baseAddress, candidate);

Console.WriteLine($"Template: {t}");
Console.WriteLine($"Candidate URI: {candidate}");

// Display contents of BoundVariables
Console.WriteLine("BoundVariables:");
foreach (string key in m1.BoundVariables.AllKeys)
{
    Console.WriteLine($"\t{key}={m1.BoundVariables[key]}");
}
// The output of the above code is  
// Template: /{state=WA}/{city=Redmond}/
// Candidate URI: http://localhost:8000/OR
// BoundVariables:
//         STATE=OR
//         CITY=Redmond
```  
  
> [!NOTE]
> Um URI como não `http://localhost:8000///` corresponde ao modelo listado no código anterior, no entanto, um URI como `http://localhost:8000/` o faz.  
  
 O código a seguir mostra como os valores de variáveis padrão são tratados ao criar um URI com um modelo.  
  
```csharp
Uri baseAddress = new Uri("http://localhost:8000/");  
Dictionary<string,string> defVals = new Dictionary<string,string> {{"a","1"}, {"b", "5"}};  
UriTemplate t = new UriTemplate("/test/{a}/{b}", defVals);  
NameValueCollection vals = new NameValueCollection();  
vals.Add("a", "10");  
  
Uri boundUri = t.BindByName(baseAddress, vals);  
Console.WriteLine("BaseAddress: {0}", baseAddress);  
Console.WriteLine("Template: {0}", t.ToString());  
  
Console.WriteLine("Values: ");  
foreach (string key in vals.AllKeys)  
{  
    Console.WriteLine("\tKey = {0}, Value = {1}", key, vals[key]);  
}  
Console.WriteLine("Bound URI: {0}", boundUri);  
  
// The output of the preceding code is  
// BaseAddress: http://localhost:8000/  
// Template: /test/{a}/{b}  
// Values:  
//     Key = a, Value = 10  
// Bound URI: http://localhost:8000/test/10/5  
```  
  
Quando uma variável recebe um valor padrão, `null` há algumas restrições adicionais. Uma variável pode ter um valor padrão de `null` se a variável estiver contida no segmento mais à direita da cadeia de caracteres do modelo ou se todos os segmentos à direita do segmento tiverem valores padrão de `null` . Veja a seguir as cadeias de caracteres de modelo válidas com valores padrão de `null` :  
  
- `UriTemplate t = new UriTemplate("shoe/{boat=null}");`

- `UriTemplate t = new UriTemplate("{shoe=null}/{boat=null}");`
  
- `UriTemplate t = new UriTemplate("{shoe=1}/{boat=null}");`

 Estas são as cadeias de caracteres de modelo inválidas com valores padrão de `null` :  
  
- `UriTemplate t = new UriTemplate("{shoe=null}/boat"); // null default must be in the right most path segment`
  
- `UriTemplate t = new UriTemplate("{shoe=null}/{boat=x}/{bed=null}"); // shoe cannot have a null default because boat does not have a default null value`

### <a name="default-values-and-matching"></a>Valores padrão e correspondência  
 Ao corresponder um URI candidato com um modelo que tem valores padrão, os valores padrão serão colocados na <xref:System.UriTemplateMatch.BoundVariables%2A> coleção se os valores não forem especificados no URI candidato.  
  
### <a name="template-equivalence"></a>Equivalência de modelo  
 Dois modelos são considerados *estruturalmente equivalentes* quando todos os literais dos modelos são correspondentes e têm variáveis nos mesmos segmentos. Por exemplo, os modelos a seguir são estruturalmente equivalentes:  
  
- /a/{var1}/b b/{var2}? x = 1&y = 2  
  
- a/{x}/b% 20B/{var1}? y = 2&x = 1  
  
- a/{y}/B% 20B/{z}/? y = 2&x = 1  
  
 Algumas coisas a serem observadas:  
  
- Se um modelo contiver barras à esquerda, somente a primeira será ignorada.  
  
- Ao comparar cadeias de caracteres de modelo para equivalência estrutural, Case é ignorado para nomes de variáveis e segmentos de caminho, cadeias de consulta diferenciam maiúsculas de minúsculas.  
  
- As cadeias de caracteres de consulta são desordenadas.  
  
## <a name="uritemplatetable"></a>UriTemplateTable  
 A <xref:System.UriTemplateTable> classe representa uma tabela associativa de <xref:System.UriTemplate> objetos associados a um objeto da escolha do desenvolvedor. Um <xref:System.UriTemplateTable> deve conter pelo menos um <xref:System.UriTemplate> antes de chamar <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29> . O conteúdo de um <xref:System.UriTemplateTable> pode ser alterado até que <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29> seja chamado. A validação é executada quando o <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29> é chamado. O tipo de validação executada depende do valor do `allowMultiple` parâmetro para <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29> .  
  
 Quando <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29> o é chamado de passagem `false` , as <xref:System.UriTemplateTable> verificações para garantir que não haja modelos na tabela. Se encontrar qualquer modelo equivalente estrutural, ele lançará uma exceção. Isso é usado em conjunto com <xref:System.UriTemplateTable.MatchSingle%28System.Uri%29> quando você deseja garantir que apenas um modelo corresponda a um URI de entrada.  
  
 Quando <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29> o é chamado de passagem `true` , o <xref:System.UriTemplateTable> permite que vários modelos equivalentes estruturalmente estejam contidos em um <xref:System.UriTemplateTable> .  
  
 Se um conjunto de <xref:System.UriTemplate> objetos adicionado a uma <xref:System.UriTemplateTable> cadeia de caracteres de consulta contém, eles não devem ser ambíguos. Cadeias de caracteres de consulta idênticas são permitidas.  
  
> [!NOTE]
> Embora o <xref:System.UriTemplateTable> permita endereços base que usam esquemas diferentes de http, o esquema e o número da porta são ignorados ao corresponder os URIs candidatos aos modelos.  
  
### <a name="query-string-ambiguity"></a>Ambiguidade da cadeia de caracteres de consulta  
 Os modelos que compartilham um caminho equivalente contêm cadeias de caracteres de consulta ambíguas se houver um URI que corresponda a mais de um modelo.  
  
 Os seguintes conjuntos de cadeias de caracteres de consulta não são ambíguos em si:  
  
- ? x = 1  
  
- ? x = 2  
  
- ? x = 3  
  
- ? x = 1&y = {var}  
  
- ? x = 2&z = {var}  
  
- ? x = 3  
  
- ? x = 1  
  
- ?  
  
- ? x = {var}  
  
- ?  
  
- ? m = obter&c = RSS  
  
- ? m = Put&c = RSS  
  
- ? m = obter&c = Atom  
  
- ? m = Put&c = Atom  
  
 Os seguintes conjuntos de modelos de cadeia de caracteres de consulta são ambíguos em si:  
  
- ? x = 1  
  
- ? x = {var}  
  
 "x = 1"-corresponde a ambos os modelos.  
  
- ? x = 1  
  
- ? y = 2  
  
 "x = 1&y = 2" corresponde a ambos os modelos. Isso ocorre porque uma cadeia de caracteres de consulta pode conter mais variáveis de cadeia de caracteres de consulta que o modelo correspondente.  
  
- ? x = 1  
  
- ? x = 1&y = {var}  
  
 "x = 1&y = 3" corresponde a ambos os modelos.  
  
- ? x = 3&y = 4  
  
- ? x = 3&z = 5  
  
> [!NOTE]
> Os caracteres á e Á são considerados caracteres diferentes quando aparecem como parte de um caminho de URI ou um <xref:System.UriTemplate> literal de segmento de caminho (mas os caracteres a e a são considerados iguais). Os caracteres á e Á são considerados os mesmos caracteres quando aparecem como parte de um <xref:System.UriTemplate> {VariableName} ou uma cadeia de caracteres de consulta (e um e um também são considerados os mesmos caracteres).  
  
## <a name="see-also"></a>Consulte também

- [Visão geral do modelo de programação HTTP Web do WCF](wcf-web-http-programming-model-overview.md)
- [Modelo de objeto de programação HTTP Web do WCF](wcf-web-http-programming-object-model.md)
- [UriTemplate](../samples/uritemplate-sample.md)
- [Tabela de UriTemplate](../samples/uritemplate-table-sample.md)
- [Dispatcher de tabela de UriTemplate](../samples/uritemplate-table-dispatcher-sample.md)
