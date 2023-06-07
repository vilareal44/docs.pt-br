---
title: Indexadores
description: Saiba mais sobre indexadores C# e como implementar propriedades indexadas, que são propriedades referenciadas usando um ou mais argumentos.
ms.date: 06/20/2016
ms.technology: csharp-fundamentals
ms.assetid: 0e9496da-e766-45a9-b92b-91820d4a350e
ms.openlocfilehash: 1369740404c500d8b44b4706959bf4640c26aa2d
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656105"
---
# <a name="indexers"></a>Indexadores

Os *indexadores* são semelhantes às propriedades. De muitas maneiras, os indexadores baseiam-se nos mesmos recursos de linguagem que as [propriedades](properties.md). Os indexadores habilitam as propriedades *indexadas*: propriedades referenciadas com o uso de um ou mais argumentos. Esses argumentos fornecem um índice em um conjunto de valores.

## <a name="indexer-syntax"></a>Sintaxe do indexador

Você pode acessar um indexador por meio de um nome de variável e colchetes. Coloque os argumentos do indexador dentro de colchetes:

```csharp
var item = someObject["key"];
someObject["AnotherKey"] = item;
```

Você declara os indexadores usando a palavra-chave `this` como o nome da propriedade e declarando os argumentos entre colchetes. Essa declaração corresponderia à utilização mostrada no parágrafo anterior:

```csharp
public int this[string key]
{
    get { return storage.Find(key); }
    set { storage.SetAt(key, value); }
}
```

Neste exemplo inicial, você pode ver a relação entre a sintaxe das propriedades e dos indexadores. Essa analogia impulsiona a maioria das regras de sintaxe para indexadores. Os indexadores podem ter qualquer modificador de acesso válido (público, protegido interno, protegido, interno, particular ou protegido de forma privada). Eles podem ser sealed, virtual ou abstract. Assim como acontece com as propriedades, você pode especificar modificadores de acesso diferentes para os acessadores get e set em um indexador.
Você também pode especificar indexadores somente leitura (omitindo o acessador set) ou indexadores somente gravação (omitindo o acessador get).

Você pode aplicar aos indexadores quase tudo o que aprendeu ao trabalhar com propriedades. A única exceção a essa regra são as *propriedades autoimplementadas*. O compilador não pode gerar sempre o armazenamento correto para um indexador.

A presença dos argumentos para referenciar um item em um conjunto de itens distingue os indexadores das propriedades. Você pode definir vários indexadores em um tipo, contanto que as listas de argumentos para cada indexador seja exclusiva. Vamos explorar diferentes cenários em que você pode usar um ou mais indexadores em uma definição de classe.

## <a name="scenarios"></a>Cenários

Você deve definir *indexadores* em seu tipo quando a API do tipo modela alguma coleção na qual você define os argumentos para essa coleção. Seu indexadores podem ou não mapear diretamente para os tipos de coleção que fazem parte da estrutura principal do .NET. O tipo pode ter outras responsabilidades, além da modelagem de uma coleção.
Os indexadores permitem que você forneça a API que corresponda à abstração do tipo, sem expor os detalhes internos de como os valores dessa abstração são armazenados ou computados.

Vamos examinar alguns dos cenários comuns de uso de *indexadores*. Você pode acessar a [pasta de exemplo para indexadores](https://github.com/dotnet/samples/tree/master/csharp/indexers). Para obter instruções de download, consulte [Exemplos e tutoriais](../samples-and-tutorials/index.md#view-and-download-samples).

### <a name="arrays-and-vectors"></a>Matrizes e vetores

Um dos cenários mais comuns para a criação de indexadores é quando seu tipo modela uma matriz ou um vetor. Você pode criar um indexador para modelar uma lista ordenada de dados.

A vantagem de criar seu próprio indexador é que você pode definir o armazenamento dessa coleção para atender às suas necessidades. Imagine um cenário em que seu tipo modela dados históricos que são muito grandes para serem carregados na memória ao mesmo tempo. Você precisa carregar e descarregar seções da coleção com base na utilização. O exemplo a seguir modela esse comportamento. Ele relata quantos pontos de dados existem. Ele cria páginas para manter as seções de dados sob demanda. Ele remove páginas da memória a fim de liberar espaço para as páginas necessárias para as solicitações mais recentes.

```csharp
public class DataSamples
{
    private class Page
    {
        private readonly List<Measurements> pageData = new List<Measurements>();
        private readonly int startingIndex;
        private readonly int length;
        private bool dirty;
        private DateTime lastAccess;

        public Page(int startingIndex, int length)
        {
            this.startingIndex = startingIndex;
            this.length = length;
            lastAccess = DateTime.Now;

            // This stays as random stuff:
            var generator = new Random();
            for(int i=0; i < length; i++)
            {
                var m = new Measurements
                {
                    HiTemp = generator.Next(50, 95),
                    LoTemp = generator.Next(12, 49),
                    AirPressure = 28.0 + generator.NextDouble() * 4
                };
                pageData.Add(m);
            }
        }
        public bool HasItem(int index) =>
            ((index >= startingIndex) &&
            (index < startingIndex + length));

        public Measurements this[int index]
        {
            get
            {
                lastAccess = DateTime.Now;
                return pageData[index - startingIndex];
            }
            set
            {
                pageData[index - startingIndex] = value;
                dirty = true;
                lastAccess = DateTime.Now;
            }
        }

        public bool Dirty => dirty;
        public DateTime LastAccess => lastAccess;
    }

    private readonly int totalSize;
    private readonly List<Page> pagesInMemory = new List<Page>();

    public DataSamples(int totalSize)
    {
        this.totalSize = totalSize;
    }

    public Measurements this[int index]
    {
        get
        {
            if (index < 0)
                throw new IndexOutOfRangeException("Cannot index less than 0");
            if (index >= totalSize)
                throw new IndexOutOfRangeException("Cannot index past the end of storage");

            var page = updateCachedPagesForAccess(index);
            return page[index];
        }
        set
        {
            if (index < 0)
                throw new IndexOutOfRangeException("Cannot index less than 0");
            if (index >= totalSize)
                throw new IndexOutOfRangeException("Cannot index past the end of storage");
            var page = updateCachedPagesForAccess(index);

            page[index] = value;
        }
    }

    private Page updateCachedPagesForAccess(int index)
    {
        foreach (var p in pagesInMemory)
        {
            if (p.HasItem(index))
            {
                return p;
            }
        }
        var startingIndex = (index / 1000) * 1000;
        var newPage = new Page(startingIndex, 1000);
        addPageToCache(newPage);
        return newPage;
    }

    private void addPageToCache(Page p)
    {
        if (pagesInMemory.Count > 4)
        {
            // remove oldest non-dirty page:
            var oldest = pagesInMemory
                .Where(page => !page.Dirty)
                .OrderBy(page => page.LastAccess)
                .FirstOrDefault();
            // Note that this may keep more than 5 pages in memory
            // if too much is dirty
            if (oldest != null)
                pagesInMemory.Remove(oldest);
        }
        pagesInMemory.Add(p);
    }
}
```

Você pode seguir esta linguagem de design para modelar qualquer tipo de coleção na qual há bons motivos para não carregar todo o conjunto de dados em uma coleção na memória. Observe que a classe `Page` é uma classe particular aninhada que não faz parte da interface pública. Esses detalhes estão ocultos de qualquer usuário dessa classe.

### <a name="dictionaries"></a>Dicionários

Outro cenário comum é quando você precisa modelar um dicionário ou um mapa. Esse cenário é quando o seu tipo armazena valores com base na chave, normalmente chaves de texto. Este exemplo cria um dicionário que mapeia os argumentos de linha de comando para [expressões lambda](delegates-overview.md) que gerenciam essas opções. O exemplo a seguir mostra duas classes: uma classe `ArgsActions` que mapeia uma opção de linha de comando para um delegado `Action` e uma `ArgsProcessor`, que usa a `ArgsActions` para executar cada `Action`, quando encontrar essa opção.

```csharp
public class ArgsProcessor
{
    private readonly ArgsActions actions;

    public ArgsProcessor(ArgsActions actions)
    {
        this.actions = actions;
    }

    public void Process(string[] args)
    {
        foreach(var arg in args)
        {
            actions[arg]?.Invoke();
        }
    }

}
public class ArgsActions
{
    readonly private Dictionary<string, Action> argsActions = new Dictionary<string, Action>();

    public Action this[string s]
    {
        get
        {
            Action action;
            Action defaultAction = () => {} ;
            return argsActions.TryGetValue(s, out action) ? action : defaultAction;
        }
    }

    public void SetOption(string s, Action a)
    {
        argsActions[s] = a;
    }
}
```

Neste exemplo, a coleção `ArgsAction` mapeia próximo à coleção subjacente.
O `get` determina se uma opção específica foi configurada. Se sim, ele retorna a `Action` associada a essa opção. Se não, ele retorna uma `Action` que não faz nada. O acessador público não inclui um acessador `set`. Em vez disso, o design usa um método público para a configuração de opções.

### <a name="multi-dimensional-maps"></a>Mapas multidimensionais

Você pode criar indexadores que usam vários argumentos. Além disso, esses argumentos não estão restritos a serem do mesmo tipo. Vamos analisar dois exemplos.

O primeiro exemplo mostra uma classe que gera valores para um conjunto de Mandelbrot. Para obter mais informações sobre a matemática por trás desse conjunto, leia [este artigo](https://en.wikipedia.org/wiki/Mandelbrot_set).
O indexador usa dois duplos para definir um ponto no plano X, Y.
O acessador get calcula o número de iterações até que um ponto seja considerado como fora do conjunto. Se o número máximo de iterações for atingido, o ponto está no conjunto e o valor da classe maxIterations será retornado. (As imagens geradas por computador, popularizadas para o conjunto de Mandelbrot, definem cores para o número de iterações necessárias para determinar que um ponto está fora do conjunto.

```csharp
public class Mandelbrot
{
    readonly private int maxIterations;

    public Mandelbrot(int maxIterations)
    {
        this.maxIterations = maxIterations;
    }

    public int this [double x, double y]
    {
        get
        {
            var iterations = 0;
            var x0 = x;
            var y0 = y;

            while ((x*x + y * y < 4) &&
                (iterations < maxIterations))
            {
                var newX = x * x - y * y + x0;
                y = 2 * x * y + y0;
                x = newX;
                iterations++;
            }
            return iterations;
        }
    }
}
```

O conjunto de Mandelbrot define valores em cada coordenada (x,y) para valores de número real.
Isso define um dicionário que poderia conter um número infinito de valores. Portanto, não há armazenamento por trás desse conjunto. Em vez disso, essa classe calcula o valor de cada ponto quando o código chama o acessador `get`. Não há nenhum armazenamento subjacente usado.

Vamos examinar um último uso de indexadores, em que o indexador recebe vários argumentos de tipos diferentes. Considere um programa que gerencia os dados históricos de temperatura. Esse indexador utiliza uma cidade e uma data para definir ou obter as temperaturas máximas e mínimas desse local:

```csharp
using DateMeasurements =
    System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>;
using CityDataMeasurements =
    System.Collections.Generic.Dictionary<string, System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>>;

public class HistoricalWeatherData
{
    readonly CityDataMeasurements storage = new CityDataMeasurements();

    public Measurements this[string city, DateTime date]
    {
        get
        {
            var cityData = default(DateMeasurements);

            if (!storage.TryGetValue(city, out cityData))
                throw new ArgumentOutOfRangeException(nameof(city), "City not found");

            // strip out any time portion:
            var index = date.Date;
            var measure = default(Measurements);
            if (cityData.TryGetValue(index, out measure))
                return measure;
            throw new ArgumentOutOfRangeException(nameof(date), "Date not found");
        }
        set
        {
            var cityData = default(DateMeasurements);

            if (!storage.TryGetValue(city, out cityData))
            {
                cityData = new DateMeasurements();
                storage.Add(city, cityData);
            }

            // Strip out any time portion:
            var index = date.Date;
            cityData[index] = value;
        }
    }
}
```

Este exemplo cria um indexador que mapeia dados meteorológicos de dois argumentos diferentes: uma cidade (representada por uma `string`) e uma data (representada por uma `DateTime`). O armazenamento interno usa duas classes `Dictionary` para representar o dicionário bidimensional. A API pública não representa mais o armazenamento subjacente. Em vez disso, os recursos de linguagem dos indexadores permite que você crie uma interface pública que representa a sua abstração, mesmo que o armazenamento subjacente deva usar diferentes tipos principais de coleção.

Há duas partes desse código que podem não ser familiares para alguns desenvolvedores. Essas duas `using` diretivas:

```csharp
using DateMeasurements = System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>;
using CityDataMeasurements = System.Collections.Generic.Dictionary<string, System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>>;
```

criam um *alias* para um tipo genérico construído. Essas instruções habilitam o código a usar, mais adiante, os nomes `DateMeasurements` e `CityDateMeasurements` mais descritivos, em vez da construção genérica de `Dictionary<DateTime, Measurements>` e `Dictionary<string, Dictionary<DateTime, Measurements> >`.
Esse constructo exige o uso de nomes de tipo totalmente qualificados no lado direito do sinal `=`.

A segunda técnica é para remover as partes de hora de qualquer objeto `DateTime` usado para indexar na coleção. O .NET não inclui um tipo somente de data.
Os desenvolvedores usam o tipo `DateTime`, mas usam a propriedade `Date` para garantir que qualquer objeto `DateTime` daquele dia sejam iguais.

## <a name="summing-up"></a>Resumindo

Você deve criar indexadores sempre que tiver um elemento semelhante a uma propriedade em sua classe, em que essa propriedade representa não um único valor, mas uma coleção de valores em que cada item individual é identificado por um conjunto de argumentos. Esses argumentos podem identificar exclusivamente qual item da coleção deve ser referenciado.
Os indexadores estendem o conceito de [Propriedades](properties.md), em que um membro é tratado como um item de dados de fora da classe, mas como um método no interior. Os indexadores permitem que os argumentos localizem um único item em uma propriedade que representa um conjunto de itens.
