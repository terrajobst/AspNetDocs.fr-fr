---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Nouveautés de Web Forms dans ASP.NET 4,5 | Microsoft Docs
author: rick-anderson
description: La nouvelle version de ASP.NET Web Forms présente un certain nombre d’améliorations axées sur l’amélioration de l’expérience utilisateur lors de l’utilisation de données. Dans les versions précédentes de...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 301af8ed877b58624e419c04f605c41f27dbdd0c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525731"
---
# <a name="whats-new-in-web-forms-in-aspnet-45"></a>Nouveautés de Web Forms dans ASP.NET 4.5

par l' [équipe Web camps](https://twitter.com/webcamps)

> La nouvelle version de ASP.NET Web Forms présente un certain nombre d’améliorations axées sur l’amélioration de l’expérience utilisateur lors de l’utilisation de données.
> 
> Dans les versions précédentes de Web Forms, lors de l’utilisation de la liaison de données pour émettre la valeur d’un membre d’objet, vous avez utilisé les expressions de liaison de données bind () ou Eval (). Dans la nouvelle version de ASP.NET, vous pouvez déclarer le type de données auquel un contrôle va être lié à l’aide d’une nouvelle propriété ItemType. La définition de cette propriété vous permet d’utiliser une variable fortement typée pour bénéficier de tous les avantages de l’expérience de développement Visual Studio, comme IntelliSense, la navigation des membres et la vérification au moment de la compilation.
> 
> Avec les contrôles liés aux données, vous pouvez maintenant également spécifier vos propres méthodes personnalisées pour la sélection, la mise à jour, la suppression et l’insertion de données, ce qui simplifie l’interaction entre les contrôles de page et votre logique d’application. En outre, des fonctionnalités de liaison de modèle ont été ajoutées à ASP.NET, ce qui signifie que vous pouvez mapper des données à partir de la page directement dans des paramètres de type de méthode.
> 
> La validation de l’entrée d’utilisateur doit également être plus facile avec la dernière version de Web Forms. Vous pouvez désormais annoter vos classes de modèle avec des attributs de validation à partir de l’espace de noms **System. ComponentModel. DataAnnotations** et demander que tous vos contrôles de site valident les entrées utilisateur à l’aide de ces informations. La validation côté client dans Web Forms est désormais intégrée à jQuery, fournissant ainsi du code plus clair côté client et des fonctionnalités JavaScript discrètes.
> 
> Dans la zone validation de la demande, des améliorations ont été apportées pour faciliter la désactivation sélective de la validation des demandes pour des parties spécifiques de vos applications ou pour lire les données de requête invalidées.
> 
> Des améliorations ont été apportées à Web Forms contrôles serveur pour tirer parti des nouvelles fonctionnalités de HTML5 :
> 
> - La propriété TextMode du contrôle TextBox a été mise à jour pour prendre en charge les nouveaux types d’entrées HTML5 comme email, DateTime, etc.
> - Le contrôle FileUpload prend désormais en charge plusieurs chargements de fichiers à partir de navigateurs qui prennent en charge cette fonctionnalité HTML5.
> - Les contrôles Validator prennent désormais en charge la validation des éléments d’entrée HTML5.
> - Les nouveaux éléments HTML5 qui possèdent des attributs qui représentent une URL prennent désormais en charge runat =&quot;Server&quot;. Par conséquent, vous pouvez utiliser des conventions ASP.NET dans les chemins d’URL, comme l’opérateur ~ pour représenter la racine de l’application (par exemple, &lt;vidéo runat =&quot;Server&quot; SRC =&quot;~/myVideo.wmv&quot;&gt;&lt;/Video&gt;).
> - Le contrôle UpdatePanel a été résolu pour prendre en charge la publication des champs d’entrée HTML5.
> 
> Dans le portail ASP.NET officiel, vous trouverez d’autres exemples de nouvelles fonctionnalités dans ASP.NET WebForms 4,5 : [Nouveautés de ASP.NET 4,5 et Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible sur [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Utiliser des expressions de liaison de données fortement typées
- Utilisez les nouvelles fonctionnalités de liaison de modèle dans Web Forms
- Utiliser des fournisseurs de valeurs pour mapper des données de page à des méthodes code-behind
- Utiliser des annotations de données pour la validation des entrées utilisateur
- Tirez parti de la validation côté client discrète avec jQuery dans Web Forms
- Implémenter la validation granulaire des demandes
- Implémenter le traitement de page asynchrone dans Web Forms

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :

- [Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation).

<a id="Setup"></a>
### <a name="setup"></a>Installation

**Installation d’extraits de code**

Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio. Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .

Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe C : utilisation d’extraits de Code](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique comprend les exercices suivants :

1. [Exercice 1 : liaison de modèle dans ASP.NET Web Forms](#Exercise1)
2. [Exercice 2 : validation des données](#Exercise2)
3. [Exercice 3 : traitement de page asynchrone dans ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices. Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.

Durée estimée pour effectuer ce laboratoire : **60 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Exercice 1 : liaison de modèle dans ASP.NET Web Forms

La nouvelle version de ASP.NET Web Forms introduit un certain nombre d’améliorations axées sur l’amélioration de l’expérience lors de l’utilisation de données. Tout au long de cet exercice, vous allez découvrir les contrôles de données et la liaison de modèle fortement typés.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Tâche 1 : utilisation de liaisons de données fortement typées

Dans cette tâche, vous allez découvrir les nouvelles liaisons fortement typées disponibles dans ASP.NET 4,5.

1. Ouvrez le **début** de la solution situé à l’emplacement **source/EX1-ModelBinding/Begin/** Folder.

   1. Vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez la page **Customers. aspx** . Placez une liste non numérotée dans le contrôle principal et incluez un contrôle Repeater dans pour répertorier chaque client. Définissez le nom du répétiteur sur **customersRepeater** comme indiqué dans le code suivant.

    Dans les versions précédentes de Web Forms, lors de l’utilisation de la liaison de données pour émettre la valeur d’un membre sur un objet auquel vous effectuez la liaison de données, vous devez utiliser une expression de liaison de données, ainsi qu’un appel à la méthode Eval, en passant le nom du membre en tant que chaîne.

    Lors de l’exécution, ces appels à eval utilisent la réflexion par rapport à l’objet actuellement lié pour lire la valeur du membre portant le nom donné, et affichent le résultat dans le code HTML. Grâce à cette approche, il est très facile de lier des données à des données arbitraires et non en forme.

    Malheureusement, vous perdez une grande partie des fonctionnalités exceptionnelles de l’expérience de développement dans Visual Studio, notamment IntelliSense pour les noms de membres, la prise en charge de la navigation (par exemple, atteindre la définition) et la vérification au moment de la compilation.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Ouvrez le fichier **Customers.aspx.cs** .
4. Ajoutez l’instruction using suivante.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. Dans la **Page\_méthode Load** , ajoutez du code pour remplir le Repeater avec la liste des clients.

    (Extrait de code- *Web Forms Lab-Ex01-lier les clients*à la source de données)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    La solution utilise EntityFramework avec CodeFirst pour créer la base de données et y accéder. Dans le code suivant, customersRepeater est lié à une requête matérialisée qui retourne tous les clients de la base de données.
6. Appuyez sur **F5** pour exécuter la solution et accédez à la page **clients** pour voir le répéteur en action. Comme la solution utilise CodeFirst, la base de données est créée et remplie dans votre instance locale de SQL Express lors de l’exécution de l’application.

    ![Liste des clients avec un répétiteur](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "Liste des clients avec un répétiteur")

    *Liste des clients avec un répétiteur*

    > [!NOTE]
    > Dans Visual Studio 2012, IIS Express est le serveur de développement Web par défaut.
7. Fermez le navigateur et revenez à Visual Studio.
8. À présent, remplacez l’implémentation pour utiliser des liaisons fortement typées. Ouvrez la page **Customers. aspx** et utilisez le nouvel attribut **ItemType** dans le Repeater pour définir le type **Customer** comme type de liaison.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    La propriété ItemType vous permet de déclarer le type de données auquel le contrôle va être lié et vous permet d’utiliser une liaison fortement typée à l’intérieur du contrôle lié aux données.
9. Remplacez le contenu ItemTemplate par le code suivant.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    L’un des inconvénients des approches ci-dessus est que les appels à Eval () et bind () sont à liaison tardive, ce qui signifie que vous transmettez des chaînes pour représenter les noms de propriété. Cela signifie que vous n’accédez pas à IntelliSense pour les noms de membres, la prise en charge de la navigation dans le code (par exemple, atteindre la définition) et la prise en charge de la vérification au moment de la compilation.

    La définition de la propriété ItemType entraîne la génération de deux nouvelles variables typées dans la portée des expressions de liaison de données : **Item** et **BindItem**. Vous pouvez utiliser ces variables fortement typées dans les expressions de liaison de données et tirer parti de l’expérience de développement Visual Studio.

    Le &quot; **:** &quot; utilisé dans l’expression encode automatiquement la sortie au format html afin d’éviter les problèmes de sécurité (par exemple, les attaques de script entre sites). Cette notation était disponible depuis .NET 4 pour l’écriture de réponses, mais elle est désormais également disponible dans les expressions de liaison de données.

    > [!NOTE]
    > Le membre de l’élément fonctionne pour la liaison unidirectionnelle. Si vous souhaitez effectuer une liaison bidirectionnelle, utilisez le membre **BindItem** .

    ![Prise en charge d’IntelliSense dans une liaison fortement typée](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "Prise en charge d’IntelliSense dans une liaison fortement typée")

    *Prise en charge d’IntelliSense dans une liaison fortement typée*
10. Appuyez sur **F5** pour exécuter la solution et accédez à la page clients pour vous assurer que les modifications fonctionnent comme prévu.

    ![Affichage des détails du client](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Affichage des détails du client")

    *Affichage des détails du client*
11. Fermez le navigateur et revenez à Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Tâche 2 : présentation de la liaison de modèle dans Web Forms

Dans les versions précédentes de ASP.NET Web Forms, lorsque vous souhaitiez effectuer une liaison de données bidirectionnelle, à la fois pour la récupération et la mise à jour des données, vous deviez utiliser un objet de source de données. Il peut s’agir d’une source de données d’objet, d’une source de données SQL, d’une source de données LINQ, etc. Toutefois, si votre scénario nécessitait du code personnalisé pour la gestion des données, vous deviez utiliser la source de données de l’objet et cela a apporté certains inconvénients. Par exemple, vous avez besoin d’éviter des types complexes et vous deviez gérer des exceptions lors de l’exécution de la logique de validation.

Dans la nouvelle version de ASP.NET Web Forms les contrôles liés aux données prennent en charge la liaison de modèle. Cela signifie que vous pouvez spécifier des méthodes Select, Update, INSERT et Delete directement dans le contrôle lié aux données pour appeler la logique à partir de votre fichier code-behind ou d’une autre classe.

Pour en savoir plus à ce sujet, vous allez utiliser un GridView pour répertorier les catégories de produits à l’aide du nouvel attribut **SelectMethod** . Cet attribut vous permet de spécifier une méthode pour récupérer les données GridView.

1. Ouvrez la page **Products. aspx** et incluez un **GridView**. Configurez le contrôle GridView comme indiqué ci-dessous pour utiliser des liaisons fortement typées et activer le tri et la pagination.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Utilisez le nouvel attribut **SelectMethod** pour configurer le contrôle GridView afin d’appeler une méthode **GetCategories** pour sélectionner les données.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Ouvrez le fichier code-behind **Products.aspx.cs** et ajoutez les instructions using suivantes.

    (Extrait de code- *Web Forms Lab-Ex01-Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Ajoutez un membre privé dans la classe **Products** et assignez une nouvelle instance de **ProductsContext**. Cette propriété stocke le Entity Framework contexte de données qui vous permet de vous connecter à la base de données.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Créez une méthode **GetCategories** pour récupérer la liste des catégories à l’aide de Linq. La requête inclut la propriété **Products** afin que le GridView puisse afficher la quantité de produits pour chaque catégorie. Notez que la méthode retourne un objet IQueryable brut qui représente la requête à exécuter ultérieurement dans le cycle de vie de la page.

    (Extrait de code- *Web Forms Lab-Ex01-GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > Dans les versions précédentes de ASP.NET Web Forms, activation du tri et de la pagination à l’aide de votre propre logique de référentiel dans un contexte de source de données d’objet, nécessaire pour écrire votre propre code personnalisé et recevoir tous les paramètres nécessaires. À présent, comme les méthodes de liaison de données peuvent retourner IQueryable et cela représente une requête à exécuter, ASP.NET peut s’occuper de la modification de la requête pour ajouter les paramètres de tri et de pagination appropriés.
6. Appuyez sur **F5** pour démarrer le débogage du site et accéder à la page produits. Vous devez voir que le contrôle GridView est rempli avec les catégories retournées par la méthode GetCategories.

    ![Remplissage d’un GridView à l’aide d’une liaison de modèle](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "Remplissage d’un GridView à l’aide d’une liaison de modèle")

    *Remplissage d’un GridView à l’aide d’une liaison de modèle*
7. Appuyez sur **maj**+**F5** arrêter le débogage.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Tâche 3-fournisseurs de valeur dans la liaison de modèle

Non seulement la liaison de modèle vous permet de spécifier des méthodes personnalisées pour utiliser vos données directement dans le contrôle lié aux données, mais vous permet également de mapper des données de la page dans des paramètres à partir de ces méthodes. Sur le paramètre de méthode, vous pouvez utiliser des attributs de fournisseur de valeur pour spécifier la source de données de la valeur. Exemple :

- Contrôles sur la page
- Valeurs de chaîne de requête
- Afficher les données
- État de session
- Cookies
- Données de formulaire publiées
- État d’affichage
- Les fournisseurs de valeurs personnalisés sont également pris en charge.

Si vous avez utilisé ASP.NET MVC 4, vous remarquerez que la prise en charge de la liaison de modèle est similaire. En effet, ces fonctionnalités ont été extraites de ASP.NET MVC et déplacées dans l’assembly **System. Web** pour pouvoir les utiliser également sur Web Forms.

Dans cette tâche, vous allez mettre à jour le contrôle GridView pour filtrer ses résultats en fonction de la quantité de produits pour chaque catégorie, en recevant le paramètre de filtre avec la liaison de modèle.

1. Revenez à la page **Products. aspx** .
2. En haut du contrôle GridView, ajoutez une **étiquette** et une **zone de liste déroulante** pour sélectionner le nombre de produits pour chaque catégorie, comme indiqué ci-dessous.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Ajoutez un **EmptyDataTemplate** au GridView pour afficher un message lorsqu’il n’existe aucune catégorie avec le nombre de produits sélectionné.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Ouvrez le code-behind **Products.aspx.cs** et ajoutez l’instruction using suivante.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modifiez la méthode **GetCategories** pour recevoir un argument **minProductsCount** entier et filtrer les résultats retournés. Pour ce faire, remplacez la méthode par le code suivant.

    (Extrait de code- *Web Forms Lab-Ex01-GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Le nouvel attribut **[Control]** sur l’argument **minProductsCount** permettra à ASP.net de savoir que sa valeur doit être remplie à l’aide d’un contrôle dans la page. ASP.NET recherchera tout contrôle correspondant au nom de l’argument (minProductsCount) et effectuera le mappage et la conversion nécessaires pour remplir le paramètre avec la valeur de contrôle.

    L’attribut fournit également un constructeur surchargé qui vous permet de spécifier le contrôle à partir duquel la valeur doit être obtenue.

    > [!NOTE]
    > L’un des objectifs des fonctionnalités de liaison de données est de réduire la quantité de code qui doit être écrite pour l’interaction entre les pages. Outre le fournisseur de valeurs [Control], vous pouvez utiliser d’autres fournisseurs de liaison de modèle dans vos paramètres de méthode. Certaines d’entre elles sont répertoriées dans l’introduction des tâches.
6. Appuyez sur **F5** pour démarrer le débogage du site et accéder à la page produits. Sélectionnez un certain nombre de produits dans la liste déroulante et remarquez comment le contrôle GridView est maintenant mis à jour.

    ![Filtrage du contrôle GridView avec une valeur de liste déroulante](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "Filtrage du contrôle GridView avec une valeur de liste déroulante")

    *Filtrage du contrôle GridView avec une valeur de liste déroulante*
7. Arrêter le débogage.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Tâche 4 : utilisation de la liaison de modèle pour le filtrage

Dans cette tâche, vous allez ajouter un deuxième contrôle GridView enfant pour afficher les produits dans la catégorie sélectionnée.

1. Ouvrez la page **Products. aspx** et mettez à jour les catégories GridView pour générer automatiquement le bouton Sélectionner.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Ajoutez un deuxième contrôle **GridView** nommé **productsGrid** en bas. Définissez le **ItemType** sur **WebFormsLab. Model. Product**, **DataKeyNames** sur **ProductID** et **SelectMethod** sur **GetProducts**. Affectez à **AutoGenerateColumns** la **valeur false** et ajoutez les colonnes pour ProductID, ProductName, description et UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Ouvrez le fichier code-behind **Products.aspx.cs** . Implémentez la méthode **GetProducts** pour recevoir l’ID de catégorie de la catégorie GridView et filtrer les produits. La liaison de modèle définit la valeur de paramètre à l’aide de la ligne sélectionnée dans le **categoriesGrid**. Étant donné que le nom de l’argument et le nom du contrôle ne correspondent pas, vous devez spécifier le nom du contrôle dans le fournisseur de valeurs du contrôle.

    (Extrait de code- *Web Forms Lab-Ex01-GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Grâce à cette approche, il est plus facile d’effectuer des tests unitaires de ces méthodes. Dans un contexte de test unitaire, où Web Forms ne s’exécute pas, l’attribut [Control] n’effectue aucune action spécifique.
4. Ouvrez la page **Products. aspx** et recherchez le GridView Products. Mettez à jour le GridView Products pour afficher un lien permettant de modifier le produit sélectionné.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Ouvrez le code-behind de la page **ProductDetails. aspx** et remplacez la méthode **SelectProduct** par le code suivant.

    (Extrait de code- *Web Forms Lab-méthode Ex01-SelectProduct*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Notez que l’attribut **[QueryString]** est utilisé pour remplir le paramètre de méthode à partir d’un paramètre ProductID dans la chaîne de requête.
6. Appuyez sur **F5** pour démarrer le débogage du site et accéder à la page produits. Sélectionnez une catégorie dans le GridView catégories et notez que le GridView Products est mis à jour.

    ![Présentation des produits de la catégorie sélectionnée](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "Présentation des produits de la catégorie sélectionnée")

    *Présentation des produits de la catégorie sélectionnée*
7. Cliquez sur le lien **Afficher** d’un produit pour ouvrir la page ProductDetails. aspx.

    Notez que la page récupère le produit avec SelectMethod en utilisant le paramètre productId de la chaîne de requête.

    ![Affichage des détails du produit](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "Affichage des détails du produit")

    *Affichage des détails du produit*

    > [!NOTE]
    > La possibilité de taper une description HTML sera implémentée dans l’exercice suivant.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Tâche 5-utilisation de la liaison de modèle pour les opérations de mise à jour

Dans la tâche précédente, vous avez utilisé la liaison de modèle principalement pour la sélection des données. dans cette tâche, vous allez apprendre à utiliser la liaison de modèle dans les opérations de mise à jour.

Vous allez mettre à jour les catégories GridView pour permettre à l’utilisateur de mettre à jour les catégories.

1. Ouvrez la page **Products. aspx** et mettez à jour les catégories GridView pour générer automatiquement le bouton modifier et utilisez le nouvel attribut **UpdateMethod** pour spécifier une méthode **UpdateCategory** pour mettre à jour l’élément sélectionné.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    L’attribut DataKeyNames dans le GridView définissent les membres qui identifient de façon unique l’objet lié au modèle et, par conséquent, qui sont les paramètres que la méthode Update doit recevoir au moins.
2. Ouvrez le fichier code-behind **Products.aspx.cs** et implémentez la méthode **UpdateCategory** . La méthode doit recevoir l’ID de catégorie pour charger la catégorie actuelle, remplir les valeurs à partir de GridView, puis mettre à jour la catégorie.

    (Extrait de code- *Web Forms Lab-Ex01-UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    La nouvelle méthode **TryUpdateModel** dans la classe page est chargée de remplir l’objet de modèle à l’aide des valeurs des contrôles de la page. Dans ce cas, elle remplace les valeurs mises à jour de la ligne GridView actuelle en cours de modification dans l’objet **Category** .

    > [!NOTE]
    > L’exercice suivant explique l’utilisation de ModelState. IsValid pour valider les données entrées par l’utilisateur lors de la modification de l’objet.
3. Exécutez le site et accédez à la page produits. Modifiez une catégorie. Tapez un nouveau nom, puis cliquez sur **mettre à jour** pour rendre les modifications persistantes.

    ![Modification des catégories](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Modification des catégories")

    *Modification des catégories*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Exercice 2 : validation des données

Dans cet exercice, vous allez découvrir les nouvelles fonctionnalités de validation des données dans ASP.NET 4,5. Vous allez découvrir les nouvelles fonctionnalités de validation discrètes dans Web Forms. Vous allez utiliser des annotations de données dans les classes de modèle d’application pour la validation des entrées utilisateur, et enfin, vous apprendrez comment activer ou désactiver la validation de demande pour des contrôles individuels dans une page.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Tâche 1 : validation discrète

Les formulaires avec des données complexes, y compris les validateurs, tendent à générer trop de code JavaScript dans la page, ce qui peut représenter environ 60% du code. Une fois la validation discrète activée, votre code HTML paraît plus clair et plus propre.

Dans cette section, vous allez activer la validation discrète dans ASP.NET pour comparer le code HTML généré par les deux configurations.

1. Ouvrez **Visual Studio 2012** et ouvrez la solution **Begin** située dans le dossier **Source\Ex2-Validation\Begin** de ce Lab. Vous pouvez également continuer à travailler sur votre solution existante à partir de l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, dans le Explorateur de solutions, cliquez sur le projet **WebFormsLab** **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Appuyez sur **F5** pour démarrer l’application Web. Accédez à la page clients, puis cliquez sur le lien **Ajouter un nouveau client** .
3. Cliquez avec le bouton droit sur la page du navigateur, puis sélectionnez Afficher l’option **source** pour ouvrir le code HTML généré par l’application.

    ![Présentation du code HTML de la page](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "Présentation du code HTML de la page")

    *Présentation du code HTML de la page*
4. Faites défiler le code source de la page et remarquez que ASP.NET a injecté du code JavaScript et des validateurs de données dans la page pour effectuer les validations et afficher la liste d’erreurs.

    ![Code JavaScript de validation dans la page CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "Code JavaScript de validation dans la page CustomerDetails")

    *Code JavaScript de validation dans la page CustomerDetails*
5. Fermez le navigateur et revenez à Visual Studio.
6. Vous allez maintenant activer la validation discrète. Ouvrez le **fichier Web. config** et recherchez la clé **ValidationSettings : UnobtrusiveValidationMode** dans la section **appSettings** **.** Définissez la valeur de clé sur **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Vous pouvez également définir cette propriété dans la **Page &quot;\_** événement de chargement&quot; au cas où vous souhaiteriez activer la validation discrète uniquement pour certaines pages.
7. Ouvrez **CustomerDetails. aspx** et appuyez sur **F5** pour démarrer l’application Web.
8. Appuyez sur la touche F12 pour ouvrir les outils de développement IE. Une fois les outils de développement ouverts, sélectionnez l’onglet script. Sélectionnez **CustomerDetails. aspx** dans le menu et prenez note que les scripts requis pour exécuter jQuery sur la page ont été chargés dans le navigateur à partir du site local.

    ![Chargement des fichiers JavaScript jQuery directement à partir du serveur IIS local](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "Chargement des fichiers JavaScript jQuery directement à partir du serveur IIS local")

    *Chargement des fichiers JavaScript jQuery directement à partir du serveur IIS local*
9. Fermez le navigateur pour revenir à Visual Studio. Rouvrez le fichier **site. Master** et recherchez **ScriptManager**. Ajoutez la propriété **EnableCdn** de l’attribut avec la valeur **true**. Cela force le chargement de jQuery à partir de l’URL en ligne, et non à partir de l’URL du site local.
10. Ouvrez **CustomerDetails. aspx** dans Visual Studio. Appuyez sur la touche F5 pour exécuter le site. Une fois Internet Explorer ouvert, appuyez sur la touche F12 pour ouvrir les outils de développement. Sélectionnez l’onglet **script** , puis examinez la liste déroulante. Notez que les fichiers JavaScript jQuery ne sont plus chargés à partir du site local, mais qu’à partir du CDN en ligne.

    ![Chargement des fichiers JavaScript jQuery à partir du CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "Chargement des fichiers JavaScript jQuery à partir du CDN")

    *Chargement des fichiers JavaScript jQuery à partir du CDN*
11. Rouvrez le code source de la page HTML à l’aide de l’option Afficher la source dans le navigateur. Notez que, en activant le ASP.NET de validation discrète, le code JavaScript injecté a été remplacé par des attributs de \*de données.

    ![Code de validation discret](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "Code de validation discret")

    *Code de validation discret*

    > [!NOTE]
    > Dans cet exemple, vous avez vu comment un résumé des validations avec des annotations de données a été simplifié pour quelques lignes HTML et JavaScript. Auparavant, sans validation discrète, plus vous ajoutez de contrôles de validation, plus votre code de validation JavaScript sera volumineux.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Tâche 2 : validation du modèle à l’aide d’annotations de données

ASP.NET 4,5 introduit la validation des annotations de données pour Web Forms. Au lieu d’avoir un contrôle de validation sur chaque entrée, vous pouvez maintenant définir des contraintes dans vos classes de modèle et les utiliser dans toutes vos applications Web. Dans cette section, vous allez apprendre à utiliser des annotations de données pour valider un formulaire nouveau client/modifier un client.

1. Ouvrez la page **CustomerDetail. aspx** . Notez que le prénom et le deuxième nom du client dans les sections **EditItemTemplate** et **InsertItemTemplate** sont validés à l’aide de contrôles RequiredFieldValidator. Chaque validateur étant associé à une condition particulière, vous devez inclure autant de validateurs que de conditions à vérifier.
2. Ajoutez des annotations de données pour valider la classe Customer Model. Ouvrez la classe **Customer.cs** dans le dossier **Model** et *décorer* chaque propriété à l’aide d’attributs d’annotation de données.

    (Extrait de code- *Web Forms Lab-Ex02-annotations de données*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET Framework 4,5 a étendu la collection d’annotations de données existante. Voici quelques-unes des annotations de données que vous pouvez utiliser : [CreditCard], [Telephone], [EmailAddress], [Range], [compare], [URL], [FileExtensions], [required], [Clé], [RegularExpression].
    > 
    > Voici quelques exemples d’utilisation :
    > 
    > [Clé]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Vous pouvez également définir vos propres messages d’erreur dans chaque attribut.
3. Ouvrez **CustomerDetails. aspx** et supprimez tous les RequiredFieldValidators pour les premier et dernier noms des champs dans les sections EditItemTemplate et InsertItemTemplate du contrôle FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > L’un des avantages de l’utilisation des annotations de données est que la logique de validation n’est pas dupliquée dans les pages de votre application. Vous la définissez une fois dans le modèle et l’utilisez dans toutes les pages d’application qui manipulent les données.
4. Ouvrez le code-behind **CustomerDetails. aspx** et recherchez la méthode SaveCustomer. Cette méthode est appelée lors de l’insertion d’un nouveau client et reçoit le paramètre Customer des valeurs du contrôle FormView. Lorsque le mappage entre les contrôles de page et l’objet de paramètre se produit, ASP.NET exécute la validation du modèle par rapport à tous les attributs d’annotation de données et remplit le dictionnaire ModelState avec les erreurs rencontrées, le cas échéant.

    ModelState. IsValid retourne la valeur true uniquement si tous les champs de votre modèle sont valides après l’exécution de la validation.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Ajoutez un contrôle **ValidationSummary** à la fin de la page CustomerDetails pour afficher la liste des erreurs de modèle.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** est une nouvelle propriété du contrôle ValidationSummary qui, lorsqu’elle est définie sur **true**, le contrôle affiche les erreurs du dictionnaire ModelState. Ces erreurs proviennent de la validation des annotations de données.
6. Appuyez sur **F5** pour exécuter l’application Web. Complétez le formulaire avec des valeurs erronées et cliquez sur **Enregistrer** pour exécuter la validation. Notez le résumé des erreurs en bas.

    ![Validation avec des annotations de données](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Validation avec des annotations de données")

    *Validation avec des annotations de données*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Tâche 3 : gestion des erreurs de base de données personnalisées avec ModelState

Dans la version précédente de Web Forms, la gestion des erreurs de base de données, telles qu’une chaîne trop longue ou une violation de clé unique, peut impliquer la levée d’exceptions dans votre code de référentiel, puis la gestion des exceptions sur votre code-behind pour afficher une erreur. Une grande quantité de code est nécessaire pour effectuer une opération relativement simple.

Dans Web Forms 4,5, l’objet ModelState peut être utilisé pour afficher les erreurs sur la page, soit à partir de votre modèle, soit à partir de la base de données, de manière cohérente.

Dans cette tâche, vous allez ajouter du code pour gérer correctement les exceptions de base de données et afficher le message approprié à l’utilisateur à l’aide de l’objet ModelState.

1. Lorsque l’application est toujours en cours d’exécution, essayez de mettre à jour le nom d’une catégorie à l’aide d’une valeur dupliquée.

    ![Mise à jour d’une catégorie avec un nom dupliqué](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "Mise à jour d’une catégorie avec un nom dupliqué")

    *Mise à jour d’une catégorie avec un nom dupliqué*

    Notez qu’une exception est levée en raison de la contrainte &quot;unique&quot; de la colonne **CategoryName** .

    ![Exception pour les noms de catégories dupliqués](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "Exception pour les noms de catégories dupliqués")

    *Exception pour les noms de catégories dupliqués*
2. Arrêter le débogage. Dans le fichier code-behind **Products.aspx.cs** , mettez à jour la méthode **UpdateCategory** pour gérer les exceptions levées par la base de code. L’appel de méthode SaveChanges () et ajoute une erreur à l’objet **ModelState** .

    La nouvelle méthode **TryUpdateModel** met à jour l’objet Category récupéré de la base de données à l’aide des données de formulaire fournies par l’utilisateur.

    (Extrait de code- *Web Forms Lab-Ex02-UpdateCategory handle Errors*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Dans l’idéal, vous devez identifier la cause du exception dbupdateexception et vérifier si la cause racine est la violation d’une contrainte de clé unique.
3. Ouvrez **Products. aspx** et ajoutez un contrôle **ValidationSummary** sous le contrôle GridView categories pour afficher la liste des erreurs de modèle.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Exécutez le site et accédez à la page produits. Essayez de mettre à jour le nom d’une catégorie à l’aide d’une valeur dupliquée.

    Notez que l’exception a été gérée et que le message d’erreur s’affiche dans le contrôle **ValidationSummary** .

    ![Erreur de catégorie dupliquée](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "Erreur de catégorie dupliquée")

    *Erreur de catégorie dupliquée*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Tâche 4 : validation de la demande dans ASP.NET Web Forms 4,5

La fonctionnalité de validation de la demande dans ASP.NET fournit un certain niveau de protection par défaut contre les attaques de script entre sites (XSS). Dans les versions précédentes de ASP.NET, la validation de la demande était activée par défaut et pouvait uniquement être désactivée pour une page entière. Avec la nouvelle version de ASP.NET Web Forms vous pouvez désormais désactiver la validation de la demande pour un seul contrôle, effectuer une validation différée des demandes ou accéder aux données de requête non validées (soyez prudent si vous le faites !).

1. Appuyez sur **CTRL + F5** pour démarrer le site sans débogage et accédez à la page produits. Sélectionnez une catégorie, puis cliquez sur le lien **modifier** sur l’un des produits.
2. Tapez une description contenant du contenu potentiellement dangereux, par exemple, avec des balises HTML. Prenez note de l’exception levée en raison de la validation de la demande.

    ![Modification d’un produit avec un contenu potentiellement dangereux](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "Modification d’un produit avec un contenu potentiellement dangereux")

    *Modification d’un produit avec un contenu potentiellement dangereux*

    ![Exception levée en raison de la validation de la demande](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "Exception levée en raison de la validation de la demande")

    *Exception levée en raison de la validation de la demande*
3. Fermez la page et, dans Visual Studio, appuyez sur **Maj + F5** pour arrêter le débogage.
4. Ouvrez la page **ProductDetails. aspx** et recherchez la zone de texte **Description** .
5. Ajoutez la nouvelle propriété **ValidateRequestMode** à la zone de texte et affectez-lui la valeur **Disabled**.

    Le nouvel attribut **ValidateRequestMode** vous permet de désactiver la validation de la demande de manière granulaire sur chaque contrôle. Cela est utile lorsque vous souhaitez utiliser une entrée qui peut recevoir du code HTML, mais que vous souhaitez conserver la validation opérationnelle pour le reste de la page.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Appuyez sur **F5** pour exécuter l’application Web. Ouvrez à nouveau la page modifier le produit et complétez la description du produit, y compris les balises HTML. Notez que vous pouvez maintenant ajouter du contenu HTML à la description.

    ![Validation de la demande désactivée pour la description du produit](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "Validation de la demande désactivée pour la description du produit")

    *Validation de la demande désactivée pour la description du produit*

    > [!NOTE]
    > Dans une application de production, vous devez nettoyer le code HTML entré par l’utilisateur pour vous assurer que seules les balises HTML sécurisées sont entrées (par exemple, il n’y a pas de balises de&gt; de script &lt;). Pour ce faire, vous pouvez utiliser la [bibliothèque de protection Web Microsoft](https://www.nuget.org/packages/AntiXSS).
7. Modifiez de nouveau le produit. Tapez le code HTML dans le champ nom, puis cliquez sur **Enregistrer**. Notez que la validation de la demande est uniquement désactivée pour le champ Description et que le reste des champs est encore validé par rapport au contenu potentiellement dangereux.

    ![Validation de la demande activée dans le reste des champs](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "Validation de la demande activée dans le reste des champs")

    *Validation de la demande activée dans le reste des champs*

    ASP.NET Web Forms 4,5 comprend un nouveau mode de validation des demandes pour effectuer une validation différée des demandes. Lorsque le mode de validation de la demande est défini sur **4,5**, si une partie du code accède à *Request. Form [&quot;Key&quot;]* , la validation de la demande de ASP.net 4.5 déclenche uniquement la validation de la demande pour cet élément spécifique dans la collection de formulaires.

    En outre, ASP.NET 4,5 comprend désormais les routines d’encodage principales de Microsoft Anti-XSS Library v 4.0. Les routines d’encodage Anti-XSS sont implémentées par le nouveau type *AntiXssEncoder* trouvé dans le nouvel espace de noms **System. Web. Security. AntiXss** . Avec le paramètre **encoderType** configuré pour utiliser *AntiXssEncoder*, tout l’encodage de sortie dans ASP.NET utilise automatiquement les nouvelles routines d’encodage.
8. La validation de requête ASP.NET 4,5 prend également en charge l’accès non validé aux données de demande. ASP.NET 4,5 ajoute une nouvelle propriété de collection à l’objet **HttpRequest** appelé **unvalidated**. Lorsque vous naviguez dans **HttpRequest. unvalidated** , vous avez accès à tous les éléments de données de requête les plus courants, y compris les formulaires, les chaînes, les cookies, les URL, etc.

    ![Objet Request. unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Objet Request. unvalidated")

    *Objet Request. unvalidated*

    > [!NOTE]
    > **Utilisez la propriété HttpRequest. unvalidated avec précaution.** Veillez à effectuer une validation personnalisée sur les données de demande brutes pour vous assurer que le texte dangereux n’est pas un aller-retour et un rendu à des clients non suspects !

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Exercice 3 : traitement de page asynchrone dans ASP.NET Web Forms

Dans cet exercice, vous allez découvrir les nouvelles fonctionnalités de traitement de page asynchrone dans ASP.NET Web Forms.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Tâche 1 : mise à jour de la page Détails du produit pour télécharger et afficher des images

Dans cette tâche, vous allez mettre à jour la page Détails du produit pour permettre à l’utilisateur de spécifier une URL d’image pour le produit et de l’afficher dans la vue en lecture seule. Vous allez créer une copie locale de l’image spécifiée en la téléchargeant de façon synchrone. Dans la tâche suivante, vous allez mettre à jour cette implémentation pour qu’elle fonctionne de manière asynchrone.

1. Ouvrez **Visual Studio 2012** et chargez la solution **Begin** située dans **Source\Ex3-Async\Begin** à partir du dossier de ce laboratoire. Vous pouvez également continuer à travailler sur votre solution existante à partir des exercices précédents.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, dans le Explorateur de solutions, cliquez sur le projet **WebFormsLab** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez la source de la page **ProductDetails. aspx** et ajoutez un champ dans le ItemTemplate du FormView pour afficher l’image du produit.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Ajoutez un champ pour spécifier l’URL de l’image dans le EditTemplate du FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Ouvrez le fichier code-behind **ProductDetails.aspx.cs** et ajoutez les directives d’espace de noms suivantes.

    (Extrait de code- *Web Forms Lab-Ex03-Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Créez une méthode **UpdateProductImage** pour stocker des images distantes dans le dossier **images** locales et mettez à jour l’entité Product avec la nouvelle valeur d’emplacement d’image.

    (Extrait de code- *Web Forms Lab-Ex03-UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Mettez à jour la méthode **UpdateProduct** pour appeler la méthode **UpdateProductImage** .

    (Extrait de code- *Web Forms Lab-Ex03-UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Exécutez l’application et essayez de télécharger une image pour un produit. Par exemple, vous pouvez utiliser l’URL d’image suivante à partir de Office clip arts : [ [http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Définition d’une image pour un produit](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "Définition d’une image pour un produit")

    *Définition d’une image pour un produit*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Tâche 2 : ajout d’un traitement asynchrone à la page Détails du produit

Dans cette tâche, vous allez mettre à jour la page Détails du produit pour qu’elle fonctionne de manière asynchrone. Vous allez améliorer une tâche à long terme (le processus de téléchargement d’image) à l’aide du traitement de page asynchrone ASP.NET 4,5.

Les méthodes asynchrones dans les applications Web peuvent être utilisées pour optimiser la façon dont les pools de threads ASP.NET sont utilisés. Dans ASP.NET, un nombre limité de threads dans le pool de threads est utilisé pour assister les demandes. par conséquent, lorsque tous les threads sont occupés, ASP.NET commence à rejeter les nouvelles demandes, envoie des messages d’erreur d’application et rend votre site indisponible.

Les opérations de longue durée sur votre site Web sont de bons candidats pour la programmation asynchrone, car elles occupent le thread affecté pendant une longue période. Cela comprend les demandes de longue durée, les pages contenant un grand nombre d’éléments et de pages différents qui nécessitent des opérations hors connexion, telles que l’interrogation d’une base de données ou l’accès à un serveur Web externe. L’avantage est que si vous utilisez des méthodes asynchrones pour ces opérations, pendant que la page est en cours de traitement, le thread est libéré et retourné au pool de threads et peut être utilisé pour assister à une nouvelle demande de page. Cela signifie que la page commence le traitement dans un thread à partir du pool de threads et peut terminer le traitement dans un thread différent, une fois le traitement asynchrone terminé.

1. Ouvrez la page **ProductDetails. aspx** . Ajoutez l’attribut **Async** dans l’élément **page** et affectez-lui la valeur **true**. Cet attribut indique à ASP.NET d’implémenter l’interface IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Ajoutez une étiquette au bas de la page pour afficher les détails des threads qui exécutent la page.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Ouvrez **ProductDetails.aspx.cs** et ajoutez les directives d’espace de noms suivantes.

    (Extrait de code- *Web Forms Lab-Ex03-Namespaces 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modifiez la méthode **UpdateProductImage** pour télécharger l’image avec une tâche asynchrone. Vous allez remplacer la méthode **DownloadFile** de **WebClient** par la méthode **DownloadFileTaskAsync** et inclure le mot clé **await** .

    (Extrait de code- *Web Forms Lab-Ex03-UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    Le RegisterAsyncTask inscrit une nouvelle tâche asynchrone de page à exécuter dans un thread différent. Elle reçoit une expression lambda avec la tâche (t) à exécuter. Le mot clé **await** dans la méthode **DownloadFileTaskAsync** convertit le reste de la méthode en un rappel qui est appelé de façon asynchrone après la fin de la méthode **DownloadFileTaskAsync** . ASP.NET reprendra l’exécution de la méthode en conservant automatiquement toutes les valeurs d’origine de la requête HTTP. Le nouveau modèle de programmation asynchrone dans .NET 4,5 vous permet d’écrire du code asynchrone qui ressemble beaucoup à du code synchrone et laisse le compilateur gérer les complications des fonctions de rappel ou du code de continuation.
    > [!NOTE]
    > RegisterAsyncTask et PageAsyncTask étaient déjà disponibles depuis .NET 2,0. Le mot clé await est une nouveauté du modèle de programmation asynchrone .NET 4,5 et peut être utilisé avec les nouvelles méthodes TaskAsync à partir de l’objet WebClient .NET.
5. Ajoutez du code pour afficher les threads sur lesquels le code a démarré et s’est terminé de s’exécuter. Pour ce faire, mettez à jour la méthode **UpdateProductImage** avec le code suivant.

    (Extrait de code- *Web Forms Lab-Ex03-Show threads*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Ouvrez le fichier **Web. config** du site Web. Ajoutez la variable appSetting suivante.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Appuyez sur **F5** pour exécuter l’application et télécharger une image pour le produit. Notez l’ID des threads où le code démarré et terminé peut être différent. Cela est dû au fait que les tâches asynchrones s’exécutent sur un thread séparé du pool de threads ASP.NET. Une fois la tâche terminée, ASP.NET replace la tâche dans la file d’attente et affecte l’un des threads disponibles.

    ![Téléchargement d’une image de façon asynchrone](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "Téléchargement d’une image de façon asynchrone")

    *Téléchargement d’une image de façon asynchrone*

> [!NOTE]
> En outre, vous pouvez déployer cette application sur Azure à l' [annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy](#AppendixB).

---

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

Dans ce laboratoire pratique, les concepts suivants ont été traités et démontrés :

- Utiliser des expressions de liaison de données fortement typées
- Utilisez les nouvelles fonctionnalités de liaison de modèle dans Web Forms
- Utiliser des fournisseurs de valeurs pour mapper des données de page à des méthodes code-behind
- Utiliser des annotations de données pour la validation des entrées utilisateur
- Tirez parti de la validation côté client discrète avec jQuery dans Web Forms
- Implémenter la validation granulaire des demandes
- Implémenter le traitement de page asynchrone dans Web Forms

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe A : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Azure</em>&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l'installation](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .

    ![Vignette VS Express pour le Web](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *Vignette VS Express pour le Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

Cette annexe va vous montrer comment créer un nouveau site Web à partir du portail Azure et publier l’application que vous avez obtenue en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tâche 1 : création d’un nouveau site Web à partir du portail Azure

1. Accédez à la [portail de gestion Azure](https://manage.windowsazure.com/) et connectez-vous à l’aide des informations d’identification Microsoft associées à votre abonnement.

    > [!NOTE]
    > Avec Azure, vous pouvez héberger gratuitement 10 sites Web ASP.NET, puis mettre à l’échelle à mesure que votre trafic augmente. Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).

    ![Ouvrir une session sur Windows Portail Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Ouvrir une session sur Windows Portail Azure")

    *Connexion au portail*
2. Cliquez sur **nouveau** dans la barre de commandes.

    ![Création d’un nouveau site Web](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "Création d’un nouveau site Web")

    *Création d’un nouveau site Web*
3. Cliquez sur **compute** | **site Web**. Sélectionnez ensuite l’option **création rapide** . Fournissez une URL disponible pour le nouveau site Web, puis cliquez sur **créer un site Web**.

    > [!NOTE]
    > Azure est l’hôte d’une application Web exécutée dans le Cloud que vous pouvez contrôler et gérer. L’option création rapide vous permet de déployer une application Web terminée sur Azure à partir de l’extérieur du portail. Elle n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un nouveau site Web à l’aide de la création rapide](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "Création d’un nouveau site Web à l’aide de la création rapide")

    *Création d’un nouveau site Web à l’aide de la création rapide*
4. Patientez jusqu’à la création du nouveau **site Web** .
5. Une fois le site Web créé, cliquez sur le lien sous la colonne **URL** . Vérifiez que le nouveau site Web fonctionne.

    ![Navigation vers le nouveau site Web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "Navigation vers le nouveau site Web")

    *Navigation vers le nouveau site Web*

    ![Site Web en cours d’exécution](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "Site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site Web dans la colonne **nom** pour afficher les pages de gestion.

    ![Ouverture des pages de gestion de site Web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "Ouverture des pages de gestion de site Web")

    *Ouverture des pages de gestion de site Web*
7. Dans la page **tableau de bord** , sous la section **Aperçu rapide** , cliquez sur le lien **Télécharger le profil de publication** .

    > [!NOTE]
    > Le *profil de publication* contient toutes les informations requises pour publier une application Web sur Azure pour chaque méthode de publication activée. Le profil de publication contient les URL, les informations d'identification de l'utilisateur et les chaînes de base de données nécessaires pour la connexion et l'authentification auprès de chacun des points de terminaison pour lesquels une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour le web** et **Microsoft Visual Studio 2012** prennent en charge la lecture des profils de publication pour automatiser la configuration de ces programmes pour la publication d’applications Web sur Azure.

    ![Téléchargement du profil de publication du site Web](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "Téléchargement du profil de publication du site Web")

    *Téléchargement du profil de publication du site Web*
8. Téléchargez le fichier de profil de publication dans un emplacement connu. Dans cet exercice, vous allez apprendre à utiliser ce fichier pour publier une application Web sur Azure à partir de Visual Studio.

    ![Enregistrement du fichier de profil de publication](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "Enregistrement du profil de publication")

    *Enregistrement du fichier de profil de publication*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application utilise des bases de données SQL Server, vous devez créer un serveur SQL Database. Si vous souhaitez déployer une application simple qui n’utilise pas SQL Server vous pouvez ignorer cette tâche.

1. Vous aurez besoin d’un serveur de SQL Database pour stocker la base de données d’application. Vous pouvez afficher les serveurs SQL Database à partir de votre abonnement dans le portail de gestion Azure, dans **bases de données SQL** | **serveurs** | **tableau de bord du serveur**. Si vous n’avez pas de serveur créé, vous pouvez en créer un à l’aide du bouton **Ajouter** de la barre de commandes. Notez le **nom et l’URL du serveur,** ainsi que le nom et le mot de passe de connexion de l’administrateur, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore la base de données, car elle sera créée dans une étape ultérieure.

    ![Tableau de bord du serveur SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "Tableau de bord du serveur SQL Database")

    *Tableau de bord du serveur SQL Database*
2. Dans la tâche suivante, vous allez tester la connexion à la base de données à partir de Visual Studio. pour cette raison, vous devez inclure votre adresse IP locale dans la liste des **adresses IP autorisées**du serveur. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de l' **adresse IP actuelle du client** et collez-la dans les zones de texte **adresse IP de début** et **adresse IP de fin** , puis cliquez sur le bouton ![ajouter-client-IP-adresse-OK-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png).

    ![Ajout de l’adresse IP du client](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Ajout de l’adresse IP du client*
3. Une fois l' **adresse IP du client** ajoutée à la liste des adresses IP autorisées, cliquez sur **Enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Confirmer les modifications*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet de site Web et sélectionnez **publier**.

    ![Publication de l’application](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "Publication de l'application")

    *Publication du site Web*
2. Importez le profil de publication que vous avez enregistré dans la première tâche.

    ![Importation du profil de publication](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la validation terminée, cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte s’affiche en regard du bouton valider la connexion.

    ![Validation de la connexion](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "Validation de la connexion")

    *Validation de la connexion*
4. Dans la page **paramètres** , sous la section **bases de données** , cliquez sur le bouton en regard de la zone de texte de votre connexion à la base de données (par exemple, **DefaultConnection**).

    ![Configuration de Web Deploy](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Configuration de Web Deploy")

    *Configuration de Web Deploy*
5. Configurez la connexion de base de données comme suit :

   - Dans **nom du serveur** , tapez l’URL de votre serveur SQL Database à l’aide du préfixe *TCP :* .
   - Dans **nom d’utilisateur** , tapez le nom de connexion de l’administrateur du serveur.
   - Dans **mot de passe** , tapez le mot de passe de connexion de l’administrateur du serveur.
   - Tapez un nouveau nom de base de données.

     ![Configuration de la chaîne de connexion de destination](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Configuration de la chaîne de connexion de destination")

     *Configuration de la chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "Création de la chaîne de base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous allez utiliser pour vous connecter à SQL Database dans Azure est indiquée dans la zone de texte connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "Chaîne de connexion pointant vers SQL Database")

    *Chaîne de connexion pointant vers SQL Database*
8. Dans la page **Aperçu** , cliquez sur **publier**.

    ![Publication de l’application Web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "Publication de l’application Web")

    *Publication de l’application Web*
9. Une fois le processus de publication terminé, votre navigateur par défaut ouvre le site Web publié.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Annexe C : utilisation d’extraits de code

Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main. Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.

![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")

*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***

1. Placez le curseur à l’endroit où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).
3. Regarder comme IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).
5. Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "Commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*

![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")

*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*

***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0. Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.

1. Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.
2. Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.

![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")

*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*

![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")

*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*
