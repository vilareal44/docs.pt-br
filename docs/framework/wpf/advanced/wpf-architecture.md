---
title: Arquitetura
description: Saiba mais sobre a hierarquia de classe de Windows Presentation Foundation, incluindo a maioria dos principais subsistemas e como eles interagem.
ms.date: 03/30/2017
helpviewer_keywords:
- properties [WPF], attached
- attached properties [WPF]
- architecture [WPF]
- unmanaged components [WPF]
- affinity thread [WPF]
- Storyboards [WPF]
- milcore [WPF]
- components [WPF], unmanaged
- painter's algorithm
- interfaces [WPF], INotifyPropertyChange
- CommandBindings [WPF]
- data templates [WPF]
- thread [WPF], affinity
ms.assetid: 8579c10b-76ab-4c52-9691-195ce02333c8
ms.openlocfilehash: a67709283b4b664e7b9ca0dced9154a43fcae8a5
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166317"
---
# <a name="wpf-architecture"></a>Arquitetura do WPF
Este tópico fornece um tour guiado pela hierarquia de classes do Windows Presentation Foundation (WPF). Ele cobre a maioria dos principais subsistemas do WPF e descreve como eles interagem. Ele também detalha algumas das opções feitas pelos arquitetos do WPF.  

<a name="System_Object"></a>
## <a name="systemobject"></a>System.Object  
 O modelo de programação WPF primário é exposto por meio de código gerenciado. No início da fase de design do WPF, havia um número de debates sobre onde a linha deve ser desenhada entre os componentes gerenciados do sistema e os não gerenciados. O CLR fornece vários recursos que tornam o desenvolvimento mais produtivo e robusto (incluindo gerenciamento de memória, tratamento de erros, Common Type System, etc.), mas eles chegam a um custo.  
  
 Os principais componentes do WPF são ilustrados na figura abaixo. As seções vermelhas do diagrama (PresentationFramework, PresentationCore e milcore) são as principais partes do código do WPF. Dessas, somente uma é um componente não gerenciado – milcore. O milcore é escrito em código não gerenciado a fim de habilitar a integração estreita com o DirectX. Todas as exibições no WPF são feitas por meio do mecanismo do DirectX, permitindo uma renderização de hardware e software eficiente. O WPF também exigiu um controle fino sobre a memória e a execução. O mecanismo de composição no milcore é extremamente sensível ao desempenho e é necessário desistir de muitas vantagens do CLR para obter desempenho.  
  
 ![A posição do WPF dentro do .NET Framework.](./media/wpf-architect1.PNG "wpf_architect1")  
  
 A comunicação entre as partes gerenciadas e não gerenciadas do WPF é discutida posteriormente neste tópico. O restante do modelo de programação gerenciado é descrito abaixo.  
  
<a name="System_Threading_DispatcherObject"></a>
## <a name="systemthreadingdispatcherobject"></a>System.Threading.DispatcherObject  
 A maioria dos objetos no WPF deriva de <xref:System.Windows.Threading.DispatcherObject> , que fornece as construções básicas para lidar com a simultaneidade e Threading. O WPF é baseado em um sistema de mensagens implementado pelo Dispatcher. Isso funciona muito bem como o familiar bombeamento de mensagens Win32; na verdade, o Dispatcher do WPF usa mensagens user32 para executar chamadas entre threads.  
  
 Há realmente dois conceitos principais a serem compreendidos ao discutir a simultaneidade no WPF – a afinidade de Dispatcher e thread.  
  
 Durante a fase de design do WPF, o objetivo era mudar para um único thread de execução, mas um modelo "relacionados" não thread. A afinidade de thread ocorre quando um componente usa a identidade do thread em execução para armazenar algum tipo de estado. A forma mais comum disso é usar o armazenamento local de thread (TLS) para armazenar o estado. Afinidade de threads requer que cada thread lógica de execução pertença a apenas um thread físico no sistema operacional, que pode passar a fazer uso intensivo de memória. No final, o modelo de threading do WPF foi mantido em sincronização com o modelo de threading User32 existente, de execução em thread único com afinidade de thread. O principal motivo para isso era a interoperabilidade – sistemas como o OLE 2,0, a área de transferência e o Internet Explorer todos exigem a execução do STA (afinidade de thread único).  
  
 Considerando que você tem objetos com threading STA, você precisa de uma maneira para se comunicar entre threads e validar que você está no thread correto. Eis aí a função do dispatcher. O dispatcher é um sistema básico de despacho de mensagens, com várias filas priorizadas. Exemplos de mensagens incluem notificações de entrada bruta (o mouse foi movido), funções de estrutura (layout) ou comandos do usuário (executar este método). Derivando de <xref:System.Windows.Threading.DispatcherObject> , você cria um objeto CLR que tem comportamento STA e receberá um ponteiro para um Dispatcher no momento da criação.  
  
<a name="System_Windows_DependencyObject"></a>
## <a name="systemwindowsdependencyobject"></a>System.Windows.DependencyObject  
 Uma das filosofias de arquitetura primárias usadas na criação do WPF foi uma preferência para propriedades de métodos ou eventos. Propriedades são declarativas e permitem que você especifique mais facilmente a intenção em vez de ação. Isso também deu suporte para um sistema controlado por modelo ou por dados para exibição do conteúdo de interface do usuário. Essa filosofia tinha o efeito desejado de criar mais propriedades do que o número ao qual você era capaz de se associar, de modo a controlar melhor o comportamento de um aplicativo.  
  
 Para ter mais sistemas controlados por propriedades, um sistema de propriedades mais avançado do que o que o CLR fornece era necessário. Um exemplo simples desta sofisticação é encontrado nas notificações de alteração. Para habilitar a associação bidirecional, você precisa que ambos os lados da associação deem suporte à notificação de alteração. Para ter comportamento ligado aos valores de propriedade, você precisa ser notificado quando o valor da propriedade é alterado. O Microsoft .NET Framework tem uma interface, **INotifyPropertyChange**, que permite que um objeto publique notificações de alteração, no entanto, é opcional.  
  
 O WPF fornece um sistema de propriedades mais avançado, derivado do <xref:System.Windows.DependencyObject> tipo. O sistema de propriedades é realmente um sistema de propriedade de "dependência", que controla as dependências entre expressões de propriedade e revalida automaticamente valores de propriedade quando as dependências são alteradas. Por exemplo, se você tiver uma propriedade que herda (como <xref:System.Windows.Controls.Control.FontSize%2A> ), o sistema será atualizado automaticamente se a propriedade for alterada em um pai de um elemento que herda o valor.  
  
 A base do sistema de propriedades do WPF é o conceito de uma expressão de propriedade. Nesta primeira versão do WPF, o sistema de expressão de propriedade é fechado e as expressões são todas fornecidas como parte da estrutura. As expressões são o motivo pelo qual o sistema de propriedades não tem associação de dados, definição de estilo ou herança fixados no código, mas sim fornecidos por camadas posteriores dentro do framework.  
  
 O sistema de propriedades também fornece armazenamento esparso de valores de propriedade. Já que os objetos podem ter dezenas (se não centenas) de propriedades e a maioria dos valores estão em seu estado padrão (herdado, definido por estilos, etc.), nem toda instância de um objeto precisa ter o peso total de todas as propriedades definidas nele.  
  
 O último novo recurso do sistema de propriedades é o conceito de propriedades anexadas. Os elementos do WPF são criados com base no princípio de composição e reutilização de componentes. Geralmente, é o caso de algum elemento contendo (como um <xref:System.Windows.Controls.Grid> elemento de layout) precisar de dados adicionais em elementos filho para controlar seu comportamento (como as informações de linha/coluna). Em vez de associar todas essas propriedades com cada elemento, qualquer objeto tem permissão para fornecer definições de propriedade para qualquer outro objeto. Isso é semelhante aos recursos "expando" do JavaScript.  
  
<a name="System_Windows_Media_Visual"></a>
## <a name="systemwindowsmediavisual"></a>System.Windows.Media.Visual  
 Com um sistema definido, a próxima etapa é fazer com que pixels sejam desenhados na tela. A <xref:System.Windows.Media.Visual> classe fornece para criar uma árvore de objetos visuais, cada um opcionalmente contendo instruções de desenho e metadados sobre como renderizar essas instruções (recorte, transformação, etc.). <xref:System.Windows.Media.Visual>o foi projetado para ser extremamente leve e flexível, portanto, a maioria dos recursos não tem exposição à API pública e depende muito das funções de retorno de chamada protegidas.  
  
 <xref:System.Windows.Media.Visual>é realmente o ponto de entrada para o sistema de composição do WPF. <xref:System.Windows.Media.Visual>é o ponto de conexão entre esses dois subsistemas, a API gerenciada e o milcore não gerenciado.  
  
 O WPF exibe dados atravessando as estruturas de dados não gerenciadas gerenciadas pelo milcore. Essas estruturas, chamadas nós de composição, representam uma árvore de exibição hierárquica com instruções de renderização em cada nó. Essa árvore, ilustrada no lado direito da figura abaixo, só é acessível por meio de um protocolo de mensagens.  
  
 Ao programar o WPF, você cria <xref:System.Windows.Media.Visual> elementos e tipos derivados, que se comunicam internamente à árvore de composição por meio desse protocolo de mensagens. Cada <xref:System.Windows.Media.Visual> no WPF pode criar um, nenhum ou vários nós de composição.  
  
 ![A árvore Visual Windows Presentation Foundation.](./media/wpf-architecture2.PNG "wpf_architecture2")  
  
 Há um detalhe arquitetural muito importante a observar aqui – toda a árvore de elementos visuais e instruções de desenho é armazenada em cache. Em termos gráficos, o WPF usa um sistema de renderização retido. Isso permite ao sistema redesenhar com altas taxas de atualização sem o bloqueio do sistema de composição em retornos de chamada para o código do usuário. Isso ajuda a impedir o surgimento de um aplicativo que não responde.  
  
 Outro detalhe importante que não é realmente perceptível no diagrama é o modo como o sistema realmente executa a composição.  
  
 No User32 e no GDI, o sistema funciona em um sistema de recorte de modo imediato. Quando um componente precisa ser renderizado, o sistema estabelece os limites de recorte fora dos quais o componente não tem permissão para tocar nos pixels e, em seguida, é solicitado ao componente que ele pinte os pixels dentro dessa caixa. Este sistema funciona muito bem em sistemas com restrição de memória porque quando algo é alterado, você só tem que tocar o componente afetado – não há jamais dois componentes que contribuam para a cor de um único pixel.  
  
 O WPF usa um modelo de pintura "algoritmo do pintor". Isso significa que, em vez de cortar cada componente, é solicitado a cada componente que renderize da parte de trás da exibição para a parte da frente. Isso permite que cada componente pinte sobre a exibição do componente anterior. A vantagem desse modelo é que você pode ter formas complexas, parcialmente transparentes. Com o hardware de gráficos moderno de hoje, esse modelo é relativamente rápido (que não era o caso quando user32/GDI foram criados).  
  
 Como mencionado anteriormente, uma filosofia principal do WPF é mudar para um modelo de programação mais declarativo, "centrado em Propriedade". No sistema visual, isso aparece em alguns locais interessantes.  
  
 Primeiro, se você pensar sobre o sistema gráfico de modo retido, isso estará realmente se movendo de um modelo de tipo DrawLine/DrawLine imperativo para um modelo orientado a dados – new Line()/new Line(). A mudança para a renderização controlada por dados permite que operações complexas nas instruções de desenho sejam expressas usando propriedades. Os tipos derivados de <xref:System.Windows.Media.Drawing> são efetivamente o modelo de objeto para renderização.  
  
 Em segundo lugar, se você avaliar o sistema de animação, verá que ele é quase completamente declarativo. Em vez de exigir que um desenvolvedor calcule o próximo local, ou a próxima cor, você pode expressar animações como um conjunto de propriedades em um objeto de animação. Essas animações podem expressar a intenção do desenvolvedor ou designer (mover esse botão daqui para lá em 5 segundos), e o sistema pode determinar a maneira mais eficiente de fazer isso.  
  
<a name="System_Windows_UIElement"></a>
## <a name="systemwindowsuielement"></a>System.Windows.UIElement  
 <xref:System.Windows.UIElement>define subsistemas principais, incluindo layout, entrada e eventos.  
  
 Layout é um conceito fundamental no WPF. Em muitos sistemas, há um conjunto fixo de modelos de layout (o HTML dá suporte a três modelos de layout; fluxo, absoluto e tabelas) ou nenhum modelo de layout (o User32 dá suporte somente a posicionamento absoluto). O WPF começou com a suposição de que desenvolvedores e designers queriam um modelo de layout extensível e flexível, que poderia ser orientado por valores de propriedade em vez de lógica imperativa. No <xref:System.Windows.UIElement> nível, o contrato básico de layout é introduzido – um modelo de duas fases com <xref:System.Windows.UIElement.Measure%2A> e <xref:System.Windows.UIElement.Arrange%2A> passa.  
  
 <xref:System.Windows.UIElement.Measure%2A>permite que um componente determine a quantidade de tamanho que gostaria de tomar. Essa é uma fase separada do <xref:System.Windows.UIElement.Arrange%2A> porque há muitas situações em que um elemento pai solicitará que um filho meça várias vezes para determinar sua posição e tamanho ideais. O fato de que elementos pai pedem elementos filho para medir demonstra outra filosofia importante do WPF – tamanho para conteúdo. Todos os controles no WPF dão suporte à capacidade de dimensionar para o tamanho natural de seu conteúdo. Isso torna a localização muito mais fácil e permite layout dinâmico dos elementos conforme as coisas são redimensionadas. A <xref:System.Windows.UIElement.Arrange%2A> fase permite que um pai Posicione e determine o tamanho final de cada filho.  
  
 Muito tempo, muitas vezes, é gasto falando sobre o lado de saída do WPF – <xref:System.Windows.Media.Visual> e objetos relacionados. No entanto, também há um tremendo montante de inovação no lado de entrada. Provavelmente, a alteração mais fundamental no modelo de entrada para o WPF é o modelo consistente pelo qual os eventos de entrada são roteados pelo sistema.  
  
 A entrada se origina como um sinal em um driver de dispositivo de modo kernel e é roteada para o processo e thread corretos por um processo intrincado, que envolve o kernel do Windows e o User32. Depois que a mensagem user32 correspondente à entrada é roteada para o WPF, ela é convertida em uma mensagem de entrada bruta do WPF e enviada ao Dispatcher. O WPF permite que eventos de entrada brutos sejam convertidos em vários eventos reais, permitindo que recursos como "MouseEnter" sejam implementados em um nível baixo do sistema com entrega garantida.  
  
 Cada evento de entrada é convertido em pelo menos dois eventos – um evento de "visualização" e o evento real. Todos os eventos no WPF têm uma noção de roteamento por meio da árvore de elementos. Os eventos são considerados "bolha" se percorrerem de um destino na árvore até a raiz e forem considerados "túnel" se começarem na raiz e percorrerem para um destino. Eventos de visualização de entrada são roteados por túnel, permitindo que qualquer elemento na árvore tenha uma oportunidade de filtrar ou executar uma ação no evento. Os eventos regulares (não visualização) são então roteados por propagação do destino até a raiz.  
  
 Essa divisão entre as fases de túnel e de propagação torna a implementação de recursos como aceleradores de teclado funcionar de forma consistente em um mundo composto. No User32, você poderia implementar aceleradores de teclado tendo uma única tabela global contendo todos os aceleradores aos quais que você quisesse dar suporte (Ctrl + N mapeando para "New"). No dispatcher para seu aplicativo, você chamaria **TranslateAccelerator**, que rastrearia as mensagens de entrada no User32 e determinaria se havia correspondência entre alguma delas e um acelerador registrado. No WPF, isso não funcionaria porque o sistema é totalmente "composto" – qualquer elemento pode manipular e usar qualquer acelerador de teclado. Esse modelo de duas fases para a entrada permite que os componentes implementem seu próprio "TranslateAccelerator".  
  
 Para levar isso um pouco além, <xref:System.Windows.UIElement> também apresenta a noção de CommandBindings. O sistema de comandos do WPF permite que os desenvolvedores definam a funcionalidade em termos de um ponto de extremidade de comando – algo que implementa <xref:System.Windows.Input.ICommand> . Associações de comando permitem a um elemento definir um mapeamento entre um gesto de entrada (Ctrl + N) e um comando (New). Tanto os gestos de entrada quanto as definições de comando são extensíveis e podem ser ligados juntos durante o tempo de uso. Isso torna trivial, por exemplo, permitir que um usuário final personalize as associações de teclas que deseja usar em um aplicativo.  
  
 Para esse ponto no tópico, os recursos "principais" do WPF – recursos implementados no assembly PresentationCore, têm sido o foco. Ao criar o WPF, uma separação clara entre as partes fundamentais (como o contrato de layout com **medida** e **organização**) e as partes da estrutura (como a implementação de um layout específico como <xref:System.Windows.Controls.Grid> ) era o resultado desejado. A meta era fornecer um ponto de extensibilidade baixo na pilha que permitisse aos desenvolvedores externos criar suas próprias estruturas, se necessário.  
  
<a name="System_Windows_FrameworkElement"></a>
## <a name="systemwindowsframeworkelement"></a>System.Windows.FrameworkElement  
 <xref:System.Windows.FrameworkElement>pode ser examinado de duas maneiras diferentes. Ele apresenta um conjunto de políticas e personalizações nos subsistemas introduzidos em camadas inferiores do WPF. Ele também introduz um conjunto de novos subsistemas.  
  
 A política principal introduzida pelo <xref:System.Windows.FrameworkElement> é o layout do aplicativo. <xref:System.Windows.FrameworkElement>baseia-se no contrato de layout básico introduzido pelo <xref:System.Windows.UIElement> e adiciona a noção de um "slot" de layout que torna mais fácil para os autores de layout ter um conjunto consistente de semânticas de layout controlado por propriedade. Propriedades como <xref:System.Windows.FrameworkElement.HorizontalAlignment%2A> , <xref:System.Windows.FrameworkElement.VerticalAlignment%2A> , <xref:System.Windows.FrameworkElement.MinWidth%2A> e <xref:System.Windows.FrameworkElement.Margin%2A> (para citar alguns) fornecem todos os componentes derivados do <xref:System.Windows.FrameworkElement> comportamento consistente dentro dos contêineres de layout.  
  
 <xref:System.Windows.FrameworkElement>também fornece uma exposição mais fácil da API a muitos recursos encontrados nas camadas principais do WPF. Por exemplo, <xref:System.Windows.FrameworkElement> fornece acesso direto à animação por meio do <xref:System.Windows.FrameworkElement.BeginStoryboard%2A> método. Um <xref:System.Windows.Media.Animation.Storyboard> fornece uma maneira de gerar um script de várias animações em relação a um conjunto de propriedades.  
  
 As duas coisas mais importantes que o <xref:System.Windows.FrameworkElement> introduz são vinculação de dados e estilos.  
  
 O subsistema de associação de dados no WPF deve ser relativamente familiar para qualquer pessoa que tenha usado Windows Forms ou ASP.NET para criar um aplicativo [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] . Em cada um desses sistemas, há uma maneira simples de expressar se você deseja que uma ou mais propriedades de um determinado elemento seja associada a uma parte dos dados. O WPF tem suporte completo para associação de propriedade, transformação e Associação de lista.  
  
 Um dos recursos mais interessantes da vinculação de dados no WPF é a introdução dos modelos de dados. Modelos de dados permitem que você especifique declarativamente como uma parte dos dados deve ser visualizada. Em vez de criar uma interface do usuário personalizada que pode ser associada a dados, você pode inverter o problema e permitir que os dados determinem a exibição que será criada.  
  
 Definição de estilo é na verdade uma forma leve de associação de dados. Usando estilização, você pode associar um conjunto de propriedades de uma definição compartilhada a uma ou mais instâncias de um elemento. Os estilos são aplicados a um elemento por referência explícita (definindo a <xref:System.Windows.FrameworkElement.Style%2A> Propriedade) ou implicitamente associando um estilo ao tipo CLR do elemento.  
  
<a name="System_Windows_Controls_Control"></a>
## <a name="systemwindowscontrolscontrol"></a>System.Windows.Controls.Control  
 O recurso mais significativo do controle é a modelagem. Se você pensar sobre o sistema de composição do WPF como um sistema de renderização de modo retido, a modelagem permitirá que um controle descreva sua renderização de maneira declarativa e parametrizada. R <xref:System.Windows.Controls.ControlTemplate> não é nada mais do que um script para criar um conjunto de elementos filho, com associações a Propriedades oferecidas pelo controle.  
  
 <xref:System.Windows.Controls.Control>fornece um conjunto de propriedades de ações,,, <xref:System.Windows.Controls.Control.Foreground%2A> <xref:System.Windows.Controls.Control.Background%2A> <xref:System.Windows.Controls.Control.Padding%2A> , para citar alguns, que os autores de modelo podem usar para personalizar a exibição de um controle. A implementação de um controle oferece um modelo de dados e um modelo de interação. O modelo de interação define um conjunto de comandos (como Close para uma janela) e associações para gestos de entrada (como clicar no X vermelho no canto superior da janela). O modelo de dados fornece um conjunto de propriedades para personalizar o modelo de interação ou então personalizar a exibição (determinado pelo modelo).  
  
 Essa divisão entre o modelo de dados (propriedades), o modelo de interação (comandos e eventos) e o modelo de exibição (modelos) permite a completa personalização da aparência e comportamento de um controle.  
  
 Um aspecto comum do modelo de dados dos controles é o modelo de conteúdo. Se você olhar um controle como <xref:System.Windows.Controls.Button> , verá que ele tem uma propriedade chamada "content" do tipo <xref:System.Object> . Em Windows Forms e ASP.NET, essa propriedade normalmente seria uma cadeia de caracteres – no entanto, isso limita o tipo de conteúdo que você pode colocar em um botão. O conteúdo de um botão pode ser uma cadeia de caracteres simples, um objeto de dados complexo ou uma árvore de elementos inteira. No caso de um objeto de dados, o modelo de dados é usado para construir uma exibição.  
  
<a name="Summary"></a>
## <a name="summary"></a>Resumo  
 O WPF foi projetado para permitir que você crie sistemas de apresentação dinâmicos e controlados por dados. Cada parte do sistema foi projetada para criar objetos através de conjuntos de propriedades que controlam o comportamento. A associação de dados é uma parte fundamental do sistema e está integrada em todas as camadas.  
  
 Aplicativos tradicionais criam uma exibição e, em seguida, associam a alguns dados. No WPF, tudo sobre o controle, todos os aspectos da exibição, é gerado por algum tipo de associação de dados. O texto encontrado em um botão é exibido por meio da criação de um controle composto dentro do botão e da associação de sua exibição à propriedade de conteúdo do botão.  
  
 Quando você começa a desenvolver aplicativos baseados em WPF, ele deve se sentir muito familiar. Você pode definir propriedades, usar objetos e associar dados da mesma maneira que você pode usar Windows Forms ou ASP.NET. Com uma investigação mais profunda sobre a arquitetura do WPF, você descobrirá que a possibilidade existe para criar aplicativos muito mais ricos que tratem fundamentalmente os dados como o principal driver do aplicativo.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Media.Visual>
- <xref:System.Windows.UIElement>
- <xref:System.Windows.Input.ICommand>
- <xref:System.Windows.FrameworkElement>
- <xref:System.Windows.Threading.DispatcherObject>
- <xref:System.Windows.Input.CommandBinding>
- <xref:System.Windows.Controls.Control>
- [Visão geral da ligação de dados](../../../desktop-wpf/data/data-binding-overview.md)
- [Layout](layout.md)
- [Visão geral da animação](../graphics-multimedia/animation-overview.md)
