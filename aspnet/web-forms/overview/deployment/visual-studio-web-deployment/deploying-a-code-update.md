---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour du code | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3881833bfe2a50a38a357614f92f434a04a8ab08
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74626778"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour de code

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).

## <a name="overview"></a>Vue d'ensemble de

Après le déploiement initial, votre travail de maintenance et de développement de votre site Web continue, et avant que vous ne souhaitiez déployer une mise à jour. Ce didacticiel vous guide tout au long du processus de déploiement d’une mise à jour de votre code d’application. La mise à jour que vous implémentez et déployez dans ce didacticiel n’implique pas de modification de la base de données. Pour plus d’informations sur le déploiement d’une modification de base de données, consultez le didacticiel suivant.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](troubleshooting.md).

## <a name="make-a-code-change"></a>Apporter une modification au code

En guise d’exemple simple de mise à jour de votre application, vous allez ajouter à la page des **enseignants** une liste de cours traités par l’instructeur sélectionné.

Si vous exécutez la **page des** formateurs, vous remarquerez qu’il y a des liens **sélectionnés** dans la grille, mais qu’ils ne font rien d’autre que de rendre l’arrière-plan de la ligne grisé.

![Page des enseignants avec sélection](deploying-a-code-update/_static/image1.png)

À présent, vous allez ajouter le code qui s’exécute lorsque vous cliquez sur le lien **Sélectionner** et afficher la liste des cours traités par l’instructeur sélectionné.

1. Dans *Instructors. aspx*, ajoutez le balisage suivant juste après le contrôle **ErrorMessageLabel** `Label` :

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Exécutez la page et sélectionnez un formateur. Vous voyez une liste des cours enseignés par cet instructeur.

    ![Page des instructeurs avec des cours enseignés](deploying-a-code-update/_static/image2.png)
3. Fermez le navigateur.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Déployer la mise à jour du code dans l’environnement de test

Avant de pouvoir utiliser vos profils de publication pour déployer en test, intermédiaire et de production, vous devez modifier les options de publication de base de données. Vous n’avez plus besoin d’exécuter les scripts de déploiement d’octroi et de données pour la base de données d’appartenance.

1. Ouvrez l’Assistant **publier le site Web** en cliquant avec le bouton droit sur le projet ContosoUniversity, puis en cliquant sur **publier**.
2. Cliquez sur le profil **test** dans la liste déroulante **Profil** .
3. Cliquez sur l’onglet **paramètres** .
4. Sous **DefaultConnection** , dans la section **bases de données** , désactivez la case à cocher **mettre à jour la base de données** .
5. Cliquez sur l’onglet **Profil** , puis cliquez sur le profil **intermédiaire** dans la liste déroulante **Profil** .
6. Quand vous êtes invité à enregistrer les modifications apportées au profil de **test** , cliquez sur **Oui**.
7. Apportez la même modification dans le profil intermédiaire.
8. Répétez le processus pour apporter la même modification dans le profil de production.
9. Fermez l’Assistant **publier le site Web** .

Le déploiement sur l’environnement de test est maintenant un simple problème d’exécution de la publication en un clic. Pour accélérer ce processus, vous pouvez utiliser la barre d’outils de **publication Web en un clic** .

1. Dans le menu **affichage** , choisissez **barres d’outils** , puis sélectionnez **publier sur le Web**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. Dans **Explorateur de solutions**, sélectionnez le projet ContosoUniversity.
3. la barre d’outils **publication Web en un clic** , choisissez le profil de publication de **test** , puis cliquez sur **publier le site Web** (l’icône avec des flèches pointant vers la gauche et la droite).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio déploie l’application mise à jour et le navigateur s’ouvre automatiquement sur la page d’hébergement.
5. Exécutez la page enseignants et sélectionnez un formateur pour vérifier que la mise à jour a été correctement déployée.

En règle générale, vous effectuez des tests de régression (autrement dit, vous testez le reste du site pour vous assurer que la nouvelle modification n’a pas permis de rompre les fonctionnalités existantes). Toutefois, pour ce didacticiel, vous allez ignorer cette étape et procéder au déploiement de la mise à jour vers l’environnement intermédiaire et de production.

Lorsque vous redéployez, Web Deploy détermine automatiquement les fichiers qui ont changé et copie uniquement les fichiers modifiés sur le serveur. Par défaut, Web Deploy utilise des dates de dernière modification sur les fichiers pour déterminer celles qui ont changé. Certains systèmes de contrôle de code source modifient les dates des fichiers même lorsque vous ne modifiez pas le contenu du fichier. Dans ce cas, vous souhaiterez peut-être configurer Web Deploy pour utiliser les sommes de contrôle de fichier afin de déterminer les fichiers qui ont été modifiés. Pour plus d’informations, consultez [pourquoi tous mes fichiers sont-ils redéployés, même si je ne les ai pas modifiés ?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) dans le Forum aux questions sur le déploiement ASP.net.

## <a name="take-the-application-offline-during-deployment"></a>Mettre l’application hors connexion pendant le déploiement

La modification que vous déployez maintenant est une simple modification apportée à une seule page. Mais parfois, vous déployez des modifications plus importantes ou vous déployez des modifications de code et de base de données, et le site peut se comporter de manière incorrecte si un utilisateur demande une page avant la fin du déploiement. Pour empêcher les utilisateurs d’accéder au site pendant que le déploiement est en cours, vous pouvez utiliser une *application\_fichier offline. htm* . Quand vous placez un fichier nommé *app\_offline. htm* dans le dossier racine de votre application, IIS affiche automatiquement ce fichier au lieu d’exécuter votre application. Pour empêcher l’accès pendant le déploiement, vous placez l' *application\_offline. htm* dans le dossier racine, vous exécutez le processus de déploiement, puis vous supprimez l' *application\_offline. htm* après un déploiement réussi.

Vous pouvez configurer Web Deploy pour placer automatiquement une application par défaut *\_fichier. htm hors connexion* sur le serveur lorsqu’elle commence à déployer et à la supprimer une fois qu’elle est terminée. Pour ce faire, il vous suffit d’ajouter l’élément XML suivant à votre fichier de profil de publication (. pubxml) :

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Pour ce didacticiel, vous allez apprendre à créer et à utiliser une *application personnalisée\_fichier offline. htm* .

L’utilisation de l' *application\_offline. htm* sur le site intermédiaire n’est pas nécessaire, car vous n’avez pas d’utilisateurs qui accèdent au site intermédiaire. Mais il est recommandé d’utiliser la mise en lots pour tester tout ce que vous prévoyez de déployer en production.

### <a name="create-app_offlinehtm"></a>Créer une application\_offline. htm

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la solution, cliquez sur **Ajouter**, puis sur **nouvel élément**.
2. Créez une **page HTML** nommée *app\_offline. htm* (supprimez le « l » final de l’extension *. html* que Visual Studio crée par défaut).
3. Remplacez le balisage de modèle par le balisage suivant :

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Enregistrez et fermez le fichier.

### <a name="copy-app_offlinehtm-to-the-root-folder-of-the-web-site"></a>Copier l’application\_offline. htm dans le dossier racine du site Web

Vous pouvez utiliser n’importe quel outil FTP pour copier des fichiers sur le site Web. [FileZilla](http://filezilla-project.org/) est un outil FTP populaire qui est affiché dans les captures d’écran.

Pour utiliser un outil FTP, vous avez besoin de trois éléments : l’URL FTP, le nom d’utilisateur et le mot de passe.

L’URL s’affiche sur la page Tableau de bord du site Web dans le Portail de gestion Azure, et le nom d’utilisateur et le mot de passe pour FTP se trouvent dans le fichier *. publishsettings* que vous avez téléchargé précédemment. Les étapes suivantes montrent comment récupérer ces valeurs.

1. Dans le Portail de gestion Azure, cliquez sur l’onglet **sites Web** , puis cliquez sur le site Web intermédiaire.
2. Dans la page **tableau de bord** , faites défiler l’onglet pour rechercher le nom d’hôte FTP dans la section **Aperçu rapide** .

    ![Nom d’hôte FTP](deploying-a-code-update/_static/image5.png)
3. Ouvrez le fichier Staging *. publishsettings* dans le bloc-notes ou dans un autre éditeur de texte.
4. Recherchez l’élément `publishProfile` pour le profil FTP.
5. Copiez les valeurs `userName` et `userPWD`.

    ![Informations d’identification FTP](deploying-a-code-update/_static/image6.png)
6. Ouvrez votre outil FTP et connectez-vous à l’URL FTP.
7. Copiez l' *application\_offline. htm* à partir du dossier de la solution dans le dossier */site/wwwroot* du site intermédiaire.

    ![Copier app_offline](deploying-a-code-update/_static/image7.png)
8. Accédez à l’URL de votre site intermédiaire. Vous voyez que la page *application\_offline. htm* s’affiche désormais à la place de votre page d’hébergement.

    ![app_offline. htm dans la fenêtre du navigateur](deploying-a-code-update/_static/image8.png)

Vous êtes maintenant prêt à effectuer le déploiement dans un environnement intermédiaire.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Déployer la mise à jour du code dans un environnement intermédiaire et de production

1. Dans la barre d’outils **publication Web en un clic** , choisissez le profil de publication **intermédiaire** , puis cliquez sur **publier le site Web**.

    Visual Studio déploie l’application mise à jour et ouvre le navigateur sur la page d’hébergement du site. L' *application\_fichier. htm hors connexion* s’affiche. Avant de pouvoir effectuer un test pour vérifier que le déploiement a réussi, vous devez supprimer l' *application\_fichier offline. htm* .
2. Revenez à votre outil FTP et supprimez l' **application\_offline. htm** du site intermédiaire.
3. Dans le navigateur, ouvrez la page des instructeurs sur le site intermédiaire et sélectionnez un formateur pour vérifier que la mise à jour a été correctement déployée.
4. Suivez la même procédure pour la production que pour la mise en lots.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Examen des modifications et déploiement de fichiers spécifiques

Visual Studio 2012 vous donne également la possibilité de déployer des fichiers individuels. Pour un fichier sélectionné, vous pouvez afficher les différences entre la version locale et la version déployée, déployer le fichier dans l’environnement de destination ou copier le fichier de l’environnement de destination vers le projet local. Dans cette section du didacticiel, vous allez apprendre à utiliser ces fonctionnalités.

### <a name="make-a-change-to-deploy"></a>Apporter une modification au déploiement

1. Ouvrez *content/site. CSS*et recherchez le bloc de la balise `body`.
2. Remplacez la valeur de `background-color` `#fff` par `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Afficher la modification dans la fenêtre d’aperçu de publication

Lorsque vous utilisez l’Assistant **publier le site Web** pour publier le projet, vous pouvez voir les modifications qui vont être publiées en double-cliquant sur le fichier dans la fenêtre d' **Aperçu** .

1. Cliquez avec le bouton droit sur le projet ContosoUniversity, puis cliquez sur **publier**.
2. Dans la liste déroulante **Profil** , sélectionnez le profil de publication de **test** .
3. Cliquez sur **Aperçu**, puis sur **Démarrer l’aperçu**.
4. Dans le volet de **visualisation** , double-cliquez sur **site. CSS**.

    ![Double-cliquez sur site. CSS](deploying-a-code-update/_static/image9.png)

    La boîte de dialogue **aperçu des modifications** affiche un aperçu des modifications qui seront déployées.

    ![Aperçu des modifications apportées à site. CSS](deploying-a-code-update/_static/image10.png)

    Si vous double-cliquez sur le fichier *Web. config* , la boîte de dialogue **aperçu des modifications** affiche l’effet de vos transformations de configuration de build et des transformations de profil de publication. À ce stade, vous n’avez rien effectué, ce qui entraînerait la modification du fichier *Web. config* sur le serveur. vous ne verrez donc aucune modification. Toutefois, la fenêtre **aperçu des modifications** affiche de manière incorrecte deux modifications. Il semble que deux éléments XML seront supprimés. Ces éléments sont ajoutés par le processus de publication lorsque vous sélectionnez **exécuter migrations code First au démarrage** de l’application pour une classe de contexte code First. La comparaison est effectuée avant que le processus de publication n’ajoute ces éléments. il semble donc qu’ils sont supprimés même s’ils ne sont pas supprimés. Cette erreur sera corrigée dans une version ultérieure.
5. Cliquez sur **Fermer**.
6. Cliquez sur **Publier**.
7. Lorsque le navigateur s’ouvre sur la page d’origine du site de test, appuyez sur CTRL + F5 pour effectuer une actualisation matérielle afin de voir l’effet de la modification CSS.

    ![Effet de la modification CSS](deploying-a-code-update/_static/image11.png)
8. Fermez le navigateur.

### <a name="publish-specific-files-from-solution-explorer"></a>Publier des fichiers spécifiques à partir d’Explorateur de solutions

Supposons que vous n’aimez pas l’arrière-plan bleu et que vous souhaitiez revenir à la couleur d’origine. Dans cette section, vous allez restaurer les paramètres d’origine en publiant un fichier spécifique directement à partir de **Explorateur de solutions**.

1. Ouvrez *content/site. CSS* et restaurez le paramètre `background-color` sur `#fff`.
2. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le fichier *content/site. CSS* .

    Le menu contextuel affiche trois options de publication.

    ![Publier les options de Explorateur de solutions](deploying-a-code-update/_static/image12.png)
3. Cliquez sur **aperçu des modifications apportées à site. CSS**.

    Une fenêtre s’ouvre pour afficher les différences entre le fichier local et sa version dans l’environnement de destination.

    ![Diff-Content/site. CSS](deploying-a-code-update/_static/image13.png)
4. Dans **Explorateur de solutions**, cliquez à nouveau avec le bouton droit sur **site. CSS** , puis cliquez sur **publier le site. CSS**.

    La fenêtre **activité de publication Web** indique que le fichier a été publié.

    ![Fenêtre d’activité de publication Web](deploying-a-code-update/_static/image14.png)
5. Ouvrez un navigateur à l’URL `http://localhost/contosouniversity`, puis appuyez sur CTRL + F5 pour effectuer une actualisation matérielle afin de voir l’effet de la modification CSS.

    ![Page d’hébergement avec CSS normal](deploying-a-code-update/_static/image15.png)
6. Fermez le navigateur.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant vu plusieurs manières de déployer une mise à jour d’application qui n’implique pas de modification de base de données, et vous avez vu comment afficher un aperçu des modifications pour vérifier que ce qui sera mis à jour est ce que vous attendez. La page des formateurs contient maintenant une section **formations enseignées** .

![Page des instructeurs avec des cours enseignés](deploying-a-code-update/_static/image16.png)

Le didacticiel suivant vous montre comment déployer une modification de base de données : vous ajouterez un champ BirthDate à la base de données et à la page des formateurs.

> [!div class="step-by-step"]
> [Précédent](deploying-to-production.md)
> [Suivant](deploying-a-database-update.md)
