---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Atelier pratique : sites Web Azure gérables : gestion des modifications et de la mise à l’échelle | Microsoft Docs'
author: rick-anderson
description: Dans ce laboratoire, Découvrez comment Microsoft Azure facilite la création et le déploiement de sites Web en production.
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: c88bae40a8aa092037c0b359ee391acaf161cf10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624270"
---
# <a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Atelier pratique : sites Web Azure gérables : gestion des modifications et de la mise à l’échelle

par l' [équipe Web camps](https://twitter.com/webcamps)

[Télécharger le kit de formation Web camps](https://aka.ms/webcamps-training-kit)

> Microsoft Azure facilite la création et le déploiement de sites Web en production. Mais vous n’avez pas terminé quand votre application est en ligne, vous venez de commencer ! Vous devez gérer les besoins en modification, les mises à jour de base de données, la mise à l’échelle, etc. Heureusement, Azure App Service vous a abordé, avec de nombreuses fonctionnalités pour vous aider à assurer le bon fonctionnement de vos sites.
>
> Azure offre des options de développement, de déploiement et de mise à l’échelle sécurisées et flexibles pour toutes les applications Web de toute taille. Servez-vous des outils existants pour créer et déployer des applications sans vous soucier de la gestion de l'infrastructure.
>
> Approvisionnez vous-même une application Web de production en quelques minutes en déployant facilement le contenu créé à l’aide de votre outil de développement préféré. Vous pouvez déployer un site existant directement à partir du contrôle de code source avec la prise en charge de **git**, **GitHub**, **bitbucket**, **tfs**et même **Dropbox**. Déployez directement à partir de votre IDE favori ou de scripts à l’aide de **PowerShell** dans Windows ou des outils **CLI** exécutés sur n’importe quel système d’exploitation. Une fois le déploiement terminé, gardez vos sites à jour en permanence grâce à la prise en charge du déploiement continu.
>
> Azure fournit des solutions de stockage, de sauvegarde et de récupération sur le Cloud évolutives et durables pour toutes les données, grand ou petit. Lors du déploiement d’applications dans un environnement de production, les services de stockage tels que les tables, les objets BLOB et les bases de données SQL, vous aident à mettre à l’échelle votre application dans le Cloud.
>
> Avec les bases de données SQL, il est important de maintenir à jour votre base de données productive lors du déploiement de nouvelles versions de votre application. Grâce à **migrations Entity Framework code First**, le développement et le déploiement de votre modèle de données ont été simplifiés pour mettre à jour vos environnements en quelques minutes. Ce laboratoire pratique vous montrera les différentes rubriques que vous pouvez rencontrer lors du déploiement de votre application Web dans des environnements de production dans Microsoft Azure.
>
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible sur [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).
>
> Pour plus d’informations sur cette rubrique, consultez [création d’applications Cloud réalistes avec Azure e-Book](building-real-world-cloud-apps-with-windows-azure/introduction.md).

<a id="Overview"></a>
## <a name="overview"></a>Présentation

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Activer Entity Framework migrations avec un modèle existant
- Mettre à jour le modèle objet et la base de données en conséquence à l’aide de Entity Framework migrations
- Déployer sur Azure App Service à l’aide de git
- Restauration d’un déploiement précédent à l’aide du portail de gestion Azure
- Utiliser le stockage Azure pour mettre à l’échelle une application Web
- Configurer la mise à l’échelle automatique pour une application Web à l’aide d’Azure Portail de gestion
- Créer et configurer un projet de test de charge dans Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Les éléments suivants sont requis pour effectuer ce laboratoire pratique :

- [Visual Studio Express 2013 pour Web](https://www.microsoft.com/visualstudio/) ou version ultérieure
- [Kit de développement logiciel (SDK) Azure pour .NET 2,2](https://www.microsoft.com/windowsazure/sdk/)
- [Système de contrôle de version GIT](http://git-scm.com/download)
- Un abonnement Microsoft Azure

    - S’inscrire à une [version d’évaluation gratuite](https://aka.ms/watk-freetrial)
    - Si vous êtes un Visual Studio Professional, Test Professional, Premium ou Ultimate avec MSDN ou Plateformes MSDN abonné, activez votre [avantage MSDN](https://aka.ms/watk-msdn) dès maintenant pour commencer à développer et à tester sur Azure.
    - Les membres de [BizSpark](https://aka.ms/watk-bizspark) bénéficient automatiquement de l’avantage Azure par le biais de leur abonnement Visual Studio Ultimate avec MSDN
    - Les membres du programme [Microsoft Partner Network](https://aka.ms/watk-mpn) Cloud Essentials reçoivent gratuitement des crédits Azure mensuels

<a id="Setup"></a>
### <a name="setup"></a>Installation

Pour exécuter les exercices dans ce laboratoire pratique, vous devez d’abord configurer votre environnement.

1. Ouvrez l’Explorateur Windows et accédez au dossier **source** du laboratoire.
2. Cliquez avec le bouton droit sur **Setup. cmd** et sélectionnez **exécuter en tant qu’administrateur** pour lancer le processus d’installation qui va configurer votre environnement et installer les extraits de code Visual Studio pour ce Lab.
3. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, confirmez l’action pour continuer.

> [!NOTE]
> Vérifiez que vous avez vérifié toutes les dépendances de ce Lab avant d’exécuter le programme d’installation.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Utilisation des extraits de code

Tout au long du document Lab, vous serez invité à insérer des blocs de code. Pour plus de commodité, la majeure partie de ce code est fournie sous la forme d’extraits de Visual Studio Code, auxquels vous pouvez accéder à partir de Visual Studio 2013 pour éviter d’avoir à l’ajouter manuellement.

> [!NOTE]
> Chaque exercice est accompagné d’une solution de démarrage située dans le dossier **Begin** de l’exercice, qui vous permet de suivre chaque exercice indépendamment des autres. N’oubliez pas que les extraits de code ajoutés au cours d’un exercice sont absents de ces solutions de démarrage et peuvent ne pas fonctionner tant que vous n’avez pas terminé l’exercice. À l’intérieur du code source d’un exercice, vous trouverez également un dossier de **fin** contenant une solution Visual Studio avec le code qui résulte de l’exécution des étapes de l’exercice correspondant. Vous pouvez utiliser ces solutions comme conseils si vous avez besoin d’aide supplémentaire au fur et à mesure que vous travaillez dans ce laboratoire pratique.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique comprend les exercices suivants :

1. [Utilisation des migrations de Entity Framework](#Exercise1)
2. [Déploiement d’une application Web dans un environnement intermédiaire](#Exercise2)
3. [Exécution d’une restauration de déploiement en production](#Exercise3)
4. [Mise à l’échelle à l’aide du stockage Azure](#Exercise4)
5. [Utilisation de la mise à l’échelle automatique pour Web Apps](#Exercise5) (facultatif pour Visual Studio 2013 Édition Ultimate)

Durée estimée pour effectuer ce laboratoire : **75 minutes**

> [!NOTE]
> Lorsque vous démarrez Visual Studio pour la première fois, vous devez sélectionner l’une des collections de paramètres prédéfinies. Chaque collection prédéfinie est conçue pour correspondre à un style de développement particulier et détermine les dispositions de fenêtres, le comportement de l’éditeur, les extraits de code IntelliSense et les options de boîte de dialogue. Les procédures de ce laboratoire décrivent les actions nécessaires pour accomplir une tâche donnée dans Visual Studio lors de l’utilisation de la collection de **paramètres de développement généraux** . Si vous choisissez une autre collection de paramètres pour votre environnement de développement, il peut y avoir des différences entre les étapes que vous devez prendre en compte.

<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Exercice 1 : utilisation de Entity Framework migrations

Lorsque vous développez une application, votre modèle de données peut changer au fil du temps. Ces modifications peuvent affecter le modèle existant dans votre base de données (si vous créez une nouvelle version) et il est important de maintenir votre base de données à jour pour éviter les erreurs.

Pour simplifier le suivi de ces modifications dans votre modèle, **migrations Entity Framework code First** détecter automatiquement les modifications en comparant votre modèle au schéma de la base de données et génère du code spécifique pour mettre à jour votre base de données et créer de nouvelles *versions* de votre base de données.

Cet exercice vous montre comment activer des **migrations** pour votre application et comment vous pouvez facilement détecter et générer des modifications pour mettre à jour vos bases de données.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Tâche 1 : activation des migrations

Dans cette tâche, vous allez passer en revue les étapes d’activation de la **migrations Entity Framework code First** à la base de données des **quiz des passionnés** , à modifier le modèle et à comprendre comment ces modifications sont reflétées dans la base de données.

1. Ouvrez Visual Studio et ouvrez le fichier solution **GeekQuiz. sln** à partir de **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.
2. Générez la solution afin de télécharger et d’installer les dépendances du package **NuGet** . Pour ce faire, cliquez avec le bouton droit sur la solution, puis cliquez sur **générer la solution** ou appuyez sur **Ctrl + Maj + B**.
3. Dans le menu **Outils** de Visual Studio, sélectionnez **Gestionnaire de package NuGet**, puis cliquez sur **console du gestionnaire de package**.
4. Dans la **console du gestionnaire de package**, entrez la commande suivante, puis appuyez sur **entrée**. Une migration initiale basée sur le modèle existant sera créée.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Activation des migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Activation des migrations")

    *Activation des migrations*

    > [!NOTE]
    > Cette commande ajoute un dossier **migrations** au projet de quiz de l’initiateur qui contient un fichier appelé **Configuration.cs**. La classe de **configuration** vous permet de configurer le comportement des migrations pour votre contexte.
5. Lorsque la migration est activée, vous devez mettre à jour la classe de **configuration** pour remplir la base de données avec les données initiales requises par le **quiz** de l’inverseur. Sous **migrations**, remplacez le fichier **Configuration.cs** par celui qui se trouve dans le dossier **Source\Assets** de ce Lab.

    > [!NOTE]
    > Étant donné que les **migrations** appellent la méthode **Seed** avec chaque mise à jour de base de données, vous devez vous assurer que les enregistrements ne sont pas dupliqués dans la base de données. La méthode **AddOrUpdate** permet d’éviter les doublons de données.
6. Pour ajouter une migration initiale, entrez la commande suivante, puis appuyez sur **entrée**.

    > [!NOTE]
    > Assurez-vous qu’il n’existe aucune base de données nommée &quot;GeekQuizProd&quot; dans votre instance de base de données locale.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Ajout d’une migration de schéma de base](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Ajout d’une migration de schéma de base")

    *Ajout d’une migration de schéma de base*

    > [!NOTE]
    > **Add-migration génère une** structure de la prochaine migration en fonction des modifications que vous avez apportées à votre modèle depuis la création de la dernière migration. Dans ce cas, comme c’est la première migration du projet, il ajoute les scripts pour créer toutes les tables définies dans la classe **TriviaContext** .
7. Exécutez la migration pour mettre à jour la base de données en exécutant la commande suivante. Pour cette commande, vous allez spécifier l’indicateur **Verbose** pour afficher les instructions SQL appliquées à la base de données cible.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Création de la base de données initiale](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Création de la base de données initiale")

    *Création de la base de données initiale*

    > [!NOTE]
    > **Update-Database** appliquera toutes les migrations en attente à la base de données. Dans ce cas, la base de données est créée à l’aide de la chaîne de connexion définie dans votre fichier Web. config.
8. Accédez au menu **affichage** et ouvrez **Explorateur d’objets SQL Server**.

    ![Ouvrir dans Explorateur d’objets SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Ouvrir dans Explorateur d’objets SQL Server")

    *Ouvrir dans Explorateur d’objets SQL Server*
9. Dans la fenêtre **Explorateur d’objets SQL Server** , connectez-vous à votre instance de base de données locale en cliquant avec le bouton droit sur le nœud **SQL Server** et en sélectionnant l’option **Ajouter un SQL Server...** .

    ![Ajout d’une instance de SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Ajout d’une instance de SQL Server")

    *Ajout d’une instance de SQL Server à Explorateur d’objets SQL Server*
10. Définissez le **nom du serveur** sur *(\v11.0)* et laissez **authentification Windows** comme mode d’authentification. Cliquez sur **Connect** pour continuer.

    ![Connexion à la base de données locale](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connexion à la base de données locale")

    *Connexion à la base de données locale*
11. Ouvrez la base de données **GeekQuizProd** et développez le nœud **tables** . Comme vous pouvez le voir, la commande **Update-Database** a généré toutes les tables définies dans la classe **TriviaContext** . Localisez le **dbo. Table TriviaQuestions** et ouvrez le nœud Columns. Dans la tâche suivante, vous allez ajouter une nouvelle colonne à cette table et mettre à jour la base de données à l’aide des **migrations**.

    ![Colonnes de questions de questions](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Colonnes de questions de questions")

    *Colonnes de questions de questions*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Tâche 2 : mise à jour du schéma de base de données à l’aide des migrations

Dans cette tâche, vous allez utiliser **migrations Entity Framework code First** pour détecter une modification dans votre modèle et générer le code nécessaire pour mettre à jour la base de données. Vous allez mettre à jour l’entité **TriviaQuestions** en lui ajoutant une nouvelle propriété. Ensuite, vous allez exécuter des commandes pour créer une nouvelle migration afin d’inclure la nouvelle colonne dans la table.

1. Dans **Explorateur de solutions**, double-cliquez sur le fichier **TriviaQuestion.cs** situé dans le dossier **modèles** .
2. Ajoutez une nouvelle propriété nommée **Hint**, comme illustré dans l’extrait de code suivant.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. Dans la **console du gestionnaire de package**, entrez la commande suivante, puis appuyez sur **entrée**. Une nouvelle migration sera créée pour refléter la modification apportée à notre modèle.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Ajouter une migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Ajouter une migration")

    *Ajouter une migration*

    > [!NOTE]
    > Un fichier de migration est composé de deux méthodes, **vers le haut et vers le** **haut** .
    >
    > - La méthode **up** sera utilisée pour spécifier les modifications que la version actuelle de notre application doit appliquer à la base de données.
    > - La valeur **vers** le haut permet d’inverser les modifications que nous avons ajoutées à la méthode **up** .
    >
    > Lorsque la migration de base de données met à jour la base de données, elle exécute toutes les migrations dans l’ordre d’horodatage, et uniquement celles qui n’ont pas été utilisées depuis la dernière mise à jour (la table \_MigrationHistory assure le suivi des migrations qui ont été appliquées). La méthode **up** de toutes les migrations sera appelée et effectuera les modifications que nous avons spécifiées dans la base de données. Si nous décidons de revenir à une migration précédente, la méthode **UpDown** sera appelée pour rétablir les modifications dans l’ordre inverse.
4. Dans la **console du gestionnaire de package**, entrez la commande suivante, puis appuyez sur **entrée**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. La sortie de la commande **Update-Database** a généré une instruction **ALTER TABLE** SQL pour ajouter une nouvelle colonne à la table **TriviaQuestions** , comme indiqué dans l’image ci-dessous.

    ![Instruction SQL d’ajout de colonne générée](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Instruction SQL d’ajout de colonne générée")

    *Instruction SQL d’ajout de colonne générée*
6. Dans **Explorateur d’objets SQL Server**, actualisez le **dbo. Table TriviaQuestions** et vérifiez que la nouvelle colonne **Hint** s’affiche.

    ![Présentation de la nouvelle colonne d’indicateur](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Présentation de la nouvelle colonne d’indicateur")

    *Présentation de la nouvelle colonne d’indicateur*
7. De retour dans l’éditeur **TriviaQuestion.cs** , ajoutez une contrainte **StringLength** à la propriété *Hint* , comme indiqué dans l’extrait de code suivant.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. Dans la **console du gestionnaire de package**, entrez la commande suivante, puis appuyez sur **entrée**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. Dans la **console du gestionnaire de package**, entrez la commande suivante, puis appuyez sur **entrée**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. La sortie de la commande **Update-Database** a généré une instruction **ALTER TABLE** SQL pour mettre à jour le type de colonne *Hint* de la table **TriviaQuestions** , comme indiqué dans l’image ci-dessous.

    ![Instruction SQL ALTER COLUMN générée](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Instruction SQL ALTER COLUMN générée")

    *Instruction SQL ALTER COLUMN générée*
11. Dans **Explorateur d’objets SQL Server**, actualisez le **dbo. Table TriviaQuestions** et vérifiez que le type de colonne **Hint** est **nvarchar (150)** .

    ![Présentation de la nouvelle contrainte](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Présentation de la nouvelle contrainte")

    *Présentation de la nouvelle contrainte*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Exercice 2 : déploiement d’une application Web dans un environnement intermédiaire

**Web Apps dans Azure App service** vous permet d’effectuer une publication intermédiaire. La publication intermédiaire crée un emplacement de site intermédiaire pour chaque site de production par défaut et vous permet d’échanger ces emplacements sans temps d’arrêt. Cela est très utile pour valider les modifications avant de les publier dans le public, pour intégrer de façon incrémentielle le contenu du site et pour restaurer si les modifications ne fonctionnent pas comme prévu.

Dans cet exercice, vous allez déployer l’application **quiz du test** sur l’environnement intermédiaire de votre application Web à l’aide du contrôle de code source git. Pour ce faire, vous allez créer l’application Web et approvisionner les composants requis dans le portail de gestion, configurer un référentiel **git** et transmettre le code source de l’application de votre ordinateur local vers l’emplacement intermédiaire. Vous allez également mettre à jour votre base de données de production avec le **migrations code First** que vous avez créé dans l’exercice précédent. Vous allez ensuite exécuter l’application dans cet environnement de test pour vérifier son fonctionnement. Une fois que vous êtes satisfait de votre travail en fonction de vos attentes, vous allez promouvoir l’application en production.

> [!NOTE]
> Pour activer la publication intermédiaire, l’application Web doit être en **mode standard**. Notez que des frais supplémentaires seront facturés si vous passez votre application Web en mode standard. Pour plus d’informations sur la tarification, consultez [app service tarification](https://azure.microsoft.com/pricing/details/app-service/).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Tâche 1 : création d’une application Web dans Azure App Service

Au cours de cette tâche, vous allez créer une application Web dans **Azure App service** à partir du portail de gestion. Vous allez également configurer un **SQL Database** pour conserver les données d’application et configurer un référentiel Git local pour le contrôle de code source.

1. Accédez au [portail de gestion Azure](https://manage.windowsazure.com) et connectez-vous à l’aide de la compte Microsoft associée à votre abonnement.

    ![Connectez-vous au portail de gestion Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Connectez-vous au portail de gestion Azure*
2. Cliquez sur **nouveau** dans la barre de commandes en bas de la page.

    ![Création d’une nouvelle application Web](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Création d’une nouvelle application Web")

    *Création d’une nouvelle application Web*
3. Cliquez sur **calcul**, **site Web** , puis sur **création personnalisée**.

    ![Création d’une application Web à l’aide de la création personnalisée](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Création d’une application Web à l’aide de la création personnalisée")

    *Création d’une application Web à l’aide de la création personnalisée*
4. Dans la boîte de dialogue **nouveau site Web-création personnalisée** , indiquez une **URL** disponible (par exemple, *quiz-quiz*), sélectionnez un emplacement dans la liste déroulante **région** , puis sélectionnez **créer une base de données SQL** dans la liste déroulante **base de données** . Enfin, activez la case à cocher **publier à partir du contrôle de code source** , puis cliquez sur **suivant**.

    ![Personnalisation de la nouvelle application Web](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Personnalisation de la nouvelle application Web*
5. Spécifiez les informations suivantes pour les paramètres de base de données :

   - Dans la zone de texte **nom** , entrez un nom de base de données (par exemple, *geekquiz\_DB*).
   - Dans la liste **déroulante** serveur, sélectionnez **nouveau serveur de base de données SQL**. Vous pouvez également sélectionner un serveur existant.
   - Dans les zones **nom d’utilisateur** et **mot de passe** de la base de données, entrez le nom d’utilisateur et le mot de passe de l’administrateur pour le serveur SQL Database. Si vous sélectionnez un serveur que vous avez déjà créé, vous êtes invité à entrer le mot de passe.

     ![Spécification des paramètres de base de données](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Spécification des paramètres de base de données*
6. Cliquez sur **Suivant** pour continuer.
7. Sélectionnez **référentiel Git local** pour le contrôle de code source à utiliser, puis cliquez sur **suivant**.

    > [!NOTE]
    > Vous pouvez être invité à entrer les informations d’identification de déploiement (un nom d’utilisateur et un mot de passe).

    ![Création du dépôt git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Création du dépôt git*
8. Attendez que la création de l’application Web soit créée.

    > [!NOTE]
    > Par défaut, Azure fournit des domaines sur *azurewebsites.net* , mais vous donne également la possibilité de définir des domaines personnalisés à l’aide du portail de gestion Azure. Toutefois, vous ne pouvez gérer des domaines personnalisés que si vous utilisez certains modes de Azure App Service.
    >
    > Azure App Service est disponible dans les éditions gratuit, partagé, de base, standard et Premium. En mode gratuit et partagé, toutes les applications Web s’exécutent dans un environnement multi-locataire et ont des quotas pour l’utilisation du processeur, de la mémoire et du réseau. Le nombre maximal d’applications gratuites peut varier en fonction de votre plan. En mode standard, vous choisissez les applications qui s’exécutent sur des machines virtuelles dédiées qui correspondent aux ressources de calcul Azure standard. Vous pouvez trouver la configuration du mode d’application Web dans le menu mettre à l' **échelle** de votre application Web.
    >
    > ![Modes de Azure App Service](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Modes de Azure App Service")
    >
    > Si vous utilisez le mode **partagé** ou **standard** , vous serez en mesure de gérer des domaines personnalisés pour votre application Web en accédant au menu **configurer** de votre application et en cliquant sur **gérer les domaines** sous noms de *domaine*.
    >
    > ![Gérer les domaines](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Gérer les domaines")
    >
    > ![Gérer les domaines personnalisés](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Gérer les domaines personnalisés")
9. Une fois l’application Web créée, cliquez sur le lien sous la colonne **URL** pour vérifier que la nouvelle application Web est en cours d’exécution.

    ![Accès à la nouvelle application Web](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Accès à la nouvelle application Web*

    ![application Web en cours d’exécution](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *application Web en cours d’exécution*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Tâche 2 : création de la SQL Database de production

Dans cette tâche, vous allez utiliser la **migrations Entity Framework code First** pour créer la base de données ciblant l’instance **Azure SQL Database** que vous avez créée lors de la tâche précédente.

1. Dans la Portail de gestion, accédez à l’application Web que vous avez créée dans la tâche précédente et accédez à son **tableau de bord**.
2. Dans la page **tableau de bord** , cliquez sur Afficher les chaînes de **connexion** dans la section **Aperçu rapide** .

    ![Afficher les chaînes de connexion](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Afficher les chaînes de connexion")

    *Afficher les chaînes de connexion*
3. Copiez la valeur de la **chaîne de connexion** et fermez la boîte de dialogue.

    ![Chaîne de connexion dans Azure Portail de gestion](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Chaîne de connexion dans Azure Portail de gestion")

    *Chaîne de connexion dans Azure Portail de gestion*
4. Cliquez sur **bases de données SQL** pour afficher la liste des bases de données SQL dans Azure

    ![Menu SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "Menu SQL Database")

    *Menu SQL Database*
5. Localisez la base de données que vous avez créée lors de la tâche précédente, puis cliquez sur le serveur.

    ![Serveur de SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "Serveur SQL Database")

    *Serveur de SQL Database*
6. Sur la page **démarrage rapide** du serveur, cliquez sur **configurer**.

    ![Menu configurer](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Menu configurer")

    *Menu configurer*
7. Dans la section **adresses IP autorisées** , cliquez sur **Ajouter au lien adresses IP autorisées** pour permettre à votre adresse IP de se connecter au serveur de SQL Database.

    ![Adresses IP autorisées](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Adresses IP autorisées")

    *Adresses IP autorisées*
8. Cliquez sur **Enregistrer** en bas de la page pour achever cette étape.
9. Revenez à Visual Studio.
10. Dans la **console du gestionnaire de package**, exécutez la commande suivante en remplaçant l’espace réservé *[votre-Connection-String]* par la chaîne de connexion que vous avez copiée à partir d’Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Mettre à jour la base de données ciblant Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Mettre à jour la base de données ciblant Windows Azure SQL Database")

    *Mettre à jour la cible de la base de données Azure SQL Database*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Tâche 3 : déploiement du quiz de l’un des passionnés dans l’environnement intermédiaire à l’aide de git

Dans cette tâche, vous allez activer la publication intermédiaire dans votre application Web. Ensuite, vous allez utiliser Git pour publier l’application de quiz de l’ordinateur local directement à partir de votre ordinateur local vers l’environnement intermédiaire de votre application Web.

1. Revenez au portail et cliquez sur le nom de l’application Web dans la colonne **nom** pour afficher les pages de gestion.

    ![Ouverture des pages de gestion des applications Web](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Ouverture des pages de gestion des applications Web*
2. Accédez à la page mettre à l' **échelle** . Dans la section **général** , sélectionnez **standard** pour la configuration, puis cliquez sur **Enregistrer** dans la barre de commandes.

    > [!NOTE]
    > Pour exécuter toutes les applications Web dans la région et l’abonnement en cours en mode **standard** , laissez la case à cocher **Sélectionner tout** activée dans la configuration **choisir des sites** . Dans le cas contraire, désactivez la case à cocher **Sélectionner tout** .

    ![Mise à niveau de l’application Web vers le mode standard](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Mise à niveau de l’application Web vers le mode standard")

    *Mise à niveau de l’application Web vers le mode standard*
3. Cliquez sur **Oui** pour confirmer les modifications.

    ![Confirmation du changement en mode standard](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Poursuite de la modification du mode d’application Web")

    *Confirmation du changement en mode standard*
4. Accédez à la page **tableau de bord** , puis cliquez sur Activer la **publication intermédiaire** sous la section **Aperçu rapide** .

    ![Activation de la publication intermédiaire](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Activation de la publication intermédiaire")

    *Activation de la publication intermédiaire*
5. Cliquez sur **Oui** pour activer la publication intermédiaire.

    ![Confirmation de la publication intermédiaire](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Cliquer sur Oui pour activer la publication intermédiaire")

    *Confirmation de la publication intermédiaire*
6. Dans la liste des applications Web, développez la marque à gauche du nom de votre application Web pour afficher l’emplacement du site intermédiaire. Il porte le nom de votre application Web suivi de ***(intermédiaire)***. Cliquez sur le site intermédiaire pour accéder à la page de gestion.

    ![Navigation vers l’application Web intermédiaire](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigation vers l’application Web intermédiaire")

    *Navigation vers l’application intermédiaire*
7. Notez que la page de gestion ressemble à toute autre page de gestion de l’application Web. Accédez à la page **déploiements** et copiez la valeur de l' **URL git** . Vous l’utiliserez plus loin dans cet exercice.

    ![Copie de la valeur de l’URL git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Copie de la valeur de l’URL git*
8. Ouvrez une nouvelle console **git bash** et exécutez les commandes suivantes. Mettez à jour l’espace réservé *[votre-application-path]* avec le chemin d’accès à la solution **GeekQuiz** , situé dans le dossier **Source\Ex1-DeployingWebSiteToStaging\Begin** de ce Lab.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Initialisation git et validation initiale](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Initialisation git et validation initiale*
9. Exécutez la commande suivante pour pousser votre application Web vers le référentiel **git** distant. Remplacez l’espace réservé par l’URL que vous avez obtenue à partir du portail de gestion. Vous serez invité à entrer votre mot de passe de déploiement.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Envoi vers Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Envoi vers Azure*

    > [!NOTE]
    > Lorsque vous déployez du contenu sur l’hôte FTP ou le référentiel GIT d’une application Web, vous devez vous authentifier à l’aide des **informations d’identification de déploiement** que vous avez créées à partir des pages de gestion **démarrage rapide** ou **tableau de bord** de l’application Web. Si vous ne connaissez pas vos informations d’identification de déploiement, vous pouvez facilement les réinitialiser à l’aide du portail de gestion. Ouvrez la page **tableau de bord** de l’application Web, puis cliquez sur le lien **réinitialiser vos informations d’identification de déploiement** . Fournissez un nouveau mot de passe et cliquez sur **OK**. Les informations d’identification de déploiement sont valides pour une utilisation avec toutes les applications Web associées à votre abonnement.
10. Pour vérifier que l’application Web a été correctement envoyée à Azure, revenez au portail de gestion et cliquez sur **sites Web**.
11. Localisez votre application Web et développez l’entrée pour afficher l’emplacement de site intermédiaire. Cliquez sur son **nom** pour accéder à la page de gestion.
12. Cliquez sur **déploiements** pour afficher l' **historique de déploiement**. Vérifiez qu’il existe un **déploiement actif** avec votre *&quot;&quot;de validation initiale* .

    ![Déploiement actif](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Déploiement actif*
13. Enfin, cliquez sur **Parcourir** dans la barre de commandes pour accéder à l’application Web.

    ![Parcourir l’application Web](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Parcourir l’application Web*
14. Si l’application est déployée avec succès, vous verrez la page de connexion au quiz du passionné.

    > [!NOTE]
    > L’URL d’adresse de l’application déployée contient le nom de votre application Web suivi de *-intermédiaire*.

    ![Application en cours d’exécution dans l’environnement intermédiaire](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Application en cours d’exécution dans l’environnement intermédiaire*
15. Si vous souhaitez explorer l’application, cliquez sur **inscrire** pour inscrire un nouvel utilisateur. Renseignez les informations du compte en entrant un nom d’utilisateur, une adresse de messagerie et un mot de passe. Ensuite, l’application affiche la première question du quiz. Répondez à quelques questions pour vous assurer qu’il fonctionne comme prévu.

    ![Application prête à être utilisée](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Application prête à être utilisée*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Tâche 4 : promotion de l’application Web en production

Maintenant que vous avez vérifié que l’application Web fonctionne correctement dans l’environnement intermédiaire, vous êtes prêt à la promouvoir en production. Au cours de cette tâche, vous allez permuter l’emplacement du site intermédiaire avec l’emplacement du site de production.

1. Revenez au portail de gestion et sélectionnez l’emplacement de site intermédiaire. Cliquez sur **permuter** dans la barre de commandes.

    ![Basculer vers la production](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Basculer vers la production*
2. Cliquez sur **Oui** dans la boîte de dialogue de confirmation pour poursuivre l’opération d’échange. Azure échangera immédiatement le contenu du site de production avec le contenu du site intermédiaire.

    > [!NOTE]
    > Certains paramètres de la version intermédiaire seront automatiquement copiés dans la version de production (par exemple, les substitutions de chaîne de connexion, les mappages de gestionnaires, etc.), mais les autres paramètres ne changeront pas (par exemple, les points de terminaison DNS, les liaisons SSL, etc.).

    ![Confirmation de l’opération d’échange](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Confirmation de l’opération d’échange*
3. Une fois l’échange terminé, sélectionnez l’emplacement de production, puis cliquez sur **Parcourir** dans la barre de commandes pour ouvrir le site de production. Notez l’URL dans la barre d’adresses.

    > [!NOTE]
    > Vous devrez peut-être actualiser votre navigateur pour vider le cache. Dans Internet Explorer, vous pouvez faire cela en appuyant sur **Ctrl + R**.

    ![Application Web en cours d’exécution dans l’environnement de production](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. Dans la console **GitBash** , mettez à jour l’URL distante du référentiel Git local pour cibler l’emplacement de production. Pour ce faire, exécutez la commande suivante en remplaçant les espaces réservés par votre nom d’utilisateur de déploiement et par le nom de votre application Web.

    > [!NOTE]
    > Dans les exercices suivants, vous effectuerez un push des modifications sur le site de production au lieu de le préparer uniquement pour la simplicité du laboratoire. Dans un scénario réel, il est recommandé de vérifier les modifications apportées à l’environnement intermédiaire avant la promotion en production.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Exercice 3 : exécution de la restauration du déploiement en production

Il existe des scénarios dans lesquels vous ne disposez pas d’un emplacement intermédiaire pour effectuer un échange à chaud entre les environnements intermédiaire et de production, par exemple, si vous travaillez avec le mode **gratuit** ou **partagé** . Dans ces scénarios, vous devez tester votre application dans un environnement de test, localement ou sur un site distant, avant le déploiement en production. Toutefois, il est possible qu’un problème non détecté au cours de la phase de test puisse se produire dans le site de production. Dans ce cas, il est important de disposer d’un mécanisme permettant de basculer facilement vers une version antérieure et plus stable de l’application, le plus rapidement possible.

Dans **Azure App service**, le déploiement continu à partir du contrôle de code source rend cela possible grâce à l’action de **redéploiement** disponible dans le portail de gestion. Azure effectue le suivi des déploiements associés aux validations transmises au référentiel et offre la possibilité de redéployer votre application à tout moment, à l’aide de l’un de vos déploiements précédents.

Dans cet exercice, vous allez apporter une modification au code de l’application de **quiz** de l’inadvertance qui injecte intentionnellement un *bogue*. Vous allez déployer l’application en production pour voir l’erreur, puis vous tirerez parti de la fonctionnalité de redéploiement pour revenir à l’état précédent.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Tâche 1 : mise à jour de l’application du quiz du passionné

Dans cette tâche, vous allez Refactoriser un petit morceau de code de la classe **TriviaController** pour extraire une partie de la logique qui récupère l’option de quiz sélectionnée de la base de données dans une nouvelle méthode.

1. Basculez vers l’instance de Visual Studio avec la solution **GeekQuiz** de l’exercice précédent.
2. Dans **Explorateur de solutions**, ouvrez le fichier **TriviaController.cs** dans le dossier **Controllers** .
3. Recherchez la méthode **StoreAsync** et sélectionnez le code mis en surbrillance dans la figure suivante.

    ![Sélection du code](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Sélection du code*
4. Cliquez avec le bouton droit sur le code sélectionné, développez le menu **Refactoriser** , puis sélectionnez **extraire la méthode...** .

    ![Extraction du code en tant que nouvelle méthode](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Sélection de la méthode Extract*
5. Dans la boîte de dialogue **extraire la méthode** , nommez la nouvelle méthode *MatchesOption* , puis cliquez sur **OK**.

    ![Spécification du nom de la méthode](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Spécification du nom de la méthode extraite*
6. Le code sélectionné est ensuite extrait dans la méthode **MatchesOption** . Le code obtenu est illustré dans l’extrait de code suivant.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Appuyez sur **CTRL + S** pour enregistrer les modifications.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Tâche 2 : redéploiement du quiz de l’application

Vous allez maintenant transmettre les modifications que vous avez apportées à la tâche précédente au référentiel, ce qui déclenchera un nouveau déploiement vers l’environnement de production. Ensuite, vous allez dépanner un problème à l’aide des **outils de développement F12** fournis par Internet Explorer, puis effectuer une restauration vers le déploiement précédent à partir du portail de gestion Azure.

1. Ouvrez une nouvelle console **git bash** pour déployer l’application mise à jour sur Azure App service.
2. Exécutez les commandes suivantes pour envoyer les modifications dans Azure. Mettez à jour l’espace réservé *[votre-application-path]* avec le chemin d’accès à la solution **GeekQuiz** . Vous serez invité à entrer votre mot de passe de déploiement.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Envoi de code refactorisé à Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Envoi de code refactorisé à Azure*
3. Ouvrez Internet Explorer et accédez à votre application Web (par exemple, `http://<your-web-site>.azurewebsites.net`). Connectez-vous à l’aide des informations d’identification créées précédemment.
4. Appuyez sur **F12** pour lancer les outils de développement, sélectionnez l’onglet **réseau** , puis cliquez sur le bouton **lecture** pour démarrer l’enregistrement.

    ![Démarrage de l’enregistrement réseau](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Démarrage de l’enregistrement réseau")

    *Démarrage de l’enregistrement réseau*
5. Sélectionnez n’importe quelle option du quiz. Vous verrez que rien ne se passe.
6. Dans la fenêtre **F12** , l’entrée correspondant à la requête HTTP de publication affiche un résultat http **500** .

    ![Erreur HTTP 500](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *Erreur HTTP 500*
7. Sélectionnez l’onglet **console** . Une erreur est consignée avec les détails de la cause.

    ![Erreur journalisée](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Erreur journalisée*
8. Recherchez la partie détails de l’erreur. En clair, cette erreur est provoquée par la refactorisation de code que vous avez validée au cours des étapes précédentes.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Ne fermez pas le navigateur.
10. Dans une nouvelle instance de navigateur, accédez au [portail de gestion Azure](https://manage.windowsazure.com) et connectez-vous à l’aide de la compte Microsoft associée à votre abonnement.
11. Sélectionnez **sites** Web, puis cliquez sur l’application Web que vous avez créée dans l’exercice 2.
12. Accédez à la page **déploiements** . Notez que toutes les validations effectuées sont répertoriées dans l’historique de déploiement.

    ![Liste des déploiements existants](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Liste des déploiements existants*
13. Sélectionnez la validation précédente, puis cliquez sur **redéployer** dans la barre de commandes.

    ![Redéploiement de la validation précédente](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Redéploiement de la validation précédente*
14. À l’invite de confirmation, cliquez sur **Yes**.

    ![Confirmation du redéploiement](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Une fois le déploiement terminé, revenez à l’instance du navigateur avec votre application Web et appuyez sur **CTRL + F5**.
16. Cliquez sur l’une des options. L’animation de retournement aura lieu et le résultat (*correct/incorrect*) s’affichera.
17. Facultatif Basculez vers la console **git bash** et exécutez les commandes suivantes pour revenir à la validation précédente.

    > [!NOTE]
    > Ces commandes créent une nouvelle validation qui annule toutes les modifications dans le référentiel git qui ont été effectuées dans la validation incorrecte. Azure redéploie ensuite l’application à l’aide de la nouvelle validation.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Exercice 4 : mise à l’échelle à l’aide du stockage Azure

Les **objets BLOB** constituent la manière la plus simple de stocker de grandes quantités de texte non structuré ou de données binaires, telles que des vidéos, de l’audio et des images. Le déplacement du contenu statique de votre application vers le stockage permet de mettre à l’échelle votre application en desservant des images ou des documents directement dans le navigateur.

Dans cet exercice, vous allez déplacer le contenu statique de votre application vers un conteneur d’objets BLOB. Ensuite, vous allez configurer votre application pour ajouter une **règle de réécriture d’URL ASP.net** dans le **fichier Web. config** pour rediriger votre contenu vers le conteneur d’objets BLOB.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Tâche 1 : création d’un compte de stockage Azure

Au cours de cette tâche, vous allez apprendre à créer un nouveau compte de stockage à l’aide du portail de gestion.

1. Accédez au [portail de gestion Azure](https://manage.windowsazure.com) et connectez-vous à l’aide de la compte Microsoft associée à votre abonnement.
2. Sélectionner **nouveau | Data Services | Stockage | Création rapide** pour commencer à créer un compte de stockage. Entrez un nom unique pour le compte et sélectionnez une **région** dans la liste. Cliquez sur **créer un compte de stockage** pour continuer.

    ![Création d’un compte de stockage](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Création d’un compte de stockage")

    *Création d’un compte de stockage*
3. Dans la section **stockage** , attendez que l’état du nouveau compte de stockage passe à *en ligne* pour passer à l’étape suivante.

    ![Compte de stockage créé](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Compte de stockage créé")

    *Compte de stockage créé*
4. Cliquez sur le nom du compte de stockage, puis sur le lien **tableau de bord** en haut de la page. La page **tableau de bord** fournit des informations sur l’état du compte et les points de terminaison de service qui peuvent être utilisés dans vos applications.

    ![Affichage du tableau de bord du compte de stockage](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Affichage du tableau de bord du compte de stockage")

    *Affichage du tableau de bord du compte de stockage*
5. Cliquez sur le bouton **gérer les clés d’accès** dans la barre de navigation.

    ![Bouton gérer les clés d’accès](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Bouton gérer les clés d’accès")

    *Bouton gérer les clés d’accès*
6. Dans la boîte de dialogue **gérer les clés d’accès** , copiez le nom du compte de **stockage** et la **clé d’accès primaire** , car vous en aurez besoin dans l’exercice suivant. Ensuite, fermez la boîte de dialogue.

    ![Boîte de dialogue gérer la clé d’accès](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Boîte de dialogue gérer la clé d’accès")

    *Boîte de dialogue gérer la clé d’accès*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Tâche 2 : chargement d’un élément multimédia dans le stockage d’objets BLOB Azure

Dans cette tâche, vous allez utiliser la fenêtre Explorateur de serveurs de Visual Studio pour vous connecter à votre compte de stockage. Vous allez ensuite créer un conteneur d’objets BLOB et télécharger un fichier avec le logo du quiz de l’un des passionnés dans le conteneur.

1. Basculez vers l’instance de Visual Studio avec la solution **GeekQuiz** de l’exercice précédent.
2. Dans la barre de menus, sélectionnez **affichage** , puis cliquez sur **Explorateur de serveurs**.
3. Dans **Explorateur de serveurs**, cliquez avec le bouton droit sur le nœud **Azure** , puis sélectionnez **se connecter à Azure.** ... Connectez-vous à l’aide de la compte Microsoft associée à votre abonnement.

    ![Connexion à Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Connexion à Azure*
4. Développez le nœud **Azure** , cliquez avec le bouton droit sur **stockage** , puis sélectionnez **attacher un stockage externe...** .
5. Dans la boîte de dialogue **Ajouter un nouveau compte de stockage** , entrez le **nom du compte** et la **clé de compte** que vous avez obtenus lors de la tâche précédente, puis cliquez sur **OK**.

    ![Boîte de dialogue Ajouter un nouveau compte de stockage](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Boîte de dialogue Ajouter un nouveau compte de stockage*
6. Votre compte de stockage doit apparaître sous le nœud **stockage** . Développez votre compte de stockage, cliquez avec le bouton droit sur **objets BLOB** , puis sélectionnez **créer un conteneur d’objets BLOB...** .

    ![Créer un conteneur d’objets BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Créer un conteneur d'objets blob")

    *Créer un conteneur d’objets BLOB*
7. Dans la boîte de dialogue **créer un conteneur d’objets BLOB** , entrez un nom pour le conteneur d’objets BLOB, puis cliquez sur **OK**.

    ![Boîte de dialogue créer un conteneur d’objets BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Boîte de dialogue créer un conteneur d’objets BLOB")

    *Boîte de dialogue créer un conteneur d’objets BLOB*
8. Le nouveau conteneur d’objets BLOB doit être ajouté au nœud **blobs** . Modifiez les autorisations d’accès dans le conteneur pour rendre le conteneur public. Pour ce faire, cliquez avec le bouton droit sur le conteneur **images** et sélectionnez **Propriétés**.

    ![Propriétés du conteneur d’images](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "Propriétés du conteneur d’images")

    *Propriétés du conteneur d’images*
9. Dans la fenêtre **Propriétés** , définissez l' **accès public en lecture** au **conteneur**.

    ![Modification de la propriété d’accès en lecture publique](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Modification de la propriété d’accès en lecture publique")

    *Modification de la propriété d’accès en lecture publique*
10. Lorsque vous êtes invité à confirmer que vous souhaitez modifier la propriété d’accès public, cliquez sur **Oui**.

    ![AVERTISSEMENT Microsoft Visual Studio](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "AVERTISSEMENT Microsoft Visual Studio")

    *AVERTISSEMENT Microsoft Visual Studio*
11. Dans **Explorateur de serveurs**, cliquez avec le bouton droit sur le conteneur d’objets BLOB **images** , puis sélectionnez **afficher le conteneur d’objets BLOB**.

    ![Afficher le conteneur d’objets BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Afficher le conteneur d’objets BLOB")

    *Afficher le conteneur d’objets BLOB*
12. Le conteneur images doit s’ouvrir dans une nouvelle fenêtre et une légende sans entrée doit être affichée. Cliquez sur l’icône **Télécharger** pour charger un fichier dans le conteneur d’objets BLOB.

    ![Conteneur d’images sans entrées](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Conteneur d’images sans entrées")

    *Conteneur d’images sans entrées*
13. Dans la boîte de dialogue **Télécharger l’objet BLOB** , accédez au dossier **composants** du Lab. Sélectionnez le fichier **logo-Big. png** , puis cliquez sur **ouvrir**.
14. Attendez que le fichier soit téléchargé. Une fois le téléchargement terminé, le fichier doit être listé dans le conteneur images. Cliquez avec le bouton droit sur l’entrée de fichier et sélectionnez **copier l’URL**.

    ![Copier l’URL de l’objet BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copier l’URL du fichier BLOB")

    *Copier l’URL de l’objet BLOB*
15. Ouvrez Internet Explorer et collez l’URL. L’image suivante doit s’afficher dans le navigateur.

    ![image logo-Big. png à partir du stockage d’objets BLOB Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "image logo-Big. png à partir du stockage")

    *image logo-Big. png à partir du stockage d’objets BLOB Azure*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Tâche 3 : mise à jour de la solution pour consommer du contenu statique à partir du stockage d’objets BLOB Azure

Dans cette tâche, vous allez configurer la solution **GeekQuiz** pour utiliser l’image chargée dans le stockage d’objets BLOB Azure (au lieu de l’image située dans l’application Web) en ajoutant une règle de réécriture d’URL ASP.net dans le fichier **Web. config** .

1. Dans Visual Studio, ouvrez le fichier **Web. config** à l’intérieur du projet **GeekQuiz** et recherchez l’élément **&lt;System. webServer&gt;** .
2. Ajoutez le code suivant pour ajouter une règle de réécriture d’URL, en mettant à jour l’espace réservé avec le nom de votre compte de stockage.

    (Extrait de code- *WebSitesInProduction-EX4-UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > La réécriture d’URL est le processus qui consiste à intercepter une requête Web entrante et à rediriger la demande vers une autre ressource. Les règles de réécriture d’URL indiquent au moteur de réécriture qu’une demande doit être redirigée et où doivent-elles être redirigées. Une règle de réécriture est composée de deux chaînes : le modèle à rechercher dans l’URL demandée (généralement, à l’aide d’expressions régulières) et la chaîne à laquelle remplacer le modèle, le cas échéant. Pour plus d’informations, consultez [réécriture d’URL dans ASP.net](https://msdn.microsoft.com/library/ms972974.aspx).
3. Appuyez sur **CTRL + S** pour enregistrer les modifications.
4. Ouvrez une nouvelle console **git bash** pour déployer l’application mise à jour sur Azure App service.
5. Exécutez les commandes suivantes pour envoyer les modifications dans Azure. Mettez à jour l’espace réservé *[votre-application-path]* avec le chemin d’accès à la solution **GeekQuiz** . Vous serez invité à entrer votre mot de passe de déploiement.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Déploiement de la mise à jour vers Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Déploiement de la mise à jour vers Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Tâche 4 : vérification

Au cours de cette tâche, vous allez utiliser **Internet Explorer** pour parcourir l’application du **quiz** de l’inversion et vérifier que la règle de réécriture d’URL pour les images fonctionne et que vous êtes redirigé vers l’image hébergée sur le **stockage d’objets BLOB Azure**.

1. Ouvrez Internet Explorer et accédez à votre application Web (par exemple, `http://<your-web-site>.azurewebsites.net`). Connectez-vous à l’aide des informations d’identification créées précédemment.

    ![Présentation de l’application Web du quiz de l’inversion avec l’image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Présentation de l’application Web du quiz de l’inversion avec l’image")

    *Présentation de l’application Web du quiz de l’inversion avec l’image*
2. Appuyez sur **F12** pour lancer les outils de développement, sélectionnez l’onglet **réseau** et démarrez l’enregistrement.

    ![Démarrage de l’enregistrement réseau](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Démarrage de l’enregistrement réseau")

    *Démarrage de l’enregistrement réseau*
3. Appuyez sur **CTRL + F5** pour actualiser la page Web.
4. Une fois la page chargée, vous devriez voir une requête HTTP pour l’URL **/img/logo-Big.png** avec un résultat http **301** (redirection) et une autre demande pour `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL avec un résultat http **200** .

    ![Vérification de la redirection de l’URL](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Présentation de la redirection dans les outils de développement")

    *Vérification de la redirection de l’URL*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Exercice 5 : utilisation de la mise à l’échelle automatique pour Web Apps

> [!NOTE]
> Cet exercice est facultatif, car il nécessite la prise en charge de la charge Web &amp; le test des performances qui est uniquement disponible pour **Visual Studio 2013 Édition intégrale**. Pour plus d’informations sur les fonctionnalités de Visual Studio 2013 spécifiques, comparez les versions [ici](https://www.microsoft.com/visualstudio/eng/products/compare).

**Azure App Service Web Apps** fournit la fonctionnalité de mise à l’échelle automatique pour les applications Web qui s’exécutent en **mode standard**. La mise à l’échelle automatique permet à Azure de mettre automatiquement à l’échelle le nombre d’instances de votre application Web en fonction de la charge. Lorsque la mise à l’échelle automatique est activée, Azure vérifie l’UC de votre application Web toutes les cinq minutes et ajoute les instances nécessaires à ce moment-là. Si l’utilisation du processeur est faible, Azure supprime les instances une fois toutes les deux heures pour s’assurer que les performances de votre application Web ne sont pas dégradées.

Dans cet exercice, vous allez suivre les étapes nécessaires à la configuration de la fonctionnalité de mise à l' **échelle** automatique pour l’application Web de **quiz** de l’inversion. Vous allez vérifier cette fonctionnalité en exécutant un test de charge Visual Studio pour générer une charge d’UC suffisante sur l’application pour déclencher la mise à niveau d’une instance.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Tâche 1 : configuration de la mise à l’échelle automatique en fonction de la métrique de l’UC

Dans cette tâche, vous allez utiliser le portail de gestion Azure pour activer la fonctionnalité de mise à l’échelle automatique pour l’application Web que vous avez créée dans l’exercice 2.

1. Dans le [portail de gestion Azure](https://manage.windowsazure.com/), sélectionnez **sites** Web, puis cliquez sur l’application Web que vous avez créée dans l’exercice 2.
2. Accédez à la page mettre à l' **échelle** . Sous la section **capacité** , sélectionnez **UC** pour la configuration d' **échelle par métrique** .

    > [!NOTE]
    > Lors de la mise à l’échelle par UC, Azure ajuste dynamiquement le nombre d’instances que l’application utilise en cas de modification de l’utilisation de l’UC.

    ![Sélection de la mise à l’échelle par UC](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Sélection de la métrique de l’UC pour la mise à l’échelle automatique")

    *Sélection de la mise à l’échelle par UC*
3. Remplacez la configuration de l' **UC cible** par **20**-**40** pour cent.

    > [!NOTE]
    > Cette plage représente l’utilisation moyenne de l’UC pour votre application Web. Azure ajoutera ou supprimera des instances pour conserver votre application Web dans cette plage. Le nombre minimal et maximal d’instances utilisées pour la mise à l’échelle est spécifié dans la configuration du **nombre** d’instances. Azure ne passera jamais au-delà de cette limite.
    >
    > Les valeurs par défaut de l' **UC cible** sont modifiées uniquement pour les besoins de cet atelier. En configurant la plage de l’UC avec des petites valeurs, vous augmentez les chances de déclencher la mise à l’échelle automatique lorsqu’une charge modérée est placée sur l’application.

    ![Modification de l’UC cible pour qu’elle soit comprise entre 20 et 40 pour cent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Modification de l’UC cible pour qu’elle soit comprise entre 20 et 40 pour cent")

    *Modification de l’UC cible pour qu’elle soit comprise entre 20 et 40 pour cent*
4. Cliquez sur **Enregistrer** dans la barre de commandes pour enregistrer les modifications.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Tâche 2 : test de charge avec Visual Studio

Maintenant que la mise à l' **échelle** automatique a été configurée, vous allez créer un **projet de test de performances Web et de charge** dans Visual Studio pour générer une charge de l’UC sur votre application Web.

1. Ouvrez **Visual Studio Ultimate 2013** et sélectionnez **fichier | Nouveau | Projet...** pour démarrer une nouvelle solution.

    ![Création d’un nouveau projet](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Création d’un projet")

    *Création d’un nouveau projet*
2. Dans la boîte de dialogue **nouveau projet** , sélectionnez **projet de test de performances Web et de charge** sous le **visuel | C# Onglet test** . Assurez-vous que **.NET Framework 4,5** est sélectionné, nommez le projet *WebAndLoadTestProject*, choisissez un **emplacement** , puis cliquez sur **OK**.

    ![Création d’un projet de test Web et de charge](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Création d’un projet de test Web et de charge")

    *Création d’un projet de test Web et de charge*
3. Dans **WebTest1. WebTest** , cliquez avec le bouton droit sur le nœud **WebTest1** , puis cliquez sur **Ajouter une demande**.

    ![Ajout d’une requête à WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Ajout d’une requête à WebTest1")

    *Ajout d’une requête à WebTest1*
4. Dans la fenêtre **Propriétés** du nouveau nœud requête, mettez à jour la propriété **URL** de sorte qu’elle pointe vers l’URL de votre application Web (par exemple, *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* ).

    ![Modification de la propriété URL](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Modification de la propriété URL")

    *Modification de la propriété URL*
5. Dans la fenêtre **WebTest1. WebTest** , cliquez avec le bouton droit sur **WebTest1** , puis cliquez sur **Ajouter une boucle...** .

    ![Ajout d’une boucle à WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Ajout d’une boucle à WebTest1")

    *Ajout d’une boucle à WebTest1*
6. Dans la boîte de dialogue **Ajouter une règle conditionnelle et des éléments à la boucle** , sélectionnez la règle **de boucle for** et modifiez les propriétés suivantes.

   1. **Valeur de fin :** 1000
   2. **Nom du paramètre de contexte :** Répétiteur
   3. **Valeur d’incrément :** 1

      ![Sélection de la règle de boucle for et mise à jour des propriétés](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Sélection de la règle de boucle for et mise à jour des propriétés")

      *Sélection de la règle de boucle for et mise à jour des propriétés*
7. Dans la section **éléments dans la boucle** , sélectionnez la requête que vous avez créée précédemment pour être le premier et le dernier élément de la boucle. Cliquez sur **OK** pour continuer.

    ![Sélection des premier et dernier éléments de la boucle](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Sélection des premier et dernier éléments de la boucle")

    *Sélection des premier et dernier éléments de la boucle*
8. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **WebAndLoadTestProject** , développez le menu **Ajouter** et sélectionnez **test de charge...** .

    ![Ajout d’un test de charge au projet WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Ajout d’un test de charge au projet WebAndLoadTestProject")

    *Ajout d’un test de charge au projet WebAndLoadTestProject*
9. Dans la boîte de dialogue **nouveau Assistant test de charge** , cliquez sur **suivant**.

    ![Nouvelle Assistant Test de charge](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Nouvelle Assistant Test de charge")

    *Nouvelle Assistant Test de charge*
10. Dans la page **scénario** , sélectionnez **ne pas utiliser les temps de réflexion** , puis cliquez sur **suivant**.

    ![Sélectionner de ne pas utiliser les temps de réflexion](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Sélectionner de ne pas utiliser les temps de réflexion")

    *Sélectionner de ne pas utiliser les temps de réflexion*
11. Dans la page **modèle de charge** , assurez-vous que l’option de **charge constante** est sélectionnée. Modifiez le paramètre du **nombre d’utilisateurs** sur **250** utilisateurs, puis cliquez sur **suivant**.

    ![Modification du nombre d’utilisateurs en 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Modification du nombre d’utilisateurs en 250")

    *Modification du nombre d’utilisateurs en 250*
12. Dans la page **modèle de combinaison de tests** , sélectionnez **basé sur l’ordre séquentiel des tests** , puis cliquez sur **suivant**.

    ![Sélection du modèle de combinaison de tests](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Sélection du modèle de combinaison de tests")

    *Sélection du modèle de combinaison de tests*
13. Dans la page **modèle de combinaison de tests** , cliquez sur **Ajouter** pour ajouter un test à la combinaison.

    ![Ajout d’un test à la combinaison de tests](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Ajout d’un test à la combinaison de tests")

    *Ajout d’un test à la combinaison de tests*
14. Dans la boîte de dialogue **Ajouter des tests** , double-cliquez sur **WebTest1** pour ajouter le test à la liste des **tests sélectionnés** . Cliquez sur **OK** pour continuer.

    ![Ajout du test WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Ajout du test WebTest1")

    *Ajout du test WebTest1*
15. De retour dans la page **combinaison de tests** , cliquez sur **suivant**.

    ![Fin de la page de combinaison de tests](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Fin de la page de combinaison de tests")

    *Fin de la page de combinaison de tests*
16. Dans la page **combinaison de réseaux** , cliquez sur **suivant**.

    ![Cliquant sur suivant dans la page de combinaison de réseaux](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Cliquant sur suivant dans la page de combinaison de réseaux")

    *Cliquant sur suivant dans la page de combinaison de réseaux*
17. Dans la page **combinaison de navigateurs** , sélectionnez **Internet Explorer 10,0** comme type de navigateur, puis cliquez sur **suivant**.

    ![Sélection du type de navigateur](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Sélection du type de navigateur")

    *Sélection du type de navigateur*
18. Dans la page **ensembles de compteurs** , cliquez sur **suivant**.

    ![Cliquant sur suivant dans la page ensembles de compteurs](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Cliquant sur suivant dans la page ensembles de compteurs")

    *Cliquant sur suivant dans la page ensembles de compteurs*
19. Dans la page **paramètres d’exécution** , définissez la durée du test de **charge** sur **5 minutes** , puis cliquez sur **Terminer**.

    ![Définition de la durée du test de charge sur 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Définition de la durée du test de charge sur 5 minutes")

    *Définition de la durée du test de charge sur 5 minutes*
20. Dans **Explorateur de solutions**, double-cliquez sur le fichier **local. Settings** pour explorer les paramètres de test. Par défaut, Visual Studio utilise votre ordinateur local pour exécuter les tests.

    > [!NOTE]
    > Vous pouvez également configurer votre projet de test pour exécuter les tests de charge dans le Cloud à l’aide de **Azure test plans**. Azure Test Plans fournit un service de test de charge basé sur le Cloud qui simule une charge plus réaliste, évitant les contraintes de l’environnement local, telles que la capacité du processeur, la mémoire disponible et la bande passante réseau. Pour plus d’informations sur l’utilisation de Azure Test Plans pour exécuter des tests de charge, consultez [scénarios de test de charge](/azure/devops/test/load-test/overview?view=vsts).

    ![Paramètres de test](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Tâche 3 : vérification de la mise à l’échelle automatique

Vous allez maintenant exécuter le test de charge que vous avez créé lors de la tâche précédente et voir comment votre application Web se comporte sous charge.

1. Dans **Explorateur de solutions**, double-cliquez sur **LoadTest1. LoadTest** pour ouvrir le test de charge.

    ![Ouverture de LoadTest1. LoadTest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Ouverture de LoadTest1. LoadTest")

    *Ouverture de LoadTest1. LoadTest*
2. Dans la fenêtre **LoadTest1. LoadTest** , cliquez sur le premier bouton de la boîte à outils pour exécuter le test de charge.

    ![Exécution du test de charge](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Exécution du test de charge")

    *Exécution du test de charge*
3. Attendez la fin du test de charge.

    > [!NOTE]
    > Le test de charge simule plusieurs utilisateurs qui envoient simultanément des demandes à l’application Web. Lorsque le test est en cours d’exécution, vous pouvez surveiller les compteurs disponibles pour détecter les erreurs, les avertissements ou d’autres informations relatives à votre série de tests de charge.

    ![Exécution du test de charge](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "En attente jusqu’à la fin du test de charge")

    *Exécution du test de charge*
4. Une fois le test terminé, revenez au portail de gestion et accédez à la page mettre à l' **échelle** de votre application Web. Dans la section **capacité** , vous devriez voir dans le graphique qu’une nouvelle instance a été déployée automatiquement.

    ![Nouvelle instance déployée automatiquement](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nouvelle instance déployée automatiquement*

    > [!NOTE]
    > Plusieurs minutes peuvent être nécessaires pour que les modifications apparaissent dans le graphique (appuyez sur **CTRL + F5** régulièrement pour actualiser la page). Si vous ne voyez aucune modification, vous pouvez essayer ce qui suit :
    >
    > - Augmentez la durée du test de charge (par exemple, jusqu’à **10 minutes**)
    > - Réduire les valeurs maximale et minimale de la plage de l' **UC cible** dans la configuration de la mise à l’échelle automatique de votre application Web
    > - Exécutez le test de charge dans le Cloud avec **Azure test plans**. Plus d’informations [ici](/azure/devops/test/load-test/index?view=vsts)

---

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

Dans ce laboratoire pratique, vous avez appris à configurer et déployer votre application sur des applications Web de production dans Azure. Vous avez démarré en détectant et en mettant à jour vos bases de données à l’aide de **migrations Entity Framework code First**, puis en déployant de nouvelles versions de votre site à l’aide de **git** et en effectuant des restaurations vers la dernière version stable de votre site. En outre, vous avez appris à mettre à l’échelle votre application à l’aide du stockage pour déplacer votre contenu statique vers un conteneur d’objets BLOB.
