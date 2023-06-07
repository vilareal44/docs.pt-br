---
title: Noções básicas da coleta de lixo
description: Saiba como o coletor de lixo funciona e como ele pode ser configurado para ter um desempenho ideal.
ms.date: 11/15/2019
ms.technology: dotnet-standard
helpviewer_keywords:
- garbage collection, generations
- garbage collection, background
- garbage collection, concurrent
- garbage collection, server
- garbage collection, workstation
- garbage collection, managed heap
ms.assetid: 67c5a20d-1be1-4ea7-8a9a-92b0b08658d2
ms.openlocfilehash: 322e079a1be556efb536b24e216e480c1950bd8c
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87917020"
---
# <a name="fundamentals-of-garbage-collection"></a>Noções básicas da coleta de lixo

No Common Language Runtime (CLR), o coletor de lixo (GC) serve como um Gerenciador de memória automático. O coletor de lixo gerencia a alocação e o lançamento da memória para um aplicativo. Para os desenvolvedores que trabalham com código gerenciado, isso significa que você não precisa escrever código para executar tarefas de gerenciamento de memória. O gerenciamento automático de memória pode eliminar problemas comuns, como esquecer de liberar um objeto e causar um vazamento de memória ou tentar acessar a memória de um objeto que já foi liberado.

Este artigo descreve os principais conceitos da coleta de lixo.

## <a name="benefits"></a>Vantagens

O coletor de lixo fornece os seguintes benefícios:

- Libera os desenvolvedores de terem que liberar memória manualmente.

- Aloca objetos no heap gerenciado com eficiência.

- Recupera os objetos que não estão sendo usados, limpa a memória e mantém a memória disponível para alocações futuras. Os objetos gerenciados obtêm automaticamente conteúdo limpo para começar, de modo que seus construtores não precisam inicializar todos os campos de dados.

- Fornece segurança de memória, assegurando que um objeto não possa usar o conteúdo de outro objeto.

## <a name="fundamentals-of-memory"></a>Conceitos básicos de memória

A lista a seguir resume conceitos importantes de memória do CLR.

- Cada processo tem seu próprio espaço de endereço virtual separado. Todos os processos no mesmo computador compartilham a mesma memória física e o arquivo de paginação, se houver um.

- Por padrão, em computadores de 32 bits, cada processo tem um espaço de endereço virtual no modo de usuário de 2 GB.

- Como desenvolvedor de aplicativos, você trabalha apenas com o espaço de endereço virtual e nunca manipula a memória física diretamente. O coletor de lixo aloca e libera memória virtual para você no heap gerenciado.

  Se você estiver escrevendo código nativo, use as funções do Windows para trabalhar com o espaço de endereço virtual. Essas funções alocam e liberam memória virtual para você em heaps nativos.

- A memória virtual pode estar em três estados:

  | Estado | Descrição |
  |---------|---------|
  | Grátis | O bloco de memória não tem referências a ele e está disponível para alocação. |
  | Reservado | O bloco de memória está disponível para seu uso e não pode ser usado para nenhuma outra solicitação de alocação. No entanto, você não pode armazenar dados nesse bloco de memória até que ele esteja comprometido. |
  | Confirmado | O bloco de memória é atribuído para armazenamento físico. |

- O espaço de endereço virtual pode ficar fragmentado. Isso significa que há blocos livres, também conhecido como furos, no espaço de endereço. Quando uma alocação de memória virtual é solicitada, o gerenciador de memória virtual precisa localizar um único bloco livre suficientemente grande para atender a essa solicitação de alocação. Mesmo que você tenha 2 GB de espaço livre, uma alocação que requer 2 GB não será bem-sucedida, a menos que todo o espaço livre esteja em um único bloco de endereço.

- Você pode ficar sem memória se não houver espaço de endereço virtual suficiente para reservar ou espaço físico para confirmar.

  O arquivo de paginação é usado mesmo se a pressão de memória física (ou seja, a demanda de memória física) estiver baixa. Na primeira vez em que a pressão de memória física é alta, o sistema operacional deve liberar espaço na memória física para armazenar dados e faz backup de alguns dos dados que estão na memória física para o arquivo de paginação. Esses dados não são paginados até que seja necessário, portanto, é possível encontrar paginação em situações em que a pressão da memória física está baixa.
  
### <a name="memory-allocation"></a>Alocação de memória

Quando você inicializa um novo processo, o runtime reserva uma região contígua de espaço de endereço para o processo. Esse espaço de endereço reservado é chamado de heap gerenciado. O heap gerenciado mantém um ponteiro para o endereço no qual o próximo objeto do heap será alocado. Inicialmente, esse ponteiro é definido como o endereço básico do heap gerenciado. Todos os tipos de referência são alocados no heap gerenciado. Quando um aplicativo cria o primeiro tipo de referência, a memória é alocada para o tipo no endereço base do heap gerenciado. Quando o aplicativo cria o próximo objeto, o coletor de lixo aloca memória para ele no espaço de endereço logo depois do primeiro objeto. Desde que exista espaço de endereço disponível, o coletor de lixo continua alocando espaço para novos objetos dessa maneira.

A alocação memória com base no heap gerenciado é mais rápida do que a alocação de memória não gerenciada. Como o tempo de execução aloca memória para um objeto adicionando um valor a um ponteiro, é quase tão rápido quanto alocar memória da pilha. Além disso, como novos objetos alocados consecutivamente são armazenados de forma contígua no heap gerenciado, um aplicativo pode acessar os objetos rapidamente.

### <a name="memory-release"></a>Liberação de memória

O mecanismo de otimização do coletor de lixo determina o melhor momento para executar uma coleta com base nas alocações que estão sendo feitas. Quando o coletor de lixo executa uma coleta, ele libera a memória dos objetos que não estão mais sendo usados pelo aplicativo. Ele determina quais objetos não estão mais sendo usados examinando as *raízes*do aplicativo. As raízes de um aplicativo incluem campos estáticos, variáveis locais e parâmetros na pilha de um thread, além de registros de CPU. Cada raiz refere-se a um objeto no heap gerenciado ou é definida como nula. O coletor de lixo tem acesso à lista de raízes ativas mantidas pelo runtime e pelo compilador JIT (just-in-time). Usando essa lista, o coletor de lixo cria um grafo que contém todos os objetos que podem ser acessados por meio das raízes.

Objetos que não estão no gráfico são inacessíveis a partir das raízes do aplicativo. O coletor de lixo considera o lixo dos objetos inacessíveis e libera a memória alocada para eles. Durante uma coleta, o coletor de lixo examina o heap gerenciado, procurando os blocos de espaço de endereço ocupados por objetos inacessíveis. Na medida em que descobre cada objeto inacessível, ele usa uma função de cópia de memória para compactar os objetos acessíveis na memória, liberando os blocos de espaços de endereço alocados para objetos inacessíveis. Uma vez que a memória dos objetos acessíveis tenha sido compactada, o coletor de lixo faz as correções necessárias no ponteiro de forma que as raízes do aplicativo apontem para os objetos em seus novos locais. Ele também posiciona o ponteiro do heap gerenciado após o último objeto acessível.

A memória é compactada somente se uma coleção descobre um número significativo de objetos inacessíveis. Se todos os objetos no heap gerenciado sobrevivem a uma coleta, não há necessidade de compactação de memória.

Para melhorar o desempenho, o runtime aloca memória para objetos grandes em um heap separado. O coletor de lixo automaticamente libera a memória para objetos grandes. No entanto, para evitar a movimentação de objetos grandes na memória, essa memória geralmente não é compactada.

## <a name="conditions-for-a-garbage-collection"></a>Condições para uma coleta de lixo

A coleta de lixo ocorre quando uma das seguintes condições é verdadeira:

- O sistema tem pouca memória física. Isso é detectado pela notificação de memória insuficiente do sistema operacional ou memória insuficiente, conforme indicado pelo host.

- A memória usada por objetos alocados no heap gerenciado ultrapassa um limite aceitável. Esse limite é ajustado continuamente enquanto o processo é executado.

- O método <xref:System.GC.Collect%2A?displayProperty=nameWithType> é chamado. Em quase todos os casos, você não precisa chamar esse método, pois o coletor de lixo é executado continuamente. Esse método é usado principalmente para situações exclusivas e testes.

## <a name="the-managed-heap"></a>O heap gerenciado

Depois que o coletor de lixo é inicializado pelo CLR, ele aloca um segmento da memória para armazenar e gerenciar objetos. Essa memória é chamada de heap gerenciado, em contraposição a um heap nativo no sistema operacional.

Há um heap gerenciado para cada processo gerenciado. Todos os threads no processo alocam memória para objetos no mesmo heap.

Para reservar memória, o coletor de lixo chama a função do Windows [VirtualAlloc](/windows/desktop/api/memoryapi/nf-memoryapi-virtualalloc) e reserva um segmento de memória por vez para aplicativos gerenciados. O coletor de lixo também reserva segmentos, conforme necessário, e libera segmentos de volta para o sistema operacional (depois de limpá-los de qualquer objeto) chamando a função [VirtualFree](/windows/desktop/api/memoryapi/nf-memoryapi-virtualfree) do Windows.

> [!IMPORTANT]
> O tamanho de segmentos alocados pelo coletor de lixo é específico da implementação e está sujeito a alterações a qualquer momento, incluindo em atualizações periódicas. Seu aplicativo nunca deve fazer suposições sobre o tamanho de um segmento em particular nem depender dele, tampouco deve tentar configurar a quantidade de memória disponível para alocações de segmento.

Quanto menos objetos alocados no heap, menos trabalho o coletor de lixo precisa fazer. Quando você aloca objetos, não usa valores arredondados que excedem suas necessidades, como alocar uma matriz de 32 bytes quando você precisa de apenas 15 bytes.

Quando uma coleta de lixo é disparada, o coletor de lixo recupera a memória ocupada por objetos mortos. O processo de recuperação compacta objetos vivos para que eles sejam movidos juntos e o espaço inativo é removido, tornando o heap menor. Isso garante que os objetos alocados juntos permaneçam juntos no heap gerenciado para preservar sua localidade.

O grau de intrusão (frequência e a duração) de coletas de lixo é o resultado do volume de alocações e da quantidade de memória restante no heap gerenciado.

O heap pode ser considerado como o acúmulo de dois heaps: o [heap de objetos grandes](large-object-heap.md) e o heap de objetos pequenos. A heap de objeto grande contém objetos que são 85.000 bytes e maiores, que geralmente são matrizes. É raro que um objeto de instância seja extremamente grande.

> [!TIP]
> Você pode [Configurar o tamanho do limite](../../core/run-time-config/garbage-collector.md#large-object-heap-threshold) para objetos a serem acessados no heap de objeto grande.

## <a name="generations"></a>Gerações

O algoritmo GC se baseia em várias considerações:

- É mais rápido compactar a memória para uma parte do heap gerenciado do que para todo o heap gerenciado.
- Os objetos mais recentes têm tempos de vida menores e objetos mais antigos têm tempos de vida mais longos.
- Os objetos mais recentes tendem a estar relacionados entre si e acessados pelo aplicativo ao mesmo tempo.

A coleta de lixo ocorre principalmente com a recuperação de objetos de curta duração. Para otimizar o desempenho do coletor de lixo, o heap gerenciado é dividido em três gerações, 0, 1 e 2, para que possa manipular objetos de vida longa e de curta duração separadamente. O coletor de lixo armazena novos objetos na geração 0. Os objetos criados no início no tempo de vida do aplicativo que sobrevivem a coleções são promovidos e armazenados nas gerações 1 e 2. Como é mais rápido compactar uma parte do heap gerenciado do que o heap inteiro, esse esquema permite que o coletor de lixo libere a memória em uma geração específica, em vez de liberar a memória para todo o heap gerenciado cada vez que executar uma coleção.

- **Geração 0**. Essa é a geração mais jovem e contém objetos de vida útil curta. Um exemplo de um objeto de vida útil curta é uma variável temporária. A coleta de lixo ocorre com mais frequência nessa geração.

  Os objetos alocados recentemente formam uma nova geração de objetos e são coleções de 0 geração implicitamente. No entanto, se forem objetos grandes, eles irão para o LOH (heap de objeto grande), que às vezes é chamado de *geração 3*. A geração 3 é uma geração física que é coletada logicamente como parte da geração 2.

  A maioria dos objetos são recuperados para a coleta de lixo na geração 0 e não sobrevivem à próxima geração.
  
  Se um aplicativo tentar criar um novo objeto quando a geração 0 estiver cheia, o coletor de lixo executará uma coleção em uma tentativa de liberar espaço de endereço para o objeto. O coletor de lixo inicia examinando os objetos na geração 0 em vez de todos os objetos no heap gerenciado. Uma coleção da geração 0 geralmente recupera memória suficiente para permitir que o aplicativo continue criando novos objetos.

- **Geração 1**. Essa geração contém objetos de vida útil curta e serve como um buffer entre objetos de vida útil curta e longa.

  Depois que o coletor de lixo executa uma coleção de geração 0, ele compacta a memória para os objetos acessíveis e promove-os para a geração 1. Como os objetos que sobrevivem a coleções tendem a ter tempos de vida mais longos, faz sentido promovê-los a uma geração mais alta. O coletor de lixo não precisa examinar novamente os objetos nas gerações 1 e 2 cada vez que executa uma coleção de geração 0.
  
  Se uma coleção de geração 0 não recuperar memória suficiente para que o aplicativo crie um novo objeto, o coletor de lixo poderá executar uma coleção de geração 1 e, em seguida, a geração 2. Os objetos na geração 1 que sobrevivem a coleções são promovidos para a geração 2.

- **Geração 2**. Essa geração contém objetos de vida útil longa. Um exemplo de um objeto de vida longa é um objeto em um aplicativo de servidor que contém dados estáticos que residem durante o processo.

  Os objetos na geração 2 que sobrevivem a uma coleção permanecem na geração 2 até que sejam determinados como inacessíveis em uma coleção futura.
  
  Objetos na heap de objeto grande (que às vezes é chamado de *geração 3*) também são coletados na geração 2.

Coletas de lixo ocorrem em gerações específicas conforme as condições permitirem. Coletar uma geração significa coletar objetos nessa geração e todas as suas gerações mais jovens. Uma coleta de lixo de geração 2 também é conhecida como uma coleta de lixo completa, porque recupera objetos em todas as gerações (ou seja, todos os objetos no heap gerenciado).

### <a name="survival-and-promotions"></a>Sobrevivência e promoções

Os objetos que não são recuperados em uma coleta de lixo são conhecidos como os sobreviventes e são promovidos para a próxima geração:

- Os objetos que sobrevivem a uma coleta de lixo de geração 0 são promovidos para a geração 1.
- Os objetos que sobrevivem a uma coleta de lixo de geração 1 são promovidos para a geração 2.
- Os objetos que sobrevivem a uma coleta de lixo de geração 2 permanecem na geração 2.

Quando o coletor de lixo detecta que a taxa de sobrevivência é alta em uma geração, ela aumenta o limite de alocações para essa geração. A próxima coleção Obtém um tamanho substancial de memória recuperada. O CLR balanceia continuamente duas prioridades: não permitir que o conjunto de trabalho de um aplicativo fique muito grande atrasando a coleta de lixo e não permitindo que a coleta de lixo seja executada com muita frequência.

### <a name="ephemeral-generations-and-segments"></a>Gerações e segmentos efêmeros

Como os objetos nas gerações 0 e 1 são de curta duração, essas gerações são conhecidas como as *gerações efêmeras*.

As gerações efêmeras são alocadas no segmento de memória que é conhecido como o segmento efêmero. Cada novo segmento adquirido pelo coletor de lixo torna-se o novo segmento efêmero e contém os objetos que sobreviveram a uma coleta de lixo da geração 0. O segmento efêmero antigo torna-se o novo segmento da geração 2.

O tamanho do segmento efêmero varia dependendo se um sistema for de 32 bits ou 64 bits e no tipo de coletor de lixo em execução ([estação de trabalho ou servidor GC](workstation-server-gc.md)). A tabela a seguir mostra os tamanhos padrão do segmento efêmero.

|Estação de trabalho/servidor GC|32 bits|64 bits|
|-|-------------|-------------|
|GC da estação de trabalho|16 MB|256 MB|
|GC do servidor|64 MB|4 GB|
|GC do Servidor com > 4 CPUs lógicas|32 MB|2 GB|
|GC do Servidor com > 8 CPUs lógicas|16 MB|1 GB|

O segmento efêmero pode incluir objetos da geração 2. Objetos da geração 2 podem usar vários segmentos (tantos quanto exigido pelo seu processo e permitido pela memória).

A quantidade de memória liberada de uma coleta de lixo efêmera é limitada ao tamanho do segmento efêmero. A quantidade de memória liberada é proporcional ao espaço que era ocupado pelos objetos inativos.

## <a name="what-happens-during-a-garbage-collection"></a>O que ocorre durante uma coleta de lixo

Uma coleta de lixo tem as seguintes fases:

- Uma fase de marcação que localiza todos os objetos vivos e cria uma lista desses objetos.

- Uma fase de relocação que atualiza as referências aos objetos que serão compactados.

- Uma fase de compactação que recupera o espaço ocupado por objetos inativos e compacta os objetos sobreviventes. A fase de compactação move objetos que sobreviveram a uma coleta de lixo em direção à extremidade mais antiga do segmento.

  Em virtude das coletas da geração 2 poderem ocupar vários segmentos, objetos que são promovidos para a geração 2 podem ser movidos para um segmento mais antigo. Tanto os sobreviventes da geração 1 quanto da geração 2 podem ser movidos para um segmento diferente, porque eles são promovidos para a geração 2.

  Normalmente, o LOH (heap de objeto grande) não é compactado, pois copiar objetos grandes impõe uma penalidade de desempenho. No entanto, no .NET Core e no .NET Framework 4.5.1 e posterior, você pode usar a <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode%2A?displayProperty=nameWithType> propriedade para compactar o heap de objeto grande sob demanda. Além disso, o LOH é compactado automaticamente quando um limite rígido é definido especificando:

  - Um limite de memória em um contêiner.
  - As opções de configuração de tempo de execução do [GCHeapHardLimit](../../core/run-time-config/garbage-collector.md#heap-limit) ou [GCHeapHardLimitPercent](../../core/run-time-config/garbage-collector.md#heap-limit-percent) .

O coletor de lixo usa as informações a seguir para determinar se os objetos estão vivos:

- **Raízes da pilha**. Variáveis de pilha fornecidas pelo compilador JIT (just-in-time) e movimentador de pilhas. As otimizações JIT podem aumentar ou diminuir as regiões do código dentro das quais as variáveis de pilha são relatadas ao coletor de lixo.

- **Identificadores de coleta de lixo**. Identificadores que apontam para objetos gerenciados e que podem ser alocados pelo código do usuário ou pelo Common Language Runtime.

- **Dados estáticos**. Objetos estáticos em domínios de aplicativo que podem fazer referência a outros objetos. Cada domínio de aplicativo controla seus objetos estáticos.

Antes de iniciar uma coleta de lixo, todos os threads gerenciados são suspensos, exceto o thread que disparou a coleta de lixo.

A ilustração a seguir mostra um thread que dispara uma coleta de lixo e faz com que outros threads sejam suspensos.

![Quando um thread dispara uma coleta de lixo](media/gc-triggered.png)

## <a name="unmanaged-resources"></a>Recursos não gerenciados

Para a maioria dos objetos que seu aplicativo cria, você pode confiar na coleta de lixo para executar automaticamente as tarefas de gerenciamento de memória necessárias. Entretanto, recursos não gerenciados requerem limpeza explícita. O tipo mais comum de recursos não gerenciados é um objeto que encapsula um recurso do sistema operacional, como um identificador de arquivo, um identificador de janela ou uma conexão de rede. Embora o coletor de lixo possa controlar o tempo de vida de um objeto gerenciado que encapsula um recurso não gerenciado, ele não tem conhecimento específico sobre como limpar o recurso.

Quando você cria um objeto que encapsula um recurso não gerenciado, é recomendável que você forneça o código necessário para limpar o recurso não gerenciado em um `Dispose` método público. Ao fornecer um método `Dispose`, você permite que usuários do seu objeto liberem, explicitamente, sua memória quando terminarem o objeto. Ao usar um objeto que encapsula um recurso não gerenciado, certifique-se de chamar `Dispose` conforme necessário.

Você também deve fornecer uma maneira para que seus recursos não gerenciados sejam liberados caso um consumidor de seu tipo se esqueça de chamar `Dispose` . Você pode usar um identificador seguro para encapsular o recurso não gerenciado ou substituir o <xref:System.Object.Finalize?displayProperty=nameWithType> método.

Para obter mais informações sobre como limpar recursos não gerenciados, consulte [limpar recursos não gerenciados](unmanaged.md).

## <a name="see-also"></a>Consulte também

- [Coleta de lixo de estação de trabalho ou de servidor](workstation-server-gc.md)
- [Coleta de lixo em segundo plano](background-gc.md)
- [Opções de configuração para GC](../../core/run-time-config/garbage-collector.md)
- [Coleta de lixo](index.md)
