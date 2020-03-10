---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profils, thèmes et composants WebPart | Microsoft Docs
author: microsoft
description: Des modifications majeures ont été apportées à la configuration et à l’instrumentation dans ASP.NET 2,0. La nouvelle API de configuration ASP.NET permet d’apporter des modifications de configuration...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: cf5c45781be6d003d28c6aa27efa08032579a6dd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587233"
---
# <a name="profiles-themes-and-web-parts"></a>Profils, thèmes et composants WebPart

par [Microsoft](https://github.com/microsoft)

> Des modifications majeures ont été apportées à la configuration et à l’instrumentation dans ASP.NET 2,0. La nouvelle API de configuration ASP.NET permet d’effectuer des modifications de configuration par programme. En outre, de nombreux nouveaux paramètres de configuration existent pour les nouvelles configurations et l’instrumentation.

ASP.NET 2,0 représente une amélioration substantielle dans le domaine des sites Web personnalisés. Outre les fonctionnalités d’adhésion que nous avons déjà traitées, les profils ASP.NET, les thèmes et les composants WebPart améliorent considérablement la personnalisation des sites Web.

## <a name="aspnet-profiles"></a>Profils ASP.NET

Les profils ASP.NET sont similaires aux sessions. La différence réside dans le fait qu’un profil est persistant alors qu’une session est perdue lors de la fermeture du navigateur. Une autre différence importante entre les sessions et les profils est que les profils sont fortement typés, ce qui vous permet d’utiliser IntelliSense pendant le processus de développement.

Un profil est défini dans le fichier de configuration machines ou le fichier Web. config de l’application. (Vous ne pouvez pas définir un profil dans un fichier Web. config de sous-dossiers.) Le code ci-dessous définit un profil pour stocker le nom et le prénom des visiteurs du site Web.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Le type de données par défaut d’une propriété de profil est System. String. Dans l’exemple ci-dessus, aucun type de données n’a été spécifié. Par conséquent, les propriétés FirstName et LastName sont toutes deux de type chaîne. Comme mentionné précédemment, les propriétés de profil sont fortement typées. Le code ci-dessous ajoute une nouvelle propriété pour Age qui est de type Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Les profils sont généralement utilisés avec l’authentification par formulaire ASP.NET. Lorsqu’il est utilisé en association avec l’authentification par formulaire, chaque utilisateur dispose d’un profil distinct associé à son ID d’utilisateur. Toutefois, il est également possible d’autoriser l’utilisation de profils dans une application anonyme à l’aide de l’élément &lt;anonymousIdentification&gt; dans le fichier de configuration avec l’attribut **AllowAnonymous** comme indiqué ci-dessous :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Lorsqu’un utilisateur anonyme parcourt le site, ASP.NET crée une instance de **ProfileCommon** pour l’utilisateur. Ce profil utilise un ID unique stocké dans un cookie sur le navigateur pour identifier l’utilisateur en tant que visiteur unique. De cette façon, vous pouvez stocker des informations de profil pour les utilisateurs qui parcourent de manière anonyme.

## <a name="profile-groups"></a>Groupes de profils

Il est possible de regrouper les propriétés des profils. En regroupant les propriétés, il est possible de simuler plusieurs profils pour une application spécifique.

La configuration suivante configure une propriété FirstName et LastName pour deux groupes ; Acheteurs et prospects.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Il est ensuite possible de définir des propriétés sur un groupe particulier comme suit :

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Stockage d’objets complexes

Jusqu’à présent, les exemples que nous avons couverts ont stocké des types de données simples dans un profil. Il est également possible de stocker des types de données complexes dans un profil en spécifiant la méthode de sérialisation à l’aide de l’attribut **SerializeAs** comme suit :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

Dans ce cas, le type est PurchaseInvoice. La classe PurchaseInvoice doit être marquée comme sérialisable et peut contenir un nombre quelconque de propriétés. Par exemple, si PurchaseInvoice a une propriété appelée **NumItemsPurchased**, vous pouvez faire référence à cette propriété dans le code comme suit :

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Héritage de profil

Il est possible de créer un profil à utiliser dans plusieurs applications. En créant une classe de profil qui dérive de ProfileBase, vous pouvez réutiliser un profil dans plusieurs applications à l’aide de l’attribut **Inherits** , comme indiqué ci-dessous :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

Dans ce cas, la classe **PurchasingProfile** ressemble à ceci :

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Fournisseurs de profils

Les profils ASP.NET utilisent le modèle de fournisseur. Le fournisseur par défaut stocke les informations dans une base de données SQL Server Express dans le dossier application\_données de l’application Web à l’aide du fournisseur SqlProfileProvider. Si la base de données n’existe pas, ASP.NET la crée automatiquement lorsque le profil tente de stocker des informations.

Dans certains cas, toutefois, vous souhaiterez peut-être développer votre propre fournisseur de profils. La fonctionnalité de profil ASP.NET vous permet d’utiliser facilement différents fournisseurs.

Vous créez un fournisseur de profils personnalisé lorsque :

- Vous devez stocker les informations de profil dans une source de données, par exemple dans une base de données FoxPro ou dans une base de données Oracle, qui n’est pas prise en charge par les fournisseurs de profils inclus avec l' .NET Framework.
- Vous devez gérer les informations de profil à l’aide d’un schéma de base de données différent du schéma de base de données utilisé par les fournisseurs inclus avec l' .NET Framework. Un exemple courant est que vous souhaitez intégrer les informations de profil aux données utilisateur dans une base de données SQL Server existante.

### <a name="required-classes"></a>Classes requises

Pour implémenter un fournisseur de profils, vous devez créer une classe qui hérite de la classe abstraite System. Web. Profile. ProfileProvider. La classe abstraite **ProfileProvider** hérite à son tour de la classe abstraite System. Configuration. SettingsProvider, qui hérite de la classe abstraite System. Configuration. Provider. ProviderBase. En raison de cette chaîne d’héritage, en plus des membres requis de la classe **ProfileProvider** , vous devez implémenter les membres requis des classes **SettingsProvider** et **ProviderBase** .

Les tableaux suivants décrivent les propriétés et les méthodes que vous devez implémenter à partir des classes abstraites **ProviderBase**, **SettingsProvider**et **ProfileProvider** .

### <a name="providerbase-members"></a>Membres ProviderBase

| **Membre** | **Description** |
| --- | --- |
| Initialize (méthode) | Prend comme entrée le nom de l’instance du fournisseur et un NameValueCollection des paramètres de configuration. Utilisé pour définir les options et les valeurs de propriété pour l’instance du fournisseur, y compris les valeurs spécifiques à l’implémentation et les options spécifiées dans la configuration de l’ordinateur ou le fichier Web. config. |

### <a name="settingsprovider-members"></a>Membres SettingsProvider

| **Membre** | **Description** |
| --- | --- |
| ApplicationName, propriété | Nom de l’application qui est stockée avec chaque profil. Le fournisseur de profils utilise le nom de l’application pour stocker les informations de profil séparément pour chaque application. Cela permet à plusieurs applications ASP.NET d’utiliser la même source de données sans conflit si le même nom d’utilisateur est créé dans des applications différentes. Vous pouvez également partager une source de données de profil à l’aide de plusieurs applications ASP.NET en spécifiant le même nom d’application. |
| Méthode GetPropertyValues | Prend comme entrée un objet SettingsContext et un objet SettingsPropertyCollection. **SettingsContext** fournit des informations sur l’utilisateur. Vous pouvez utiliser les informations comme clé primaire pour récupérer les informations de propriété de profil de l’utilisateur. Utilisez l’objet **SettingsContext** pour récupérer le nom d’utilisateur et si l’utilisateur est authentifié ou anonyme. L’objet **SettingsPropertyCollection** contient une collection d’objets SettingsProperty. Chaque objet **SettingsProperty** fournit le nom et le type de la propriété, ainsi que des informations supplémentaires telles que la valeur par défaut pour la propriété et si la propriété est en lecture seule. La méthode **GetPropertyValues** remplit un objet SettingsPropertyValueCollection avec des objets SettingsPropertyValue en fonction des objets **SettingsProperty** fournis comme entrée. Les valeurs de la source de données pour l’utilisateur spécifié sont affectées aux propriétés PropertyValue pour chaque objet **SettingsPropertyValue** et la collection entière est retournée. L’appel de la méthode met également à jour la valeur LastActivityDate pour le profil utilisateur spécifié à la date et à l’heure actuelles. |
| Méthode SetPropertyValues | Prend comme entrée un objet **SettingsContext** et un objet **SettingsPropertyValueCollection** . **SettingsContext** fournit des informations sur l’utilisateur. Vous pouvez utiliser les informations comme clé primaire pour récupérer les informations de propriété de profil de l’utilisateur. Utilisez l’objet **SettingsContext** pour récupérer le nom d’utilisateur et si l’utilisateur est authentifié ou anonyme. **SettingsPropertyValueCollection** contient une collection d’objets **SettingsPropertyValue** . Chaque objet **SettingsPropertyValue** fournit le nom, le type et la valeur de la propriété, ainsi que des informations supplémentaires telles que la valeur par défaut pour la propriété et si la propriété est en lecture seule. La méthode **SetPropertyValues** met à jour les valeurs de propriété de profil dans la source de données pour l’utilisateur spécifié. L’appel de la méthode met également à jour les valeurs **LastActivityDate** et LastUpdatedDate pour le profil utilisateur spécifié à la date et à l’heure actuelles. |

### <a name="profileprovider-members"></a>Membres ProfileProvider

| **Membre** | **Description** |
| --- | --- |
| Méthode DeleteProfiles | Prend comme entrée un tableau de chaînes de noms d’utilisateur et supprime de la source de données toutes les informations de profil et les valeurs de propriété pour les noms spécifiés où le nom de l’application correspond à la valeur de propriété **applicationName** . Si votre source de données prend en charge les transactions, il est recommandé d’inclure toutes les opérations de suppression dans une transaction et de restaurer la transaction et de lever une exception en cas d’échec d’une opération de suppression. |
| Méthode DeleteProfiles | Prend comme entrée une collection d’objets ProfileInfo et supprime de la source de données toutes les informations de profil et les valeurs de propriété pour chaque profil où le nom de l’application correspond à la valeur de propriété **applicationName** . Si votre source de données prend en charge les transactions, il est recommandé d’inclure toutes les opérations de suppression dans une transaction et de restaurer la transaction et de lever une exception en cas d’échec d’une opération de suppression. |
| Méthode DeleteInactiveProfiles | Prend comme entrée une valeur ProfileAuthenticationOption et un objet DateTime, et supprime de la source de données toutes les informations de profil et les valeurs de propriété où la date de la dernière activité est inférieure ou égale à la date et l’heure spécifiées et où le nom de l’application correspond à la valeur de la propriété **applicationName** . Le paramètre **ProfileAuthenticationOption** spécifie si seuls les profils anonymes, les profils authentifiés ou tous les profils doivent être supprimés. Si votre source de données prend en charge les transactions, il est recommandé d’inclure toutes les opérations de suppression dans une transaction et de restaurer la transaction et de lever une exception en cas d’échec d’une opération de suppression. |
| Méthode GetAllProfiles | Prend comme entrée une valeur **ProfileAuthenticationOption** , un entier qui spécifie l’index de la page, un entier qui spécifie la taille de la page et une référence à un entier qui sera défini sur le nombre total de profils. Retourne un ProfileInfoCollection qui contient des objets **ProfileInfo** pour tous les profils de la source de données où le nom de l’application correspond à la valeur de la propriété **applicationName** . Le paramètre **ProfileAuthenticationOption** spécifie si seuls les profils anonymes, les profils authentifiés ou tous les profils doivent être retournés. Les résultats retournés par la méthode **GetAllProfiles** sont limités par les valeurs d’index de page et de taille de page. La valeur taille de la page spécifie le nombre maximal d’objets **ProfileInfo** à retourner dans le **ProfileInfoCollection**. La valeur d’index de page spécifie la page de résultats à retourner, où 1 identifie la première page. Le paramètre du nombre total d’enregistrements est un paramètre de sortie (vous pouvez utiliser **ByRef** dans Visual Basic) qui est défini sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et que la valeur d’index de la page est 2 avec une taille de page de 5, le **ProfileInfoCollection** retourné contient le sixième au dixième profiles. La valeur total des enregistrements est définie sur 13 lorsque la méthode est retournée. |
| Méthode GetAllInactiveProfiles | Prend comme entrée une valeur **ProfileAuthenticationOption** , un objet **DateTime** , un entier qui spécifie l’index de la page, un entier qui spécifie la taille de la page et une référence à un entier qui sera défini sur le nombre total de profils. Retourne un **ProfileInfoCollection** qui contient des objets **ProfileInfo** pour tous les profils de la source de données où la date de la dernière activité est inférieure ou égale à la valeur **DateTime** spécifiée et où le nom de l’application correspond à la valeur de la propriété **applicationName** . Le paramètre **ProfileAuthenticationOption** spécifie si seuls les profils anonymes, les profils authentifiés ou tous les profils doivent être retournés. Les résultats retournés par la méthode **GetAllInactiveProfiles** sont limités par les valeurs d’index de page et de taille de page. La valeur taille de la page spécifie le nombre maximal d’objets **ProfileInfo** à retourner dans le **ProfileInfoCollection**. La valeur d’index de page spécifie la page de résultats à retourner, où 1 identifie la première page. Le paramètre du nombre total d’enregistrements est un paramètre de sortie (vous pouvez utiliser **ByRef** dans Visual Basic) qui est défini sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et que la valeur d’index de la page est 2 avec une taille de page de 5, le **ProfileInfoCollection** retourné contient le sixième au dixième profiles. La valeur total des enregistrements est définie sur 13 lorsque la méthode est retournée. |
| Méthode FindProfilesByUserName | Prend comme entrée une valeur **ProfileAuthenticationOption** , une chaîne contenant un nom d’utilisateur, un entier qui spécifie l’index de page, un entier qui spécifie la taille de la page et une référence à un entier qui sera défini sur le nombre total de profils. Retourne un **ProfileInfoCollection** qui contient des objets **ProfileInfo** pour tous les profils de la source de données où le nom d’utilisateur correspond au nom d’utilisateur spécifié et où le nom de l’application correspond à la valeur de la propriété **applicationName** . Le paramètre **ProfileAuthenticationOption** spécifie si seuls les profils anonymes, les profils authentifiés ou tous les profils doivent être retournés. Si votre source de données prend en charge des fonctionnalités de recherche supplémentaires, telles que des caractères génériques, vous pouvez fournir des fonctionnalités de recherche plus étendues pour les noms d’utilisateur. Les résultats retournés par la méthode **FindProfilesByUserName** sont limités par les valeurs d’index de page et de taille de page. La valeur taille de la page spécifie le nombre maximal d’objets **ProfileInfo** à retourner dans le **ProfileInfoCollection**. La valeur d’index de page spécifie la page de résultats à retourner, où 1 identifie la première page. Le paramètre du nombre total d’enregistrements est un paramètre de sortie (vous pouvez utiliser **ByRef** dans Visual Basic) qui est défini sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et que la valeur d’index de la page est 2 avec une taille de page de 5, le **ProfileInfoCollection** retourné contient le sixième au dixième profiles. La valeur total des enregistrements est définie sur 13 lorsque la méthode est retournée. |
| Méthode FindInactiveProfilesByUserName | Prend comme entrée une valeur **ProfileAuthenticationOption** , une chaîne contenant un nom d’utilisateur, un objet **DateTime** , un entier qui spécifie l’index de la page, un entier qui spécifie la taille de la page et une référence à un entier qui sera défini sur le nombre total de profils. Retourne un **ProfileInfoCollection** qui contient des objets **ProfileInfo** pour tous les profils de la source de données où le nom d’utilisateur correspond au nom d’utilisateur spécifié, où la date de la dernière activité est inférieure ou égale à la valeur **DateTime**spécifiée, et où le nom de l’application correspond à la valeur de la propriété **applicationName** . Le paramètre **ProfileAuthenticationOption** spécifie si seuls les profils anonymes, les profils authentifiés ou tous les profils doivent être retournés. Si votre source de données prend en charge des fonctionnalités de recherche supplémentaires, telles que des caractères génériques, vous pouvez fournir des fonctionnalités de recherche plus étendues pour les noms d’utilisateur. Les résultats retournés par la méthode **FindInactiveProfilesByUserName** sont limités par les valeurs d’index de page et de taille de page. La valeur taille de la page spécifie le nombre maximal d’objets **ProfileInfo** à retourner dans le **ProfileInfoCollection**. La valeur d’index de page spécifie la page de résultats à retourner, où 1 identifie la première page. Le paramètre du nombre total d’enregistrements est un paramètre de sortie (vous pouvez utiliser **ByRef** dans Visual Basic) qui est défini sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et que la valeur d’index de la page est 2 avec une taille de page de 5, le **ProfileInfoCollection** retourné contient le sixième au dixième profiles. La valeur total des enregistrements est définie sur 13 lorsque la méthode est retournée. |
| Méthode GetNumberOfInActiveProfiles | Prend comme entrée une valeur **ProfileAuthenticationOption** et un objet **DateTime** et retourne le nombre de profils dans la source de données où la date de la dernière activité est inférieure ou égale à la valeur **DateTime** spécifiée et où le nom de l’application correspond à la valeur de la propriété **applicationName** . Le paramètre **ProfileAuthenticationOption** spécifie si seuls les profils anonymes, les profils authentifiés ou tous les profils doivent être comptés. |

### <a name="applicationname"></a>ApplicationName

Étant donné que les fournisseurs de profils stockent les informations de profil séparément pour chaque application, vous devez vous assurer que votre schéma de données inclut le nom de l’application et que les requêtes et les mises à jour incluent également le nom de l’application. Par exemple, la commande suivante est utilisée pour récupérer une valeur de propriété d’une base de données en fonction du nom d’utilisateur et si le profil est anonyme, et s’assure que la valeur **applicationName** est incluse dans la requête.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Thèmes ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Que sont les thèmes ASP.NET 2,0 ?

L’un des aspects les plus importants d’une application Web est une apparence cohérente sur le site. Les développeurs ASP.NET 1. x utilisent généralement feuilles de style en cascade (CSS) pour implémenter une apparence et une convivialité cohérentes. Les thèmes ASP.NET 2,0 s’améliorent de manière significative au format CSS, car ils donnent au développeur ASP.NET la possibilité de définir l’apparence des contrôles serveur ASP.NET ainsi que des éléments HTML. Les thèmes ASP.NET peuvent être appliqués à des contrôles individuels, à une page Web spécifique ou à une application Web entière. Les thèmes utilisent une combinaison de fichiers CSS, un fichier d’apparence facultatif et un répertoire d’images facultatif si des images sont nécessaires. Le fichier d’apparence contrôle l’apparence visuelle des contrôles serveur ASP.NET.

## <a name="where-are-themes-stored"></a>Où sont stockés les thèmes ?

L’emplacement de stockage des thèmes diffère en fonction de leur étendue. Les thèmes qui peuvent être appliqués à n’importe quelle application sont stockés dans le dossier suivant :

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Un thème spécifique à une application particulière est stocké dans un répertoire `App\_Themes\<Theme\_Name>` à la racine du site Web.

> [!NOTE]
> Un fichier d’apparence ne doit modifier que les propriétés du contrôle serveur qui affectent l’apparence.

Un thème global est un thème qui peut être appliqué à n’importe quelle application ou site Web s’exécutant sur le serveur Web. Ces thèmes sont stockés par défaut dans le répertoire ASP. NETClientfiles\Themes qui se trouve dans le répertoire v2. x. xxxxx. Vous pouvez également déplacer les fichiers de thème dans le dossier ASPNET\_client/système\_Web/[version]/Themes/[Theme\_Name] à la racine de votre site Web.

Les thèmes spécifiques à l’application ne peuvent être appliqués qu’à l’application dans laquelle résident les fichiers. Ces fichiers sont stockés dans le répertoire `App\_Themes/<theme\_name>` à la racine du site Web.

## <a name="the-components-of-a-theme"></a>Composants d’un thème

Un thème est constitué d’un ou de plusieurs fichiers CSS, d’un fichier d’apparence facultatif et d’un dossier d’images facultatif. Les fichiers CSS peuvent être n’importe quel nom de votre choix (par exemple, default. CSS ou Theme. CSS, etc.) et doivent se trouver à la racine du dossier themes. Les fichiers CSS sont utilisés pour définir des classes et des attributs CSS ordinaires pour des sélecteurs spécifiques. Pour appliquer l’une des classes CSS à un élément de page, la propriété **CssClass** est utilisée.

Le fichier d’apparence est un fichier XML qui contient des définitions de propriétés pour les contrôles serveur ASP.NET. Le code ci-dessous est un exemple de fichier d’apparence.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

La **figure 1** ci-dessous montre une petite page ASP.net parcourue sans thème appliqué. La **figure 2** montre le même fichier avec un thème appliqué. La couleur d’arrière-plan et la couleur du texte sont configurées via un fichier CSS. L’apparence du bouton et de la zone de texte est configurée à l’aide du fichier d’apparence indiqué ci-dessus.

![Aucun thème](profiles-themes-and-web-parts/_static/image1.gif)

**Figure 1**: aucun thème

![Thème appliqué](profiles-themes-and-web-parts/_static/image2.gif)

**Figure 2**: thème appliqué

Le fichier d’apparence indiqué ci-dessus définit une apparence par défaut pour tous les contrôles de zone de texte et contrôles de bouton. Cela signifie que chaque contrôle de zone de texte et contrôle de bouton inséré sur une page prend cette apparence. Vous pouvez également définir une apparence qui peut être appliquée à des instances spécifiques de ces contrôles à l’aide de la propriété **SkinID** du contrôle.

Le code ci-dessous définit une apparence pour un contrôle bouton. Seuls les contrôles Button avec une propriété **SkinID** de **goButton** prennent la forme de l’apparence.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Vous ne pouvez avoir qu’une apparence par défaut par type de contrôle serveur. Si vous avez besoin d’apparences supplémentaires, vous devez utiliser la propriété SkinID.

## <a name="applying-themes-to-pages"></a>Application de thèmes à des pages

Un thème peut être appliqué à l’aide de l’une des méthodes suivantes :

- Dans l’élément &lt;pages&gt; du fichier Web. config
- Dans la directive @Page d’une page
- Par programmation

## <a name="applying-a-theme-in-the-configuration-file"></a>Application d’un thème dans le fichier de configuration

Pour appliquer un thème dans le fichier de configuration des applications, utilisez la syntaxe suivante :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Le nom du thème spécifié ici doit correspondre au nom du dossier themes. Ce dossier peut se trouver dans l’un des emplacements mentionnés précédemment dans ce cours. Si vous tentez d’appliquer un thème qui n’existe pas, une erreur de configuration se produit.

## <a name="applying-a-theme-in-the-page-directive"></a>Application d’un thème dans la directive page

Vous pouvez également appliquer un thème dans la directive @ page. Cette méthode vous permet d’utiliser un thème pour une page spécifique.

Pour appliquer un thème dans la directive @Page, utilisez la syntaxe suivante :

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Là encore, le thème spécifié ici doit correspondre au dossier thème mentionné précédemment. Si vous tentez d’appliquer un thème qui n’existe pas, une erreur de génération se produit. Visual Studio met également en surbrillance l’attribut et vous informe qu’il n’existe pas de thème de ce type.

## <a name="applying-a-theme-programmatically"></a>Application d’un thème par programmation

Pour appliquer un thème par programme, vous devez spécifier la propriété **Theme** pour la page dans la **page\_méthode PreInit** .

Pour appliquer un thème par programme, utilisez la syntaxe suivante :

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Il est nécessaire d’appliquer le thème dans la méthode PreInit en raison du cycle de vie de la page. Si vous l’appliquez après ce point, le thème des pages aura déjà été appliqué par le runtime et une modification à ce stade est trop tard dans le cycle de vie. Si vous appliquez un thème qui n’existe pas, un **HttpException** se produit. Quand un thème est appliqué par programme, un avertissement de génération se produit si des contrôles serveur ont une propriété SkinID spécifiée. Cet avertissement est destiné à vous informer qu’aucun thème n’est appliqué de façon déclarative et qu’il peut être ignoré.

## <a name="exercise-1--applying-a-theme"></a>Exercice 1 : application d’un thème

Dans cet exercice, vous allez appliquer un thème ASP.NET à un site Web.

> [!IMPORTANT]
> Si vous utilisez Microsoft Word pour entrer des informations dans un fichier d’apparence, assurez-vous que vous ne remplacez pas les guillemets habituels par des guillemets typographiques. Les guillemets typographiques entraînent des problèmes avec les fichiers d’apparence.

1. Créez un site Web ASP.NET.
2. Cliquez avec le bouton droit sur le projet dans Explorateur de solutions et choisissez Ajouter un nouvel élément.
3. Choisissez fichier de configuration Web dans la liste des fichiers, puis cliquez sur Ajouter.
4. Cliquez avec le bouton droit sur le projet dans Explorateur de solutions et choisissez Ajouter un nouvel élément.
5. Choisissez fichier d’apparence, puis cliquez sur Ajouter.
6. Cliquez sur Oui lorsque vous y êtes invité si vous souhaitez placer le fichier dans le dossier App\_themes.
7. Cliquez avec le bouton droit sur le dossier SkinFile dans le dossier App\_themes dans Explorateur de solutions et choisissez Ajouter un nouvel élément.
8. Choisissez feuille de style dans la liste des fichiers, puis cliquez sur Ajouter. Vous disposez maintenant de tous les fichiers nécessaires pour implémenter votre nouveau thème. Toutefois, Visual Studio a nommé votre dossier themes SkinFile. Cliquez avec le bouton droit sur ce dossier et remplacez le nom par CoolTheme.
9. Ouvrez le fichier SkinFile. Skin et ajoutez le code suivant à la fin du fichier : 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Enregistrez le fichier SkinFile. skin.
11. Ouvrez StyleSheet. css.
12. Remplacez tout le texte qu’il contient par ce qui suit : 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Enregistrez le fichier StyleSheet. css.
14. Ouvrez la page default. aspx.
15. Ajoutez un contrôle TextBox et un contrôle Button.
16. Enregistrez la page. À présent, parcourez la page default. aspx. Il doit s’afficher comme un formulaire Web normal.
17. Ouvrez le fichier Web. config.
18. Ajoutez le code suivant directement sous la balise `<system.web>` ouvrante : 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Enregistrez le fichier Web. config. À présent, parcourez la page default. aspx. Elle doit s’afficher avec le thème appliqué.
20. S’il n’est pas déjà ouvert, ouvrez la page default. aspx dans Visual Studio.
21. Sélectionnez le bouton.
22. Modifiez la propriété **SkinID** en goButton. Notez que Visual Studio fournit une liste déroulante avec des valeurs SkinID valides pour un contrôle Button.
23. Enregistrez la page. À présent, affichez à nouveau la page dans votre navigateur. Le bouton doit maintenant indiquer « Go » et doit être plus grand en apparence.

À l’aide de la propriété **SkinID** , vous pouvez facilement configurer différentes apparences pour différentes instances d’un type particulier de contrôle serveur.

## <a name="the-stylesheettheme-property"></a>La propriété StyleSheetTheme

Jusqu’à présent, nous avons parlé uniquement de l’application de thèmes à l’aide de la propriété Theme. Quand vous utilisez la propriété Theme, le fichier d’apparence remplace tous les paramètres déclaratifs des contrôles serveur. Par exemple, dans l’exercice 1, vous avez spécifié un SkinID de « goButton » pour le contrôle Button et qui a remplacé le texte du bouton par « Go ». Vous avez peut-être remarqué que la propriété Text du bouton dans le concepteur était définie sur « Button », mais le thème a remplacé. Le thème remplacera toujours tous les paramètres de propriété dans le concepteur.

Si vous souhaitez être en mesure de substituer les propriétés définies dans le fichier d’apparence du thème avec les propriétés spécifiées dans le concepteur, vous pouvez utiliser la propriété **StyleSheetTheme** à la place de la propriété Theme. La propriété StyleSheetTheme est identique à la propriété Theme, sauf qu’elle ne remplace pas tous les paramètres de propriété explicites comme le fait la propriété Theme.

Pour voir cela en action, ouvrez le fichier Web. config à partir du projet dans l’exercice 1 et remplacez l’élément `<pages>` par ce qui suit :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

À présent, parcourez la page default. aspx et vous verrez que le contrôle Button a une nouvelle propriété Text de « Button ». Cela est dû au fait que le paramètre de propriété explicite dans le concepteur remplace la propriété Text définie par le goButton SkinID.

## <a name="overriding-themes"></a>Remplacer des thèmes

Un thème global peut être substitué en appliquant un thème portant le même nom dans le dossier App\_themes de l’application. Toutefois, le thème n’est pas appliqué dans un scénario de remplacement réel. Si le runtime rencontre des fichiers de thème dans le dossier App\_Themes, il applique le thème à l’aide de ces fichiers et ignore le thème global.

La propriété StyleSheetTheme est substituable et peut être substituée dans le code comme suit :

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>WebParts

ASP.NET composants WebPart est un ensemble intégré de contrôles pour la création de sites Web qui permettent aux utilisateurs finaux de modifier le contenu, l’apparence et le comportement de pages Web directement à partir d’un navigateur. Les modifications peuvent être appliquées à tous les utilisateurs du site ou à des utilisateurs individuels. Lorsque les utilisateurs modifient les pages et les contrôles, les paramètres peuvent être enregistrés pour conserver les préférences personnelles d’un utilisateur dans les futures sessions de navigation, une fonctionnalité appelée personnalisation. Ces fonctionnalités de composants WebPart signifient que les développeurs peuvent aider les utilisateurs finaux à personnaliser une application Web de manière dynamique, sans intervention du développeur ou de l’administrateur.

À l’aide de l’ensemble de contrôles composants WebPart, vous en êtes un développeur peut permettre aux utilisateurs finaux d’effectuer les opérations suivantes :

- Personnaliser le contenu de la page. Les utilisateurs peuvent ajouter de nouveaux contrôles de composants WebPart à une page, les supprimer, les masquer ou les réduire comme des fenêtres ordinaires.
- Personnaliser la mise en page. Les utilisateurs peuvent faire glisser un contrôle composants WebPart vers une zone différente sur une page, ou modifier l’apparence, les propriétés et le comportement.
- Exporter et importer des contrôles. Les utilisateurs peuvent importer ou exporter des paramètres de contrôle composants WebPart pour les utiliser dans d’autres pages ou sites, en conservant les propriétés, l’apparence et même les données dans les contrôles. Cela réduit les demandes d’entrée et de configuration de données pour les utilisateurs finaux.
- Créer des connexions. Les utilisateurs peuvent établir des connexions entre les contrôles afin que, par exemple, un contrôle Chart puisse afficher un graphique pour les données dans un contrôle de cotation boursière. Les utilisateurs peuvent personnaliser non seulement la connexion elle-même, mais aussi l’apparence et les détails de la façon dont le contrôle Chart affiche les données.
- Gérez et personnalisez les paramètres au niveau du site. Les utilisateurs autorisés peuvent configurer des paramètres au niveau du site, déterminer qui peut accéder à un site ou une page, définir l’accès en fonction du rôle aux contrôles, et ainsi de suite. Par exemple, un utilisateur dans un rôle d’administrateur peut définir un contrôle de composants WebPart pour qu’il soit partagé par tous les utilisateurs et empêcher les utilisateurs qui ne sont pas des administrateurs de personnaliser le contrôle partagé.

En général, vous travaillez avec composants WebPart de l’une des trois façons suivantes : créer des pages qui utilisent des contrôles composants WebPart, créer des contrôles composants WebPart individuels ou créer des applications Web complètes et personnalisables, telles qu’un portail.

## <a name="page-development"></a>Développement de pages

Les développeurs de pages peuvent utiliser des outils de conception visuelle comme Microsoft Visual Studio 2005 pour créer des pages qui utilisent composants WebPart. L’un des avantages de l’utilisation d’un outil tel que Visual Studio est que le jeu de contrôles composants WebPart fournit des fonctionnalités pour la création et la configuration de composants WebPart contrôles dans un concepteur visuel. Par exemple, vous pouvez utiliser le concepteur pour faire glisser une zone de composants WebPart ou un contrôle d’éditeur de composants WebPart, sur l’aire de conception, puis configurer le contrôle directement dans le concepteur à l’aide de l’interface utilisateur fournie par le jeu de contrôles de composants WebPart. Cela peut accélérer le développement d’applications composants WebPart et réduire la quantité de code à écrire.

## <a name="control-development"></a>Développement de contrôles

Vous pouvez utiliser n’importe quel contrôle ASP.NET existant comme contrôle composants WebPart, y compris les contrôles serveur Web standard, les contrôles serveur personnalisés et les contrôles utilisateur. Pour un contrôle par programmation maximal de votre environnement, vous pouvez également créer des contrôles de composants WebPart personnalisés qui dérivent de la classe WebPart. Pour le développement d’un contrôle de composants WebPart individuel, vous allez généralement créer un contrôle utilisateur et l’utiliser en tant que contrôle composants WebPart, ou développer un contrôle composants WebPart personnalisé.

En guise d’exemple de développement d’un contrôle de composants WebPart personnalisé, vous pouvez créer un contrôle pour fournir toutes les fonctionnalités fournies par d’autres contrôles serveur ASP.NET qui peuvent être utiles pour le package en tant que contrôle composants WebPart personnalisable : calendriers, listes, informations financières, nouvelles, calculatrices, contrôles de texte enrichi pour la mise à jour de contenu, grilles modifiables qui se connectent aux bases de données, graphiques qui mettent à jour leurs affichages de manière dynamique, ou les informations météorologiques et de voyage. Si vous fournissez un concepteur visuel avec votre contrôle, n’importe quel développeur de pages à l’aide de Visual Studio peut simplement faire glisser votre contrôle dans une zone de composants WebPart et le configurer au moment du design sans avoir à écrire du code supplémentaire.

La personnalisation est la base de la fonctionnalité composants WebPart. Il permet aux utilisateurs de modifier ou de personnaliser : la disposition, l’apparence et le comportement des contrôles composants WebPart sur une page. Les paramètres personnalisés sont de longue durée : ils sont conservés non seulement pendant la session de navigateur active (comme avec l’état d’affichage), mais aussi dans le stockage à long terme, afin que les paramètres d’un utilisateur soient également enregistrés pour les futures sessions de navigateur. La personnalisation est activée par défaut pour les pages de composants WebPart.

Les composants structurels de l’interface utilisateur s’appuient sur la personnalisation et fournissent la structure et les services principaux nécessaires à tous les contrôles composants WebPart. Un composant structurel de l’interface utilisateur requis sur chaque page de composants WebPart est le contrôle WebPartManager. Bien qu’il ne soit jamais visible, ce contrôle a la tâche critique de coordonner tous les contrôles de composants WebPart sur une page. Par exemple, il effectue le suivi de tous les contrôles de composants WebPart individuels. Il gère les zones de composants WebPart (régions qui contiennent des contrôles composants WebPart sur une page) et Quels contrôles sont dans quelles zones. Il suit également et contrôle les différents modes d’affichage dans lesquels une page peut se trouver, par exemple parcourir, connecter, modifier ou mode catalogue, et si les modifications de personnalisation s’appliquent à tous les utilisateurs ou à des utilisateurs individuels. Enfin, il initie et effectue le suivi des connexions et de la communication entre les contrôles de composants WebPart.

Le deuxième type de composant structurel de l’interface utilisateur est la zone. Les zones agissent en tant que gestionnaires de dispositions sur une page composants WebPart. Ils contiennent et organisent les contrôles qui dérivent de la classe part (contrôles part) et offrent la possibilité d’effectuer une mise en page modulaire en orientation horizontale ou verticale. Les zones offrent également des éléments d’interface utilisateur communs et cohérents (tels que le style d’en-tête et de pied de page, le titre, le style de bordure, les boutons d’action, etc.) pour chaque contrôle qu’elles contiennent. ces éléments communs sont appelés chrome d’un contrôle. Plusieurs types spécialisés de zones sont utilisés dans les différents modes d’affichage et avec différents contrôles. Les différents types de zones sont décrits dans la section composants WebPart les contrôles essentiels ci-dessous.

Les contrôles d’interface utilisateur composants WebPart, qui dérivent tous de la classe **part** , constituent l’interface utilisateur principale sur une page composants WebPart. Le jeu de contrôles composants WebPart est flexible et inclusif dans les options qui vous permettent de créer des contrôles part. Outre la création de vos propres contrôles de composants WebPart personnalisés, vous pouvez également utiliser des contrôles serveur ASP.NET, des contrôles utilisateur ou des contrôles serveur personnalisés existants comme composants WebPart contrôles. Les contrôles essentiels les plus couramment utilisés pour créer des pages de composants WebPart sont décrits dans la section suivante.

## <a name="web-parts-essential-controls"></a>composants WebPart les contrôles essentiels

Le jeu de contrôles composants WebPart est étendu, mais certains contrôles sont essentiels, soit parce qu’ils sont requis pour le fonctionnement de composants WebPart, soit parce qu’il s’agit des contrôles les plus fréquemment utilisés sur les pages composants WebPart. Lorsque vous commencez à utiliser composants WebPart et à créer des pages de composants WebPart de base, il est utile de vous familiariser avec les contrôles de composants WebPart essentiels décrits dans le tableau suivant.

| **Contrôle composants WebPart** | **Description** |
| --- | --- |
| WebPartManager | Gère tous les contrôles de composants WebPart sur une page. Un contrôle **WebPartManager** (et un seul) est requis pour chaque page de composants WebPart. |
| CatalogZone | Contient des contrôles CatalogPart. Utilisez cette zone pour créer un catalogue de composants WebPart contrôles à partir desquels les utilisateurs peuvent sélectionner des contrôles à ajouter à une page. |
| EditorZone | Contient des contrôles EditorPart. Utilisez cette zone pour permettre aux utilisateurs de modifier et de personnaliser composants WebPart contrôles d’une page. |
| WebPartZone | Contient et fournit la disposition générale pour les contrôles WebPart qui composent l’interface utilisateur principale d’une page. Utilisez cette zone chaque fois que vous créez des pages avec des contrôles de composants WebPart. Les pages peuvent contenir une ou plusieurs zones. |
| ConnectionsZone | Contient des contrôles WebPartConnection et fournit une interface utilisateur pour la gestion des connexions. |
| WebPart (GenericWebPart) | Génère le rendu de l’interface utilisateur principale ; la plupart des composants WebPart contrôles d’interface utilisateur appartiennent à cette catégorie. Pour un contrôle par programmation maximal, vous pouvez créer des contrôles de composants WebPart personnalisés qui dérivent du contrôle **WebPart** de base. Vous pouvez également utiliser des contrôles serveur, des contrôles utilisateur ou des contrôles personnalisés existants comme composants WebPart contrôles. Chaque fois que l’un de ces contrôles est placé dans une zone, le contrôle **WebPartManager** les encapsule automatiquement avec les contrôles **GenericWebPart** au moment de l’exécution afin que vous puissiez les utiliser avec des fonctionnalités de composants WebPart. |
| CatalogPart | Contient la liste des contrôles composants WebPart disponibles que les utilisateurs peuvent ajouter à la page. |
| WebPartConnection | Crée une connexion entre deux contrôles composants WebPart sur une page. La connexion définit l’un des contrôles composants WebPart en tant que fournisseur (de données) et l’autre en tant que consommateur. |
| EditorPart | Sert de classe de base pour les contrôles d’éditeur spécialisés. |
| Contrôles EditorPart (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart et PropertyGridEditorPart) | Autoriser les utilisateurs à personnaliser différents aspects des contrôles d’interface utilisateur composants WebPart sur une page |

## <a name="lab-create-a-web-part-page"></a>Lab : créer une page de composants WebPart

Dans cet atelier, vous allez créer une page de composants WebPart qui rendra les informations persistantes via les profils ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Création d’une page simple avec composants WebPart

Dans cette partie de la procédure pas à pas, vous allez créer une page qui utilise composants WebPart contrôles pour afficher le contenu statique. La première étape de l’utilisation de composants WebPart consiste à créer une page avec deux éléments structurels requis. Tout d’abord, une page WebPart a besoin d’un contrôle WebPartManager pour suivre et coordonner tous les contrôles composants WebPart. Deuxièmement, une page de composants WebPart a besoin d’une ou plusieurs zones, qui sont des contrôles composites qui contiennent des contrôles WebPart ou d’autres contrôles serveur et occupent une région spécifiée d’une page.

> [!NOTE]
> Vous n’avez rien à faire pour activer la personnalisation composants WebPart ; Il est activé par défaut pour le jeu de contrôles composants WebPart. Lorsque vous exécutez pour la première fois une page composants WebPart sur un site, ASP.NET configure un fournisseur de personnalisations par défaut pour stocker les paramètres de personnalisation de l’utilisateur. Pour plus d’informations sur la personnalisation, consultez composants WebPart vue d’ensemble de la personnalisation.

### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Pour créer une page destinée à contenir des contrôles composants WebPart

1. Fermez la page par défaut et ajoutez une nouvelle page au site nommé WebPartsDemo. aspx.
2. Basculez en mode **Design** .
3. Dans le menu **affichage** , assurez-vous que les options **contrôles non visuels** et **Détails** sont sélectionnées afin que vous puissiez voir les balises de mise en page et les contrôles qui n’ont pas d’interface utilisateur.
4. Placez le point d’insertion avant les balises `<div>` sur l’aire de conception, puis appuyez sur entrée pour ajouter une nouvelle ligne. Placez le point d’insertion avant le caractère de nouvelle ligne, cliquez sur le contrôle de liste déroulante **format de bloc** dans le menu, puis sélectionnez l’option **titre 1** . Dans l’en-tête, ajoutez le texte **composants WebPart page de démonstration**.
5. À partir de l’onglet **WebParts** de la boîte à outils, faites glisser un contrôle **WebPartManager** sur la page, en le positionnant juste après le caractère de nouvelle ligne et avant les balises `<div>`.   
  
   Le contrôle **WebPartManager** n’affiche aucune sortie, il apparaît donc sous la forme d’une zone grise sur l’aire du concepteur.
6. Placez le point d’insertion dans les balises `<div>`.
7. Dans le menu **disposition** , cliquez sur **Insérer un tableau**, puis créez une nouvelle table qui comporte une ligne et trois colonnes. Cliquez sur le bouton Propriétés de la **cellule** , sélectionnez **haut** dans la liste déroulante **alignement vertical** , cliquez sur **OK**, puis de nouveau sur **OK** pour créer la table.
8. Faites glisser un contrôle WebPartZone dans la colonne de la table de gauche. Cliquez avec le bouton droit sur le contrôle **WebPartZone** , choisissez **Propriétés**, puis définissez les propriétés suivantes :   
  
   ID : SidebarZone   
  
   HeaderText : encadré
9. Faites glisser un deuxième contrôle **WebPartZone** vers la colonne de la table centrale et définissez les propriétés suivantes :   
  
   ID : MainZone   
  
   HeaderText : main
10. Enregistrez le fichier.

Votre page contient désormais deux zones distinctes que vous pouvez contrôler séparément. Toutefois, aucune zone n’a de contenu, donc la création d’un contenu est l’étape suivante. Pour cette procédure pas à pas, vous travaillez avec des contrôles composants WebPart qui affichent uniquement du contenu statique.

La disposition d’une zone de composants WebPart est spécifiée par un élément&gt; &lt;ZoneTemplate. Dans le modèle de zone, vous pouvez ajouter n’importe quel contrôle ASP.NET, qu’il s’agisse d’un contrôle composants WebPart personnalisé, d’un contrôle utilisateur ou d’un contrôle serveur existant. Notez qu’ici, vous utilisez le contrôle Label et que vous ajoutez simplement du texte statique. Quand vous placez un contrôle serveur normal dans une zone **WebPartZone** , ASP.net traite le contrôle comme un contrôle de composants WebPart au moment de l’exécution, ce qui active composants WebPart fonctionnalités sur le contrôle.

**Pour créer du contenu pour la zone principale**

1. En mode **conception** , faites glisser un contrôle **label** de l’onglet **standard** de la boîte à outils vers la zone contenu de la zone dont la propriété **ID** est définie sur MainZone.
2. Basculez en mode **source** . Notez qu’un élément &lt;ZoneTemplate&gt; a été ajouté pour encapsuler le contrôle **label** dans le MainZone.
3. Ajoutez un attribut nommé **title** à l’élément &lt;asp : label&gt; et affectez-lui la valeur Content. Supprimez l’attribut Text = "label" de l’élément &lt;asp : label&gt;. Entre les balises d’ouverture et de fermeture de l’élément &lt;asp : label&gt;, ajoutez du texte, par exemple **Bienvenue dans ma page d’accueil** au sein d’une paire de balises d’élément &lt;H2&gt;. Votre code doit se présenter comme suit. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Enregistrez le fichier.

Ensuite, créez un contrôle utilisateur qui peut également être ajouté à la page en tant que contrôle composants WebPart.

### <a name="to-create-a-user-control"></a>Pour créer un contrôle utilisateur

1. Ajoutez un nouveau contrôle utilisateur Web à votre site pour servir de contrôle de recherche. Désélectionnez l’option permettant de **Placer le code source dans un fichier séparé**. Ajoutez-le dans le même répertoire que la page WebPartsDemo. aspx, puis nommez-le SearchUserControl. ascx.   
  
    > [!NOTE]
    > Le contrôle utilisateur pour cette procédure pas à pas n’implémente pas les fonctionnalités de recherche réelles. elle sert uniquement à illustrer les fonctionnalités de composants WebPart.
2. Basculez en mode **Design** . À partir de l’onglet **standard** de la boîte à outils, faites glisser un contrôle TextBox sur la page.
3. Placez le point d’insertion après la zone de texte que vous venez d’ajouter, puis appuyez sur entrée pour ajouter une nouvelle ligne.
4. Faites glisser un contrôle Button sur la page sur la nouvelle ligne en dessous de la zone de texte que vous venez d’ajouter.
5. Basculez en mode **source** . Assurez-vous que le code source du contrôle utilisateur ressemble à l’exemple suivant. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Enregistrez et fermez le fichier.

Vous pouvez maintenant ajouter des contrôles de composants WebPart à la zone de barre latérale. Vous ajoutez deux contrôles à la zone Sidebar, un contenant une liste de liens et un autre qui est le contrôle utilisateur que vous avez créé dans la procédure précédente. Les liens sont ajoutés en tant que contrôle serveur d' **étiquette** standard, de la même façon que vous avez créé le texte statique pour la zone principale. Toutefois, même si les contrôles serveur individuels contenus dans le contrôle utilisateur peuvent être contenus directement dans la zone (comme le contrôle Label), dans ce cas, ils ne le sont pas. Au lieu de cela, elles font partie du contrôle utilisateur que vous avez créé dans la procédure précédente. Cela illustre une méthode courante pour empaqueter les contrôles et les fonctionnalités supplémentaires souhaités dans un contrôle utilisateur, puis référencer ce contrôle dans une zone comme un contrôle de composants WebPart.

Au moment de l’exécution, le jeu de contrôles composants WebPart encapsule les deux contrôles avec les contrôles GenericWebPart. Lorsqu’un contrôle **GenericWebPart** encapsule un contrôle serveur Web, le contrôle de partie générique est le contrôle parent et vous pouvez accéder au contrôle serveur via la propriété ChildControl du contrôle parent. Cette utilisation de contrôles de composant génériques permet aux contrôles serveur Web standard d’avoir le même comportement et les mêmes attributs de base que les contrôles composants WebPart qui dérivent de la classe **WebPart** .

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Pour ajouter des contrôles composants WebPart à la zone de barre latérale

1. Ouvrez la page WebPartsDemo. aspx.
2. Basculez en mode **Design** .
3. Faites glisser la page de contrôle utilisateur que vous avez créée, SearchUserControl. ascx, de **Explorateur de solutions** vers la zone dont la propriété **ID** est définie sur SidebarZone, puis déposez-la.
4. Enregistrez la page WebPartsDemo. aspx.
5. Basculez en mode **source** .
6. À l’intérieur de l’élément &lt;asp : WebPartZone&gt; pour SidebarZone, juste au-dessus de la référence à votre contrôle utilisateur, ajoutez un élément &lt;asp : label&gt; avec les liens contenus, comme indiqué dans l’exemple suivant. Ajoutez également un attribut **title** à la balise de contrôle utilisateur, avec la valeur **Search**, comme indiqué. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Enregistrez et fermez le fichier.

Vous pouvez maintenant tester votre page en y accédant dans votre navigateur. La page affiche les deux zones. La capture d’écran suivante montre la page.

**Page de démonstration composants WebPart avec deux zones**

![Capture d’écran composants WebPart VS 1](profiles-themes-and-web-parts/_static/image3.gif)

**Figure 3**: capture d’écran composants WebPart vs 1

Dans la barre de titre de chaque contrôle se trouve une flèche vers le bas qui permet d’accéder à un menu de verbes des actions disponibles que vous pouvez effectuer sur un contrôle. Cliquez sur le menu verbes pour l’un des contrôles, puis cliquez sur le verbe **réduire** et notez que le contrôle est réduit. Dans le menu verbes, cliquez sur **restaurer**pour que le contrôle retourne à sa taille normale.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Permettre aux utilisateurs de modifier des pages et de modifier la disposition

Composants WebPart offre aux utilisateurs la possibilité de modifier la disposition des contrôles composants WebPart en les faisant glisser d’une zone à l’autre. En plus de permettre aux utilisateurs de déplacer des contrôles **WebPart** d’une zone à une autre, vous pouvez autoriser les utilisateurs à modifier différentes caractéristiques des contrôles, notamment leur apparence, leur disposition et leur comportement. Le jeu de contrôles composants WebPart fournit des fonctionnalités d’édition de base pour les contrôles **WebPart** . Même si vous ne le faites pas dans cette procédure pas à pas, vous pouvez également créer des contrôles d’éditeur personnalisés qui permettent aux utilisateurs de modifier les fonctionnalités des contrôles **WebPart** . Comme pour la modification de l’emplacement d’un contrôle **WebPart** , la modification des propriétés d’un contrôle s’appuie sur la personnalisation de ASP.net pour enregistrer les modifications apportées par les utilisateurs.

Dans cette partie de la procédure pas à pas, vous ajoutez la possibilité pour les utilisateurs de modifier les caractéristiques de base d’un contrôle **WebPart** sur la page. Pour activer ces fonctionnalités, vous ajoutez un autre contrôle utilisateur personnalisé à la page, ainsi qu’un élément &lt;asp : EditorZone&gt; et deux contrôles d’édition.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Pour créer un contrôle utilisateur qui permet de modifier la disposition de la page

1. Dans Visual Studio, dans le menu **fichier** , sélectionnez le sous-menu **nouveau** , puis cliquez sur l’option **fichier** .
2. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **contrôle utilisateur Web**. Nommez le nouveau fichier DisplayModeMenu. ascx. Désélectionnez l’option permettant de **Placer le code source dans un fichier séparé**.
3. Cliquez sur Ajouter pour créer le contrôle.
4. Basculez en mode **source** .
5. Supprimez tout le code existant dans le nouveau fichier, puis collez le code suivant. Ce code de contrôle utilisateur utilise les fonctionnalités du jeu de contrôles composants WebPart qui permettent à une page de modifier son mode d’affichage ou d’affichage, et vous permet également de modifier l’apparence physique et la disposition de la page pendant que vous êtes dans certains modes d’affichage. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Enregistrez le fichier en cliquant sur l’icône Enregistrer dans la barre d’outils, ou en sélectionnant **Enregistrer** dans le menu **fichier** .

### <a name="to-enable-users-to-change-the-layout"></a>Pour permettre aux utilisateurs de modifier la disposition

1. Ouvrez la page WebPartsDemo. aspx et basculez en mode **Design** .
2. Placez le point d’insertion en mode **Design** juste après le contrôle **WebPartManager** que vous avez ajouté précédemment. Ajoutez un retour à la ligne après le texte afin qu’il y ait une ligne vide après le contrôle **WebPartManager** . Placez le point d’insertion sur la ligne vide.
3. Faites glisser le contrôle utilisateur que vous venez de créer (le fichier se nomme DisplayModeMenu. ascx) dans la page WebPartsDemo. aspx et déposez-le sur la ligne vide.
4. Faites glisser un contrôle EditorZone de la section **WebParts** de la boîte à outils vers la cellule de table ouverte restante dans la page WebPartsDemo. aspx.
5. À partir de la section **WebParts** de la boîte à outils, faites glisser un contrôle AppearanceEditorPart et un contrôle LayoutEditorPart dans le contrôle **EditorZone** .
6. Basculez en mode **source** . Le code résultant dans la cellule de tableau doit se présenter comme dans le code suivant. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Enregistrez le fichier WebPartsDemo. aspx. Vous avez créé un contrôle utilisateur qui vous permet de modifier les modes d’affichage et de modifier la mise en page, et vous avez référencé le contrôle sur la page Web principale.

Vous pouvez maintenant tester la fonctionnalité de modification des pages et de modification de la disposition.

### <a name="to-test-layout-changes"></a>Pour tester les modifications de disposition

1. Chargez la page dans un navigateur.
2. Cliquez sur le menu déroulant **mode d’affichage** , puis sélectionnez **modifier**. Les titres de zone s’affichent.
3. Faites glisser le contrôle **My Links** sur sa barre de titre de la zone de barre latérale vers le bas de la zone principale. Votre page doit ressembler à la capture d’écran suivante.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Page de démonstration composants WebPart avec le contrôle My Links déplacé

![Capture d’écran composants WebPart VS-démonstration 2](profiles-themes-and-web-parts/_static/image4.gif)

**Figure 4**: capture d’écran composants WebPart vs de la procédure pas à pas 2

1. Cliquez sur le menu déroulant **mode d’affichage** , puis sélectionnez **Parcourir**. La page est actualisée, les noms de zone disparaissent et le contrôle **mes liens** reste là où vous l’avez positionné.
2. Pour montrer que la personnalisation fonctionne, fermez le navigateur, puis rechargez la page. Les modifications que vous avez apportées sont enregistrées pour les futures sessions de navigation.
3. Dans le menu **mode d’affichage** , sélectionnez **modifier**.   
  
   Chaque contrôle de la page s’affiche maintenant avec une flèche vers le bas dans sa barre de titre, qui contient le menu déroulant des verbes.
4. Cliquez sur la flèche pour afficher le menu des verbes dans le contrôle **My Links** . Cliquez sur le verbe **Edit** .   
  
   Le contrôle **EditorZone** apparaît, affichant les contrôles EditorPart que vous avez ajoutés.
5. Dans la section **apparence** du contrôle d’édition, remplacez le **titre** par mes favoris, utilisez la liste déroulante **type de chrome** pour sélectionner **titre uniquement**, puis cliquez sur **appliquer**. La capture d’écran suivante montre la page en mode édition.

### <a name="web-parts-demo-page-in-edit-mode"></a>composants WebPart page de démonstration en mode édition

![Capture d’écran composants WebPart VS 3](profiles-themes-and-web-parts/_static/image5.gif)

**Figure 5**: capture d’écran composants WebPart vs 3

1. Cliquez sur le menu **mode d’affichage** , puis sélectionnez **Parcourir** pour revenir au mode de navigation.
2. Le contrôle a maintenant un titre mis à jour et aucune bordure, comme illustré dans la capture d’écran suivante.

### <a name="edited-web-parts-demo-page"></a>Page de démonstration composants WebPart modifiée

![Capture d’écran composants WebPart VS 4](profiles-themes-and-web-parts/_static/image6.gif)

**Figure 4**: capture d’écran composants WebPart vs 4

### <a name="adding-web-parts-at-run-time"></a>Ajout de composants WebPart au moment de l’exécution

Vous pouvez également permettre aux utilisateurs d’ajouter des contrôles de composants WebPart à leur page au moment de l’exécution. Pour ce faire, configurez la page avec un catalogue composants WebPart, qui contient une liste de composants WebPart contrôles que vous souhaitez mettre à la disposition des utilisateurs.

**Pour permettre aux utilisateurs d’ajouter des composants WebPart au moment de l’exécution**

1. Ouvrez la page WebPartsDemo. aspx et basculez en mode **Design** .
2. Dans l’onglet **WebPart** de la boîte à outils, faites glisser un contrôle CatalogZone dans la colonne de droite du tableau, sous le contrôle **EditorZone** .   
  
   Les deux contrôles peuvent se trouver dans la même cellule de table, car ils ne s’affichent pas en même temps.
3. Dans le volet Propriétés, affectez la chaîne **Add composants WebPart** à la propriété HeaderText du contrôle **CatalogZone** .
4. À partir de la section **WebParts** de la boîte à outils, faites glisser un contrôle DeclarativeCatalogPart dans la zone de contenu du contrôle **CatalogZone** .
5. Cliquez sur la flèche dans le coin supérieur droit du contrôle **DeclarativeCatalogPart** pour afficher le menu tâches, puis sélectionnez **modifier les modèles**.
6. À partir de la section **standard** de la boîte à outils, faites glisser un contrôle **FileUpload** et un contrôle **Calendar** dans la section **WebPartsTemplate** du contrôle **DeclarativeCatalogPart** .
7. Basculez en mode **source** . Examinez le code source de l’élément &lt;asp : CatalogZone&gt;. Notez que le contrôle **DeclarativeCatalogPart** contient un élément &lt;WebPartsTemplate&gt; avec les deux contrôles serveur joints que vous pourrez ajouter à votre page à partir du catalogue.
8. Ajoutez une propriété **title** à chacun des contrôles que vous avez ajoutés au catalogue, à l’aide de la valeur de chaîne indiquée pour chaque titre dans l’exemple de code ci-dessous. Même si le titre n’est pas une propriété que vous pouvez normalement définir sur ces deux contrôles serveur au moment du design, quand un utilisateur ajoute ces contrôles à une zone **WebPartZone** à partir du catalogue au moment de l’exécution, ils sont tous encapsulés avec un contrôle **GenericWebPart** . Cela leur permet d’agir en tant que composants WebPart des contrôles, afin qu’ils puissent afficher les titres.   
  
   Le code des deux contrôles contenus dans le contrôle **DeclarativeCatalogPart** doit se présenter comme suit. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Enregistrez la page.

Vous pouvez maintenant tester le catalogue.

### <a name="to-test-the-web-parts-catalog"></a>Pour tester le catalogue de composants WebPart

1. Chargez la page dans un navigateur.
2. Cliquez sur le menu déroulant **mode d’affichage** , puis sélectionnez **catalogue**.   
  
   Le catalogue intitulé **Add composants WebPart** s’affiche.
3. Faites glisser le contrôle **mes favoris** de la zone principale vers le haut de la zone de barre latérale, puis déposez-le à cet endroit.
4. Dans le catalogue **Ajouter des composants WebPart** , activez les deux cases à cocher, puis sélectionnez **principal** dans la liste déroulante qui contient les zones disponibles.
5. Cliquez sur **Ajouter** dans le catalogue. Les contrôles sont ajoutés à la zone principale. Si vous le souhaitez, vous pouvez ajouter plusieurs instances de contrôles du catalogue à votre page.   
  
   La capture d’écran suivante montre la page avec le contrôle de téléchargement de fichiers et le calendrier dans la zone principale. 

![Contrôles ajoutés à la zone principale à partir du catalogue](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Cliquez sur le menu déroulant **mode d’affichage** , puis sélectionnez **Parcourir**. Le catalogue disparaît et la page est actualisée.
7. Fermez le navigateur. Rechargez la page. Les modifications que vous avez apportées sont conservées.
