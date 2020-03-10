---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Contrôles de source de données | Microsoft Docs
author: microsoft
description: Le contrôle DataGrid dans ASP.NET 1. x a marqué une amélioration considérable dans l’accès aux données dans les applications Web. Toutefois, il n’était pas aussi convivial que possible....
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: a2e2cfbec3e5aebf42a2de30bab7d45b4b610298
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639411"
---
# <a name="data-source-controls"></a>Contrôles de source de données

par [Microsoft](https://github.com/microsoft)

> Le contrôle DataGrid dans ASP.NET 1. x a marqué une amélioration considérable dans l’accès aux données dans les applications Web. Toutefois, ce n’était pas aussi convivial que possible. Il nécessitait toujours une quantité considérable de code pour obtenir des fonctionnalités beaucoup plus utiles. C’est le modèle de tous les efforts d’accès aux données dans 1. x.

Le contrôle DataGrid dans ASP.NET 1. x a marqué une amélioration considérable dans l’accès aux données dans les applications Web. Toutefois, ce n’était pas aussi convivial que possible. Il nécessitait toujours une quantité considérable de code pour obtenir des fonctionnalités beaucoup plus utiles. C’est le modèle de tous les efforts d’accès aux données dans 1. x.

ASP.NET 2,0 traite cela en partie avec les contrôles de source de données. Les contrôles de source de données dans ASP.NET 2,0 fournissent aux développeurs un modèle déclaratif pour la récupération des données, l’affichage des données et la modification des données. L’objectif des contrôles de source de données est de fournir une représentation cohérente des données aux contrôles liés aux données, quelle que soit la source de ces données. Au cœur des contrôles de source de données dans ASP.NET 2,0 se trouve la classe abstraite DataSourceControl. La classe DataSourceControl fournit une implémentation de base de l’interface IDataSource et de l’interface IListSource, ce dernier vous permet d’assigner le contrôle de source de données comme source de données d’un contrôle lié aux données (via la nouvelle propriété DataSourceId abordé plus tard) et exposez les données en tant que liste. Chaque liste de données d’un contrôle de source de données est exposée sous la forme d’un objet DataSourceView. L’accès aux instances de DataSourceView est assuré par l’interface IDataSource. Par exemple, la méthode GetViewNames retourne une ICollection qui vous permet d’énumérer les DataSourceViews associées à un contrôle de source de données particulier, et la méthode GetView vous permet d’accéder à une instance DataSourceView particulière par nom.

Les contrôles de source de données n’ont pas d’interface utilisateur. Elles sont implémentées en tant que contrôles serveur afin qu’elles puissent prendre en charge la syntaxe déclarative et pour qu’elles aient accès à l’état de la page, le cas échéant. Les contrôles de source de données n’affichent pas de balisage HTML sur le client.

> [!NOTE]
> Comme vous le verrez plus tard, il existe également des avantages en matière de mise en cache obtenus à l’aide de contrôles de source de données.

## <a name="storing-connection-strings"></a>Stockage des chaînes de connexion

Avant de voir comment configurer les contrôles de source de données, nous devons aborder une nouvelle fonctionnalité de ASP.NET 2,0 concernant les chaînes de connexion. ASP.NET 2,0 introduit une nouvelle section dans le fichier de configuration qui vous permet de stocker facilement des chaînes de connexion qui peuvent être lues dynamiquement au moment de l’exécution. La section &lt;connectionStrings&gt; facilite le stockage des chaînes de connexion.

L’extrait de code ci-dessous ajoute une nouvelle chaîne de connexion.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Tout comme avec la section&gt; &lt;appSettings, la section &lt;connectionStrings&gt; apparaît en dehors de la section &lt;System. Web&gt; dans le fichier de configuration.

Pour utiliser cette chaîne de connexion, vous pouvez utiliser la syntaxe suivante lors de la définition de l’attribut ConnectionString d’un contrôle serveur.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

La section &lt;connectionStrings&gt; peut également être chiffrée afin que les informations sensibles ne soient pas exposées. Cette capacité sera traitée dans un module ultérieur.

## <a name="caching-data-sources"></a>Mise en cache des sources de données

Chaque DataSourceControl fournit quatre propriétés pour la configuration de la mise en cache. EnableCaching, CacheDuration, CacheExpirationPolicy et CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching est une propriété booléenne qui détermine si la mise en cache est activée ou non pour le contrôle de source de données.

## <a name="cacheduration-property"></a>Propriété CacheDuration

La propriété CacheDuration définit le nombre de secondes pendant lesquelles le cache reste valide. Si cette propriété a la valeur **0** , le cache reste valide jusqu’à ce qu’il soit explicitement invalidé.

## <a name="cacheexpirationpolicy-property"></a>Propriété CacheExpirationPolicy

La propriété CacheExpirationPolicy peut avoir la valeur **Absolute** ou **glissante**. Si vous affectez la valeur Absolute, la durée maximale de mise en cache des données est le nombre de secondes spécifié par la propriété CacheDuration. En l’affectant à la valeur coulissante, le délai d’expiration est réinitialisé lorsque chaque opération est effectuée.

## <a name="cachekeydependency-property"></a>Propriété CacheKeyDependency

Si une valeur de chaîne est spécifiée pour la propriété CacheKeyDependency, ASP.NET configure une nouvelle dépendance de cache en fonction de cette chaîne. Cela vous permet d’invalider explicitement le cache en modifiant ou en supprimant simplement le CacheKeyDependency.

**Important**: si l’emprunt d’identité est activé et que l’accès à la source de données et/ou au contenu de données est basé sur l’identité du client, il est recommandé de désactiver la mise en cache en affectant à EnableCaching la valeur false. Si la mise en cache est activée dans ce scénario et qu’un utilisateur autre que l’utilisateur qui a demandé à l’origine les données émet une demande, l’autorisation à la source de données n’est pas appliquée. Les données seront simplement servies à partir du cache.

## <a name="the-sqldatasource-control"></a>Le contrôle SqlDataSource

Le contrôle SqlDataSource permet à un développeur d’accéder aux données stockées dans toute base de données relationnelle qui prend en charge ADO.NET. Il peut utiliser le fournisseur System. Data. SqlClient pour accéder à une base de données SQL Server, au fournisseur System. Data. OleDb, au fournisseur System. Data. ODBC ou au fournisseur System. Data. OracleClient pour accéder à Oracle. Par conséquent, le SqlDataSource est certainement non seulement utilisé pour accéder aux données d’une base de données SQL Server.

Pour pouvoir utiliser le SqlDataSource, il vous suffit de fournir une valeur pour la propriété ConnectionString et de spécifier une commande SQL ou une procédure stockée. Le contrôle SqlDataSource s’occupe de l’utilisation de l’architecture ADO.NET sous-jacente. Il ouvre la connexion, interroge la source de données ou exécute la procédure stockée, retourne les données, puis ferme la connexion pour vous.

> [!NOTE]
> Étant donné que la classe DataSourceControl ferme automatiquement la connexion pour vous, elle doit réduire le nombre d’appels client générés par la fuite des connexions de base de données.

L’extrait de code ci-dessous lie un contrôle DropDownList à un contrôle SqlDataSource à l’aide de la chaîne de connexion stockée dans le fichier de configuration, comme indiqué ci-dessus.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Comme illustré ci-dessus, la propriété DataSourceMode du SqlDataSource spécifie le mode de la source de données. Dans l’exemple ci-dessus, DataSourceMode a la valeur DataReader. Dans ce cas, le SqlDataSource retourne un objet IDataReader à l’aide d’un curseur avant uniquement et en lecture seule. Le type d’objet spécifié qui est retourné est contrôlé par le fournisseur utilisé. Dans ce cas, j’utilise le fournisseur System. Data. SqlClient comme spécifié dans la section &lt;connectionStrings&gt; du fichier Web. config. Par conséquent, l’objet retourné sera de type SqlDataReader. En spécifiant une valeur DataSourceMode de DataSet, les données peuvent être stockées dans un jeu de données sur le serveur. Ce mode vous permet d’ajouter des fonctionnalités telles que le tri, la pagination, etc. Si j’avais lié aux données le SqlDataSource à un contrôle GridView, j’aurais choisi le mode DataSet. Toutefois, dans le cas d’un DropDownList, le mode DataReader est le bon choix.

> [!NOTE]
> Lors de la mise en cache d’un SqlDataSource ou d’un AccessDataSource, la propriété DataSourceMode doit être définie sur DataSet. Une exception se produit si vous activez la mise en cache avec un DataSourceMode de DataReader.

## <a name="sqldatasource-properties"></a>Propriétés SqlDataSource

Voici quelques-unes des propriétés du contrôle SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Valeur booléenne qui spécifie si une commande SELECT est annulée si l’un des paramètres a la valeur null. La valeur par défaut est True.

### <a name="conflictdetection"></a>ConflictDetection

Dans le cas où plusieurs utilisateurs peuvent mettre à jour une source de données en même temps, la propriété ConflictDetection détermine le comportement du contrôle SqlDataSource. Cette propriété correspond à l’une des valeurs de l’énumération ConflictOptions. Ces valeurs sont **CompareAllValues** et **OverwriteChanges**. Si la valeur est OverwriteChanges, la dernière personne à écrire des données dans la source de données remplace toutes les modifications précédentes. Toutefois, si la propriété ConflictDetection est définie sur CompareAllValues, les paramètres sont créés pour les colonnes retournées par SelectCommand et les paramètres sont également créés pour contenir les valeurs d’origine dans chacune de ces colonnes, ce qui permet au SqlDataSource de Déterminez si les valeurs ont changé depuis l’exécution de SelectCommand.

### <a name="deletecommand"></a>DeleteCommand

Définit ou obtient la chaîne SQL utilisée lors de la suppression de lignes de la base de données. Il peut s’agir d’une requête SQL ou d’un nom de procédure stockée.

### <a name="deletecommandtype"></a>DeleteCommandType

Définit ou obtient le type de commande Delete, une requête SQL (texte) ou une procédure stockée (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Retourne les paramètres utilisés par le DeleteCommand de l’objet SqlDataSourceView associé au contrôle SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Cette propriété est utilisée pour spécifier le format des paramètres de valeur d’origine dans les cas où la propriété ConflictDetection est définie sur CompareAllValues. La valeur par défaut est {0}, ce qui signifie que les paramètres de valeur d’origine prennent le même nom que le paramètre d’origine. En d’autres termes, si le nom de champ est EmployeeID, le paramètre de valeur d’origine est @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Définit ou obtient la chaîne SQL utilisée pour récupérer des données de la base de données. Il peut s’agir d’une requête SQL ou d’un nom de procédure stockée.

### <a name="selectcommandtype"></a>SelectCommandType

Définit ou obtient le type de commande SELECT, à savoir une requête SQL (texte) ou une procédure stockée (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Retourne les paramètres utilisés par le SelectCommand de l’objet SqlDataSourceView associé au contrôle SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Obtient ou définit le nom d’un paramètre de procédure stockée qui est utilisé lors du tri des données récupérées par le contrôle de source de données. Valide uniquement lorsque SelectCommandType a la valeur StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Chaîne délimitée par des points-virgules spécifiant les bases de données et les tables utilisées dans une dépendance de cache SQL Server. (Les dépendances de cache SQL seront abordées dans un module ultérieur.)

### <a name="updatecommand"></a>UpdateCommand

Définit ou obtient la chaîne SQL utilisée lors de la mise à jour des données de la base de données. Il peut s’agir d’une requête SQL ou d’un nom de procédure stockée.

### <a name="updatecommandtype"></a>UpdateCommandType

Définit ou obtient le type de commande de mise à jour, à savoir une requête SQL (texte) ou une procédure stockée (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Retourne les paramètres utilisés par le UpdateCommand de l’objet SqlDataSourceView associé au contrôle SqlDataSource.

## <a name="the-accessdatasource-control"></a>Contrôle AccessDataSource

Le contrôle AccessDataSource dérive de la classe SqlDataSource et est utilisé pour la liaison de données à une base de données Microsoft Access. La propriété ConnectionString pour le contrôle AccessDataSource est une propriété en lecture seule. Au lieu d’utiliser la propriété ConnectionString, la propriété DataFile est utilisée pour pointer vers la base de données Access, comme indiqué ci-dessous.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource définit toujours le ProviderName du SqlDataSource de base sur System. Data. OleDb et se connecte à la base de données à l’aide du fournisseur de OLE DB Microsoft. jet. OLEDB. 4.0. Vous ne pouvez pas utiliser le contrôle AccessDataSource pour vous connecter à une base de données Access protégée par mot de passe. Si vous devez vous connecter à une base de données protégée par mot de passe, vous devez utiliser le contrôle SqlDataSource.

> [!NOTE]
> Les bases de données Access stockées dans le site Web doivent être placées dans le répertoire de données de l’application\_. ASP.NET n’autorise pas l’exploration des fichiers de ce répertoire. Vous devez accorder au compte de processus des autorisations en lecture et en écriture sur le répertoire de données de l’application\_lors de l’utilisation de bases de données Access.

## <a name="the-xmldatasource-control"></a>Contrôle XmlDataSource

XmlDataSource est utilisé pour lier des données XML à des contrôles liés aux données. Vous pouvez effectuer une liaison à un fichier XML à l’aide de la propriété DataFile ou vous pouvez effectuer une liaison à une chaîne XML à l’aide de la propriété Data. XmlDataSource expose des attributs XML en tant que champs pouvant être liés. Dans les cas où vous devez effectuer une liaison à des valeurs qui ne sont pas représentées en tant qu’attributs, vous devrez utiliser une transformation XSL. Vous pouvez également utiliser des expressions XPath pour filtrer des données XML.

Prenons le fichier XML suivant :

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Notez que le XmlDataSource utilise une propriété XPath de *personnes/* personnes afin de filtrer uniquement les nœuds &lt;personne&gt;. Le DropDownList est ensuite lié aux données de l’attribut LastName à l’aide de la propriété DataTextField.

Bien que le contrôle XmlDataSource soit principalement utilisé pour la liaison de données à des données XML en lecture seule, il est possible de modifier le fichier de données XML. Notez que dans de tels cas, l’insertion automatique, la mise à jour et la suppression d’informations dans le fichier XML ne se produisent pas automatiquement, comme c’est le cas avec les autres contrôles de source de données. Au lieu de cela, vous devrez écrire du code pour modifier manuellement les données à l’aide des méthodes suivantes du contrôle XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument

Récupère un objet XmlDocument contenant le code XML récupéré par le XmlDataSource.

### <a name="save"></a>Enregistrer

Enregistre le XmlDocument en mémoire dans la source de données.

Il est important de comprendre que la méthode Save ne fonctionnera que si les deux conditions suivantes sont remplies :

1. XmlDataSource utilise la propriété DataFile pour effectuer une liaison à un fichier XML au lieu de la propriété Data pour effectuer une liaison à des données XML en mémoire.
2. Aucune transformation n’est spécifiée via la propriété Transform ou TransformFile.

Notez également que la méthode Save peut produire des résultats inattendus lorsqu’elle est appelée par plusieurs utilisateurs simultanément.

## <a name="the-objectdatasource-control"></a>Contrôle ObjectDataSource

Les contrôles de source de données que nous avons abordés jusqu’à présent sont des choix excellents pour les applications à deux niveaux où le contrôle de source de données communique directement avec le magasin de données. Toutefois, de nombreuses applications réelles sont des applications multicouches où un contrôle de source de données peut avoir besoin de communiquer avec un objet métier qui, à son tour, communique avec la couche de données. Dans ce cas, l’ObjectDataSource remplit la facture. ObjectDataSource fonctionne conjointement avec un objet source. Le contrôle ObjectDataSource crée une instance de l’objet source, appelle la méthode spécifiée et supprime l’instance de l’objet dans la portée d’une requête unique, si votre objet a des méthodes d’instance au lieu de méthodes statiques (partagées dans Visual Basic). Par conséquent, votre objet doit être sans État. Autrement dit, votre objet doit acquérir et libérer toutes les ressources requises au sein de l’étendue d’une requête unique. Vous pouvez contrôler la façon dont l’objet source est créé en gérant l’événement ObjectCreating du contrôle ObjectDataSource. Vous pouvez créer une instance de l’objet source, puis définir la propriété ObjectInstance de la classe ObjectDataSourceEventArgs sur cette instance. Le contrôle ObjectDataSource utilise l’instance créée dans l’événement ObjectCreating au lieu de créer une instance seule.

Si l’objet source d’un contrôle ObjectDataSource expose des méthodes statiques publiques (partagées dans Visual Basic) qui peuvent être appelées pour récupérer et modifier des données, un contrôle ObjectDataSource appellera ces méthodes directement. Si un contrôle ObjectDataSource doit créer une instance de l’objet source afin d’effectuer des appels de méthode, l’objet doit inclure un constructeur public qui ne prend aucun paramètre. Le contrôle ObjectDataSource appellera ce constructeur lorsqu’il crée une nouvelle instance de l’objet source.

Si l’objet source ne contient pas de constructeur public sans paramètres, vous pouvez créer une instance de l’objet source qui sera utilisée par le contrôle ObjectDataSource dans l’événement ObjectCreating.

## <a name="specifying-object-methods"></a>Spécification des méthodes d’objet

L’objet source d’un contrôle ObjectDataSource peut contenir un nombre quelconque de méthodes utilisées pour sélectionner, insérer, mettre à jour ou supprimer des données. Ces méthodes sont appelées par le contrôle ObjectDataSource en fonction du nom de la méthode, tel qu’identifié à l’aide de la propriété SelectMethod, InsertMethod, UpdateMethod ou DeleteMethod du contrôle ObjectDataSource. L’objet source peut également inclure une méthode SelectCoun facultative, qui est identifiée par le contrôle ObjectDataSource à l’aide de la propriété SelectCountMethod, qui retourne le nombre total d’objets au niveau de la source de données. Le contrôle ObjectDataSource appellera la méthode SelectCoun après l’appel d’une méthode Select pour récupérer le nombre total d’enregistrements dans la source de données à utiliser lors de la pagination.

## <a name="lab-using-data-source-controls"></a>Lab utilisant des contrôles de source de données

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Exercice 1 : affichage des données avec le contrôle SqlDataSource

L’exercice suivant utilise le contrôle SqlDataSource pour se connecter à la base de données Northwind. Il part du principe que vous avez accès à la base de données Northwind sur une instance SQL Server 2000.

1. Créez un site Web ASP.NET.
2. Ajoutez un nouveau fichier Web. config.

    1. Cliquez avec le bouton droit sur le projet dans Explorateur de solutions, puis cliquez sur Ajouter un nouvel élément.
    2. Choisissez fichier de configuration Web dans la liste des modèles, puis cliquez sur Ajouter.
3. Modifiez la section &lt;connectionStrings&gt; comme suit : 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Basculez en mode Code et ajoutez un attribut ConnectionString et un attribut SelectCommand au contrôle &lt;asp : SqlDataSource&gt; comme suit : 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. À partir de Mode Création, ajoutez un nouveau contrôle GridView.
6. Dans la liste déroulante choisir la source de données du menu Tâches GridView, choisissez SqlDataSource1.
7. Cliquez avec le bouton droit sur default. aspx, puis choisissez afficher dans le navigateur dans le menu. Cliquez sur Oui lorsque vous êtes invité à enregistrer.
8. Le contrôle GridView affiche les données de la table Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Exercice 2-modification des données avec le contrôle SqlDataSource

L’exercice suivant montre comment lier des données à un contrôle DropDownList à l’aide de la syntaxe déclarative et vous permet de modifier les données présentées dans le contrôle DropDownList.

1. Dans Mode Création, supprimez le contrôle GridView de default. aspx. 

    **Important**: laissez le contrôle SqlDataSource sur la page.
2. Ajoutez un contrôle DropDownList à default. aspx.
3. Basculez en mode Source.
4. Ajoutez un attribut DataSourceId, DataTextField et DataValueField au contrôle &lt;asp : DropDownList&gt; comme suit : 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Enregistrez default. aspx et affichez-le dans le navigateur. Notez que le DropDownList contient tous les produits de la base de données Northwind.
6. Fermez le navigateur.
7. En mode source de default. aspx, ajoutez un nouveau contrôle TextBox sous le contrôle DropDownList. Remplacez la valeur de la propriété ID de la zone de texte par txtProductName.
8. Sous le contrôle TextBox, ajoutez un nouveau contrôle Button. Remplacez la valeur de la propriété ID du bouton par btnUpdate et la propriété Text par **Update Product Name**.
9. En mode source de default. aspx, ajoutez une propriété UpdateCommand et deux nouvelles UpdateParameters à la balise SqlDataSource comme suit : 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Notez qu’il existe deux paramètres de mise à jour (ProductName et ProductID) ajoutés dans ce code. Ces paramètres sont mappés à la propriété Text de la zone de texte txtProductName et à la propriété SelectedValue de la DropDownList ddlProducts.
10. Basculez vers Mode Création et double-cliquez sur le contrôle Button pour ajouter un gestionnaire d’événements.
11. Ajoutez le code suivant à btnUpdate\_cliquez sur code : 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Cliquez avec le bouton droit sur default. aspx et choisissez de l’afficher dans le navigateur. Cliquez sur Oui lorsque vous êtes invité à enregistrer toutes les modifications.
13. Les classes partielles ASP.NET 2,0 autorisent la compilation au moment de l’exécution. Il n’est pas nécessaire de générer une application pour que les modifications du code prennent effet.
14. Sélectionnez un produit dans le DropDownList.
15. Entrez un nouveau nom pour le produit sélectionné dans la zone de texte, puis cliquez sur le bouton mettre à jour.
16. Le nom du produit est mis à jour dans la base de données.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Exercice 3 utilisation du contrôle ObjectDataSource

Cet exercice vous montre comment utiliser le contrôle ObjectDataSource et un objet source pour interagir avec la base de données Northwind.

1. Cliquez avec le bouton droit sur le projet dans Explorateur de solutions, puis cliquez sur Ajouter un nouvel élément.
2. Sélectionnez Web Form dans la liste des modèles. Remplacez le nom par Object. aspx, puis cliquez sur Ajouter.
3. Cliquez avec le bouton droit sur le projet dans Explorateur de solutions, puis cliquez sur Ajouter un nouvel élément.
4. Sélectionnez classe dans la liste modèles. Remplacez le nom de la classe par NorthwindData.cs et cliquez sur Ajouter.
5. Cliquez sur Oui lorsque vous êtes invité à ajouter la classe au dossier App\_code.
6. Ajoutez le code suivant au fichier NorthwindData.cs : 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Ajoutez le code suivant à la vue source de Object. aspx : 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Enregistrez tous les fichiers et parcourez objet. aspx.
9. Interagissez avec l’interface en affichant les détails, en modifiant les employés, en ajoutant des employés et en supprimant des employés.
