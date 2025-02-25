---
title: 'Procedura: Creare oggetti GenericPrincipal e GenericIdentity'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Creating Generic Identity Objects
- GenericPrincipal Objects
- Creating GenericPrincipal Objects
- GenericIdentity Objects
ms.assetid: 465694cf-258b-4747-9dae-35b01a5bcdbb
author: mairaw
ms.author: mairaw
ms.openlocfilehash: b47f4c093acb094188cbd5a8a0a0026c67eb3f2c
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61795161"
---
# <a name="how-to-create-genericprincipal-and-genericidentity-objects"></a>Procedura: Creare oggetti GenericPrincipal e GenericIdentity

È possibile usare la <xref:System.Security.Principal.GenericIdentity> classe in combinazione con il <xref:System.Security.Principal.GenericPrincipal> classe per creare uno schema di autorizzazione che esista indipendentemente da un dominio di Windows.

### <a name="to-create-a-genericprincipal-object"></a>Per creare un oggetto GenericPrincipal

1. Creare una nuova istanza della classe di identità e inizializzarla con il nome che si desidera conservare. Il codice seguente crea un nuovo oggetto **GenericIdentity** e lo inizializza con il nome `MyUser`.

    ```vb
    Dim myIdentity As New GenericIdentity("MyUser")
    ```

    ```csharp
    GenericIdentity myIdentity = new GenericIdentity("MyUser");
    ```

2. Creare una nuova istanza della classe **GenericPrincipal** e inizializzarla con l'oggetto **GenericIdentity** creato in precedenza e una matrice di stringhe che rappresenta i ruoli che si desidera associare a questa entità. L'esempio di codice seguente specifica una matrice di stringhe che rappresenta un ruolo amministratore e un ruolo utente. Il **GenericPrincipal** viene quindi inizializzato con il precedente **GenericIdentity** e la matrice di stringhe.

    ```vb
    Dim myStringArray As String() = {"Manager", "Teller"}
    DIm myPrincipal As New GenericPrincipal(myIdentity, myStringArray)
    ```

    ```csharp
    String[] myStringArray = {"Manager", "Teller"};
    GenericPrincipal myPrincipal = new GenericPrincipal(myIdentity, myStringArray);
    ```

3. Usare il codice seguente per allegare l'entità al thread attuale. Questo è particolarmente utile nelle situazioni in cui l'entità deve essere convalidata più volte, deve essere convalidata da un altro codice in esecuzione nell'applicazione o deve essere convalidata da un <xref:System.Security.Permissions.PrincipalPermission> oggetto. Si può comunque eseguire la convalida basata sui ruoli sull'oggetto entità senza allegarlo al thread. Per maggiori informazioni, vedere [Sostituzione di oggetti Principal](../../../docs/standard/security/replacing-a-principal-object.md).

    ```vb
    Thread.CurrentPrincipal = myPrincipal
    ```

    ```csharp
    Thread.CurrentPrincipal = myPrincipal;
    ```

## <a name="example"></a>Esempio

L'esempio di codice seguente illustra come creare un'istanza di **GenericPrincipal** e di **GenericIdentity**. Questo codice visualizza i valori di questi oggetti nella console.

```vb
Imports System
Imports System.Security.Principal
Imports System.Threading

Public Class Class1

    Public Shared Sub Main()
        ' Create generic identity.
        Dim myIdentity As New GenericIdentity("MyIdentity")

        ' Create generic principal.
        Dim myStringArray As String() =  {"Manager", "Teller"}
        Dim myPrincipal As New GenericPrincipal(myIdentity, myStringArray)

        ' Attach the principal to the current thread.
        ' This is not required unless repeated validation must occur,
        ' other code in your application must validate, or the
        ' PrincipalPermission object is used.
        Thread.CurrentPrincipal = myPrincipal

        ' Print values to the console.
        Dim name As String = myPrincipal.Identity.Name
        Dim auth As Boolean = myPrincipal.Identity.IsAuthenticated
        Dim isInRole As Boolean = myPrincipal.IsInRole("Manager")

        Console.WriteLine("The name is: {0}", name)
        Console.WriteLine("The isAuthenticated is: {0}", auth)
        Console.WriteLine("Is this a Manager? {0}", isInRole)

    End Sub

End Class
```

```csharp
using System;
using System.Security.Principal;
using System.Threading;

public class Class1
{
    public static int Main(string[] args)
    {
    // Create generic identity.
    GenericIdentity myIdentity = new GenericIdentity("MyIdentity");

    // Create generic principal.
    String[] myStringArray = {"Manager", "Teller"};
    GenericPrincipal myPrincipal =
        new GenericPrincipal(myIdentity, myStringArray);

    // Attach the principal to the current thread.
    // This is not required unless repeated validation must occur,
    // other code in your application must validate, or the
    // PrincipalPermission object is used.
    Thread.CurrentPrincipal = myPrincipal;

    // Print values to the console.
    String name =  myPrincipal.Identity.Name;
    bool auth =  myPrincipal.Identity.IsAuthenticated;
    bool isInRole =  myPrincipal.IsInRole("Manager");

    Console.WriteLine("The name is: {0}", name);
    Console.WriteLine("The isAuthenticated is: {0}", auth);
    Console.WriteLine("Is this a Manager? {0}", isInRole);

    return 0;
    }
}
```

In esecuzione, l'applicazione visualizza un output simile al seguente.

```
The Name is: MyIdentity
The IsAuthenticated is: True
Is this a Manager? True
```

## <a name="see-also"></a>Vedere anche

- <xref:System.Security.Principal.GenericIdentity>
- <xref:System.Security.Principal.GenericPrincipal>
- <xref:System.Security.Permissions.PrincipalPermission>
- [Sostituzione di oggetti Principal](../../../docs/standard/security/replacing-a-principal-object.md)
- [Oggetti Principal e Identity](../../../docs/standard/security/principal-and-identity-objects.md)
