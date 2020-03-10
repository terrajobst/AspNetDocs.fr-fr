---
uid: web-pages/overview/data/working-with-files
title: Utilisation de fichiers dans un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre explique comment lire, écrire, ajouter, supprimer et télécharger des fichiers.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 684c47a8a8480dc040e5144144577c94c35d39e5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642365"
---
# <a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Utilisation de fichiers dans un site pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment lire, écrire, ajouter, supprimer et télécharger des fichiers sur un site pages Web ASP.NET (Razor).
> 
> > [!NOTE]
> > Si vous souhaitez télécharger des images et les manipuler (par exemple, les retourner ou les redimensionner), consultez [utilisation des images dans un Site pages Web ASP.net](/aspnet/web-pages/overview/ui-layouts-and-themes/9-working-with-images).
> 
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment créer un fichier texte et y écrire des données.
> - Comment ajouter des données à un fichier existant.
> - Comment lire un fichier et l’afficher à partir de celui-ci.
> - Comment supprimer des fichiers d’un site Web.
> - Comment permettre aux utilisateurs de télécharger un ou plusieurs fichiers.
> 
> Voici les fonctionnalités de programmation ASP.NET présentées dans l’article :
> 
> - Objet `File`, qui permet de gérer les fichiers.
> - Le programme d’assistance `FileUpload`.
> - Objet `Path`, qui fournit des méthodes qui vous permettent de manipuler les noms de chemin d’accès et de fichier.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Ce didacticiel fonctionne également avec WebMatrix 3.

<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Création d’un fichier texte et écriture de données dans celui-ci

Outre l’utilisation d’une base de données dans votre site Web, vous pouvez utiliser des fichiers. Par exemple, vous pouvez utiliser des fichiers texte comme moyen simple pour stocker des données pour le site. (Un fichier texte utilisé pour stocker des données est parfois appelé *fichier plat*). Les fichiers texte peuvent être dans différents formats, tels que *. txt*, *. xml*ou *. csv* (valeurs délimitées par des virgules).

Si vous souhaitez stocker des données dans un fichier texte, vous pouvez utiliser la méthode `File.WriteAllText` pour spécifier le fichier à créer et les données à y écrire. Dans cette procédure, vous allez créer une page qui contient un formulaire simple avec trois éléments `input` (prénom, nom et adresse de messagerie) et un bouton **Envoyer** . Lorsque l’utilisateur envoie le formulaire, vous stockez l’entrée de l’utilisateur dans un fichier texte.

1. Créez un dossier nommé *App\_Data*, s’il n’existe pas déjà.
2. À la racine de votre site Web, créez un nouveau fichier nommé *UserData. cshtml*.
3. Remplacez le contenu existant par ce qui suit : 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Le balisage HTML crée le formulaire avec les trois zones de texte. Dans le code, vous utilisez la propriété `IsPost` pour déterminer si la page a été envoyée avant de commencer le traitement.

    La première tâche consiste à récupérer l’entrée utilisateur et à l’attribuer à des variables. Le code concatène ensuite les valeurs des variables distinctes en une chaîne délimitée par des virgules, qui est ensuite stockée dans une variable différente. Notez que le séparateur de virgule est une chaîne contenue entre guillemets (","), car vous incorporez littéralement une virgule dans la Big String que vous êtes en train de créer. À la fin des données que vous concaténez ensemble, vous ajoutez `Environment.NewLine`. Cela ajoute un saut de ligne (un caractère de saut de ligne). Ce que vous créez avec toute cette concaténation est une chaîne qui ressemble à ceci :

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Avec un saut de ligne invisible à la fin.)

    Vous créez ensuite une variable (`dataFile`) qui contient l’emplacement et le nom du fichier dans lequel stocker les données. La définition de l’emplacement nécessite un traitement spécial. Dans les sites Web, il est déconseillé de faire référence à des chemins d’accès absolus comme *C:\Folder\File.txt* pour les fichiers sur le serveur Web. Si un site Web est déplacé, un chemin d’accès absolu est incorrect. En outre, pour un site hébergé (par opposition à sur votre ordinateur), vous ne connaissez pas le bon chemin d’accès lorsque vous écrivez le code.

    Mais parfois (comme maintenant, pour l’écriture d’un fichier), vous avez besoin d’un chemin d’accès complet. La solution consiste à utiliser la méthode `MapPath` de l’objet `Server`. Cela retourne le chemin d’accès complet à votre site Web. Pour obtenir le chemin d’accès à la racine du site Web, vous devez utiliser l’opérateur `~` (pour replacer la racine virtuelle du site) sur `MapPath`. (Vous pouvez également lui transmettre un nom de sous-dossier, par exemple *~/App\_Data/* , pour obtenir le chemin d’accès à ce sous-dossier.) Vous pouvez ensuite concaténer des informations supplémentaires sur ce que la méthode retourne afin de créer un chemin d’accès complet. Dans cet exemple, vous ajoutez un nom de fichier. (Pour plus d’informations sur l’utilisation des chemins d’accès aux fichiers et aux dossiers, consultez [Introduction à la programmation d’pages Web ASP.net à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Le fichier est enregistré dans le dossier de données de l' *application\_* . Ce dossier est un dossier spécial dans ASP.NET qui est utilisé pour stocker les fichiers de données, comme décrit dans [Introduction à l’utilisation d’une base de données dans des Sites pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=195209).

    La méthode `WriteAllText` de l’objet `File` écrit les données dans le fichier. Cette méthode prend deux paramètres : le nom (avec le chemin d’accès) du fichier dans lequel écrire et les données réelles à écrire. Notez que le nom du premier paramètre a un caractère `@` en tant que préfixe. Cela indique à ASP.NET que vous fournissez un littéral de chaîne textuelle et que les caractères tels que « / » ne doivent pas être interprétés de manière spéciale. (Pour plus d’informations, consultez [Présentation de la programmation Web ASP.net à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Pour que votre code puisse enregistrer des fichiers dans le dossier de données de l' *application\_* , l’application a besoin d’autorisations en lecture-écriture pour ce dossier. Sur votre ordinateur de développement, ce n’est généralement pas un problème. Toutefois, lorsque vous publiez votre site sur le serveur Web d’un fournisseur d’hébergement, vous devrez peut-être définir explicitement ces autorisations. Si vous exécutez ce code sur le serveur d’un fournisseur d’hébergement et que vous obtenez des erreurs, contactez le fournisseur d’hébergement pour savoir comment définir ces autorisations.

- Exécutez la page dans un navigateur. 

    ![](working-with-files/_static/image1.jpg)
- Entrez des valeurs dans les champs, puis cliquez sur **Envoyer**.
- Fermez le navigateur.
- Revenez au projet et actualisez la vue.
- Ouvrez le fichier *Data. txt* . Les données que vous avez envoyées dans le formulaire se trouvent dans le fichier. 

    ![[image]](working-with-files/_static/image2.jpg)
- Fermez le fichier *Data. txt* .

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Ajout de données à un fichier existant

Dans l’exemple précédent, vous avez utilisé `WriteAllText` pour créer un fichier texte contenant simplement un élément de données. Si vous rappelez la méthode et que vous lui transmettez le même nom de fichier, le fichier existant est entièrement remplacé. Toutefois, une fois que vous avez créé un fichier, vous souhaitez souvent ajouter de nouvelles données à la fin du fichier. Vous pouvez le faire à l’aide de la méthode `AppendAllText` de l’objet `File`.

1. Dans le site Web, effectuez une copie du fichier *UserData. cshtml* et nommez la copie *UserDataMultiple. cshtml*.
2. Remplacez le bloc de code avant l’ouverture de la balise `<!DOCTYPE html>` par le bloc de code suivant : 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Ce code a une modification dans l’exemple précédent. Au lieu d’utiliser `WriteAllText`, elle utilise `the AppendAllText` méthode. Les méthodes sont similaires, à ceci près que `AppendAllText` ajoute les données à la fin du fichier. Comme avec `WriteAllText`, `AppendAllText` crée le fichier s’il n’existe pas déjà.
3. Exécutez la page dans un navigateur.
4. Entrez des valeurs pour les champs, puis cliquez sur **Envoyer**.
5. Ajoutez d’autres données et soumettez de nouveau le formulaire.
6. Revenez à votre projet, cliquez avec le bouton droit sur le dossier du projet, puis cliquez sur **Actualiser**.
7. Ouvrez le fichier *Data. txt* . Elle contient maintenant les nouvelles données que vous venez d’entrer. 

    ![[image]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Lecture et affichage de données à partir d’un fichier

Même si vous n’avez pas besoin d’écrire des données dans un fichier texte, vous devrez probablement lire les données d’un. Pour ce faire, vous pouvez utiliser à nouveau l’objet `File`. Vous pouvez utiliser l’objet `File` pour lire chaque ligne individuellement (séparées par des sauts de ligne) ou pour lire un élément individuel, quelle que soit la façon dont ils sont séparés.

Cette procédure vous montre comment lire et afficher les données que vous avez créées dans l’exemple précédent.

1. À la racine de votre site Web, créez un nouveau fichier nommé *displayData. cshtml*.
2. Remplacez le contenu existant par ce qui suit : 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Le code commence par lire le fichier que vous avez créé dans l’exemple précédent dans une variable nommée `userData`, à l’aide de cet appel de méthode :

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Pour ce faire, le code est à l’intérieur d’une instruction `if`. Lorsque vous souhaitez lire un fichier, il est judicieux d’utiliser la méthode `File.Exists` pour déterminer d’abord si le fichier est disponible. Le code vérifie également si le fichier est vide.

    Le corps de la page contient deux boucles `foreach`, une imbriquée à l’intérieur de l’autre. La boucle `foreach` externe obtient une ligne à la fois à partir du fichier de données. Dans ce cas, les lignes sont définies par des sauts de ligne &#8212; dans le fichier qui est, chaque élément de données se trouve sur sa propre ligne. La boucle externe crée un nouvel élément (élément`<li>`) à l’intérieur d’une liste triée (élément`<ol>`).

    La boucle interne fractionne chaque ligne de données en éléments (champs) à l’aide d’une virgule comme délimiteur. (En fonction de l’exemple précédent, cela signifie que chaque ligne contient trois &#8212; champs, le prénom, le nom et l’adresse de messagerie, chacun étant séparé par une virgule.) La boucle interne crée également une liste de `<ul>` et affiche un élément de liste pour chaque champ de la ligne de données.

    Le code illustre l’utilisation de deux types de données : un tableau et le type de données `char`. Le tableau est obligatoire, car la méthode `File.ReadAllLines` retourne les données sous forme de tableau. Le type de données `char` est obligatoire, car la méthode `Split` retourne un `array` dans lequel chaque élément est de type `char`. (Pour plus d’informations sur les tableaux, consultez [Introduction à la programmation Web ASP.net à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Exécutez la page dans un navigateur. Les données que vous avez entrées pour les exemples précédents sont affichées. 

    ![[image]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Affichage de données à partir d’un fichier délimité par des virgules Microsoft Excel**
> 
> Vous pouvez utiliser Microsoft Excel pour enregistrer les données contenues dans une feuille de calcul sous la forme d’un fichier délimité par des virgules ( *. csv* ). Dans ce cas, le fichier est enregistré en texte brut, et non au format Excel. Chaque ligne de la feuille de calcul est séparée par un saut de ligne dans le fichier texte, et chaque élément de données est séparé par une virgule. Vous pouvez utiliser le code indiqué dans l’exemple précédent pour lire un fichier Excel délimité par des virgules simplement en modifiant le nom du fichier de données dans votre code.

<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Suppression de fichiers

Pour supprimer des fichiers de votre site Web, vous pouvez utiliser la méthode `File.Delete`. Cette procédure montre comment permettre aux utilisateurs de supprimer une image (fichier *. jpg* ) d’un dossier *images* s’ils connaissent le nom du fichier.

> [!NOTE] 
> 
> **Important** Dans un site Web de production, vous limitez généralement les personnes autorisées à apporter des modifications aux données. Pour plus d’informations sur la configuration de l’appartenance et sur les façons d’autoriser les utilisateurs à effectuer des tâches sur le site, consultez [Ajout de la sécurité et de l’appartenance à un site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202904).

1. Dans le site Web, créez un sous-dossier nommé *images*.
2. Copiez un ou plusieurs fichiers *. jpg* dans le dossier *images* .
3. À la racine du site Web, créez un nouveau fichier nommé *FileDelete. cshtml*.
4. Remplacez le contenu existant par ce qui suit : 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Cette page contient un formulaire dans lequel les utilisateurs peuvent entrer le nom d’un fichier image. Ils n’entrent pas l’extension de nom de fichier *. jpg* ; en restreignant le nom de fichier comme celui-ci, vous pouvez empêcher les utilisateurs de supprimer des fichiers arbitraires sur votre site.

    Le code lit le nom de fichier que l’utilisateur a entré, puis construit un chemin d’accès complet. Pour créer le chemin d’accès, le code utilise le chemin d’accès au site Web actuel (tel que retourné par la méthode `Server.MapPath`), le nom du dossier *images* , le nom que l’utilisateur a fourni et « . jpg » comme chaîne littérale.

    Pour supprimer le fichier, le code appelle la méthode `File.Delete`, en lui transmettant le chemin d’accès complet que vous venez de construire. À la fin du balisage, le code affiche un message de confirmation indiquant que le fichier a été supprimé.
5. Exécutez la page dans un navigateur. 

    ![[image]](working-with-files/_static/image5.jpg)
6. Entrez le nom du fichier à supprimer, puis cliquez sur **Envoyer**. Si le fichier a été supprimé, le nom du fichier s’affiche au bas de la page.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Permettre aux utilisateurs de charger un fichier

Le `FileUpload` Helper permet aux utilisateurs de charger des fichiers sur votre site Web. La procédure suivante vous montre comment permettre aux utilisateurs de charger un fichier unique.

1. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas encore ajoutée.
2. Dans le dossier de données de l' *application\_* , créez un dossier et nommez-le *uploadedfiles*.
3. Dans la racine, créez un nouveau fichier nommé *FileUpload. cshtml*.
4. Remplacez le contenu existant de la page par ce qui suit : 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    La partie du corps de la page utilise le programme d’assistance `FileUpload` pour créer la zone de téléchargement et les boutons que vous connaissez probablement :

    ![[image]](working-with-files/_static/image6.jpg)

    Les propriétés que vous définissez pour le `FileUpload` Helper spécifient que vous souhaitez qu’une seule zone soit téléchargée pour le fichier et que vous souhaitiez que le bouton Envoyer Lise le **Téléchargement**. (Vous ajouterez d’autres zones plus loin dans cet article.)

    Quand l’utilisateur clique sur **Télécharger**, le code situé en haut de la page obtient le fichier et l’enregistre. L’objet `Request` que vous utilisez habituellement pour obtenir des valeurs à partir des champs de formulaire possède également un tableau `Files` contenant le ou les fichiers qui ont été téléchargés. Vous pouvez obtenir des fichiers individuels d’une position spécifique dans le &#8212; tableau. par exemple, pour obtenir le premier fichier téléchargé, vous avez `Request.Files[0]`, pour obtenir le deuxième fichier, vous devez obtenir `Request.Files[1]`, et ainsi de suite. (N’oubliez pas que, en programmation, le comptage commence généralement à zéro.)

    Lorsque vous récupérez un fichier téléchargé, vous le placez dans une variable (ici, `uploadedFile`) afin de pouvoir le manipuler. Pour déterminer le nom du fichier téléchargé, il vous suffit d’obtenir sa propriété `FileName`. Toutefois, lorsque l’utilisateur charge un fichier, `FileName` contient le nom d’origine de l’utilisateur, qui inclut le chemin d’accès complet. Elle peut se présenter comme suit :

    *C:\Users\Public\Sample.txt*

    Toutefois, vous ne souhaitez pas toutes ces informations de chemin d’accès, car il s’agit du chemin d’accès sur l’ordinateur de l’utilisateur, et non pas pour votre serveur. Vous souhaitez juste le nom de fichier réel (*Sample. txt*). Vous pouvez supprimer uniquement le fichier d’un chemin d’accès à l’aide de la méthode `Path.GetFileName`, comme suit :

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    L’objet `Path` est un utilitaire qui a plusieurs méthodes comme celles-ci, que vous pouvez utiliser pour supprimer les chemins d’accès, combiner les chemins d’accès, etc.

    Une fois que vous avez obtenu le nom du fichier téléchargé, vous pouvez créer un nouveau chemin d’accès pour l’emplacement où vous souhaitez stocker le fichier chargé dans votre site Web. Dans ce cas, vous combinez `Server.MapPath`, les noms de dossiers (*application\_Data/uploadedfiles*) et le nom de fichier nouvellement supprimé pour créer un nouveau chemin d’accès. Vous pouvez ensuite appeler la méthode `SaveAs` du fichier chargé pour enregistrer le fichier.
5. Exécutez la page dans un navigateur. 

    ![[image]](working-with-files/_static/image7.jpg)
6. Cliquez sur **Parcourir** , puis sélectionnez un fichier à charger. 

    ![[image]](working-with-files/_static/image8.jpg)

    La zone de texte en regard du bouton **Parcourir** contient le chemin d’accès et l’emplacement du fichier.

    ![[image]](working-with-files/_static/image9.jpg)
7. Cliquez sur **Upload**.
8. Dans le site Web, cliquez avec le bouton droit sur le dossier du projet, puis cliquez sur **Actualiser**.
9. Ouvrez le dossier *uploadedfiles* . Le fichier que vous avez chargé se trouve dans le dossier. 

    ![[image]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Permettre aux utilisateurs de télécharger plusieurs fichiers

Dans l’exemple précédent, vous autorisez les utilisateurs à télécharger un fichier. Toutefois, vous pouvez utiliser le programme d’assistance `FileUpload` pour télécharger plusieurs fichiers à la fois. Cela est pratique pour des scénarios tels que le chargement de photos, où le téléchargement d’un fichier à la fois est fastidieux. (Vous pouvez en savoir plus sur le chargement de photos dans [utilisation d’images dans un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202897).) Cet exemple montre comment permettre aux utilisateurs de télécharger deux à la fois, bien que vous puissiez utiliser la même technique pour télécharger plus que cela.

1. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas déjà fait.
2. Créez une nouvelle page nommée *FileUploadMultiple. cshtml*.
3. Remplacez le contenu existant de la page par ce qui suit :  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    Dans cet exemple, le programme d’assistance `FileUpload` dans le corps de la page est configuré pour permettre aux utilisateurs de télécharger deux fichiers par défaut. Étant donné que `allowMoreFilesToBeAdded` a la valeur `true`, le programme d’assistance affiche un lien qui permet à l’utilisateur d’ajouter d’autres zones de téléchargement :

    ![[image]](working-with-files/_static/image11.jpg)

    Pour traiter les fichiers que l’utilisateur charge, le code utilise la même technique de base que celle utilisée dans l’exemple &#8212; précédent pour obtenir un fichier de `Request.Files` puis l’enregistrer. (Y compris les différents éléments que vous devez faire pour connaître le nom de fichier et le chemin d’accès appropriés). Cette fois-ci, l’innovation est que l’utilisateur peut charger plusieurs fichiers et que vous n’en connaissez pas beaucoup. Pour en savoir plus, vous pouvez obtenir `Request.Files.Count`.

    Avec ce nombre en main, vous pouvez parcourir `Request.Files`, extraire chaque fichier à son tour, puis l’enregistrer. Lorsque vous souhaitez effectuer une boucle d’un nombre de fois connu dans une collection, vous pouvez utiliser une boucle `for`, comme suit :

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    La variable `i` est simplement un compteur temporaire qui passera de zéro à la limite supérieure que vous définissez. Dans ce cas, la limite supérieure est le nombre de fichiers. Toutefois, étant donné que le compteur commence à zéro, comme c’est généralement le cas pour les scénarios de comptage dans ASP.NET, la limite supérieure est en fait inférieure d’une unité au nombre de fichiers. (Si trois fichiers sont téléchargés, le nombre est de 0 à 2.)

    La variable `uploadedCount` totalise tous les fichiers qui ont été téléchargés et enregistrés avec succès. Ce code tient compte de la possibilité qu’un fichier attendu ne puisse pas être téléchargé.
4. Exécutez la page dans un navigateur. Le navigateur affiche la page et ses deux zones de téléchargement.
5. Sélectionnez deux fichiers à charger.
6. Cliquez sur **Ajouter un autre fichier**. La page affiche une nouvelle zone de téléchargement. 

    ![[image]](working-with-files/_static/image12.jpg)
7. Cliquez sur **Upload**.
8. Dans le site Web, cliquez avec le bouton droit sur le dossier du projet, puis cliquez sur **Actualiser**.
9. Ouvrez le dossier *uploadedfiles* pour voir les fichiers chargés avec succès.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[Utilisation d’images dans un site pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897)

[Exportation vers un fichier CSV](https://msdn.microsoft.com/library/ms155919.aspx)
