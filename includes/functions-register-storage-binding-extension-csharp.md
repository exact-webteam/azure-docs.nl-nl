---
ms.openlocfilehash: 1dd9e377a88d8b35cc5ad43f3e8dd1c48dc35d7c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "78201946"
---
::: zone pivot="programming-language-csharp"  
## <a name="register-binding-extensions"></a>Binding-extensies registreren

Met uitzondering van HTTP- en timertriggers worden bindingen geïmplementeerd als uitbreidingspakketten. Voer de volgende [dotnet add package](/dotnet/core/tools/dotnet-add-package)-opdracht in het terminalvenster uit om het Storage-extensiepakket toe te voegen aan uw project.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

Nu kunt u de Storage-uitvoerbinding toevoegen aan uw project.  
::: zone-end  