---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Prise en charge des actions OData dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: 'Dans OData, les actions sont un moyen d’ajouter des comportements côté serveur qui ne sont pas facilement définis en tant qu’opérations CRUD sur les entités. Certaines utilisations des actions incluent : implémenter...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: ae8b23f0868f992cb2bbbf14ee3f7ac848501515
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600351"
---
# <a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Prise en charge des actions OData dans API Web ASP.NET 2

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Dans OData, les *actions* sont un moyen d’ajouter des comportements côté serveur qui ne sont pas facilement définis en tant qu’opérations CRUD sur les entités. Certaines utilisations des actions sont les suivantes :
> 
> - Implémentation de transactions complexes.
> - Manipulation de plusieurs entités à la fois.
> - Autorisation des mises à jour uniquement pour certaines propriétés d’une entité.
> - Envoi d’informations au serveur qui n’est pas défini dans une entité.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - API Web 2
> - OData version 3
> - Entity Framework 6

## <a name="example-rating-a-product"></a>Exemple : évaluation d’un produit

Dans cet exemple, nous souhaitons permettre aux utilisateurs de évaluer les produits, puis d’exposer les évaluations moyennes pour chaque produit. Sur la base de données, nous stockerons une liste de classifications, indexées sur les produits.

Voici le modèle que nous pouvons utiliser pour représenter les évaluations dans Entity Framework :

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Mais nous ne voulons pas que les clients PUBLIEnt un objet `ProductRating` dans une collection « Ratings ». Intuitivement, l’évaluation est associée à la collection Products et le client doit uniquement poster la valeur d’évaluation.

Par conséquent, au lieu d’utiliser les opérations CRUD normales, nous définissons une action qu’un client peut appeler sur un produit. Dans la terminologie OData, l’action est *liée* aux entités Product.

>Les actions ont des effets secondaires sur le serveur. Pour cette raison, elles sont appelées à l’aide de requêtes HTTP HTTP. Les actions peuvent avoir des paramètres et des types de retour, qui sont décrits dans les métadonnées du service. Le client envoie les paramètres dans le corps de la demande, et le serveur envoie la valeur de retour dans le corps de la réponse. Pour appeler l’action « taux de produit », le client envoie un billet à un URI comme suit :

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Les données de la demande de publication sont simplement le classement du produit :

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Déclarez l’action dans le Entity Data Model

Dans la configuration de votre API Web, ajoutez l’action à l’EDM (Entity Data Model) :

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Ce code définit « RateProduct » en tant qu’action qui peut être effectuée sur les entités Product. Elle déclare également que l’action prend un paramètre **int** nommé « Rating » et retourne une valeur **int** .

## <a name="add-the-action-to-the-controller"></a>Ajouter l’action au contrôleur

L’action « RateProduct » est liée aux entités Product. Pour implémenter l’action, ajoutez une méthode nommée `RateProduct` au contrôleur Products :

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Notez que le nom de la méthode correspond au nom de l’action dans le modèle EDM. La méthode a deux paramètres :

- *clé*: clé du produit à évaluer.
- *Parameters*: dictionnaire de valeurs de paramètre d’action.

Si vous utilisez les conventions de routage par défaut, le paramètre de clé doit être nommé « Key ». Il est également important d’inclure l’attribut **[FromOdataUri]** , comme indiqué. Cet attribut indique à l’API Web d’utiliser les règles de syntaxe OData lorsqu’elle analyse la clé à partir de l’URI de la demande.

Utilisez le dictionnaire de *paramètres* pour récupérer les paramètres d’action :

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Si le client envoie les paramètres d’action dans le format correct, la valeur de **ModelState. IsValid** est true. Dans ce cas, vous pouvez utiliser le dictionnaire **ODataActionParameters** pour récupérer les valeurs des paramètres. Dans cet exemple, l’action `RateProduct` accepte un seul paramètre nommé « Rating ».

## <a name="action-metadata"></a>Métadonnées d’action

Pour afficher les métadonnées du service, envoyez une demande de récupération à/OData/$metadata. Voici la partie des métadonnées qui déclare l’action `RateProduct` :

[!code-xml[Main](odata-actions/samples/sample7.xml)]

L’élément **FunctionImport** déclare l’action. La plupart des champs sont explicites, mais deux à noter :

- **IsBindable** signifie que l’action peut être appelée sur l’entité cible, au moins une partie du temps.
- **IsAlwaysBindable** signifie que l’action peut toujours être appelée sur l’entité cible.

La différence est que certaines actions sont toujours disponibles pour les clients, mais d’autres peuvent dépendre de l’état de l’entité. Supposons, par exemple, que vous définissiez une action « acheter ». Vous ne pouvez acheter qu’un article en stock. Si l’élément est en rupture de stock, un client ne peut pas appeler cette action.

Lorsque vous définissez le modèle EDM, la méthode d' **action** crée une action toujours pouvant être liée :

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Je parlerai des actions non toujours liées (également appelées actions *transitoires* ) plus loin dans cette rubrique.

## <a name="invoking-the-action"></a>Appel de l’action

Voyons maintenant comment un client appelle cette action. Supposons que le client souhaite attribuer une évaluation de 2 au produit avec ID = 4. Voici un exemple de message de demande, à l’aide du format JSON pour le corps de la demande :

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Voici le message de réponse :

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Liaison d’une action à un jeu d’entités

Dans l’exemple précédent, l’action est liée à une seule entité : le client évalue un produit unique. Vous pouvez également lier une action à une collection d’entités. Effectuez simplement les modifications suivantes :

Dans le modèle EDM, ajoutez l’action à la propriété de **collection** de l’entité.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

Dans la méthode Controller, omettez le paramètre *Key* .

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

À présent, le client appelle l’action sur le jeu d’entités Products :

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Actions avec paramètres de regroupement

Les actions peuvent avoir des paramètres qui acceptent une collection de valeurs. Dans le modèle EDM, utilisez **CollectionParameter&lt;t&gt;** pour déclarer le paramètre.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Cela déclare un paramètre nommé « ratings » qui prend une collection de valeurs **int** . Dans la méthode Controller, vous recevez toujours la valeur de paramètre de l’objet **ODataActionParameters** , mais maintenant la valeur est une valeur **ICollection&lt;int&gt;** :

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Actions transitoires

Dans l’exemple « RateProduct », les utilisateurs peuvent toujours évaluer un produit, de sorte que l’action est toujours disponible. Toutefois, certaines actions dépendent de l’état de l’entité. Par exemple, dans un service de location vidéo, l’action « CheckOut » n’est pas toujours disponible. (Cela dépend de la disponibilité d’une copie de cette vidéo.) Ce type d’action est appelé action *transitoire* .

Dans les métadonnées du service, une action temporaire a **IsAlwaysBindable** égal à false. Il s’agit en fait de la valeur par défaut, de sorte que les métadonnées ressemblent à ceci :

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Voici pourquoi cela est important : si une action est temporaire, le serveur doit indiquer au client quand l’action est disponible. Pour ce faire, il inclut un lien vers l’action dans l’entité. Voici un exemple pour une entité de film :

[!code-console[Main](odata-actions/samples/sample17.cmd)]

La propriété « #CheckOut » contient un lien vers l’action d’extraction. Si l’action n’est pas disponible, le serveur omet le lien.

Pour déclarer une action transitoire dans le modèle EDM, appelez la méthode **TransientAction** :

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Vous devez également fournir une fonction qui retourne un lien d’action pour une entité donnée. Définissez cette fonction en appelant **HasActionLink**. Vous pouvez écrire la fonction en tant qu’expression lambda :

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Si l’action est disponible, l’expression lambda retourne un lien vers l’action. Le sérialiseur OData comprend ce lien lorsqu’il sérialise l’entité. Lorsque l’action n’est pas disponible, la fonction retourne `null`.

## <a name="additional-resources"></a>Ressources supplémentaires

[Exemple d’actions OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
