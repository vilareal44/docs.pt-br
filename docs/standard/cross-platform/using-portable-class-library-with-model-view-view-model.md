---
title: Usando a Biblioteca de Classes Portátil com Modelo MVVM
ms.date: 07/18/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Portable Class Library [.NET Framework], and MVVM
- MVVM, and Portable Class Library
ms.assetid: 41a0b9f8-15a2-431a-bc35-e310b2953b03
ms.openlocfilehash: ff34b295ba443088115d470d8ade0c986ac1d856
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288843"
---
# <a name="using-portable-class-library-with-model-view-view-model"></a>Usando a Biblioteca de Classes Portátil com Modelo MVVM
Você pode usar a .NET Framework [biblioteca de classes portátil](cross-platform-development-with-the-portable-class-library.md) para implementar o padrão MVVM (Model-View-View Model) e compartilhar assemblies em várias plataformas.

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]

 MVVM é um padrão de aplicativo que isola a interface do usuário da lógica de negócios subjacente. Você pode implementar as classes modelo e exibir modelo em um projeto de biblioteca de classes portátil no Visual Studio 2012 e, em seguida, criar exibições que são personalizadas para diferentes plataformas. Essa abordagem permite que você grave o modelo de dados e a lógica de negócios somente uma vez e use esse código em aplicativos de .NET Framework, Silverlight, Windows Phone e Windows 8. x da loja, conforme mostrado na ilustração a seguir.

 ![Mostra a biblioteca de classes portátil com o MVVM Sharing assemblies entre plataformas.](./media/using-portable-class-library-with-model-view-view-model/mvvm-share-assemblies-across-platforms.png)

 Este tópico não fornece informações gerais sobre o padrão MVVM. Ele fornece apenas informações sobre como usar a biblioteca de classes portátil para implementar o MVVM. Para obter mais informações sobre o MVVM, consulte o [MVVM QuickStart usando a biblioteca Prism 5,0 para WPF](https://docs.microsoft.com/previous-versions/msp-n-p/gg430857(v=pandp.40)).

## <a name="classes-that-support-mvvm"></a>Classes que dão suporte a MVVM
 Quando você direciona o .NET Framework 4,5, .NET para aplicativos da loja do Windows 8. x, Silverlight ou Windows Phone 7,5 para seu projeto de biblioteca de classes portátil, as seguintes classes estão disponíveis para implementar o padrão MVVM:

- Classe <xref:System.Collections.ObjectModel.ObservableCollection%601?displayProperty=nameWithType>

- Classe <xref:System.Collections.ObjectModel.ReadOnlyObservableCollection%601?displayProperty=nameWithType>

- Classe <xref:System.Collections.Specialized.INotifyCollectionChanged?displayProperty=nameWithType>

- Classe <xref:System.Collections.Specialized.NotifyCollectionChangedAction?displayProperty=nameWithType>

- Classe <xref:System.Collections.Specialized.NotifyCollectionChangedEventArgs?displayProperty=nameWithType>

- Classe <xref:System.Collections.Specialized.NotifyCollectionChangedEventHandler?displayProperty=nameWithType>

- Classe <xref:System.ComponentModel.DataErrorsChangedEventArgs?displayProperty=nameWithType>

- Classe <xref:System.ComponentModel.INotifyDataErrorInfo?displayProperty=nameWithType>

- Classe <xref:System.ComponentModel.INotifyPropertyChanged?displayProperty=nameWithType>

- Classe <xref:System.Windows.Input.ICommand?displayProperty=nameWithType>

- Todas as classes no <xref:System.ComponentModel.DataAnnotations?displayProperty=nameWithType> namespace

## <a name="implementing-mvvm"></a>Implementando o MVVM
 Para implementar o MVVM, você normalmente cria o modelo e o modelo de exibição em um projeto de biblioteca de classes portátil, pois um projeto de biblioteca de classes portátil não pode fazer referência a um projeto não portátil. O modelo e o modelo de exibição podem estar no mesmo projeto ou em projetos separados. Se você usar projetos separados, adicione uma referência do projeto de modelo de exibição ao projeto de modelo.

 Depois de compilar o modelo e exibir projetos de modelo, você faz referência a esses assemblies no aplicativo que contém a exibição. Se a exibição interage apenas com o modelo de exibição, você só precisa referenciar o assembly que contém o modelo de exibição.

### <a name="model"></a>Modelo
 O exemplo a seguir mostra uma classe de modelo simplificada que pode residir em um projeto de biblioteca de classes portátil.

 [!code-csharp[PortableClassLibraryMVVM#1](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/customer.cs#1)]
 [!code-vb[PortableClassLibraryMVVM#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customer.vb#1)]

 O exemplo a seguir mostra uma maneira simples de popular, recuperar e atualizar os dados em um projeto de biblioteca de classes portátil. Em um aplicativo real, você recuperaria os dados de uma fonte, como um serviço Windows Communication Foundation (WCF).

 [!code-csharp[PortableClassLibraryMVVM#2](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/customerrepository.cs#2)]
 [!code-vb[PortableClassLibraryMVVM#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customerrepository.vb#2)]

### <a name="view-model"></a>Exibir modelo
 Uma classe base para modelos de exibição é frequentemente adicionada ao implementar o padrão MVVM. O exemplo a seguir mostra uma classe base.

 [!code-csharp[PortableClassLibraryMVVM#3](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/viewmodelbase.cs#3)]
 [!code-vb[PortableClassLibraryMVVM#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/viewmodelbase.vb#3)]

 Uma implementação da <xref:System.Windows.Input.ICommand> interface é frequentemente usada com o padrão MVVM. O exemplo a seguir mostra uma implementação da interface <xref:System.Windows.Input.ICommand>.

 [!code-csharp[PortableClassLibraryMVVM#4](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/relaycommand.cs#4)]
 [!code-vb[PortableClassLibraryMVVM#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/relaycommand.vb#4)]

 O exemplo a seguir mostra um modelo de exibição simplificado.

 [!code-csharp[PortableClassLibraryMVVM#5](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/mainpageviewmodel.cs#5)]
 [!code-vb[PortableClassLibraryMVVM#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customerviewmodel.vb#5)]  
  
### <a name="view"></a>Visualizar  
 De um aplicativo .NET Framework 4,5, aplicativo da loja do Windows 8. x, aplicativo baseado no Silverlight ou Windows Phone aplicativo 7,5, você pode fazer referência ao assembly que contém os projetos modelo e exibir modelo.  Em seguida, você cria um modo de exibição que interage com o modelo de exibição. O exemplo a seguir mostra um aplicativo de Windows Presentation Foundation simplificado (WPF) que recupera e atualiza dados do modelo de exibição. Você pode criar exibições semelhantes nos aplicativos do Silverlight, Windows Phone ou Windows 8. x da loja.  
  
 [!code-xaml[PortableClassLibraryMVVM#6](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/mainwindow.xaml#6)]  
  
## <a name="see-also"></a>Veja também

- [Biblioteca de Classes Portátil](cross-platform-development-with-the-portable-class-library.md)
