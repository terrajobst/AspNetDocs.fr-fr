---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Nouveautés de la Entity Framework 4,0 | Microsoft Docs
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le Prise en main avec la série de didacticiels Entity Framework 4,0. ...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 29d2bd61e8361b130e7b9415dad4fe1d5b847945
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642673"
---
# <a name="whats-new-in-the-entity-framework-40"></a>Nouveautés d’Entity Framework 4.0

par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le [prise en main avec la](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série de didacticiels Entity Framework. Si vous n’avez pas terminé les didacticiels précédents, comme point de départ pour ce didacticiel, vous pouvez [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous auriez créée. Vous pouvez également [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créée par la série de didacticiels complète. Si vous avez des questions sur les didacticiels, vous pouvez les poster sur le [Forum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).

Dans le didacticiel précédent, vous avez vu des méthodes permettant d’optimiser les performances d’une application Web qui utilise le Entity Framework. Ce didacticiel passe en revue certaines des nouvelles fonctionnalités les plus importantes de la version 4 du Entity Framework, ainsi que des liens vers des ressources qui fournissent une présentation plus complète de toutes les nouvelles fonctionnalités. Les fonctionnalités mises en évidence dans ce didacticiel sont les suivantes :

- Associations de clé étrangère.
- Exécution de commandes SQL définies par l’utilisateur.
- Développement modèle-premier.
- Prise en charge d’POCO.

En outre, ce didacticiel introduira brièvement le *développement du code en premier*, une fonctionnalité qui est disponible dans la prochaine version du Entity Framework.

Pour démarrer le didacticiel, démarrez Visual Studio et ouvrez l’application Web Contoso University avec laquelle vous travailliez dans le didacticiel précédent.

## <a name="foreign-key-associations"></a>Associations de clé étrangère

La version 3,5 de la Entity Framework inclut les propriétés de navigation, mais elle n’inclut pas les propriétés de clé étrangère dans le modèle de données. Par exemple, les colonnes `CourseID` et `StudentID` de la table `StudentGrade` sont omises de l’entité `StudentGrade`.

[![image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

La raison de cette approche était que, strictement parlant, les clés étrangères sont des détails d’implémentation physique et n’appartiennent pas à un modèle de données conceptuel. Toutefois, dans la pratique, il est souvent plus facile d’utiliser des entités dans le code lorsque vous avez un accès direct aux clés étrangères.

Pour obtenir un exemple de la façon dont les clés étrangères dans le modèle de données peuvent simplifier votre code, réfléchissez à la façon dont vous auriez dû coder la page *DepartmentsAdd. aspx* sans les utiliser. Dans l’entité `Department`, la propriété `Administrator` est une clé étrangère qui correspond à `PersonID` dans l’entité `Person`. Pour établir l’association entre un nouveau service et son administrateur, il vous suffit de définir la valeur de la propriété `Administrator` dans le gestionnaire d’événements `ItemInserting` du contrôle DataBound :

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Sans clés étrangères dans le modèle de données, vous devez gérer l’événement `Inserting` du contrôle de source de données au lieu de l’événement `ItemInserting` du contrôle DataBound, afin d’obtenir une référence à l’entité elle-même avant l’ajout de l’entité au jeu d’entités. Lorsque vous avez cette référence, vous établissez l’Association à l’aide de code similaire à celui des exemples suivants :

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Comme vous pouvez le voir dans le billet de blog de l’équipe Entity Framework [sur les associations de clé étrangère](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), il existe d’autres cas où la différence de complexité du code est bien supérieure. Pour répondre aux besoins de ceux qui préfèrent vivre avec les détails de l’implémentation dans le modèle de données conceptuels à des fins de code plus simple, le Entity Framework vous donne la possibilité d’inclure des clés étrangères dans le modèle de données.

Dans Entity Framework terminologie, si vous incluez des clés étrangères dans le modèle de données que vous utilisez des *associations de clé étrangère*, et si vous excluez les clés étrangères, vous utilisez des *associations indépendantes*.

## <a name="executing-user-defined-sql-commands"></a>Exécution de commandes SQL définies par l’utilisateur

Dans les versions antérieures du Entity Framework, il n’existait aucun moyen simple de créer vos propres commandes SQL à la volée et de les exécuter. Soit la Entity Framework les commandes SQL générées de manière dynamique pour vous, soit vous deviez créer une procédure stockée et l’importer en tant que fonction. La version 4 ajoute des méthodes `ExecuteStoreQuery` et `ExecuteStoreCommand` la classe `ObjectContext` qui facilitent la transmission d’une requête directement à la base de données.

Supposons que les administrateurs de Contoso University souhaitent pouvoir effectuer des modifications en bloc dans la base de données sans avoir à passer par le processus de création d’une procédure stockée et d’importation dans le modèle de données. Leur première demande concerne une page qui leur permet de modifier le nombre de crédits pour tous les cours de la base de données. Sur la page Web, ils souhaitent pouvoir entrer un nombre à utiliser pour multiplier la valeur de chaque colonne de la `Course` `Credits`.

Créez une nouvelle page qui utilise la page maître *site. Master* et nommez-la *UpdateCredits. aspx*. Ajoutez ensuite le balisage suivant au contrôle `Content` nommé `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Ce balisage crée un contrôle `TextBox` dans lequel l’utilisateur peut entrer la valeur du multiplicateur, un `Button` contrôle à cliquer pour exécuter la commande, et un contrôle de `Label` pour indiquer le nombre de lignes affectées.

Ouvrez *UpdateCredits.aspx.cs*, puis ajoutez l’instruction `using` suivante et un gestionnaire pour l’événement `Click` du bouton :

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Ce code exécute la commande SQL `Update` à l’aide de la valeur de la zone de texte et utilise l’étiquette pour afficher le nombre de lignes affectées. Avant d’exécuter la page, exécutez la page *courses. aspx* pour obtenir une image « avant » de certaines données.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Exécutez *UpdateCredits. aspx*, entrez « 10 » comme multiplicateur, puis cliquez sur **exécuter**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Réexécutez la page *courses. aspx* pour afficher les données modifiées.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Si vous souhaitez redéfinir le nombre de crédits à leurs valeurs d’origine, dans *UpdateCredits.aspx.cs* , modifiez `Credits * {0}` pour `Credits / {0}` et réexécutez la page, en entrant 10 comme diviseur.)

Pour plus d’informations sur l’exécution de requêtes que vous définissez dans le code, consultez [Comment : exécuter directement des commandes sur la source de données](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Développement modèle-premier

Dans ces procédures pas à pas, vous avez créé la base de données avant de générer le modèle de données basé sur la structure de la base de données. Dans le Entity Framework 4, vous pouvez démarrer avec le modèle de données à la place et générer la base de données en fonction de la structure du modèle de données. Si vous créez une application pour laquelle la base de données n’existe pas encore, l’approche basée sur le modèle vous permet de créer des entités et des relations qui ont un sens conceptuel pour l’application, sans se soucier des détails d’implémentation physique . (Cela reste vrai uniquement au cours des phases initiales du développement, toutefois. Finalement, la base de données est créée et contient des données de production, et la recréation à partir du modèle n’est plus pratique. à ce stade, vous allez revenir à la première approche basée sur la base de données.)

Dans cette section du didacticiel, vous allez créer un modèle de données simple et générer la base de données à partir de celui-ci.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *dal* et sélectionnez **Ajouter un nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément** , sous **modèles installés** , sélectionnez **données** , puis sélectionnez le modèle **ADO.NET Entity Data Model** . Nommez le nouveau fichier *AlumniAssociationModel. edmx* , puis cliquez sur **Ajouter**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Cela lance l’Assistant Entity Data Model. Dans l’étape **choisir le contenu du modèle** , sélectionnez **modèle vide** , puis cliquez sur **Terminer**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

Le **Concepteur de Entity Data Model** s’ouvre avec une aire de conception vide. Faites glisser un élément d' **entité** de la **boîte à outils** vers l’aire de conception.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Remplacez le nom de l’entité `Entity1` par `Alumnus`, remplacez le nom de la propriété `Id` par `AlumnusId`, puis ajoutez une nouvelle propriété scalaire nommée `Name`. Pour ajouter de nouvelles propriétés, vous pouvez appuyer sur entrée après avoir modifié le nom de la `Id` colonne, ou cliquer avec le bouton droit sur l’entité et sélectionner **Ajouter une propriété scalaire**. Le type par défaut des nouvelles propriétés est `String`, ce qui est parfait pour cette démonstration simple, mais bien sûr, vous pouvez modifier des éléments tels que le type de données dans la fenêtre **Propriétés** .

Créez une autre entité de la même façon et nommez-la `Donation`. Remplacez la propriété `Id` par `DonationId` et ajoutez une propriété scalaire nommée `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Pour ajouter une association entre ces deux entités, cliquez avec le bouton droit sur l’entité `Alumnus`, sélectionnez **Ajouter**, puis sélectionnez **Association**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Les valeurs par défaut de la boîte de dialogue **Ajouter une association** sont celles que vous souhaitez (un-à-plusieurs, inclure les propriétés de navigation, inclure les clés étrangères). par conséquent, il vous suffit de cliquer sur **OK**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Le concepteur ajoute une ligne d’association et une propriété de clé étrangère.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Vous êtes maintenant prêt à créer la base de données. Cliquez avec le bouton droit sur l’aire de conception et sélectionnez **générer la base de données à partir du modèle**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

L’Assistant génération de la base de données s’ouvre. (Si vous voyez des avertissements indiquant que les entités ne sont pas mappées, vous pouvez les ignorer pour le moment.)

Dans l’étape **choisir votre connexion de données** , cliquez sur **nouvelle connexion**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

Dans la boîte de dialogue **Propriétés de connexion** , sélectionnez l’instance de SQL Server Express locale et nommez la base de données `AlumniAssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Cliquez sur **Oui** lorsque vous êtes invité à créer la base de données. Lorsque l’étape **choisir votre connexion de données** s’affiche à nouveau, cliquez sur **suivant**.

Dans l’étape **Résumé et paramètres** , cliquez sur **Terminer**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Un fichier *. SQL* avec les commandes DDL (Data Definition Language) est créé, mais les commandes n’ont pas encore été exécutées.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Utilisez un outil tel que **SQL Server Management Studio** pour exécuter le script et créer les tables, comme vous l’avez peut-être fait lorsque vous avez créé la base de données `School` pour [le premier didacticiel de la série de didacticiels prise en main](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Sauf si vous avez téléchargé la base de données.)

Vous pouvez maintenant utiliser le modèle de données `AlumniAssociation` dans vos pages Web de la même façon que vous utilisez le modèle de `School`. Pour essayer cela, ajoutez des données aux tables et créez une page Web qui affiche les données.

À l’aide de **Explorateur de serveurs**, ajoutez les lignes suivantes aux tables `Alumnus` et `Donation`.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Créez une nouvelle page Web nommée *étudiants. aspx* qui utilise la page maître *site. Master* . Ajoutez le balisage suivant au contrôle `Content` nommé `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Ce balisage crée des contrôles de `GridView` imbriqués, le code externe pour afficher les noms de étudiants et le nom interne pour afficher les dates et les montants des dons.

Ouvrez *Alumni.aspx.cs*. Ajoutez une instruction `using` pour la couche d’accès aux données et un gestionnaire pour l’événement `RowDataBound` du contrôle `GridView` externe :

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Ce code établit le contrôle de `GridView` interne à l’aide de la propriété de navigation `Donations` de l’entité `Alumnus` de la ligne actuelle.

Exécutez la page.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Remarque : cette page est incluse dans le projet téléchargeable, mais pour qu’elle fonctionne, vous devez créer la base de données dans votre instance de SQL Server Express locale ; la base de données n’est pas incluse en tant que fichier *. mdf* dans le dossier de données de l' *application\_* .)

Pour plus d’informations sur l’utilisation de la fonction Model-First de l’Entity Framework, voir [modèle-First dans le Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Prise en charge POCO

Lorsque vous utilisez une méthodologie de conception pilotée par domaine, vous concevez des classes de données qui représentent les données et le comportement qui s’appliquent au domaine d’entreprise. Ces classes doivent être indépendantes de toute technologie spécifique utilisée pour stocker (conserver) les données ; en d’autres termes, ils doivent être ignorés par la *persistance*. L’ignorance de la persistance peut également faciliter le test unitaire de la classe, car le projet de test unitaire peut utiliser la technologie de persistance la plus pratique pour les tests. Les versions antérieures de la Entity Framework offraient une prise en charge limitée de l’ignorance de la persistance, car les classes d’entité devaient hériter de la classe `EntityObject` et comprenaient donc un grand nombre de fonctionnalités spécifiques à Entity Framework.

Le Entity Framework 4 introduit la possibilité d’utiliser des classes d’entité qui n’héritent pas de la classe `EntityObject` et qui, par conséquent, ignorent la persistance. Dans le contexte de la Entity Framework, les classes comme celles-ci sont généralement appelées *objets CLR ordinaires* (POCO ou poco). Vous pouvez écrire des classes POCO manuellement, ou vous pouvez les générer automatiquement en fonction d’un modèle de données existant à l’aide des modèles de boîte à outils de transformation de texte (T4) fournis par le Entity Framework.

Pour plus d’informations sur l’utilisation des POCO dans le Entity Framework, consultez les ressources suivantes :

- [Utilisation des entités POCO](https://msdn.microsoft.com/library/dd456853.aspx). Il s’agit d’un document MSDN qui est une vue d’ensemble des POCO, avec des liens vers d’autres documents qui contiennent des informations plus détaillées.
- [Procédure pas à pas : modèle POCO pour le Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) Il s’agit d’un billet de blog de l’équipe de développement Entity Framework, avec des liens vers d’autres billets de blog sur les POCO.

## <a name="code-first-development"></a>Développement Code First

La prise en charge de POCO dans le Entity Framework 4 requiert toujours que vous créez un modèle de données et que vous liez vos classes d’entité au modèle de données. La prochaine version du Entity Framework inclura une fonctionnalité appelée *développement Code-First*. Cette fonctionnalité vous permet d’utiliser le Entity Framework avec vos propres classes POCO sans avoir à utiliser le concepteur de modèle de données ou un fichier XML de modèle de données. (Par conséquent, cette option est également appelée *code uniquement*; *Code First* et *code-only* font tous deux référence à la même fonctionnalité de Entity Framework.)

Pour plus d’informations sur l’utilisation de l’approche code First pour le développement, consultez les ressources suivantes :

- [Développement Code-First avec Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Il s’agit d’un billet de blog de Scott Guthrie présentant le développement de code First.
- [Blog de l’équipe de développement Entity Framework-publie les CodeOnly marqués](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Blog de l’équipe de développement Entity Framework-publie les balises Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Didacticiel sur le magasin de musique MVC-partie 4 : modèles et accès aux données](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Prise en main avec MVC 3-partie 4 : Entity Framework le développement Code-First](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

En outre, un nouveau didacticiel MVC code First qui crée une application similaire à l’application Contoso University est projeté pour être publié au printemps de 2011 à [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Plus d'informations

Cela termine la présentation des nouveautés de la Entity Framework et se poursuit avec la série de didacticiels Entity Framework. Pour plus d’informations sur les nouvelles fonctionnalités du Entity Framework 4 qui ne sont pas abordées ici, consultez les ressources suivantes :

- [Nouveautés de ADO.net](https://msdn.microsoft.com/library/ex6y04yf.aspx) Rubrique MSDN sur les nouvelles fonctionnalités de la version 4 du Entity Framework.
- [Annonce de la publication de Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) Le billet de blog de l’équipe de développement Entity Framework sur les nouvelles fonctionnalités de la version 4.

> [!div class="step-by-step"]
> [Précédent](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
