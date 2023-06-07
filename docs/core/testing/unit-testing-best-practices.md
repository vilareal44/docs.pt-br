---
title: Melhores práticas para escrever testes de unidade
description: Conheça as melhores práticas para escrever testes de unidade que promovem a qualidade e a resiliência do código em projetos .NET Core e .NET Standard.
author: jpreese
ms.author: wiwagn
ms.date: 07/28/2018
ms.openlocfilehash: ffeaa1e11512cab64695c120f844594b8c5014a8
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281102"
---
# <a name="unit-testing-best-practices-with-net-core-and-net-standard"></a>Melhores práticas de teste de unidade com .NET Core e .NET Standard

Há diversas vantagens para escrever testes de unidade; eles ajudam com a regressão, fornecem a documentação e facilitam o bom design. No entanto, testes de unidade difíceis de ler e frágeis podem causar estragos na sua base de código. Este artigo descreve algumas melhores práticas com relação ao design de teste de unidade para projetos .NET Core e .NET Standard.

Neste guia, você aprenderá algumas melhores práticas ao escrever testes de unidade para manter seus testes resilientes e fáceis de entender.

Por [John Reese](https://reese.dev) com agradecimentos especiais a [Roy Osherove](https://osherove.com/)

## <a name="why-unit-test"></a>Por que o teste de unidade?

### <a name="less-time-performing-functional-tests"></a>Menos tempo realizando testes funcionais

Testes funcionais são caros. Normalmente, envolvem abrir o aplicativo e realizar uma série de etapas que você (ou outra pessoa) deve seguir a fim de validar o comportamento esperado. Essas etapas nem sempre podem ser de conhecimento do testador, o que significa que ele terá que entrar em contato com alguém com maior conhecimento na área para executar o teste. O teste em si pode levar segundos para alterações triviais ou minutos para alterações maiores. Por fim, esse processo deve ser repetido para todas as alterações feitas no sistema.

Os testes de unidade, por outro lado, levam milissegundos, podem ser executados à imprensa de um botão e não necessariamente exigem qualquer conhecimento do sistema em geral. A aprovação ou reprovação no teste cabe ao executor do teste, não ao indivíduo.

### <a name="protection-against-regression"></a>Proteção contra regressão

Defeitos de regressão são defeitos introduzidos quando uma alteração foi feita no aplicativo. É comum os testadores não só testarem seu novo recurso, mas também recursos que existiam antes a fim de verificar se recursos previamente implementados ainda funcionam conforme o esperado.

Com o teste de unidade, é possível executar novamente todo o conjunto de testes após todo build ou até mesmo após a alteração de uma linha de código. Você poderá confiar que o novo código não interromperá a funcionalidade existente.

### <a name="executable-documentation"></a>Documentação executável

Nem sempre pode ser óbvio o que um método específico faz ou como ele se comporta diante de uma determinada entrada. Você pode se perguntar: como este método se comportará se eu passar uma cadeia de caracteres em branco? Nulo?

Quando você tem um conjunto de testes de unidade bem nomeados, cada teste deve ser capaz de explicar claramente a saída esperada de uma determinada entrada. Além disso, ele deve ser capaz de verificar se isso realmente funciona.

### <a name="less-coupled-code"></a>Código menos acoplado

Pode ser difícil para o teste de unidade quando o código está bem acoplado. Sem criar testes de unidade para o código que você está escrevendo, o acoplamento pode ser menos aparente.

Escrever testes para seu código o desacoplará naturalmente, porque seria mais difícil testar, caso contrário.

## <a name="characteristics-of-a-good-unit-test"></a>Características de um bom teste de unidade

- **Rápido**. Não é incomum para projetos maduros ter milhares de testes de unidade. Os testes de unidade devem levar muito pouco tempo para serem executados. Milissegundos.
- **Isolado**. Testes de unidade são autônomos, podem ser executados em isolamento e não têm dependências em nenhum fator externo, como um sistema de arquivos ou o banco de dados.
- **Repetível**. A execução de um teste de unidade deve ser consistente com seus resultados, ou seja, ele sempre retornará o mesmo resultado se você não alterar nada entre execuções.
- **Verificação automática**. O teste deve ser capaz de detectar automaticamente se ele foi aprovado ou reprovado sem nenhuma interação humana.
- **Em tempo hábil**. Um teste de unidade não deve levar um tempo desproporcionalmente longo para ser escrito comparado com o código que está sendo testado. Se você achar que o teste do código está levando uma grande quantidade de tempo comparado com a escrita do código, considere um design mais testável.

## <a name="code-coverage"></a>Cobertura de código

Uma porcentagem alta de cobertura de código geralmente está associada a uma qualidade de código mais alta. No entanto, a própria medição *não pode* determinar a qualidade do código. Definir uma meta de porcentagem de cobertura de código excessivamente ambicioso pode ser de pré-produção. Imagine um projeto complexo com milhares de ramificações condicionais e imagine que você definiu uma meta de cobertura de código de 95%. Atualmente, o projeto mantém a cobertura de código de 90%. A quantidade de tempo que leva para levar em conta todos os casos de borda dos 5% restantes pode ser um grande empreendimento, e a proposta de valor diminui rapidamente.

Uma porcentagem alta de cobertura de código não é um indicador de sucesso, nem significa alta qualidade de código. Ele representa apenas a quantidade de código coberta pelos testes de unidade. Para obter mais informações, consulte [cobertura de código de teste de unidade](unit-testing-code-coverage.md).

## <a name="lets-speak-the-same-language"></a>Vamos falar a mesma língua

A *simulação* de termos, infelizmente, geralmente é usada para falar sobre os testes. Os seguintes pontos definem os tipos mais comuns de *falsificações* ao escrever testes de unidade:

*Falso* -uma falsificação é um termo genérico que pode ser usado para descrever um stub ou um objeto fictício. Seja um stub ou uma simulação depende do contexto no qual ele é usado. Então, em outras palavras, uma falsificação pode ser um stub ou uma simulação.

*Simulação* -um objeto fictício é um objeto falso no sistema que decide se um teste de unidade foi aprovado ou não. Uma simulação começa como falsa até ser declarada.

*Stub* – Um stub é uma substituição controlável para uma dependência existente (ou colaborador) no sistema. Ao usar um stub, é possível testar seu código sem lidar diretamente com a dependência. Por padrão, uma falsificação começa como um stub.

Considere o seguinte snippet de código:

```csharp
var mockOrder = new MockOrder();
var purchase = new Purchase(mockOrder);

purchase.ValidateOrders();

Assert.True(purchase.CanBeShipped);
```

Este seria um exemplo de um stub sendo chamado de simulação. Nesse caso, ele é um stub. Você está passando a ordem como um meio de poder instanciar `Purchase` (o sistema em teste). O nome `MockOrder` também é enganoso porque, novamente, a ordem não é uma simulação.

Uma abordagem melhor seria

```csharp
var stubOrder = new FakeOrder();
var purchase = new Purchase(stubOrder);

purchase.ValidateOrders();

Assert.True(purchase.CanBeShipped);
```

Renomeando a classe para `FakeOrder`, você tornou a classe muito mais genérica, a classe pode ser usada como uma simulação ou um stub. O que for melhor para o caso de teste. No exemplo acima, `FakeOrder` é usado como um stub. Você não está usando o `FakeOrder` em qualquer forma durante a declaração. `FakeOrder`foi passado para a `Purchase` classe para atender aos requisitos do construtor.

Para usá-lo como uma Simulação, você poderia fazer algo como isto

```csharp
var mockOrder = new FakeOrder();
var purchase = new Purchase(mockOrder);

purchase.ValidateOrders();

Assert.True(mockOrder.Validated);
```

Nesse caso, você está verificando uma propriedade na Falsificação (declarando em relação a ela), então no snippet de código acima, o `mockOrder` é uma Simulação.

> [!IMPORTANT]
> É importante usar esta terminologia corretamente. Se você chamar seus stubs de "simulações", outros desenvolvedores farão suposições falsas sobre sua intenção.

O ponto principal a lembrar sobre simulações versus stub é que simulações são como stubs, mas você declara com relação ao objeto fictício, enquanto você não declara com relação a um stub.

## <a name="best-practices"></a>Práticas recomendadas

### <a name="naming-your-tests"></a>Nomeando seus testes

O nome do seu teste deve ser composto por três partes:

- O nome do método que está sendo testado.
- O cenário em que ele está sendo testado.
- O comportamento esperado quando o cenário é invocado.

#### <a name="why"></a>Por quê?

- Padrões de nomenclatura são importantes, porque eles expressam explicitamente a intenção do teste.

Testes são mais do que apenas verificar se seu código funciona; eles também fornecem documentação. Apenas examinando o conjunto de testes de unidade, você deve poder inferir o comportamento do seu código sem mesmo examinar o código em si. Além disso, quando os testes falharem, será possível ver exatamente quais cenários não atendem às suas expectativas.

#### <a name="bad"></a>Ruim:

[!code-csharp[BeforeNaming](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeNaming)]

#### <a name="better"></a>Melhor:

[!code-csharp[AfterNamingAndMinimallyPassing](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterNamingAndMinimallyPassing)]

### <a name="arranging-your-tests"></a>Organizando seus testes

**Organizar, Agir, Declarar** é um padrão comum ao testar unidades. Como o nome implica, ele é composto por três ações principais:

- *Organizar* seus objetos, criando e configurando-os conforme necessário.
- *Agir* em um objeto.
- *Declarar* que algo está conforme o esperado.

#### <a name="why"></a>Por quê?

- Isso separa claramente o que está sendo testado das etapas *organizar* e *declarar*.
- Menos oportunidade de combinar declarações com o código "Agir".

A legibilidade é um dos aspectos mais importante ao escrever um teste. A separação de cada uma dessas ações dentro do teste realça claramente as dependências necessárias para chamar seu código, a maneira como seu código está sendo chamado e o que você está tentando declarar. Embora seja possível combinar algumas etapas e reduzir o tamanho do seu teste, a principal meta é tornar o teste o mais legível possível.

#### <a name="bad"></a>Ruim:

[!code-csharp[BeforeArranging](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeArranging)]

#### <a name="better"></a>Melhor:

[!code-csharp[AfterArranging](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterArranging)]

### <a name="write-minimally-passing-tests"></a>Escrever testes com o mínimo de aprovação

A entrada a ser usada em um teste de unidade deve ser a mais simples possível a fim de verificar o comportamento que você está testando no momento.

#### <a name="why"></a>Por quê?

- Os testes se tornam mais resilientes a alterações futuras na base de código.
- Mais próximo do comportamento do teste ao longo da implementação.

Testes que incluem mais informações do que as necessárias para serem aprovados têm maior chance de introduzir erros no teste e podem deixar a intenção dele menos clara. Ao escrever testes, você deseja se concentrar no comportamento. Definir propriedades extra em modelos ou usar valores diferentes de zero quando não for necessário só fará desviar do que você está tentando provar.

#### <a name="bad"></a>Ruim:

[!code-csharp[BeforeMinimallyPassing](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeMinimallyPassing)]

#### <a name="better"></a>Melhor:

[!code-csharp[AfterNamingAndMinimallyPassing](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterNamingAndMinimallyPassing)]

### <a name="avoid-magic-strings"></a>Evitar cadeias de caracteres mágicas

Nomear variáveis em testes de unidade é tão importante, se não mais importante, do que nomear variáveis no código de produção. Os testes de unidade não devem conter cadeias de caracteres mágicas.

#### <a name="why"></a>Por quê?

- Evita a necessidade de o leitor do teste inspecionar o código de produção a fim de descobrir o que torna o valor especial.
- Mostra explicitamente o que você está tentando *provar*, em vez de o que você está tentando *realizar*.

Cadeias de caracteres mágicas podem gerar confusão no leitor dos seus testes. Se uma cadeia de caracteres parecer estar fora do comum, eles poderão ser perguntar por que um determinado valor foi escolhido para um parâmetro ou valor retornado. Talvez isso os leve a examinar mais detalhadamente os detalhes da implementação, em vez de se concentrar no teste.

> [!TIP]
> Ao escrever testes, seu objetivo deve ser expressar o máximo de intenção possível. No caso de cadeias de caracteres mágicas, uma boa abordagem é atribuir esses valores a constantes.

#### <a name="bad"></a>Ruim:

[!code-csharp[BeforeMagicString](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeMagicString)]

#### <a name="better"></a>Melhor:

[!code-csharp[AfterMagicString](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterMagicString)]

### <a name="avoid-logic-in-tests"></a>Evitar a lógica em testes

Ao escrever seus testes de unidade, evite a concatenação manual de cadeia de caracteres e condições lógicas como `if`, `while`, `for`, `switch`, etc.

#### <a name="why"></a>Por quê?

- Menor chance de introduzir um bug dentro dos seus testes.
- Concentre-se no resultado final, em vez de nos detalhes da implementação.

Quando você introduz a lógica em seu conjunto de testes, a possibilidade de introduzir um bug nela aumenta drasticamente. O último lugar que você deseja encontrar um bug é dentro do seu conjunto de testes. Você deve ter um alto nível de confiança de que seus testes funcionam; caso contrário, você não confiará neles. Testes nos quais você não confia não fornecem nenhum valor. Quando um teste falha, convém ter uma noção de que algo está realmente errado com o seu código e de que isso não pode ser ignorado.

> [!TIP]
> Se a lógica em seu teste parecer inevitável, considere dividir o teste em dois ou mais testes diferentes.

#### <a name="bad"></a>Ruim:

[!code-csharp[LogicInTests](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#LogicInTests)]

#### <a name="better"></a>Melhor:

[!code-csharp[AfterTestLogic](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterTestLogic)]

### <a name="prefer-helper-methods-to-setup-and-teardown"></a>Preferir métodos auxiliares para instalação (setup) e desinstalação (teardown)

Se você precisar de um objeto ou estado semelhante para seus testes, prefira um método auxiliar do que usar atributos Setup e Teardown, se existirem.

#### <a name="why"></a>Por quê?

- Menos confusão ao ler os testes, uma vez que todo o código está visível de dentro de cada teste.
- Menos chance de configurar demais ou de menos para o teste específico.
- Menos chance de compartilhar o estado entre os testes, que cria dependências indesejadas entre eles.

Nas estruturas de teste de unidade, `Setup` é chamado antes de cada teste de unidade dentro do seu conjunto de testes. Embora alguns possam conceber isso como uma ferramenta útil, ela geralmente acaba levando a testes difíceis de ler e sobrecarregados. Cada teste geralmente terá requisitos diferentes para fazer o teste entrar em funcionamento. Infelizmente, `Setup` força você a usar os mesmos requisitos exatos para cada teste.

> [!NOTE]
> O xUnit removeu SetUp e TearDown a partir da versão 2.x

#### <a name="bad"></a>Ruim:

[!code-csharp[BeforeSetup](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeSetup)]

```csharp
// more tests...
```

[!code-csharp[BeforeHelperMethod](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeHelperMethod)]

#### <a name="better"></a>Melhor:

[!code-csharp[AfterHelperMethod](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterHelperMethod)]

```csharp
// more tests...
```

[!code-csharp[AfterSetup](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterSetup)]

### <a name="avoid-multiple-asserts"></a>Evitar várias declarações

Ao escrever seus testes, tente incluir apenas uma declaração por teste. Abordagens comuns para usar apenas uma declaração incluem:

- Criar um teste separado para cada declaração.
- Usar testes parametrizados.

#### <a name="why"></a>Por quê?

- Se uma declaração falhar, as subsequentes não serão avaliadas.
- Garante que você não está declarando vários casos em seus testes.
- Oferece a você todo o panorama do porquê seus testes estão falhando.

Ao introduzir várias declarações em um caso de teste, não há garantida de que todas elas serão executadas. Na maioria das estruturas de teste de unidade, após uma declaração falhar em um teste de unidade, os testes seguintes serão considerados reprovados automaticamente. Isso pode ser confuso, porque a funcionalidade que está realmente em funcionamento será mostrada como falha.

> [!NOTE]
> Uma exceção comum a essa regra é ao declarar em relação a um objeto. Nesse caso, é geralmente aceitável ter várias declarações com relação a cada propriedade para garantir que o objeto está no estado em que você espera que ele esteja.

#### <a name="bad"></a>Ruim:

[!code-csharp[BeforeMultipleAsserts](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeMultipleAsserts)]

#### <a name="better"></a>Melhor:

[!code-csharp[AfterMultipleAsserts](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterMultipleAsserts)]

### <a name="validate-private-methods-by-unit-testing-public-methods"></a>Validar métodos privados por métodos públicos de teste de unidade

Na maioria dos casos, não deve haver a necessidade de testar um método privado. Métodos privados são um detalhe de implementação. Você pode pensar dessa forma: métodos privados nunca existem isoladamente. Em algum momento, haverá um método voltado para o público que chame o método privado como parte de sua implementação. O que importa é o resultado final do método público que é chamado no privado.

Considere o seguinte caso

```csharp
public string ParseLogLine(string input)
{
    var sanitizedInput = TrimInput(input);
    return sanitizedInput;
}

private string TrimInput(string input)
{
    return input.Trim();
}
```

Sua primeira reação pode ser começar a escrever um teste para `TrimInput` porque você deseja certificar-se de que o método está funcionando conforme o esperado. No entanto, é totalmente possível que `ParseLogLine` manipule `sanitizedInput` de uma maneira inesperada, tornando um teste em relação a `TrimInput` inútil.

O teste real deve ser feito em relação ao método voltado para o público `ParseLogLine`, porque esse é o que importa no fim das contas.

```csharp
public void ParseLogLine_StartsAndEndsWithSpace_ReturnsTrimmedResult()
{
    var parser = new Parser();

    var result = parser.ParseLogLine(" a ");

    Assert.Equals("a", result);
}
```

Com esse ponto de vista, se você vir um método privado, encontre o método público e escreva seus testes com relação a esse método. Só porque um método privado retorna o resultado esperado não significa que o sistema que eventualmente chama o método privado usa o resultado corretamente.

### <a name="stub-static-references"></a>Referências estáticas de stub

Um dos princípios de um teste de unidade é que ele deve ter controle completo do sistema em teste. Isso pode ser problemático quando o código de produção inclui chamadas para referências estáticas (por exemplo, `DateTime.Now` ). Considere o código a seguir

```csharp
public int GetDiscountedPrice(int price)
{
    if (DateTime.Now.DayOfWeek == DayOfWeek.Tuesday)
    {
        return price / 2;
    }
    else
    {
        return price;
    }
}
```

Como esse código pode possivelmente ter unidades testadas? Você pode tentar uma abordagem como

```csharp
public void GetDiscountedPrice_NotTuesday_ReturnsFullPrice()
{
    var priceCalculator = new PriceCalculator();

    var actual = priceCalculator.GetDiscountedPrice(2);

    Assert.Equals(2, actual)
}

public void GetDiscountedPrice_OnTuesday_ReturnsHalfPrice()
{
    var priceCalculator = new PriceCalculator();

    var actual = priceCalculator.GetDiscountedPrice(2);

    Assert.Equals(1, actual);
}
```

Infelizmente, você perceberá rapidamente que há alguns problemas com seus testes.

- Se o conjunto de testes for executado em uma terça-feira, o segundo teste será aprovado, mas o primeiro não.
- Se o conjunto de testes for executado em qualquer outro dia, o primeiro teste será aprovado, mas o segundo não.

Para resolver esses problemas, será necessário introduzir uma *costura* em seu código de produção. Uma abordagem é encapsular o código que você precisa controlar em uma interface e fazer o código de produção depender dessa interface.

```csharp
public interface IDateTimeProvider
{
    DayOfWeek DayOfWeek();
}

public int GetDiscountedPrice(int price, IDateTimeProvider dateTimeProvider)
{
    if (dateTimeProvider.DayOfWeek() == DayOfWeek.Tuesday)
    {
        return price / 2;
    }
    else
    {
        return price;
    }
}
```

Seu conjunto de testes agora se torna

```csharp
public void GetDiscountedPrice_NotTuesday_ReturnsFullPrice()
{
    var priceCalculator = new PriceCalculator();
    var dateTimeProviderStub = new Mock<IDateTimeProvider>();
    dateTimeProviderStub.Setup(dtp => dtp.DayOfWeek()).Returns(DayOfWeek.Monday);

    var actual = priceCalculator.GetDiscountedPrice(2, dateTimeProviderStub);

    Assert.Equals(2, actual);
}

public void GetDiscountedPrice_OnTuesday_ReturnsHalfPrice()
{
    var priceCalculator = new PriceCalculator();
    var dateTimeProviderStub = new Mock<IDateTimeProvider>();
    dateTimeProviderStub.Setup(dtp => dtp.DayOfWeek()).Returns(DayOfWeek.Tuesday);

    var actual = priceCalculator.GetDiscountedPrice(2, dateTimeProviderStub);

    Assert.Equals(1, actual);
}
```

Agora o conjunto de testes tem controle total sobre `DateTime.Now` e pode fazer stub de qualquer valor ao ser chamado no método.
