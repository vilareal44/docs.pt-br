---
title: Propriedades de Dependência
ms.date: 10/22/2008
ms.technology: dotnet-standard
ms.assetid: 212cfb1e-cec4-4047-94a6-47209b387f6f
ms.openlocfilehash: 476ef1bb1ac5ec1f551979c64a41cbae55c554bc
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84280251"
---
# <a name="dependency-properties"></a>Propriedades de Dependência
Uma propriedade de dependência (DP) é uma propriedade regular que armazena seu valor em um repositório de propriedades, em vez de armazená-la em uma variável de tipo (campo), por exemplo.

 Uma propriedade de dependência anexada é um tipo de propriedade de dependência modelada como métodos get e Set estáticos que representam "Propriedades" que descrevem as relações entre objetos e seus contêineres (por exemplo, a posição de um `Button` objeto em um `Panel` contêiner).

 ✔️ fornecer as propriedades de dependência, se você precisar das propriedades para oferecer suporte a recursos do WPF, como estilo, gatilhos, vinculação de dados, animações, recursos dinâmicos e herança.

## <a name="dependency-property-design"></a>Design de propriedade de dependência
 ✔️ herdar de <xref:System.Windows.DependencyObject> , ou de um de seus subtipos, ao implementar propriedades de dependência. O tipo fornece uma implementação muito eficiente de um repositório de propriedades e oferece suporte automático à ligação de dados do WPF.

 ✔️ fornecem uma propriedade CLR regular e um campo somente leitura estático público que armazena uma instância do <xref:System.Windows.DependencyProperty?displayProperty=nameWithType> para cada propriedade de dependência.

 ✔️ implementar propriedades de dependência chamando métodos de instância <xref:System.Windows.DependencyObject.GetValue%2A?displayProperty=nameWithType> e <xref:System.Windows.DependencyObject.SetValue%2A?displayProperty=nameWithType> .

 ✔️ nomear o campo estático da propriedade de dependência, sufixando o nome da propriedade com "Property".

 ❌Não defina valores padrão de propriedades de dependência explicitamente no código; em vez disso, defina-os nos metadados.

 Se você definir uma propriedade padrão explicitamente, poderá impedir que essa propriedade seja definida por alguns meios implícitos, como um estilo.

 ❌Não coloque o código nos acessadores de propriedade que não sejam o código padrão para acessar o campo estático.

 Esse código não será executado se a propriedade for definida por meios implícitos, como um estilo, porque o estilo usa o campo estático diretamente.

 ❌Não use Propriedades de dependência para armazenar dados seguros. Até mesmo as propriedades de dependência privada podem ser acessadas publicamente.

## <a name="attached-dependency-property-design"></a>Design de propriedade de dependência anexada
 As propriedades de dependência descritas na seção anterior representam propriedades intrínsecas do tipo declarativo; por exemplo, a `Text` propriedade é uma propriedade de `TextButton` , que a declara. Um tipo especial de propriedade de dependência é a propriedade de dependência anexada.

 Um exemplo clássico de uma propriedade anexada é a <xref:System.Windows.Controls.Grid.Column%2A?displayProperty=nameWithType> propriedade. A propriedade representa a posição da coluna do botão (não da grade), mas só será relevante se o botão estiver contido em uma grade e, portanto, "anexado" a botões por grades.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>

    <Button Grid.Column="0">Click</Button>
    <Button Grid.Column="1">Clack</Button>
</Grid>
```

 A definição de uma propriedade anexada é parecida com a de uma propriedade de dependência regular, exceto que os acessadores são representados pelos métodos get e Set estáticos:

```csharp
public class Grid {

    public static int GetColumn(DependencyObject obj) {
        return (int)obj.GetValue(ColumnProperty);
    }

    public static void SetColumn(DependencyObject obj, int value) {
        obj.SetValue(ColumnProperty,value);
    }

    public static readonly DependencyProperty ColumnProperty =
        DependencyProperty.RegisterAttached(
            "Column",
            typeof(int),
            typeof(Grid)
    );
}
```

## <a name="dependency-property-validation"></a>Validação de propriedade de dependência
 As propriedades geralmente implementam o que é chamado de validação. A lógica de validação é executada quando é feita uma tentativa de alterar o valor de uma propriedade.

 Infelizmente, os acessadores de propriedade de dependência não podem conter código de validação arbitrário. Em vez disso, a lógica de validação da propriedade de dependência precisa ser especificada durante o registro da propriedade.

 ❌Não coloque a lógica de validação da propriedade de dependência nos acessadores da propriedade. Em vez disso, passe um retorno de chamada de validação para o `DependencyProperty.Register` método.

## <a name="dependency-property-change-notifications"></a>Notificações de alteração de propriedade de dependência
 ❌Não implemente a lógica de notificação de alteração em acessadores de propriedade de dependência. As propriedades de dependência têm um recurso interno de notificações de alteração que deve ser usado ao fornecer um retorno de chamada de notificação de alteração para o <xref:System.Windows.PropertyMetadata> .

## <a name="dependency-property-value-coercion"></a>Coerção de valor de propriedade de dependência
 A coerção de propriedade ocorre quando o valor fornecido a um setter de propriedade é modificado pelo setter antes que o repositório de propriedades seja realmente modificado.

 ❌Não implemente a lógica de coerção em acessadores de propriedade de dependência.

 As propriedades de dependência têm um recurso de coerção interno e podem ser usadas fornecendo um retorno de chamada de coerção para o `PropertyMetadata` .

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de estrutura](index.md)
- [Padrões de design comuns](common-design-patterns.md)
