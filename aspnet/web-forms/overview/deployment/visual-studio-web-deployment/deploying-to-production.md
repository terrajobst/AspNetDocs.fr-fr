---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement en production | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: ddc3d15f0436c4c3a24491cf0377111768da67df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632782"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement en production

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).

## <a name="overview"></a>Présentation

Dans ce didacticiel, vous allez configurer un compte Microsoft Azure, créer des environnements intermédiaires et de production, puis déployer votre application Web ASP.NET dans les environnements intermédiaire et de production à l’aide de la fonctionnalité de publication en un clic de Visual Studio.

Si vous préférez, vous pouvez effectuer le déploiement sur un fournisseur d’hébergement tiers. La plupart des procédures décrites dans ce didacticiel sont les mêmes pour un fournisseur d’hébergement ou pour Azure, à ceci près que chaque fournisseur possède sa propre interface utilisateur pour la gestion des comptes et des sites Web. Vous pouvez trouver un fournisseur d’hébergement dans la [Galerie de fournisseurs](https://www.microsoft.com/web/hosting) sur le site Web Microsoft.com.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous suivez le didacticiel, consultez la page de résolution des problèmes dans cette série de didacticiels.

## <a name="get-a-microsoft-azure-account"></a>Obtenir un compte Microsoft Azure

Si vous n’avez pas encore de compte Azure, vous pouvez créer un compte d’évaluation gratuit en quelques minutes. Pour plus d’informations, consultez [Essai gratuit Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Créer un environnement intermédiaire

> [!NOTE]
> Depuis l’écriture de ce didacticiel, Azure App Service ajouté une nouvelle fonctionnalité pour automatiser un grand nombre des processus de création d’environnements intermédiaires et de production. Consultez [configurer des environnements intermédiaires pour les applications Web dans Azure App service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).

Comme expliqué dans le [didacticiel déployer dans l’environnement de test](deploying-to-iis.md), l’environnement de test le plus fiable est un site Web au niveau du fournisseur d’hébergement qui se présente comme le site Web de production. Chez de nombreux fournisseurs d’hébergement, vous auriez à évaluer les avantages de cette solution par rapport à des coûts supplémentaires significatifs, mais dans Azure, vous pouvez créer une application Web gratuite supplémentaire en tant qu’application intermédiaire. Vous avez également besoin d’une base de données, et les dépenses supplémentaires pour celles-ci au détriment de votre base de données de production seront soit None, soit minimal. Dans Azure, vous payez la quantité de stockage de base de données que vous utilisez plutôt que pour chaque base de données, et la quantité de stockage supplémentaire que vous allez utiliser dans la mise en lots sera minime.

Comme expliqué dans le [didacticiel déployer dans l’environnement de test](deploying-to-iis.md), dans intermédiaire et de production, vous allez déployer vos deux bases de données dans une seule base de données. Si vous souhaitez les conserver séparées, le processus est le même, sauf que vous créez une base de données supplémentaire pour chaque environnement et que vous sélectionnez la chaîne de destination correcte pour chaque base de données lorsque vous créez le profil de publication.

Dans cette section du didacticiel, vous allez créer une application Web et une base de données à utiliser pour l’environnement intermédiaire, et vous allez effectuer un déploiement dans un environnement intermédiaire et de test avant de créer et de déployer dans l’environnement de production.

> [!NOTE]
> Les étapes suivantes montrent comment créer une application Web dans Azure App Service à l’aide du portail de gestion Azure. Dans la dernière version du kit de développement logiciel (SDK) Azure, vous pouvez également effectuer cette opération sans quitter Visual Studio, à l’aide de Explorateur de serveurs. Dans Visual Studio 2013, vous pouvez également créer une application Web directement à partir de la boîte de dialogue publier. Pour plus d’informations, consultez [créer une application web ASP.net dans Azure App service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)

1. Dans le [portail de gestion Azure](https://manage.windowsazure.com/), cliquez sur **sites Web**, puis sur **nouveau**.
2. Cliquez sur **site Web**, puis sur **création personnalisée**.

    L’Assistant **nouveau site Web-création personnalisée** s’ouvre. L’Assistant **création personnalisée** vous permet de créer un site Web et une base de données en même temps.
3. Dans l’étape **créer un site Web** de l’Assistant, entrez une chaîne dans la zone **URL** à utiliser comme URL unique pour l’environnement intermédiaire de votre application. Par exemple, entrez ContosoUniversity-staging123 (y compris des nombres aléatoires à la fin pour le rendre unique au cas où l’intermédiaire de ContosoUniversity est pris).

    L’URL complète se composera du texte entré dans cette zone, ainsi que du suffixe affiché en regard de la zone.
4. Dans la liste déroulante **région** , choisissez la région la plus proche de vous.

    Ce paramètre spécifie le centre de données dans lequel votre application Web s’exécutera.
5. Dans la liste déroulante **base de données** , choisissez **créer une nouvelle base de données SQL**.
6. Dans la zone nom de la **chaîne de connexion de base de base** , laissez la valeur par défaut, *DefaultConnection*.
7. Cliquez sur la flèche qui pointe vers la droite au bas de la zone.

    L’illustration suivante montre la boîte de dialogue **créer un site Web** contenant des exemples de valeurs. L’URL et la région que vous avez entrées seront différentes.

    ![Créer une étape de site Web](deploying-to-production/_static/image1.png)

    L’Assistant passe à l’étape **Specify database settings** .
8. Dans la zone **nom** , entrez *ContosoUniversity* plus un nombre aléatoire pour le rendre unique, par exemple *ContosoUniversity123*.
9. Dans la zone **serveur** , sélectionnez **nouveau SQL Database serveur**.
10. Entrez un nom d’administrateur et un mot de passe.

    Vous n’entrez pas de nom et de mot de passe existants ici. Vous entrez de nouveaux nom et mot de passe que vous définissez maintenant pour les utiliser ultérieurement lorsque vous accédez à la base de données.
11. Dans la zone **région** , choisissez la même région que celle que vous avez choisie pour l’application Web.

    Le fait de conserver le serveur Web et le serveur de base de données dans la même région vous offre les meilleures performances et réduit les dépenses.
12. Cliquez sur la coche en bas de la zone pour indiquer que vous avez terminé.

    L’illustration suivante montre la boîte de dialogue **spécifier les paramètres de base de données** avec des exemples de valeurs. Les valeurs que vous avez entrées peuvent être différentes.

    ![Étape des paramètres de base de données du nouveau site Web-Assistant créer avec base de données](deploying-to-production/_static/image2.png)

    La Portail de gestion retourne à la page sites Web, et la colonne **État** indique que l’application Web est en cours de création. Après un certain temps (généralement inférieur à une minute), la colonne **État** indique que l’application Web a été créée avec succès. Dans la barre de navigation de gauche, le nombre d’applications Web que vous avez dans votre compte apparaît en regard de l’icône de **sites Web** , et le nombre de bases de données s’affiche en regard de l’icône **bases de données SQL** .

    ![Page sites Web de Portail de gestion, site Web créé](deploying-to-production/_static/image3.png)

    Le nom de votre application Web sera différent de l’exemple d’application dans l’illustration.

## <a name="deploy-the-application-to-staging"></a>Déployer l’application dans un environnement intermédiaire

Maintenant que vous avez créé une application Web et une base de données pour l’environnement intermédiaire, vous pouvez y déployer le projet.

> [!NOTE]
> Ces instructions indiquent comment créer un profil de publication en téléchargeant un fichier *. publishsettings* , qui fonctionne non seulement pour Azure, mais également pour les fournisseurs d’hébergement tiers. Le dernier Kit de développement logiciel (SDK) Azure vous permet également de vous connecter directement à Azure à partir de Visual Studio et de choisir parmi une liste d’applications Web dont vous disposez dans votre compte Azure. Dans Visual Studio 2013, vous pouvez vous connecter à Azure à partir de la boîte de dialogue publier sur le **Web** ou de la fenêtre **Explorateur de serveurs** . Pour plus d’informations, consultez [créer une application web ASP.net dans Azure App service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

### <a name="download-the-publishsettings-file"></a>Télécharger le fichier. publishsettings

1. Cliquez sur le nom de l’application Web que vous venez de créer.

    ![Cliquez sur le site pour accéder au tableau de bord.](deploying-to-production/_static/image4.png)
2. Sous **Aperçu rapide** de l’onglet **tableau de bord** , cliquez sur Télécharger le profil de **publication**.

    ![Télécharger le lien du profil de publication](deploying-to-production/_static/image5.png)

    Cette étape télécharge un fichier qui contient tous les paramètres dont vous avez besoin pour déployer une application sur votre application Web. Vous allez importer ce fichier dans Visual Studio pour ne pas avoir à entrer ces informations manuellement.
3. Enregistrez le fichier *. publishsettings* dans un dossier auquel vous pouvez accéder à partir de Visual Studio.

    ![enregistrement du fichier. publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Sécurité : le fichier *. publishsettings* contient vos informations d’identification (non codées) utilisées pour gérer vos abonnements et services Azure. Pour des raisons de sécurité, il est recommandé de stocker ce fichier temporairement en dehors de vos répertoires sources (par exemple, dans le dossier Bibliothèques\Documents), puis de le supprimer une fois l'importation terminée. Un utilisateur malveillant qui accède au fichier *. publishsettings* peut modifier, créer et supprimer vos services Azure.

### <a name="create-a-publish-profile"></a>Créer un profil de publication

1. Dans Visual Studio, cliquez avec le bouton droit sur le projet ContosoUniversity dans **Explorateur de solutions** , puis sélectionnez **publier** dans le menu contextuel.

    L'Assistant **Publier le site Web** s'ouvre.
2. Cliquez sur l’onglet **Profil**.
3. Cliquez sur **Importer**.
4. Accédez au fichier *. publishsettings* que vous avez téléchargé précédemment, puis cliquez sur **ouvrir**.

    ![Boîte de dialogue Importer les paramètres de publication](deploying-to-production/_static/image7.png)
5. Dans l’onglet **connexion** , cliquez sur **valider la connexion** pour vous assurer que les paramètres sont corrects.

    Une fois la connexion validée, une coche verte s’affiche en regard du bouton **valider la connexion** .

    Pour certains fournisseurs d’hébergement, lorsque vous cliquez sur **valider la connexion**, une boîte de dialogue **erreur de certificat** peut s’afficher. Si vous le faites, vérifiez que le nom du serveur correspond à ce que vous attendez. Si le nom du serveur est correct, sélectionnez **enregistrer ce certificat pour les futures sessions de Visual Studio** , puis cliquez sur **accepter**. (Cette erreur signifie que le fournisseur d’hébergement a choisi d’éviter les dépenses liées à l’achat d’un certificat SSL pour l’URL vers laquelle vous effectuez le déploiement. Si vous préférez établir une connexion sécurisée à l’aide d’un certificat valide, contactez votre fournisseur d’hébergement.)
6. Cliquez sur **Next**.

    ![icône connexion réussie et bouton suivant dans l’onglet connexion](deploying-to-production/_static/image8.png)
7. Dans l’onglet **paramètres** , développez **options de publication de fichiers**, puis sélectionnez **exclure des fichiers du dossier de données de l’application\_** .

    Pour plus d’informations sur les autres options disponibles sous **options de publication de fichier**, consultez le didacticiel [déploiement sur IIS](deploying-to-iis.md) . La capture d’écran qui montre le résultat de cette étape et les étapes de configuration de base de données suivantes se trouve à la fin des étapes de configuration de la base de données.
8. Sous **DefaultConnection** dans la section **bases de données** , configurez le déploiement de base de données pour la base de données d’appartenance.
9. 1. Sélectionnez **mettre à jour la base de données**.

        La zone **chaîne de connexion distante** située directement sous **DefaultConnection** est renseignée avec la chaîne de connexion du fichier. publishsettings. La chaîne de connexion comprend SQL Server informations d’identification, qui sont stockées en texte brut dans le fichier *. pubxml* . Si vous préférez ne pas les stocker de manière permanente, vous pouvez les supprimer du profil de publication après le déploiement de la base de données et les stocker dans Azure à la place. Pour plus d’informations, consultez [Comment garder vos chaînes de connexion de base de données ASP.NET sécurisées lors du déploiement sur Azure à partir de la source](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) sur le blog de Scott Hanselman.
      2. Cliquez sur **configurer les mises à jour de la base de données**.
      3. Dans la boîte de dialogue **configurer les mises à jour de la base de données** , cliquez sur **Ajouter un script SQL**.
      4. Dans la zone **Ajouter un script SQL** , accédez au script *ASPNET-Data-prod. SQL* que vous avez enregistré précédemment dans le dossier de la solution, puis cliquez sur **ouvrir**.
      5. Fermez la boîte **de dialogue Configurer les mises à jour de la base de données** .
10. Sous **SchoolContext** dans la section **bases de données** , sélectionnez **exécuter migrations code First (s’exécute au démarrage de l’application)** .

    Visual Studio affiche **exécuter migrations code First** au lieu de **mettre à jour la base de données** pour les classes `DbContext`. Si vous souhaitez utiliser le fournisseur dbDacFx au lieu des migrations pour déployer une base de données à laquelle vous accédez à l’aide d’une classe `DbContext`, consultez [Comment faire déployer une base de données Code First sans migrations ?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) dans le Forum aux questions sur le déploiement Web pour Visual Studio et ASP.net sur MSDN.

    L’onglet **paramètres** ressemble maintenant à l’exemple suivant :

    ![Onglet Paramètres pour la mise en lots](deploying-to-production/_static/image9.png)
11. Pour enregistrer le profil et le renommer en *intermédiaire*, procédez comme suit :

    1. Cliquez sur l’onglet **Profil** , puis sur **gérer les profils**.
    2. L’importation a créé deux nouveaux profils, un pour FTP et un pour Web Deploy. Vous avez configuré le profil de Web Deploy : renommez ce profil en *intermédiaire*.

        ![Renommer le profil en intermédiaire](deploying-to-production/_static/image10.png)
    3. Fermez la boîte de dialogue **modifier les profils de publication Web** .
    4. Fermez l’Assistant **publier le site Web** .

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Configurer une transformation de profil de publication pour l’indicateur d’environnement

> [!NOTE]
> Cette section montre comment configurer une transformation Web. config pour l’indicateur d’environnement. Étant donné que l’indicateur se trouve dans l’élément `<appSettings>`, vous disposez d’une autre solution pour spécifier la transformation lorsque vous effectuez le déploiement sur Azure App Service. Pour plus d’informations, consultez [spécification des paramètres Web. config dans Azure](web-config-transformations.md#watransforms).

1. Dans **Explorateur de solutions**, développez **Propriétés**, puis **PublishProfiles**.
2. Cliquez avec le bouton droit sur *Staging. pubxml*, puis cliquez sur **Ajouter une transformation de configuration**.

    ![Ajouter une transformation de configuration pour la mise en lots](deploying-to-production/_static/image11.png)

    Visual Studio crée le fichier de transformation *Web. Staging. config* et l’ouvre.
3. Dans le fichier de transformation *Web. Staging. config* , insérez le code suivant immédiatement après la balise d’ouverture `configuration`.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Lorsque vous utilisez le profil de publication intermédiaire, cette transformation définit l’indicateur d’environnement sur « prod ». Dans l’application Web déployée, vous ne verrez aucun suffixe tel que « (dev) » ou « (test) » après l’en-tête « Contoso University » H1.
4. Cliquez avec le bouton droit sur le fichier *Web. Staging. config* , puis cliquez sur Aperçu de la **transformation** pour vous assurer que la transformation que vous avez codée produit les modifications attendues.

    La fenêtre d' **aperçu de Web. config** affiche le résultat de l’application des transformations *Web. Release. config* et des transformations *Web. Staging. config* .

### <a name="prevent-public-use-of-the-test-app"></a>Empêcher l’utilisation publique de l’application de test

Une considération importante pour l’application intermédiaire est qu’elle est en ligne sur Internet, mais que vous ne voulez pas que le public l’utilise. Pour réduire la probabilité que les gens la trouvent et l’utilisent, vous pouvez utiliser une ou plusieurs des méthodes suivantes :

- Définir des règles de pare-feu qui autorisent l’accès à l’application intermédiaire uniquement à partir d’adresses IP que vous utilisez pour tester la mise en lots.
- Utilisez une URL obscurcie qui serait impossible à deviner.
- Créez un fichier *robots. txt* pour vous assurer que les moteurs de recherche n’analyseront pas l’application de test et les liens de rapport vers ce fichier dans les résultats de la recherche.

La première de ces méthodes est la plus efficace, mais n’est pas traitée dans ce didacticiel, car elle nécessiterait le déploiement vers un service Cloud Azure au lieu de Azure App Service. Pour plus d’informations sur les services Cloud et les restrictions d’adresse IP dans Azure, consultez [options d’hébergement de calcul fournies par Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) et [empêcher des adresses IP spécifiques d’accéder à un rôle Web](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Si vous déployez sur un fournisseur d’hébergement tiers, contactez le fournisseur pour savoir comment implémenter des restrictions d’adresse IP.

Pour ce didacticiel, vous allez créer un fichier *robots. txt* .

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet ContosoUniversity, puis cliquez sur **Ajouter un nouvel élément**.
2. Créez un **fichier texte** nommé *robots. txt*et placez-y le texte suivant :

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    La ligne `User-agent` indique aux moteurs de recherche que les règles du fichier s’appliquent à tous les robots d’indexation (robots) du moteur de recherche et que la ligne `Disallow` spécifie qu’aucune page du site ne doit être analysée.

    Vous souhaitez que les moteurs de recherche cataloguent votre application de production. vous devez donc exclure ce fichier du déploiement de production. Pour ce faire, vous configurez un paramètre dans le profil de publication de production lorsque vous le créez.

### <a name="deploy-to-staging"></a>Déployer vers un environnement intermédiaire

1. Ouvrez l’Assistant **publier le site Web** en cliquant avec le bouton droit sur le projet Contoso University et en cliquant sur **publier**.
2. Assurez-vous que le profil **intermédiaire** est sélectionné.
3. Cliquez sur **Publier**.

    La fenêtre **Output** indique les actions de déploiement entreprises et signale la réussite du déploiement. Le navigateur par défaut s’ouvre automatiquement à l’URL de l’application Web déployée.

## <a name="test-in-the-staging-environment"></a>Test dans l’environnement intermédiaire

Notez que l’indicateur d’environnement est absent (il n’y a aucun « (test) » ou « (dev) » après l’en-tête H1, qui indique que la transformation *Web. config* de l’indicateur d’environnement a réussi.

![Mise en lots des pages d’hébergement](deploying-to-production/_static/image12.png)

Exécutez la page **students** pour vérifier que la base de données déployée ne dispose d’aucun étudiant.

Exécutez la page des **enseignants** pour vérifier que code First amorcé la base de données avec les données de l’instructeur :

Sélectionnez **Ajouter des élèves** dans le menu **élèves** , ajoutez un étudiant, puis affichez le nouvel étudiant dans la page des **étudiants** pour vérifier que vous pouvez écrire correctement dans la base de données.

Sur la page **cours** , cliquez sur **mettre à jour les crédits**. La page **mettre à jour les crédits** requiert des autorisations d’administrateur, donc la page **de connexion** s’affiche. Entrez les informations d’identification du compte d’administrateur que vous avez créées précédemment (« admin » et « prodpwd »). La page **mettre à jour les crédits** s’affiche, qui vérifie que le compte d’administrateur que vous avez créé dans le didacticiel précédent a été correctement déployé dans l’environnement de test.

Demandez une URL non valide pour provoquer une erreur que ELMAH effectue le suivi, puis demandez le rapport d’erreurs ELMAH. Si vous déployez sur un fournisseur d’hébergement tiers, vous constaterez probablement que le rapport est vide pour la raison pour laquelle il était vide dans le didacticiel précédent. Vous devrez utiliser les outils de gestion de compte du fournisseur d’hébergement pour configurer les autorisations de dossier afin de permettre à ELMAH d’écrire dans le dossier du journal.

L’application que vous avez créée est maintenant exécutée dans le Cloud dans une application Web comme ce que vous allez utiliser pour la production. Dans la mesure où tout fonctionne correctement, l’étape suivante consiste à déployer en production.

## <a name="deploy-to-production"></a>Déployer en production

Le processus de création d’une application Web de production et de déploiement en production est le même que pour la mise en lots, sauf que vous devez exclure le format *robots. txt* du déploiement. Pour ce faire, vous allez modifier le fichier de profil de publication.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Créer l’environnement de production et le profil de publication de production

1. Créez l’application Web et la base de données de production dans Azure, en suivant la même procédure que celle utilisée pour la mise en lots.

    Lorsque vous créez la base de données, vous pouvez choisir de la placer sur le serveur que vous avez créé précédemment ou de créer un nouveau serveur.
2. Téléchargez le fichier *. publishsettings* .
3. Créez le profil de publication en important le fichier production *. publishsettings* en suivant la procédure que vous avez utilisée pour la mise en lots.

    N’oubliez pas de configurer le script de déploiement de données sous **DefaultConnection** dans la section **bases de données** de l’onglet **paramètres** .
4. Renommez le profil de publication en *production*.
5. Configurez une transformation de profil de publication pour l’indicateur d’environnement, en suivant la procédure que vous avez utilisée pour la mise en lots.

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Modifiez le fichier. pubxml pour exclure robots. txt

Les fichiers de profil de publication sont nommés &lt;ProfileName&gt; *. pubxml* et se trouvent dans le dossier *PublishProfiles* . Le dossier *PublishProfiles* se trouve sous le dossier *Propriétés* d' C# un projet d’application Web, sous le dossier *mon projet* dans un projet d’application web VB, ou sous le dossier *application\_données* dans un projet d’application Web. Chaque fichier *. pubxml* contient des paramètres qui s’appliquent à un profil de publication. Les valeurs que vous entrez dans l’Assistant publier le site Web sont stockées dans ces fichiers et vous pouvez les modifier pour créer ou modifier des paramètres qui ne sont pas mis à disposition dans l’interface utilisateur de Visual Studio.

Par défaut, les fichiers *. pubxml* sont inclus dans le projet lorsque vous créez un profil de publication, mais vous pouvez les exclure du projet et Visual Studio les utilisera toujours. Visual Studio recherche les fichiers *. pubxml* dans le dossier *PublishProfiles* , qu’ils soient inclus ou non dans le projet.

Chaque fichier *. pubxml* contient un fichier *. pubxml. User* . Le fichier *. pubxml. User* contient le mot de passe chiffré si vous avez sélectionné l’option **enregistrer le mot de passe** . par défaut, il est exclu du projet.

Un fichier *. pubxml* contient les paramètres qui se rapportent à un profil de publication spécifique. Si vous souhaitez configurer des paramètres qui s’appliquent à tous les profils, vous pouvez créer un fichier *. WPP. targets* . Le processus de génération importe ces fichiers dans le fichier projet *. csproj* ou *. vbproj* , de sorte que la plupart des paramètres que vous pouvez configurer dans le fichier projet peuvent être configurés dans ces fichiers. Pour plus d’informations sur les fichiers *. pubxml* et *. WPP. targets* , consultez [procédure : modifier les paramètres de déploiement dans les fichiers de profil de publication (. pubxml) et le fichier. WPP. targets dans les projets Web Visual Studio](https://msdn.microsoft.com/library/ff398069.aspx).

1. Dans **Explorateur de solutions**, développez **Propriétés** et développez **PublishProfiles**.
2. Cliquez avec le bouton droit sur *production. pubxml* , puis cliquez sur **ouvrir**.

    ![Ouvrez le fichier. pubxml](deploying-to-production/_static/image13.png)
3. Cliquez avec le bouton droit sur *production. pubxml* , puis cliquez sur **ouvrir**.
4. Ajoutez les lignes suivantes juste avant l’élément `PropertyGroup` de fermeture :

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Le fichier. pubxml ressemble maintenant à l’exemple suivant :

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Pour plus d’informations sur l’exclusion de fichiers et de dossiers, consultez peux- [je exclure des fichiers ou des dossiers spécifiques du déploiement ?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) dans le **Forum aux questions sur le déploiement Web pour Visual Studio et ASP.net** sur MSDN.

### <a name="deploy-to-production"></a>Déployer en production

1. Ouvrez l' **Assistant publier le site Web** . Assurez-vous que le profil de publication de **production** est sélectionné, puis cliquez sur **Démarrer l’aperçu** sous l’onglet **Aperçu** pour vérifier que le fichier *robots. txt* n’est pas copié dans l’application de production.

    ![Aperçu des fichiers à publier en production](deploying-to-production/_static/image14.png)

    Passez en revue la liste des fichiers qui seront copiés. Vous verrez que tous les fichiers *. cs* , y compris les fichiers *. aspx.cs*, *. aspx.Designer.cs*, *Master.cs*et *Master.Designer.cs* sont omis. Tout ce code a été compilé dans les fichiers *ContosoUniversity. dll* et *ContosoUniversity. pdb* que vous trouverez dans le dossier *bin* . Étant donné que seul le *fichier. dll* est nécessaire pour exécuter l’application, et que vous avez spécifié précédemment que seuls les fichiers nécessaires à l’exécution de l’application doivent être déployés, aucun fichier *. cs* n’a été copié dans l’environnement de destination. Le dossier *obj* et les fichiers *ContosoUniversity. csproj* et *. csproj. User* sont omis pour la même raison.

    Cliquez sur **publier** pour déployer dans l’environnement de production.
2. Testez en production en suivant la même procédure que celle utilisée pour la mise en lots.

    Tout est identique à la mise en lots, à l’exception de l’URL et de l’absence du fichier *robots. txt* .

## <a name="summary"></a>Récapitulatif

Vous avez maintenant déployé et testé votre application Web avec succès et elle est disponible publiquement sur Internet.

![Production de la page d’hébergement](deploying-to-production/_static/image15.png)

Dans le didacticiel suivant, vous allez mettre à jour le code de l’application et déployer la modification dans les environnements de test, intermédiaire et de production.

> [!NOTE]
> Lorsque votre application est en cours d’utilisation dans l’environnement de production, vous devez implémenter un plan de récupération. Autrement dit, vous devez sauvegarder régulièrement vos bases de données de l’application de production vers un emplacement de stockage sécurisé, et vous devez conserver plusieurs générations de ces sauvegardes. Lorsque vous mettez à jour la base de données, vous devez effectuer une copie de sauvegarde immédiatement avant la modification. Ensuite, si vous faites une erreur et que vous ne la Découvrez pas tant que vous ne l’avez pas déployée en production, vous pourrez toujours récupérer la base de données dans l’État où elle se trouvait avant qu’elle ne soit endommagée. Pour en savoir plus, consultez [Sauvegarde et restauration d’une base de données Azure SQL](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> Dans ce didacticiel, l’édition SQL Server sur laquelle vous effectuez le déploiement est Azure SQL Database. Alors que le processus de déploiement est similaire à d’autres éditions de SQL Server, une application de production réelle peut nécessiter du code spécial pour Azure SQL Database dans certains scénarios. Pour plus d’informations, consultez [utilisation de Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) et [choix entre SQL Server et Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Précédent](setting-folder-permissions.md)
> [Suivant](deploying-a-code-update.md)
