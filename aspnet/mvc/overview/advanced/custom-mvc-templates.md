---
uid: mvc/overview/advanced/custom-mvc-templates
title: Modèle MVC personnalisé | Microsoft Docs
author: joeloff
description: Créez un modèle en tant qu’extension VSIX.
ms.author: riande
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 0603bc24e070e223551813f66a75889a2e46fd35
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616353"
---
# <a name="custom-mvc-template"></a>Modèle MVC personnalisé

par [Jacques Eloff](https://github.com/joeloff)

La publication de la mise à jour des outils MVC 3 pour Visual Studio 2010 a introduit un Assistant projet distinct pour les projets MVC. La modification a été déclenchée par deux facteurs. Tout d’abord, l’introduction de nouveaux modèles dans MVC 3 et la prise en charge de moteurs d’affichage supplémentaires tels que Razor entraînent la surcharge de la boîte de dialogue Nouveau projet dans Visual Studio. Deuxièmement, les clients ont demandé des points d’extensibilité et l’Assistant Nouveau projet MVC nous permettrait de répondre à ces demandes.

L’ajout de modèles personnalisés était un processus ardu reposant sur l’utilisation du Registre pour rendre visibles de nouveaux modèles dans l’Assistant projet MVC. L’auteur d’un nouveau modèle devait l’encapsuler dans un MSI pour s’assurer que les entrées de Registre nécessaires seraient créées au moment de l’installation. L’alternative consistait à créer un fichier ZIP contenant le modèle disponible et à faire en sorte que l’utilisateur final crée manuellement les entrées de Registre requises.

Aucune des approches mentionnées ci-dessus n’est idéale. nous avons donc décidé de tirer parti de l’infrastructure existante fournie par les extensions [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) pour faciliter la création, la distribution et l’installation de modèles MVC personnalisés à partir de MVC 4 pour Visual Studio 2012. Voici quelques-uns des avantages offerts par cette approche :

- Une extension VSIX peut contenir plusieurs modèles qui prennent en charge desC# langages différents (et Visual Basic) et plusieurs moteurs d’affichage (aspx et Razor).
- Une extension VSIX peut cibler plusieurs références SKU de Visual Studio, y compris les références SKU Express.
- La [Galerie Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) facilite la distribution de l’extension à un public étendu.
- Les extensions VSIX peuvent être mises à niveau, ce qui facilite la création de corrections et de mises à jour de vos modèles personnalisés.

## <a name="prerequisites"></a>Conditions préalables requises

- Les utilisateurs doivent être familiarisés avec la création de modèles de projet, y compris le balisage requis pour les fichiers vstemplate, etc.
- Visual Studio Professional et versions ultérieures doivent être installés sur les utilisateurs. Les références SKU Express ne prennent pas en charge la création de projets VSIX.
- Le [Kit de développement logiciel (SDK) Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30668) est installé.

## <a name="example"></a>Exemple

La première étape consiste à créer un projet VSIX à l’aide C# de ou de Visual Basic. Sélectionnez **fichier > nouveau projet**, puis cliquez sur **extensibilité** dans le volet gauche et sélectionnez le **projet VSIX**.

![Nouveau projet](custom-mvc-templates/_static/image1.jpg)

Une fois le projet créé, le concepteur VSIX s’ouvre.

![Métadonnées du concepteur de projets](custom-mvc-templates/_static/image2.jpg)

Le concepteur peut être utilisé pour modifier certaines des propriétés générales de l’extension qui seront affichées aux utilisateurs lors de l’installation de l’extension ou parcourir les extensions installées dans Visual Studio (**outils > extensions et mises à jour**). Une fois que vous avez terminé les informations générales, cliquez sur l' **onglet installer les cibles**.

![Project Designer-cibles d’installation](custom-mvc-templates/_static/image3.jpg)

Cet onglet permet de spécifier les références (SKU) et les versions de Visual Studio prises en charge par votre extension. Activez la case à cocher pour que **ce VSIX soit installé pour tous les utilisateurs** afin d’activer les installations par ordinateur de l’extension VSIX. Cliquez sur le bouton **nouveau** sur la droite pour ajouter des références SKU supplémentaires, telles que Web Developer Express (VWD existant).

![Ajouter une nouvelle cible d’installation](custom-mvc-templates/_static/image4.jpg)

Si vous envisagez de prendre en charge toutes les références SKU professionnelles et supérieures (Professional, Premium et Ultimate), vous devez uniquement sélectionner la référence SKU minimale de la famille **Microsoft.VisualStudio.Pro**. N’oubliez pas d’enregistrer toutes vos modifications une fois que vous avez terminé les cibles d’installation.

![Project Designer-cibles d’installation](custom-mvc-templates/_static/image5.jpg)

L’onglet **ressources** permet d’ajouter tous vos fichiers de contenu à l’extension VSIX. Comme MVC nécessite des métadonnées personnalisées, vous allez modifier le code XML brut du fichier manifeste VSIX au lieu d’utiliser l’onglet **ressources** pour ajouter du contenu. Commencez par ajouter le contenu du modèle au projet VSIX. Il est important que la structure du dossier et le contenu reflètent la disposition du projet. L’exemple ci-dessous contient quatre modèles de projet dérivés du modèle de projet MVC de base. Assurez-vous que tous les fichiers qui composent votre modèle de projet (tout ce qui se trouve sous le dossier ProjectTemplates) sont ajoutés au ItemGroup de **contenu** dans le fichier projet VSIX et que chaque élément contient les métadonnées **CopyToOutputDirectory** et **IncludeInVsix** définies comme indiqué dans l’exemple ci-dessous.

&lt;contenu include =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;toujours&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/content&gt;

Si ce n’est pas le cas, l’IDE tente de compiler le contenu du modèle lorsque vous générez le VSIX, et vous verrez probablement une erreur. Les fichiers de code des modèles contiennent souvent des [paramètres de modèle](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) spéciaux utilisés par Visual Studio lorsque le modèle de projet est instancié et ne peuvent donc pas être compilés dans l’IDE.

![l'Explorateur de solutions](custom-mvc-templates/_static/image6.jpg)

Fermez le concepteur VSIX, cliquez avec le bouton droit sur le fichier **source. extension. manifest** dans **Explorateur de solutions** , puis sélectionnez **Ouvrir avec** et choisissez l’option **éditeur XML (texte)** .

![Ouvrir avec la boîte de dialogue](custom-mvc-templates/_static/image7.jpg)

Créez un élément **&lt;ressources&gt;** et ajoutez un élément **&lt;Asset&gt;** pour chaque fichier qui doit être inclus dans le VSIX. L’attribut de **type** de chaque élément de **&gt;&lt;Asset** doit être défini sur **Microsoft. VisualStudio. Mvc. Template**. Il s’agit d’un espace de noms personnalisé que seul l’Assistant projet MVC comprend. Reportez-vous à la documentation du schéma VSIX 2,0 pour plus d’informations sur la structure et la mise en page du fichier manifeste.

L’ajout des fichiers à VSIX n’est pas suffisant pour inscrire les modèles à l’aide de l’Assistant MVC. Vous devez fournir des informations telles que le nom du modèle, la description, les moteurs d’affichage pris en charge et le langage de programmation à l’Assistant MVC. Ces informations sont transmises dans des attributs personnalisés associés à l’élément **&lt;&gt;Asset** pour chaque fichier **VSTemplate** .

&lt;Asset d :VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type =&quot;Microsoft. VisualStudio. Mvc. Template&quot;

d :Source =&quot;&quot; de fichier

Path =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType =&quot;MVC&quot;

Language =&quot;C#&quot;

Moteur =&quot;aspx&quot;

TemplateId =&quot;MyMvcApplication&quot;

Title =&quot;&quot; d’application Web de base personnalisée

Description =&quot;un modèle personnalisé dérivé d’une application Web MVC de base (Razor)&quot;

Version =&quot;4,0&quot;/&gt;

Vous trouverez ci-dessous une explication des attributs personnalisés qui doivent être présents :

- **ProjectType** doit avoir la valeur Mvc.
- La **langue** désigne le langage de développement pris en charge par le modèle. Les valeurs valides C# sont ou VB.
- **Moteur** désigne le moteur d’affichage pris en charge par le modèle, par exemple aspx ou Razor. Vous pouvez spécifier une valeur personnalisée pour ce champ.
- **TemplateID** est utilisé pour regrouper les modèles. Si la valeur correspond à un ID de modèle existant, elle est remplacée par les modèles précédemment inscrits auprès de l’Assistant MVC.
- **Titre** désigne la brève description affichée dans l’Assistant MVC sous chaque modèle de projet.
- **Description** désigne une description plus détaillée du modèle.

Une fois que vous avez ajouté tous les fichiers au manifeste et que vous l’avez enregistré, vous remarquerez que l’onglet **ressources** dans le concepteur affiche tous les fichiers, mais pas les attributs personnalisés que vous avez ajoutés aux éléments de **&gt;&lt;Asset** pour les fichiers **VSTemplate** .

![Ressources du concepteur de projets](custom-mvc-templates/_static/image8.jpg)

Tout ce qui reste à présent est de compiler le projet VSIX et de l’installer.

Assurez-vous que toutes les instances de Visual Studio sont fermées sur l’ordinateur sur lequel vous envisagez de tester l’extension VSIX. Visual Studio recherche de nouvelles extensions au démarrage. par conséquent, si l’IDE est ouvert lors de l’installation d’un VSIX, vous devrez redémarrer Visual Studio. Dans l’Explorateur, double-cliquez sur le fichier VSIX pour lancer le **programme d’installation VSIX**, cliquez sur **installer** , puis lancez Visual Studio.

![Programme d’installation VSIX](custom-mvc-templates/_static/image9.jpg)

Dans le menu, sélectionnez **outils > extensions et mises à jour** pour confirmer que votre extension a été installée. Si le programme d’installation VSIX signale des erreurs lors de l’installation de l’extension, vous pouvez afficher le journal du programme d’installation VSIX pour plus d’informations. Le journal est généralement créé dans le dossier **% temp%** de l’utilisateur qui a installé l’extension, par exemple **C:\Users\Bob\AppData\Local\Temp**.

![Extensions et mises à jour](custom-mvc-templates/_static/image10.jpg)

Après avoir fermé la fenêtre, vous pouvez créer un projet MVC 4 pour voir si vos nouveaux modèles sont affichés dans l’Assistant MVC.

![Nouveau projet ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Limitations

1. L’Assistant MVC ne prend pas en charge les modèles personnalisés localisés.
2. L’Assistant ne signale aucune erreur s’il ne parvient pas à localiser les modèles personnalisés. Si l’un des attributs personnalisés requis est absent, le modèle est simplement exclu de l’Assistant.
