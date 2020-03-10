---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Créer un modèle avec des validations de règles d’entreprise | Microsoft Docs
author: microsoft
description: L’étape 3 montre comment créer un modèle que nous pouvons utiliser pour interroger et mettre à jour la base de données de notre application NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 6ebf1b71c089229ba9139ff7dc788b8978724046
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541663"
---
# <a name="build-a-model-with-business-rule-validations"></a>Créer un modèle avec des validations de règles d’entreprise

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 3 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 3 montre comment créer un modèle que nous pouvons utiliser pour interroger et mettre à jour la base de données de notre application NerdDinner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner étape 3 : génération du modèle

Dans un Framework Model-View-Controller, le terme « modèle » fait référence aux objets qui représentent les données de l’application, ainsi qu’à la logique de domaine correspondante qui intègre la validation et les règles d’entreprise. Le modèle est en grande partie le « cœur » d’une application basée sur MVC, et comme nous le verrons plus loin, le comportement de celui-ci est fondamental.

L’infrastructure MVC ASP.NET prend en charge l’utilisation de n’importe quelle technologie d’accès aux données, et les développeurs peuvent choisir parmi une large gamme d’options de données .NET riches pour implémenter leurs modèles, notamment : LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM ou simplement des ADO bruts. Les DataReaders ou les jeux de données NET.

Pour notre application NerdDinner, nous allons utiliser LINQ to SQL pour créer un modèle simple qui correspond plutôt à la conception de la base de données et ajoute une logique de validation personnalisée et des règles d’entreprise. Nous implémenterons ensuite une classe de référentiel qui permet de soustraire l’implémentation de la persistance des données du reste de l’application, et nous permet d’effectuer facilement des tests unitaires.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL est un ORM (Object Relational Mapper) fourni avec .NET 3,5.

LINQ to SQL offre un moyen simple de mapper des tables de base de données à des classes .NET que vous pouvez coder. Pour notre application NerdDinner, nous allons l’utiliser pour mapper les dîners et les tables RSVP au sein de notre base de données aux classes de dîner et RSVP. Les colonnes des tables dîners et RSVP correspondent aux propriétés des classes dîner et RSVP. Chaque objet Dinner et RSVP représente une ligne distincte dans les tables dîners ou RSVP de la base de données.

LINQ to SQL nous permet d’éviter d’avoir à construire manuellement des instructions SQL pour récupérer et mettre à jour les objets dîner et RSVP avec les données de la base de données. Au lieu de cela, nous allons définir les classes dîner et RSVP, comment elles sont mappées vers/depuis la base de données, et les relations entre elles. LINQ to SQL prend alors soin de générer la logique d’exécution SQL appropriée à utiliser au moment de l’exécution lorsque nous les interagissez et les utilisez.

Nous pouvons utiliser la prise en charge du langage LINQ C# dans VB et écrire des requêtes expressifs qui récupèrent les objets dîner et RSVP à partir de la base de données. Cela réduit la quantité de code de données que nous devons écrire et nous permet de créer des applications vraiment propres.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Ajout de classes de LINQ to SQL à notre projet

Nous allons commencer par cliquer avec le bouton droit sur le dossier « Models » dans notre projet, puis sélectionner la commande de menu **Add-&gt;New Item** :

![](build-a-model-with-business-rule-validations/_static/image1.png)

La boîte de dialogue Ajouter un nouvel élément s’affiche. Nous allons filtrer par la catégorie « données » et sélectionner le modèle « classes de LINQ to SQL » au sein de celle-ci :

![](build-a-model-with-business-rule-validations/_static/image2.png)

Nous allons nommer l’élément « NerdDinner » et cliquer sur le bouton « Ajouter ». Visual Studio ajoute un fichier NerdDinner. dbml sous notre répertoire \Models, puis ouvre le LINQ to SQL concepteur objet relationnel :

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Création de classes de modèle de données avec LINQ to SQL

LINQ to SQL nous permet de créer rapidement des classes de modèle de données à partir d’un schéma de base de données existant. Pour ce faire, nous allons ouvrir la base de données NerdDinner dans le Explorateur de serveurs, puis sélectionner les tables que vous souhaitez modéliser :

![](build-a-model-with-business-rule-validations/_static/image4.png)

Nous pouvons ensuite faire glisser les tables sur l’aire du concepteur de LINQ to SQL. Lorsque nous procédons ainsi LINQ to SQL créera automatiquement des classes dîner et RSVP à l’aide du schéma des tables (avec les propriétés de classe qui mappent aux colonnes de la table de base de données) :

![](build-a-model-with-business-rule-validations/_static/image5.png)

Par défaut, le concepteur de LINQ to SQL « pluralise » automatiquement les noms de table et de colonne lorsqu’il crée des classes basées sur un schéma de base de données. Par exemple : la table « dîners » dans l’exemple ci-dessus a entraîné une classe « dîner ». Ce nom de classe permet de faire en sorte que nos modèles soient cohérents avec les conventions de nommage .NET, et je trouve généralement que le concepteur a résolu cette pratique (surtout lors de l’ajout de nombreuses tables). Toutefois, si vous n’aimez pas le nom d’une classe ou d’une propriété générée par le concepteur, vous pouvez toujours le remplacer et le remplacer par le nom de votre choix. Pour ce faire, vous pouvez modifier le nom de l’entité/de la propriété en ligne dans le concepteur ou en le modifiant par le biais de la grille des propriétés.

Par défaut, le concepteur de LINQ to SQL inspecte également les relations clé primaire/clé étrangère des tables et, en fonction de celles-ci, crée automatiquement les « associations de relations » par défaut entre les différentes classes de modèle créées. Par exemple, lorsque nous avons fait glisser les tables dîners et RSVP sur le concepteur de LINQ to SQL, une association de relation un-à-plusieurs entre les deux a été déduite en fonction du fait que la table RSVP avait une clé étrangère vers la table des dîners (cela est indiqué par la flèche dans la concepteur) :

![](build-a-model-with-business-rule-validations/_static/image6.png)

L’Association ci-dessus amènera LINQ to SQL à ajouter une propriété « dîner » fortement typée à la classe RSVP que les développeurs peuvent utiliser pour accéder au dîner associé à un RSVP donné. La classe Dinner aura également une propriété de collection « RSVPs » qui permet aux développeurs de récupérer et de mettre à jour les objets RSVP associés à un dîner particulier.

Vous trouverez ci-dessous un exemple d’IntelliSense dans Visual Studio quand nous créons un nouvel objet RSVP et que vous l’ajoutez à la collection RSVPs d’un dîner. Notez comment LINQ to SQL ajouté automatiquement une collection « RSVP » sur l’objet dîner :

![](build-a-model-with-business-rule-validations/_static/image7.png)

En ajoutant l’objet RSVP à la collection RSVP du dîner, nous indiquons LINQ to SQL d’associer une relation de clé étrangère entre le dîner et la ligne RSVP dans notre base de données :

![](build-a-model-with-business-rule-validations/_static/image8.png)

Si vous n’aimez pas comment le concepteur a modélisé ou nommé une association de tables, vous pouvez le remplacer. Cliquez simplement sur la flèche Association dans le concepteur et accédez à ses propriétés par le biais de la grille des propriétés pour la renommer, la supprimer ou la modifier. Pour notre application NerdDinner, cependant, les règles d’association par défaut fonctionnent bien pour les classes de modèle de données que nous créons et nous pouvons simplement utiliser le comportement par défaut.

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext, classe

Visual Studio crée automatiquement des classes .NET qui représentent les modèles et les relations de base de données définis à l’aide du concepteur de LINQ to SQL. Une classe LINQ to SQL DataContext est également générée pour chaque fichier LINQ to SQL Designer ajouté à la solution. Comme nous avons nommé notre élément de classe LINQ to SQL « NerdDinner », la classe DataContext créée sera appelée « NerdDinnerDataContext ». Cette classe NerdDinnerDataContext est le principal moyen d’interaction avec la base de données.

Notre classe NerdDinnerDataContext expose deux propriétés : « dîners » et « RSVPs », qui représentent les deux tables que nous avons modélisées dans la base de données. Nous pouvons utiliser C# pour écrire des requêtes LINQ sur ces propriétés pour interroger et récupérer des objets dîner et RSVP à partir de la base de données.

Le code suivant montre comment instancier un objet NerdDinnerDataContext et exécuter une requête LINQ sur ce dernier pour obtenir une séquence de dîners qui se produisent à l’avenir. Visual Studio fournit une fonctionnalité IntelliSense complète lors de l’écriture de la requête LINQ, et les objets qui lui sont retournés sont fortement typés et prennent également en charge IntelliSense :

![](build-a-model-with-business-rule-validations/_static/image9.png)

En plus de nous permettre de demander des objets de dîner et RSVP, un NerdDinnerDataContext suit également automatiquement toutes les modifications que nous apportons par la suite au dîner et aux objets RSVP que nous récupérons par la suite. Nous pouvons utiliser cette fonctionnalité pour enregistrer facilement les modifications dans la base de données, sans avoir à écrire de code de mise à jour SQL explicite.

Par exemple, le code ci-dessous montre comment utiliser une requête LINQ pour récupérer un objet dîner unique à partir de la base de données, mettre à jour deux des propriétés de dîner, puis enregistrer les modifications dans la base de données :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

L’objet NerdDinnerDataContext dans le code ci-dessus a automatiquement suivi les modifications de propriété apportées à l’objet dîner que nous avons récupéré à partir de celui-ci. Quand nous avons appelé la méthode « SubmitChanges () », elle exécute une instruction SQL « UPDATE » appropriée dans la base de données pour conserver les valeurs mises à jour.

### <a name="creating-a-dinnerrepository-class"></a>Création d’une classe DinnerRepository

Pour les petites applications, il est parfois parfait que les contrôleurs fonctionnent directement par rapport à une classe LINQ to SQL DataContext et incorporent des requêtes LINQ dans les contrôleurs. Cependant, à mesure que les applications deviennent plus volumineuses, cette approche devient lourde à gérer et à tester. Cela peut également entraîner la duplication des mêmes requêtes LINQ à plusieurs emplacements.

Une approche qui peut rendre les applications plus faciles à gérer et à tester consiste à utiliser un modèle de « référentiel ». Une classe de référentiel permet d’encapsuler la logique de persistance et d’interrogation des données, et soustrait les détails d’implémentation de la persistance des données de l’application. En plus de rendre le code de l’application plus propre, l’utilisation d’un modèle de référentiel peut faciliter la modification des implémentations de stockage de données à l’avenir, et peut faciliter le test unitaire d’une application sans nécessiter de base de données réelle.

Pour notre application NerdDinner, nous allons définir une classe DinnerRepository avec la signature ci-dessous :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Remarque : plus loin dans ce chapitre, nous allons extraire une interface IDinnerRepository de cette classe et activer l’injection de dépendances avec elle sur nos contrôleurs. Pour commencer, cependant, nous allons Démarrer simple et travailler directement avec la classe DinnerRepository.*

Pour implémenter cette classe, cliquez avec le bouton droit sur le dossier « Models », puis sélectionnez la commande de menu **Add-&gt;New Item** . Dans la boîte de dialogue Ajouter un nouvel élément, sélectionnez le modèle « classe » et nommez le fichier « DinnerRepository.cs » :

![](build-a-model-with-business-rule-validations/_static/image10.png)

Nous pouvons ensuite implémenter notre classe DinnerRepository à l’aide du code ci-dessous :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Récupération, mise à jour, insertion et suppression à l’aide de la classe DinnerRepository

Maintenant que nous avons créé notre classe DinnerRepository, examinons quelques exemples de code qui illustrent les tâches courantes que nous pouvons y faire :

#### <a name="querying-examples"></a>Exemples d’interrogation

Le code ci-dessous récupère un dîner unique à l’aide de la valeur DinnerID :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Le code ci-dessous récupère tous les dîners et boucles à venir sur ceux-ci :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Exemples d’insertion et de mise à jour

Le code ci-dessous illustre l’ajout de deux nouveaux dîners. Les ajouts et les modifications apportées au référentiel ne sont pas validées dans la base de données tant que la méthode « Save () » n’est pas appelée dessus. LINQ to SQL encapsule automatiquement toutes les modifications dans une transaction de base de données. par conséquent, soit toutes les modifications se produisent, soit aucune ne le font lorsque notre référentiel enregistre :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Le code ci-dessous récupère un objet dîner existant et modifie deux propriétés sur celui-ci. Les modifications sont validées dans la base de données lors de l’appel de la méthode « Save () » sur notre référentiel :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Le code ci-dessous récupère un dîner, puis lui ajoute une réponse. Pour ce faire, il utilise la collection RSVP sur l’objet dîner que LINQ to SQL créé pour nous (car il existe une relation clé primaire/clé étrangère entre les deux dans la base de données). Cette modification est rendue persistante dans la base de données sous la forme d’une nouvelle ligne de table RSVP lorsque la méthode « Save () » est appelée sur le référentiel :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Exemple de suppression

Le code ci-dessous récupère un objet dîner existant, puis le marque comme étant supprimé. Lorsque la méthode « Save () » est appelée sur le référentiel, elle valide la suppression dans la base de données :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Intégration de la logique de validation et de règle d’entreprise aux classes de modèle

L’intégration de la logique de validation et de règle d’entreprise est un élément essentiel de toute application qui fonctionne avec les données.

#### <a name="schema-validation"></a>Validation de schéma

Lorsque les classes de modèle sont définies à l’aide de LINQ to SQL Designer, les types de données des propriétés dans les classes de modèle de données correspondent aux types de données de la table de base de données. Par exemple : si la colonne « EventDate » de la table dinners est une valeur « DateTime », la classe de modèle de données créée par LINQ to SQL sera de type « DateTime » (qui est un type de données .NET intégré). Cela signifie que vous obtiendrez des erreurs de compilation si vous tentez d’assigner un entier ou un booléen à partir du code, et une erreur est déclenchée automatiquement si vous tentez de convertir implicitement un type de chaîne non valide en au moment de l’exécution.

LINQ to SQL gère également automatiquement l’échappement des valeurs SQL lors de l’utilisation de chaînes, ce qui vous permet de vous protéger contre les attaques par injection SQL lors de son utilisation.

#### <a name="validation-and-business-rule-logic"></a>Logique de validation et de règle d’entreprise

La validation de schéma est utile comme première étape, mais elle est rarement suffisante. La plupart des scénarios réels nécessitent la possibilité de spécifier une logique de validation plus riche qui peut couvrir plusieurs propriétés, exécuter du code et souvent avoir conscience de l’état d’un modèle (par exemple : est-il en cours de création/updated/Deleted ou dans un état spécifique au domaine comme « Archivé »). Il existe un large éventail de modèles et d’infrastructures différents qui peuvent être utilisés pour définir et appliquer des règles de validation aux classes de modèle, et il existe plusieurs infrastructures .NET qui peuvent être utilisées pour faciliter ce type d’utilisation. Vous pouvez utiliser quasiment n’importe lequel d’entre eux dans les applications ASP.NET MVC.

Dans le cadre de notre application NerdDinner, nous allons utiliser un modèle relativement simple et direct où nous exposons une propriété IsValid et une méthode GetRuleViolations () sur notre objet de modèle dîner. La propriété IsValid retourne la valeur true ou false selon que les règles de validation et d’entreprise sont toutes valides. La méthode GetRuleViolations () renverra une liste d’erreurs de règle.

Nous allons implémenter IsValid et GetRuleViolations () pour notre modèle de dîner en ajoutant une « classe partielle » à notre projet. Les classes partielles peuvent être utilisées pour ajouter des méthodes/propriétés/événements aux classes gérées par un concepteur Visual Studio (par exemple, la classe dîner générée par le LINQ to SQL Designer) et aider à éviter que l’outil ne passe à notre code. Nous pouvons ajouter une nouvelle classe partielle à notre projet en cliquant avec le bouton droit sur le dossier \Models, puis en sélectionnant la commande de menu « Ajouter un nouvel élément ». Nous pouvons ensuite choisir le modèle « classe » dans la boîte de dialogue « Ajouter un nouvel élément » et le nommer Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

En cliquant sur le bouton « Ajouter », vous ajoutez un fichier Dinner.cs à votre projet et vous l’ouvrez dans l’IDE. Nous pouvons ensuite implémenter une infrastructure de mise en application des règles/validations de base à l’aide du code ci-dessous :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Quelques remarques sur le code ci-dessus :

- La classe dîner est précédée d’un mot clé « Partial », ce qui signifie que le code qu’elle contient sera combiné avec la classe générée/gérée par le concepteur de LINQ to SQL et compilée dans une classe unique.
- La classe RuleViolation est une classe d’assistance que nous ajouterons au projet, qui nous permet de fournir des détails supplémentaires sur une violation de règle.
- La méthode dinner. GetRuleViolations () permet d’évaluer nos règles de validation et d’entreprise (nous les implémenterons bientôt). Elle retourne ensuite une séquence d’objets RuleViolation qui fournissent des détails supplémentaires sur les erreurs de règle.
- La propriété dinner. IsValid fournit une propriété d’assistance pratique qui indique si l’objet dîner a des RuleViolations actifs. Il peut être vérifié de manière proactive par un développeur à l’aide de l’objet dîner à tout moment (et ne lève pas d’exception).
- La méthode partielle dîner. OnValidate () est un hook fourni par LINQ to SQL qui nous permet d’être averti chaque fois que l’objet Dinner est sur le point d’être conservé dans la base de données. Notre implémentation OnValidate () ci-dessus garantit que le dîner n’a pas de RuleViolations avant son enregistrement. S’il est dans un État non valide, une exception est levée, ce qui entraîne l’abandon par LINQ to SQL de la transaction.

Cette approche fournit une infrastructure simple dans laquelle nous pouvons intégrer la validation et les règles d’entreprise. Pour l’instant, nous allons ajouter les règles ci-dessous à notre méthode GetRuleViolations () :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Nous utilisons la fonctionnalité « yield return » de C# pour retourner une séquence de tous les RuleViolations. Les six premières vérifications de règle ci-dessus appliquent simplement que les propriétés de chaîne sur notre dîner ne peuvent pas être null ou vides. La dernière règle est un peu plus intéressante et appelle une méthode d’assistance PhoneValidator. IsValidNumber () que nous pouvons ajouter à notre projet pour vérifier que le format de nombre ContactPhone correspond au pays du dîner.

Nous pouvons utiliser. Prise en charge des expressions régulières du .net pour implémenter cette prise en charge de la validation du téléphone. Vous trouverez ci-dessous une implémentation de PhoneValidator simple que nous pouvons ajouter à notre projet qui nous permet d’ajouter des vérifications de modèles d’expressions régulières spécifiques à un pays :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Gestion des violations de la logique métier et de la validation

Maintenant que nous avons ajouté le code de validation et de règle d’entreprise ci-dessus, chaque fois que nous essayons de créer ou de mettre à jour un dîner, nos règles de logique de validation seront évaluées et appliquées.

Les développeurs peuvent écrire du code comme ci-dessous pour déterminer de manière proactive si un objet dîner est valide et récupérer une liste de toutes les violations qu’il contient sans lever d’exceptions :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Si nous tentons d’enregistrer un dîner dans un État non valide, une exception est levée quand nous appelons la méthode Save () sur DinnerRepository. Cela se produit parce que LINQ to SQL appelle automatiquement notre méthode de dinner. OnValidate () de façon partielle avant d’enregistrer les modifications du dîner, et nous avons ajouté du code à dinner. OnValidate () pour lever une exception si une violation de règle existe dans le dîner. Nous pouvons intercepter cette exception et récupérer de manière réactive une liste des violations à résoudre :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Étant donné que nos règles de validation et d’entreprise sont implémentées dans notre couche de modèle, et non dans la couche d’interface utilisateur, elles sont appliquées et utilisées dans tous les scénarios de notre application. Nous pouvons modifier ou ajouter des règles d’entreprise ultérieurement et faire en sorte que tout le code qui fonctionne avec nos objets dîner les honore.

La possibilité de modifier les règles d’entreprise à un seul emplacement, sans que ces modifications ne soient réparties dans l’application et la logique de l’interface utilisateur, est un signe d’une application bien écrite et un avantage qu’une infrastructure MVC aide à encourager.

### <a name="next-step"></a>Étape suivante

Nous disposons désormais d’un modèle que nous pouvons utiliser pour interroger et mettre à jour notre base de données.

Nous allons maintenant ajouter des contrôleurs et des vues au projet, que nous pouvons utiliser pour créer une expérience d’interface utilisateur HTML autour de lui.

> [!div class="step-by-step"]
> [Précédent](create-a-database.md)
> [Suivant](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
