---
title: Suporte a DPI alto
description: Saiba mais sobre o suporte em Windows Forms para cenários de DPI dinâmico e DPI comuns. Saiba também como configurar aplicativos de Windows Forms para suporte a DPI alto.
ms.date: 05/16/2017
helpviewer_keywords:
- High DPI in Windows Forms
- Dynamic rescaling in Windows Forms
- Windows Forms layout
- Windows Forms dynamic resizing
ms.assetid: 075ea4c3-900c-4f8a-9dd2-13ea6804346b
ms.openlocfilehash: a9e0766307095da447c772de5a3065c18b7b7154
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325648"
---
# <a name="high-dpi-support-in-windows-forms"></a>Suporte a alto DPI no Windows Forms

A partir do .NET Framework 4,7, Windows Forms inclui aprimoramentos para cenários comuns de dpi e DPI dinâmicos. Eles incluem:

- Melhorias no dimensionamento e no layout de vários controles de Windows Forms, como o <xref:System.Windows.Forms.MonthCalendar> controle e o <xref:System.Windows.Forms.CheckedListBox> controle.

- Escala de passagem única.  No .NET Framework 4,6 e versões anteriores, o dimensionamento foi realizado por meio de várias passagens, o que fazia com que alguns controles fosse escalado mais do que o necessário.

- Suporte para cenários de DPI dinâmico em que o usuário altera o fator de escala ou DPI depois que um aplicativo de Windows Forms foi iniciado.

Nas versões do .NET Framework a partir do .NET Framework 4,7, o suporte a alto DPI avançado é um recurso opcional. Você deve configurar seu aplicativo para tirar proveito dele.

## <a name="configuring-your-windows-forms-app-for-high-dpi-support"></a>Configurando seu aplicativo Windows Forms para suporte a alto DPI

Os novos recursos Windows Forms que dão suporte a reconhecimento de DPI alta estão disponíveis somente em aplicativos direcionados para o .NET Framework 4,7 e estão em execução em sistemas operacionais Windows, começando com a atualização do Windows 10 para criadores.

Além disso, para configurar o suporte de DPI alto no seu aplicativo Windows Forms, você deve fazer o seguinte:

- Declare a compatibilidade com o Windows 10.

  Para fazer isso, adicione o seguinte ao seu arquivo de manifesto:

  ```xml
  <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
    <application>
      <!-- Windows 10 compatibility -->
      <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    </application>
  </compatibility>
  ```

- Habilite o reconhecimento de DPI por monitor no arquivo de *app.config* .

  Windows Forms introduz um novo [`<System.Windows.Forms.ApplicationConfigurationSection>`](../configure-apps/file-schema/winforms/index.md) elemento para dar suporte a novos recursos e personalizações adicionadas a partir do .NET Framework 4,7. Para aproveitar os novos recursos que dão suporte a DPI alto, adicione o seguinte ao arquivo de configuração do aplicativo.

  ```xml
  <System.Windows.Forms.ApplicationConfigurationSection>
    <add key="DpiAwareness" value="PerMonitorV2" />
  </System.Windows.Forms.ApplicationConfigurationSection>
  ```

  > [!IMPORTANT]
  > Nas versões anteriores do .NET Framework, você usou o manifesto para adicionar suporte a DPI alto. Essa abordagem não é mais recomendada, já que ela substitui as configurações definidas no arquivo app.config.

- Chame o <xref:System.Windows.Forms.Application.EnableVisualStyles%2A> método estático.

  Deve ser a primeira chamada de método no ponto de entrada do aplicativo. Por exemplo:

  ```csharp
  static void Main()
  {
      Application.EnableVisualStyles();
      Application.SetCompatibleTextRenderingDefault(false);
      Application.Run(new Form2());
  }
  ```

## <a name="opting-out-of-individual-high-dpi-features"></a>Recusando recursos de DPI alta individuais

Definir o `DpiAwareness` valor como `PerMonitorV2` habilita todos os recursos de reconhecimento de DPI alto suportados pelas versões .NET Framework começando com o .NET Framework 4,7. Normalmente, isso é adequado para a maioria dos aplicativos Windows Forms. No entanto, talvez você queira recusar um ou mais recursos individuais. O motivo mais importante para fazer isso é que o código do aplicativo existente já lida com esse recurso.  Por exemplo, se seu aplicativo lida com o dimensionamento automático, talvez você queira desabilitar o recurso de redimensionamento automático da seguinte maneira:

```xml
<System.Windows.Forms.ApplicationConfigurationSection>
  <add key="DpiAwareness" value="PerMonitorV2" />
  <add key="EnableWindowsFormsHighDpiAutoResizing" value="false" />
</System.Windows.Forms.ApplicationConfigurationSection>
```

Para obter uma lista de chaves individuais e seus valores, consulte [Windows Forms adicionar elemento de configuração](../configure-apps/file-schema/winforms/windows-forms-add-configuration-element.md).

## <a name="new-dpi-change-events"></a>Novos eventos de alteração de DPI

A partir do .NET Framework 4,7, três novos eventos permitem que você manipule programaticamente alterações de DPI dinâmicas:

- <xref:System.Windows.Forms.Control.DpiChangedAfterParent>, que é disparado quando a configuração de DPI de um controle é alterada programaticamente após um evento de alteração de DPI para o controle pai ou formulário ter ocorrido.
- <xref:System.Windows.Forms.Control.DpiChangedBeforeParent>, que é acionado quando a configuração de DPI de um controle é alterada programaticamente antes de um evento de alteração de DPI para seu controle pai ou formulário ter ocorrido.
- <xref:System.Windows.Forms.Form.DpiChanged>, que é disparado quando a configuração de DPI é alterada no dispositivo de vídeo onde o formulário está atualmente exibido.

## <a name="new-helper-methods-and-properties"></a>Novos métodos auxiliares e propriedades

O .NET Framework 4,7 também adiciona uma série de novos métodos auxiliares e propriedades que fornecem informações sobre o dimensionamento de DPI e permitem que você execute o dimensionamento de DPI. Eles incluem:

- <xref:System.Windows.Forms.Control.LogicalToDeviceUnits%2A>, que converte um valor dos pixels lógicos para de dispositivo.

- <xref:System.Windows.Forms.Control.ScaleBitmapLogicalToDevice%2A>, que dimensiona uma imagem de bitmap para o DPI lógico de um dispositivo.

- <xref:System.Windows.Forms.Control.DeviceDpi%2A>, que retorna o DPI para o dispositivo atual.

## <a name="versioning-considerations"></a>Considerações sobre controle de versão

Além de ser executado na atualização do .NET Framework 4,7 e do Windows 10 para criadores, seu aplicativo também pode ser executado em um ambiente no qual não é compatível com melhorias de DPI alto. Nesse caso, você precisará desenvolver um fallback para seu aplicativo. Você pode fazer isso para executar o [desenho personalizado](./controls/user-drawn-controls.md) para lidar com o dimensionamento.

Para fazer isso, você também precisa determinar o sistema operacional no qual seu aplicativo está sendo executado. Você pode fazer isso com um código semelhante ao seguinte:

```csharp
// Create a reference to the OS version of Windows 10 Creators Update.
Version OsMinVersion = new Version(10, 0, 15063, 0);

// Access the platform/version of the current OS.
Console.WriteLine(Environment.OSVersion.Platform.ToString());
Console.WriteLine(Environment.OSVersion.VersionString);

// Compare the current version to the minimum required version.
Console.WriteLine(Environment.OSVersion.Version.CompareTo(OsMinVersion));
```

Observe que seu aplicativo não detectará com êxito o Windows 10 se ele não estiver listado como um sistema operacional com suporte no manifesto do aplicativo.

Você também pode verificar a versão do .NET Framework em que o aplicativo foi criado:

```csharp
Console.WriteLine(AppDomain.CurrentDomain.SetupInformation.TargetFrameworkName);
```

## <a name="see-also"></a>Veja também

- [Windows Forms adicionar elemento de configuração](../configure-apps/file-schema/winforms/windows-forms-add-configuration-element.md)
- [Ajustar o tamanho e a escala do Windows Forms](adjusting-the-size-and-scale-of-windows-forms.md)
