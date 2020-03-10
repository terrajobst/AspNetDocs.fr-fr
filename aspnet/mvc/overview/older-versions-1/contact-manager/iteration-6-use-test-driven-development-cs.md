---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: '#6 d’itération : utiliser le développement piloté parC#les tests () | Microsoft Docs'
author: microsoft
description: Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code sur les tests unitaires. Dans cette itération,...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: aee0ff9d8d7f17e8a00dab12467bd3a3457fbe18
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601800"
---
# <a name="iteration-6--use-test-driven-development-c"></a>#6 d’itération : utiliser le développement piloté parC#les tests ()

par [Microsoft](https://github.com/microsoft)

[Télécharger le code](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code sur les tests unitaires. Dans cette itération, nous ajoutons des groupes de contacts.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Création d’une application MVC ASP.NET de gestionC#des contacts ()

Dans cette série de didacticiels, nous créons une application de gestion de contacts entière du début à la fin. L’application Gestionnaire de contacts vous permet de stocker les informations de contact, les noms de téléphone et les adresses de messagerie, pour obtenir la liste des personnes.

Nous générons l’application sur plusieurs itérations. À chaque itération, nous améliorons progressivement l’application. L’objectif de cette approche à plusieurs itérations est de vous permettre de comprendre la raison de chaque modification.

- #1 d’itération : créez l’application. Dans la première itération, nous créons le gestionnaire de contacts de la manière la plus simple possible. Nous ajoutons la prise en charge des opérations de base de données de base : créer, lire, mettre à jour et supprimer (CRUD).

- Itération #2 : rendez l’application agréable. Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page maître par défaut de la vue MVC ASP.NET et la feuille de style en cascade.

- Itération #3 : ajouter une validation de formulaire. Dans la troisième itération, nous ajoutons la validation de base du formulaire. Nous empêchons les utilisateurs de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également les adresses de messagerie et les numéros de téléphone.

- Itération #4 : rendez l’application faiblement couplée. Dans cette quatrième itération, nous tirant parti de plusieurs modèles de conception de logiciels pour faciliter la gestion et la modification de l’application de gestionnaire de contacts. Par exemple, nous refactorisons notre application pour utiliser le modèle de référentiel et le modèle d’injection de dépendances.

- #5 d’itération-créer des tests unitaires. Dans la cinquième itération, nous rendons notre application plus facile à gérer et à modifier en ajoutant des tests unitaires. Nous imitons nos classes de modèle de données et créons des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6-Utilisez le développement piloté par les tests. Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code sur les tests unitaires. Dans cette itération, nous ajoutons des groupes de contacts.

- #7 d’itération-ajoutez des fonctionnalités AJAX. Dans la septième itération, nous améliorons la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.

## <a name="this-iteration"></a>Cette itération

Dans l’itération précédente de l’application du gestionnaire de contacts, nous avons créé des tests unitaires pour fournir un filet de sécurité pour notre code. La motivation de la création des tests unitaires consistait à rendre notre code plus résilient pour la modification. Avec les tests unitaires en place, nous pouvons apporter toute modification à notre code et savoir immédiatement si nous avons cassé les fonctionnalités existantes.

Dans cette itération, nous utilisons des tests unitaires dans un but totalement différent. Dans cette itération, nous utilisons des tests unitaires dans le cadre de la philosophie de conception d’une application appelée *développement piloté par les tests*. Quand vous surveillez le développement piloté par les tests, vous écrivez d’abord des tests, puis vous écrivez du code sur les tests.

Plus précisément, lors de la pratique du développement piloté par les tests, trois étapes sont nécessaires lors de la création du code (rouge/vert/refactorer) :

1. Écrire un test unitaire qui échoue (rouge)
2. Écrire du code qui réussit le test unitaire (vert)
3. Refactoriser votre code (refactorisation)

Tout d’abord, vous écrivez le test unitaire. Le test unitaire doit exprimer votre intention sur la façon dont vous vous attendez à ce que votre code se comporte. Lorsque vous créez pour la première fois le test unitaire, le test unitaire doit échouer. Le test doit échouer, car vous n’avez pas encore écrit de code d’application conforme au test.

Ensuite, vous écrivez juste suffisamment de code pour que le test unitaire réussisse. L’objectif est d’écrire le code dans le laziest, sloppiest et le plus rapide possible. Vous ne devez pas perdre du temps à réfléchir à l’architecture de votre application. Au lieu de cela, vous devez vous concentrer sur l’écriture de la quantité minimale de code nécessaire pour satisfaire l’intention exprimée par le test unitaire.

Enfin, une fois que vous avez écrit suffisamment de code, vous pouvez revenir en arrière et prendre en compte l’architecture globale de votre application. Dans cette étape, vous réécrivez (refactorisez) votre code en tirant parti des modèles de conception de logiciels, tels que le modèle de référentiel, afin que votre code soit plus gérable. Vous pouvez intrépidité réécrire votre code dans cette étape, car votre code est couvert par les tests unitaires.

Il y a de nombreux avantages qui résultent de la pratique du développement piloté par les tests. Tout d’abord, le développement piloté par les tests vous oblige à vous concentrer sur le code qui doit réellement être écrit. Étant donné que vous êtes constamment concentré sur l’écriture de suffisamment de code pour passer un test particulier, vous n’êtes pas en mesure de vous concentrer sur les mauvaises herbes et d’écrire des quantités importantes de code que vous n’utiliserez jamais.

Deuxièmement, une méthodologie de conception « test First » vous oblige à écrire du code du point de vue de l’utilisation de votre code. En d’autres termes, lors de la pratique du développement piloté par les tests, vous écrivez constamment vos tests du point de vue de l’utilisateur. Par conséquent, le développement piloté par les tests peut entraîner des API plus claires et plus compréhensibles.

Enfin, le développement piloté par les tests vous oblige à écrire des tests unitaires dans le cadre du processus normal d’écriture d’une application. En tant qu’approche de l’échéance du projet, le test est généralement la première chose qui sort de la fenêtre. En revanche, lors de la pratique du développement piloté par les tests, il est plus probable que vous soyez vertueux sur l’écriture de tests unitaires, car le développement piloté par les tests permet aux tests unitaires de centraliser le processus de création d’une application.

> [!NOTE] 
> 
> Pour en savoir plus sur le développement piloté par les tests, je vous recommande de lire le livre plumes Michael qui **fonctionne efficacement avec du code hérité**.

Dans cette itération, nous ajoutons une nouvelle fonctionnalité à notre application de gestion des contacts. Nous ajoutons la prise en charge des groupes de contacts. Vous pouvez utiliser des groupes de contacts pour organiser vos contacts en catégories telles que les groupes professionnels et amis.

Nous allons ajouter cette nouvelle fonctionnalité à notre application en suivant un processus de développement piloté par les tests. Nous allons d’abord écrire nos tests unitaires et nous allons écrire tout notre code dans le cadre de ces tests.

## <a name="what-gets-tested"></a>Ce qui est testé

Comme nous l’avons vu dans l’itération précédente, vous n’écrivez généralement pas de tests unitaires pour la logique d’accès aux données ou la logique de la vue. Vous ne pouvez pas écrire de tests unitaires pour la logique d’accès aux données, car l’accès à une base de données est une opération relativement lente. Vous ne pouvez pas écrire de tests unitaires pour la logique d’affichage, car l’accès à une vue nécessite la mise en place d’un serveur Web qui est une opération relativement lente. Vous ne devez pas écrire de test unitaire, sauf si le test peut être exécuté à nouveau très rapidement.

Étant donné que le développement piloté par les tests est piloté par des tests unitaires, nous nous concentrons initialement sur l’écriture du contrôleur et de la logique métier. Nous Évitez de toucher la base de données ou les vues. Nous n’allons pas modifier la base de données ou créer nos vues jusqu’à la fin de ce didacticiel. Nous commençons par ce qui peut être testé.

## <a name="creating-user-stories"></a>Créer des récits utilisateur

Lors du développement piloté par les tests, vous commencez toujours par écrire un test. Cela soulève immédiatement la question : Comment décider du test à écrire en premier ? Pour répondre à cette question, vous devez écrire un ensemble de [**récits utilisateur**](http://en.wikipedia.org/wiki/User_stories).

Un récit utilisateur est une description très brève (généralement une phrase) d’une spécification logicielle. Il doit s’agir d’une description non technique d’une exigence écrite du point de vue de l’utilisateur.

Ici, l’ensemble des récits utilisateur qui décrivent les fonctionnalités requises par la nouvelle fonctionnalité de groupe de contacts :

1. L’utilisateur peut afficher la liste des groupes de contacts.
2. L’utilisateur peut créer un groupe de contacts.
3. L’utilisateur peut supprimer un groupe de contacts existant.
4. L’utilisateur peut sélectionner un groupe de contacts lors de la création d’un contact.
5. L’utilisateur peut sélectionner un groupe de contacts lors de la modification d’un contact existant.
6. Une liste de groupes de contacts s’affiche dans la vue index.
7. Lorsqu’un utilisateur clique sur un groupe de contacts, une liste de contacts correspondants s’affiche.

Notez que cette liste de récits utilisateur est entièrement compréhensible par un client. Il n’y a aucune mention des détails de l’implémentation technique.

Pendant le processus de création de votre application, l’ensemble des récits utilisateur peut devenir plus affiné. Vous pouvez décomposer un récit utilisateur en plusieurs récits (exigences). Par exemple, vous pouvez décider que la création d’un nouveau groupe de contacts doit impliquer la validation. L’envoi d’un groupe de contacts sans nom doit retourner une erreur de validation.

Une fois que vous avez créé une liste de récits utilisateur, vous êtes prêt à écrire votre premier test unitaire. Nous allons commencer par créer un test unitaire pour afficher la liste des groupes de contacts.

## <a name="listing-contact-groups"></a>Liste des groupes de contacts

Notre premier récit utilisateur est qu’un utilisateur doit être en mesure d’afficher la liste des groupes de contacts. Nous devons exprimer cet article avec un test.

Créez un nouveau test unitaire en cliquant avec le bouton droit sur le dossier Controllers dans le projet ContactManager. tests, en sélectionnant **Ajouter, nouveau test**et en sélectionnant le modèle de **test unitaire** (voir la figure 1). Nommez le nouveau test unitaire GroupControllerTest.cs, puis cliquez sur le bouton **OK** .

[![de l’ajout du test unitaire GroupControllerTest](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**Figure 01**: ajout du test unitaire GroupControllerTest ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-cs/_static/image2.png))

Notre premier test unitaire est contenu dans la liste 1. Ce test vérifie que la méthode index () du contrôleur de groupe retourne un ensemble de groupes. Le test vérifie qu’une collection de groupes est retournée dans les données d’affichage.

**Liste 1-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

Quand vous tapez le code dans la liste 1 dans Visual Studio pour la première fois, vous obtenez un grand nombre de lignes ondulées rouges. Nous n’avons pas créé les classes GroupController ou Group.

À ce stade, nous pouvons même créer notre application afin de pouvoir exécuter notre premier test unitaire. C’est bon. Cela compte comme un test ayant échoué. Par conséquent, nous avons maintenant l’autorisation de commencer l’écriture du code de l’application. Nous devons écrire suffisamment de code pour exécuter notre test.

La classe de contrôleur de groupe dans Listing 2 contient le minimum de code requis pour passer le test unitaire. L’action index () renvoie une liste de groupes codée statiquement (la classe de groupe est définie dans la liste 3).

**Liste 2-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**Liste 3-Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

Après avoir ajouté les classes GroupController et Group à notre projet, notre premier test unitaire se termine correctement (voir la figure 2). Nous avons effectué le travail minimal requis pour réussir le test. Il est temps de célébrer.

[![réussie !](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**Figure 02**: réussite ! ([Cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-cs/_static/image4.png))

## <a name="creating-contact-groups"></a>Création de groupes de contacts

Nous pouvons maintenant passer au deuxième récit utilisateur. Nous devons être en mesure de créer des groupes de contacts. Nous devons exprimer cet objectif à l’aide d’un test.

Le test de la liste 4 vérifie que l’appel de la méthode Create () avec un nouveau groupe ajoute le groupe à la liste des groupes retournés par la méthode index (). En d’autres termes, si je crée un nouveau groupe, je dois pouvoir récupérer le nouveau groupe à partir de la liste des groupes renvoyés par la méthode index ().

**Liste 4-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

Le test de la liste 4 appelle la méthode Group Controller Create () avec un nouveau groupe de contacts. Ensuite, le test vérifie que l’appel de la méthode index du contrôleur de groupe () renvoie le nouveau groupe dans afficher les données.

Le contrôleur de groupe modifié dans la liste 5 contient le minimum de modifications nécessaires pour réussir le nouveau test.

**Liste 5-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Le contrôleur de groupe dans la liste 5 contient une nouvelle action créer (). Cette action ajoute un groupe à une collection de groupes. Notez que l’action index () a été modifiée pour retourner le contenu de la collection de groupes.

Une fois encore, nous avons effectué la quantité minimale de travail nécessaire pour réussir le test unitaire. Après avoir apporté ces modifications au contrôleur de groupe, tous nos tests unitaires réussissent.

## <a name="adding-validation"></a>Ajout de la validation

Cette exigence n’a pas été indiquée explicitement dans le récit utilisateur. Toutefois, il est raisonnable d’exiger qu’un groupe ait un nom. Dans le cas contraire, l’Organisation des contacts en groupes n’est pas très utile.

La liste 6 contient un nouveau test qui exprime cet objectif. Ce test vérifie que la tentative de création d’un groupe sans fournir de nom génère un message d’erreur de validation dans l’état du modèle.

**Liste 6-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

Pour satisfaire ce test, nous devons ajouter une propriété Name à notre classe Group (voir la liste 7). En outre, nous devons ajouter un peu de logique de validation à notre action de création () du contrôleur de groupe (voir le Listing 8).

**Liste 7-Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**Liste 8-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

Notez que l’action de création de contrôleur de groupe () contient maintenant la logique de validation et la logique de base de données. Actuellement, la base de données utilisée par le contrôleur de groupe ne se compose que d’une collection en mémoire.

## <a name="time-to-refactor"></a>Durée de refactorisation

La troisième étape de la couleur rouge/vert/refactorisation est la partie refactorisation. À ce stade, nous devons revenir à notre code et réfléchir à la façon dont nous pouvons Refactoriser notre application pour améliorer sa conception. L’étape de refactorisation est l’étape à laquelle nous réfléchissons à la meilleure façon d’implémenter les principes et les modèles de conception de logiciels.

Nous sommes libres de modifier notre code de quelque façon que ce soit pour améliorer la conception du code. Nous disposons d’un filet de sécurité pour les tests unitaires qui nous empêchent de rompre les fonctionnalités existantes.

Pour le moment, notre contrôleur de groupe est un bon du point de vue de la bonne conception de logiciels. Le contrôleur de groupe contient un code de validation et d’accès aux données toile. Pour éviter de violer le principe de responsabilité unique, nous devons séparer ces préoccupations en différentes classes.

Notre classe de contrôleur de groupe refactorisé est contenue dans la liste 9. Le contrôleur a été modifié pour utiliser la couche de service ContactManager. Il s’agit de la même couche de service que celle utilisée avec le contrôleur de contact.

La liste 10 contient les nouvelles méthodes ajoutées à la couche de service ContactManager pour prendre en charge la validation, la liste et la création de groupes. L’interface IContactManagerService a été mise à jour pour inclure les nouvelles méthodes.

La liste 11 contient une nouvelle classe FakeContactManagerRepository qui implémente l’interface IContactManagerRepository. Contrairement à la classe EntityContactManagerRepository qui implémente également l’interface IContactManagerRepository, notre nouvelle classe FakeContactManagerRepository ne communique pas avec la base de données. La classe FakeContactManagerRepository utilise une collection en mémoire comme un proxy pour la base de données. Nous allons utiliser cette classe dans nos tests unitaires en tant que couche de référentiel factice.

**Liste 9-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**Liste 10-Controllers\ContactManagerService.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**Liste 11-Controllers\FakeContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

La modification de l’interface IContactManagerRepository nécessite l’utilisation de pour implémenter les méthodes CreateGroup () et ListGroups () dans la classe EntityContactManagerRepository. Le laziest et le moyen le plus rapide d’y parvenir consiste à ajouter des méthodes stub qui ressemblent à ceci :   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]

Enfin, ces modifications apportées à la conception de notre application nous obligent à apporter des modifications à nos tests unitaires. Nous devons maintenant utiliser le FakeContactManagerRepository lors de l’exécution des tests unitaires. La classe GroupControllerTest mise à jour est contenue dans la liste 12.

**Liste 12-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

Une fois toutes ces modifications apportées, une fois encore, tous nos tests unitaires réussissent. Nous avons terminé la totalité du cycle du rouge/vert/refactor. Nous avons implémenté les deux premiers récits utilisateur. Nous avons maintenant pris en charge des tests unitaires pour les spécifications exprimées dans les récits utilisateur. L’implémentation du reste des récits utilisateur implique la répétition du même cycle de rouge/vert/refactor.

## <a name="modifying-our-database"></a>Modification de notre base de données

Malheureusement, bien que nous ayons satisfait à toutes les exigences exprimées par nos tests unitaires, notre travail n’est pas effectué. Nous devons toujours modifier notre base de données.

Nous devons créer une nouvelle table de base de données de groupe. Procédez comme suit :

1. Dans la fenêtre Explorateur de serveurs, cliquez avec le bouton droit sur le dossier tables et sélectionnez l’option de menu **Ajouter une nouvelle table**.
2. Entrez les deux colonnes décrites ci-dessous dans la Concepteur de tables.
3. Marquez la colonne ID comme une clé primaire et une colonne d’identité.
4. Enregistrez la nouvelle table avec les groupes de noms en cliquant sur l’icône de la disquette.

<a id="0.11_table01"></a>

| **Nom de la colonne** | **Type de données** | **Null autorisé** |
| --- | --- | --- |
| Id | int | False |
| Nom | nvarchar(50) | False |

Ensuite, nous devons supprimer toutes les données de la table contacts (dans le cas contraire, nous ne pourrons pas créer une relation entre les tables contacts et groupes). Procédez comme suit :

1. Cliquez avec le bouton droit sur la table contacts et sélectionnez l’option de menu **afficher les données**de la table.
2. Supprimez toutes les lignes.

Ensuite, nous devons définir une relation entre la table de base de données de groupes et la table de base de données de contacts existante. Procédez comme suit :

1. Double-cliquez sur la table contacts dans la fenêtre Explorateur de serveurs pour ouvrir le Concepteur de tables.
2. Ajoutez une nouvelle colonne d’entiers à la table contacts nommée GroupId.
3. Cliquez sur le bouton relation pour ouvrir la boîte de dialogue relations de clé étrangère (voir figure 3).
4. Cliquez sur le bouton Ajouter.
5. Cliquez sur le bouton de sélection qui s’affiche en regard du bouton spécification de la table et des colonnes.
6. Dans la boîte de dialogue tables et colonnes, sélectionnez groupes comme table de clé primaire et ID comme colonne de clé primaire. Sélectionnez contacts comme table de clé étrangère et GroupId comme colonne de clé étrangère (voir figure 4). Cliquez sur le bouton OK.
7. Sous **spécification d’insertion et de mise à jour**, sélectionnez la règle de valeur **cascade** pour **suppression**.
8. Cliquez sur le bouton Fermer pour fermer la boîte de dialogue relations de clé étrangère.
9. Cliquez sur le bouton enregistrer pour enregistrer les modifications apportées à la table contacts.

[![de la création d’une relation de table de base de données](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**Figure 03**: création d’une relation de table de base de données ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-cs/_static/image6.png))

[![spécification des relations entre les tables](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**Figure 04**: spécification des relations entre[les tables (cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-cs/_static/image8.png))

### <a name="updating-our-data-model"></a>Mise à jour de notre modèle de données

Ensuite, nous devons mettre à jour notre modèle de données pour représenter la nouvelle table de base de données. Procédez comme suit :

1. Double-cliquez sur le fichier ContactManagerModel. edmx dans le dossier Models pour ouvrir le Entity Designer.
2. Cliquez avec le bouton droit sur l’aire du concepteur et sélectionnez l’option de menu **mettre à jour le modèle à partir de la base de données**.
3. Dans l’Assistant Mise à jour, sélectionnez le tableau groupes, puis cliquez sur le bouton Terminer (voir figure 5).
4. Cliquez avec le bouton droit sur l’entité groupes, puis sélectionnez l’option de menu **Renommer**. Modifiez le nom de l’entité *groupes* en *groupe* (singulier).
5. Cliquez avec le bouton droit sur la propriété de navigation groupes qui apparaît en bas de l’entité contact. Remplacez le nom de la propriété *de navigation* Groups par *Group* (singulier).

[![de la mise à jour d’un modèle de Entity Framework à partir de la base de données](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**Figure 05**: mise à jour d’un modèle de Entity Framework à partir de la base de données ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-cs/_static/image10.png))

Une fois ces étapes effectuées, votre modèle de données représente à la fois les tables contacts et groupes. Le Entity Designer doit afficher les deux entités (voir figure 6).

[![Entity Designer affichage du groupe et du contact](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**Figure 06**: Entity designer affichage du groupe et du contact ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-cs/_static/image12.png))

### <a name="creating-our-repository-classes"></a>Création de nos classes de référentiel

Nous devons ensuite implémenter notre classe de référentiel. Au cours de cette itération, nous avons ajouté plusieurs nouvelles méthodes à l’interface IContactManagerRepository tout en écrivant du code pour satisfaire nos tests unitaires. La version finale de l’interface IContactManagerRepository est contenue dans la liste 14.

**Liste 14-Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

Nous n’avons pas implémenté les méthodes associées à l’utilisation des groupes de contacts. Actuellement, la classe EntityContactManagerRepository a des méthodes stub pour chacune des méthodes de groupe de contacts listées dans l’interface IContactManagerRepository. Par exemple, la méthode ListGroups () se présente comme suit :

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

Les méthodes stub nous permettaient de compiler notre application et de passer les tests unitaires. Toutefois, il est maintenant temps d’implémenter ces méthodes. La version finale de la classe EntityContactManagerRepository est contenue dans la liste 13.

**Liste 13-Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>Création des vues

Application ASP.NET MVC lorsque vous utilisez le moteur d’affichage ASP.NET par défaut. Par conséquent, vous ne créez pas d’affichages en réponse à un test unitaire particulier. Toutefois, étant donné qu’une application serait inutile sans affichages, nous pouvons effectuer cette itération sans créer ni modifier les vues contenues dans l’application du gestionnaire de contacts.

Nous devons créer les nouvelles vues suivantes pour la gestion des groupes de contacts (voir la figure 7) :

- Views\Group\Index.aspx-affiche la liste des groupes de contacts
- Views\Group\Delete.aspx : affiche le formulaire de confirmation pour la suppression d’un groupe de contacts

[![la vue index du groupe](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**Figure 07**: vue d’index de groupe ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-cs/_static/image14.png))

Nous devons modifier les vues existantes suivantes pour qu’elles incluent les groupes de contacts :

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Vous pouvez voir les affichages modifiés en examinant l’application Visual Studio qui accompagne ce didacticiel. Par exemple, la figure 8 illustre la vue d’index de contact.

[![la vue de l’index des contacts](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**Figure 08**: vue de l’index des contacts ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-cs/_static/image16.png))

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons ajouté de nouvelles fonctionnalités à notre application de gestion des contacts en suivant une méthodologie de conception d’application de développement piloté par des tests. Nous avons commencé par créer un ensemble de récits utilisateur. Nous avons créé un ensemble de tests unitaires qui correspond aux spécifications exprimées par les récits utilisateur. Enfin, nous avons écrit juste suffisamment de code pour répondre aux exigences exprimées par les tests unitaires.

Après avoir écrit suffisamment de code pour répondre aux exigences exprimées par les tests unitaires, nous avons mis à jour notre base de données et les vues. Nous avons ajouté une nouvelle table Groups à notre base de données et mis à jour notre modèle de données Entity Framework. Nous avons également créé et modifié un ensemble de vues.

Dans l’itération suivante (dernière itération), nous réécrivons notre application pour tirer parti d’Ajax. En tirant parti d’Ajax, nous allons améliorer la réactivité et les performances de l’application du gestionnaire de contacts.

> [!div class="step-by-step"]
> [Précédent](iteration-5-create-unit-tests-cs.md)
> [Suivant](iteration-7-add-ajax-functionality-cs.md)
