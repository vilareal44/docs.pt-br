---
title: 'Passo a passo: Fazer chamadas de APIs do Windows'
ms.date: 07/20/2015
helpviewer_keywords:
- DLLs, calling
- Windows API, walkthroughs
- platform invoke, walkthroughs
- API calls [Visual Basic], walkthroughs [Visual Basic]
- Windows API, calling
- walkthroughs [Visual Basic], API calls
- DllImport attribute, calling Windows API
- Declare statement [Visual Basic], declaring DLL functions
ms.assetid: 9280ca96-7a93-47a3-8d01-6d01be0657cb
ms.openlocfilehash: c7b84a3bb12329ae235e5ea03dc5e86f921112c4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396747"
---
# <a name="walkthrough-calling-windows-apis-visual-basic"></a>Instruções passo a passo: chamando APIs do Windows (Visual Basic)
As APIs do Windows são DLLs (bibliotecas de vínculo dinâmico) que fazem parte do sistema operacional Windows. Você os usa para executar tarefas quando é difícil escrever procedimentos equivalentes de sua preferência. Por exemplo, o Windows fornece uma função chamada `FlashWindowEx` que permite que você crie a barra de título para um aplicativo alternativo entre tons leves e escuros.  
  
 A vantagem de usar APIs do Windows em seu código é que elas podem economizar tempo de desenvolvimento porque contêm dezenas de funções úteis que já foram gravadas e aguardando para serem usadas. A desvantagem é que as APIs do Windows podem ser difíceis de trabalhar e Unforgiving quando as coisas dão errado.  
  
 As APIs do Windows representam uma categoria especial de interoperabilidade. As APIs do Windows não usam código gerenciado, não têm bibliotecas de tipos internas e usam tipos de dados diferentes daqueles usados com o Visual Studio. Devido a essas diferenças, e como as APIs do Windows não são objetos COM, a interoperabilidade com APIs do Windows e a .NET Framework é executada usando a invocação de plataforma, ou PInvoke. A invocação de plataforma é um serviço que permite que o código gerenciado chame funções não gerenciadas implementadas em DLLs. Para obter mais informações, consulte [consumindo funções de dll não gerenciadas](../../../framework/interop/consuming-unmanaged-dll-functions.md). Você pode usar o PInvoke em Visual Basic usando a `Declare` instrução ou aplicando o `DllImport` atributo a um procedimento vazio.  
  
 As chamadas à API do Windows eram uma parte importante da programação de Visual Basic no passado, mas raramente são necessárias com o Visual Basic .NET. Sempre que possível, você deve usar código gerenciado do .NET Framework para executar tarefas, em vez de chamadas à API do Windows. Este passo a passos fornece informações para as situações em que é necessário usar APIs do Windows.  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="api-calls-using-declare"></a>Chamadas de API usando declare  
 A maneira mais comum de chamar APIs do Windows é usando a `Declare` instrução.  
  
### <a name="to-declare-a-dll-procedure"></a>Para declarar um procedimento de DLL  
  
1. Determine o nome da função que você deseja chamar, além de seus argumentos, tipos de argumento e valor de retorno, bem como o nome e o local da DLL que o contém.  
  
    > [!NOTE]
    > Para obter informações completas sobre as APIs do Windows, consulte a documentação do SDK do Win32 na API do Windows do SDK da plataforma. Para obter mais informações sobre as constantes que as APIs do Windows usam, examine os arquivos de cabeçalho como o Windows. h incluído com o Platform SDK.  
  
2. Abra um novo projeto de aplicativo do Windows clicando em **novo** no menu **arquivo** e, em seguida, clicando em **projeto**. A caixa de diálogo **Novo Projeto** aparecerá.  
  
3. Selecione **aplicativo do Windows** na lista de modelos de projeto Visual Basic. O novo projeto é exibido.  
  
4. Adicione a seguinte `Declare` função à classe ou ao módulo no qual você deseja usar a dll:  
  
     [!code-vb[VbVbalrInterop#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#9)]  
  
### <a name="parts-of-the-declare-statement"></a>Partes da instrução Declare  
 A `Declare` instrução inclui os elementos a seguir.  
  
#### <a name="auto-modifier"></a>Modificador automático  
 O `Auto` modificador instrui o tempo de execução para converter a cadeia de caracteres com base no nome do método de acordo com Common Language Runtime regras (ou nome do alias, se especificado).  
  
#### <a name="lib-and-alias-keywords"></a>Palavras-chave lib e alias  
 O nome após a `Function` palavra-chave é o nome que seu programa usa para acessar a função importada. Pode ser igual ao nome real da função que você está chamando, ou você pode usar qualquer nome de procedimento válido e, em seguida, empregar a `Alias` palavra-chave para especificar o nome real da função que você está chamando.  
  
 Especifique a `Lib` palavra-chave, seguida do nome e do local da dll que contém a função que você está chamando. Você não precisa especificar o caminho para os arquivos localizados nos diretórios de sistema do Windows.  
  
 Use a `Alias` palavra-chave se o nome da função que você está chamando não for um nome de procedimento de Visual Basic válido ou estiver em conflito com o nome de outros itens em seu aplicativo. `Alias`indica o nome verdadeiro da função que está sendo chamada.  
  
#### <a name="argument-and-data-type-declarations"></a>Declarações de tipo de dados e de argumento  
 Declare os argumentos e seus tipos de dados. Essa parte pode ser desafiadora porque os tipos de dados que o Windows usa não correspondem aos tipos de dados do Visual Studio. Visual Basic faz grande parte do trabalho para você convertendo argumentos para tipos de dados compatíveis, um processo chamado *marshaling*. Você pode controlar explicitamente como os argumentos são empacotados usando o <xref:System.Runtime.InteropServices.MarshalAsAttribute> atributo definido no <xref:System.Runtime.InteropServices> namespace.  
  
> [!NOTE]
> As versões anteriores do Visual Basic permitiam a você declarar parâmetros `As Any` , o que significa que os dados de qualquer tipo de dados poderiam ser usados. Visual Basic requer que você use um tipo de dados específico para todas as `Declare` instruções.  
  
#### <a name="windows-api-constants"></a>Constantes da API do Windows  
 Alguns argumentos são combinações de constantes. Por exemplo, a `MessageBox` API mostrada neste passo a passos aceita um argumento inteiro chamado `Typ` que controla como a caixa de mensagem é exibida. Você pode determinar o valor numérico dessas constantes examinando as `#define` instruções no arquivo WinUser. h. Os valores numéricos geralmente são mostrados em hexadecimal, portanto, talvez você queira usar uma calculadora para adicioná-los e convertê-los em decimal. Por exemplo, se você quiser combinar as constantes para o estilo de exclamação `MB_ICONEXCLAMATION` 0x00000030 e o estilo de ' Sim/não ' `MB_YESNO` 0x00000004, poderá adicionar os números e obter um resultado de 0x00000034, ou 52 decimal. Embora você possa usar o resultado decimal diretamente, é melhor declarar esses valores como constantes em seu aplicativo e combiná-los usando o `Or` operador.  
  
##### <a name="to-declare-constants-for-windows-api-calls"></a>Para declarar constantes para chamadas à API do Windows  
  
1. Consulte a documentação da função do Windows que você está chamando. Determine o nome das constantes que ele usa e o nome do arquivo. h que contém os valores numéricos para essas constantes.  
  
2. Use um editor de texto, como o bloco de notas, para exibir o conteúdo do arquivo de cabeçalho (. h) e localize os valores associados às constantes que você está usando. Por exemplo, a `MessageBox` API usa a constante `MB_ICONQUESTION` para mostrar um ponto de interrogação na caixa de mensagem. A definição de `MB_ICONQUESTION` está em winuser. h e aparece da seguinte maneira:  
  
     `#define MB_ICONQUESTION             0x00000020L`  
  
3. Adicione instruções equivalentes `Const` à sua classe ou módulo para tornar essas constantes disponíveis para seu aplicativo. Por exemplo:  
  
     [!code-vb[VbVbalrInterop#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#11)]  
  
###### <a name="to-call-the-dll-procedure"></a>Para chamar o procedimento de DLL  
  
1. Adicione um botão chamado `Button1` ao formulário de inicialização para seu projeto e clique duas vezes nele para exibir seu código. O manipulador de eventos para o botão é exibido.  
  
2. Adicione código ao `Click` manipulador de eventos para o botão que você adicionou, para chamar o procedimento e fornecer os argumentos apropriados:  
  
     [!code-vb[VbVbalrInterop#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#12)]  
  
3. Execute o projeto pressionando F5. A caixa de mensagem é exibida com os botões **Sim** e **não** de resposta. Clique em um deles.  
  
#### <a name="data-marshaling"></a>Marshaling de dados  
 Visual Basic converte automaticamente os tipos de dados de parâmetros e valores de retorno para chamadas à API do Windows, mas você pode usar o `MarshalAs` atributo para especificar explicitamente os tipos de dados não gerenciados esperados por uma API. Para obter mais informações sobre o marshaling de interoperabilidade, consulte [marshaling interop](../../../framework/interop/interop-marshaling.md).  
  
##### <a name="to-use-declare-and-marshalas-in-an-api-call"></a>Para usar declare e MarshalAs em uma chamada à API  
  
1. Determine o nome da função que você deseja chamar, além de seus argumentos, tipos de dados e valor de retorno.  
  
2. Para simplificar o acesso ao `MarshalAs` atributo, adicione uma `Imports` instrução à parte superior do código para a classe ou módulo, como no exemplo a seguir:  
  
     [!code-vb[VbVbalrInterop#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#13)]  
  
3. Adicione um protótipo de função para a função importada à classe ou módulo que você está usando e aplique o `MarshalAs` atributo aos parâmetros ou ao valor de retorno. No exemplo a seguir, uma chamada à API que espera que o tipo `void*` seja empacotada como `AsAny` :  
  
     [!code-vb[VbVbalrInterop#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#14)]  
  
## <a name="api-calls-using-dllimport"></a>Chamadas de API usando DllImport  
 O `DllImport` atributo fornece uma segunda maneira de chamar funções em DLLs sem bibliotecas de tipos. `DllImport`é aproximadamente equivalente a usar uma `Declare` instrução, mas fornece mais controle sobre como as funções são chamadas.  
  
 Você pode usar `DllImport` com a maioria das chamadas de API do Windows, desde que a chamada se refira a um método compartilhado (às vezes chamado de *estático*). Você não pode usar métodos que exigem uma instância de uma classe. Ao contrário das `Declare` instruções, `DllImport` as chamadas não podem usar o `MarshalAs` atributo.  
  
### <a name="to-call-a-windows-api-using-the-dllimport-attribute"></a>Para chamar uma API do Windows usando o atributo DllImport  
  
1. Abra um novo projeto de aplicativo do Windows clicando em **novo** no menu **arquivo** e, em seguida, clicando em **projeto**. A caixa de diálogo **Novo Projeto** aparecerá.  
  
2. Selecione **aplicativo do Windows** na lista de modelos de projeto Visual Basic. O novo projeto é exibido.  
  
3. Adicione um botão chamado `Button2` ao formulário de inicialização.  
  
4. Clique duas vezes `Button2` para abrir o modo de exibição de código para o formulário.  
  
5. Para simplificar o acesso ao `DllImport` , adicione uma `Imports` instrução à parte superior do código para a classe do formulário de inicialização:  
  
     [!code-vb[VbVbalrInterop#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#13)]  
  
6. Declare uma função vazia que precede a `End Class` instrução do formulário e nomeie a função `MoveFile` .  
  
7. Aplique os `Public` `Shared` modificadores e à declaração de função e defina os parâmetros para `MoveFile` com base nos argumentos que a função da API do Windows usa:  
  
     [!code-vb[VbVbalrInterop#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#16)]  
  
     Sua função pode ter qualquer nome de procedimento válido; o `DllImport` atributo especifica o nome na dll. Ele também lida com o marshaling de interoperabilidade para os parâmetros e valores de retorno, de modo que você pode escolher os tipos de dados do Visual Studio que são semelhantes aos tipos de dados usados pela API.  
  
8. Aplique o `DllImport` atributo à função Empty. O primeiro parâmetro é o nome e o local da DLL que contém a função que você está chamando. Você não precisa especificar o caminho para os arquivos localizados nos diretórios de sistema do Windows. O segundo parâmetro é um argumento nomeado que especifica o nome da função na API do Windows. Neste exemplo, o `DllImport` atributo força chamadas a serem `MoveFile` encaminhadas para `MoveFileW` no Kernel32. DLL. O `MoveFileW` método copia um arquivo do caminho `src` para o caminho `dst` .  
  
     [!code-vb[VbVbalrInterop#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#17)]  
  
9. Adicione código ao `Button2_Click` manipulador de eventos para chamar a função:  
  
     [!code-vb[VbVbalrInterop#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#18)]  
  
10. Crie um arquivo chamado Test. txt e coloque-o no diretório C:\Tmp no seu disco rígido. Crie o diretório tmp, se necessário.  
  
11. Pressione F5 para iniciar o aplicativo. O formulário principal é exibido.  
  
12. Clique em **Button2**. A mensagem "o arquivo foi movido com êxito" será exibida se o arquivo puder ser movido.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.InteropServices.DllImportAttribute>
- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Instrução Declare](../../language-reference/statements/declare-statement.md)
- [Automático](../../language-reference/modifiers/auto.md)
- [Receber](../../language-reference/statements/alias-clause.md)
- [Interoperabilidade COM](index.md)
- [Criando protótipos em código gerenciado](../../../framework/interop/creating-prototypes-in-managed-code.md)
- [Realizando marshaling de um delegado como um método de retorno de chamada](../../../framework/interop/marshaling-a-delegate-as-a-callback-method.md)
