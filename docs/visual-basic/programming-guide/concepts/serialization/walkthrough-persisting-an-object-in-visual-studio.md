---
title: persistir um objeto no Visual Studio
ms.date: 07/20/2015
ms.assetid: f1d0b562-e349-4dce-ab5f-c05108467030
ms.openlocfilehash: e4eaf87c99ea1577d7f3ca40e628c00cc2700511
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413123"
---
# <a name="walkthrough-persisting-an-object-in-visual-studio-visual-basic"></a>Passo a passo: mantendo um objeto no Visual Studio (Visual Basic)
Embora você possa definir as propriedades de um objeto para os valores padrão em tempo de design, qualquer valor inserido em tempo de execução será perdido quando o objeto for destruído. Você pode usar a serialização para manter os dados de um objeto entre instâncias, o que permite armazenar valores e recuperá-los na próxima vez que o objeto for instanciado.  
  
> [!NOTE]
> No Visual Basic, para armazenar dados simples, como um nome ou número, você pode usar o objeto `My.Settings`. Para obter mais informações, consulte [Objeto My.Settings](../../../language-reference/objects/my-settings-object.md).  
  
 Neste passo a passo, você criará um objeto `Loan` simples e manterá seus dados em um arquivo. Em seguida, você recuperará os dados do arquivo quando recriar o objeto.  
  
> [!IMPORTANT]
> Este exemplo cria um novo arquivo, se o arquivo ainda não existe. Se um aplicativo precisar criar um arquivo, esse aplicativo precisará da permissão `Create` para a pasta. Permissões são definidas usando listas de controle de acesso. Se o arquivo já existir, o aplicativo precisará somente da permissão `Write`, que é uma permissão menor. Sempre que possível, é mais seguro criar o arquivo durante a implantação e somente conceder permissões `Read` a um único arquivo (em vez de permissões Create para uma pasta). Além disso, é mais seguro gravar dados em pastas de usuário do que na pasta raiz ou na pasta Arquivos de Programas.  
  
> [!IMPORTANT]
> Este exemplo armazena dados em um arquivo binário. Esses formatos não devem ser usados para dados confidenciais, como senhas ou informações de cartão de crédito.  
  
> [!NOTE]
> As caixas de diálogo e os comandos de menu encontrados podem diferir daqueles descritos na Ajuda, dependendo das configurações ativas ou edição. Para alterar as configurações, clique em **Importar e exportar configurações** no menu **Ferramentas**. Para obter mais informações, consulte [Personalizar o IDE do Visual Studio](/visualstudio/ide/personalizing-the-visual-studio-ide).  
  
## <a name="creating-the-loan-object"></a>Criando o objeto Loan  
 A primeira etapa é criar uma classe `Loan` e um aplicativo de teste que usa a classe.  
  
### <a name="to-create-the-loan-class"></a>Para criar a classe Loan  
  
1. Crie um novo projeto de Biblioteca de classes e denomine-o “LoanClass". Para obter mais informações, consulte [Criando soluções e projetos](https://docs.microsoft.com/visualstudio/ide/creating-solutions-and-projects).  
  
2. No **Gerenciador de Soluções**, abra o menu de atalho para o arquivo Class1 e escolha **Renomear**. Renomeie o arquivo como `Loan` e pressione ENTER. Renomear o arquivo também renomeará a classe para `Loan`.  
  
3. Adicione os seguintes membros públicos à classe:  
  
    ```vb  
    Public Class Loan  
        Implements System.ComponentModel.INotifyPropertyChanged  
  
        Public Property LoanAmount As Double  
        Public Property InterestRate As Double  
        Public Property Term As Integer  
  
        Private p_Customer As String  
        Public Property Customer As String  
            Get  
                Return p_Customer  
            End Get  
            Set(ByVal value As String)  
                p_Customer = value  
                RaiseEvent PropertyChanged(Me,  
                  New System.ComponentModel.PropertyChangedEventArgs("Customer"))  
            End Set  
        End Property  
  
        Event PropertyChanged As System.ComponentModel.PropertyChangedEventHandler _  
          Implements System.ComponentModel.INotifyPropertyChanged.PropertyChanged  
  
        Public Sub New(ByVal loanAmount As Double,  
                       ByVal interestRate As Double,  
                       ByVal term As Integer,  
                       ByVal customer As String)  
  
            Me.LoanAmount = loanAmount  
            Me.InterestRate = interestRate  
            Me.Term = term  
            p_Customer = customer  
        End Sub  
    End Class  
    ```  
  
 Você também precisará criar um aplicativo simples que usa a classe `Loan`.  
  
### <a name="to-create-a-test-application"></a>Para criar um aplicativo de teste  
  
1. Para adicionar um projeto de Aplicativo do Windows Forms à sua solução, no menu **Arquivo**, escolha **Adicionar**, **Novo Projeto**.  
  
2. Na caixa de diálogo **Adicionar Novo Projeto**, escolha **Aplicativo do Windows Forms** e insira `LoanApp` como o nome do projeto e clique **OK** para fechar a caixa de diálogo.  
  
3. No **Gerenciador de Soluções**, escolha o projeto LoanApp.  
  
4. No menu **Projeto**, escolha **Definir como Projeto de Inicialização**.  
  
5. No menu **Projeto**, escolha **Adicionar Referência**.  
  
6. Na caixa de diálogo **Adicionar Referência**, escolha a guia **Projetos** e escolha o projeto LoanClass.  
  
7. Clique em **OK** para fechar a caixa de diálogo.  
  
8. No designer, adicione quatro controles <xref:System.Windows.Forms.TextBox> ao formulário.  
  
9. No Editor de Códigos, adicione o seguinte código:  
  
    ```vb  
    Private WithEvents TestLoan As New LoanClass.Loan(10000.0, 0.075, 36, "Neil Black")  
  
    Private Sub Form1_Load() Handles MyBase.Load  
        TextBox1.Text = TestLoan.LoanAmount.ToString  
        TextBox2.Text = TestLoan.InterestRate.ToString  
        TextBox3.Text = TestLoan.Term.ToString  
        TextBox4.Text = TestLoan.Customer  
    End Sub  
    ```  
  
10. Adicione um manipulador de eventos para o evento `PropertyChanged` do formulário usando o seguinte código:  
  
    ```vb  
    Public Sub CustomerPropertyChanged(  
          ByVal sender As Object,  
          ByVal e As System.ComponentModel.PropertyChangedEventArgs  
        ) Handles TestLoan.PropertyChanged  
  
        MsgBox(e.PropertyName & " has been changed.")  
    End Sub  
    ```  
  
 Neste ponto, você pode compilar e executar o aplicativo. Observe que os valores padrão da classe `Loan` aparecem nas caixas de texto. Tente alterar o valor de taxa de juros de 7,5 para 7,1 e, em seguida, feche o aplicativo e execute-o novamente – o valor será revertido para o padrão de 7,5.  
  
 No mundo real, as taxas de juros mudam periodicamente, mas não necessariamente toda vez que o aplicativo for executado. Em vez de fazer o usuário atualizar a taxa de juros sempre que o aplicativo for executado, é melhor preservar a taxa de juros mais recente entre instâncias do aplicativo. Na próxima etapa, você fará exatamente isso adicionando a serialização à classe Loan.  
  
## <a name="using-serialization-to-persist-the-object"></a>Usando a serialização para manter o objeto  
 Para manter os valores da classe Loan, primeiro você deve marcar a classe com o atributo `Serializable`.  
  
### <a name="to-mark-a-class-as-serializable"></a>Para marcar uma classe como serializável  
  
- Altere a declaração da classe para a classe Loan da seguinte maneira:  
  
    ```vb  
    <Serializable()>  
    Public Class Loan  
    ```  
  
 O atributo `Serializable` informa ao compilador que tudo na classe pode ser mantido em um arquivo. Como o evento `PropertyChanged` é manipulado por um objeto do Windows Forms, ele não pode ser serializado. O atributo `NonSerialized` pode ser usado para marcar os membros da classe que não devem ser mantidos.  
  
### <a name="to-prevent-a-member-from-being-serialized"></a>Para impedir que um membro seja serializado  
  
- Altere a declaração para o evento `PropertyChanged` da seguinte maneira:  
  
    ```vb  
    <NonSerialized()>  
    Event PropertyChanged As System.ComponentModel.PropertyChangedEventHandler _  
      Implements System.ComponentModel.INotifyPropertyChanged.PropertyChanged  
    ```  
  
 A etapa seguinte é adicionar o código de serialização ao aplicativo LoanApp. Para serializar a classe e gravá-la em um arquivo, use os namespaces <xref:System.IO> e <xref:System.Xml.Serialization>. Para evitar digitar os nomes totalmente qualificados, você pode adicionar referências às bibliotecas de classe necessárias.  
  
### <a name="to-add-references-to-namespaces"></a>Para adicionar referências a namespaces  
  
- Adicione as seguintes instruções ao topo da classe `Form1`:  
  
    ```vb  
    Imports System.IO  
    Imports System.Runtime.Serialization.Formatters.Binary  
    ```  
  
     Nesse caso, você está usando um formatador binário para salvar o objeto em um formato binário.  
  
 A próxima etapa é adicionar código para desserializar o objeto do arquivo quando o objeto for criado.  
  
### <a name="to-deserialize-an-object"></a>Para desserializar um objeto  
  
1. Adicione uma constante à classe para o nome do arquivo de dados serializado.  
  
    ```vb  
    Const FileName As String = "..\..\SavedLoan.bin"  
    ```  
  
2. Modifique o código no procedimento do evento `Form1_Load` da seguinte maneira:  
  
    ```vb  
    Private WithEvents TestLoan As New LoanClass.Loan(10000.0, 0.075, 36, "Neil Black")  
  
    Private Sub Form1_Load() Handles MyBase.Load  
        If File.Exists(FileName) Then  
            Dim TestFileStream As Stream = File.OpenRead(FileName)  
            Dim deserializer As New BinaryFormatter  
            TestLoan = CType(deserializer.Deserialize(TestFileStream), LoanClass.Loan)  
            TestFileStream.Close()  
        End If  
  
        AddHandler TestLoan.PropertyChanged, AddressOf Me.CustomerPropertyChanged  
  
        TextBox1.Text = TestLoan.LoanAmount.ToString  
        TextBox2.Text = TestLoan.InterestRate.ToString  
        TextBox3.Text = TestLoan.Term.ToString  
        TextBox4.Text = TestLoan.Customer  
    End Sub  
    ```  
  
     Observe que primeiro você deve verificar se o arquivo existe. Se ele existir, crie uma classe <xref:System.IO.Stream> para ler o arquivo binário e uma classe <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> para converter o arquivo. Você também precisa converter do tipo de fluxo para o tipo de objeto Loan.  
  
 Em seguida, você deve adicionar código para salvar os dados inseridos nas caixas de texto na classe `Loan` e, então, precisa serializar a classe em um arquivo.  
  
### <a name="to-save-the-data-and-serialize-the-class"></a>Para salvar os dados e serializar a classe  
  
- Adicione o seguinte código ao procedimento do evento `Form1_FormClosing`:  
  
    ```vb  
    Private Sub Form1_FormClosing() Handles MyBase.FormClosing  
        TestLoan.LoanAmount = CDbl(TextBox1.Text)  
        TestLoan.InterestRate = CDbl(TextBox2.Text)  
        TestLoan.Term = CInt(TextBox3.Text)  
        TestLoan.Customer = TextBox4.Text  
  
        Dim TestFileStream As Stream = File.Create(FileName)  
        Dim serializer As New BinaryFormatter  
        serializer.Serialize(TestFileStream, TestLoan)  
        TestFileStream.Close()  
    End Sub  
    ```  
  
 Neste ponto, você pode compilar e executar o aplicativo novamente. Inicialmente, os valores padrão aparecem nas caixas de texto. Tente alterar os valores e digite um nome na quarta caixa de texto. Feche o aplicativo e execute-o novamente. Observe que agora os novos valores aparecem nas caixas de texto.  
  
## <a name="see-also"></a>Confira também

- [Serialização (Visual Basic)](index.md)
- [Guia de programação do Visual Basic](../../index.md)
