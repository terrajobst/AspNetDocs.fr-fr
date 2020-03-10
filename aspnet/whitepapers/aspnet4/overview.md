---
uid: whitepapers/aspnet4/overview
title: Vue d’ensemble du développement Web ASP.NET 4 et Visual Studio 2010 | Microsoft Docs
author: rick-anderson
description: Ce document fournit une vue d’ensemble des nouvelles fonctionnalités de ASP.NET incluses dans the.NET Framework 4 et dans Visual Studio 2010.
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: ecde48f6bd88ee5f569bfeb8b70c26a50bc869c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630171"
---
# <a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>Vue d’ensemble du développement web ASP.NET 4 et Visual Studio 2010

> Ce document fournit une vue d’ensemble des nouvelles fonctionnalités de ASP.NET incluses dans the.NET Framework 4 et dans Visual Studio 2010.
> 
> [Téléchargez ce livre blanc](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)

**Sommaire**

**[Services principaux](#0.2__Toc253429238 "_Toc253429238")**  
[Refactorisation de fichier Web. config](#0.2__Toc253429239 "_Toc253429239")  
[Mise en cache de sortie extensible](#0.2__Toc253429240 "_Toc253429240")  
[Démarrage automatique des applications Web](#0.2__Toc253429241 "_Toc253429241")  
[Redirection permanente d’une page](#0.2__Toc253429242 "_Toc253429242")  
[Réduction de l’état de session](#0.2__Toc253429243 "_Toc253429243")  
[Extension de la plage d’URL autorisées](#0.2__Toc253429244 "_Toc253429244")  
[Validation de requête extensible](#0.2__Toc253429245 "_Toc253429245")  
[Mise en cache d’objets et extensibilité de la mise en cache d’objets](#0.2__Toc253429246 "_Toc253429246")  
[Extensible HTML, URL et encodage d’en-tête HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Analyse des performances pour les applications individuelles dans un processus de travail unique](#0.2__Toc253429248 "_Toc253429248")  
[Multi-ciblage](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery inclus avec Web Forms et MVC](#0.2__Toc253429251 "_Toc253429251")  
[Prise en charge du réseau de distribution de contenu](#0.2__Toc253429252 "_Toc253429252")  
[Scripts explicites ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Définition de méta-balises avec les propriétés page. MetaTags et page. MetaTags](#0.2__Toc253429257 "_Toc253429257")  
[Activation de l’état d’affichage pour les contrôles individuels](#0.2__Toc253429258 "_Toc253429258")  
[Modifications apportées aux fonctionnalités du navigateur](#0.2__Toc253429259 "_Toc253429259")  
[Routage dans ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Définition des ID client](#0.2__Toc253429261 "_Toc253429261")  
[Persistance de la sélection de lignes dans les contrôles de données](#0.2__Toc253429262 "_Toc253429262")  
[Contrôle Chart ASP.NET](#0.2__Toc253429263 "_Toc253429263")  
[Filtrage des données avec le contrôle QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Expressions de code encodées en HTML](#0.2__Toc253429265 "_Toc253429265")  
[Modifications du modèle de projet](#0.2__Toc253429266 "_Toc253429266")  
[Améliorations CSS](#0.2__Toc253429267 "_Toc253429267")  
[Masquage d’éléments div autour de champs masqués](#0.2__Toc253429268 "_Toc253429268")  
[Rendu d’une table externe pour les contrôles basés sur un modèle](#0.2__Toc253429269 "_Toc253429269")  
[Améliorations du contrôle ListView](#0.2__Toc253429270 "_Toc253429270")  
[Amélioration des contrôles CheckBoxList et RadioButtonList](#0.2__Toc253429271 "_Toc253429271")  
[Améliorations du contrôle Menu](#0.2__Toc253429272 "_Toc253429272")  
[Assistant et contrôles CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Prise en charge des zones](#0.2__Toc253429275 "_Toc253429275")  
[Prise en charge de la validation des attributs d’annotation de données](#0.2__Toc253429276 "_Toc253429276")  
[Assistances basées sur un modèle](#0.2__Toc253429277 "_Toc253429277")

**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**  
[Activation d’Dynamic Data pour les projets existants](#0.2__Toc253429279 "_Toc253429279")  
[Syntaxe déclarative du contrôle DynamicDataManager](#0.2__Toc253429280 "_Toc253429280")  
[Modèles d’entité](#0.2__Toc253429281 "_Toc253429281")  
[Nouveaux modèles de champ pour les URL et les adresses de messagerie](#0.2__Toc253429282 "_Toc253429282")  
[Création de liens avec le contrôle DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Prise en charge de l’héritage dans le modèle de données](#0.2__Toc253429284 "_Toc253429284")  
[Prise en charge des relations plusieurs-à-plusieurs (Entity Framework uniquement)](#0.2__Toc253429285 "_Toc253429285")  
[Nouveaux attributs pour contrôler l’affichage et les énumérations de prise en charge](#0.2__Toc253429286 "_Toc253429286")  
[Prise en charge améliorée des filtres](#0.2__Toc253429287 "_Toc253429287")

**[Améliorations du développement Web dans Visual Studio 2010](#0.2__Toc253429288 "_Toc253429288")**  
[Compatibilité CSS améliorée](#0.2__Toc253429289 "_Toc253429289")  
[Extraits de code HTML et JavaScript](#0.2__Toc253429290 "_Toc253429290")  
[Améliorations de JavaScript IntelliSense](#0.2__Toc253429291 "_Toc253429291")

**[Déploiement d’applications Web avec Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**  
[Empaquetage Web](#0.2__Toc253429293 "_Toc253429293")  
[Transformation Web. config](#0.2__Toc253429294 "_Toc253429294")  
[Déploiement de base de données](#0.2__Toc253429295 "_Toc253429295")  
[Publication en un clic pour les applications Web](#0.2__Toc253429296 "_Toc253429296")  
[Ressources](#0.2__Toc253429297 "_Toc253429297")

**[AVERTISSEMENT](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Services principaux

ASP.NET 4 introduit un certain nombre de fonctionnalités qui améliorent les principaux services ASP.NET tels que la mise en cache de sortie et le stockage d’état de session.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Refactorisation de fichier Web. config

Le fichier `Web.config` qui contient la configuration d’une application Web a considérablement augmenté depuis les versions précédentes du .NET Framework à mesure que de nouvelles fonctionnalités ont été ajoutées, comme Ajax, le routage et l’intégration avec IIS 7. Cela complique la configuration ou le démarrage de nouvelles applications Web sans outil tel que Visual Studio. Dans le .NET Framework 4, les éléments de configuration principaux ont été déplacés vers le fichier `machine.config`, et les applications héritent maintenant de ces paramètres. Cela permet au fichier `Web.config` dans les applications ASP.NET 4 d’être vide ou de contenir uniquement les lignes suivantes, qui spécifient pour Visual Studio la version du Framework ciblé par l’application :

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Mise en cache de sortie extensible

Depuis le lancement de ASP.NET 1,0, la mise en cache de la sortie a permis aux développeurs de stocker la sortie générée des pages, des contrôles et des réponses HTTP en mémoire. Lors des requêtes Web suivantes, ASP.NET peut traiter le contenu plus rapidement en extrayant la sortie générée à partir de la mémoire au lieu de régénérer la sortie de zéro. Toutefois, cette approche a une limitation : le contenu généré doit toujours être stocké en mémoire, et sur les serveurs qui rencontrent beaucoup de trafic, la mémoire consommée par la mise en cache de sortie peut rivaliser avec les demandes de mémoire d’autres parties d’une application Web.

ASP.NET 4 ajoute un point d’extensibilité à la mise en cache de sortie qui vous permet de configurer un ou plusieurs fournisseurs de cache de sortie personnalisés. Les fournisseurs de cache de sortie peuvent utiliser n’importe quel mécanisme de stockage pour conserver le contenu HTML. Cela permet de créer des fournisseurs de cache de sortie personnalisés pour divers mécanismes de persistance, qui peuvent inclure des disques locaux ou distants, le stockage cloud et des moteurs de cache distribué.

Vous créez un fournisseur de cache de sortie personnalisé en tant que classe qui dérive du nouveau type *System. Web. Caching. OutputCacheProvider* . Vous pouvez ensuite configurer le fournisseur dans le fichier `Web.config` à l’aide de la sous-section nouveaux *fournisseurs* de l’élément *OutputCache* , comme indiqué dans l’exemple suivant :

[!code-xml[Main](overview/samples/sample2.xml)]

Par défaut, dans ASP.NET 4, toutes les réponses HTTP, les pages rendues et les contrôles utilisent le cache de sortie en mémoire, comme indiqué dans l’exemple précédent, où l’attribut *defaultProvider* a la valeur AspNetInternalProvider. Vous pouvez modifier le fournisseur de cache de sortie par défaut utilisé pour une application Web en spécifiant un nom de fournisseur différent pour *defaultProvider*.

En outre, vous pouvez sélectionner différents fournisseurs de cache de sortie par contrôle et par demande. Le moyen le plus simple de choisir un autre fournisseur de cache de sortie pour différents contrôles utilisateur Web est de le faire de façon déclarative à l’aide du nouvel attribut *providerName* dans une directive de contrôle, comme indiqué dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample3.aspx)]

La spécification d’un autre fournisseur de cache de sortie pour une requête HTTP nécessite un peu plus de travail. Au lieu de spécifier de manière déclarative le fournisseur, vous substituez la nouvelle méthode *GetOuputCacheProviderName* dans le fichier `Global.asax` pour spécifier par programmation le fournisseur à utiliser pour une requête spécifique. L'exemple suivant montre comment effectuer cette opération.

[!code-csharp[Main](overview/samples/sample4.cs)]

Grâce à l’ajout de l’extensibilité du fournisseur de caches de sortie à ASP.NET 4, vous pouvez désormais poursuivre des stratégies de mise en cache de sortie plus agressives et plus intelligentes pour vos sites Web. Par exemple, il est désormais possible de mettre en cache les 10 premières pages d’un site en mémoire, tout en mettant en cache les pages qui obtiennent un trafic plus faible sur le disque. Vous pouvez également mettre en cache chaque combinaison variable d’une page rendue, mais utiliser un cache distribué afin que la consommation de mémoire soit déchargée des serveurs Web frontaux.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Démarrage automatique des applications Web

Certaines applications Web ont besoin de charger de grandes quantités de données ou d’effectuer un traitement d’initialisation coûteux avant de servir la première requête. Dans les versions antérieures de ASP.NET, pour ces situations, vous deviez concevoir des approches personnalisées pour « réveiller » une application ASP.NET, puis exécuter le code d’initialisation pendant l' *application\_méthode Load* dans le fichier `Global.asax`.

Une nouvelle fonctionnalité d’extensibilité nommée *démarrage automatique* qui répond directement à ce scénario est disponible quand ASP.net 4 s’exécute sur IIS 7,5 sur Windows Server 2008 R2. La fonctionnalité de démarrage automatique fournit une approche contrôlée pour démarrer un pool d’applications, initialiser une application ASP.NET, puis accepter des demandes HTTP.

> [!NOTE] 
> 
> Module de préchauffage d’application IIS pour IIS 7,5
> 
> L’équipe IIS a publié la première version test bêta du module de préchauffage d’application pour IIS 7,5. Cela rend la préchauffage de vos applications encore plus facile que celle décrite précédemment. Au lieu d’écrire du code personnalisé, vous spécifiez les URL des ressources à exécuter avant que l’application Web n’accepte les demandes du réseau. Cette préchauffage se produit au démarrage du service IIS (si vous avez configuré le pool d’applications IIS en tant que *AlwaysRunning*) et lors du recyclage d’un processus de travail IIS. Pendant le recyclage, l’ancien processus de travail IIS continue à exécuter des demandes jusqu’à ce que le processus de travail nouvellement généré soit entièrement nettoyé, de sorte que les applications ne subissent aucune interruption ou autre problème en raison des caches non amorcés. Notez que ce module fonctionne avec n’importe quelle version de ASP.NET, à partir de la version 2,0.
> 
> Pour plus d’informations, consultez [application chauffante](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) sur le site Web IIS.net. Pour obtenir une procédure pas à pas illustrant l’utilisation de la fonctionnalité de préparation à chaud, consultez [prise en main avec le module de mise en route de l’application IIS 7,5](https://www.iis.net/learn/manage) sur le site Web IIS.net.

Pour utiliser la fonctionnalité de démarrage automatique, un administrateur IIS définit un pool d’applications dans IIS 7,5 pour qu’il soit démarré automatiquement à l’aide de la configuration suivante dans le fichier `applicationHost.config` :

[!code-xml[Main](overview/samples/sample5.xml)]

Étant donné qu’un seul pool d’applications peut contenir plusieurs applications, vous spécifiez des applications individuelles qui doivent être démarrées automatiquement à l’aide de la configuration suivante dans le fichier `applicationHost.config` :

[!code-xml[Main](overview/samples/sample6.xml)]

Lorsqu’un serveur IIS 7,5 est démarré à froid ou lorsqu’un pool d’applications est recyclé, IIS 7,5 utilise les informations contenues dans le fichier `applicationHost.config` pour déterminer les applications Web qui doivent être démarrées automatiquement. Pour chaque application marquée pour un démarrage automatique, IIS 7.5 envoie une requête à ASP.NET 4 pour démarrer l’application dans un État pendant lequel l’application n’accepte pas temporairement les demandes HTTP. Lorsqu’il est dans cet État, ASP.NET instancie le type défini par l’attribut *serviceAutoStartProvider* (comme indiqué dans l’exemple précédent) et appelle dans son point d’entrée public.

Vous créez un type de démarrage automatique managé avec le point d’entrée nécessaire en implémentant l’interface *IProcessHostPreloadClient* , comme indiqué dans l’exemple suivant :

[!code-csharp[Main](overview/samples/sample7.cs)]

Une fois que votre code d’initialisation s’exécute dans la méthode de *préchargement* et que la méthode retourne, l’application ASP.net est prête à traiter les demandes.

Avec l’ajout du démarrage automatique à IIS 5 et ASP.NET 4, vous disposez désormais d’une approche bien définie pour effectuer une initialisation d’application coûteuse avant de traiter la première requête HTTP. Par exemple, vous pouvez utiliser la nouvelle fonctionnalité de démarrage automatique pour initialiser une application, puis signaler à un équilibreur de charge que l’application a été initialisée et prête à accepter le trafic HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Redirection permanente d’une page

Il est courant dans les applications Web de déplacer des pages et d’autres contenus dans le temps, ce qui peut entraîner une accumulation de liens obsolètes dans les moteurs de recherche. Dans ASP.NET, les développeurs ont traditionnellement géré les demandes aux anciennes URL à l’aide de la méthode *Response. Redirect* pour transférer une requête à la nouvelle URL. Toutefois, la méthode de *redirection* émet une réponse http 302 trouvée (redirection temporaire), ce qui entraîne un aller-retour http supplémentaire lorsque les utilisateurs essaient d’accéder aux anciennes URL.

ASP.NET 4 ajoute une nouvelle méthode d’assistance *RedirectPermanent* qui facilite l’émission de réponses http 301 déplacées de façon permanente, comme dans l’exemple suivant :

[!code-csharp[Main](overview/samples/sample8.cs)]

Les moteurs de recherche et les autres agents utilisateur qui reconnaissent les redirections permanentes stockent la nouvelle URL associée au contenu, ce qui élimine l’aller-retour inutile effectué par le navigateur pour les redirections temporaires.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Réduction de l’état de session

ASP.NET fournit deux options par défaut pour le stockage de l’état de session sur une batterie de serveurs Web : un fournisseur d’état de session qui appelle un serveur d’état de session hors processus et un fournisseur d’état de session qui stocke les données dans une base de données Microsoft SQL Server. Étant donné que les deux options impliquent le stockage des informations d’État en dehors du processus de travail d’une application Web, l’état de session doit être sérialisé avant d’être envoyé au stockage étendu. En fonction de la quantité d’informations qu’un développeur enregistre dans l’état de session, la taille des données sérialisées peut croître de manière importante.

ASP.NET 4 introduit une nouvelle option de compression pour les deux types de fournisseurs d’état de session hors processus. Lorsque l’option de configuration *compressionEnabled* illustrée dans l’exemple suivant est définie sur *true*, ASP.net compresse (et décompresse) l’état de session sérialisé à l’aide de la classe .NET Framework *System. IO. compression. GZipStream* .

[!code-xml[Main](overview/samples/sample9.xml)]

Avec l’ajout simple du nouvel attribut au fichier `Web.config`, les applications avec des cycles d’UC de rechange sur les serveurs Web peuvent réaliser des réductions substantielles de la taille des données d’état de session sérialisées.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Extension de la plage d’URL autorisées

ASP.NET 4 introduit de nouvelles options pour l’extension de la taille des URL d’application. Dans les versions précédentes de ASP.NET, les longueurs de chemin d’URL sont limitées à 260 caractères, selon la limite du chemin d’accès au fichier NTFS. Dans ASP.NET 4, vous avez la possibilité d’augmenter (ou de diminuer) cette limite en fonction de vos applications, à l’aide de deux nouveaux attributs de configuration *httpRuntime* . L’exemple suivant montre ces nouveaux attributs.

[!code-xml[Main](overview/samples/sample10.xml)]

Pour autoriser des chemins d’accès plus longs ou plus courts (la partie de l’URL qui n’inclut pas le protocole, le nom du serveur et la chaîne de requête), modifiez l’attribut *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* . Pour autoriser des chaînes de requête plus longues ou plus courtes, modifiez la valeur de l’attribut *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* .

ASP.NET 4 vous permet également de configurer les caractères utilisés par la vérification des caractères URL. Quand ASP.NET trouve un caractère non valide dans la partie chemin d’accès d’une URL, il rejette la requête et émet une erreur HTTP 400. Dans les versions précédentes de ASP.NET, les contrôles de caractères d’URL étaient limités à un ensemble fixe de caractères. Dans ASP.NET 4, vous pouvez personnaliser le jeu de caractères valides à l’aide du nouvel attribut *requestPathInvalidCharacters* de l’élément de configuration *httpRuntime* , comme indiqué dans l’exemple suivant :

[!code-xml[Main](overview/samples/sample11.xml)]

Par défaut, l’attribut *requestPathInvalidCharacters* définit huit caractères comme étant non valides. (Dans la chaîne qui est assignée par défaut à *requestPathInvalidCharacters* , les caractères inférieur à (&lt;), supérieur à (&gt;) et perluète (&amp;) sont encodés, car le fichier `Web.config` est un fichier XML.) Vous pouvez personnaliser le jeu de caractères non valides en fonction des besoins.

> [!NOTE]
> Remarque ASP.NET 4 rejette toujours les chemins d’accès d’URL qui contiennent des caractères de la plage ASCII 0x00 à 0x1F, car il s’agit de caractères d’URL non valides, tels que définis dans le document RFC 2396 de l’IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). Sur les versions de Windows Server qui exécutent IIS 6 ou version ultérieure, le pilote de périphérique de protocole http. sys rejette automatiquement les URL avec ces caractères.

<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Validation de requête extensible

La validation de la demande ASP.NET recherche les chaînes qui sont couramment utilisées dans les attaques de script entre sites (XSS) pour les données de requête HTTP entrantes. Si des chaînes XSS potentielles sont détectées, la validation de la demande signale la chaîne suspecte et retourne une erreur. La validation de la demande intégrée retourne une erreur uniquement lorsqu’elle trouve les chaînes les plus courantes utilisées dans les attaques XSS. Les tentatives précédentes visant à rendre la validation XSS plus agressive ont entraîné un trop grand nombre de faux positifs. Toutefois, les clients peuvent souhaiter que la validation des demandes soit plus agressive, ou inversement, pour délibérer intentionnellement des contrôles XSS pour des pages spécifiques ou pour des types de demandes spécifiques.

Dans ASP.NET 4, la fonctionnalité de validation de demande a été rendue extensible afin que vous puissiez utiliser la logique de validation de requête personnalisée. Pour étendre la validation de la demande, vous créez une classe qui dérive du nouveau type *System. Web. util. RequestValidator* et vous configurez l’application (dans la section *httpRuntime* du fichier `Web.config`) pour utiliser le type personnalisé. L’exemple suivant montre comment configurer une classe de validation de demande personnalisée :

[!code-xml[Main](overview/samples/sample12.xml)]

Le nouvel attribut *RequestValidationType* requiert une chaîne d’identificateur de type .NET Framework standard qui spécifie la classe qui fournit la validation de demande personnalisée. Pour chaque demande, ASP.NET appelle le type personnalisé pour traiter chaque élément de données de requête HTTP entrantes. L’URL entrante, tous les en-têtes HTTP (cookies et en-têtes personnalisés) et le corps d’entité sont tous disponibles pour inspection par une classe de validation de demande personnalisée comme celle illustrée dans l’exemple suivant :

[!code-csharp[Main](overview/samples/sample13.cs)]

Dans les cas où vous ne souhaitez pas inspecter un élément de données HTTP entrantes, la classe de validation des demandes peut revenir pour permettre l’exécution de la validation de la demande par défaut ASP.NET en appelant simplement la *base. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Mise en cache d’objets et extensibilité de la mise en cache d’objets

Depuis sa première version, ASP.NET a inclus un cache d’objets en mémoire puissant (*System. Web. Caching. cache*). L’implémentation du cache est tellement populaire qu’elle a été utilisée dans des applications non Web. Toutefois, il est difficile pour une application Windows Forms ou WPF d’inclure une référence à `System.Web.dll` simplement pour pouvoir utiliser le cache d’objets ASP.NET.

Pour rendre la mise en cache disponible pour toutes les applications, le .NET Framework 4 introduit un nouvel assembly, un nouvel espace de noms, certains types de base et une implémentation de mise en cache concrète. Le nouvel assembly `System.Runtime.Caching.dll` contient une nouvelle API de mise en cache dans l’espace de noms *System. Runtime. Caching* . L’espace de noms contient deux ensembles principaux de classes :

- Types abstraits qui fournissent la Fondation pour la génération de tout type d’implémentation de cache personnalisée.
- Implémentation du cache d’objets en mémoire concrète (classe *System. Runtime. Caching. MemoryCache* ).

La nouvelle classe *MemoryCache* est modelée en détail dans le cache ASP.net et elle partage une grande partie de la logique du moteur de cache interne avec ASP.net. Bien que les API de mise en cache publiques dans *System. Runtime. Caching* aient été mises à jour pour prendre en charge le développement de caches personnalisés, si vous avez utilisé l’objet *cache* ASP.net, vous trouverez des concepts familiers dans les nouvelles API.

Une présentation détaillée de la nouvelle classe *MemoryCache* et de la prise en charge des API de base nécessiterait l’intégralité d’un document. Toutefois, l’exemple suivant vous donne une idée du fonctionnement de la nouvelle API de cache. L’exemple a été écrit pour une application Windows Forms, sans aucune dépendance sur `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Extensible HTML, URL et encodage d’en-tête HTTP

Dans ASP.NET 4, vous pouvez créer des routines d’encodage personnalisées pour les tâches courantes d’encodage de texte suivantes :

- Encodage HTML.
- Encodage d’URL.
- Encodage d’attribut HTML.
- Encodage des en-têtes HTTP sortants.

Vous pouvez créer un encodeur personnalisé en dérivant du nouveau type *System. Web. util. HttpEncoder* , puis en configurant ASP.net pour utiliser le type personnalisé dans la section *httpRuntime* du fichier `Web.config`, comme indiqué dans l’exemple suivant :

[!code-xml[Main](overview/samples/sample15.xml)]

Après la configuration d’un encodeur personnalisé, ASP.NET appelle automatiquement l’implémentation d’encodage personnalisée à chaque fois que les méthodes d’encodage publiques des classes *System. Web. HttpUtility* ou *System. Web. HttpServerUtility* sont appelées. Cela permet à une partie d’une équipe de développement Web de créer un encodeur personnalisé qui implémente l’encodage de caractères agressif, tandis que le reste de l’équipe de développement Web continue d’utiliser les API d’encodage ASP.NET publiques. En configurant de manière centralisée un encodeur personnalisé dans l’élément *httpRuntime* , vous êtes assuré que tous les appels d’encodage de texte provenant des API d’encodage ASP.net publiques sont routés via l’encodeur personnalisé.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Analyse des performances pour les applications individuelles dans un processus de travail unique

Afin d’augmenter le nombre de sites Web pouvant être hébergés sur un seul serveur, de nombreux hébergeurs exécutent plusieurs applications ASP.NET dans un même processus de travail. Toutefois, si plusieurs applications utilisent un seul processus de travail partagé, il est difficile pour les administrateurs de serveur d’identifier une application individuelle qui rencontre des problèmes.

ASP.NET 4 tire parti de la nouvelle fonctionnalité d’analyse des ressources introduite par le CLR. Pour activer cette fonctionnalité, vous pouvez ajouter l’extrait de configuration XML suivant au fichier de configuration `aspnet.config`.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Notez que le fichier `aspnet.config` se trouve dans le répertoire d’installation du .NET Framework. Il ne s’agit pas du fichier `Web.config`.

Lorsque la fonctionnalité *appDomainResourceMonitoring* a été activée, deux nouveaux compteurs de performance sont disponibles dans la catégorie de performances « applications ASP.net » : *% du temps processeur géré* et de la *mémoire managée utilisée*. Ces deux compteurs de performance utilisent la nouvelle fonctionnalité de gestion des ressources de domaine d’application CLR pour suivre le temps processeur estimé et l’utilisation de la mémoire gérée des applications ASP.NET individuelles. Par conséquent, avec ASP.NET 4, les administrateurs disposent désormais d’une vue plus granulaire de la consommation des ressources des applications individuelles s’exécutant dans un processus de travail unique.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Multi-ciblage

Vous pouvez créer une application qui cible une version spécifique du .NET Framework. Dans ASP.NET 4, un nouvel attribut dans l’élément *compilation* du fichier `Web.config` vous permet de cibler le .NET Framework 4 et versions ultérieures. Si vous ciblez explicitement le .NET Framework 4 et si vous incluez des éléments facultatifs dans le fichier `Web.config`, tels que les entrées pour *System. CodeDom*, ces éléments doivent être corrects pour le .NET Framework 4. (Si vous ne ciblez pas explicitement le .NET Framework 4, la version cible de .NET Framework est déduite de l’absence d’une entrée dans le fichier `Web.config`.)

L’exemple suivant illustre l’utilisation de l’attribut *targetFramework* dans l’élément *compilation* du fichier `Web.config`.

[!code-xml[Main](overview/samples/sample17.xml)]

Notez les points suivants concernant le ciblage d’une version spécifique du .NET Framework :

- Dans un pool d’applications .NET Framework 4, le système de génération ASP.NET part du principe que le .NET Framework 4 comme cible si le fichier `Web.config` n’inclut pas l’attribut *targetFramework* ou si le fichier `Web.config` est manquant. (Vous devrez peut-être apporter des modifications de codage à votre application pour qu’elle s’exécute sous le .NET Framework 4.)
- Si vous incluez l’attribut *targetFramework* et que l’élément *System. CodeDom* est défini dans le fichier `Web.config`, ce fichier doit contenir les entrées correctes pour le .NET Framework 4.
- Si vous utilisez la commande *aspnet\_du compilateur* pour précompiler votre application (par exemple dans un environnement de génération), vous devez utiliser la version correcte de la commande du *compilateur de\_ASPNET* pour la version cible de .NET Framework. Utilisez le compilateur fourni avec la .NET Framework 2,0 (%windir%\Microsoft.NET\Framework\v2.0.50727.) pour compiler les .NET Framework 3,5 et versions antérieures. Utilisez le compilateur fourni avec le .NET Framework 4 pour compiler les applications créées à l’aide de ce Framework ou à l’aide des versions ultérieures.
- Au moment de l’exécution, le compilateur utilise les derniers assemblys de Framework installés sur l’ordinateur (et par conséquent dans le GAC). Si une mise à jour est effectuée ultérieurement dans l’infrastructure (par exemple, si une version hypothétique 4,1 est installée), vous pouvez utiliser les fonctionnalités de la version plus récente de l’infrastructure, même si l’attribut *targetFramework* cible une version antérieure (par exemple, 4,0). (Toutefois, au moment de la conception dans Visual Studio 2010 ou lorsque vous utilisez la commande *aspnet\_du compilateur* , l’utilisation de nouvelles fonctionnalités de l’infrastructure entraînera des erreurs de compilation).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery inclus avec Web Forms et MVC

Les modèles Visual Studio pour Web Forms et MVC incluent la bibliothèque jQuery Open source. Lorsque vous créez un nouveau site Web ou projet, un dossier scripts contenant les 3 fichiers suivants est créé :

- jQuery-1.4.1. js : version unminified explicite de la bibliothèque jQuery.
- jQuery-14.1. min. js : version minimisés de la bibliothèque jQuery.
- jQuery-1.4.1-vsdoc. js : fichier de documentation IntelliSense pour la bibliothèque jQuery.

Incluez la version unminified de jQuery lors du développement d’une application. Incluez la version minimisés de jQuery pour les applications de production.

Par exemple, la page de Web Forms suivante montre comment vous pouvez utiliser jQuery pour modifier la couleur d’arrière-plan des contrôles TextBox ASP.NET en jaune lorsqu’ils ont le focus.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Prise en charge du réseau de distribution de contenu

Le réseau de distribution de contenu (CDN) Microsoft Ajax vous permet d’ajouter facilement des scripts ASP.NET AJAX et jQuery à vos applications Web. Par exemple, vous pouvez commencer à utiliser la bibliothèque jQuery simplement en ajoutant une balise `<script>` à votre page qui pointe vers Ajax.microsoft.com comme suit :

[!code-html[Main](overview/samples/sample19.html)]

En tirant parti du CDN Microsoft Ajax, vous pouvez améliorer considérablement la performance de vos applications Ajax. Le contenu du CDN Microsoft AJAX est mis en cache sur les serveurs situés dans le monde entier. En outre, le CDN Microsoft Ajax permet aux navigateurs de réutiliser des fichiers JavaScript mis en cache pour les sites Web qui se trouvent dans des domaines différents.

Le réseau de distribution de contenu Microsoft Ajax prend en charge le protocole SSL (HTTPs) au cas où vous devriez traiter une page Web à l’aide de l’protocole SSL.

Implémentez une solution de secours lorsque le CDN n’est pas disponible. Testez le secours.

Pour en savoir plus sur le CDN Microsoft Ajax, visitez le site Web suivant :

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

ASP.NET ScriptManager prend en charge le CDN Microsoft Ajax. En définissant simplement une propriété, la propriété EnableCdn, vous pouvez récupérer tous les fichiers JavaScript ASP.NET Framework à partir du CDN :

[!code-aspx[Main](overview/samples/sample20.aspx)]

Une fois que vous avez défini la propriété EnableCdn sur la valeur true, l’infrastructure ASP.NET récupère tous les fichiers ASP.NET Framework JavaScript du CDN, y compris tous les fichiers JavaScript utilisés pour la validation et UpdatePanel. La définition de cette propriété peut avoir un impact spectaculaire sur les performances de votre application Web.

Vous pouvez définir le chemin CDN pour vos propres fichiers JavaScript à l’aide de l’attribut Resource. La nouvelle propriété CdnPath spécifie le chemin d’accès au CDN utilisé lorsque vous affectez la valeur true à la propriété EnableCdn :

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Scripts explicites ScriptManager

Dans le passé, si vous utilisiez ASP.NET ScriptManger, vous deviez charger l’intégralité de la bibliothèque ASP.NET AJAX monolithique. En tirant parti de la nouvelle propriété ScriptManager. AjaxFrameworkMode, vous pouvez contrôler exactement quels composants de la bibliothèque AJAX ASP.NET sont chargés et charger uniquement les composants de la bibliothèque AJAX ASP.NET dont vous avez besoin.

La propriété ScriptManager. AjaxFrameworkMode peut avoir les valeurs suivantes :

- Enabled : spécifie que le contrôle ScriptManager comprend automatiquement le fichier de script MicrosoftAjax. js, qui est un fichier de script combiné de chaque script d’infrastructure principale (comportement hérité).
- Disabled : spécifie que toutes les fonctionnalités de script Microsoft AJAX sont désactivées et que le contrôle ScriptManager ne fait référence à aucun script automatiquement.
- Explicit : spécifie que vous incluez explicitement des références de script à chaque fichier de script Framework Core requis par votre page et que vous incluez des références aux dépendances requises par chaque fichier de script.

Par exemple, si vous affectez à la propriété AjaxFrameworkMode la valeur Explicit, vous pouvez spécifier les scripts de composant ASP.NET AJAX spécifiques dont vous avez besoin :

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Forms

Web Forms est une fonctionnalité de base de ASP.NET depuis la sortie de ASP.NET 1,0. De nombreuses améliorations ont été apportées dans ce domaine pour ASP.NET 4, y compris les éléments suivants :

- La possibilité de définir des balises *méta* .
- Plus de contrôle sur l’état d’affichage.
- Méthodes plus faciles pour utiliser les fonctionnalités du navigateur.
- Prise en charge de l’utilisation du routage ASP.NET avec Web Forms.
- Plus de contrôle sur les ID générés.
- Capacité à rendre persistantes les lignes sélectionnées dans les contrôles de données.
- Plus de contrôle sur le HTML rendu dans les contrôles *FormView* et *ListView* .
- Filtrage de la prise en charge des contrôles de source de données.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Définition de méta-balises avec les propriétés page. MetaTags et page. MetaTags

ASP.NET 4 ajoute deux propriétés à la classe de *page* , aux *Mots clés* et à la *redescription*. Ces deux propriétés représentent des balises *méta* correspondantes dans votre page, comme illustré dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample23.aspx)]

Ces deux propriétés fonctionnent de la même façon que la propriété *title* de la page. Ils suivent les règles suivantes :

1. S’il n’y a pas de balises *méta* dans l’élément *Head* qui correspondent aux noms de propriétés (autrement dit, Name = "keywords" pour *page.* metatagss et Name = "Description" pour *page.* MetaTags, ce qui signifie que ces propriétés n’ont pas été définies), les balises *méta* sont ajoutées à la page lors de leur rendu.
2. S’il existe déjà des balises *méta* avec ces noms, ces propriétés agissent comme des méthodes d’extraction et de définition pour le contenu des balises existantes.

Vous pouvez définir ces propriétés au moment de l’exécution, ce qui vous permet d’obtenir le contenu à partir d’une base de données ou d’une autre source, et de définir les balises de manière dynamique pour décrire le rôle d’une page particulière.

Vous pouvez également définir les propriétés des *Mots clés* et de la *Description* dans la directive *@ page* en haut du balisage de la page Web Forms, comme dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample24.aspx)]

Cette opération remplace le contenu de la balise *meta* (le cas échéant) déjà déclaré dans la page.

Le contenu de la balise *meta* Description est utilisé pour améliorer les aperçus de la liste de recherche dans Google. (Pour plus d’informations, consultez [améliorer les extraits de code avec une méta-Description permodernisation](https://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) sur le blog Google Webmaster central.) Google et Windows Live Search n’utilisent pas le contenu des mots clés pour tout, mais d’autres moteurs de recherche peuvent l’être. Pour plus d’informations, consultez les conseils sur les [Mots clés META](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) sur le site Web du Guide du moteur de recherche.

Ces nouvelles propriétés sont une fonctionnalité simple, mais elles vous permettent de vous éviter de les ajouter manuellement ou d’écrire votre propre code pour créer les balises *méta* .

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Activation de l’état d’affichage pour les contrôles individuels

Par défaut, l’état d’affichage est activé pour la page, avec pour résultat que chaque contrôle sur la page stocke potentiellement l’état d’affichage, même s’il n’est pas requis pour l’application. Les données d’état d’affichage sont incluses dans le balisage généré par une page et augmentent le temps nécessaire à l’envoi d’une page au client et à sa publication. Le stockage d’un plus grand état d’affichage que ce qui est nécessaire peut entraîner une dégradation significative des performances. Dans les versions antérieures de ASP.NET, les développeurs pouvaient désactiver l’état d’affichage pour les contrôles individuels afin de réduire la taille de la page, mais ils devaient le faire explicitement pour les contrôles individuels. Dans ASP.NET 4, les contrôles serveur Web incluent une propriété *ViewStateMode* qui vous permet de désactiver l’état d’affichage par défaut, puis de l’activer uniquement pour les contrôles qui l’exigent dans la page.

La propriété *ViewStateMode* prend une énumération qui a trois valeurs : *Enabled*, *Disabled*et *inherit*. *Activé* active l’état d’affichage pour ce contrôle et pour tous les contrôles enfants qui ont la valeur *inherit* ou qui n’ont rien défini. *Désactivé* désactive l’état d’affichage, et *inherit* spécifie que le contrôle utilise le paramètre *ViewStateMode* du contrôle parent.

L’exemple suivant illustre le fonctionnement de la propriété *ViewStateMode* . Le balisage et le code des contrôles de la page suivante incluent des valeurs pour la propriété *ViewStateMode* :

[!code-aspx[Main](overview/samples/sample25.aspx)]

Comme vous pouvez le voir, le code désactive l’état d’affichage pour le contrôle PlaceHolder1. Le contrôle Label1 enfant hérite de cette valeur de propriété (*inherit* est la valeur par défaut pour *ViewStateMode* pour les contrôles) et n’enregistre donc pas d’état d’affichage. Dans le contrôle PlaceHolder2, *ViewStateMode* est défini sur *activé*. par conséquent, Label2 hérite de cette propriété et enregistre l’état d’affichage. Lorsque la page est chargée pour la première fois, la propriété *Text* des deux contrôles *label* est définie sur la chaîne « [DynamicValue] ».

L’effet de ces paramètres est que lorsque la page se charge pour la première fois, la sortie suivante s’affiche dans le navigateur :

Désactivée `: [DynamicValue]`

Activé :`[DynamicValue]`

Toutefois, après une publication (postback), la sortie suivante s’affiche :

Désactivée `: [DeclaredValue]`

Activé :`[DynamicValue]`

Le contrôle Label1 (dont la valeur *ViewStateMode* est définie sur *Disabled*) n’a pas conservé la valeur à laquelle il a été défini dans le code. Toutefois, le contrôle Label2 (dont la valeur *ViewStateMode* est définie sur *activé*) a conservé son état.

Vous pouvez également définir *ViewStateMode* dans la directive *@ page* , comme dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample26.aspx)]

La classe *page* n’est qu’un autre contrôle ; Il agit en tant que contrôle parent pour tous les autres contrôles de la page. La valeur par défaut de *ViewStateMode* est *activée* pour les instances de *page*. Comme les contrôles *héritent*par défaut, les contrôles hériteront de la valeur de la propriété *Enabled* , sauf si vous définissez *ViewStateMode* au niveau de la page ou du contrôle.

La valeur de la propriété *ViewStateMode* détermine si l’état d’affichage est conservé uniquement si la propriété *EnableViewState* a la valeur *true*. Si la propriété *EnableViewState* est définie sur *false*, l’état d’affichage ne sera pas conservé même si *ViewStateMode* est défini sur *activé*.

Pour cette fonctionnalité, il est recommandé d’utiliser les contrôles *ContentPlaceHolder* dans les pages maîtres, où vous pouvez définir *ViewStateMode* sur *désactivé* pour la page maître, puis l’activer individuellement pour les contrôles *ContentPlaceHolder* qui, à leur tour, contiennent des contrôles qui requièrent un état d’affichage.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Modifications apportées aux fonctionnalités du navigateur

ASP.NET détermine les fonctionnalités du navigateur qu’un utilisateur utilise pour parcourir votre site à l’aide d’une fonctionnalité appelée *fonctionnalités du navigateur*. Les fonctionnalités du navigateur sont représentées par l’objet *HttpBrowserCapabilities* (exposé par la propriété *Request. Browser* ). Par exemple, vous pouvez utiliser l’objet *HttpBrowserCapabilities* pour déterminer si le type et la version du navigateur actuel prennent en charge une version particulière de JavaScript. Ou bien, vous pouvez utiliser l’objet *HttpBrowserCapabilities* pour déterminer si la demande provient d’un appareil mobile.

L’objet *HttpBrowserCapabilities* est piloté par un ensemble de fichiers de définition de navigateur. Ces fichiers contiennent des informations sur les fonctionnalités des navigateurs particuliers. Dans ASP.NET 4, ces fichiers de définition de navigateur ont été mis à jour pour contenir des informations sur les navigateurs et les appareils récemment introduits, tels que Google Chrome, Research dans Motion BlackBerry smartphones et Apple iPhone.

La liste suivante répertorie les nouveaux fichiers de définition de navigateur :

- *BlackBerry. Browser*
- *chrome. Browser*
- *Par défaut. Browser*
- *Firefox. Browser*
- *passerelle. Browser*
- *générique. Browser*
- *IE. Browser*
- *iemobile. Browser*
- *iPhone. Browser*
- *Opera. Browser*
- *Safari. Browser*

#### <a name="using-browser-capabilities-providers"></a>Utilisation des fournisseurs de fonctionnalités de navigateur

Dans ASP.NET version 3,5 Service Pack 1, vous pouvez définir les fonctionnalités d’un navigateur de l’une des manières suivantes :

- Au niveau de l’ordinateur, vous créez ou mettez à jour un fichier XML `.browser` dans le dossier suivant :

- [!code-console[Main](overview/samples/sample27.cmd)]

- Une fois que vous avez défini la fonctionnalité du navigateur, exécutez la commande suivante à partir de l’invite de commandes de Visual Studio afin de régénérer l’assembly des fonctionnalités du navigateur et de l’ajouter au GAC :

- [!code-console[Main](overview/samples/sample28.cmd)]

- Pour une application individuelle, vous créez un fichier `.browser` dans le dossier `App_Browsers` de l’application.

Ces approches nécessitent de modifier les fichiers XML et, pour les modifications au niveau de l’ordinateur, vous devez redémarrer l’application après avoir exécuté le processus ASPNET\_RegBrowsers. exe.

ASP.NET 4 comprend une fonctionnalité appelée fournisseurs de *fonctionnalités de navigateur*. Comme son nom l’indique, cela vous permet de créer un fournisseur qui, à son tour, vous permet d’utiliser votre propre code pour déterminer les fonctionnalités du navigateur.

En pratique, les développeurs ne définissent pas souvent des fonctionnalités de navigateur personnalisées. Les fichiers de navigateur sont difficiles à mettre à jour, le processus de mise à jour est assez complexe et la syntaxe XML pour les fichiers de `.browser` peut être complexe à utiliser et à définir. Ce processus serait beaucoup plus simple, s’il existait une syntaxe de définition de navigateur commune, ou une base de données qui contenait des définitions de navigateur à jour, ou même un service Web pour une telle base de données. La nouvelle fonctionnalité des fournisseurs de fonctionnalités de navigateur rend ces scénarios possibles et pratiques pour les développeurs tiers.

Il existe deux approches principales pour l’utilisation de la nouvelle fonctionnalité de fournisseur de fonctionnalités de navigateur ASP.NET 4 : extension de la fonctionnalité de définition des fonctionnalités du navigateur ASP.NET, ou remplacement total de celle-ci. Les sections suivantes décrivent d’abord comment remplacer la fonctionnalité, puis comment l’étendre.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Remplacement de la fonctionnalité de fonctionnalités du navigateur ASP.NET

Pour remplacer complètement la fonctionnalité de définition des fonctionnalités du navigateur ASP.NET, procédez comme suit :

1. Créez une classe de fournisseur qui dérive de *HttpCapabilitiesProvider* et qui remplace la méthode *GetBrowserCapabilities* , comme dans l’exemple suivant : 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Le code de cet exemple crée un nouvel objet *HttpBrowserCapabilities* , en spécifiant uniquement la fonctionnalité nommée Browser et en définissant cette fonctionnalité sur MyCustomBrowser.
2. Inscrivez le fournisseur auprès de l’application. 

    Pour pouvoir utiliser un fournisseur avec une application, vous devez ajouter l’attribut *Provider* à la section *browserCaps* des fichiers `Web.config` ou `Machine.config`. (Vous pouvez également définir les attributs du fournisseur dans un élément *emplacement* pour des répertoires spécifiques dans l’application, par exemple dans un dossier pour un appareil mobile spécifique.) L’exemple suivant montre comment définir l’attribut *Provider* dans un fichier de configuration :

    [!code-xml[Main](overview/samples/sample30.xml)]

    Une autre façon d’inscrire la nouvelle définition de la fonctionnalité du navigateur consiste à utiliser le code, comme indiqué dans l’exemple suivant :

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Ce code doit s’exécuter dans l' *Application\_* événement de démarrage du fichier `Global.asax`. Toute modification apportée à la classe *BrowserCapabilitiesProvider* doit se produire avant l’exécution du code de l’application, afin de s’assurer que le cache reste dans un état valide pour l’objet *HttpCapabilitiesBase* résolu.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Mise en cache de l’objet HttpBrowserCapabilities

L’exemple précédent présente un problème, à savoir que le code s’exécute chaque fois que le fournisseur personnalisé est appelé afin d’accéder à l’objet *HttpBrowserCapabilities* . Cela peut se produire plusieurs fois lors de chaque demande. Dans l’exemple, le code du fournisseur ne fait pas grand-chose. Toutefois, si le code de votre fournisseur personnalisé effectue un travail significatif afin d’accéder à l’objet *HttpBrowserCapabilities* , cela peut affecter les performances. Pour éviter ce problème, vous pouvez mettre en cache l’objet *HttpBrowserCapabilities* . Procédez comme suit :

1. Créez une classe qui dérive de *HttpCapabilitiesProvider*, comme celle de l’exemple suivant : 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    Dans l’exemple, le code génère une clé de cache en appelant une méthode BuildCacheKey personnalisée et obtient la durée de mise en cache en appelant une méthode GetCacheTime personnalisée. Le code ajoute ensuite l’objet *HttpBrowserCapabilities* résolu au cache. L’objet peut être récupéré à partir du cache et réutilisé sur les demandes suivantes qui utilisent le fournisseur personnalisé.
2. Inscrivez le fournisseur auprès de l’application, comme décrit dans la procédure précédente.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Extension de la fonctionnalité de fonctionnalités du navigateur ASP.NET

La section précédente a décrit comment créer un nouvel objet *HttpBrowserCapabilities* dans ASP.net 4. Vous pouvez également étendre la fonctionnalité de fonctionnalités du navigateur ASP.NET en ajoutant de nouvelles définitions de fonctionnalités de navigateur à celles qui figurent déjà dans ASP.NET. Vous pouvez le faire sans utiliser les définitions du navigateur XML. La procédure suivante montre comment procéder.

1. Créez une classe qui dérive de *HttpCapabilitiesEvaluator* et qui remplace la méthode *GetBrowserCapabilities* , comme indiqué dans l’exemple suivant : 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Ce code utilise tout d’abord la fonctionnalité de fonctionnalités du navigateur ASP.NET pour tenter d’identifier le navigateur. Toutefois, si aucun navigateur n’est identifié en fonction des informations définies dans la demande (autrement dit, si la propriété *Browser* de l’objet *HttpBrowserCapabilities* est la chaîne « Unknown »), le code appelle le fournisseur personnalisé (MyBrowserCapabilitiesEvaluator) pour identifier le navigateur.
2. Inscrivez le fournisseur auprès de l’application, comme décrit dans l’exemple précédent.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Extension des fonctionnalités des fonctionnalités du navigateur en ajoutant de nouvelles fonctionnalités aux définitions de fonctionnalités existantes

En plus de créer un fournisseur de définitions de navigateur personnalisé et de créer dynamiquement de nouvelles définitions de navigateur, vous pouvez étendre les définitions de navigateur existantes avec des fonctionnalités supplémentaires. Cela vous permet d’utiliser une définition qui est proche de ce que vous souhaitez, mais qui ne dispose que de quelques fonctionnalités. Pour ce faire, effectuez les étapes suivantes.

1. Créez une classe qui dérive de *HttpCapabilitiesEvaluator* et qui remplace la méthode *GetBrowserCapabilities* , comme indiqué dans l’exemple suivant : 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    L’exemple de code étend la classe ASP.NET *HttpCapabilitiesEvaluator* existante et obtient l’objet *HttpBrowserCapabilities* qui correspond à la définition de la requête actuelle à l’aide du code suivant :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Le code peut ensuite ajouter ou modifier une fonctionnalité pour ce navigateur. Il existe deux façons de spécifier une nouvelle fonctionnalité de navigateur :

    - Ajoutez une paire clé/valeur à l’objet *IDictionary* exposé par la propriété *Capabilities* de l’objet *HttpCapabilitiesBase* . Dans l’exemple précédent, le code ajoute une fonctionnalité nommée multipoint avec la valeur *true*.
    - Définissez les propriétés existantes de l’objet *HttpCapabilitiesBase* . Dans l’exemple précédent, le code définit la propriété *frames* sur *true*. Cette propriété est simplement un accesseur pour l’objet *IDictionary* qui est exposé par la propriété *Capabilities* . 

        > [!NOTE]
        > Notez que ce modèle s’applique à toute propriété de *HttpBrowserCapabilities*, y compris les adaptateurs de contrôle.
2. Inscrivez le fournisseur auprès de l’application, comme décrit dans la procédure précédente.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Routage dans ASP.NET 4

ASP.NET 4 ajoute une prise en charge intégrée de l’utilisation du routage avec Web Forms. Le routage vous permet de configurer une application pour accepter des URL de requête qui ne sont pas mappées à des fichiers physiques. Au lieu de cela, vous pouvez utiliser le routage pour définir des URL qui sont significatives pour les utilisateurs et qui peuvent faciliter l’optimisation du moteur de recherche (SEO) pour votre application. Par exemple, l’URL d’une page qui affiche des catégories de produits dans une application existante peut se présenter comme dans l’exemple suivant :

[!code-console[Main](overview/samples/sample36.cmd)]

En utilisant le routage, vous pouvez configurer l’application pour qu’elle accepte l’URL suivante pour afficher les mêmes informations :

[!code-console[Main](overview/samples/sample37.cmd)]

Le routage est disponible à partir de ASP.NET 3,5 SP1. (Pour obtenir un exemple d’utilisation du routage dans ASP.NET 3,5 SP1, consultez l’entrée [utilisation du routage avec WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "Titre de cette entrée.") sur le blog de Phil Haack.) Toutefois, ASP.NET 4 inclut des fonctionnalités qui facilitent l’utilisation du routage, notamment les suivantes :

- La classe *PageRouteHandler* , qui est un simple gestionnaire HTTP que vous utilisez lorsque vous définissez des itinéraires. La classe transmet les données à la page vers laquelle la demande est routée.
- Les nouvelles propriétés *HttpRequest. RequestContext* et *page. RouteData* (qui est un proxy pour l’objet *HttpRequest. RequestContext. RouteData* ). Ces propriétés facilitent l’accès aux informations qui sont transmises à partir de l’itinéraire.
- Les nouveaux générateurs d’expressions suivants, qui sont définis dans *System. Web. Compilation. RouteUrlExpressionBuilder* et *System. Web. Compilation. RouteValueExpressionBuilder*:
- *RouteUrl*, qui fournit un moyen simple de créer une URL qui correspond à une URL de routage dans un contrôle serveur ASP.net.
- *RouteValue*, qui fournit un moyen simple d’extraire des informations de l’objet *RouteContext* .
- La classe *RouteParameter* , qui permet de passer plus facilement les données contenues dans un objet *RouteContext* à une requête pour un contrôle de source de données (semblable à [*FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Routage pour les pages Web Forms

L’exemple suivant montre comment définir un itinéraire Web Forms à l’aide de la nouvelle méthode *MapPageRoute* de la classe *route* :

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 introduit la méthode *MapPageRoute* . L’exemple suivant est équivalent à la définition SearchRoute indiquée dans l’exemple précédent, mais utilise la classe *PageRouteHandler* .

[!code-csharp[Main](overview/samples/sample39.cs)]

Le code de l’exemple mappe l’itinéraire à une page physique (dans le premier itinéraire, à `~/search.aspx`). La première définition de route spécifie également que le paramètre nommé searchTerm doit être extrait de l’URL et transmis à la page.

La méthode *MapPageRoute* prend en charge les surcharges de méthode suivantes :

- *MapPageRoute (String routeName, String routeUrl, String physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute (String routeName, String routeUrl, String physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary valeurs par défaut)*
- *MapPageRoute (String routeName, String routeUrl, String physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary valeurs par défaut, contraintes RouteValueDictionary)*

Le paramètre *checkPhysicalUrlAccess* spécifie si l’itinéraire doit vérifier les autorisations de sécurité pour la page physique en cours de routage (dans ce cas, Search. aspx) et les autorisations sur l’URL entrante (dans ce cas, search/{searchterm}). Si la valeur de *checkPhysicalUrlAccess* est *false*, seules les autorisations de l’URL entrante sont vérifiées. Ces autorisations sont définies dans le fichier `Web.config` à l’aide de paramètres tels que les suivants :

[!code-xml[Main](overview/samples/sample40.xml)]

Dans l’exemple de configuration, l’accès est refusé à la page physique `search.aspx` pour tous les utilisateurs, à l’exception de ceux qui appartiennent au rôle d’administrateur. Lorsque le paramètre *checkPhysicalUrlAccess* a la valeur *true* (valeur par défaut), seuls les utilisateurs administrateurs sont autorisés à accéder à l’URL/search/{searchterm}, car la page physique Search. aspx est limitée aux utilisateurs de ce rôle. Si *checkPhysicalUrlAccess* est défini sur *false* et que le site est configuré comme indiqué dans l’exemple précédent, tous les utilisateurs authentifiés sont autorisés à accéder à l’URL/search/{searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Lecture des informations de routage dans une page Web Forms

Dans le code de la page physique Web Forms, vous pouvez accéder aux informations que le routage a extraites de l’URL (ou d’autres informations qu’un autre objet a ajoutées à l’objet *RouteData* ) à l’aide de deux nouvelles propriétés : *HttpRequest. RequestContext* et *page. RouteData*. (*Page. RouteData* inclut *HttpRequest. RequestContext. RouteData*.) L’exemple suivant montre comment utiliser *page. RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Le code extrait la valeur qui a été transmise pour le paramètre searchTerm, comme défini dans l’exemple d’itinéraire précédent. Examinez l’URL de requête suivante :

[!code-console[Main](overview/samples/sample42.cmd)]

Lorsque cette demande est effectuée, le mot « Scott » s’affiche dans la page `search.aspx`.

#### <a name="accessing-routing-information-in-markup"></a>Accès aux informations de routage dans le balisage

La méthode décrite dans la section précédente montre comment obtenir des données de routage dans le code d’une page de Web Forms. Vous pouvez également utiliser des expressions dans le balisage qui vous permettent d’accéder aux mêmes informations. Les générateurs d’expressions sont un moyen puissant et élégant d’utiliser du code déclaratif. (Pour plus d’informations, consultez l’entrée [Express avec les générateurs d’expressions personnalisées](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) sur le blog de Phil Haack.)

ASP.NET 4 comprend deux nouveaux générateurs d’expressions pour le routage Web Forms. L’exemple suivant montre comment les utiliser.

[!code-aspx[Main](overview/samples/sample43.aspx)]

Dans l’exemple, l’expression *RouteUrl* est utilisée pour définir une URL basée sur un paramètre d’itinéraire. Cela vous évite d’avoir à coder en dur l’URL complète dans le balisage et vous permet de modifier la structure d’URL ultérieurement sans avoir à modifier ce lien.

En fonction de l’itinéraire défini précédemment, ce balisage génère l’URL suivante :

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET utilise automatiquement l’itinéraire correct (autrement dit, il génère l’URL correcte) en fonction des paramètres d’entrée. Vous pouvez également inclure un nom d’itinéraire dans l’expression, ce qui vous permet de spécifier un itinéraire à utiliser.

L’exemple suivant montre comment utiliser l’expression *RouteValue* .

[!code-aspx[Main](overview/samples/sample45.aspx)]

Quand la page qui contient ce contrôle s’exécute, la valeur « Scott » s’affiche dans l’étiquette.

L’expression *RouteValue* facilite l’utilisation des données d’itinéraire dans le balisage et évite d’avoir à utiliser la syntaxe page. RouteData ["x"] plus complexe dans le balisage.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Utilisation des données d’itinéraire pour les paramètres de contrôle de source de données

La classe *RouteParameter* vous permet de spécifier des données de routage comme valeur de paramètre pour les requêtes dans un contrôle de source de données. Il [fonctionne très bien comme la](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) classe, comme illustré dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample46.aspx)]

Dans ce cas, la valeur du paramètre d’itinéraire searchTerm sera utilisée pour le paramètre @companyname dans l’instruction *Select* .

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Définition des ID client

La nouvelle propriété *ClientIDMode* résout un problème de longue durée dans ASP.net, à savoir comment les contrôles créent l’attribut *ID* pour les éléments qu’ils restituent. Il est important de connaître l’attribut *ID* pour les éléments rendus si votre application comprend un script client qui fait référence à ces éléments.

L’attribut *ID* dans le code HTML rendu pour les contrôles serveur Web est généré en fonction de la propriété *ClientID* du contrôle. Jusqu’à ASP.NET 4, l’algorithme de génération de l’attribut *ID* à partir de la propriété *ClientID* était de concaténer le conteneur d’attribution de noms (le cas échéant) avec l’ID et, dans le cas de contrôles répétés (comme dans les contrôles de données), d’ajouter un préfixe et un numéro séquentiel. Bien que cela ait toujours garanti que les ID des contrôles dans la page soient uniques, l’algorithme a généré des ID de contrôle qui n’étaient pas prévisibles et étaient donc difficiles à référencer dans le script client.

La nouvelle propriété *ClientIDMode* vous permet de spécifier plus précisément la manière dont l’ID client est généré pour les contrôles. Vous pouvez définir la propriété *ClientIDMode* pour n’importe quel contrôle, y compris pour la page. Les paramètres possibles sont les suivants :

- *AutoID* : il s’agit de l’équivalent de l’algorithme pour générer les valeurs des propriétés *ClientID* qui ont été utilisées dans les versions antérieures de ASP.net.
- *Static* : spécifie que la valeur *ClientID* sera identique à celle de l’ID sans concaténer les ID des conteneurs d’attribution de noms parents. Cela peut être utile dans les contrôles utilisateur Web. Étant donné qu’un contrôle utilisateur Web peut se trouver sur des pages différentes et dans des contrôles conteneurs différents, il peut être difficile d’écrire un script client pour les contrôles qui utilisent l’algorithme *AutoID* , car vous ne pouvez pas prédire les valeurs d’ID.
- *Prévisible* : cette option est principalement destinée à être utilisée dans les contrôles de données qui utilisent des modèles répétitifs. Elle concatène les propriétés d’ID des conteneurs d’attribution de noms du contrôle, mais les valeurs *ClientID* générées ne contiennent pas de chaînes telles que « ctlxxx ». Ce paramètre fonctionne conjointement avec la propriété *ClientIDRowSuffix* du contrôle. Vous définissez la propriété *ClientIDRowSuffix* sur le nom d’un champ de données, et la valeur de ce champ est utilisée comme suffixe pour la valeur *ClientID* générée. En général, vous utilisez la clé primaire d’un enregistrement de données comme valeur *ClientIDRowSuffix* .
- *Inherit* : ce paramètre est le comportement par défaut pour les contrôles. elle spécifie que la génération d’ID d’un contrôle est identique à celle de son parent.

Vous pouvez définir la propriété *ClientIDMode* au niveau de la page. Cela définit la valeur *ClientIDMode* par défaut pour tous les contrôles de la page active.

La valeur *ClientIDMode* par défaut au niveau de la page est *AutoID*, et la valeur *ClientIDMode* par défaut au niveau du contrôle est *inherit*. Par conséquent, si vous ne définissez pas cette propriété n’importe où dans votre code, tous les contrôles auront comme valeur par défaut l’algorithme *AutoID* .

Vous définissez la valeur au niveau de la page dans la directive *@ page* , comme indiqué dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample47.aspx)]

Vous pouvez également définir la valeur *ClientIDMode* dans le fichier de configuration, au niveau de l’ordinateur (machine) ou au niveau de l’application. Cela définit le paramètre *ClientIDMode* par défaut pour tous les contrôles dans toutes les pages de l’application. Si vous définissez la valeur au niveau de l’ordinateur, elle définit le paramètre *ClientIDMode* par défaut pour tous les sites Web sur cet ordinateur. L’exemple suivant montre le paramètre *ClientIDMode* dans le fichier de configuration :

[!code-xml[Main](overview/samples/sample48.xml)]

Comme indiqué précédemment, la valeur de la propriété *ClientID* est dérivée du conteneur d’attribution de noms pour le parent d’un contrôle. Dans certains scénarios, par exemple quand vous utilisez des pages maîtres, les contrôles peuvent se retrouver avec des ID tels que ceux figurant dans le code HTML suivant :

[!code-html[Main](overview/samples/sample49.html)]

Bien que l’élément *d’entrée* affiché dans le balisage (à partir d’un contrôle *TextBox* ) ne soit que deux conteneurs d’attribution de noms en profondeur dans la page (les contrôles *ContentPlaceHolder* imbriqués), en raison de la façon dont les pages maîtres sont traitées, le résultat final est un ID de contrôle similaire à ce qui suit :

[!code-console[Main](overview/samples/sample50.cmd)]

Cet ID est garanti comme étant unique dans la page, mais il est inutilement long dans la plupart des cas. Imaginez que vous souhaitez réduire la longueur de l’ID rendu et avoir davantage de contrôle sur la façon dont l’ID est généré. (Par exemple, vous voulez éliminer les préfixes « ctlxxx ».) Le moyen le plus simple pour y parvenir consiste à définir la propriété *ClientIDMode* comme indiqué dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample51.aspx)]

Dans cet exemple, la propriété *ClientIDMode* est définie sur *static* pour l’élément *NamingPanel* le plus à l’extérieur, et définie sur *Predictable* pour l’élément *NamingControl* interne. Ces paramètres entraînent le balisage suivant (le reste de la page et la page maître sont supposés être les mêmes que dans l’exemple précédent) :

[!code-html[Main](overview/samples/sample52.html)]

Le paramètre *static* a pour effet de réinitialiser la hiérarchie d’attribution de noms pour tous les contrôles à l’intérieur de l’élément *NamingPanel* le plus à l’extérieur, et d’éliminer les ID *CONTENTPLACEHOLDER* et *masterpage* de l’ID généré. (L’attribut *Name* des éléments rendus n’est pas affecté, donc la fonctionnalité normale ASP.net est conservée pour les événements, l’état d’affichage, etc.) Un effet secondaire de la réinitialisation de la hiérarchie d’attribution de noms est que même si vous déplacez le balisage des éléments *NamingPanel* vers un autre contrôle *ContentPlaceHolder* , les ID des clients rendus restent les mêmes.

> [!NOTE]
> Notez que vous devez vous assurer que les ID de contrôle rendus sont uniques. Si ce n’est pas le cas, il peut interrompre toute fonctionnalité qui requiert des ID uniques pour des éléments HTML individuels, tels que la fonction client *document. getElementById* .

#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Création d’ID de client prévisibles dans des contrôles liés aux données

Les valeurs *ClientID* générées pour les contrôles dans un contrôle de liste lié aux données par l’algorithme hérité peuvent être longues et ne sont pas vraiment prévisibles. La fonctionnalité *ClientIDMode* peut vous aider à mieux contrôler la façon dont ces ID sont générés.

Le balisage de l’exemple suivant comprend un contrôle *ListView* :

[!code-aspx[Main](overview/samples/sample53.aspx)]

Dans l’exemple précédent, les propriétés *ClientIDMode* et *RowClientIDRowSuffix* sont définies dans le balisage. La propriété *ClientIDRowSuffix* peut être utilisée uniquement dans les contrôles liés aux données, et son comportement diffère selon le contrôle que vous utilisez. Les différences sont les suivantes :

- Contrôle *GridView* : vous pouvez spécifier le nom d’une ou plusieurs colonnes de la source de données, qui sont combinées au moment de l’exécution pour créer les ID client. Par exemple, si vous affectez à *RowClientIDRowSuffix* la valeur « ProductName, ProductID », les ID de contrôle pour les éléments rendus auront un format similaire à ce qui suit :

- [!code-console[Main](overview/samples/sample54.cmd)]

- Contrôle *ListView* : vous pouvez spécifier une colonne unique dans la source de données qui est ajoutée à l’ID client. Par exemple, si vous affectez à *ClientIDRowSuffix* la valeur « ProductName », les ID de contrôle rendus auront un format similaire à ce qui suit :

- [!code-console[Main](overview/samples/sample55.cmd)]

- Dans ce cas, la fin 1 est dérivée de l’ID de produit de l’élément de données actuel.

- Contrôle *Repeater* : ce contrôle ne prend pas en charge la propriété *ClientIDRowSuffix* . Dans un contrôle *Repeater* , l’index de la ligne actuelle est utilisé. Lorsque vous utilisez ClientIDMode = "prévisible" avec un contrôle *Repeater* , les ID clients sont générés au format suivant :

- [!code-console[Main](overview/samples/sample56.cmd)]

- La valeur 0 de fin est l’index de la ligne actuelle.

Les contrôles *FormView* et *DetailsView* n’affichent pas plusieurs lignes. ils ne prennent donc pas en charge la propriété *ClientIDRowSuffix* .

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Persistance de la sélection de lignes dans les contrôles de données

Les contrôles *GridView* et *ListView* peuvent permettre aux utilisateurs de sélectionner une ligne. Dans les versions précédentes de ASP.NET, la sélection a été basée sur l’index de ligne sur la page. Par exemple, si vous sélectionnez le troisième élément sur la page 1 et que vous passez à la page 2, le troisième élément de cette page est sélectionné.

La sélection persistante n’a initialement été prise en charge que dans les projets Dynamic Data dans le .NET Framework 3,5 SP1. Lorsque cette fonctionnalité est activée, l’élément actuellement sélectionné est basé sur la clé de données de l’élément. Cela signifie que si vous sélectionnez la troisième ligne sur la page 1 et que vous passez à la page 2, rien n’est sélectionné sur la page 2. Lorsque vous revenez à la page 1, la troisième ligne est toujours sélectionnée. La sélection rendue persistante est maintenant prise en charge pour les contrôles *GridView* et *ListView* dans tous les projets à l’aide de la propriété *EnablePersistedSelection* , comme indiqué dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Contrôle Chart ASP.NET

Le contrôle ASP.NET *Chart* développe les offres de visualisation des données dans la .NET Framework. À l’aide du contrôle *Chart* , vous pouvez créer des pages ASP.net avec des graphiques intuitifs et visuellement attrayants pour une analyse statistique ou financière complexe. Le contrôle ASP.NET *Chart* a été introduit en tant que module complémentaire de la version 3,5 SP1 de .NET Framework et fait partie de la version .NET Framework 4.

Le contrôle comprend les fonctionnalités suivantes :

- 35 types de graphiques distincts.
- Nombre illimité de zones de graphique, titres, légendes et annotations.
- Un large éventail de paramètres d’apparence pour tous les éléments de graphique.
- prise en charge 3D pour la plupart des types de graphiques.
- Étiquettes de données intelligentes qui peuvent être ajustées automatiquement autour des points de données.
- Franges, séparateurs d’échelle et mise à l’échelle logarithmique.
- Plus de 50 formules financières et statistiques pour l'analyse et la transformation de données.
- Liaison et manipulation simples des données de graphique.
- Prise en charge des formats de données courants, tels que les dates, les heures et les devises.
- Prise en charge de l’interactivité et de la personnalisation pilotée par les événements, y compris les événements Click client à l’aide d’Ajax.
- Gestion d'état.
- Flux binaires.

Les figures suivantes présentent des exemples de graphiques financiers produits par le contrôle Chart ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Figure 2 : exemples de contrôles Chart ASP.NET

Pour obtenir plus d’exemples d’utilisation du contrôle Chart ASP.NET, téléchargez l’exemple de code sur la page [exemples d’environnement pour les contrôles Microsoft Chart](https://go.microsoft.com/fwlink/?LinkId=128300) sur le site Web MSDN. Vous trouverez d’autres exemples de contenu de la communauté sur le [Forum de contrôle graphique](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Ajout du contrôle Chart à une page ASP.NET

L’exemple suivant montre comment ajouter un contrôle *Chart* à une page ASP.net à l’aide du balisage. Dans l’exemple, le contrôle *Chart* produit un histogramme pour les points de données statiques.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Utilisation de graphiques 3D

Le contrôle *Chart* contient une collection *ChartAreas* , qui peut contenir des objets *ChartArea* qui définissent les caractéristiques des zones de graphique. Par exemple, pour utiliser le langage 3D pour une zone de graphique, utilisez la propriété *Area3DStyle* comme dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample59.aspx)]

La figure ci-dessous illustre un graphique 3D avec quatre séries de type graphique à *barres* .

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Figure 3:3-graphique à barres D

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Utilisation des séparateurs d’échelle et des échelles logarithmiques

Les séparations d’échelle et les échelles logarithmiques sont deux méthodes supplémentaires pour ajouter une sophistication au graphique. Ces fonctionnalités sont spécifiques à chaque axe dans une zone de graphique. Par exemple, pour utiliser ces fonctionnalités sur l’axe des Y principal d’une zone de graphique, utilisez les propriétés *AxisY. IsLogarithmic* et *ScaleBreakStyle* dans un objet *ChartArea* . L’extrait de code suivant montre comment utiliser des séparations d’échelle sur l’axe des Y principal.

[!code-aspx[Main](overview/samples/sample60.aspx)]

La figure ci-dessous montre l’axe Y avec les séparations d’échelle activées.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Figure 4 : séparations d’échelle

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrage des données avec le contrôle QueryExtender

Pour les développeurs qui créent des pages Web pilotées par les données, une tâche très courante consiste à filtrer les données. Cela a traditionnellement été effectué en créant des clauses *Where* dans les contrôles de source de données. Cette approche peut être compliquée et, dans certains cas, la syntaxe *Where* ne vous permet pas de tirer parti des fonctionnalités complètes de la base de données sous-jacente.

Pour faciliter le filtrage, un nouveau contrôle *QueryExtender* a été ajouté dans ASP.net 4. Ce contrôle peut être ajouté aux contrôles *EntityDataSource* ou *LinqDataSource* afin de filtrer les données retournées par ces contrôles. Étant donné que le contrôle *QueryExtender* s’appuie sur LINQ, le filtre est appliqué sur le serveur de base de données avant que les données ne soient envoyées à la page, ce qui se traduit par des opérations très efficaces.

Le contrôle *QueryExtender* prend en charge plusieurs options de filtre. Les sections suivantes décrivent ces options et fournissent des exemples de leur utilisation.

#### <a name="search"></a>Rechercher

Pour l’option de recherche, le contrôle *QueryExtender* effectue une recherche dans les champs spécifiés. Dans l’exemple suivant, le contrôle utilise le texte entré dans le contrôle TextBoxSearch et recherche son contenu dans le `ProductName` et `Supplier.CompanyName` colonnes dans les données retournées à partir du contrôle *LinqDataSource* .

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Range

L’option Range est similaire à l’option de recherche, mais elle spécifie une paire de valeurs pour définir la plage. Dans l’exemple suivant, le contrôle *QueryExtender* recherche dans la colonne `UnitPrice` dans les données retournées par le contrôle *LinqDataSource* . La plage est lue à partir des contrôles TextBoxFrom et TextBoxTo sur la page.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

L’option expression de propriété vous permet de définir une comparaison avec une valeur de propriété. Si l’expression prend la *valeur true*, les données en cours d’examen sont retournées. Dans l’exemple suivant, le contrôle *QueryExtender* filtre les données en comparant les données de la colonne `Discontinued` à la valeur du contrôle CheckBoxDiscontinued sur la page.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Enfin, vous pouvez spécifier une expression personnalisée à utiliser avec le contrôle *QueryExtender* . Cette option vous permet d’appeler une fonction dans la page qui définit la logique de filtre personnalisée. L’exemple suivant montre comment spécifier de manière déclarative une expression personnalisée dans le contrôle *QueryExtender* .

[!code-aspx[Main](overview/samples/sample64.aspx)]

L’exemple suivant illustre la fonction personnalisée appelée par le contrôle *QueryExtender* . Dans ce cas, au lieu d’utiliser une requête de base de données qui comprend une clause *Where* , le code utilise une requête LINQ pour filtrer les données.

[!code-csharp[Main](overview/samples/sample65.cs)]

Ces exemples montrent qu’une seule expression est utilisée dans le contrôle *QueryExtender* à la fois. Toutefois, vous pouvez inclure plusieurs expressions dans le contrôle *QueryExtender* .

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Expressions de code encodées en HTML

Certains sites ASP.NET (surtout avec ASP.NET MVC) s’appuient essentiellement sur l’utilisation de `<%`= `expression %>` syntaxe (souvent appelée « code pépites ») pour écrire du texte dans la réponse. Lorsque vous utilisez des expressions de code, il est facile d’oublier le codage HTML du texte, si le texte provient d’une entrée d’utilisateur, il peut laisser des pages ouvertes à une attaque XSS (cross site scripting).

ASP.NET 4 introduit la nouvelle syntaxe suivante pour les expressions de code :

[!code-aspx[Main](overview/samples/sample66.aspx)]

Cette syntaxe utilise l’encodage HTML par défaut lors de l’écriture dans la réponse. Cette nouvelle expression se traduit en réalité comme suit :

[!code-aspx[Main](overview/samples/sample67.aspx)]

Par exemple, &lt;% : Request ["UserInput"]%&gt; effectue un encodage HTML sur la valeur de la *demande ["UserInput"]* .

L’objectif de cette fonctionnalité est de permettre de remplacer toutes les instances de l’ancienne syntaxe par la nouvelle syntaxe, de sorte que vous n’êtes pas obligé de décider à chaque étape celle à utiliser. Toutefois, dans certains cas, le texte en sortie est destiné à être au format HTML ou déjà encodé, auquel cas cela peut entraîner un double encodage.

Dans ces cas, ASP.NET 4 introduit une nouvelle interface, *IHtmlString*, ainsi qu’une implémentation concrète, *HtmlString*. Les instances de ces types vous permettent d’indiquer que la valeur de retour est déjà encodée (ou examinée) pour l’affichage au format HTML, et que par conséquent, la valeur ne doit pas être encodée au format HTML. Par exemple, le code suivant ne doit pas être (et ne pas) être encodé au format HTML :

[!code-aspx[Main](overview/samples/sample68.aspx)]

Les méthodes d’assistance ASP.NET MVC 2 ont été mises à jour pour fonctionner avec cette nouvelle syntaxe afin qu’elles ne soient pas codées en deux, mais uniquement lorsque vous exécutez ASP.NET 4. Cette nouvelle syntaxe ne fonctionne pas lorsque vous exécutez une application à l’aide de ASP.NET 3,5 SP1.

Gardez à l’esprit que cela ne garantit pas la protection contre les attaques XSS. Par exemple, le code HTML qui utilise des valeurs d’attribut qui ne sont pas entre guillemets peut contenir une entrée d’utilisateur qui est toujours sensible. Notez que la sortie des contrôles ASP.NET et des applications auxiliaires MVC ASP.NET comprend toujours des valeurs d’attribut entre guillemets, ce qui est l’approche recommandée.

De même, cette syntaxe n’effectue pas d’encodage JavaScript, par exemple lorsque vous créez une chaîne JavaScript basée sur l’entrée d’utilisateur.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Modifications du modèle de projet

Dans les versions antérieures de ASP.NET, lorsque vous utilisez Visual Studio pour créer un projet de site Web ou un projet d’application Web, les projets résultants contiennent uniquement une page default. aspx, un fichier de `Web.config` par défaut et le dossier `App_Data`, comme indiqué dans l’illustration suivante :

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio prend également en charge un type de projet de site Web vide, qui ne contient aucun fichier, comme illustré dans la figure suivante :

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Le résultat est que pour le débutant, il y a très peu d’instructions sur la façon de créer une application Web de production. Par conséquent, ASP.NET 4 introduit trois nouveaux modèles, l’un pour un projet d’application Web vide et l’autre pour une application Web et un projet de site Web.

#### <a name="empty-web-application-template"></a>Modèle d’application Web vide

Comme son nom l’indique, le modèle d’application Web vide est un projet d’application Web supprimé. Vous sélectionnez ce modèle de projet dans la boîte de dialogue Nouveau projet de Visual Studio, comme illustré dans la figure suivante :

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Cliquez pour afficher l’image en taille réelle](overview/_static/image8.png))

Quand vous créez une application Web ASP.NET vide, Visual Studio crée la disposition de dossier suivante :

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Cela est similaire à la disposition de site Web vide des versions antérieures de ASP.NET, à une exception près. Dans Visual Studio 2010, les projets d’application Web vide et de site Web vide contiennent le fichier `Web.config` minimal suivant qui contient les informations utilisées par Visual Studio pour identifier l’infrastructure ciblée par le projet :

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Sans cette propriété *targetFramework* , Visual Studio cible par défaut le .NET Framework 2,0 afin de préserver la compatibilité lors de l’ouverture d’anciennes applications.

#### <a name="web-application-and-web-site-project-templates"></a>Modèles de projet d’application Web et de site Web

Les deux autres modèles de projet fournis avec Visual Studio 2010 contiennent des modifications majeures. L’illustration suivante montre la disposition du projet qui est créée lorsque vous créez un projet d’application Web. (La disposition d’un projet de site Web est pratiquement identique.)

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Le projet comprend un certain nombre de fichiers qui n’ont pas été créés dans des versions antérieures. En outre, le nouveau projet d’application Web est configuré avec la fonctionnalité d’appartenance de base, qui vous permet de commencer rapidement à sécuriser l’accès à la nouvelle application. En raison de cette inclusion, le fichier `Web.config` pour le nouveau projet comprend des entrées utilisées pour configurer l’appartenance, les rôles et les profils. L’exemple suivant montre le fichier `Web.config` pour un nouveau projet d’application Web. (Dans ce cas, *roleManager* est désactivé.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Cliquez pour afficher l’image en taille réelle](overview/_static/image14.png))

Le projet contient également un deuxième `Web.config` fichier dans le répertoire `Account`. Le deuxième fichier de configuration fournit un moyen de sécuriser l’accès à la page ChangePassword. aspx pour les utilisateurs non connectés. L’exemple suivant montre le contenu du deuxième `Web.config` fichier.

![](overview/_static/image15.png)

Les pages créées par défaut dans les nouveaux modèles de projet contiennent également plus de contenu que dans les versions précédentes. Le projet contient une page maître et un fichier CSS par défaut, et la page par défaut (default. aspx) est configurée pour utiliser la page maître par défaut. Par conséquent, lorsque vous exécutez l’application Web ou le site Web pour la première fois, la page par défaut (page d’hébergement) est déjà fonctionnelle. En fait, il est similaire à la page par défaut qui s’affiche lorsque vous démarrez une nouvelle application MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Cliquez pour afficher l’image en taille réelle](overview/_static/image18.png))

Les modifications apportées aux modèles de projet ont pour but de vous guider dans la création d’une nouvelle application Web. Avec un balisage conforme à XHTML 1,0 strict et correct et une disposition spécifiée à l’aide de CSS, les pages des modèles représentent les meilleures pratiques pour la création d’applications Web ASP.NET 4. Les pages par défaut ont également une disposition à deux colonnes que vous pouvez facilement personnaliser.

Par exemple, imaginez que pour une nouvelle application Web, vous souhaitez modifier certaines des couleurs et insérer le logo de votre société à la place du logo de l’application My ASP.NET. Pour ce faire, vous créez un répertoire sous `Content` pour stocker l’image de votre logo :

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Pour ajouter l’image à la page, ouvrez le fichier `Site.Master`, recherchez l’emplacement où est défini le texte de l’application My ASP.NET, puis remplacez-le par un élément *image* dont l’attribut *src* a pour valeur la nouvelle image de logo, comme dans l’exemple suivant :

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Cliquez pour afficher l’image en taille réelle](overview/_static/image22.png))

Vous pouvez ensuite accéder au fichier site. CSS et modifier les définitions de classe CSS pour modifier la couleur d’arrière-plan de la page ainsi que celle de l’en-tête.

Le résultat de ces modifications est que vous pouvez afficher une page d’hébergement personnalisée avec très peu d’efforts :

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Cliquez pour afficher l’image en taille réelle](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Améliorations CSS

L’une des principales zones de travail dans ASP.NET 4 a été de vous aider à effectuer le rendu du code HTML conforme aux normes HTML les plus récentes. Cela comprend les modifications apportées à la façon dont les contrôles de serveur Web ASP.NET utilisent les styles CSS.

#### <a name="compatibility-setting-for-rendering"></a>Paramètre de compatibilité pour le rendu

Par défaut, lorsqu’une application Web ou un site Web cible le .NET Framework 4, l’attribut *controlRenderingCompatibilityVersion* de l’élément *pages* est défini sur « 4,0 ». Cet élément est défini dans le fichier de `Web.config` au niveau de l’ordinateur et s’applique par défaut à toutes les applications ASP.NET 4 :

[!code-xml[Main](overview/samples/sample69.xml)]

La valeur de *controlRenderingCompatibility* est une chaîne, ce qui permet d’éventuelles nouvelles définitions de version dans les futures versions. Dans la version actuelle, les valeurs suivantes sont prises en charge pour cette propriété :

- "3.5". Ce paramètre indique un rendu et un balisage hérités. Le balisage rendu par les contrôles est de 100% à compatibilité descendante et le paramètre de la propriété *xhtmlConformance* est respecté.
- "4.0". Si la propriété a ce paramètre, les contrôles serveur Web ASP.NET effectuent les opérations suivantes :
- La propriété *xhtmlConformance* est toujours traitée comme « strict ». Par conséquent, les contrôles affichent le balisage XHTML 1,0 strict.
- La désactivation des contrôles non-entrée n’affiche plus les styles non valides.
- les éléments *div* autour des champs masqués sont maintenant stylisés, de sorte qu’ils n’interfèrent pas avec les règles CSS créées par l’utilisateur.
- Les contrôles de menu restituent le balisage qui est sémantiquement correct et conforme aux règles d’accessibilité.
- Les contrôles de validation ne restituent pas de styles intralignes.
- Les contrôles qui ont précédemment rendu border = "0" (les contrôles qui dérivent du contrôle *Table* ASP.net et le contrôle *image* ASP.net) n’affichent plus cet attribut.

#### <a name="disabling-controls"></a>Désactivation de contrôles

Dans ASP.NET 3,5 SP1 et les versions antérieures, le Framework restitue l’attribut *Disabled* dans le balisage HTML pour tout contrôle dont la propriété *Enabled* a la valeur *false*. Toutefois, en fonction de la spécification HTML 4,01, seuls les éléments *d’entrée* doivent avoir cet attribut.

Dans ASP.NET 4, vous pouvez définir la propriété *controlRenderingCompatibilityVersion* sur « 3,5 », comme dans l’exemple suivant :

[!code-xml[Main](overview/samples/sample70.xml)]

Vous pouvez créer un balisage pour un contrôle *label* comme celui qui suit, ce qui désactive le contrôle :

[!code-aspx[Main](overview/samples/sample71.aspx)]

Le contrôle *label* affiche le code HTML suivant :

[!code-html[Main](overview/samples/sample72.html)]

Dans ASP.NET 4, vous pouvez définir le *controlRenderingCompatibilityVersion* sur « 4,0 ». Dans ce cas, seuls les contrôles qui restituent des éléments *d’entrée* rendront un attribut *désactivé* lorsque la propriété *Enabled* du contrôle est définie sur *false*. Les contrôles qui n’affichent pas *d’éléments d’entrée* HTML à la place restituent un attribut de *classe* qui référence une classe CSS que vous pouvez utiliser pour définir une apparence désactivée pour le contrôle. Par exemple, le contrôle *label* indiqué dans l’exemple précédent générerait le balisage suivant :

[!code-html[Main](overview/samples/sample73.html)]

La valeur par défaut de la classe qui a été spécifiée pour ce contrôle est « aspNetDisabled ». Toutefois, vous pouvez modifier cette valeur par défaut en définissant la propriété statique *DisabledCssClass* statique de la classe *WebControl* . Pour les développeurs de contrôles, le comportement à utiliser pour un contrôle spécifique peut également être défini à l’aide de la propriété *SupportsDisabledAttribute* .

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Masquage d’éléments div autour de champs masqués

ASP.NET 2,0 et les versions ultérieures restituent les champs masqués spécifiques au système (tels que l’élément *masqué* utilisé pour stocker les informations d’état d’affichage) à l’intérieur de l’élément *div* afin de se conformer à la norme XHTML. Toutefois, cela peut poser un problème lorsqu’une règle CSS affecte des éléments *div* sur une page. Par exemple, elle peut entraîner l’affichage d’une ligne d’un pixel autour des éléments *div* masqués. Dans ASP.NET 4, les éléments *div* qui encadrent les champs masqués générés par ASP.net ajoutent une référence de classe CSS comme dans l’exemple suivant :

[!code-html[Main](overview/samples/sample74.html)]

Vous pouvez ensuite définir une classe CSS qui s’applique uniquement aux éléments *masqués* générés par ASP.net, comme dans l’exemple suivant :

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Rendu d’une table externe pour les contrôles basés sur un modèle

Par défaut, les contrôles serveur Web ASP.NET suivants qui prennent en charge les modèles sont automatiquement encapsulés dans une table externe qui est utilisée pour appliquer des styles intralignes :

- *FormView*
- *Connexion*
- *PasswordRecovery*
- *ChangePassword*
- *Assistant*
- *CreateUserWizard*

Une nouvelle propriété nommée *RenderOuterTable* a été ajoutée à ces contrôles pour permettre la suppression de la table externe du balisage. Prenons l’exemple suivant d’un contrôle *FormView* :

[!code-aspx[Main](overview/samples/sample76.aspx)]

Ce balisage restitue la sortie suivante sur la page, qui comprend un tableau HTML :

[!code-html[Main](overview/samples/sample77.html)]

Pour empêcher le rendu de la table, vous pouvez définir la propriété *RenderOuterTable* du contrôle *FormView* , comme dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample78.aspx)]

L’exemple précédent restitue la sortie suivante, sans les éléments *table*, *TR*et *TD* :

> Contenu

Cette amélioration peut faciliter le style du contenu du contrôle avec CSS, car aucune balise inattendue n’est restituée par le contrôle.

> [!NOTE]
> Remarque Cette modification désactive la prise en charge de la fonction de mise en forme automatique dans le concepteur Visual Studio 2010, car il n’y a plus d’élément de *table* qui peut héberger des attributs de style générés par l’option de mise en forme automatique.

<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Améliorations du contrôle ListView

Le contrôle *ListView* a été simplifié pour une utilisation dans ASP.net 4. La version antérieure du contrôle nécessitait que vous spécifiiez un modèle de disposition qui contenait un contrôle serveur avec un ID connu. Le balisage suivant montre un exemple classique d’utilisation du contrôle *ListView* dans ASP.net 3,5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

Dans ASP.NET 4, le contrôle *ListView* ne requiert pas de modèle de disposition. Le balisage illustré dans l’exemple précédent peut être remplacé par le balisage suivant :

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Amélioration des contrôles CheckBoxList et RadioButtonList

Dans ASP.NET 3,5, vous pouvez spécifier la disposition pour *CheckBoxList* et *RadioButtonList* à l’aide des deux paramètres suivants :

- *Flow*. Le contrôle restitue des éléments *span* pour contenir son contenu.
- *Table*. Le contrôle restitue un élément *table* pour contenir son contenu.

L’exemple suivant montre un balisage pour chacun de ces contrôles.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Par défaut, les contrôles affichent du code HTML similaire à ce qui suit :

[!code-html[Main](overview/samples/sample82.html)]

Étant donné que ces contrôles contiennent des listes d’éléments, pour restituer du code HTML sémantiquement correct, ils doivent restituer leur contenu à l’aide d’éléments de liste HTML (*Li*). Cela facilite la lecture des pages Web à l’aide de la technologie d’assistance par les utilisateurs et facilite le style des contrôles à l’aide de CSS.

Dans ASP.NET 4, les contrôles *CheckBoxList* et *RadioButtonList* prennent en charge les nouvelles valeurs suivantes pour la propriété *RepeatLayout* :

- *OrderedList* : le contenu est rendu sous la forme d’éléments *Li* dans un élément *ol* .
- *Dispositions UnorderedList* : le contenu est rendu sous la forme d’éléments *Li* dans un élément *UL* .

L’exemple suivant montre comment utiliser ces nouvelles valeurs.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Le balisage ci-dessus génère le code HTML suivant :

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Remarque Si vous affectez à *RepeatLayout* la valeur *OrderedList* ou *dispositions UnorderedList*, la propriété *RepeatDirection* ne peut plus être utilisée et lèvera une exception au moment de l’exécution si la propriété a été définie dans votre balise ou votre code. La propriété n’aurait aucune valeur, car la disposition visuelle de ces contrôles est définie à l’aide de CSS.

<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Améliorations du contrôle Menu

Avant le ASP.NET 4, le contrôle *menu* affichait une série de tableaux html. Cela rendait plus difficile l’application de styles CSS en dehors du paramétrage des propriétés inline et n’était pas non plus conforme aux normes d’accessibilité.

Dans ASP.NET 4, le contrôle restitue désormais du code HTML à l’aide du balisage sémantique qui se compose d’éléments de liste et de liste non triés. L’exemple suivant illustre le balisage dans une page ASP.NET pour le contrôle *menu* .

[!code-aspx[Main](overview/samples/sample85.aspx)]

Lorsque la page est rendue, le contrôle produit le code HTML suivant (le code *OnClick* a été omis par souci de clarté) :

[!code-html[Main](overview/samples/sample86.html)]

En plus des améliorations du rendu, la navigation au clavier du menu a été améliorée grâce à la gestion du focus. Quand le contrôle de *menu* reçoit le focus, vous pouvez utiliser les touches de direction pour naviguer parmi les éléments. Le contrôle *menu* attache désormais également des rôles et des attributs Aria (Rich Internet applications) accessibles Vale aile pour améliorer[l'](http://www.w3.org/TR/wai-aria-practices/#menu "Instructions du menu ARIA")accessibilité.

Les styles pour le contrôle de menu sont rendus dans un bloc de style en haut de la page, plutôt qu’en ligne avec les éléments HTML rendus. Si vous souhaitez prendre le contrôle total sur le style du contrôle, vous pouvez affecter à la nouvelle propriété *IncludeStyleBlock* la *valeur false*, auquel cas le bloc de style n’est pas émis. Une façon d’utiliser cette propriété consiste à utiliser la fonctionnalité de mise en forme automatique dans le concepteur Visual Studio pour définir l’apparence du menu. Vous pouvez ensuite exécuter la page, ouvrir la page source, puis copier le bloc de style rendu dans un fichier CSS externe. Dans Visual Studio, annulez le style et définissez *IncludeStyleBlock* sur *false*. Le résultat est que l’apparence du menu est définie à l’aide de styles dans une feuille de style externe.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Assistant et contrôles CreateUserWizard

L' *assistant* ASP.net et les contrôles *CreateUserWizard* prennent en charge les modèles qui vous permettent de définir le code html qu’ils restituent. (*CreateUserWizard* dérive de l' *Assistant*.) L’exemple suivant illustre le balisage pour un contrôle *CreateUserWizard* entièrement basé sur un modèle :

[!code-aspx[Main](overview/samples/sample87.aspx)]

Le contrôle restitue le code HTML similaire à ce qui suit :

[!code-html[Main](overview/samples/sample88.html)]

Dans ASP.NET 3,5 SP1, bien que vous puissiez modifier le contenu du modèle, vous disposez toujours d’un contrôle limité sur la sortie du contrôle de l' *Assistant* . Dans ASP.NET 4, vous pouvez créer un modèle *LayoutTemplate* et insérer des contrôles d' *espace réservé* (à l’aide de noms réservés) pour spécifier la façon dont vous souhaitez que le contrôle de l' *Assistant* s’affiche. L’exemple suivant illustre cela :

[!code-aspx[Main](overview/samples/sample89.aspx)]

L’exemple contient les espaces réservés nommés suivants dans l’élément *LayoutTemplate* :

- *headerPlaceholder* : au moment de l’exécution, cette valeur est remplacée par le contenu de l’élément *HeaderTemplate* .
- *sideBarPlaceholder* : au moment de l’exécution, cette valeur est remplacée par le contenu de l’élément *SideBarTemplate* .
- *wizardStepPlaceHolder* : au moment de l’exécution, cette valeur est remplacée par le contenu de l’élément *WizardStepTemplate* .
- *navigationPlaceholder* : au moment de l’exécution, elle est remplacée par tous les modèles de navigation que vous avez définis.

Le balisage de l’exemple qui utilise des espaces réservés restitue le code HTML suivant (sans le contenu réellement défini dans les modèles) :

[!code-html[Main](overview/samples/sample90.html)]

Le seul code HTML qui n’est pas défini par l’utilisateur est un élément *span* . (Nous pensons que dans les versions ultérieures, même l’élément *span* ne sera pas rendu.) Cela vous donne maintenant un contrôle total sur pratiquement tout le contenu généré par le contrôle de l' *Assistant* .

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC a été introduit comme Framework complémentaire pour ASP.NET 3,5 SP1 en mars 2009. Visual Studio 2010 comprend ASP.NET MVC 2, qui comprend de nouvelles fonctionnalités et fonctionnalités.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Prise en charge des zones

Les zones vous permettent de regrouper des contrôleurs et des vues dans des sections d’une grande application dans un isolement relatif à partir d’autres sections. Chaque zone peut être implémentée en tant que projet MVC ASP.NET distinct qui peut ensuite être référencé par l’application principale. Cela permet de gérer la complexité lorsque vous générez une grande application et facilite la collaboration de plusieurs équipes sur une même application.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Prise en charge de la validation des attributs d’annotation de données

Les attributs *DataAnnotations* vous permettent d’attacher une logique de validation à un modèle à l’aide des attributs de métadonnées. Les attributs *DataAnnotations* ont été introduits dans ASP.NET Dynamic Data dans ASP.net 3,5 SP1. Ces attributs ont été intégrés dans le classeur de modèles par défaut et fournissent un moyen piloté par les métadonnées pour valider l’entrée d’utilisateur.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Assistances basées sur un modèle

Les applications auxiliaires basées sur un modèle vous permettent d’associer automatiquement des modèles de modification et d’affichage avec des types de données. Par exemple, vous pouvez utiliser un programme d’assistance de modèle pour spécifier qu’un élément d’interface utilisateur de sélecteur de dates est automatiquement restitué pour une valeur *System. DateTime* . Cela est similaire aux modèles de champ dans ASP.NET Dynamic Data.

Les méthodes d’assistance *html. EditorFor* et *html. DisplayFor* offrent une prise en charge intégrée du rendu des types de données standard, ainsi que des objets complexes avec plusieurs propriétés. Ils personnalisent également le rendu en vous permettant d’appliquer des attributs d’annotation de données tels que *DisplayName* et *ScaffoldColumn* à l’objet *ViewModel* .

Souvent, vous souhaitez personnaliser la sortie des applications auxiliaires de l’interface utilisateur et avoir un contrôle total sur ce qui est généré. Les méthodes d’assistance *html. EditorFor* et *html. DisplayFor* prennent en charge cette méthode à l’aide d’un mécanisme de création de modèles qui vous permet de définir des modèles externes qui peuvent substituer et contrôler la sortie rendue. Les modèles peuvent être rendus individuellement pour une classe.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamic Data

Dynamic Data a été introduite dans la version .NET Framework 3,5 SP1 à mi-2008. Cette fonctionnalité offre de nombreuses améliorations pour créer des applications pilotées par les données, notamment les suivantes :

- Une expérience RAD pour créer rapidement un site Web piloté par les données.
- Validation automatique basée sur les contraintes définies dans le modèle de données.
- La possibilité de modifier facilement le balisage généré pour les champs dans les contrôles *GridView* et *DetailsView* à l’aide des modèles de champ qui font partie de votre projet Dynamic Data.

> [!NOTE]
> Remarque pour plus d’informations, consultez la [documentation Dynamic Data](https://msdn.microsoft.com/library/cc488545.aspx) dans MSDN Library.

Pour ASP.NET 4, Dynamic Data a été amélioré pour offrir aux développeurs encore plus de puissance pour créer rapidement des sites Web pilotés par les données.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Activation d’Dynamic Data pour les projets existants

Dynamic Data fonctionnalités fournies dans le .NET Framework 3,5 SP1 ont introduit de nouvelles fonctionnalités telles que les suivantes :

- Modèles de champ : ils fournissent des modèles basés sur les types de données pour les contrôles liés aux données. Les modèles de champs offrent un moyen plus simple de personnaliser l’apparence des contrôles de données que d’utiliser des champs de modèle pour chaque champ.
- Validation : Dynamic Data vous permet d’utiliser des attributs sur des classes de données pour spécifier la validation pour les scénarios courants, tels que les champs obligatoires, la vérification de la plage, la vérification des types, les critères spéciaux à l’aide d’expressions régulières et la validation personnalisée. La validation est appliquée par les contrôles de données.

Toutefois, ces fonctionnalités ont les exigences suivantes :

- La couche d’accès aux données devait reposer sur Entity Framework ou LINQ to SQL.
- Les seuls contrôles de source de données pris en charge pour ces fonctionnalités étaient les contrôles *EntityDataSource* ou *LinqDataSource* .
- Les fonctionnalités nécessitaient un projet Web qui avait été créé à l’aide des modèles d’entités Dynamic Data ou Dynamic Data afin que tous les fichiers nécessaires à la prise en charge de la fonctionnalité soient disponibles.

L’un des principaux objectifs de la prise en charge de Dynamic Data dans ASP.NET 4 consiste à activer la nouvelle fonctionnalité de Dynamic Data pour n’importe quelle application ASP.NET. L’exemple suivant montre un balisage pour les contrôles qui peuvent tirer parti des fonctionnalités de Dynamic Data dans une page existante.

[!code-aspx[Main](overview/samples/sample91.aspx)]

Dans le code de la page, le code suivant doit être ajouté pour permettre Dynamic Data la prise en charge de ces contrôles :

[!code-csharp[Main](overview/samples/sample92.cs)]

Quand le contrôle *GridView* est en mode édition, Dynamic Data valide automatiquement que les données entrées sont au format approprié. Si ce n’est pas le cas, un message d’erreur s’affiche.

Cette fonctionnalité offre également d’autres avantages, tels que la possibilité de spécifier des valeurs par défaut pour le mode insertion. Sans Dynamic Data, pour implémenter une valeur par défaut pour un champ, vous devez effectuer un attachement à un événement, localiser le contrôle (à l’aide de *FindControl*) et définir sa valeur. Dans ASP.NET 4, l’appel *EnableDynamicData* prend en charge un deuxième paramètre qui vous permet de passer des valeurs par défaut pour n’importe quel champ de l’objet, comme illustré dans cet exemple :

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Syntaxe déclarative du contrôle DynamicDataManager

Le contrôle *DynamicDataManager* a été amélioré afin que vous puissiez le configurer de façon déclarative, comme avec la plupart des contrôles dans ASP.net, et non uniquement dans le code. Le balisage du contrôle *DynamicDataManager* se présente comme dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample94.aspx)]

Ce balisage Active Dynamic Data comportement pour le contrôle GridView1 référencé dans la section *DataControls* du contrôle *DynamicDataManager* .

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Modèles d’entité

Les modèles d’entité offrent un nouveau moyen de personnaliser la disposition des données sans avoir à créer une page personnalisée. Les modèles de page utilisent le contrôle *FormView* (au lieu du contrôle *DetailsView* , tel qu’il est utilisé dans les modèles de page dans les versions antérieures de Dynamic Data) et le contrôle *DynamicEntity* pour afficher les modèles d’entité. Cela vous donne davantage de contrôle sur le balisage rendu par Dynamic Data.

La liste suivante affiche la nouvelle disposition de répertoire du projet qui contient des modèles d’entité :

[!code-console[Main](overview/samples/sample95.cmd)]

Le répertoire `EntityTemplate` contient des modèles permettant d’afficher des objets de modèle de données. Par défaut, les objets sont rendus à l’aide du modèle `Default.ascx`, qui fournit le balisage qui ressemble exactement au balisage créé par le contrôle *DetailsView* utilisé par Dynamic Data dans ASP.net 3,5 SP1. L’exemple suivant illustre le balisage pour le contrôle `Default.ascx` :

[!code-aspx[Main](overview/samples/sample96.aspx)]

Les modèles par défaut peuvent être modifiés pour modifier l’apparence de l’ensemble du site. Il existe des modèles pour les opérations d’affichage, de modification et d’insertion. De nouveaux modèles peuvent être ajoutés en fonction du nom de l’objet de données afin de modifier l’apparence d’un seul type d’objet. Par exemple, vous pouvez ajouter le modèle suivant :

[!code-console[Main](overview/samples/sample97.cmd)]

Le modèle peut contenir le balisage suivant :

[!code-aspx[Main](overview/samples/sample98.aspx)]

Les nouveaux modèles d’entité s’affichent sur une page à l’aide du nouveau contrôle *DynamicEntity* . Au moment de l’exécution, ce contrôle est remplacé par le contenu du modèle d’entité. Le balisage suivant montre le contrôle *FormView* dans le modèle de page `Detail.aspx` qui utilise le modèle d’entité. Notez l’élément *DynamicEntity* dans le balisage.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Nouveaux modèles de champ pour les URL et les adresses de messagerie

ASP.NET 4 introduit deux nouveaux modèles de champs prédéfinis, `EmailAddress.ascx` et `Url.ascx`. Ces modèles sont utilisés pour les champs marqués comme *EmailAddress* ou *URL* avec l’attribut *DataType* . Pour les objets *EmailAddress* , le champ est affiché sous la forme d’un lien hypertexte créé à l’aide du protocole *mailto :* . Lorsque les utilisateurs cliquent sur le lien, il ouvre le client de messagerie de l’utilisateur et crée un message squelette. Les objets de type *URL* sont affichés sous forme de liens hypertexte ordinaires.

L’exemple suivant montre comment les champs sont marqués.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Création de liens avec le contrôle DynamicHyperLink

Dynamic Data utilise la nouvelle fonctionnalité de routage qui a été ajoutée dans le .NET Framework 3,5 SP1 pour contrôler les URL que les utilisateurs finaux voient quand ils accèdent au site Web. Le nouveau contrôle *DynamicHyperLink* facilite la création de liens vers les pages d’un site Dynamic Data. L’exemple suivant montre comment utiliser le contrôle *DynamicHyperLink* :

[!code-aspx[Main](overview/samples/sample101.aspx)]

Ce balisage crée un lien qui pointe vers la page de liste pour la table `Products` en fonction des itinéraires définis dans le fichier `Global.asax`. Le contrôle utilise automatiquement le nom de la table par défaut sur lequel est basée la page Dynamic Data.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Prise en charge de l’héritage dans le modèle de données

Le Entity Framework et le LINQ to SQL prennent en charge l’héritage dans leurs modèles de données. Il peut s’agir, par exemple, d’une base de données qui contient une table `InsurancePolicy`. Il peut également contenir des tables `CarPolicy` et `HousePolicy` qui ont les mêmes champs que `InsurancePolicy`, puis ajouter d’autres champs. Dynamic Data a été modifiée pour comprendre les objets hérités dans le modèle de données et pour prendre en charge la génération de modèles automatique pour les tables héritées.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Prise en charge des relations plusieurs-à-plusieurs (Entity Framework uniquement)

La Entity Framework offre une prise en charge complète des relations plusieurs-à-plusieurs entre les tables, qui est implémentée en exposant la relation en tant que collection sur un objet d' *entité* . De nouveaux modèles de champs `ManyToMany.ascx` et `ManyToMany_Edit.ascx` ont été ajoutés pour prendre en charge l’affichage et la modification des données impliquées dans des relations plusieurs-à-plusieurs.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nouveaux attributs pour contrôler l’affichage et les énumérations de prise en charge

Le *DisplayAttribute* a été ajouté pour vous permettre de contrôler davantage la façon dont les champs sont affichés. L’attribut *DisplayName* des versions antérieures de Dynamic Data vous permettait de modifier le nom utilisé comme légende pour un champ. La nouvelle classe *DisplayAttribute* vous permet de spécifier d’autres options pour l’affichage d’un champ, telles que l’ordre dans lequel un champ est affiché et si un champ sera utilisé comme filtre. L’attribut fournit également un contrôle indépendant du nom utilisé pour les étiquettes dans un contrôle *GridView* , du nom utilisé dans un contrôle *DetailsView* , du texte d’aide pour le champ et du filigrane utilisé pour le champ (si le champ accepte la saisie de texte).

La classe *EnumDataTypeAttribute* a été ajoutée pour vous permettre de mapper les champs aux énumérations. Lorsque vous appliquez cet attribut à un champ, vous spécifiez un type d’énumération. Dynamic Data utilise le nouveau modèle de champ `Enumeration.ascx` pour créer l’interface utilisateur permettant d’afficher et de modifier les valeurs d’énumération. Le modèle mappe les valeurs de la base de données aux noms de l’énumération.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Prise en charge améliorée des filtres

Dynamic Data 1,0 fourni avec des filtres intégrés pour les colonnes booléennes et les colonnes de clé étrangère. Les filtres ne vous permettaient pas de spécifier s’ils étaient affichés ou dans l’ordre dans lequel ils ont été affichés. Le nouvel attribut *DisplayAttribute* résout ces deux problèmes en vous donnant la possibilité de contrôler si une colonne est affichée en tant que filtre et dans quel ordre il sera affiché.

Une amélioration supplémentaire est que la prise en charge du filtrage a été[réécrite pour utiliser la nouvelle](#0.2__QueryExtender "_QueryExtender") fonctionnalité de Web Forms. Cela vous permet de créer des filtres sans avoir besoin de connaître le contrôle de source de données avec lequel les filtres seront utilisés. Avec ces extensions, les filtres ont également été convertis en contrôles de modèle, ce qui vous permet d’en ajouter de nouveaux. Enfin, la classe *DisplayAttribute* mentionnée précédemment permet le remplacement du filtre par défaut, de la même façon que la procédure *UIHint* autorise le remplacement du modèle de champ par défaut pour une colonne.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Améliorations du développement Web dans Visual Studio 2010

Le développement Web dans Visual Studio 2010 a été amélioré pour une plus grande compatibilité CSS, une productivité accrue grâce à des extraits de code HTML et ASP.NET et de nouveaux JavaScript IntelliSense dynamiques.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Compatibilité CSS améliorée

Le concepteur Visual Web Developer dans Visual Studio 2010 a été mis à jour pour améliorer la conformité aux normes CSS 2,1. Le concepteur conserve mieux l’intégrité de la source HTML et est plus fiable que dans les versions précédentes de Visual Studio. Dans le cadre des coulisses, des améliorations architecturales ont également été apportées pour améliorer les futures améliorations du rendu, de la mise en page et de la facilité de maintenance.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>Extraits de code HTML et JavaScript

Dans l’éditeur HTML, IntelliSense complète automatiquement les noms de balises. La fonctionnalité d’extraits de code IntelliSense complète automatiquement les balises entières, et bien plus encore. Dans Visual Studio 2010, les extraits de code IntelliSense sont pris en charge C# pour JavaScript, en parallèle et Visual Basic, qui étaient pris en charge dans les versions antérieures de Visual Studio.

Visual Studio 2010 inclut plus de 200 extraits de code qui vous aident à compléter automatiquement les balises ASP.NET et HTML courantes, y compris les attributs requis (tels que runat = "Server") et les attributs communs spécifiques à une balise (par exemple, *ID*, *DataSourceID*, *ControlToValidate*et *Text*).

Vous pouvez télécharger des extraits de code supplémentaires, ou vous pouvez écrire vos propres extraits de code qui encapsulent les blocs de balisage que vous ou votre équipe utilisez pour les tâches courantes.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Améliorations de JavaScript IntelliSense

Dans Visual 2010, JavaScript IntelliSense a été repensé pour offrir une expérience d’édition encore plus riche. IntelliSense reconnaît désormais les objets qui ont été générés dynamiquement par des méthodes telles que *registerNamespace* et par des techniques similaires utilisées par d’autres infrastructures JavaScript. Les performances ont été améliorées pour analyser les grandes bibliothèques de scripts et pour afficher IntelliSense avec peu ou pas de délai de traitement. La compatibilité a été considérablement augmentée pour prendre en charge presque toutes les bibliothèques tierces et pour prendre en charge divers styles de codage. Les commentaires de documentation sont maintenant analysés au fur et à mesure que vous tapez et sont immédiatement exploités par IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Déploiement d’applications Web avec Visual Studio 2010

Lorsque les développeurs ASP.NET déploient une application Web, ils découvrent souvent qu’ils rencontrent des problèmes tels que les suivants :

- Le déploiement sur un site d’hébergement partagé nécessite des technologies telles que FTP, qui peuvent être lentes. En outre, vous devez effectuer manuellement des tâches telles que l’exécution de scripts SQL pour configurer une base de données et vous devez modifier les paramètres IIS, tels que la configuration d’un dossier de répertoire virtuel en tant qu’application.
- Dans un environnement d’entreprise, en plus du déploiement des fichiers d’application Web, les administrateurs doivent souvent modifier les fichiers de configuration ASP.NET et les paramètres IIS. Les administrateurs de base de données doivent exécuter une série de scripts SQL pour obtenir l’exécution de la base de données d’application. De telles installations sont gourmandes en main-d’œuvre, elles prennent souvent des heures à se terminer et doivent être soigneusement documentées.

Visual Studio 2010 comprend des technologies qui résolvent ces problèmes et qui vous permettent de déployer des applications Web en toute transparence. L’une de ces technologies est l’outil de déploiement Web IIS (MsDeploy. exe).

Les fonctionnalités de déploiement Web dans Visual Studio 2010 incluent les principaux éléments suivants :

- Empaquetage Web
- Transformation Web. config
- Déploiement de base de données
- Publication en un clic pour les applications Web

Les sections suivantes fournissent des détails sur ces fonctionnalités.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Empaquetage Web

Visual Studio 2010 utilise l’outil MSDeploy pour créer un fichier compressé (. zip) pour votre application, appelé *Package Web*. Le fichier de package contient les métadonnées relatives à votre application plus le contenu suivant :

- Les paramètres IIS, qui incluent les paramètres du pool d’applications, les paramètres de page d’erreur, etc.
- Le contenu Web réel, qui comprend des pages Web, des contrôles utilisateur, du contenu statique (images et fichiers HTML), et ainsi de suite.
- SQL Server les schémas de base de données et les données.
- Certificats de sécurité, composants à installer dans le GAC, paramètres du Registre, etc.

Vous pouvez copier un package Web sur n’importe quel serveur, puis l’installer manuellement à l’aide du gestionnaire des services Internet. Pour un déploiement automatisé, vous pouvez également installer le package à l’aide des commandes de ligne de commande ou à l’aide des API de déploiement.

Visual Studio 2010 fournit des tâches et des cibles MSBuild intégrées pour créer des packages Web. Pour plus d’informations, consultez [vue d’ensemble du déploiement d’un projet d’application web ASP.net](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) sur le site Web MSDN et [10 + 20 raisons pour lesquelles vous devez créer un package Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) sur le blog de Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Transformation Web. config

Pour le déploiement d’applications Web, Visual Studio 2010 introduit la [transformation de document XML (xdt)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), qui est une fonctionnalité qui vous permet de transformer un fichier `Web.config` des paramètres de développement en paramètres de production. Les paramètres de transformation sont spécifiés dans les fichiers de transformation nommés `web.debug.config`, `web.release.config`, etc. (Les noms de ces fichiers correspondent aux configurations MSBuild.) Un fichier de transformation comprend uniquement les modifications que vous devez apporter à un fichier `Web.config` déployé. Vous spécifiez les modifications à l’aide d’une syntaxe simple.

L’exemple suivant montre une partie d’un fichier `web.release.config` qui peut être produit pour le déploiement de votre configuration Release. Le mot clé replace dans l’exemple spécifie que, pendant le déploiement, le nœud *ConnectionString* dans le fichier `Web.config` est remplacé par les valeurs répertoriées dans l’exemple.

[!code-xml[Main](overview/samples/sample102.xml)]

Pour plus d’informations, consultez [syntaxe de transformation Web. config pour le déploiement](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) d’un projet <a id="0.2_a"></a> d’application Web sur le site Web MSDN et[déploiement Web : transformation Web. config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) sur le blog de Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Déploiement de base de données

Un package de déploiement Visual Studio 2010 peut inclure des dépendances sur les bases de données SQL Server. Dans le cadre de la définition du package, vous fournissez la chaîne de connexion pour votre base de données source. Lorsque vous créez le package Web, Visual Studio 2010 crée des scripts SQL pour le schéma de la base de données et éventuellement pour les données, puis les ajoute au package. Vous pouvez également fournir des scripts SQL personnalisés et spécifier l’ordre dans lequel ils doivent s’exécuter sur le serveur. Au moment du déploiement, vous fournissez une chaîne de connexion appropriée pour le serveur cible. le processus de déploiement utilise ensuite cette chaîne de connexion pour exécuter les scripts qui créent le schéma de la base de données et ajoutent les données.

En outre, en utilisant la publication en un clic, vous pouvez configurer le déploiement pour publier votre base de données directement lorsque l’application est publiée sur un site d’hébergement partagé distant. Pour plus d’informations, consultez [Comment : déployer une base de données avec un projet d’application Web](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) sur le site Web MSDN et [déploiement de base de données avec vs 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) sur le blog de Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Publication en un clic pour les applications Web

Visual Studio 2010 vous permet également d’utiliser le service de gestion à distance IIS pour publier une application Web sur un serveur distant. Vous pouvez créer un profil de publication pour votre compte d’hébergement ou pour tester des serveurs ou des serveurs de test. Chaque profil peut enregistrer les informations d’identification appropriées en toute sécurité. Vous pouvez ensuite effectuer un déploiement sur n’importe quel serveur cible en un clic à l’aide de la barre d’outils de publication Web en un clic. Avec Visual Studio 2010, vous pouvez également publier à l’aide de la ligne de commande MSBuild. Cela vous permet de configurer votre environnement Team Build pour inclure la publication dans un modèle d’intégration continue.

Pour plus d’informations, consultez [Comment : déployer un projet d’application Web à l’aide de la publication en un clic et Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) sur le site Web MSDN et [Web 1-cliquez sur publier avec vs 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) sur le blog de Vishal Joshi. Pour afficher des présentations vidéo sur le déploiement d’applications Web dans Visual Studio 2010, consultez [VS 2010 pour les aperçus de développeur Web](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) sur le blog de Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Ressources

Les sites Web suivants fournissent des informations supplémentaires sur ASP.NET 4 et Visual Studio 2010.

- [ASP.net 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) : documentation officielle de ASP.net 4 sur le site Web MSDN.
- [https://www.asp.net/](https://www.asp.net/) : le site Web de l’équipe ASP.net.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) et [ASP.NET Dynamic Data plan de contenu](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) : ressources en ligne sur le site d’équipe ASP.net et dans la documentation officielle de ASP.NET Dynamic Data.
- [https://www.asp.net/ajax/](../../ajax/index.md) : ressource Web principale pour le développement ASP.NET AJAX.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) : le blog de l’équipe Visual Web Developer, qui comprend des informations sur les fonctionnalités de visual studio 2010.
- [ASP.net webstack](https://github.com/aspnet/AspNetWebStack) : ressource Web principale pour les versions préliminaires de ASP.net.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Exclusion de responsabilité

Ce document est une version préliminaire et peut être modifié substantiellement avant le lancement de la mise en production commerciale finale du logiciel qu’il décrit.

Les informations contenues dans ce document correspondent à la connaissance que Microsoft Corporation possède des problèmes abordés à la date de la publication. Microsoft devant répondre à des conditions de marché qui évoluent, ce document ne doit pas être considéré comme un engagement de sa part, et Microsoft ne peut pas garantir l’exactitude des informations présentées à la date de la publication.

Ce livre blanc est fourni à titre d'information uniquement. MICROSOFT NE FOURNIT AUCUNE GARANTIE, EXPRESSE, IMPLICITE OU LÉGALE, QUANT AUX INFORMATIONS CONTENUES DANS CE DOCUMENT.

L'utilisateur est tenu d'observer la réglementation relative aux droits d'auteur applicable dans son pays. Aucune partie de ce document ne peut être reproduite, stockée ou introduite dans un système de restitution, ou transmise à quelque fin ou par quelque moyen que ce soit (électronique, mécanique, photocopie, enregistrement ou autre) sans la permission expresse et écrite de Microsoft Corporation.

Microsoft peut détenir des brevets, avoir déposé des demandes d'enregistrement de brevets ou être titulaire de marques, droits d'auteur ou autres droits de propriété intellectuelle portant sur tout ou partie des éléments qui font l'objet du présent document. Sauf stipulation expresse contraire d'un contrat de licence écrit de Microsoft, la fourniture de ce document n'a pas pour effet de vous concéder une licence sur ces brevets, marques, droits d'auteur ou autres droits de propriété intellectuelle.

Sauf mention contraire, les exemples de sociétés, d’organisations, de produits, de noms de domaine, d’adresses de messagerie, de logos, de personnes, de lieux et d’événements mentionnés dans le présent document sont fictifs et ne sont pas associés à une société, une organisation, un produit, un nom de domaine, une adresse de messagerie réels une adresse, un logo, une personne, un lieu ou un événement est intentionnel ou doit être déduit.

© 2009 Microsoft Corporation. Tous droits réservés.

Microsoft et Windows sont soit des marques déposées de Microsoft Corporation, soit des marques de Microsoft Corporation aux États-Unis d'Amérique et/ou dans d'autres pays.

Les noms des sociétés et des produits mentionnés dans le présent document peuvent être des marques de leurs propriétaires respectifs.
