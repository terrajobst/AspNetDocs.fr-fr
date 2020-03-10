---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Activer les tests unitaires automatisés | Microsoft Docs
author: microsoft
description: L’étape 12 montre comment développer une suite de tests unitaires automatisés qui vérifient nos fonctionnalités NerdDinner et qui nous donneront la certitude d’apporter des modifications...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 09a7aa186605a6cce48ee94028425ded957c00d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541677"
---
# <a name="enable-automated-unit-testing"></a>Activer les tests unitaires automatisés

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 12 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 12 montre comment développer une suite de tests unitaires automatisés qui vérifient nos fonctionnalités NerdDinner, et qui nous donne la certitude de pouvoir apporter des modifications et des améliorations à l’application à l’avenir.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner étape 12 : tests unitaires

Nous allons développer une suite de tests d’unités automatisés qui vérifient nos fonctionnalités NerdDinner et nous donnerons la certitude de pouvoir apporter des modifications et des améliorations à l’application à l’avenir.

### <a name="why-unit-test"></a>Pourquoi un test unitaire ?

Sur le disque, vous avez un clignotement soudain sur une application sur laquelle vous travaillez. Vous réalisez qu’il existe une modification que vous pouvez implémenter pour améliorer considérablement l’application. Il peut s’agir d’une refactorisation qui nettoie le code, ajoute une nouvelle fonctionnalité ou corrige un bogue.

La question qui vous est posée lorsque vous arrivez sur votre ordinateur est : « quel est le niveau de sécurité pour améliorer cette amélioration ? » Que se passe-t-il si la modification a des effets secondaires ou brise un élément ? La modification peut être simple et prendre quelques minutes seulement pour l’implémentation, mais que se passe-t-il si des heures sont nécessaires pour tester manuellement tous les scénarios d’application ? Que se passe-t-il si vous oubliez de traiter un scénario et qu’une application interrompue passe en production ? Cette amélioration mérite-t-elle vraiment l’effort ?

Les tests unitaires automatisés peuvent fournir un filet de sécurité qui vous permet d’améliorer continuellement vos applications et d’éviter d’avoir peur du code sur lequel vous travaillez. Le fait de disposer de tests automatisés qui vérifient rapidement la fonctionnalité vous permet de coder en toute confiance, et vous permet d’apporter des améliorations que vous n’aurez sans doute pas à vous sentir à l’aise. Ils permettent également de créer des solutions qui sont plus faciles à gérer et qui ont une durée de vie plus longue, ce qui donne lieu à un retour sur investissement nettement plus élevé.

L’infrastructure MVC ASP.NET simplifie la fonctionnalité d’application de test unitaire. Il permet également un flux de travail de développement piloté par les tests (TDD) qui permet le développement basé sur des tests.

### <a name="nerddinnertests-project"></a>Projet NerdDinner. tests

Lorsque nous avons créé notre application NerdDinner au début de ce didacticiel, une boîte de dialogue s’affiche pour vous demander si nous voulions créer un projet de test unitaire à utiliser avec le projet d’application :

![](enable-automated-unit-testing/_static/image1.png)

Nous avons conservé la case d’option « Oui, créer un projet de test unitaire » sélectionnée, ce qui a entraîné l’ajout d’un projet « NerdDinner. tests » à notre solution :

![](enable-automated-unit-testing/_static/image2.png)

Le projet NerdDinner. tests fait référence à l’assembly de projet d’application NerdDinner et nous permet d’y ajouter facilement des tests automatisés qui vérifient la fonctionnalité de l’application.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Création de tests unitaires pour notre classe dîner Model

Nous allons ajouter des tests à notre projet NerdDinner. tests qui vérifient la classe dîner que nous avons créée lorsque nous avons créé notre couche de modèle.

Nous allons commencer par créer un nouveau dossier dans notre projet de test appelé « modèles », où nous allons placer nos tests liés au modèle. Nous allons ensuite cliquer avec le bouton droit sur le dossier et choisir la commande de menu **Ajouter-&gt;nouveau test** . La boîte de dialogue Ajouter un nouveau test s’affiche.

Nous allons choisir de créer un « test unitaire » et de le nommer « DinnerTest.cs » :

![](enable-automated-unit-testing/_static/image3.png)

Lorsque nous cliquons sur le bouton « OK », Visual Studio ajoute (et ouvre) un fichier DinnerTest.cs au projet :

![](enable-automated-unit-testing/_static/image4.png)

Le modèle de test unitaire Visual Studio par défaut comporte un ensemble de code de plaque de chaudière que je trouve un peu confus. Nous allons le nettoyer pour qu’il contienne simplement le code ci-dessous :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

L’attribut [TestClass] sur la classe DinnerTest ci-dessus l’identifie comme une classe qui contiendra des tests, ainsi qu’une initialisation de test facultative et du code de destruction. Nous pouvons définir des tests au sein de celui-ci en ajoutant des méthodes publiques qui possèdent un attribut [TestMethod].

Vous trouverez ci-dessous le premier des deux tests que nous allons ajouter qui font preuve de notre dîner. Le premier test vérifie que notre dîner n’est pas valide si un nouveau dîner est créé sans que toutes les propriétés soient correctement définies. Le deuxième test vérifie que notre dîner est valide quand toutes les propriétés du dîner sont définies avec des valeurs valides :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Vous remarquerez ci-dessus que nos noms de test sont très explicites (et quelque peu détaillés). Nous faisons cela en raison de la création de centaines ou de milliers de tests de petite taille, et nous souhaitons faciliter la détermination rapide de l’intention et du comportement de chacun d’eux (en particulier lorsque nous examinons une liste de défaillances dans un test Runner). Les noms de test doivent être nommés d’après les fonctionnalités qu’ils testent. Au-dessus de nous utilisons un modèle d’attribution de nom « substantif\_doit\_».

Nous structurons les tests à l’aide du modèle de test « AAA », qui signifie « arrange, Act, Assert » :

- Organiser : configurer l’unité en cours de test
- Act : exercice de l’unité en cours de test et capture des résultats
- Assertion : vérifier le comportement

Lorsque nous écrivons des tests, nous voulons éviter que les tests individuels soient trop nombreux. Au lieu de cela, chaque test doit vérifier un seul concept (ce qui facilite grandement l’identification de la cause des défaillances). Une bonne recommandation consiste à essayer et à n’avoir qu’une seule instruction Assert pour chaque test. Si vous avez plus d’une instruction Assert dans une méthode de test, assurez-vous qu’elles sont toutes utilisées pour tester le même concept. En cas de doute, effectuez un autre test.

### <a name="running-tests"></a>Exécution des tests

Visual Studio 2008 Professional (et versions ultérieures) comprend un testeur de test intégré qui peut être utilisé pour exécuter des projets de test unitaire Visual Studio dans l’IDE. Nous pouvons sélectionner la commande de menu **test-&gt;exécuter&gt;tous les tests dans la solution** (ou taper Ctrl R, A) pour exécuter tous nos tests unitaires. Ou bien, nous pouvons positionner notre curseur dans une classe de test ou une méthode de test spécifique et utiliser les **tests d’exécution de test&gt;&gt;dans la commande de menu contextuel actuelle** (ou en appuyant sur Ctrl R, t) pour exécuter un sous-ensemble des tests unitaires.

Nous allons positionner notre curseur dans la classe DinnerTest et taper « Ctrl R, T » pour exécuter les deux tests que nous venons de définir. Quand nous procédons ainsi, une fenêtre « Résultats des tests » s’affiche dans Visual Studio et nous voyons les résultats de notre série de tests répertoriée dans celle-ci :

![](enable-automated-unit-testing/_static/image5.png)

*Remarque : la fenêtre résultats des tests VS n’affiche pas la colonne nom de la classe par défaut. Vous pouvez l’ajouter en cliquant avec le bouton droit dans la fenêtre de Résultats des tests et en utilisant la commande de menu Ajouter/supprimer des colonnes.*

Nos deux tests ont pris uniquement une fraction de seconde pour s’exécuter, et comme vous pouvez le voir, ils sont tous deux réussis. Nous pouvons maintenant les enrichir en créant des tests supplémentaires qui vérifient les validations de règles spécifiques, ainsi que pour couvrir les deux méthodes d’assistance, IsUserHost () et IsUserRegistered (), que nous avons ajoutées à la classe dinner. Le fait que tous ces tests soient en place pour la classe dîner rend plus facile et plus sûr d’ajouter de nouvelles règles d’entreprise et de les valider à l’avenir. Nous pouvons ajouter notre nouvelle logique de règle au dîner, puis, en quelques secondes, vérifier qu’elle n’a pas enfreint les fonctionnalités logiques précédentes.

Notez que l’utilisation d’un nom de test descriptif permet de comprendre rapidement ce que chaque test vérifie. Je recommande d’utiliser la commande de menu **Outils-&gt;options** , en ouvrant l’écran outils de test-&gt;de la configuration de l’exécution de tests, puis en activant la case à cocher « double-clic sur un résultat de test unitaire échec ou non concluant affiche le point de défaillance dans le test ». Cela vous permettra de double-cliquer sur un échec dans la fenêtre résultats des tests et d’accéder immédiatement à l’échec de l’assertion.

### <a name="creating-dinnerscontroller-unit-tests"></a>Création de tests unitaires DinnersController

Créons maintenant des tests unitaires qui vérifient nos fonctionnalités DinnersController. Nous allons commencer par cliquer avec le bouton droit sur le dossier « Controllers » dans notre projet de test, puis choisir la commande de menu **Ajouter-&gt;nouveau test** . Nous allons créer un « test unitaire » et le nommer « DinnersControllerTest.cs ».

Nous allons créer deux méthodes de test qui vérifient la méthode d’action Details () sur DinnersController. La première vérifie qu’une vue est retournée lorsqu’un dîner existant est demandé. Le second vérifie qu’une vue « NotFound » est retournée quand un dîner inexistant est demandé :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Le code ci-dessus compile Clean. Toutefois, lorsque nous exécutons les tests, ils échouent tous les deux :

![](enable-automated-unit-testing/_static/image6.png)

Si nous examinons les messages d’erreur, nous voyons que la raison de l’échec des tests est que notre classe DinnersRepository n’a pas pu se connecter à une base de données. Notre application NerdDinner utilise une chaîne de connexion à un fichier de SQL Server Express local qui réside dans le répertoire de données \App\_du projet d’application NerdDinner. Comme notre projet NerdDinner. tests compile et s’exécute dans un autre répertoire, le projet d’application, l’emplacement de chemin d’accès relatif de notre chaîne de connexion est incorrect.

Nous *pouvons résoudre ce problème en* copiant le fichier de base de données SQL Express dans notre projet de test, puis en lui ajoutant une chaîne de connexion de test appropriée dans le fichier app. config de notre projet de test. Cela entraînerait le déblocage et l’exécution des tests ci-dessus.

Toutefois, le code de test unitaire utilisant une base de données réelle apporte un certain nombre de défis. Plus précisément :

- Cela ralentit considérablement le temps d’exécution des tests unitaires. Plus la durée d’exécution des tests est longue, moins il est probable que vous les exécutiez fréquemment. Dans l’idéal, vous souhaitez que vos tests unitaires puissent être exécutés en quelques secondes, et qu’il s’agit d’une opération aussi naturelle que la compilation du projet.
- Cela complique la logique d’installation et de nettoyage au sein des tests. Vous souhaitez que chaque test unitaire soit isolé et indépendant des autres (sans effets secondaires ou dépendances). Lorsque vous travaillez sur une base de données réelle, vous devez être attentif à l’État et le réinitialiser entre les tests.

Examinons un modèle de conception appelé « injection de dépendances » qui peut nous aider à contourner ces problèmes et à éviter d’avoir à utiliser une base de données réelle avec nos tests.

### <a name="dependency-injection"></a>Injection de dépendances

Pour le moment, DinnersController est étroitement « couplé » à la classe DinnerRepository. « Couplage » fait référence à une situation dans laquelle une classe s’appuie explicitement sur une autre classe afin de fonctionner :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Étant donné que la classe DinnerRepository requiert l’accès à une base de données, la dépendance étroitement couplée de la classe DinnersController sur le DinnerRepository finit par avoir une base de données pour que les méthodes d’action DinnersController soient testées.

Nous pouvons contourner ce cas en utilisant un modèle de conception appelé « injection de dépendances », qui est une approche dans laquelle les dépendances (telles que les classes de référentiel qui fournissent l’accès aux données) ne sont plus créées implicitement dans les classes qui les utilisent. Au lieu de cela, les dépendances peuvent être passées explicitement à la classe qui les utilise à l’aide d’arguments de constructeur. Si les dépendances sont définies à l’aide d’interfaces, nous avons ensuite la possibilité de passer des implémentations de dépendance « factices » pour les scénarios de test unitaire. Cela nous permet de créer des implémentations de dépendance spécifiques aux tests qui n’ont pas besoin d’accéder à une base de données.

Pour voir cela en action, nous allons implémenter l’injection de dépendances avec notre DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extraction d’une interface IDinnerRepository

Notre première étape consiste à créer une nouvelle interface IDinnerRepository qui encapsule le contrat de référentiel dont nos contrôleurs ont besoin pour récupérer et mettre à jour les dîners.

Nous pouvons définir ce contrat d’interface manuellement en cliquant avec le bouton droit sur le dossier \Models, puis en choisissant la commande de menu **Add-&gt;New Item** et en créant une nouvelle interface nommée IDinnerRepository.cs.

Vous pouvez également utiliser les outils de refactorisation intégrés Visual Studio Professional (et versions ultérieures) pour extraire et créer automatiquement une interface pour nous à partir de notre classe DinnerRepository existante. Pour extraire cette interface à l’aide de VS, placez simplement le curseur dans l’éditeur de texte sur la classe DinnerRepository, puis cliquez avec le bouton droit et choisissez la commande de menu **Refactoriser-&gt;extraire l’interface** :

![](enable-automated-unit-testing/_static/image7.png)

La boîte de dialogue « Extraire l’interface » s’ouvre et vous invite à entrer le nom de l’interface à créer. La valeur par défaut est IDinnerRepository et sélectionne automatiquement toutes les méthodes publiques de la classe DinnerRepository existante à ajouter à l’interface :

![](enable-automated-unit-testing/_static/image8.png)

Lorsque nous cliquons sur le bouton « OK », Visual Studio ajoute une nouvelle interface IDinnerRepository à notre application :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Et que notre classe DinnerRepository existante sera mise à jour pour qu’elle implémente l’interface :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Mise à jour de DinnersController pour prendre en charge l’injection de constructeur

Nous allons maintenant mettre à jour la classe DinnersController pour utiliser la nouvelle interface.

Actuellement, DinnersController est codé en dur, de sorte que son champ « dinnerRepository » est toujours une classe DinnerRepository :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Nous allons le modifier afin que le champ « dinnerRepository » soit de type IDinnerRepository au lieu de DinnerRepository. Nous allons ensuite ajouter deux constructeurs DinnersController publics. L’un des constructeurs permet de passer un IDinnerRepository comme argument. L’autre est un constructeur par défaut qui utilise notre implémentation DinnerRepository existante :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Étant donné que ASP.NET MVC crée par défaut des classes de contrôleur à l’aide des constructeurs par défaut, notre DinnersController au moment de l’exécution continuera à utiliser la classe DinnerRepository pour effectuer l’accès aux données.

Nous pouvons à présent mettre à jour nos tests unitaires pour transmettre une implémentation de Repository de dîner « factice » à l’aide du constructeur de paramètre. Ce dépôt de dîner « factice » ne nécessitera pas l’accès à une base de données réelle, et utilisera à la place des exemples de données en mémoire.

#### <a name="creating-the-fakedinnerrepository-class"></a>Création de la classe FakeDinnerRepository

Créons une classe FakeDinnerRepository.

Nous allons commencer par créer un répertoire « simulation » dans le projet NerdDinner. tests, puis y ajouter une nouvelle classe FakeDinnerRepository (cliquez avec le bouton droit sur le dossier et choisissez **Ajouter-&gt;nouvelle classe**) :

![](enable-automated-unit-testing/_static/image9.png)

Nous allons mettre à jour le code afin que la classe FakeDinnerRepository implémente l’interface IDinnerRepository. Nous pouvons ensuite cliquer dessus avec le bouton droit et choisir la commande de menu contextuel « implémenter l’interface IDinnerRepository » :

![](enable-automated-unit-testing/_static/image10.png)

Dans ce cas, Visual Studio ajoute automatiquement tous les membres de l’interface IDinnerRepository à notre classe FakeDinnerRepository avec les implémentations « stub out » par défaut :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Nous pouvons ensuite mettre à jour l’implémentation FakeDinnerRepository pour qu’elle fonctionne à partir d’une liste en mémoire&lt;dîner&gt; collection qui lui est transmise en tant qu’argument de constructeur :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Nous disposons maintenant d’une implémentation IDinnerRepository factice qui ne nécessite pas de base de données et qui peut à la place travailler sur une liste en mémoire d’objets dîner.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Utilisation de FakeDinnerRepository avec des tests unitaires

Revenons aux tests unitaires de DinnersController ayant échoué précédemment, car la base de données n’était pas disponible. Nous pouvons mettre à jour les méthodes de test pour utiliser un FakeDinnerRepository rempli avec des exemples de données de dîner en mémoire vers DinnersController à l’aide du code ci-dessous :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Et maintenant, lorsque nous exécutons ces tests, ceux-ci passent :

![](enable-automated-unit-testing/_static/image11.png)

Mieux encore, elles ne prennent qu’une fraction de seconde pour s’exécuter et ne nécessitent pas de logique d’installation/nettoyage compliquée. Nous pouvons maintenant effectuer un test unitaire de l’ensemble de notre code de méthode d’action DinnersController (notamment la liste, la pagination, les détails, la création, la mise à jour et la suppression) sans jamais avoir à vous connecter à une base de données réelle.

| **Rubrique latérale : infrastructures d’injection de dépendances** |
| --- |
| L’exécution manuelle de la dépendance (comme nous l’avons vu) fonctionne bien, mais devient plus difficile à gérer à mesure que le nombre de dépendances et de composants dans une application augmente. Il existe plusieurs infrastructures d’injection de dépendances pour .NET qui peuvent contribuer à offrir une plus grande flexibilité en matière de gestion des dépendances. Ces infrastructures, parfois appelées conteneurs d’inversion de contrôle (IoC, inversion of Control), fournissent des mécanismes qui permettent un niveau supplémentaire de prise en charge de la configuration pour spécifier et passer des dépendances aux objets au moment de l’exécution (le plus souvent à l’aide de l’injection de constructeur ). Voici quelques-unes des plus populaires d’injection de dépendances OSS/infrastructures IOC dans .NET : AutoFac, Ninject, Spring.NET, StructureMap et Windsor. ASP.NET MVC expose des API d’extensibilité qui permettent aux développeurs de participer à la résolution et à l’instanciation des contrôleurs, et qui permet aux développeurs d’effectuer un intégration propre à l’injection de dépendances/IoC dans ce processus. L’utilisation d’une infrastructure DI/IOC nous permettrait également de supprimer le constructeur par défaut de notre DinnersController, ce qui supprimerait complètement le couplage entre lui et le DinnerRepository. Nous n’utiliserons pas une infrastructure d’injection de dépendances/IOC avec notre application NerdDinner. Mais c’est un point que nous pourrions envisager à l’avenir si la base de code et les fonctionnalités NerdDinner augmentaient. |

### <a name="creating-edit-action-unit-tests"></a>Création de tests unitaires de modification d’action

Créons maintenant des tests unitaires qui vérifient la fonctionnalité de modification du DinnersController. Nous allons commencer par tester la version HTTP-obten de notre action de modification :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Nous allons créer un test qui vérifie qu’une vue avec un objet DinnerFormViewModel est restituée lorsqu’un dîner valide est demandé :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Lorsque nous exécutons le test, toutefois, nous constatons qu’il échoue, car une exception de référence null est levée lorsque la méthode Edit accède à la propriété User.Identity.Name pour effectuer la vérification dinner. IsHostedBy ().

L’objet utilisateur de la classe de base Controller encapsule des détails sur l’utilisateur connecté et est rempli par ASP.NET MVC lorsqu’il crée le contrôleur au moment de l’exécution. Étant donné que nous testons le DinnersController en dehors d’un environnement de serveur Web, l’objet utilisateur n’est pas défini (par conséquent, l’exception de référence null).

### <a name="mocking-the-useridentityname-property"></a>Simulation de la propriété User.Identity.Name

Les infrastructures factices facilitent les tests en nous permettant de créer dynamiquement des versions factices des objets dépendants qui prennent en charge nos tests. Par exemple, nous pouvons utiliser un Framework fictif dans notre test action de modification pour créer dynamiquement un objet utilisateur que notre DinnersController peut utiliser pour rechercher un nom d’utilisateur simulé. Cela permet d’éviter la levée d’une référence Null lorsque nous exécutons notre test.

De nombreuses infrastructures de simulation .NET peuvent être utilisées avec ASP.NET MVC (vous pouvez en afficher une liste ici : [http://www.mockframeworks.com/](http://www.mockframeworks.com/)). Pour tester notre application NerdDinner, nous allons utiliser une infrastructure fictive Open source appelée « MOQ », qui peut être téléchargée gratuitement à partir de [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq).

Une fois téléchargé, nous allons ajouter une référence dans le projet NerdDinner. tests à l’assembly MOQ. dll :

![](enable-automated-unit-testing/_static/image12.png)

Nous allons ensuite ajouter une méthode d’assistance « CreateDinnersControllerAs (username) » à notre classe de test qui prend un nom d’utilisateur en tant que paramètre, puis qui « factice » la propriété User.Identity.Name sur l’instance DinnersController :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Plus haut, nous utilisons MOQ pour créer un objet fictif qui simule un objet ControllerContext (ce que ASP.NET MVC transmet aux classes de contrôleur pour exposer des objets d’exécution tels que User, request, Response et session). Nous appelons la méthode « SetupGet » sur le simulacre pour indiquer que la propriété HttpContext.User.Identity.Name sur ControllerContext doit retourner la chaîne de nom d’utilisateur transmise à la méthode d’assistance.

Nous pouvons simuler n’importe quel nombre de propriétés et de méthodes ControllerContext. Pour illustrer cela, j’ai également ajouté un appel SetupGet () pour la propriété Request. IsAuthenticated (qui n’est pas réellement nécessaire pour les tests ci-dessous, mais qui permet d’illustrer comment vous pouvez simuler des propriétés de requête). Quand nous avons terminé, nous assignons une instance de l’ControllerContext factice à DinnersController la méthode d’assistance retourne.

Nous pouvons maintenant écrire des tests unitaires qui utilisent cette méthode d’assistance pour tester des scénarios d’édition impliquant différents utilisateurs :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Et maintenant, lorsque nous exécutons les tests, ils réussissent :

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Test des scénarios UpdateModel ()

Nous avons créé des tests qui couvrent la version HTTP-obten de l’action de modification. Créons maintenant des tests qui vérifient la version HTTP-poster de l’action modifier :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Le nouveau scénario de test intéressant pour nous pour la prise en charge de cette méthode d’action est son utilisation de la méthode d’assistance UpdateModel () sur la classe de base du contrôleur. Nous utilisons cette méthode d’assistance pour lier des valeurs de publication de formulaire à notre instance d’objet dîner.

Vous trouverez ci-dessous deux tests qui montrent comment nous pouvons fournir des valeurs de formulaire publiées pour la méthode d’assistance UpdateModel () à utiliser. Pour ce faire, nous allons créer et remplir un objet FormCollection, puis l’affecter à la propriété « ValueProvider » sur le contrôleur.

Le premier test vérifie que lors d’une sauvegarde réussie, le navigateur est redirigé vers l’action détails. Le deuxième test vérifie que lorsque l’entrée non valide est publiée, l’action réaffiche la vue de modification avec un message d’erreur.

[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Test de l’habillage

Nous avons abordé les principaux concepts impliqués dans les classes de contrôleur de test unitaire. Nous pouvons utiliser ces techniques pour créer facilement des centaines de tests simples qui vérifient le comportement de notre application.

Étant donné que nos tests de contrôleur et de modèle ne nécessitent pas de base de données réelle, ils sont extrêmement rapides et faciles à exécuter. Nous serons en mesure d’exécuter des centaines de tests automatisés en quelques secondes, et d’obtenir immédiatement des commentaires sur la nécessité d’une modification que nous avons apportée. Cela nous permet de nous aider à améliorer continuellement, Refactoriser et affiner notre application.

Nous avons abordé les tests comme la dernière rubrique de ce chapitre, mais ce n’est pas parce que le test est une opération à effectuer à la fin d’un processus de développement. En revanche, vous devez écrire des tests automatisés aussi tôt que possible dans votre processus de développement. Cela vous permet d’obtenir des commentaires immédiats au fur et à mesure que vous développez, vous aide à réfléchir aux scénarios de cas d’utilisation de votre application et vous guide dans la conception de votre application avec des couches propres et un couplage à l’esprit.

Un chapitre ultérieur du livre abordera le développement piloté par les tests (TDD) et comment l’utiliser avec ASP.NET MVC. TDD est une pratique de codage itérative dans laquelle vous écrivez d’abord les tests qui seront conformes à votre code résultant. Avec TDD, vous commencez chaque fonctionnalité en créant un test qui vérifie les fonctionnalités que vous vous apprêtez à implémenter. L’écriture d’abord du test unitaire permet de s’assurer que vous comprenez clairement la fonctionnalité et comment elle est supposée fonctionner. Une fois que le test est écrit (et que vous avez vérifié qu’il échoue), implémentez-vous ensuite les fonctionnalités réelles que le test vérifie. Étant donné que vous avez déjà passé du temps à réfléchir à la façon dont la fonctionnalité est supposée fonctionner, vous aurez une meilleure compréhension des exigences et de la meilleure façon de les implémenter. Une fois l’implémentation terminée, vous pouvez réexécuter le test et obtenir des commentaires immédiats pour savoir si la fonctionnalité fonctionne correctement. Nous aborderons le TDD en plus du chapitre 10.

### <a name="next-step"></a>Étape suivante

Certains commentaires finals de synthèse.

> [!div class="step-by-step"]
> [Précédent](use-ajax-to-implement-mapping-scenarios.md)
> [Suivant](nerddinner-wrap-up.md)
