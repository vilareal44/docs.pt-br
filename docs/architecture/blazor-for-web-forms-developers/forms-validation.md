---
title: Formulários e validação
description: Saiba como criar formulários com validação no lado do cliente no Blazor .
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- Blazor WebAssembly
ms.date: 09/19/2019
ms.openlocfilehash: d2dce23996e996a736b04c9cdd1ccf3b549ff3ff
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267549"
---
# <a name="forms-and-validation"></a>Formulários e validação

O ASP.NET Web Forms Framework inclui um conjunto de controles de servidor de validação que lidam com a validação da entrada do usuário inserida em um formulário ( `RequiredFieldValidator` ,, `CompareValidator` `RangeValidator` e assim por diante). O ASP.NET Web Forms Framework também dá suporte à associação de modelo e à validação do modelo com base em anotações de dados ( `[Required]` ,, `[StringLength]` `[Range]` e assim por diante). A lógica de validação pode ser imposta tanto no servidor quanto no cliente usando a validação não invasiva baseada em JavaScript. O `ValidationSummary` controle de servidor é usado para exibir um resumo dos erros de validação para o usuário.

Blazor dá suporte ao compartilhamento de lógica de validação entre o cliente e o servidor. O ASP.NET fornece implementações de JavaScript predefinidas de muitas validações de servidor comuns. Em muitos casos, o desenvolvedor ainda precisa escrever JavaScript para implementar totalmente a lógica de validação específica do aplicativo. Os mesmos tipos de modelo, anotações de dados e lógica de validação podem ser usados no servidor e no cliente.

Blazor fornece um conjunto de componentes de entrada. Os componentes de entrada lidam com os dados do campo de associação a um modelo e validam a entrada do usuário quando o formulário é enviado.

|Componente de entrada|Elemento HTML renderizado    |
|---------------|-------------------------|
|`InputCheckbox`|`<input type="checkbox">`|
|`InputDate`    |`<input type="date">`    |
|`InputNumber`  |`<input type="number">`  |
|`InputSelect`  |`<select>`               |
|`InputText`    |`<input>`                |
|`InputTextArea`|`<textarea>`             |

O `EditForm` componente encapsula esses componentes de entrada e orquestra o processo de validação por meio de um `EditContext` . Ao criar um `EditForm` , especifique a instância de modelo a ser associada usando o `Model` parâmetro. Normalmente, a validação é feita usando as anotações de dados e é extensível. Para habilitar a validação baseada em anotação de dados, adicione o `DataAnnotationsValidator` componente como um filho do `EditForm` . O `EditForm` componente fornece um evento conveniente para lidar com `OnValidSubmit` envios válidos () e inválidos ( `OnInvalidSubmit` ). Há também um evento mais genérico `OnSubmit` que permite disparar e lidar com a validação por conta própria.

Para exibir um resumo de erro de validação, use o `ValidationSummary` componente. Para exibir mensagens de validação para um campo de entrada específico, use o `ValidationMessage` componente, especificando uma expressão lambda para o `For` parâmetro que aponta para o membro de modelo apropriado.

O tipo de modelo a seguir define várias regras de validação usando anotações de dados:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16,
        ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000,
        ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true",
        ErrorMessage = "This form disallows unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

O componente a seguir demonstra como criar um formulário Blazor com base no `Starship` tipo de modelo:

```razor
<h1>New Ship Entry Form</h1>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" @bind-Value="starship.Identifier" />
        <ValidationMessage For="() => starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea id="description" @bind-Value="starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" @bind-Value="starship.Classification">
            <option value="">Select classification ...</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
            <option value="Defense">Defense</option>
        </InputSelect>
        <ValidationMessage For="() => starship.Classification" />
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" @bind-Value="starship.MaximumAccommodation" />
        <ValidationMessage For="() => starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" @bind-Value="starship.IsValidatedDesign" />
        <ValidationMessage For="() => starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate id="productionDate" @bind-Value="starship.ProductionDate" />
        <ValidationMessage For="() => starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>
</EditForm>

@code {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        // Save the data
    }
}
```

Após o envio do formulário, os dados associados ao modelo não foram salvos em nenhum armazenamento de dados, como um banco de dado. Em um Blazor WebAssembly aplicativo, os dados devem ser enviados para o servidor. Por exemplo, usando uma solicitação HTTP POST. Em um Blazor aplicativo de servidor, os dados já estão no servidor, mas devem persistir. O tratamento de acesso Blazor a dados em aplicativos é o assunto da seção [lidando com dados](data.md) .

## <a name="additional-resources"></a>Recursos adicionais

Para obter mais informações sobre [formulários e validação](/aspnet/core/blazor/forms-validation) em Blazor aplicativos, consulte a Blazor documentação.

>[!div class="step-by-step"]
>[Anterior](state-management.md) 
> [Avançar](data.md)
