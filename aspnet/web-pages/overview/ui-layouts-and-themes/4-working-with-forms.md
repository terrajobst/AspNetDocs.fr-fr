---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Utilisation des formulaires HTML dans les sites pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Un formulaire est une section d’un document HTML dans laquelle vous placez des contrôles d’entrée d’utilisateur, tels que des zones de texte, des cases à cocher, des cases d’option et des listes déroulantes. Vous utilisez Forms WH...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: c7d4802063c8610a246afe67bd15eea429f7304a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639705"
---
# <a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Utilisation des formulaires HTML dans les sites pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment traiter un formulaire HTML (avec des zones de texte et des boutons) quand vous travaillez sur un site Web pages Web ASP.NET (Razor).
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment créer un formulaire HTML.
> - Comment lire l’entrée d’utilisateur à partir du formulaire.
> - Comment valider une entrée d’utilisateur.
> - Comment restaurer des valeurs de formulaire après l’envoi de la page.
> 
> Voici les concepts de programmation ASP.NET présentés dans l’article :
> 
> - Objet `Request`.
> - Validation de l’entrée.
> - Encodage HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2.

## <a name="creating-a-simple-html-form"></a>Création d’un formulaire HTML simple

1. Créez un site Web.
2. Dans le dossier racine, créez une page Web nommée *Form. cshtml* et entrez le balisage suivant :

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Lancez la page dans votre navigateur. (Dans WebMatrix, dans l’espace de travail **fichiers** , cliquez avec le bouton droit sur le fichier, puis sélectionnez **lancer dans le navigateur**.) Un formulaire simple avec trois champs d’entrée et un bouton **Envoyer** s’affiche.

    ![Capture d’écran d’un formulaire avec trois zones de texte.](4-working-with-forms/_static/image1.png)

    À ce stade, si vous cliquez sur le bouton **Envoyer** , rien ne se produit. Pour rendre le formulaire utile, vous devez ajouter du code qui s’exécutera sur le serveur.

## <a name="reading-user-input-from-the-form"></a>Lecture d’une entrée d’utilisateur à partir du formulaire

Pour traiter le formulaire, vous ajoutez du code qui lit les valeurs de champ soumises et effectue une opération avec eux. Cette procédure vous montre comment lire les champs et afficher les entrées utilisateur sur la page. (Dans une application de production, vous faites généralement des choses plus intéressantes avec l’entrée de l’utilisateur. Vous le ferez dans l’article sur l’utilisation des bases de données.)

1. En haut du fichier *Form. cshtml* , entrez le code suivant :

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Lorsque l’utilisateur demande la page pour la première fois, seul le formulaire vide s’affiche. L’utilisateur (qui vous sera) remplit le formulaire, puis clique sur **Envoyer**. Cela envoie (publie) l’entrée utilisateur au serveur. Par défaut, la demande est envoyée à la même page (à savoir, *Form. cshtml*).

    Lorsque vous soumettez la page cette fois-ci, les valeurs que vous avez entrées sont affichées juste au-dessus du formulaire :

    ![Capture d’écran qui montre les valeurs que vous avez entrées affichées sur la page.](4-working-with-forms/_static/image2.png)

    Examinez le code de la page. Vous utilisez d’abord la méthode `IsPost` pour déterminer si la page est en &#8212; cours de publication, c’est-à-dire si un utilisateur a cliqué sur le bouton **Envoyer** . S’il s’agit d’une publication, `IsPost` retourne la valeur true. Il s’agit de la méthode standard dans pages Web ASP.NET pour déterminer si vous utilisez une requête initiale (une demande de récupération) ou une publication (une demande de publication). (Pour plus d’informations sur obtenir et poster, consultez l’encadré « HTTP : obtenir et poster et la propriété IsPost » dans [Introduction à la programmation de pages Web ASP.net à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Ensuite, vous récupérez les valeurs que l’utilisateur a renseignées à partir de l’objet `Request.Form` et vous les placez dans des variables pour une version ultérieure. L’objet `Request.Form` contient toutes les valeurs qui ont été soumises avec la page, chacune identifiée par une clé. La clé est l’équivalent de l’attribut `name` du champ de formulaire que vous souhaitez lire. Par exemple, pour lire le champ `companyname` (zone de texte), vous utilisez `Request.Form["companyname"]`.

    Les valeurs de formulaire sont stockées dans l’objet `Request.Form` sous forme de chaînes. Par conséquent, quand vous devez utiliser une valeur comme un nombre ou une date ou un autre type, vous devez la convertir à partir d’une chaîne vers ce type. Dans l’exemple, la méthode `AsInt` de la `Request.Form` est utilisée pour convertir la valeur du champ Employees (qui contient un nombre Employee) en entier.
2. Lancez la page dans votre navigateur, renseignez les champs du formulaire, puis cliquez sur **Envoyer**. La page affiche les valeurs que vous avez entrées.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Codage HTML pour l’apparence et la sécurité
> 
> HTML a des utilisations spéciales pour les caractères tels que `<`, `>`et `&`. Si ces caractères spéciaux apparaissent là où ils ne sont pas attendus, ils peuvent gâcher l’apparence et la fonctionnalité de votre page Web. Par exemple, le navigateur interprète le caractère `<` (sauf s’il est suivi d’un espace) comme le début d’un élément HTML, comme `<b>` ou `<input ...>`. Si le navigateur ne reconnaît pas l’élément, il ignore simplement la chaîne qui commence par `<` jusqu’à ce qu’il reconnaisse à nouveau. Évidemment, cela peut entraîner un rendu étrange dans la page.
> 
> L’encodage HTML remplace ces caractères réservés par un code que les navigateurs interprètent comme le symbole correct. Par exemple, le caractère `<` est remplacé par `&lt;` et le caractère `>` est remplacé par `&gt;`. Le navigateur affiche ces chaînes de remplacement comme les caractères que vous souhaitez afficher.
> 
> Il est judicieux d’utiliser l’encodage HTML chaque fois que vous affichez des chaînes (entrée) que vous avez obtenues d’un utilisateur. Si vous ne le faites pas, un utilisateur peut essayer d’obtenir votre page Web pour exécuter un script malveillant ou faire une autre chose qui compromet la sécurité de votre site ou ce n’est pas ce que vous pensez. (Ceci est particulièrement important si vous prenez des entrées utilisateur, les stockez dans un endroit, puis &#8212; vous les affichez ultérieurement, par exemple, sous la forme d’un commentaire de blog, d’une revue de l’utilisateur ou d’un autre type.)
> 
> Pour éviter ces problèmes, pages Web ASP.NET code en HTML automatiquement tout contenu de texte que vous avez généré à partir de votre code. Par exemple, lorsque vous affichez le contenu d’une variable ou d’une expression à l’aide d’un code tel que `@MyVar`, pages Web ASP.NET encode automatiquement la sortie.

## <a name="validating-user-input"></a>Validation de l'entrée utilisateur

Les utilisateurs font des erreurs. Vous leur demandez de remplir un champ, et ils oublient, ou vous leur demandez d’entrer le nombre d’employés et ils tapent un nom à la place. Pour vous assurer qu’un formulaire a été rempli correctement avant de le traiter, vous validez l’entrée de l’utilisateur.

Cette procédure montre comment valider les trois champs de formulaire pour s’assurer que l’utilisateur ne les a pas laissés vides. Vous vérifiez également que la valeur du nombre d’employés est un nombre. Si des erreurs se produisent, vous affichez un message d’erreur qui indique à l’utilisateur les valeurs qui n’ont pas été validées.

1. Dans le fichier *Form. cshtml* , remplacez le premier bloc de code par le code suivant : 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Pour valider l’entrée d’utilisateur, vous utilisez le programme d’assistance `Validation`. Vous inscrivez les champs requis en appelant `Validation.RequireField`. Vous inscrivez d’autres types de validation en appelant `Validation.Add` et en spécifiant le champ à valider et le type de validation à effectuer.

    Lorsque la page s’exécute, ASP.NET effectue toutes les validations pour vous. Vous pouvez vérifier les résultats en appelant `Validation.IsValid`, qui retourne true si tout a réussi et false si la validation d’un champ a échoué. En général, vous appelez `Validation.IsValid` avant d’effectuer un traitement sur l’entrée d’utilisateur.
2. Mettez à jour l’élément `<body>` en ajoutant trois appels à la méthode `Html.ValidationMessage`, comme suit :

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Pour afficher les messages d’erreur de validation, vous pouvez appeler html.`ValidationMessage` et transmettez-lui le nom du champ pour lequel vous souhaitez obtenir le message.
3. Exécutez la page. Laissez les champs vides, puis cliquez sur **Envoyer**. Des messages d’erreur s’affichent.

    ![Capture d’écran qui affiche les messages d’erreur affichés si l’entrée de l’utilisateur ne réussit pas la validation.](4-working-with-forms/_static/image3.jpg)
4. Ajoutez une chaîne (par exemple, « ABC ») au champ **Employee Count (nombre d’employés** ), puis cliquez à nouveau sur **Submit (envoyer** ). Cette fois, vous voyez une erreur qui indique que la chaîne n’est pas dans le format approprié, à savoir un entier.

    ![Capture d’écran qui affiche les messages d’erreur affichés si les utilisateurs entrent une chaîne pour le champ employés.](4-working-with-forms/_static/image4.jpg)

Pages Web ASP.NET fournit plus d’options pour valider l’entrée d’utilisateur, y compris la possibilité d’effectuer automatiquement une validation à l’aide d’un script client, afin que les utilisateurs reçoivent des commentaires immédiats dans le navigateur. Pour plus d’informations, consultez la section [ressources supplémentaires](#Additional_Resources) .

## <a name="restoring-form-values-after-postbacks"></a>Restauration des valeurs de formulaire après des publications

Quand vous avez testé la page de la section précédente, vous avez peut-être remarqué que si vous aviez une erreur de validation, tout ce que vous avez entré (pas seulement les données non valides) a été supprimé, et vous deviez entrer à nouveau les valeurs de tous les champs. Cela illustre un point important : lorsque vous envoyez une page, la traitez, puis restituez à nouveau la page, la page est recréée à partir de zéro. Comme vous l’avez vu, cela signifie que toutes les valeurs qui figuraient dans la page lors de leur envoi sont perdues.

Toutefois, vous pouvez résoudre ce problème facilement. Vous avez accès aux valeurs qui ont été soumises (dans l’objet `Request.Form`, vous pouvez ainsi remplir ces valeurs dans les champs de formulaire lors du rendu de la page.

1. Dans le fichier *Form. cshtml* , remplacez les attributs `value` des éléments `<input>` à l’aide de l’attribut `value`. : 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    L’attribut `value` des éléments `<input>` a été défini pour lire dynamiquement la valeur de champ hors de l’objet `Request.Form`. La première fois que la page est demandée, les valeurs de l’objet `Request.Form` sont toutes vides. Cela fonctionne bien, car le formulaire est vide.
2. Lancez la page dans votre navigateur, renseignez les champs du formulaire ou laissez-les vides, puis cliquez sur **Envoyer**. Une page qui affiche les valeurs soumises est affichée.

    ![formulaires-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [1 001 façons d’accéder aux données des utilisateurs Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Utilisation des formulaires et traitement des entrées utilisateur](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Validation des entrées utilisateur dans les sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Utilisation de la saisie semi-automatique dans les formulaires HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
