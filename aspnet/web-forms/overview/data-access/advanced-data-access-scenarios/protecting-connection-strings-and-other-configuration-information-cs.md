---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: Protection des chaînes de connexion et d’autresC#informations de configuration () | Microsoft Docs
author: rick-anderson
description: Une application ASP.NET stocke généralement les informations de configuration dans un fichier Web. config. Certaines de ces informations sont sensibles et garantissent la protection. Par déf...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 15970bee1e990d3a139673efe12486e08f79814c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74573495"
---
# <a name="protecting-connection-strings-and-other-configuration-information-c"></a>Protection des chaînes de connexion et d’autres informations de configuration (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip) ou [Télécharger le PDF](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> Une application ASP.NET stocke généralement les informations de configuration dans un fichier Web. config. Certaines de ces informations sont sensibles et garantissent la protection. Par défaut, ce fichier n’est pas pris en charge par un visiteur du site Web, mais un administrateur ou un pirate peut accéder au système de fichiers du serveur Web et afficher le contenu du fichier. Dans ce didacticiel, nous avons appris que ASP.NET 2,0 nous permet de protéger des informations sensibles en chiffrant les sections du fichier Web. config.

## <a name="introduction"></a>Introduction

Les informations de configuration pour les applications ASP.NET sont généralement stockées dans un fichier XML nommé `Web.config`. Au cours de ces didacticiels, nous avons mis à jour le `Web.config` à quelques instants. Lors de la création du DataSet typé `Northwind` dans le [premier didacticiel](../introduction/creating-a-data-access-layer-cs.md), par exemple, des informations de chaîne de connexion ont été automatiquement ajoutées à `Web.config` dans la section `<connectionStrings>`. Plus tard, dans le didacticiel sur les [pages maîtres et la navigation](../introduction/master-pages-and-site-navigation-cs.md) dans les sites, nous avons mis à jour manuellement `Web.config`, en ajoutant un élément `<pages>` indiquant que toutes les pages ASP.net de notre projet doivent utiliser le thème `DataWebControls`.

Étant donné que `Web.config` peut contenir des données sensibles telles que des chaînes de connexion, il est important que le contenu de `Web.config` soit protégé et masqué par des observateurs non autorisés. Par défaut, toute requête HTTP adressée à un fichier avec l’extension `.config` est gérée par le moteur ASP.NET, qui retourne le message *ce type de page n’est pas traité* , comme indiqué dans la figure 1. Cela signifie que les visiteurs ne peuvent pas afficher le contenu de vos fichiers `Web.config` en entrant simplement http://www.YourServer.com/Web.config dans la barre d’adresse de leur navigateur.

[![visitant Web. config via un navigateur retourne un message indiquant que ce type de page n’est pas pris en charge](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**Figure 1**: consultation de `Web.config` via un navigateur retourne un message ce type de page n’est pas pris en charge ([cliquez pour afficher l’image en taille réelle](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))

Mais que se passe-t-il si une personne malveillante est en mesure de trouver une autre faille qui lui permet d’afficher le contenu de vos fichiers de `Web.config` ? Qu’est-ce qu’une personne malveillante peut faire avec ces informations et quelles sont les étapes à suivre pour protéger davantage les informations sensibles dans `Web.config`? Heureusement, la plupart des sections de `Web.config` ne contiennent pas d’informations sensibles. Quels dommages peuvent être attaqués par une personne malveillante s’ils savent le nom du thème par défaut utilisé par vos pages ASP.NET ?

Toutefois, certaines sections `Web.config` contiennent des informations sensibles qui peuvent inclure des chaînes de connexion, des noms d’utilisateurs, des mots de passe, des noms de serveur, des clés de chiffrement, etc. Ces informations se trouvent généralement dans les sections `Web.config` suivantes :

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

Dans ce didacticiel, nous allons examiner les techniques permettant de protéger ces informations de configuration sensibles. Comme nous le verrons, le .NET Framework version 2,0 comprend un système de configuration protégée qui permet de chiffrer et de déchiffrer des sections de configuration sélectionnées par programmation.

> [!NOTE]
> Ce didacticiel se termine par un examen des recommandations de Microsoft en matière de connexion à une base de données à partir d’une application ASP.NET. Outre le chiffrement de vos chaînes de connexion, vous pouvez renforcer la sécurité de votre système en vous assurant que vous vous connectez à la base de données de façon sécurisée.

## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Étape 1 : exploration des options de configuration protégées de ASP.NET 2,0 s

ASP.NET 2,0 comprend un système de configuration protégé pour le chiffrement et le déchiffrement des informations de configuration. Cela comprend les méthodes de la .NET Framework qui peuvent être utilisées pour chiffrer ou déchiffrer des informations de configuration. Le système de configuration protégé utilise le [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), qui permet aux développeurs de choisir l’implémentation de chiffrement utilisée.

Le .NET Framework est fourni avec deux fournisseurs de configuration protégée :

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) : utilise l' [algorithme RSA](http://en.wikipedia.org/wiki/Rsa) asymétrique pour le chiffrement et le déchiffrement.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) : utilise l' [API de protection des données (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) Windows pour le chiffrement et le déchiffrement.

Étant donné que le système de configuration protégé implémente le modèle de conception du fournisseur, il est possible de créer votre propre fournisseur de configuration protégée et de l’intégrer à votre application. Pour plus d’informations sur ce processus, consultez [implémentation d’un fournisseur de configuration protégé](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) .

Les fournisseurs RSA et DPAPI utilisent des clés pour leurs routines de chiffrement et de déchiffrement, et ces clés peuvent être stockées au niveau de l’ordinateur ou de l’utilisateur. Les clés au niveau de l’ordinateur sont idéales pour les scénarios où l’application Web s’exécute sur son propre serveur dédié ou s’il existe plusieurs applications sur un serveur qui doivent partager des informations chiffrées. Les clés au niveau de l’utilisateur sont une option plus sécurisée dans les environnements d’hébergement partagés où d’autres applications sur le même serveur ne doivent pas être en mesure de déchiffrer les sections de configuration protégées de votre application.

Dans ce didacticiel, nos exemples vont utiliser le fournisseur DPAPI et les clés au niveau de l’ordinateur. Plus précisément, nous examinerons le chiffrement de la section `<connectionStrings>` dans `Web.config`, même si le système de configuration protégé peut être utilisé pour chiffrer la plupart des `Web.config` section. Pour plus d’informations sur l’utilisation des clés au niveau de l’utilisateur ou de l’utilisation du fournisseur RSA, consultez les ressources dans la section lectures supplémentaires à la fin de ce didacticiel.

> [!NOTE]
> Les fournisseurs `RSAProtectedConfigurationProvider` et `DPAPIProtectedConfigurationProvider` sont enregistrés dans le fichier `machine.config` avec les noms des fournisseurs `RsaProtectedConfigurationProvider` et `DataProtectionConfigurationProvider`, respectivement. Lors du chiffrement ou du déchiffrement des informations de configuration, nous devons fournir le nom de fournisseur approprié (`RsaProtectedConfigurationProvider` ou `DataProtectionConfigurationProvider`) plutôt que le nom de type réel (`RSAProtectedConfigurationProvider` et `DPAPIProtectedConfigurationProvider`). Vous pouvez trouver le fichier `machine.config` dans le dossier `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG`.

## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Étape 2 : chiffrement et déchiffrement des sections de configuration par programmation

Avec quelques lignes de code, nous pouvons chiffrer ou déchiffrer une section de configuration particulière à l’aide d’un fournisseur spécifié. Le code, comme nous le verrons bientôt, doit simplement référencer par programmation la section de configuration appropriée, appeler sa méthode `ProtectSection` ou `UnprotectSection`, puis appeler la méthode `Save` pour rendre les modifications persistantes. En outre, le .NET Framework comprend un utilitaire de ligne de commande utile qui peut chiffrer et déchiffrer les informations de configuration. Nous allons explorer cet utilitaire de ligne de commande à l’étape 3.

Pour illustrer la protection des informations de configuration par programme, créez une page ASP.NET qui comprend des boutons de chiffrement et de déchiffrement de la section `<connectionStrings>` dans `Web.config`.

Commencez par ouvrir la page `EncryptingConfigSections.aspx` dans le dossier `AdvancedDAL`. Faites glisser un contrôle TextBox de la boîte à outils vers le concepteur, en affectant à sa propriété `ID` la valeur `WebConfigContents`, à sa propriété `TextMode` la valeur `MultiLine`, et à ses propriétés `Width` et `Rows` la valeur 95% et 15, respectivement. Ce contrôle TextBox affiche le contenu de `Web.config` nous permettant de voir rapidement si le contenu est chiffré ou non. Bien sûr, dans une application réelle, vous ne voudriez jamais afficher le contenu de `Web.config`.

Sous la zone de texte, ajoutez deux contrôles Button nommés `EncryptConnStrings` et `DecryptConnStrings`. Définissez leurs propriétés Text pour chiffrer les chaînes de connexion et déchiffrer les chaînes de connexion.

À ce stade, votre écran doit ressembler à la figure 2.

[![ajouter une zone de texte et deux contrôles Web bouton à la page](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**Figure 2**: ajouter une zone de texte et deux contrôles Web bouton à la page ([cliquez pour afficher l’image en taille réelle](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))

Ensuite, nous devons écrire du code qui charge et affiche le contenu de `Web.config` dans la zone de texte `WebConfigContents` lorsque la page est chargée pour la première fois. Ajoutez le code suivant à la classe code-behind de la page. Ce code ajoute une méthode nommée `DisplayWebConfig` et l’appelle à partir du gestionnaire d’événements `Page_Load` lorsque `Page.IsPostBack` est `false`:

[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

La méthode `DisplayWebConfig` utilise la [classe`File`](https://msdn.microsoft.com/library/system.io.file.aspx) pour ouvrir le fichier d' `Web.config` de l’application, la [classe`StreamReader`](https://msdn.microsoft.com/library/system.io.streamreader.aspx) pour lire son contenu dans une chaîne, et la [classe`Path`](https://msdn.microsoft.com/library/system.io.path.aspx) pour générer le chemin d’accès physique au fichier `Web.config`. Ces trois classes se trouvent toutes dans l' [espace de noms`System.IO`](https://msdn.microsoft.com/library/system.io.aspx). Par conséquent, vous devez ajouter une instruction `using` `System.IO` en haut de la classe code-behind ou, sinon, préfixer ces noms de classe avec `System.IO.`.

Ensuite, nous devons ajouter des gestionnaires d’événements pour les deux contrôles Button `Click` Events et ajouter le code nécessaire pour chiffrer et déchiffrer la section `<connectionStrings>` à l’aide d’une clé au niveau de l’ordinateur avec le fournisseur DPAPI. Dans le concepteur, double-cliquez sur chacun des boutons pour ajouter un gestionnaire d’événements `Click` dans la classe code-behind, puis ajoutez le code suivant :

[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

Le code utilisé dans les deux gestionnaires d’événements est quasiment identique. Ils commencent tous les deux par obtenir des informations sur le fichier de `Web.config` de l’application actuel par le biais de la [méthode`OpenWebConfiguration`](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx) [`WebConfigurationManager`](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) . Cette méthode retourne le fichier de configuration Web pour le chemin d’accès virtuel spécifié. Ensuite, la section `Web.config` file s `<connectionStrings>` est accessible via la méthode [`Configuration` Class](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [`GetSection(sectionName)`](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), qui retourne un objet [`ConfigurationSection`](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) .

L’objet `ConfigurationSection` inclut une [propriété`SectionInformation`](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) qui fournit des informations et des fonctionnalités supplémentaires concernant la section de configuration. Comme le montre le code ci-dessus, nous pouvons déterminer si la section de configuration est chiffrée en vérifiant la propriété `SectionInformation` propriété s `IsProtected`. En outre, la section peut être chiffrée ou déchiffrée par le biais des méthodes `SectionInformation` `ProtectSection(provider)` et `UnprotectSection`.

La méthode `ProtectSection(provider)` accepte comme entrée une chaîne spécifiant le nom du fournisseur de configuration protégée à utiliser lors du chiffrement. Dans le gestionnaire d’événements du bouton `EncryptConnString`, nous transmettons DataProtectionConfigurationProvider dans la méthode `ProtectSection(provider)` afin que le fournisseur DPAPI soit utilisé. La méthode `UnprotectSection` peut déterminer le fournisseur qui a été utilisé pour chiffrer la section de configuration et ne nécessite donc pas de paramètres d’entrée.

Après avoir appelé la méthode `ProtectSection(provider)` ou `UnprotectSection`, vous devez appeler la méthode `Configuration` [`Save`](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) des objets pour rendre les modifications persistantes. Une fois que les informations de configuration ont été chiffrées ou déchiffrées et que les modifications ont été enregistrées, nous appelons `DisplayWebConfig` pour charger le contenu `Web.config` mis à jour dans le contrôle TextBox.

Une fois que vous avez entré le code ci-dessus, testez-le en visitant la page de `EncryptingConfigSections.aspx` via un navigateur. Vous devez initialement voir une page qui répertorie le contenu de `Web.config` avec la section `<connectionStrings>` affichée en texte brut (voir figure 3).

[![ajouter une zone de texte et deux contrôles Web bouton à la page](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**Figure 3**: ajouter une zone de texte et deux contrôles Web bouton à la page ([cliquez pour afficher l’image en taille réelle](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))

Cliquez maintenant sur le bouton Encrypt Connection Strings. Si la validation de la demande est activée, le balisage publié à partir de la zone de texte `WebConfigContents` produira un `HttpRequestValidationException`, qui affiche le message, une valeur de `Request.Form` potentiellement dangereuse a été détectée à partir du client. La validation de la demande, qui est activée par défaut dans ASP.NET 2,0, interdit les publications qui incluent du code HTML non codé et est conçue pour empêcher les attaques par injection de script. Cette vérification peut être désactivée au niveau de la page ou de l’application. Pour la désactiver pour cette page, définissez le paramètre `ValidateRequest` sur `false` dans la directive `@Page`. La directive `@Page` se trouve en haut du balisage déclaratif de la page.

[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

Pour plus d’informations sur la validation de la demande, son objectif, comment la désactiver au niveau de la page et de l’application, ainsi que comment encoder le balisage HTML, consultez [validation des demandes-prévention des attaques de script](../../../../whitepapers/request-validation.md).

Après la désactivation de la validation de la demande pour la page, essayez de cliquer à nouveau sur le bouton chiffrer les chaînes de connexion. Lors de la publication (postback), le fichier de configuration est accessible et sa section `<connectionStrings>` chiffrée à l’aide du fournisseur DPAPI. La zone de texte est ensuite mise à jour pour afficher le nouveau contenu `Web.config`. Comme le montre la figure 4, les informations de `<connectionStrings>` sont désormais chiffrées.

[![cliquant sur le bouton Encrypt Connection Strings, vous chiffrez la section &lt;connectionString&gt;](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**Figure 4**: cliquez sur le bouton chiffrer les chaînes de connexion pour chiffrer la section `<connectionString>` ([cliquez pour afficher l’image en taille réelle](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))

La section de `<connectionStrings>` chiffrée générée sur mon ordinateur suit, bien qu’une partie du contenu de l’élément `<CipherData>` ait été supprimée par souci de concision :

[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> L’élément `<connectionStrings>` spécifie le fournisseur utilisé pour effectuer le chiffrement (`DataProtectionConfigurationProvider`). Ces informations sont utilisées par la méthode `UnprotectSection` lorsque l’utilisateur clique sur le bouton déchiffrer les chaînes de connexion.

Lorsque les informations de la chaîne de connexion sont accessibles à partir de `Web.config` par le code que nous écrivons, à partir d’un contrôle SqlDataSource ou au code généré automatiquement à partir des TableAdapters dans nos DataSets typés, il est automatiquement déchiffré. En bref, nous n’avons pas besoin d’ajouter de code ou de logique supplémentaire pour déchiffrer la section de `<connectionString>` chiffrée. Pour illustrer cela, consultez l’un des didacticiels précédents pour l’instant, par exemple le didacticiel affichage simple de la section Reporting de base (`~/BasicReporting/SimpleDisplay.aspx`). Comme le montre la figure 5, le didacticiel fonctionne exactement comme prévu, ce qui indique que les informations de chaîne de connexion chiffrées sont automatiquement déchiffrées par la page ASP.NET.

[![la couche d’accès aux données déchiffre automatiquement les informations de chaîne de connexion](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**Figure 5**: la couche d’accès aux données déchiffre automatiquement les informations de la chaîne de connexion ([cliquez pour afficher l’image en taille réelle](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))

Pour rétablir la section de `<connectionStrings>` dans sa représentation en texte brut, cliquez sur le bouton déchiffrer les chaînes de connexion. Lors de la publication (postback), vous devez voir les chaînes de connexion dans `Web.config` en texte brut. À ce stade, votre écran doit ressembler à celui qui s’est affiché lors de la première visite de cette page (voir la figure 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnet_regiisexe"></a>Étape 3 : chiffrement des sections de configuration à l’aide de`aspnet_regiis.exe`

Le .NET Framework comprend un large éventail d’outils en ligne de commande dans le dossier `$WINDOWS$\Microsoft.NET\Framework\version\`. Dans le didacticiel [utilisation des dépendances de cache SQL](../caching-data/using-sql-cache-dependencies-cs.md) , par exemple, nous avons vu comment utiliser l’outil de ligne de commande `aspnet_regsql.exe` pour ajouter l’infrastructure nécessaire pour les dépendances de cache SQL. L’outil [d’inscription IIS ASP.net (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx)est un autre outil en ligne de commande utile dans ce dossier. Comme son nom l’indique, l’outil d’inscription IIS ASP.NET est principalement utilisé pour inscrire une application ASP.NET 2,0 avec le serveur Web de niveau professionnel de Microsoft, IIS. Outre ses fonctionnalités IIS, l’outil d’inscription IIS ASP.NET peut également être utilisé pour chiffrer ou déchiffrer des sections de configuration spécifiées dans `Web.config`.

L’instruction suivante montre la syntaxe générale utilisée pour chiffrer une section de configuration avec l’outil de ligne de commande `aspnet_regiis.exe` :

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*section de* configuration à chiffrer (par exemple, connectionStrings), le *répertoire physique\_* est le chemin d’accès physique complet au répertoire racine de l’application Web, et le *fournisseur* est le nom du fournisseur de configuration protégée à utiliser (par exemple, DataProtectionConfigurationProvider). Sinon, si l’application Web est inscrite dans IIS, vous pouvez entrer le chemin d’accès virtuel au lieu du chemin d’accès physique à l’aide de la syntaxe suivante :

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

L’exemple de `aspnet_regiis.exe` suivant chiffre la section `<connectionStrings>` à l’aide du fournisseur DPAPI avec une clé au niveau de l’ordinateur :

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

De même, l’outil en ligne de commande `aspnet_regiis.exe` peut être utilisé pour déchiffrer des sections de configuration. Au lieu d’utiliser le commutateur `-pef`, utilisez `-pdf` (ou au lieu de `-pe`, utilisez `-pd`). Notez également que le nom du fournisseur n’est pas nécessaire lors du déchiffrement.

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> Étant donné que nous utilisons le fournisseur DPAPI, qui utilise des clés spécifiques à l’ordinateur, vous devez exécuter `aspnet_regiis.exe` à partir de l’ordinateur à partir duquel les pages Web sont servies. Par exemple, si vous exécutez ce programme en ligne de commande à partir de votre ordinateur de développement local, puis chargez le fichier Web. config chiffré sur le serveur de production, le serveur de production ne sera pas en mesure de déchiffrer les informations de la chaîne de connexion, car il a été chiffré utilisation de clés spécifiques à votre ordinateur de développement. Le fournisseur RSA n’a pas cette limitation, car il est possible d’exporter les clés RSA vers un autre ordinateur.

## <a name="understanding-database-authentication-options"></a>Fonctionnement des options d’authentification de base de données

Avant qu’une application puisse émettre des requêtes `SELECT`, `INSERT`, `UPDATE`ou `DELETE` vers une base de données Microsoft SQL Server, la base de données doit d’abord identifier le demandeur. Ce processus est appelé *authentification* et SQL Server fournit deux méthodes d’authentification :

- **Authentification Windows** : le processus sous lequel l’application s’exécute est utilisé pour communiquer avec la base de données. Lors de l’exécution d’une application ASP.NET à l’aide de Visual Studio 2005 s Serveur de développement ASP.NET, l’application ASP.NET utilise l’identité de l’utilisateur actuellement connecté. Pour les applications ASP.NET sur Microsoft Internet Information Server (IIS), les applications ASP.NET prennent généralement l’identité de `domainName``\MachineName` ou `domainName``\NETWORK SERVICE`, bien que cela puisse être personnalisé.
- **Authentification SQL** : les valeurs d’ID d’utilisateur et de mot de passe sont fournies en tant qu’informations d’identification pour l’authentification. Avec l’authentification SQL, l’ID d’utilisateur et le mot de passe sont fournis dans la chaîne de connexion.

L’authentification Windows est préférable à l’authentification SQL, car elle est plus sécurisée. Avec l’authentification Windows, la chaîne de connexion est gratuite à partir d’un nom d’utilisateur et d’un mot de passe, et si le serveur Web et le serveur de base de données résident sur deux ordinateurs différents, les informations d’identification ne sont pas envoyées sur le réseau en texte brut. Avec l’authentification SQL, toutefois, les informations d’identification d’authentification sont codées en dur dans la chaîne de connexion et transmises du serveur Web vers le serveur de base de données en texte brut.

Ces didacticiels ont utilisé l’authentification Windows. Vous pouvez indiquer le mode d’authentification utilisé en inspectant la chaîne de connexion. La chaîne de connexion dans `Web.config` pour nos didacticiels a été :

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

La sécurité intégrée = true et l’absence d’un nom d’utilisateur et d’un mot de passe indiquent que l’authentification Windows est utilisée. Dans certaines chaînes de connexion, le terme connexion approuvée = oui ou sécurité intégrée = SSPI est utilisé à la place de sécurité intégrée = true, mais les trois indiquent l’utilisation de l’authentification Windows.

L’exemple suivant illustre une chaîne de connexion qui utilise l’authentification SQL. Notez les informations d’identification incorporées dans la chaîne de connexion :

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Imaginez qu’une personne malveillante parvient à afficher le fichier de `Web.config` de votre application. Si vous utilisez l’authentification SQL pour vous connecter à une base de données accessible via Internet, l’attaquant peut utiliser cette chaîne de connexion pour se connecter à votre base de données par le biais de SQL Management Studio ou de pages ASP.NET sur son propre site Web. Pour aider à atténuer cette menace, chiffrez les informations de chaîne de connexion dans `Web.config` à l’aide du système de configuration protégé.

> [!NOTE]
> Pour plus d’informations sur les différents types d’authentification disponibles dans SQL Server, consultez [création d’Applications ASP.NET sécurisées : authentification, autorisation et communication sécurisée](https://msdn.microsoft.com/library/aa302392.aspx). Pour obtenir d’autres exemples de chaînes de connexion illustrant les différences entre la syntaxe d’authentification Windows et SQL, consultez [connectionStrings.com](http://www.connectionstrings.com/).

## <a name="summary"></a>Récapitulatif

Par défaut, les fichiers avec une extension de `.config` dans une application ASP.NET ne sont pas accessibles par le biais d’un navigateur. Ces types de fichiers ne sont pas renvoyés, car ils peuvent contenir des informations sensibles, telles que des chaînes de connexion à la base de données, des noms d’utilisateur et des mots de passe, etc. Le système de configuration protégé dans .NET 2,0 permet de mieux protéger les informations sensibles en autorisant le chiffrement des sections de configuration spécifiées. Il existe deux fournisseurs de configuration protégée intégrés : l’un utilisant l’algorithme RSA et l’autre qui utilise l’API de protection des données (DPAPI) Windows.

Dans ce didacticiel, nous avons vu comment chiffrer et déchiffrer les paramètres de configuration à l’aide du fournisseur DPAPI. Cela peut être accompli à la fois par programme, comme nous l’avons vu à l’étape 2, ainsi qu’à l’aide de l’outil de ligne de commande `aspnet_regiis.exe`, traité à l’étape 3. Pour plus d’informations sur l’utilisation des clés au niveau de l’utilisateur ou sur le fournisseur RSA, consultez les ressources dans la section lectures supplémentaires.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Création d’une application ASP.NET sécurisée : authentification, autorisation et communication sécurisée](https://msdn.microsoft.com/library/aa302392.aspx)
- [Chiffrement des informations de configuration dans les applications ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Chiffrement des valeurs de `Web.config` dans ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Comment : chiffrer des sections de configuration dans ASP.NET 2,0 à l’aide de DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Comment : chiffrer des sections de configuration dans ASP.NET 2,0 à l’aide de RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [API de configuration dans .NET 2,0](http://www.odetocode.com/Articles/418.aspx)
- [Protection des données Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Teresa Murphy et Randy Schmidt. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
> [Suivant](debugging-stored-procedures-cs.md)
