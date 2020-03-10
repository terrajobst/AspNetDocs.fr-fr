---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Sérialisation JSON et XML dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Décrit les formateurs JSON et XML dans API Web ASP.NET pour ASP.NET 4. x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 00fa07f00eabf7e6c883c5e9ceaf9a38a8f49605
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557469"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a>Sérialisation JSON et XML dans API Web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

Cet article décrit les formateurs JSON et XML dans API Web ASP.NET.

Dans API Web ASP.NET, un *formateur de type de média* est un objet qui peut :

- Lire des objets CLR à partir d’un corps de message HTTP
- Écrire des objets CLR dans le corps d’un message HTTP

L’API Web fournit des formateurs de type de média pour JSON et XML. L’infrastructure insère ces formateurs dans le pipeline par défaut. Les clients peuvent demander JSON ou XML dans l’en-tête Accept de la requête HTTP.

## <a name="contents"></a>Contenu

- [Formateur de type de média JSON](#json_media_type_formatter)

    - [Propriétés en lecture seule](#json_readonly)
    - [Indiquée](#json_dates)
    - [Mise en retrait](#json_indenting)
    - [Casse mixte](#json_camelcasing)
    - [Objets anonymes et faiblement typés](#json_anon)
- [Formateur de type de média XML](#xml_media_type_formatter)

    - [Propriétés en lecture seule](#xml_readonly)
    - [Indiquée](#xml_dates)
    - [Mise en retrait](#xml_indenting)
    - [Définition de sérialiseurs XML par type](#xml_pertype)
- [Suppression du formateur JSON ou XML](#removing_the_json_or_xml_formatter)
- [Gestion des références d’objets circulaires](#handling_circular_object_references)
- [Test de la sérialisation d’un objet](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formateur de type de média JSON

La mise en forme JSON est fournie par la classe **JsonMediaTypeFormatter** . Par défaut, **JsonMediaTypeFormatter** utilise la bibliothèque [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) pour effectuer la sérialisation. Json.NET est un projet open source tiers.

Si vous préférez, vous pouvez configurer la classe **JsonMediaTypeFormatter** pour utiliser **DataContractJsonSerializer** au lieu de JSON.net. Pour ce faire, affectez la valeur **true**à la propriété **UseDataContractJsonSerializer** :

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Sérialisation JSON

Cette section décrit certains comportements spécifiques du formateur JSON, à l’aide du sérialiseur [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) par défaut. Il ne s’agit pas d’une documentation complète de la bibliothèque Json.NET. Pour plus d’informations, consultez la [Documentation JSON.net](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Que est-ce qui est sérialisé ?

Par défaut, tous les champs et propriétés publics sont inclus dans le JSON sérialisé. Pour omettre une propriété ou un champ, décorez-le avec l’attribut **JsonIgnore** .

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Si vous préférez une approche de&quot; d’abonnement &quot;, Décorez la classe avec l’attribut **DataContract** . Si cet attribut est présent, les membres sont ignorés, sauf s’ils ont le **DataMember**. Vous pouvez également utiliser **DataMember** pour sérialiser des membres privés.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Propriétés en lecture seule

Les propriétés en lecture seule sont sérialisées par défaut.

<a id="json_dates"></a>
### <a name="dates"></a>Dates

Par défaut, Json.NET écrit les dates au format [ISO 8601](http://www.w3.org/TR/NOTE-datetime) . Les dates au format UTC (temps universel coordonné) sont écrites avec un suffixe « Z ». Les dates en heure locale incluent un décalage de fuseau horaire. Exemple :

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Par défaut, Json.NET conserve le fuseau horaire. Vous pouvez le remplacer en définissant la propriété DateTimeZoneHandling :

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Si vous préférez utiliser le [format de date Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) au lieu de la norme ISO 8601, définissez la propriété **DateFormatHandling** sur les paramètres du sérialiseur :

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Mise en retrait

Pour écrire du code JSON mis en retrait, définissez le paramètre de **mise** en forme sur **mise en forme. en retrait**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Casse mixte

Pour écrire des noms de propriété JSON avec la casse mixte, sans modifier votre modèle de données, définissez **CamelCasePropertyNamesContractResolver** sur le sérialiseur :

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Objets anonymes et faiblement typés

Une méthode d’action peut retourner un objet anonyme et la sérialiser au format JSON. Exemple :

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Le corps du message de réponse contient le code JSON suivant :

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Si votre API Web reçoit des objets JSON faiblement structurés des clients, vous pouvez désérialiser le corps de la requête en un type **Newtonsoft. JSON. Linq. JObject** .

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Toutefois, il est généralement préférable d’utiliser des objets de données fortement typés. Vous n’avez pas besoin d’analyser les données vous-même et vous bénéficiez des avantages de la validation du modèle.

Le sérialiseur XML ne prend pas en charge les types anonymes ou les instances **JObject** . Si vous utilisez ces fonctionnalités pour vos données JSON, vous devez supprimer le formateur XML du pipeline, comme décrit plus loin dans cet article.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formateur de type de média XML

La mise en forme XML est fournie par la classe **XmlMediaTypeFormatter** . Par défaut, **XmlMediaTypeFormatter** utilise la classe **DataContractSerializer** pour effectuer la sérialisation.

Si vous préférez, vous pouvez configurer **XmlMediaTypeFormatter** pour utiliser le **XmlSerializer** au lieu de **DataContractSerializer**. Pour ce faire, affectez la valeur **true**à la propriété **UseXmlSerializer** :

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

La classe **XmlSerializer** prend en charge un ensemble plus étroit de types que **DataContractSerializer**, mais offre davantage de contrôle sur le XML résultant. Pensez à utiliser **XmlSerializer** si vous devez faire correspondre un schéma XML existant.

### <a name="xml-serialization"></a>Sérialisation XML

Cette section décrit certains comportements spécifiques du formateur XML, à l’aide de **DataContractSerializer**par défaut.

Par défaut, DataContractSerializer se comporte comme suit :

- Toutes les propriétés et tous les champs publics en lecture/écriture sont sérialisés. Pour omettre une propriété ou un champ, décorez-le avec l’attribut **IgnoreDataMember** .
- Les membres privés et protégés ne sont pas sérialisés.
- Les propriétés en lecture seule ne sont pas sérialisées. (Toutefois, le contenu d’une propriété de collection en lecture seule est sérialisé.)
- Les noms de classes et de membres sont écrits dans le XML exactement tels qu’ils apparaissent dans la déclaration de classe.
- Un espace de noms XML par défaut est utilisé.

Si vous avez besoin de davantage de contrôle sur la sérialisation, vous pouvez décorer la classe avec l’attribut **DataContract** . Lorsque cet attribut est présent, la classe est sérialisée comme suit :

- &quot;adopter&quot; approche : les propriétés et les champs ne sont pas sérialisés par défaut. Pour sérialiser une propriété ou un champ, décorez-le avec l’attribut **DataMember** .
- Pour sérialiser un membre privé ou protégé, décorez-le avec l’attribut **DataMember** .
- Les propriétés en lecture seule ne sont pas sérialisées.
- Pour modifier le mode d’affichage du nom de la classe dans le code XML, définissez le paramètre *Name* dans l’attribut **DataContract** .
- Pour modifier le mode d’affichage d’un nom de membre dans le XML, définissez le paramètre *Name* dans l’attribut **DataMember** .
- Pour modifier l’espace de noms XML, définissez le paramètre d' *espace de noms* dans la classe **DataContract** .

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Propriétés en lecture seule

Les propriétés en lecture seule ne sont pas sérialisées. Si une propriété en lecture seule contient un champ privé de sauvegarde, vous pouvez marquer le champ privé avec l’attribut **DataMember** . Cette approche nécessite l’attribut **DataContract** sur la classe.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Dates

Les dates sont écrites au format ISO 8601. Par exemple, &quot;2012-05-23T20:21:37.9116538 Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Mise en retrait

Pour écrire du code XML mis en retrait, affectez la valeur **true**à la propriété **indent** :

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Définition de sérialiseurs XML par type

Vous pouvez définir différents sérialiseurs XML pour différents types CLR. Par exemple, vous pouvez avoir un objet de données particulier qui requiert **XmlSerializer** pour des raisons de compatibilité descendante. Vous pouvez utiliser **XmlSerializer** pour cet objet et continuer à utiliser **DataContractSerializer** pour d’autres types.

Pour définir un sérialiseur XML pour un type particulier, appelez **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Vous pouvez spécifier un **XmlSerializer** ou tout objet qui dérive de **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Suppression du formateur JSON ou XML

Vous pouvez supprimer le module de formatage JSON ou XML de la liste des formateurs, si vous ne souhaitez pas les utiliser. Les principales raisons sont les suivantes :

- Pour limiter les réponses de votre API Web à un type de média particulier. Par exemple, vous pouvez décider de prendre en charge uniquement les réponses JSON et supprimer le formateur XML.
- Pour remplacer le formateur par défaut par un formateur personnalisé. Par exemple, vous pouvez remplacer le formateur JSON par votre propre implémentation personnalisée d’un formateur JSON.

Le code suivant montre comment supprimer les formateurs par défaut. Appelez-le à partir de votre **Application\_méthode Start** , définie dans global. asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Gestion des références d’objets circulaires

Par défaut, les formateurs JSON et XML écrivent tous les objets en tant que valeurs. Si deux propriétés font référence au même objet, ou si le même objet apparaît deux fois dans une collection, le formateur sérialise l’objet deux fois. Il s’agit d’un problème particulier si votre graphique d’objets contient des cycles, car le sérialiseur lèvera une exception lorsqu’il détecte une boucle dans le graphique.

Tenez compte des modèles objet et du contrôleur suivants.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

L’appel de cette action entraîne la levée d’une exception par le formateur, qui se traduit par une réponse de code d’état 500 (erreur de serveur interne) au client.

Pour conserver les références d’objet dans JSON, ajoutez le code suivant à l' **Application\_méthode Start** dans le fichier global. asax :

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

À présent, l’action du contrôleur renverra un JSON ressemblant à ceci :

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Notez que le sérialiseur ajoute une &quot;$id&quot; propriété aux deux objets. En outre, il détecte que la propriété Employee. Department crée une boucle, de sorte qu’elle remplace la valeur par une référence d’objet : {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Les références d’objet ne sont pas standard dans JSON. Avant d’utiliser cette fonctionnalité, déterminez si vos clients peuvent analyser les résultats. Il peut être préférable de supprimer simplement des cycles du graphique. Par exemple, le lien de retour de l’employé au service n’est pas vraiment nécessaire dans cet exemple.

Pour conserver les références d’objet dans XML, vous avez deux options. L’option la plus simple consiste à ajouter `[DataContract(IsReference=true)]` à votre classe de modèle. Le paramètre *IsReference* active les références d’objet. Rappelez-vous que **DataContract** fait un abonnement de sérialisation. vous devrez donc également ajouter des attributs **DataMember** aux propriétés :

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

À présent, le formateur produira un XML similaire à ce qui suit :

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Si vous souhaitez éviter des attributs sur votre classe de modèle, il existe une autre option : créer une instance **DataContractSerializer** spécifique au type et affecter à *preserveObjectReferences* la **valeur true** dans le constructeur. Ensuite, définissez cette instance en tant que sérialiseur par type sur le formateur de type de média XML. Le code suivant montre comment procéder :

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Test de la sérialisation d’un objet

Au fur et à mesure que vous concevez votre API Web, il est utile de tester la manière dont vos objets de données seront sérialisés. Vous pouvez le faire sans créer de contrôleur ni appeler une action de contrôleur.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
