---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Ajout de la confirmation côté client lors deC#la suppression () | Microsoft Docs
author: rick-anderson
description: Dans les interfaces que nous avons créées jusqu’à présent, un utilisateur peut supprimer accidentellement des données en cliquant sur le bouton supprimer lorsqu’il veut cliquer sur le bouton modifier. Dans ce t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: e7d53bc65fdbbfa9ce9bfa5fbdbfa0dea598eebe
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74623574"
---
# <a name="adding-client-side-confirmation-when-deleting-c"></a>Ajout d’une confirmation côté client lors de la suppression (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) ou [Télécharger le PDF](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> Dans les interfaces que nous avons créées jusqu’à présent, un utilisateur peut supprimer accidentellement des données en cliquant sur le bouton supprimer lorsqu’il veut cliquer sur le bouton modifier. Dans ce didacticiel, nous allons ajouter une boîte de dialogue de confirmation côté client qui s’affiche lorsque vous cliquez sur le bouton supprimer.

## <a name="introduction"></a>Introduction

Au cours des quelques didacticiels, nous avons vu comment utiliser notre architecture d’application, ObjectDataSource et les contrôles Web de données de concert pour fournir des fonctionnalités d’insertion, de modification et de suppression. Les interfaces de suppression que nous avons examinées jusqu’à présent ont été composées d’un bouton supprimer qui, lorsque vous cliquez dessus, provoquent une publication (postback) et appelle la méthode ObjectDataSource s `Delete()`. La méthode `Delete()` appelle ensuite la méthode configurée à partir de la couche de logique métier, qui propage l’appel à la couche d’accès aux données, en émettant l’instruction `DELETE` réelle à la base de données.

Bien que cette interface utilisateur permette aux visiteurs de supprimer des enregistrements via les contrôles GridView, DetailsView ou FormView, elle n’a pas de confirmation lorsque l’utilisateur clique sur le bouton supprimer. Si un utilisateur clique accidentellement sur le bouton supprimer lorsqu’il veut cliquer sur modifier, l’enregistrement qu’il voulait mettre à jour sera supprimé à la place. Pour éviter cela, dans ce didacticiel, nous allons ajouter une boîte de dialogue de confirmation côté client qui s’affiche lorsque vous cliquez sur le bouton supprimer.

La fonction JavaScript `confirm(string)` affiche son paramètre d’entrée de chaîne en tant que texte dans une boîte de dialogue modale qui est équipée de deux boutons : OK et annuler (voir figure 1). La fonction `confirm(string)` retourne une valeur booléenne en fonction du bouton sur lequel l’utilisateur clique (`true`, si l’utilisateur clique sur OK, et `false` s’il clique sur Annuler).

![La méthode JavaScript confirm (String) affiche un MessageBox modal côté client](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Figure 1**: la méthode de `confirm(string)` JavaScript affiche une MessageBox modale côté client

Lors de l’envoi d’un formulaire, si une valeur de `false` est retournée à partir d’un gestionnaire d’événements côté client, l’envoi du formulaire est annulé. À l’aide de cette fonctionnalité, nous pouvons faire en sorte que le bouton supprimer `onclick` gestionnaire d’événements retourne la valeur d’un appel à `confirm("Are you sure you want to delete this product?")`. Si l’utilisateur clique sur Annuler, `confirm(string)` retourne la valeur false, ce qui provoque l’annulation de l’envoi du formulaire. Sans publication (postback), le produit dont l’utilisateur a cliqué sur le bouton supprimer ne sera pas supprimé. Toutefois, si l’utilisateur clique sur OK dans la boîte de dialogue de confirmation, la publication continue de s’interrompre et le produit est supprimé. Pour plus d’informations sur cette technique, consultez [utilisation de JavaScript s `confirm()` méthode pour contrôler l’envoi](http://www.webreference.com/programming/javascript/confirm/) d’un formulaire.

L’ajout du script côté client nécessaire diffère légèrement si vous utilisez des modèles que lors de l’utilisation d’un CommandField. Par conséquent, dans ce didacticiel, nous allons examiner un exemple FormView et GridView.

> [!NOTE]
> À l’aide de techniques de confirmation côté client, comme celles abordées dans ce didacticiel, suppose que vos utilisateurs se rendent dans des navigateurs qui prennent en charge JavaScript et que JavaScript est activé. Si l’une de ces hypothèses n’est pas vraie pour un utilisateur particulier, le fait de cliquer sur le bouton supprimer entraîne immédiatement une publication (sans afficher de confirmation MessageBox).

## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Étape 1 : création d’un FormView qui prend en charge la suppression

Commencez par ajouter un contrôle FormView à la page `ConfirmationOnDelete.aspx` dans le dossier `EditInsertDelete`, en le liant à un nouvel ObjectDataSource qui extrait les informations sur le produit par le biais de la méthode `GetProducts()` de la classe `ProductsBLL`. Configurez également l’ObjectDataSource afin que la méthode de la classe `ProductsBLL` s `DeleteProduct(productID)` soit mappée à la méthode de `Delete()` ObjectDataSource s. Vérifiez que les listes déroulantes insérer et mettre à jour les onglets sont définies sur (aucune). Enfin, cochez la case Activer la pagination dans la balise active FormView s.

Après ces étapes, le nouveau balisage déclaratif s ressemble à ce qui suit :

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

Comme dans les exemples passés qui n’utilisaient pas l’accès concurrentiel optimiste, prenez un moment pour effacer la propriété ObjectDataSource s `OldValuesParameterFormatString`.

Étant donné qu’il a été lié à un contrôle ObjectDataSource qui ne prend en charge que la suppression, la `ItemTemplate` FormView n’offre que le bouton supprimer, qui ne contient pas les boutons nouveau et mettre à jour. Toutefois, le balisage déclaratif des s FormView comprend un `EditItemTemplate` et `InsertItemTemplate`superflus, qui peuvent être supprimés. Prenez un moment pour personnaliser le `ItemTemplate` afin qu’il n’affiche qu’un sous-ensemble des champs de données de produit. J’ai configuré la mienne pour afficher le nom du produit dans une `<h3>` en-tête au-dessus de ses noms de fournisseur et de catégorie (avec le bouton supprimer).

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Avec ces modifications, nous disposons d’une page Web entièrement fonctionnelle qui permet à un utilisateur de basculer les produits l’un après l’autre, avec la possibilité de supprimer un produit simplement en cliquant sur le bouton supprimer. La figure 2 illustre une capture d’écran de notre progression jusqu’à présent dans un navigateur.

[![le FormView affiche des informations sur un produit unique](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Figure 2**: le FormView affiche des informations sur un seul produit ([cliquez pour afficher l’image en taille réelle](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))

## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Étape 2 : appel de la fonction confirm (String) à partir des boutons supprimer événement OnClick côté client

Une fois le FormView créé, l’étape finale consiste à configurer le bouton supprimer de telle sorte que, lorsqu’il clique sur le visiteur, la fonction JavaScript `confirm(string)` est appelée. L’ajout d’un script côté client à un événement de `onclick` côté client Button, LinkButton ou ImageButton peut être accompli à l’aide de l' `OnClientClick property`, qui est une nouveauté de ASP.NET 2,0. Étant donné que nous souhaitons que la valeur de la fonction `confirm(string)` soit retournée, il vous suffit de définir cette propriété sur : `return confirm('Are you certain that you want to delete this product?');`

Après cette modification, la syntaxe déclarative de Delete LinkButton s devrait ressembler à ceci :

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

C’est tout ! La figure 3 illustre une capture d’écran de cette confirmation en action. Le fait de cliquer sur le bouton supprimer ouvre la boîte de dialogue confirmer. Si l’utilisateur clique sur Annuler, la publication (postback) est annulée et le produit n’est pas supprimé. Toutefois, si l’utilisateur clique sur OK, la publication continue et la méthode d' `Delete()` ObjectDataSource est appelée, ce qui se termine dans l’enregistrement de base de données en cours de suppression.

> [!NOTE]
> La chaîne transmise dans la `confirm(string)` fonction JavaScript est délimitée par des apostrophes (plutôt que des guillemets). En JavaScript, les chaînes peuvent être délimitées à l’aide de l’un ou l’autre caractère. Nous utilisons ici des apostrophes afin que les délimiteurs de la chaîne passée dans `confirm(string)` n’introduisent pas d’ambiguïté avec les délimiteurs utilisés pour la valeur de propriété `OnClientClick`.

[![une confirmation s’affiche à présent quand vous cliquez sur le bouton supprimer](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Figure 3**: une confirmation s’affiche à présent quand vous cliquez sur le bouton supprimer ([cliquez pour afficher l’image en taille réelle](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))

## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Étape 3 : configuration de la propriété OnClientClick pour le bouton supprimer dans un CommandField

Quand vous utilisez un bouton, LinkButton ou ImageButton directement dans un modèle, une boîte de dialogue de confirmation peut être associée en configurant simplement sa propriété `OnClientClick` pour retourner les résultats de la fonction JavaScript `confirm(string)`. Toutefois, CommandField-qui ajoute un champ de boutons supprimer à un GridView ou DetailsView-n’a pas de propriété `OnClientClick` qui peut être définie de façon déclarative. Au lieu de cela, nous devons référencer par programmation le bouton supprimer dans le gestionnaire d’événements `DataBound` GridView ou DetailsView s approprié, puis définir sa propriété `OnClientClick` ici.

> [!NOTE]
> Lors de la définition de la propriété bouton supprimer des `OnClientClick` dans le gestionnaire d’événements `DataBound` approprié, nous avons accès aux données qui étaient liées à l’enregistrement actif. Cela signifie que nous pouvons étendre le message de confirmation pour inclure des détails sur l’enregistrement en question, par exemple « voulez-vous vraiment supprimer le produit Chai ? ». Une telle personnalisation est également possible dans les modèles utilisant la syntaxe DataBinding.

Pour vous entraîner à définir la propriété `OnClientClick` pour le ou les boutons de suppression dans une CommandField, ajoutez un GridView à la page. Configurez ce GridView pour utiliser le même contrôle ObjectDataSource que celui utilisé par le FormView. Limitez également le BoundFields de GridView s pour inclure uniquement le nom du produit, la catégorie et le fournisseur. Enfin, cochez la case Activer la suppression de la balise active GridView s. Cela ajoute un CommandField à la collection de `Columns` GridView, avec la propriété `ShowDeleteButton` définie sur `true`.

Une fois ces modifications effectuées, votre balisage déclaratif s doit ressembler à ce qui suit :

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

CommandField contient une seule instance de contrôle LinkButton qui est accessible par programme à partir du gestionnaire d’événements `RowDataBound` GridView s. Une fois référencé, nous pouvons définir sa propriété `OnClientClick` en conséquence. Créez un gestionnaire d’événements pour l’événement `RowDataBound` à l’aide du code suivant :

[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Ce gestionnaire d’événements fonctionne avec les lignes de données (celles qui comporteront le bouton supprimer) et commence par programmer la référencement du bouton supprimer. En général, utilisez le modèle suivant :

[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* est le type de bouton utilisé par le bouton CommandField-Button, LinkButton ou ImageButton. Par défaut, CommandField utilise LinkButtons, mais il peut être personnalisé via le `ButtonType property`CommandField s. *CommandFieldIndex* est l’index ordinal de CommandField au sein de la collection de `Columns` GridView, tandis que *controlIndex* est l’index du bouton supprimer dans la collection de `Controls` CommandField s. La valeur *controlIndex* dépend de la position du bouton par rapport à d’autres boutons dans CommandField. Par exemple, si le seul bouton affiché dans CommandField est le bouton supprimer, utilisez un index égal à 0. Toutefois, si vous avez un bouton modifier qui précède le bouton supprimer, utilisez un index de 2. La raison pour laquelle un index de 2 est utilisé est que deux contrôles sont ajoutés par CommandField avant le bouton supprimer : le bouton modifier et un LiteralControl utilisé pour ajouter de l’espace entre les boutons modifier et supprimer.

Pour notre exemple particulier, CommandField utilise LinkButtons et, étant le champ le plus à gauche, a un *commandFieldIndex* de 0. Étant donné qu’il n’y a pas d’autres boutons mais le bouton supprimer dans CommandField, nous utilisons un *controlIndex* de 0.

Après avoir référencé le bouton supprimer dans CommandField, nous allons ensuite récupérer des informations sur le produit lié à la ligne GridView actuelle. Enfin, nous définissons le bouton supprimer `OnClientClick` la propriété sur le code JavaScript approprié, qui comprend le nom du produit. Étant donné que la chaîne JavaScript transmise à la fonction `confirm(string)` est délimitée par des apostrophes, nous devons échapper toutes les apostrophes qui apparaissent dans le nom du produit. En particulier, toute apostrophe dans le nom du produit est placée dans une séquence d’échappement avec «`\'`».

Une fois ces modifications terminées, le fait de cliquer sur un bouton supprimer dans le contrôle GridView affiche une boîte de dialogue de confirmation personnalisée (voir figure 4). Comme avec la MessageBox de confirmation du FormView, si l’utilisateur clique sur Annuler, la publication est annulée, ce qui empêche la suppression de se produire.

> [!NOTE]
> Cette technique peut également être utilisée pour accéder par programmation au bouton supprimer dans CommandField dans un DetailsView. Toutefois, pour le contrôle DetailsView, vous créez un gestionnaire d’événements pour l’événement `DataBound`, puisque le DetailsView n’a pas d’événement `RowDataBound`.

[![cliquant sur le bouton de suppression de GridView affiche une boîte de dialogue de confirmation personnalisée](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Figure 4**: un clic sur le bouton de suppression de GridView affiche une boîte de dialogue de confirmation personnalisée ([cliquez pour afficher l’image en taille réelle](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))

## <a name="using-templatefields"></a>Utilisation de TemplateFields

L’un des inconvénients de la valeur CommandField est que ses boutons doivent être accessibles par le biais de l’indexation et que l’objet résultant doit être converti en type de bouton approprié (Button, LinkButton ou ImageButton). L’utilisation de « nombres magiques » et de types codés en dur invite à détecter les problèmes qui ne peuvent pas être découverts avant l’exécution. Par exemple, si vous, ou un autre développeur, ajoutez de nouveaux boutons à CommandField à un moment donné (par exemple, un bouton modifier) ou modifiez la propriété `ButtonType`, le code existant se compilera toujours sans erreur, mais la visite de la page peut provoquer une exception ou un comportement inattendu, en fonction de la façon dont votre code a été écrit et des modifications apportées.

Une autre approche consiste à convertir les CommandFields GridView et DetailsView s en TemplateFields. Cela génère un TemplateField avec un `ItemTemplate` avec LinkButton (ou Button ou ImageButton) pour chaque bouton dans CommandField. Ces boutons `OnClientClick` propriétés peuvent être assignés de manière déclarative, comme nous l’avons vu dans le FormView, ou ils peuvent être accessibles par programmation dans le gestionnaire d’événements `DataBound` approprié à l’aide du modèle suivant :

[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Où *ControlID* est la valeur de la propriété button s `ID`. Bien que ce modèle nécessite toujours un type codé en dur pour le cast, il supprime le besoin d’indexation, ce qui permet la modification de la disposition sans entraîner d’erreur d’exécution.

## <a name="summary"></a>Récapitulatif

La fonction JavaScript `confirm(string)` est une technique couramment utilisée pour contrôler le flux de travail d’envoi de formulaire. Lorsqu’elle est exécutée, la fonction affiche une boîte de dialogue modale, côté client, qui comprend deux boutons : OK et annuler. Si l’utilisateur clique sur OK, la fonction `confirm(string)` retourne `true`; Cliquez sur Annuler pour retourner `false`. Cette fonctionnalité, couplée avec le comportement des navigateurs pour annuler un envoi de formulaire si un gestionnaire d’événements au cours du processus de soumission retourne `false`, peut être utilisé pour afficher un message de confirmation lors de la suppression d’un enregistrement.

La fonction `confirm(string)` peut être associée à un gestionnaire d’événements côté client du contrôle Web Button `onclick` via la propriété Control s `OnClientClick`. Lors de l’utilisation d’un bouton supprimer dans un modèle, que ce soit dans l’un des modèles FormView ou dans un TemplateField dans DetailsView ou GridView, cette propriété peut être définie de façon déclarative ou par programme, comme nous l’avons vu dans ce didacticiel.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](implementing-optimistic-concurrency-cs.md)
> [Suivant](limiting-data-modification-functionality-based-on-the-user-cs.md)
