---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Utilisation de CascadingDropDown avec une base de données (VB) | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un DropDownList chargent les valeurs associées dans anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 4482aa18c4446ec8f5f160c423008398ea2e1d0d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599472"
---
# <a name="using-cascadingdropdown-with-a-database-vb"></a>Utilisation de CascadingDropDown avec une base de données (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. Pour que cela fonctionne, vous devez créer un service Web spécial.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList. (Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) Pour que cela fonctionne, vous devez créer un service Web spécial.

## <a name="steps"></a>Étapes

Tout d’abord, une source de données est requise. Cet exemple utilise la base de données AdventureWorks et la Microsoft SQL Server édition Express 2005. La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition Express) et est également disponible sous forme de téléchargement distinct sous [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de données AdventureWorks fait partie des exemples SQL Server 2005 et des exemples de bases de données (téléchargez à l’adresse [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)en). Le moyen le plus simple de définir la base de données consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)en) et à attacher le fichier de base de données de `AdventureWorks.mdf`.

Pour cet exemple, nous partons du principe que l’instance de la SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur Web. Il s’agit également de l’installation par défaut. Si votre installation diffère, vous devez adapter les informations de connexion de la base de données.

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans le &lt;`form`élément &gt;) :

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

À l’étape suivante, deux contrôles DropDownList sont requis. Dans cet exemple, nous utilisons le fournisseur et les informations de contact d’AdventureWorks. nous créons donc une liste pour les fournisseurs disponibles et une pour les contacts disponibles :

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Ensuite, deux extendeurs CascadingDropDown doivent être ajoutés à la page. L’une remplit la première liste (Vendors) et l’autre remplit la deuxième liste (contacts). Les attributs suivants doivent être définis :

- `ServicePath`: URL d’un service Web qui fournit les entrées de liste
- `ServiceMethod`: méthode Web qui fournit les entrées de liste
- `TargetControlID`: ID de la liste déroulante
- `Category`: informations de catégorie soumises à la méthode Web quand elles sont appelées
- `PromptText`: texte affiché lors du chargement asynchrone de données de liste à partir du serveur
- `ParentControlID`: (facultatif) liste déroulante parente qui déclenche le chargement de la liste actuelle

Selon le langage de programmation utilisé, le nom du service Web en question change, mais toutes les autres valeurs d’attribut sont les mêmes. Voici l’élément CascadingDropDown pour la première liste déroulante :

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Les extendeurs de contrôle pour la deuxième liste doivent définir l’attribut `ParentControlID` afin que la sélection d’une entrée dans la liste Vendors déclenche le chargement des éléments associés dans la liste Contacts.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

Le travail réel est ensuite effectué dans le service Web, qui est configuré comme suit. Notez que l’attribut `[ScriptService]` est utilisé, sinon ASP.NET AJAX ne peut pas créer le proxy JavaScript pour accéder aux méthodes Web à partir du code de script côté client.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

La signature des méthodes Web appelées par CascadingDropDown est la suivante :

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

La valeur de retour doit donc être un tableau de type `CascadingDropDownNameValue` qui est défini par la boîte à outils de contrôle. La méthode `GetVendors()` est relativement facile à implémenter : le code se connecte à la base de données AdventureWorks et interroge les 25 premiers fournisseurs. Le premier paramètre dans le constructeur de `CascadingDropDownNameValue` est la légende de l’entrée de liste, la deuxième sa valeur (attribut value dans le &lt;`option`élément &gt; du code HTML). Voici le code :

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

L’obtention des contacts associés pour un fournisseur (nom de la méthode : `GetContactsForVendor()`) est un peu plus compliquée. Tout d’abord, le fournisseur sélectionné dans la première liste déroulante doit être déterminé. Le Control Toolkit définit une méthode d’assistance pour cette tâche : la méthode `ParseKnownCategoryValuesString()` retourne un élément `StringDictionary` avec les données de liste déroulante :

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

Pour des raisons de sécurité, ces données doivent être validées en premier. Ainsi, s’il existe une entrée de fournisseur (car la propriété `Category` du premier élément CascadingDropDown a la valeur `"Vendor"`), l’ID du fournisseur sélectionné peut être récupéré :

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

Le reste de la méthode est relativement simple, puis. L’ID du fournisseur est utilisé comme paramètre pour une requête SQL qui récupère tous les contacts associés à ce fournisseur. Une fois encore, la méthode retourne un tableau de type `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Chargez la page ASP.NET et, après un bref instant, la liste des fournisseurs contient 25 entrées. Sélectionnez une entrée et notez comment la deuxième liste déroulante est remplie de données.

[![la première liste est remplie automatiquement](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

La première liste est automatiquement remplie ([cliquez pour afficher l’image en taille réelle](using-cascadingdropdown-with-a-database-vb/_static/image3.png))

[![la deuxième liste est remplie en fonction de la sélection dans la première liste](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

La deuxième liste est remplie en fonction de la sélection dans la première liste ([cliquez pour afficher l’image en taille réelle](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Précédent](filling-a-list-using-cascadingdropdown-vb.md)
> [Suivant](presetting-list-entries-with-cascadingdropdown-vb.md)
