---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implémenter une pagination des données efficace | Microsoft Docs
author: microsoft
description: L’étape 8 montre comment ajouter la prise en charge de la pagination à notre URL/dinners. ainsi, au lieu d’afficher des milliers de dîners à la fois, nous n’afficherons que 10 dîners à venir...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 2d9a0dba381b71755ac626f76d52bc5bcb434447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601051"
---
# <a name="implement-efficient-data-paging"></a>Implémenter une pagination des données efficace

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 8 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 8 montre comment ajouter la prise en charge de la pagination à notre URL/dinners. ainsi, au lieu d’afficher des milliers de dîners à la fois, nous n’affichons que 10 dîners à la fois, ce qui permet aux utilisateurs finaux de revenir en arrière et d’avancer dans toute la liste d’une manière conviviale SEO.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-8-paging-support"></a>NerdDinner étape 8 : prise en charge de la pagination

Si notre site réussit, il aura des milliers de dîners à venir. Nous devons nous assurer que notre interface utilisateur est mise à l’échelle pour gérer tous ces dîners et permettre aux utilisateurs de les parcourir. Pour ce faire, nous ajouterons la prise en charge de la pagination à notre URL */dinners* . ainsi, au lieu d’afficher des milliers de dîners à la fois, nous n’afficherons que 10 dîners à venir, et autoriser les utilisateurs finaux à revenir en arrière et à transférer l’intégralité de la liste à l’aide d’un outil SEO.

### <a name="index-action-method-recap"></a>Récapitulatif de la méthode d’action d’index ()

La méthode d’action index () au sein de notre classe DinnersController ressemble actuellement à ce qui suit :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Lorsqu’une demande est envoyée à l’URL */dinners* , elle récupère une liste de tous les dîners à venir, puis affiche une liste de tous les dîners :

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iqueryablelttgt"></a>Understanding IQueryable&lt;T&gt;

*Iqueryable&lt;t&gt;* est une interface qui a été introduite avec LINQ dans le cadre de .net 3,5. Il permet d’effectuer des scénarios d’exécution différée performants que nous pouvons utiliser pour implémenter la prise en charge de la pagination.

Dans le cadre de notre DinnerRepository, nous renvoyons un IQueryable&lt;&gt; de la séquence de dîner à partir de notre méthode FindUpcomingDinners () :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

L’objet IQueryable&lt;dîner&gt; retourné par notre méthode FindUpcomingDinners () encapsule une requête pour récupérer des objets de notre base de données à l’aide d’LINQ to SQL. Plus important encore, il n’exécutera pas la requête sur la base de données tant que nous n’aurons pas tenté d’accéder aux données de la requête ou d’en effectuer une itération, ou jusqu’à ce que nous appelons la méthode ToList () sur celle-ci. Le code appelant notre méthode FindUpcomingDinners () peut éventuellement choisir d’ajouter des opérations/filtres « chaînés » supplémentaires à l’objet IQueryable&lt;dîner&gt; avant d’exécuter la requête. LINQ to SQL est alors suffisamment intelligent pour exécuter la requête combinée sur la base de données lorsque les données sont demandées.

Pour implémenter la logique de pagination, nous pouvons mettre à jour la méthode d’action index () de DinnersController afin qu’elle applique des opérateurs « Skip » et « Take » supplémentaires à la séquence IQueryable retournée&lt;dîner&gt; avant d’appeler ToList () sur celle-ci :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Le code ci-dessus ignore les 10 premiers dîners à venir dans la base de données, puis renvoie 20 dîners. LINQ to SQL est suffisamment intelligent pour construire une requête SQL optimisée qui effectue cette logique ignorée dans la base de données SQL, et non dans le serveur Web. Cela signifie que même si nous avons des millions de dîners à venir dans la base de données, seuls les 10 qui nous intéressent seront récupérés dans le cadre de cette demande (ce qui la rend efficace et évolutive).

### <a name="adding-a-page-value-to-the-url"></a>Ajout d’une valeur « page » à l’URL

Au lieu de coder en dur une plage de pages spécifique, nous voulons que nos URL incluent un paramètre « page » qui indique la plage de dîner demandée par un utilisateur.

#### <a name="using-a-querystring-value"></a>Utilisation d’une valeur QueryString

Le code ci-dessous montre comment nous pouvons mettre à jour notre méthode d’action d’index () pour prendre en charge un paramètre QueryString et activer des URL telles que */dinners ? page = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

La méthode d’action index () ci-dessus a un paramètre nommé « page ». Le paramètre est déclaré en tant qu’entier Nullable (c’est ce que int ? indique). Cela signifie que l’URL */dinners ? page = 2* entraîne la transmission d’une valeur « 2 » comme valeur de paramètre. L’URL */dinners* (sans valeur QueryString) entraîne la transmission d’une valeur null.

Nous multipliant la valeur de la page par la taille de la page (dans le cas présent, 10 lignes) pour déterminer le nombre de dîners à ignorer. Nous utilisons l' [ C# opérateur null de « fusion » ( ??)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) qui est utile lorsque vous traitez des types Nullable. Le code ci-dessus assigne la valeur 0 à la page si le paramètre de page a la valeur null.

#### <a name="using-embedded-url-values"></a>Utilisation de valeurs d’URL incorporées

Une alternative à l’utilisation d’une valeur QueryString consiste à incorporer le paramètre de page dans l’URL proprement dite. Par exemple : */dinners/page/2* ou */dinners/2*. ASP.NET MVC comprend un moteur de routage d’URL puissant qui facilite la prise en charge de scénarios comme celui-ci.

Nous pouvons inscrire des règles de routage personnalisées qui mappent des URL ou des formats d’URL entrants à n’importe quelle classe de contrôleur ou méthode d’action de votre choix. Tout ce que nous devons faire, c’est d’ouvrir le fichier global. asax au sein de notre projet :

![](implement-efficient-data-paging/_static/image2.png)

Puis, enregistrez une nouvelle règle de mappage à l’aide de la méthode d’assistance MapRoute () comme le premier appel aux itinéraires. MapRoute () ci-dessous :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Au-dessus de nous enregistrons une nouvelle règle de routage nommée « UpcomingDinners ». Nous indiquons qu’il a le format d’URL « dîners/page/{page} », où {page} est une valeur de paramètre incorporée dans l’URL. Le troisième paramètre de la méthode MapRoute () indique que nous devons mapper des URL qui correspondent à ce format à la méthode d’action index () sur la classe DinnersController.

Nous pouvons utiliser exactement le même code d’index () que dans notre scénario QueryString. à l’exception de ce dernier, notre paramètre « page » vient de l’URL et non de la chaîne de chaîne :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Et maintenant, lorsque nous exécutons l’application et que j’ai tapé */dinners* , nous verrons les 10 premiers dîners à venir :

![](implement-efficient-data-paging/_static/image3.png)

Lorsque nous frapperons */dinners/page/1* , nous verrons la page suivante sur les dîners :

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Ajout de l’interface utilisateur de navigation entre les pages

La dernière étape pour effectuer notre scénario de pagination consiste à implémenter l’interface utilisateur de navigation « Next » et « Previous » dans notre modèle de vue pour permettre aux utilisateurs d’ignorer facilement les données du dîner.

Pour l’implémenter correctement, nous devrons connaître le nombre total de dîners dans la base de données, ainsi que le nombre de pages de données traduites. Nous devons ensuite calculer si la valeur « page » actuellement demandée se trouve au début ou à la fin des données, et afficher ou masquer l’interface utilisateur « précédente » et « suivante » en conséquence. Nous pourrions implémenter cette logique dans notre méthode d’action d’index (). Vous pouvez également ajouter une classe d’assistance à notre projet qui encapsule cette logique de façon plus réutilisable.

Vous trouverez ci-dessous une classe d’assistance « PaginatedList » simple qui dérive de la liste&lt;T&gt; classe de collection intégrée à la .NET Framework. Il implémente une classe de collection réutilisable qui peut être utilisée pour paginer n’importe quelle séquence de données IQueryable. Dans notre application NerdDinner, nous allons travailler sur IQueryable&lt;dîner&gt; résultats, mais cela peut tout aussi facilement être utilisé par rapport à IQueryable&lt;Product&gt; ou IQueryable&lt;client&gt; résultats dans d’autres scénarios d’application :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Notez qu’elle calcule, puis expose des propriétés telles que « PageIndex », « PageSize », « TotalCount » et « TotalPages ». Il expose également deux propriétés d’assistance « HasPreviousPage » et « HasNextPage » qui indiquent si la page de données dans la collection est au début ou à la fin de la séquence d’origine. Le code ci-dessus entraîne l’exécution de deux requêtes SQL : la première pour récupérer le nombre total d’objets dîner (cela ne retourne pas les objets, au lieu d’exécuter une instruction « SELECT COUNT » qui retourne un entier) et la seconde pour récupérer uniquement les lignes de données dont nous avons besoin à partir de notre base de données pour la page de données actuelle.

Nous pouvons ensuite mettre à jour notre méthode d’assistance DinnersController. index () pour créer un PaginatedList&lt;dîner&gt; à partir de notre résultat DinnerRepository. FindUpcomingDinners () et le transmettre à notre modèle de vue :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Nous pouvons ensuite mettre à jour le modèle de vue \Views\Dinners\Index.aspx pour qu’il hérite de ViewPage&lt;NerdDinner. helpers. PaginatedList&lt;dîner&gt;&gt; au lieu de ViewPage&lt;IEnumerable&lt;dîner&gt;&gt;, puis ajoutez le code suivant en bas de notre modèle d’affichage pour afficher ou masquer l’interface utilisateur de navigation suivante et précédente :

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Notez que nous utilisons la méthode d’assistance HTML. RouteLink () pour générer nos liens hypertexte. Cette méthode est similaire à la méthode d’assistance HTML. ActionLink () que nous avons utilisée précédemment. La différence est que nous générons l’URL à l’aide de la règle de routage « UpcomingDinners » que nous avons configurée dans notre fichier global. asax. Cela garantit que nous allons générer des URL dans notre méthode d’action d’index () qui ont le format : */dinners/page/{page}* – où la valeur {page} est une variable que nous fournissons ci-dessus en fonction du pageIndex actuel.

Maintenant, lorsque nous exécutons à nouveau notre application, nous verrons 10 dîners à la fois dans notre navigateur :

![](implement-efficient-data-paging/_static/image5.png)

Nous avons également &lt;&lt;&lt; et &gt;&gt;&gt; interface utilisateur de navigation en bas de la page qui nous permet d’avancer et de revenir en arrière sur nos données à l’aide d’URL accessibles du moteur de recherche :

![](implement-efficient-data-paging/_static/image6.png)

| **Rubrique latérale : comprendre les implications de IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; est une fonctionnalité très puissante qui active un large éventail de scénarios d’exécution différés intéressants (tels que les requêtes basées sur la pagination et la composition). Comme avec toutes les fonctionnalités puissantes, vous devez faire attention à la façon dont vous les utilisez et vous assurer qu’elles ne sont pas abusives. Il est important de reconnaître que le retour d’un IQueryable&lt;T&gt; résultat de votre dépôt permet au code d’appel d’ajouter des méthodes d’opérateur chaînées à ce dernier, et donc de participer à l’exécution de la requête finale. Si vous ne souhaitez pas fournir à du code appelant cette possibilité, vous devez retourner les résultats IList&lt;T&gt; ou IEnumerable&lt;T&gt;, qui contiennent les résultats d’une requête qui a déjà été exécutée. Pour les scénarios de pagination, vous devez pousser la logique réelle de pagination des données dans la méthode de dépôt appelée. Dans ce scénario, nous pouvons mettre à jour notre méthode de recherche FindUpcomingDinners () pour avoir une signature qui retournait un PaginatedList : PaginatedList&lt; dîner&gt; FindUpcomingDinners (int pageIndex, int PageSize) {} ou retourner un TotalCount de dîner&gt;, et utiliser un paramètre out «» pour retourner le nombre total de dîners : IList&lt;dîner&gt;&lt;FindUpcomingDinners (int pageIndex, int PageSize, out int totalCount) {} |

### <a name="next-step"></a>Étape suivante

Voyons maintenant comment nous pouvons ajouter la prise en charge de l’authentification et de l’autorisation à notre application.

> [!div class="step-by-step"]
> [Précédent](re-use-ui-using-master-pages-and-partials.md)
> [Suivant](secure-applications-using-authentication-and-authorization.md)
