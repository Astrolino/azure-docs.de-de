---
title: Anzeigen verknüpfter Datenobjekte in Azure Data Catalog
description: Dieser Artikel beschreibt, wie Sie in Azure Data Catalog die verknüpften Datenobjekte eines ausgewählten Datenobjekts anzeigen.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 156673bfac9bfa38772e4daca166e3431f81c09a
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405006"
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a>Anzeigen verknüpfter Datenobjekte in Azure Data Catalog
Azure Data Catalog ermöglicht Ihnen das Anzeigen von Datenobjekten, die mit einem ausgewählten Datenobjekt verknüpft sind, und der Beziehungen zwischen beiden. 

## <a name="supported-data-sources"></a>Unterstützte Datenquellen 
Wenn Sie Datenobjekte aus den folgenden Datenquellen registrieren, registriert Azure Data Catalog automatisch Metadaten zu den Verknüpfungsbeziehungen zwischen den ausgewählten Datenobjekten. 

- SQL Server
- Azure SQL-Datenbank
- MySQL
- Oracle

> [!NOTE]
> Damit Data Catalog die Beziehung zwischen zwei Datenobjekten importiert, müssen Sie beide Objekte zum gleichen Zeitpunkt registrieren. Wenn Sie ein Objekt separat hinzugefügt haben, fügen Sie dieses und das andere Datenobjekt erneut hinzu, um die Beziehung zwischen ihnen zu importieren.

## <a name="view-related-data-assets"></a>Anzeigen verknüpfter Datenobjekte
Verwenden Sie zum Anzeigen von Datenobjekten, die mit einem ausgewählten Dataset verknüpft sind, die Registerkarte **Beziehungen** wie in der folgenden Abbildung gezeigt: 

![Azure Data Catalog: Anzeigen verknüpfter Datenobjekte](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

In diesem Beispiel, es gibt zwei Beziehungen für das ausgewählte Datenobjekt **ProductSubCategory**: 

- Die Spalte „ProductSubcategoryID“ der Tabelle „Product“ hat eine Fremdschlüsselbeziehung mit der Spalte „ProductSubcategoryID“ der ausgewählten Tabelle „ProductSubcategory“. 
- Die Spalte „ProductCategoryID“ der Tabelle „ProductSubCategory“ hat eine Fremdschlüsselbeziehung mit der Spalte „ProductCategoryID“ der ausgewählten Tabelle „ProductCategory“.

> [!NOTE]
> Beachten Sie die Richtung des Pfeils in der Strukturansicht der Beziehungen.  

Um weitere Details anzuzeigen, wie z.B. den vollqualifizierten Namen der Spalte, bewegen Sie den Mauszeiger darüber, woraufhin wie in der folgenden Abbildung ein Popup eingeblendet wird: 

![Azure Data Catalog: Popup zu Beziehungen](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

Um Beziehungen zwischen Objekte einzuschließen, die bereits registriert wurden, müssen Sie diese Objekte erneut registrieren.

## <a name="next-steps"></a>Nächste Schritte
- [Verwalten von Datenassets](data-catalog-how-to-manage.md)