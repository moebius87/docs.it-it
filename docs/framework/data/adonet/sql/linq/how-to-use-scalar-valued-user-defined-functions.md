---
title: "Procedura: Usare funzioni definite dall'utente con valori scalari"
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 714e252f-c053-4bbb-b1f3-924111cd4d97
ms.openlocfilehash: ffb24468c81cb4ec9f41645f8888c2c4ba021609
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "62033579"
---
# <a name="how-to-use-scalar-valued-user-defined-functions"></a>Procedura: Usare funzioni definite dall'utente con valori scalari
È possibile eseguire il mapping di un metodo client definito in una classe a una funzione definita dall'utente usando l'attributo <xref:System.Data.Linq.Mapping.FunctionAttribute>. Notare che nel corpo del metodo viene costruita un'espressione che acquisisce lo scopo della chiamata al metodo e passa quell'espressione a <xref:System.Data.Linq.DataContext> per la conversione e l'esecuzione.  
  
> [!NOTE]
>  Si verifica l'esecuzione diretta solo se la funzione viene chiamata all'esterno di una query. Per altre informazioni, vedere [Procedura: Chiamare funzioni definite dall'utente Inline](../../../../../../docs/framework/data/adonet/sql/linq/how-to-call-user-defined-functions-inline.md).  
  
## <a name="example"></a>Esempio  
 Nel codice SQL seguente viene presentata una funzione con valori scalari definita dall'utente `ReverseCustName()`.  
  
```  
CREATE FUNCTION ReverseCustName(@string varchar(100))  
RETURNS varchar(100)  
AS  
BEGIN  
    DECLARE @custName varchar(100)  
    -- Implementation left as exercise for users.  
    RETURN @custName  
END  
```  
  
 Per questo codice si eseguirà il mapping di un metodo client come il seguente:  
  
 [!code-csharp[DLinqUDFS#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqUDFS/cs/northwind-tfunc.cs#3)]
 [!code-vb[DLinqUDFS#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqUDFS/vb/northwind-tfunc.vb#3)]  
  
## <a name="see-also"></a>Vedere anche

- [Funzioni definite dall'utente](../../../../../../docs/framework/data/adonet/sql/linq/user-defined-functions.md)
