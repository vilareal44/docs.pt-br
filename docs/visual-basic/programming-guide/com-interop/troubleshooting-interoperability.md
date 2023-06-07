---
title: Solução de problemas de Interoperabilidade
ms.date: 07/20/2015
helpviewer_keywords:
- interop, deploying assemblies
- assemblies [Visual Basic]
- interop, installing assemblies that share components
- COM objects, troubleshooting
- interop, sharing components
- troubleshooting interoperability [Visual Basic]
- interoperability, troubleshooting
- COM interop [Visual Basic], troubleshooting
- assemblies [Visual Basic], deploying
- troubleshooting Visual Basic, interoperability
- interop assemblies
- interoperability, sharing components
- shared components, using with assemblies
ms.assetid: b324cc1e-b03c-4f39-aea6-6a6d5bfd0e37
ms.openlocfilehash: 0890bac80396feaf37d4ba917c1e3fafb8579981
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396760"
---
# <a name="troubleshooting-interoperability-visual-basic"></a>Solucionando problemas de interoperabilidade (Visual Basic)
Ao interoperar entre COM e o código gerenciado do .NET Framework, você pode encontrar um ou mais dos seguintes problemas comuns.  
  
## <a name="interop-marshaling"></a><a name="vbconinteroperabilitymarshalinganchor1"></a>Marshaling de interoperabilidade  
 Às vezes, talvez seja necessário usar tipos de dados que não fazem parte do .NET Framework. Os assemblies de interoperabilidade lidam com a maior parte do trabalho para objetos COM, mas você pode ter que controlar os tipos de dados que são usados quando objetos gerenciados são expostos a COM. Por exemplo, estruturas em bibliotecas de classes devem especificar o `BStr` tipo não gerenciado em cadeias de caracteres enviadas a objetos com criados por Visual Basic 6,0 e versões anteriores. Nesses casos, você pode usar o <xref:System.Runtime.InteropServices.MarshalAsAttribute> atributo para fazer com que os tipos gerenciados sejam expostos como tipos não gerenciados.  
  
## <a name="exporting-fixed-length-strings-to-unmanaged-code"></a><a name="vbconinteroperabilitymarshalinganchor2"></a>Exportando cadeias de caracteres de comprimento fixo para código não gerenciado  
 No Visual Basic 6,0 e versões anteriores, as cadeias de caracteres são exportadas para objetos COM como sequências de bytes sem um caractere de terminação nulo. Para compatibilidade com outras linguagens, Visual Basic .NET inclui um caractere de terminação ao exportar cadeias de caracteres. A melhor maneira de resolver essa incompatibilidade é exportar cadeias de caracteres que não tenham o caractere de término como matrizes de `Byte` ou `Char` .  
  
## <a name="exporting-inheritance-hierarchies"></a><a name="vbconinteroperabilitymarshalinganchor3"></a>Exportando hierarquias de herança  
 Hierarquias de classe gerenciada mesclam quando expostas como objetos COM. Por exemplo, se você definir uma classe base com um membro e, em seguida, herdar a classe base em uma classe derivada que é exposta como um objeto COM, os clientes que usam a classe derivada no objeto COM não poderão usar os membros herdados. Os membros da classe base podem ser acessados de objetos COM somente como instâncias de uma classe base e, em seguida, somente se a classe base também for criada como um objeto COM.  
  
## <a name="overloaded-methods"></a>Métodos Sobrecarregados  
 Embora você possa criar métodos sobrecarregados com Visual Basic, eles não têm suporte do COM. Quando uma classe que contém métodos sobrecarregados é exposta como um objeto COM, novos nomes de método são gerados para os métodos sobrecarregados.  
  
 Por exemplo, considere uma classe que tem duas sobrecargas do `Synch` método. Quando a classe é exposta como um objeto COM, os novos nomes de método gerados podem ser `Synch` e `Synch_2` .  
  
 A renomeação pode causar dois problemas para os consumidores do objeto COM.  
  
1. Os clientes podem não esperar os nomes de métodos gerados.  
  
2. Os nomes de métodos gerados na classe exposta como um objeto COM podem ser alterados quando novas sobrecargas são adicionadas à classe ou sua classe base. Isso pode causar problemas de controle de versão.  
  
 Para resolver os dois problemas, dê a cada método um nome exclusivo, em vez de usar sobrecarga, ao desenvolver objetos que serão expostos como objetos COM.  
  
## <a name="use-of-com-objects-through-interop-assemblies"></a><a name="vbconinteroperabilitymarshalinganchor4"></a>Uso de objetos COM por meio de assemblies de interoperabilidade  
 Você usa assemblies de interoperabilidade quase como se eles fossem substituições de código gerenciado para os objetos COM que eles representam. No entanto, como são wrappers e não objetos COM reais, há algumas diferenças entre usar assemblies de interoperabilidade e assemblies padrão. Essas áreas de diferença incluem a exposição de classes e tipos de dados para parâmetros e valores de retorno.  
  
## <a name="classes-exposed-as-both-interfaces-and-classes"></a><a name="vbconinteroperabilitymarshalinganchor5"></a>Classes expostas como interfaces e classes  
 Ao contrário das classes em assemblies padrão, as classes COM são expostas em assemblies de interoperabilidade como uma interface e uma classe que representa a classe COM. O nome da interface é idêntico ao da classe COM. O nome da classe Interop é o mesmo da classe COM original, mas com a palavra "Class" acrescentada. Por exemplo, suponha que você tenha um projeto com uma referência a um assembly de interoperabilidade para um objeto COM. Se a classe COM for nomeada `MyComClass` , o IntelliSense e o pesquisador de objetos mostrarão uma interface chamada `MyComClass` e uma classe denominada `MyComClassClass` .  
  
## <a name="creating-instances-of-a-net-framework-class"></a><a name="vbconinteroperabilitymarshalinganchor6"></a>Criando instâncias de uma classe de .NET Framework  
 Em geral, você cria uma instância de uma classe de .NET Framework usando a `New` instrução com um nome de classe. Ter uma classe COM representada por um assembly de interoperabilidade é o único caso em que você pode usar a `New` instrução com uma interface. A menos que você esteja usando a classe COM com uma `Inherits` instrução, você pode usar a interface da mesma forma que faria com uma classe. O código a seguir demonstra como criar um `Command` objeto em um projeto que tem uma referência ao objeto com da biblioteca do Microsoft ActiveX Data Objects 2,8:  
  
 [!code-vb[VbVbalrInterop#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#20)]  
  
 No entanto, se você estiver usando a classe COM como a base para uma classe derivada, deverá usar a classe Interop que representa a classe COM, como no código a seguir:  
  
 [!code-vb[VbVbalrInterop#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#21)]  
  
> [!NOTE]
> Os assemblies de interoperabilidade implementam implicitamente interfaces que representam classes COM. Você não deve tentar usar a `Implements` instrução para implementar essas interfaces ou um erro será resultado.  
  
## <a name="data-types-for-parameters-and-return-values"></a><a name="vbconinteroperabilitymarshalinganchor7"></a>Tipos de dados para parâmetros e valores de retorno  
 Ao contrário dos membros de assemblies padrão, os membros do assembly de interoperabilidade podem ter tipos de dados diferentes daqueles usados na declaração de objeto original. Embora os assemblies de interoperabilidade convertam implicitamente tipos COM em tipos de Common Language Runtime compatíveis, você deve prestar atenção aos tipos de dados que são usados por ambos os lados para evitar erros de tempo de execução. Por exemplo, em objetos COM criados no Visual Basic 6,0 e em versões anteriores, os valores do tipo `Integer` assumem o .NET Framework tipo equivalente, `Short` . É recomendável que você use o pesquisador de objetos para examinar as características dos membros importados antes de usá-los.  
  
## <a name="module-level-com-methods"></a><a name="vbconinteroperabilitymarshalinganchor8"></a>Métodos COM de nível de módulo  
 A maioria dos objetos COM é usada pela criação de uma instância de uma classe COM usando a `New` palavra-chave e, em seguida, a chamada de métodos do objeto. Uma exceção a essa regra envolve objetos COM que contêm `AppObj` ou `GlobalMultiUse` classes com. Essas classes parecem métodos de nível de módulo em classes Visual Basic .NET. Visual Basic 6,0 e versões anteriores criam implicitamente instâncias desses objetos para você pela primeira vez que você chama um de seus métodos. Por exemplo, no Visual Basic 6,0, você pode adicionar uma referência à biblioteca de objetos do Microsoft DAO 3,6 e chamar o `DBEngine` método sem primeiro criar uma instância:  
  
```vb  
Dim db As DAO.Database  
' Open the database.  
Set db = DBEngine.OpenDatabase("C:\nwind.mdb")  
' Use the database object.  
```  
  
 Visual Basic .NET requer que você sempre Crie instâncias de objetos COM antes de poder usar seus métodos. Para usar esses métodos no Visual Basic, declare uma variável da classe desejada e use a nova palavra-chave para atribuir o objeto à variável de objeto. A `Shared` palavra-chave pode ser usada quando você deseja garantir que apenas uma instância da classe seja criada.  
  
 [!code-vb[VbVbalrInterop#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#23)]  
  
## <a name="unhandled-errors-in-event-handlers"></a><a name="vbconinteroperabilitymarshalinganchor9"></a>Erros sem tratamento em manipuladores de eventos  
 Um problema de interoperabilidade comum envolve erros em manipuladores de eventos que manipulam eventos gerados por objetos COM. Esses erros são ignorados, a menos que você verifique especificamente se há erros usando `On Error` `Try...Catch...Finally` instruções or. Por exemplo, o exemplo a seguir é de um projeto Visual Basic .NET que tem uma referência ao objeto COM da biblioteca do Microsoft ActiveX Data Objects 2,8.  
  
 [!code-vb[VbVbalrInterop#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#24)]  
  
 Este exemplo gera um erro como esperado. No entanto, se você tentar o mesmo exemplo sem o `Try...Catch...Finally` bloco, o erro será ignorado como se você tiver usado a `OnError Resume Next` instrução. Sem tratamento de erros, a divisão por zero falha silenciosamente. Como esses erros nunca geram erros de exceção sem tratamento, é importante que você use alguma forma de manipulação de exceção em manipuladores de eventos que manipulam eventos de objetos COM.  
  
### <a name="understanding-com-interop-errors"></a>Noções básicas sobre erros de interoperabilidade COM  
 Sem tratamento de erros, as chamadas de interoperabilidade geralmente geram erros que fornecem pouca informação. Sempre que possível, use o tratamento de erros estruturado para fornecer mais informações sobre problemas quando eles ocorrerem. Isso pode ser especialmente útil quando você depura aplicativos. Por exemplo:  
  
 [!code-vb[VbVbalrInterop#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#25)]  
  
 Você pode encontrar informações como a descrição do erro, HRESULT e a fonte de erros COM examinando o conteúdo do objeto de exceção.  
  
## <a name="activex-control-issues"></a><a name="vbconinteroperabilitymarshalinganchor10"></a>Problemas do controle ActiveX  
 A maioria dos controles ActiveX que funcionam com o Visual Basic 6,0 funcionam com o Visual Basic .NET sem problemas. As principais exceções são controles de contêiner ou controles que contêm visualmente outros controles. Alguns exemplos de controles mais antigos que não funcionam corretamente com o Visual Studio são os seguintes:  
  
- Controle de quadro do Microsoft Forms 2,0  
  
- Controle de cima para baixo, também conhecido como controle de rotação  
  
- Controle da guia Sheridan  
  
 Há apenas algumas soluções alternativas para problemas de controle ActiveX sem suporte. Você pode migrar controles existentes para o Visual Studio se possuir o código-fonte original. Caso contrário, você pode verificar se os fornecedores de software foram atualizados. Versões compatíveis com NET de controles para substituir controles ActiveX sem suporte.  
  
## <a name="passing-readonly-properties-of-controls-byref"></a><a name="vbconinteroperabilitymarshalinganchor11"></a>Passando Propriedades ReadOnly de controles ByRef  
 O Visual Basic .NET às vezes gera erros COM, como "Error 0x800A017F CTL_E_SETNOTSUPPORTED", quando você passa `ReadOnly` Propriedades de alguns controles ActiveX mais antigos como `ByRef` parâmetros para outros procedimentos. Chamadas de procedimento semelhantes do Visual Basic 6,0 não geram um erro e os parâmetros são tratados como se você os passasse por valor. A mensagem de erro Visual Basic .NET indica que você está tentando alterar uma propriedade que não tem um procedimento de propriedade `Set` .  
  
 Se você tiver acesso ao procedimento que está sendo chamado, poderá evitar esse erro usando a `ByVal` palavra-chave para declarar parâmetros que aceitam `ReadOnly` Propriedades. Por exemplo:  
  
 [!code-vb[VbVbalrInterop#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#26)]  
  
 Se você não tiver acesso ao código-fonte para o procedimento que está sendo chamado, poderá forçar a propriedade a ser passada por valor adicionando um conjunto extra de colchetes em volta do procedimento de chamada. Por exemplo, em um projeto que tem uma referência ao objeto COM da biblioteca do Microsoft ActiveX Data Objects 2,8, você pode usar:  
  
 [!code-vb[VbVbalrInterop#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#27)]  
  
## <a name="deploying-assemblies-that-expose-interop"></a><a name="vbconinteroperabilitymarshalinganchor12"></a>Implantando assemblies que expõem a interoperabilidade  
 A implantação de assemblies que expõem interfaces COM apresenta alguns desafios exclusivos. Por exemplo, um problema potencial ocorre quando aplicativos separados fazem referência ao mesmo assembly COM. Essa situação é comum quando uma nova versão de um assembly é instalada e outro aplicativo ainda está usando a versão antiga do assembly. Se você desinstalar um assembly que compartilha uma DLL, você poderá torná-la inadvertidamente indisponível para os outros assemblies.  
  
 Para evitar esse problema, você deve instalar assemblies compartilhados no GAC (cache de assembly global) e usar um MergeModule para o componente. Se você não puder instalar o aplicativo no GAC, ele deverá ser instalado em CommonFilesFolder em um subdiretório específico da versão.  
  
 Os assemblies que não são compartilhados devem estar localizados lado a lado no diretório com o aplicativo de chamada.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Interoperabilidade COM](index.md)
- [Tlbimp. exe (tipo de importador de biblioteca de tipos)](../../../framework/tools/tlbimp-exe-type-library-importer.md)
- [TlbExp. exe (tipo de exportador da biblioteca de tipos)](../../../framework/tools/tlbexp-exe-type-library-exporter.md)
- [Passo a passo: Implementação de herança com objetos COM](walkthrough-implementing-inheritance-with-com-objects.md)
- [Instrução Inherits](../../language-reference/statements/inherits-statement.md)
- [Cache de assemblies global](../../../framework/app-domains/gac.md)
