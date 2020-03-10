---
uid: web-forms/videos/how-do-i/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel
title: '[Comment faire :] Exporter des données vers un fichier délimité par des virgules (CSV) pour une application comme Excel | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo, Chris pixels montre comment prendre des données à partir d’une base de données ou d’une autre source et les exporter vers un fichier délimité par des virgules qui peut être utilisé dans une application Li...
ms.author: riande
ms.date: 01/22/2009
ms.assetid: c9df86ad-aec2-43d5-bb8a-413ebb666673
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel
msc.type: video
ms.openlocfilehash: f27873eee345fe5347023b154de2b3833c9b6138
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78567899"
---
# <a name="how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel"></a><span data-ttu-id="bc93b-103">[Comment faire :] Exporter des données vers un fichier délimité par des virgules (CSV) pour une application comme Excel</span><span class="sxs-lookup"><span data-stu-id="bc93b-103">[How Do I:] Export Data to a Comma Delimited (CSV) File for an Application Like Excel</span></span>

<span data-ttu-id="bc93b-104">par [Chris pixels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="bc93b-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="bc93b-105">Dans cette vidéo, Chris pixels montre comment prendre des données à partir d’une base de données ou d’une autre source et les exporter vers un fichier délimité par des virgules qui peut être utilisé dans une application comme Excel.</span><span class="sxs-lookup"><span data-stu-id="bc93b-105">In this video Chris Pels shows how to take data from a database or other source and export it to a comma delimited file that can be used in an application like Excel.</span></span> <span data-ttu-id="bc93b-106">Tout d’abord, un ensemble de données est créé en tant qu’objet DataTable.</span><span class="sxs-lookup"><span data-stu-id="bc93b-106">First, a set of data is created as a DataTable object.</span></span> <span data-ttu-id="bc93b-107">Ensuite, la réponse à la demande de page Web actuelle est effacée et l’en-tête et le type de contenu sont configurés pour être un fichier CSV.</span><span class="sxs-lookup"><span data-stu-id="bc93b-107">Next, the Response for the current web page request is cleared and the header and content type are configured to be a csv file.</span></span> <span data-ttu-id="bc93b-108">Ensuite, les données réelles sont ajoutées au flux de réponse en écrivant d’abord les en-têtes de colonnes du fichier CSV suivi des valeurs de données.</span><span class="sxs-lookup"><span data-stu-id="bc93b-108">Then the actual data is added to the response stream by first writing the column headers for the csv file followed by the data values.</span></span> <span data-ttu-id="bc93b-109">Cette approche peut être utile lorsque les utilisateurs requièrent une exportation de données afin qu’elles puissent être manipulées localement dans un programme tel qu’Excel.</span><span class="sxs-lookup"><span data-stu-id="bc93b-109">This approach can be useful when users require an export of data so it can be manipulated locally in a program like Excel.</span></span>

[<span data-ttu-id="bc93b-110">&#9654;Regarder la vidéo (19 minutes)</span><span class="sxs-lookup"><span data-stu-id="bc93b-110">&#9654; Watch video (19 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel)
