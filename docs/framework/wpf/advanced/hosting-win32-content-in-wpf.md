---
title: Hospedando conteúdo Win32
ms.date: 03/30/2017
helpviewer_keywords:
- interoperability [WPF], tutorials
- Win32 code [WPF], WPF interoperation
- interoperability [WPF], Win32
ms.assetid: 3cc8644a-34f3-4082-9ddc-77623e4df2d8
ms.openlocfilehash: c0c62f1999feaf591c512314515f01e83fa12591
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86052085"
---
# <a name="hosting-win32-content-in-wpf"></a>Hospedando conteúdo Win32 no WPF

## <a name="prerequisites"></a>Pré-requisitos

Consulte [interoperação do WPF e do Win32](wpf-and-win32-interoperation.md).

## <a name="a-walkthrough-of-win32-inside-windows-presentation-framework-hwndhost"></a>Um passo a passo do Win32 no Windows Presentation Framework (HwndHost)

Para reutilizar o conteúdo Win32 dentro de [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativos, use <xref:System.Windows.Interop.HwndHost> , que é um controle que faz com que os HWNDs pareçam [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] conteúdo. Como <xref:System.Windows.Interop.HwndSource> , <xref:System.Windows.Interop.HwndHost> é simples de usar: derivar de <xref:System.Windows.Interop.HwndHost> e implementar `BuildWindowCore` e `DestroyWindowCore` métodos, instanciar sua <xref:System.Windows.Interop.HwndHost> classe derivada e colocá-la dentro de seu [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativo.

Se a lógica do Win32 já estiver empacotada como um controle, a `BuildWindowCore` implementação será pouco mais do que uma chamada para `CreateWindow` . Por exemplo, para criar um controle LISTBOX do Win32 em C++:

```cpp
virtual HandleRef BuildWindowCore(HandleRef hwndParent) override {
    HWND handle = CreateWindowEx(0, L"LISTBOX",
    L"this is a Win32 listbox",
    WS_CHILD | WS_VISIBLE | LBS_NOTIFY
    | WS_VSCROLL | WS_BORDER,
    0, 0, // x, y
    30, 70, // height, width
    (HWND) hwndParent.Handle.ToPointer(), // parent hwnd
    0, // hmenu
    0, // hinstance
    0); // lparam

    return HandleRef(this, IntPtr(handle));
}

virtual void DestroyWindowCore(HandleRef hwnd) override {
    // HwndHost will dispose the hwnd for us
}
```

Mas suponha que o código Win32 não seja tão independente? Nesse caso, você pode criar uma caixa de diálogo do Win32 e inserir seu conteúdo em um [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativo maior. O exemplo mostra isso no Visual Studio e no C++, embora também seja possível fazer isso em um idioma diferente ou na linha de comando.

Comece com uma caixa de diálogo simples, que é compilada em um projeto da DLL do C++.

Em seguida, coloque a caixa de diálogo no aplicativo [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] maior:

- Compilar a DLL como gerenciada ( `/clr` )

- Transforme a caixa de diálogo em um controle

- Definir a classe derivada de <xref:System.Windows.Interop.HwndHost> com `BuildWindowCore` `DestroyWindowCore` métodos e

- Substitua o método `TranslateAccelerator` para manipular as teclas do diálogo

- Substitua o método `TabInto` para dar suporte à tabulação

- Substitua o método `OnMnemonic` para dar suporte aos mnemônicos

- Criar uma instância da <xref:System.Windows.Interop.HwndHost> subclasse e colocá-la no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] elemento à direita

### <a name="turn-the-dialog-into-a-control"></a>Transforme a Caixa de Diálogo em um Controle

Você pode transformar uma caixa de diálogo em um HWND filho usando os estilos WS_CHILD e DS_CONTROL. Vá para o arquivo de recurso (.rc) no qual a caixa de diálogo está definida e localize o início da definição da caixa de diálogo:

```text
IDD_DIALOG1 DIALOGEX 0, 0, 303, 121
STYLE DS_SETFONT | DS_MODALFRAME | DS_FIXEDSYS | WS_POPUP | WS_CAPTION | WS_SYSMENU
```

Altere a segunda linha para:

```text
STYLE DS_SETFONT | WS_CHILD | WS_BORDER | DS_CONTROL
```

Essa ação não o empacota totalmente em um controle independente; Você ainda precisa chamar `IsDialogMessage()` para que o Win32 possa processar determinadas mensagens, mas a alteração de controle fornece uma maneira simples de colocar esses controles dentro de outro HWND.

## <a name="subclass-hwndhost"></a>Subclasse HwndHost

Importe os seguintes namespaces:

```cpp
namespace ManagedCpp
{
    using namespace System;
    using namespace System::Windows;
    using namespace System::Windows::Interop;
    using namespace System::Windows::Input;
    using namespace System::Windows::Media;
    using namespace System::Runtime::InteropServices;
```

Em seguida, crie uma classe derivada de <xref:System.Windows.Interop.HwndHost> e substitua os `BuildWindowCore` `DestroyWindowCore` métodos e:

```cpp
public ref class MyHwndHost : public HwndHost, IKeyboardInputSink {
    private:
        HWND dialog;

    protected:
        virtual HandleRef BuildWindowCore(HandleRef hwndParent) override {
            InitializeGlobals();
            dialog = CreateDialog(hInstance,
                MAKEINTRESOURCE(IDD_DIALOG1),
                (HWND) hwndParent.Handle.ToPointer(),
                (DLGPROC) About);
            return HandleRef(this, IntPtr(dialog));
        }

        virtual void DestroyWindowCore(HandleRef hwnd) override {
            // hwnd will be disposed for us
        }
```

Aqui você vai usar o `CreateDialog` para criar a caixa de diálogo que é, na realidade, um controle. Como esse é um dos primeiros métodos chamados dentro da DLL, você também deve fazer alguma inicialização padrão do Win32 chamando uma função que você definirá posteriormente, chamada `InitializeGlobals()` :

```cpp
bool initialized = false;
    void InitializeGlobals() {
        if (initialized) return;
        initialized = true;

        // TODO: Place code here.
        MSG msg;
        HACCEL hAccelTable;

        // Initialize global strings
        LoadString(hInstance, IDS_APP_TITLE, szTitle, MAX_LOADSTRING);
        LoadString(hInstance, IDC_TYPICALWIN32DIALOG, szWindowClass, MAX_LOADSTRING);
        MyRegisterClass(hInstance);
```

### <a name="override-translateaccelerator-method-to-handle-dialog-keys"></a>Substitua o método TranslateAccelerator para manipular as teclas do diálogo

Se executar este exemplo agora, você obterá um controle de caixa de diálogo que exibe, mas ele ignoraria todo o processamento do teclado que torna uma caixa de diálogo funcional. Agora, você deve substituir a `TranslateAccelerator` implementação (que vem de `IKeyboardInputSink` uma interface que <xref:System.Windows.Interop.HwndHost> implementa). Esse método é chamado quando o aplicativo recebe WM_KEYDOWN e WM_SYSKEYDOWN.

```cpp
#undef TranslateAccelerator
        virtual bool TranslateAccelerator(System::Windows::Interop::MSG% msg,
            ModifierKeys modifiers) override
        {
            ::MSG m = ConvertMessage(msg);

            // Win32's IsDialogMessage() will handle most of our tabbing, but doesn't know
            // what to do when it reaches the last tab stop
            if (m.message == WM_KEYDOWN && m.wParam == VK_TAB) {
                HWND firstTabStop = GetDlgItem(dialog, IDC_EDIT1);
                HWND lastTabStop = GetDlgItem(dialog, IDCANCEL);
                TraversalRequest^ request = nullptr;

                if (GetKeyState(VK_SHIFT) && GetFocus() == firstTabStop) {
                    // this code should work, but there’s a bug with interop shift-tab in current builds
                    request = gcnew TraversalRequest(FocusNavigationDirection::Last);
                }
                else if (!GetKeyState(VK_SHIFT) && GetFocus() == lastTabStop) {
                    request = gcnew TraversalRequest(FocusNavigationDirection::Next);
                }

                if (request != nullptr)
                    return ((IKeyboardInputSink^) this)->KeyboardInputSite->OnNoMoreTabStops(request);

            }

            // Only call IsDialogMessage for keys it will do something with.
            if (msg.message == WM_SYSKEYDOWN || msg.message == WM_KEYDOWN) {
                switch (m.wParam) {
                    case VK_TAB:
                    case VK_LEFT:
                    case VK_UP:
                    case VK_RIGHT:
                    case VK_DOWN:
                    case VK_EXECUTE:
                    case VK_RETURN:
                    case VK_ESCAPE:
                    case VK_CANCEL:
                        IsDialogMessage(dialog, &m);
                        // IsDialogMessage should be called ProcessDialogMessage --
                        // it processes messages without ever really telling you
                        // if it handled a specific message or not
                        return true;
                }
            }

            return false; // not a key we handled
        }
```

É muito código de uma vez, então as explicações poderiam ser um pouco mais detalhadas. Primeiro, o código que usa macros C++ e C++; Você precisa estar ciente de que já existe uma macro chamada `TranslateAccelerator` , que é definida em winuser. h:

```cpp
#define TranslateAccelerator  TranslateAcceleratorW
```

Certifique-se de definir um método `TranslateAccelerator` e não um método `TranslateAcceleratorW`.

Da mesma forma, há o winuser.h MSG não gerenciado e o struct `Microsoft::Win32::MSG` gerenciado. Você pode desambiguar entre os dois usando o `::` operador C++.

```cpp
virtual bool TranslateAccelerator(System::Windows::Interop::MSG% msg,
    ModifierKeys modifiers) override
{
    ::MSG m = ConvertMessage(msg);
}
```

Os MSGs têm os mesmos dados, mas, às vezes, é mais fácil trabalhar com a definição não gerenciada, portanto, neste exemplo, você poderá definir a rotina de conversão óbvia:

```cpp
::MSG ConvertMessage(System::Windows::Interop::MSG% msg) {
    ::MSG m;
    m.hwnd = (HWND) msg.hwnd.ToPointer();
    m.lParam = (LPARAM) msg.lParam.ToPointer();
    m.message = msg.message;
    m.wParam = (WPARAM) msg.wParam.ToPointer();

    m.time = msg.time;

    POINT pt;
    pt.x = msg.pt_x;
    pt.y = msg.pt_y;
    m.pt = pt;

    return m;
}
```

Voltar para `TranslateAccelerator`. O princípio básico é chamar a função do Win32 `IsDialogMessage` para fazer o máximo de trabalho possível, mas não `IsDialogMessage` tem acesso a nada fora da caixa de diálogo. Como uma guia de usuário na caixa de diálogo, quando a tabulação passar do último controle na nossa caixa de diálogo, você precisará definir o foco para a parte [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] chamando `IKeyboardInputSite::OnNoMoreStops`.

```cpp
// Win32's IsDialogMessage() will handle most of the tabbing, but doesn't know
// what to do when it reaches the last tab stop
if (m.message == WM_KEYDOWN && m.wParam == VK_TAB) {
    HWND firstTabStop = GetDlgItem(dialog, IDC_EDIT1);
    HWND lastTabStop = GetDlgItem(dialog, IDCANCEL);
    TraversalRequest^ request = nullptr;

    if (GetKeyState(VK_SHIFT) && GetFocus() == firstTabStop) {
        request = gcnew TraversalRequest(FocusNavigationDirection::Last);
    }
    else if (!GetKeyState(VK_SHIFT) && GetFocus() ==  lastTabStop) { {
        request = gcnew TraversalRequest(FocusNavigationDirection::Next);
    }

    if (request != nullptr)
        return ((IKeyboardInputSink^) this)->KeyboardInputSite->OnNoMoreTabStops(request);
}
```

Finalmente, chame `IsDialogMessage`. Mas uma das responsabilidades de um método `TranslateAccelerator` está informando [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] se você lidou com o pressionamento de teclas ou não. Se você não lidou com isso, o evento de entrada poderá criar um túnel e uma bolha pelo resto do aplicativo. Aqui, você vai expor uma peculiaridade da manipulação de messange de teclado e da natureza da arquitetura de entrada no Win32. Infelizmente, `IsDialogMessage` não é retornado de nenhuma forma independentemente de lidar ou não com um determinado pressionamento de tecla. Ainda pior, ele chamará `DispatchMessage()` nos pressionamentos de teclas que ele não deveria manipular!  Assim você terá que fazer engenharia reversa de `IsDialogMessage` e apenas chamá-lo para as teclas que você sabe que ele manipulará:

```cpp
// Only call IsDialogMessage for keys it will do something with.
if (msg.message == WM_SYSKEYDOWN || msg.message == WM_KEYDOWN) {
    switch (m.wParam) {
        case VK_TAB:
        case VK_LEFT:
        case VK_UP:
        case VK_RIGHT:
        case VK_DOWN:
        case VK_EXECUTE:
        case VK_RETURN:
        case VK_ESCAPE:
        case VK_CANCEL:
            IsDialogMessage(dialog, &m);
            // IsDialogMessage should be called ProcessDialogMessage --
            // it processes messages without ever really telling you
            // if it handled a specific message or not
            return true;
    }
```

### <a name="override-tabinto-method-to-support-tabbing"></a>Substituir o método TabInto para dar suporte à tabulação

Agora que você implementou `TranslateAccelerator`, um usuário pode tabular dentro da caixa de diálogo caixa e fora dela no aplicativo [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] maior. Mas um usuário não pode tabular novamente na caixa de diálogo. Para resolver isso, você deve substituir `TabInto`:

```cpp
public:
    virtual bool TabInto(TraversalRequest^ request) override {
        if (request->FocusNavigationDirection == FocusNavigationDirection::Last) {
            HWND lastTabStop = GetDlgItem(dialog, IDCANCEL);
            SetFocus(lastTabStop);
        }
        else {
            HWND firstTabStop = GetDlgItem(dialog, IDC_EDIT1);
            SetFocus(firstTabStop);
        }
        return true;
    }
```

O parâmetro `TraversalRequest` informa se a ação da guia é uma guia ou guia de deslocamento.

### <a name="override-onmnemonic-method-to-support-mnemonics"></a>Substitua o método OnMnemonic para dar suporte aos mnemônicos

A manipulação de teclado está quase concluída, mas há uma coisa faltando: os mnemônicos não funcionam. Se um usuário pressionar alt-F, o foco não pulará para a caixa de edição “Nome:”. Portanto, você substitui o método `OnMnemonic`:

```cpp
virtual bool OnMnemonic(System::Windows::Interop::MSG% msg, ModifierKeys modifiers) override {
    ::MSG m = ConvertMessage(msg);

    // If it's one of our mnemonics, set focus to the appropriate hwnd
    if (msg.message == WM_SYSCHAR && GetKeyState(VK_MENU /*alt*/)) {
        int dialogitem = 9999;
        switch (m.wParam) {
            case 's': dialogitem = IDOK; break;
            case 'c': dialogitem = IDCANCEL; break;
            case 'f': dialogitem = IDC_EDIT1; break;
            case 'l': dialogitem = IDC_EDIT2; break;
            case 'p': dialogitem = IDC_EDIT3; break;
            case 'a': dialogitem = IDC_EDIT4; break;
            case 'i': dialogitem = IDC_EDIT5; break;
            case 't': dialogitem = IDC_EDIT6; break;
            case 'z': dialogitem = IDC_EDIT7; break;
        }
        if (dialogitem != 9999) {
            HWND hwnd = GetDlgItem(dialog, dialogitem);
            SetFocus(hwnd);
            return true;
        }
    }
    return false; // key unhandled
};
```

Por que não chamar `IsDialogMessage` aqui?  Você tem o mesmo problema que antes: precisa ser capaz de informar o código [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] se seu código manipulou o pressionamento de teclas ou não e `IsDialogMessage` não pode fazer isso. Há também um segundo problema, pois `IsDialogMessage` se recusará a processar o mnemônico se o HWND com o foco não estiver dentro da caixa de diálogo.

### <a name="instantiate-the-hwndhost-derived-class"></a>Instanciar a classe derivada de HwndHost

Por fim, agora que todo o suporte de chave e guia está em vigor, você pode colocar seu <xref:System.Windows.Interop.HwndHost> no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativo maior. Se o aplicativo principal for escrito no [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] , a maneira mais fácil de colocá-lo no lugar certo é deixar um elemento vazio <xref:System.Windows.Controls.Border> onde você deseja colocar o <xref:System.Windows.Interop.HwndHost> . Aqui, você cria um <xref:System.Windows.Controls.Border> nome `insertHwndHostHere` :

```xaml
<Window x:Class="WPFApplication1.Window1"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    Title="Windows Presentation Framework Application"
    Loaded="Window1_Loaded"
    >
    <StackPanel>
        <Button Content="WPF button"/>
        <Border Name="insertHwndHostHere" Height="200" Width="500"/>
        <Button Content="WPF button"/>
    </StackPanel>
</Window>
```

Então, tudo o que resta é encontrar um bom local na sequência de código para criar uma instância do <xref:System.Windows.Interop.HwndHost> e conectá-lo ao <xref:System.Windows.Controls.Border> . Neste exemplo, você irá colocá-lo dentro do construtor para a <xref:System.Windows.Window> classe derivada:

```csharp
public partial class Window1 : Window {
    public Window1() {
    }

    void Window1_Loaded(object sender, RoutedEventArgs e) {
        HwndHost host = new ManagedCpp.MyHwndHost();
        insertHwndHostHere.Child = host;
    }
}
```

O que fornece:

![Captura de tela do aplicativo WPF em execução.](./media/hosting-win32-content-in-wpf/windows-presentation-foundation-application.png)

## <a name="see-also"></a>Consulte também

- [Interoperação Win32 e WPF](wpf-and-win32-interoperation.md)
