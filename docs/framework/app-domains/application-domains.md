---
title: Domínios de aplicativo
description: Leia sobre domínios de aplicativo, que fornecem um limite de isolamento entre aplicativos para segurança, confiabilidade, controle de versão & descarregamento de assemblies no .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- process boundaries for isolation
- application isolation
- application domains, about
- common language runtime, application domains
- application domains
- runtime, application domains
- isolation between applications
- code, verification process
- verification testing code
ms.assetid: 113a8bbf-6875-4a72-a49d-ca2d92e19cc8
ms.openlocfilehash: d6accd11e33c0556fdd7596b2790f4787dce7ae1
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84903474"
---
# <a name="application-domains"></a>Domínios de aplicativo

Sistemas operacionais e ambientes em runtime normalmente fornecem alguma forma de isolamento entre aplicativos. Por exemplo, o Windows usa processos para isolar aplicativos. Esse isolamento é necessário para garantir que o código em execução em um aplicativo não possa afetar outros aplicativos não relacionados.  
  
 Domínios de aplicativos oferecem um limite de isolamento para segurança, confiabilidade e controle de versão, além de descarregar assemblies. Domínios de aplicativos normalmente são criados por hosts de tempo de execução, que são responsáveis pela inicialização do Common Language Runtime antes de um aplicativo ser executado.  
  
## <a name="the-benefits-of-isolating-applications"></a>Os benefícios do isolamento de aplicativos

 Historicamente, os limites de processo têm sido usados para isolar aplicativos em execução no mesmo computador. Cada aplicativo é carregado em um processo separado, que isola o aplicativo de outros aplicativos em execução no mesmo computador.  
  
 Os aplicativos são isolados porque endereços de memória são relativos a processos; um ponteiro de memória passado de um processo para outro não pode ser usado de maneira significativa no processo de destino. Além disso, você não pode fazer chamadas diretas entre dois processos. Em vez disso, você deve usar proxies, que fornecem um nível de indireção.  
  
 O código gerenciado deve ser passado por meio de um processo de verificação antes de ser executado (a menos que o administrador tenha concedido permissão para ignorar a verificação). O processo de verificação determina se o código pode tentar acessar endereços de memória inválidos ou realizar alguma outra ação que possa causar falha no processo no qual ele está sendo executado. Diz-se que o código que passa pelo teste de verificação é chamado de fortemente tipado. A capacidade de verificar o código como fortemente tipado permite que o Common Language Runtime forneça um ótimo nível de isolamento como o limite de processo, a um custo de desempenho muito menor.  
  
 Domínios de aplicativo oferecem uma unidade mais segura e versátil de processamento que o Common Language Runtime pode usar para fornecer isolamento entre aplicativos. Você pode executar vários domínios de aplicativo em um único processo com o mesmo nível de isolamento que existiria em processos separados, mas sem incorrer na sobrecarga adicional ao fazer chamadas cruzadas ou alternar processos. A capacidade de executar vários aplicativos em um único processo aumenta muito a escalabilidade do servidor.  
  
 Isolar aplicativos também é importante para a segurança do aplicativo. Por exemplo, você pode executar controles de vários aplicativos Web em um único processo de navegador de tal forma que os controles não possam acessar dados e recursos uns dos outros.  
  
 O isolamento fornecido pelos domínios de aplicativo possui os seguintes benefícios:  
  
- Falhas em um aplicativo não podem afetar outros aplicativos. Como um código fortemente tipado não pode causar falhas de memória, usar domínios de aplicativo garante que o código em execução em um domínio não possa afetar outros aplicativos no processo.  
  
- Aplicativos individuais podem ser interrompidos sem interromper o processo inteiro. Usar domínios de aplicativo permite que você descarregue o código em execução em um único aplicativo.  
  
    > [!NOTE]
    > Você não pode descarregar assemblies ou tipos individuais. Apenas um domínio completo pode ser descarregado.  
  
- O código em execução em um aplicativo não pode diretamente acessar o código ou os recursos de outro aplicativo. O Common Language Runtime impõe esse isolamento, impedindo chamadas diretas entre objetos em domínios de aplicativo diferentes. Objetos que passam entre domínios são copiados ou acessados pelo proxy. Se o objeto for copiado, a chamada para o objeto será local. Ou seja, tanto o chamador quanto o objeto referenciado estão no mesmo domínio de aplicativo. Se o objeto for acessado por meio de um proxy, a chamada para o objeto será remota. Nesse caso, o chamador e o objeto referenciado estão em domínios diferentes. Chamadas entre domínios usam a mesma infraestrutura de chamada remota entre dois processos ou entre dois computadores. Assim, os metadados para o objeto referenciado devem estar disponíveis para ambos os domínios de aplicativo para permitir que a chamada de método seja compilada corretamente por JIT. Se o domínio de chamada não tiver acesso aos metadados do objeto que está sendo chamado, a compilação poderá falhar com uma exceção do tipo <xref:System.IO.FileNotFoundException>. Para obter mais informações, confira [Objetos remotos](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/72x4h507(v=vs.100)). O mecanismo para determinar como objetos podem ser acessados em domínios é determinado pelo objeto. Para obter mais informações, consulte <xref:System.MarshalByRefObject?displayProperty=nameWithType>.  
  
- O escopo do comportamento do código é dado pelo aplicativo no qual ele é executado. Em outras palavras, o domínio do aplicativo fornece definições de configuração, como as políticas de versão do aplicativo, o local de qualquer assembly remoto acessado e informações sobre onde localizar assemblies carregados no domínio.  
  
- Permissões concedidas ao código podem ser controladas pelo domínio do aplicativo no qual o código está sendo executado.  
  
## <a name="application-domains-and-assemblies"></a>Domínios de aplicativo e assemblies

 Esta seção descreve o relacionamento entre domínios do aplicativo e assemblies. Você deve carregar um assembly em um domínio de aplicativo para executar o código que ele contém. Executar um aplicativo típico faz vários assemblies serem carregados em um domínio de aplicativo.  
  
 A maneira como um assembly é carregado determina se o seu código compilado por JIT (just-in-time) pode ser compartilhado por vários domínios de aplicativo no processo e se o assembly pode ser descarregado do processo.  
  
- Se um assembly for carregado em domínio neutro, todos os domínios de aplicativo que compartilharem o mesmo conjunto de concessões de segurança poderão compartilhar o mesmo código compilado por JIT, o que reduz a memória exigida pelo aplicativo. No entanto, o assembly jamais pode ser descarregado do processo.  
  
- Se um assembly não for carregado em domínio neutro, ele deverá ser compilado por JIT em cada domínio de aplicativo no qual ele for carregado. No entanto, o assembly pode ser descarregado do processo descarregando-se todos os domínios de aplicativo em que ele está carregado.  
  
 O host de runtime decidirá se é necessário carregar os assemblies como neutros com relação ao domínio quando ele carregar o runtime em um processo. Para aplicativos gerenciados, aplique o atributo <xref:System.LoaderOptimizationAttribute> ao método de ponto de entrada do processo e especifique um valor da enumeração <xref:System.LoaderOptimization> associada. Para aplicativos não gerenciados que hospedam o Common Language Runtime, especifique o sinalizador apropriado ao chamar o método [CorBindToRuntimeEx Function](../unmanaged-api/hosting/corbindtoruntimeex-function.md).  
  
 Existem três opções para carregar assemblies de domínio neutro:  
  
- <xref:System.LoaderOptimization.SingleDomain?displayProperty=nameWithType> não carrega assemblies como neutros em relação ao domínio, exceto Mscorlib, que é sempre carregado como neutro em relação ao domínio. Essa configuração é chamada de domínio único, porque ela normalmente é usada quando o host está executando apenas um único aplicativo no processo.

- <xref:System.LoaderOptimization.MultiDomain?displayProperty=nameWithType> carrega todos os assemblies como neutros em relação ao domínio. Use essa configuração quando houver vários domínios de aplicativo no processo, todos executando o mesmo código.

- <xref:System.LoaderOptimization.MultiDomainHost?displayProperty=nameWithType> carrega assemblies de nome forte como neutros em relação ao domínio, se eles e todas as suas dependências foram instalados na cache de assembly global. Outros assemblies são carregados e compilados por JIT separadamente para cada domínio de aplicativo em que são carregados e, assim, podem ser descarregados do processo. Use essa configuração ao executar mais de um aplicativo no mesmo processo, ou se você tiver uma combinação de assemblies compartilhados por vários domínios de aplicativos e assemblies que precisam ser descarregados do processo.
  
 Códigos compilados por JIT não podem ser compartilhados por assemblies carregados no contexto de carga, usando o método <xref:System.Reflection.Assembly.LoadFrom%2A> da classe <xref:System.Reflection.Assembly>, ou carregados com base em imagens usando sobrecargas do método <xref:System.Reflection.Assembly.Load%2A> que especificam matrizes de bytes.  
  
 Os assemblies que foram compilados para código nativo usando o [Ngen.exe (Gerador de Imagem Nativa)](../tools/ngen-exe-native-image-generator.md) poderão ser compartilhados entre domínios de aplicativo, se eles tiverem sido carregados como neutros em relação ao domínio na primeira vez em que eles forem carregados em um processo.  
  
 Códigos compilados por JIT para o assembly que contém o ponto de entrada do aplicativo só serão compartilhados se todas as suas dependências puderem ser compartilhadas.  
  
 Um assembly neutro em relação ao domínio pode ser compilado por JIT mais de uma vez. Por exemplo, quando os conjuntos de concessões de segurança de dois domínios de aplicativo forem diferentes, eles não poderão compartilhar o mesmo código compilado por JIT. No entanto, cada cópia do assembly compilado por JIT pode ser compartilhada com outros domínios de aplicativo que tenham o mesmo conjunto de concessões.  
  
 Ao decidir se deve carregar assemblies como neutros em relação ao domínio, você deve fazer uma escolha entre reduzir o uso de memória e outros fatores de desempenho.  
  
- O acesso a dados e métodos estáticos é mais lento para assemblies neutros em relação ao domínio devido à necessidade de isolamento dos assemblies. Cada domínio do aplicativo que acessa o assembly deve ter uma cópia separada dos dados estáticos, para evitar que referências a objetos em campos estáticos cruzem os limites do domínio. Como resultado, o runtime contém a lógica adicional para direcionar um chamador para a cópia apropriada de dados estáticos ou método. Essa lógica extra retarda a chamada.  
  
- Todas as dependências de um assembly deverão estar localizadas e carregadas quando o assembly for carregado como neutro em relação ao domínio, porque uma dependência que não puder ser carregada como neutra em relação ao domínio impedirá que o assembly seja carregado como neutro em relação ao domínio.  
  
## <a name="application-domains-and-threads"></a>Domínios do aplicativo e threads

 Um domínio de aplicativo forma um limite de isolamento para segurança, controle de versão, confiabilidade e descarregamento de código gerenciado. Um thread é o constructo do sistema operacional usado pelo Common Language Runtime para executar código. No tempo de execução, todos os códigos gerenciados são carregados em um domínio de aplicativo e são executados por um ou mais threads gerenciados.  
  
 Não há uma correlação um-para-um entre domínios de aplicativo e threads. Vários threads podem ser executados em um único domínio de aplicativo a qualquer momento e um determinado thread não está confinado a um único domínio de aplicativo. Ou seja, threads são livres para atravessar limites de domínio de aplicativo; um novo thread não é criado para cada domínio de aplicativo.  
  
 A qualquer momento, cada thread é executado em um domínio de aplicativo. Zero, um ou vários threads podem estar em execução em um determinado domínio de aplicativo. O runtime acompanha quais threads estão em execução em quais domínios do aplicativo. Você pode localizar o domínio no qual um thread está em execução a qualquer momento chamando o método <xref:System.Threading.Thread.GetDomain%2A?displayProperty=nameWithType>.

### <a name="application-domains-and-cultures"></a>Domínios do aplicativo e culturas

 A cultura, que é representada por um objeto <xref:System.Globalization.CultureInfo>, está associada a threads. Você pode obter a cultura associada ao thread em execução no momento usando a propriedade <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> e obter ou definir a cultura associada ao thread em execução no momento usando a propriedade <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType>. Se a cultura associada a um thread tiver sido definida explicitamente usando-se a propriedade <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType>, ela continuará sendo associada a esse thread quando ele cruzar os limites de domínio do aplicativo. Do contrário, a cultura associada ao thread em um dado momento é determinada pelo valor da propriedade <xref:System.Globalization.CultureInfo.DefaultThreadCurrentCulture%2A?displayProperty=nameWithType> no domínio do aplicativo no qual o thread está sendo executado:  
  
- Se o valor da propriedade não for `null`, a cultura retornada pela propriedade estará associada ao thread (e, assim, retornada pelas propriedades <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> e <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType>).  
  
- Se o valor da propriedade for `null`, a cultura do sistema atual estará associada ao thread.  
  
## <a name="programming-with-application-domains"></a>Programação com domínios do aplicativo

 Domínios de aplicativo costumam ser criados e manipulados programaticamente por hosts de runtime. Entretanto, às vezes, um aplicativo também pode preferir trabalhar com domínios de aplicativo. Por exemplo, um programa de aplicativo pode carregar um componente do aplicativo em um domínio para poder descarregar o domínio (e o componente) sem precisar interromper o aplicativo inteiro.  
  
 O <xref:System.AppDomain> é a interface programática para domínios de aplicativo. Essa classe inclui métodos para criar e descarregar domínios, para criar instâncias de tipos em domínios e registrar várias notificações como o descarregamento de domínio do aplicativo. A tabela a seguir lista os métodos <xref:System.AppDomain> mais usados.  
  
|Método AppDomain|Description|  
|----------------------|-----------------|  
|<xref:System.AppDomain.CreateDomain%2A>|Cria um novo domínio de aplicativo. É recomendável usar uma sobrecarga desse método que especifique um objeto <xref:System.AppDomainSetup>. Essa é a maneira preferencial para definir as propriedades de um novo domínio, como a base do aplicativo ou o diretório raiz do aplicativo; o local do arquivo de configuração do domínio e o caminho de pesquisa que o Common Language Runtime deve usar para carregar assemblies dentro do domínio.|  
|<xref:System.AppDomain.ExecuteAssembly%2A> e <xref:System.AppDomain.ExecuteAssemblyByName%2A>|Executa um assembly no domínio do aplicativo. Como esse é um método de instância, ele pode ser usado para executar código em outro domínio de aplicativo para o qual você tenha uma referência.|  
|<xref:System.AppDomain.CreateInstanceAndUnwrap%2A>|Cria uma instância de um tipo especificado no domínio de aplicativo e retorna um proxy. Use esse método para evitar carregar o assembly contendo o tipo criado no assembly da chamada.|  
|<xref:System.AppDomain.Unload%2A>|Executa um desligamento elegante do domínio. O domínio do aplicativo não é descarregado até que todos os threads em execução no domínio tenham sido interrompidos ou não estejam mais no domínio.|  
  
> [!NOTE]
> Como o Common Language Runtime não dá suporte à serialização de métodos globais, representantes não podem ser usados para executar métodos globais em outros domínios de aplicativo.  
  
 As interfaces não gerenciadas descritas na Especificação Hospedando Interfaces do Common Language Runtime também dão acesso a domínios de aplicativo. Hosts de runtime podem usar interfaces do código não gerenciado para criar e obter acesso aos domínios de aplicativo dentro de um processo.  
  
## <a name="the-complus_loaderoptimization-environment-variable"></a>A variável de ambiente COMPLUS_LoaderOptimization

 Uma variável de ambiente que define a política padrão de otimização do carregador de um aplicativo executável.  
  
### <a name="syntax"></a>Sintaxe  
  
```env  
COMPLUS_LoaderOptimization = 1  
```  
  
### <a name="remarks"></a>Comentários

 Um aplicativo típico carrega vários assemblies em um domínio de aplicativo antes que o código que eles contêm possa ser executado.  
  
 A maneira que o assembly é carregado determina se o seu código compilado por JIT (just-in-time) pode ser compartilhado por vários domínios de aplicativo no processo.  
  
- Se um assembly for carregado em domínio neutro, todos os domínios de aplicativo que compartilhem o mesmo conjunto de concessão de segurança poderão compartilhar o mesmo código compilado por JIT. Isso reduz a memória exigida pelo aplicativo.  
  
- Se um assembly não for carregado em domínio neutro, ele deverá ser compilado por JIT em cada domínio de aplicativo em que for carregado e o carregador não deverá compartilhar recursos internos entre domínios de aplicativo.  
  
 Quando definido como 1, o sinalizador de ambiente COMPLUS_LoaderOptimization força o host de runtime a carregar todos os assemblies de maneira de domínio não neutro, conhecido como SingleDomain. SingleDomain não carrega assemblies como neutros em relação ao domínio, exceto Mscorlib, que é sempre carregado como neutro em relação ao domínio. Essa configuração é chamada de domínio único, porque ela normalmente é usada quando o host está executando apenas um único aplicativo no processo.  
  
> [!CAUTION]
> O sinalizador de ambiente COMPLUS_LoaderOptimization foi projetado para ser usado em cenários de diagnóstico e de teste. Ter o sinalizador ativado pode causar uma grave lentidão e aumentar o uso de memória.  
  
### <a name="code-example"></a>Exemplo de código

 Para forçar todos os assemblies a não serem carregados como domínios neutros para o serviço IISADMIN, pode ser obtido acrescentando `COMPLUS_LoaderOptimization=1` ao valor de várias cadeia de caracteres de ambiente na chave HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\IISADMIN.  
  
```env  
Key = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\IISADMIN  
Name = Environment  
Type = REG_MULTI_SZ  
Value (to append) = COMPLUS_LoaderOptimization=1  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.AppDomain?displayProperty=nameWithType>
- <xref:System.MarshalByRefObject?displayProperty=nameWithType>
- [Programação com domínios do aplicativo e assemblies](index.md)
- [Usando domínios do aplicativo](use.md)
