---
title: Gerenciamento automático de memória
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- garbage collection, automatic memory management
- memory, allocating
- memory, automatic memory management
- memory, releasing
- common language runtime, automatic memory management
- automatic memory management
- managed heap
- runtime, automatic memory management
ms.assetid: d4850de5-fa63-4936-a250-5678d118acba
ms.openlocfilehash: a9b0e9a02d519eb18debe4249623df010e6f0e6d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84276186"
---
# <a name="automatic-memory-management"></a>Gerenciamento automático de memória
O gerenciamento automático de memória é um dos serviços que o Common Language Runtime fornece durante a [execução gerenciada](managed-execution-process.md). O coletor de lixo do Common Language Runtime gerencia a alocação e a liberação de memória para um aplicativo. Para desenvolvedores, isso significa que você não tem que escrever código para executar tarefas de gerenciamento de memória quando desenvolver aplicativos gerenciados. O gerenciamento automático de memória pode eliminar problemas comuns, como esquecer de liberar um objeto e causar um vazamento de memória ou tentar acessar a memória de um objeto que já tinha sido liberado. Esta seção descreve como o coletor de lixo aloca e libera memória.  
  
## <a name="allocating-memory"></a>Alocando memória  
 Quando você inicializa um novo processo, o runtime reserva uma região contígua de espaço de endereço para o processo. Esse espaço de endereço reservado é chamado de heap gerenciado. O heap gerenciado mantém um ponteiro para o endereço no qual o próximo objeto do heap será alocado. Inicialmente, esse ponteiro é definido como o endereço básico do heap gerenciado. Todos os [tipos de referência](base-types/common-type-system.md) são alocados no heap gerenciado. Quando um aplicativo cria o primeiro tipo de referência, a memória é alocada para o tipo no endereço base do heap gerenciado. Quando o aplicativo cria o próximo objeto, o coletor de lixo aloca memória para ele no espaço de endereço logo depois do primeiro objeto. Desde que exista espaço de endereço disponível, o coletor de lixo continua alocando espaço para novos objetos dessa maneira.  
  
 A alocação memória com base no heap gerenciado é mais rápida do que a alocação de memória não gerenciada. Como o runtime aloca memória para um objeto adicionando um valor a um ponteiro, ele é quase tão rápido quanto a alocação de memória com base na pilha. Além disso, como novos objetos que são alocados consecutivamente são armazenados contiguamente no heap gerenciado, um aplicativo pode acessar os objetos muito rapidamente.  
  
<a name="cpconautomaticmemorymanagementreleasingmemoryanchor1"></a>
## <a name="releasing-memory"></a>Liberando memória  
 O mecanismo de otimização do coletor de lixo determina o melhor momento para executar uma coleta com base nas alocações que estão sendo feitas. Quando o coletor de lixo executa uma coleta, ele libera a memória dos objetos que não estão mais sendo usados pelo aplicativo. Ele determina quais objetos não estão mais sendo usados examinando as raízes do aplicativo. Cada aplicativo tem um conjunto de raízes. Cada raiz refere-se a um objeto no heap gerenciado ou é definida como nula. As raízes de um aplicativo incluem campos estáticos, variáveis locais e parâmetros na pilha de um thread, além de registros de CPU. O coletor de lixo tem acesso à lista de raízes ativas que o [compilador JIT (just-in-time)](managed-execution-process.md) e o tempo de execução mantêm. Usando essa lista, ele examina as raízes de um aplicativo e, no processo, cria um gráfico que contém todos os objetos que possam ser alcançados nas raízes.  
  
 Objetos que não estão no gráfico são inacessíveis a partir das raízes do aplicativo. O coletor de lixo considera lixo os objetos inacessíveis e liberará a memória alocada para eles. Durante uma coleta, o coletor de lixo examina o heap gerenciado, procurando os blocos de espaço de endereço ocupados por objetos inacessíveis. Na medida em que descobre cada objeto inacessível, ele usa uma função de cópia de memória para compactar os objetos acessíveis na memória, liberando os blocos de espaços de endereço alocados para objetos inacessíveis. Uma vez que a memória dos objetos acessíveis tenha sido compactada, o coletor de lixo faz as correções necessárias no ponteiro de forma que as raízes do aplicativo apontem para os objetos em seus novos locais. Ele também posiciona o ponteiro do heap gerenciado após o último objeto acessível. Observe que memória é compactada somente se uma coleta descobre um número significativo de objetos inacessíveis. Se todos os objetos no heap gerenciado sobrevivem a uma coleta, não há necessidade de compactação de memória.  
  
 Para melhorar o desempenho, o runtime aloca memória para objetos grandes em um heap separado. O coletor de lixo automaticamente libera a memória para objetos grandes. No entanto, para evitar mover objetos grandes na memória, essa memória não é compactada.  
  
## <a name="generations-and-performance"></a>Gerações e desempenho  
 Para otimizar o desempenho do coletor de lixo, o heap gerenciado é dividido em três gerações: 0, 1 e 2. O algoritmo da coleta de lixo do runtime é baseado em várias generalizações que a indústria de software de computador descobriu serem verdadeiras experimentando os esquemas da coleta de lixo. Primeiro, é mais rápido compactar a memória para uma parte do heap gerenciado do que para o heap gerenciado inteiro. Em segundo lugar, objetos mais recentes terão vidas úteis menores e objetos mais antigos objetos terão vidas úteis maiores. Finalmente, objetos mais recentes tendem a se relacionar e serem acessados pelo aplicativo aproximadamente ao mesmo tempo.  
  
 O coletor de lixo do runtime armazena novos objetos na geração 0. Os objetos criados no início no tempo de vida do aplicativo que sobrevivem a coleções são promovidos e armazenados nas gerações 1 e 2. O processo de promoção do objeto será descrito mais adiante neste tópico. Como é mais rápido compactar uma parte do heap gerenciado do que o heap inteiro, esse esquema permite que o coletor de lixo libere a memória em uma geração específica em vez liberar a memória para toda a memória gerenciada a cada vez que ele executa uma coleta.  
  
 Na verdade, o coletor de lixo executa uma coleta quando a geração 0 está cheia. Se um aplicativo tentar criar um novo objeto quando a geração 0 está cheia, o coletor de lixo descobre que não existe nenhum espaço de endereço restante na geração 0 para alocar para o objeto. O coletor de lixo executa uma coleta em uma tentativa de liberar espaço de endereço na geração 0 para o objeto. O coletor de lixo inicia examinando os objetos na geração 0 em vez de todos os objetos no heap gerenciado. Isso é a abordagem mais eficiente, porque novos objetos costumam ter tempos de vida curtos e é esperado que muitos dos objetos na geração 0 não estejam mais em uso pelo aplicativo quando uma coleta é executada. Além disso, uma única coleta de geração 0 normalmente recupera memória suficiente para permitir ao aplicativo continuar criando novos objetos.  
  
 Após o coletor de lixo executar uma coleta de geração 0, ele compacta a memória para os objetos acessíveis conforme explicado anteriormente neste tópico em [Liberando memória](#cpconautomaticmemorymanagementreleasingmemoryanchor1). Assim, o coletor de lixo promove esses objetos e considera isso parte da geração 1 do heap gerenciado. Como os objetos que sobrevivem a coleções tendem a ter tempos de vida mais longos, faz sentido promovê-los a uma geração mais alta. Como resultado, o coletor de lixo não tem que reexaminar os objetos em gerações 1 e 2 sempre que executa uma coleta de geração 0.  
  
 Depois que o coletor de lixo executa sua primeira coleta de geração 0 e promove os objetos atingíveis para a geração 1, ele considera o restante do heap gerenciado a geração 0. Ele continua alocando a memória para novos objetos na geração 0 até que a geração 0 esteja completa e seja necessário executar outra coleta. Nesse ponto, o mecanismo de otimização do coletor de lixo determina se é necessário examinar os objetos em gerações mais antigas. Por exemplo, se uma coleta de geração 0 não recupera memória suficiente para que o aplicativo conclua com êxito a sua tentativa de criar um novo objeto, o coletor de lixo pode executar uma coleta de geração 1 e, então, de geração 2. Se isso não recuperar memória suficiente, o coletor de lixo poderá executar uma coleta de gerações 2, 1 e 0. Depois de cada coleta, o coletor de lixo compactará os objetos atingíveis na geração 0 e os promoverá para a geração 1. Os objetos na geração 1 que sobrevivem a coleções são promovidos para a geração 2. Como o coletor de lixo só dá suporte a três gerações, os objetos na geração 2 que sobrevivem a uma coleta permanecem na geração 2 até serem considerados inacessíveis em uma coleta futura.  
  
## <a name="releasing-memory-for-unmanaged-resources"></a>Liberando memória para recursos não gerenciados  
 Para a maioria dos objetos que seu aplicativo cria, você pode confiar no coletor de lixo para executar automaticamente as tarefas de gerenciamento de memória necessárias. Entretanto, recursos não gerenciados requerem limpeza explícita. O tipo mais comum de recursos não gerenciados é um objeto que encapsula um recurso do sistema operacional, como um identificador de arquivo, um identificador de janela ou uma conexão de rede. Embora o coletor de lixo seja capaz de acompanhar o tempo de vida de um objeto gerenciado que encapsule um recurso não gerenciado, ele não tem conhecimento específico sobre como limpar o recurso. Quando você cria um objeto que encapsula um recurso não gerenciado, é recomendável fornecer o código necessário para limpar o recurso não gerenciado em um método público **Dispose**. Ao fornecer um método **Dispose**, você permite que usuários do seu objeto liberem, explicitamente, sua memória quando terminarem com o objeto. Quando usa um objeto que encapsula um recurso não gerenciado, você deve estar ciente de **Dispose** e chamá-lo conforme necessário. Para obter mais informações sobre a limpeza de recursos não gerenciados e um exemplo de um padrão de design para implementar **Dispose**, consulte [Coleta de lixo](garbage-collection/index.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.GC>
- [Coleta de lixo](garbage-collection/index.md)
- [Processo de execução gerenciada](managed-execution-process.md)
