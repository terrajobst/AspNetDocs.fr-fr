---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Utilisation de Visual Studio 2013 pour créer une page de Web Forms de base ASP.NET 4,5
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 5d13a51128eecd92a82cfd06054448582a348e11
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445675"
---
# <a name="using-visual-studio-2013-to-create-a-basic-aspnet-45-web-forms-page"></a>Utilisation de Visual Studio 2013 pour créer une page de Web Forms de base ASP.NET 4,5

par [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[](~/includes/rp.md)]

Cette procédure pas à pas vous fournit une introduction à l’environnement de développement Web dans [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) et dans [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Cette procédure pas à pas vous guide tout au long de la création d’une page de Web Forms ASP.NET simple et illustre les techniques de base permettant de créer une page, d’ajouter des contrôles et d’écrire du code.

Cette procédure pas à pas décrit notamment les tâches suivantes :

- Création d’un système de fichiers Web Forms projet d’application.
- Familiarisez-vous avec Visual Studio.
- Création d’une page ASP.NET.
- Ajout de contrôles.
- Ajout de gestionnaires d’événements.
- Exécution et test d’une page à partir de Visual Studio.

## <a name="prerequisites"></a>Configuration requise

Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Le .NET Framework est installé automatiquement. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 et Microsoft Visual Studio Express 2013 pour le Web sont souvent appelés Visual Studio dans cette série de didacticiels.  
    >   
    > Si vous utilisez Visual Studio, cette procédure pas à pas suppose que vous avez sélectionné la collection de paramètres **développement Web** la première fois que vous avez démarré Visual Studio. Pour plus d’informations, consultez [Comment : sélectionner les paramètres de l’environnement de développement Web](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Création d’un projet d’application Web et d’une page

<a id="sectionToggle0"></a>

Dans cette partie de la procédure pas à pas, vous allez créer un projet d’application Web et lui ajouter une nouvelle page. Vous allez également ajouter du texte HTML et exécuter la page dans votre navigateur.

### <a name="to-create-a-web-application-project"></a>Pour créer un projet d’application Web

1. Ouvrez Microsoft Visual Studio.
2. Dans le menu **Fichier**, sélectionnez **Nouveau projet**.  
    ![menu fichier](creating-a-basic-web-forms-page/_static/image1.png)

    La boîte de dialogue **Nouveau projet** s’affiche.
3. Sélectionnez les **modèles** -&gt; le groupe **Visual C#**  -&gt; de modèles **Web** sur la gauche.
4. Choisissez le modèle **application Web ASP.net** dans la colonne centrale.
5. Nommez votre projet ***BasicWebApp*** et cliquez sur le bouton **OK** .   
![boîte de dialogue Nouveau projet](creating-a-basic-web-forms-page/_static/image2.png)
6. Ensuite, sélectionnez le modèle **Web Forms** , puis cliquez sur le bouton **OK** pour créer le projet.  
![boîte de dialogue Nouveau projet ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio crée un projet qui comprend des fonctionnalités prégénérées basées sur le modèle de Web Forms. Elle vous fournit non seulement une page main *. aspx* , une page *about. aspx* , une page *contact. aspx* , mais inclut également la fonctionnalité d’appartenance qui inscrit les utilisateurs et enregistre leurs informations d’identification afin qu’ils puissent se connecter à votre site Web. Quand une nouvelle page est créée, par défaut, Visual Studio affiche la page en mode **source** , où vous pouvez voir les éléments HTML de la page. L’illustration suivante montre ce que vous pouvez voir en mode **source** si vous avez créé une nouvelle page Web nommée *BasicWebApp. aspx*.  
    ![Mode Source](creating-a-basic-web-forms-page/_static/image4.png)

### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Visite guidée de l’environnement de développement Web de Visual Studio

Avant de continuer en modifiant la page, il est utile de vous familiariser avec l’environnement de développement Visual Studio. L’illustration suivante montre les fenêtres et les outils disponibles dans Visual Studio et Visual Studio Express pour le Web.

> [!NOTE] 
> 
> Ce diagramme affiche les emplacements des fenêtres et des fenêtres par défaut. Le menu **affichage** vous permet d’afficher des fenêtres supplémentaires et de réorganiser et redimensionner des fenêtres en fonction de vos préférences. Si des modifications ont déjà été apportées à la disposition de la fenêtre, ce que vous voyez ne correspondra pas à l’illustration.

 L’environnement Visual Studio

![Environnement Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Familiarisez-vous avec les Web designer

Examinez l’illustration ci-dessus et faites correspondre le texte à la liste suivante, qui décrit les fenêtres et les outils les plus couramment utilisés. (Toutes les fenêtres et tous les outils que vous voyez ne sont pas répertoriés ici, mais uniquement ceux qui sont marqués dans l’illustration précédente.)

- Barres d’outils. Fournissez des commandes pour mettre en forme le texte, Rechercher du texte, et ainsi de suite. Certaines barres d’outils sont disponibles uniquement lorsque vous travaillez en mode **conception** .
- **Explorateur de solutions** fenêtre. Affiche les fichiers et les dossiers de votre application Web.
- Fenêtre de document. Affiche les documents sur lesquels vous travaillez dans les fenêtres à onglets. Vous pouvez basculer entre les documents en cliquant sur les onglets.
- Fenêtre **Propriétés** . Vous permet de modifier les paramètres de la page, des éléments HTML, des contrôles et d’autres objets.
- Afficher les onglets. Présente différentes vues du même document. Le mode **Design** est une surface de modification proche de WYSIWYG. Le mode **source** est l’éditeur HTML de la page. Le mode **fractionné** affiche à la fois le mode **Design** et le mode **source** du document. Vous utiliserez les vues de la **conception** et de la **source** plus loin dans cette procédure pas à pas. Si vous préférez ouvrir des pages Web en mode **conception** , dans le menu **Outils** , cliquez sur **options**, sélectionnez le nœud **Concepteur HTML** , puis modifiez l’option démarrer les **pages dans** .
- **Boîte à outils**. Fournit des contrôles et des éléments HTML que vous pouvez faire glisser sur votre page. Les éléments de **boîte à outils** sont regroupés par fonction commune.
- **Explorateur**de serveurs. Affiche les connexions de base de données. Si Explorateur de serveurs n’est pas visible, dans le menu Affichage, cliquez sur Explorateur de serveurs.

### <a name="creating-a-new-aspnet-web-forms-page"></a>Création d’une nouvelle page de Web Forms ASP.NET

Quand vous créez une application de Web Forms à l’aide du modèle de projet d' **application Web ASP.net** , Visual Studio ajoute une page ASP.net (page Web Forms) nommée *default. aspx*, ainsi que plusieurs autres fichiers et dossiers. Vous pouvez utiliser la page *default. aspx* comme page d’hébergement de votre application Web. Toutefois, pour cette procédure pas à pas, vous allez créer et utiliser une nouvelle page.

### <a name="to-add-a-page-to-the-web-application"></a>Pour ajouter une page à l’application Web

1. Fermez la page *default. aspx* . Pour ce faire, cliquez sur l’onglet qui affiche le nom du fichier, puis cliquez sur l’option fermer.
2. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nom de l’application Web (dans ce didacticiel, le nom de l’application est **BasicWebSite**), puis cliquez sur **Ajouter** -&gt; **nouvel élément**.   
La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
3. Sélectionnez le **groupe C# Visual** -&gt; des modèles **Web** sur la gauche. Ensuite, sélectionnez **Web Form** dans la liste du milieu, puis nommez-le *FirstWebPage. aspx*.   
    ![boîte de dialogue Ajouter un nouvel élément](creating-a-basic-web-forms-page/_static/image6.png)
4. Cliquez sur **Ajouter** pour ajouter la page Web à votre projet.  
Visual Studio crée la page et l’ouvre.

### <a name="adding-html-to-the-page"></a>Ajout de code HTML à la page

Dans cette partie de la procédure pas à pas, vous allez ajouter du texte statique à la page.

### <a name="to-add-text-to-the-page"></a>Pour ajouter du texte à la page

1. En bas de la fenêtre de document, cliquez sur l’onglet **conception** pour basculer en mode **Design** .

    Mode Création affiche la page actuelle d’une façon similaire à WYSIWYG. À ce stade, vous n’avez aucun texte ou aucun contrôle sur la page ; la page est donc vide, à l’exception d’une ligne en pointillés qui présente un rectangle. Ce rectangle représente un élément **div** sur la page.
2. Cliquez à l’intérieur du rectangle qui est entouré d’une ligne en pointillés.
3. Tapez **Bienvenue dans Visual Web Developer** , puis appuyez deux fois sur **entrée** .

    L’illustration suivante montre le texte que vous avez tapé en mode **Design** .

    ![Texte de bienvenue dans Mode Création](creating-a-basic-web-forms-page/_static/image7.png "Texte de bienvenue en mode Design")
4. Basculez en mode **source** .

    Vous pouvez voir le code HTML en mode **source** que vous avez créé lorsque vous avez tapé en mode **Design** .  
    ![page Web avec du texte statique](creating-a-basic-web-forms-page/_static/image8.png)

### <a name="running-the-page"></a>Exécution de la page

Avant de continuer en ajoutant des contrôles à la page, vous pouvez d’abord l’exécuter.

### <a name="to-run-the-page"></a>Pour exécuter la page

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *FirstWebPage. aspx* et sélectionnez **définir comme page de démarrage**.
2. Appuyez sur **CTRL + F5** pour exécuter la page.

    La page s’affiche dans le navigateur. Bien que la page que vous avez créée ait une extension de nom de fichier *. aspx*, elle s’exécute actuellement comme n’importe quelle page html.

    Pour afficher une page dans le navigateur, vous pouvez également cliquer avec le bouton droit sur la page dans **Explorateur de solutions** et sélectionner **afficher dans le navigateur**.
3. Fermez le navigateur pour arrêter l’application Web.

## <a name="adding-and-programming-controls"></a>Ajout et programmation de contrôles

<a id="sectionToggle1"></a>

Vous allez maintenant ajouter des contrôles serveur à la page. Les contrôles serveur, tels que les boutons, les étiquettes, les zones de texte et d’autres contrôles familiers, fournissent des fonctionnalités de traitement de formulaire classiques pour vos pages Web Forms. Toutefois, vous pouvez programmer les contrôles avec du code qui s’exécute sur le serveur, plutôt que sur le client.

Vous allez ajouter un contrôle [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , un contrôle [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) et un contrôle [label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) à la page et écrire du code pour gérer l’événement [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) du contrôle [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .

### <a name="to-add-controls-to-the-page"></a>Pour ajouter des contrôles à la page

1. Cliquez sur l’onglet **conception** pour basculer en mode **conception** .
2. Placez le point d’insertion à la fin du texte **Bienvenue dans Visual Web Developer** et appuyez sur la touche **entrée** cinq fois ou plus pour faire de la place dans la zone de l’élément **div** .
3. Dans la **boîte à outils**, développez le groupe **standard** s’il n’est pas déjà développé.  
Notez que vous devrez peut-être développer la fenêtre **boîte à outils** à gauche pour l’afficher.
4. Faites glisser un contrôle [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) sur la page et déposez-le au milieu de la zone d’élément **div** qui contient **Visual Web Developer** sur la première ligne.
5. Faites glisser un contrôle [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) sur la page et déposez-le à droite du contrôle [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) .
6. Faites glisser un contrôle [label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) sur la page et déposez-le sur une ligne distincte sous le contrôle [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .
7. Placez le point d’insertion au-dessus du contrôle [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) , puis tapez **Entrez votre nom :** .

    Ce texte HTML statique est la légende du contrôle [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) . Vous pouvez mélanger des contrôles HTML et serveur statiques sur la même page. L’illustration suivante montre comment les trois contrôles s’affichent en mode **Design** .

    ![Trois contrôles dans Mode Création](creating-a-basic-web-forms-page/_static/image9.png "Trois contrôles en mode Design")

### <a name="setting-control-properties"></a>Définition des propriétés de contrôle

Visual Studio vous permet de définir les propriétés des contrôles sur la page de différentes façons. Dans cette partie de la procédure pas à pas, vous allez définir les propriétés en mode **Design** et en mode **source** .

### <a name="to-set-control-properties"></a>Pour définir des propriétés de contrôle

1. Tout d’abord, affichez les fenêtres de **Propriétés** en sélectionnant dans le menu **affichage** -&gt; **autres fenêtres** -&gt; **fenêtre Propriétés**. Vous pouvez également sélectionner **F4** pour afficher la fenêtre **Propriétés** .
2. Sélectionnez le contrôle [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , puis, dans la fenêtre **Propriétés** , affectez à **texte** la valeur **afficher le nom**. Le texte que vous avez entré apparaît sur le bouton dans le concepteur, comme indiqué dans l’illustration suivante.

    ![Définir le texte du bouton](creating-a-basic-web-forms-page/_static/image10.png "Texte du bouton Définir")
3. Basculez en mode **source** .

    Le mode **source** affiche le code HTML de la page, y compris les éléments créés par Visual Studio pour les contrôles serveur. Les contrôles sont déclarés à l’aide d’une syntaxe de type HTML, sauf que les balises utilisent le préfixe **asp :** et incluent l’attribut **runat =&quot;Server&quot;** .

    Les propriétés de contrôle sont déclarées en tant qu’attributs. Par exemple, lorsque vous définissez la propriété [Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) du contrôle [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , à l’étape 1, vous avez en fait défini l’attribut **Text** dans le balisage du contrôle.

    > [!NOTE] 
    > 
    > Tous les contrôles se trouvent à l’intérieur d’un élément de **formulaire** , qui a également l’attribut **runat =&quot;Server&quot;** . L’attribut **runat =&quot;server&quot;** et le préfixe **asp :** pour les balises de contrôle marquent les contrôles afin qu’ils soient traités par ASP.net sur le serveur lors de l’exécution de la page. Du code en dehors de **&lt;forme runat =&quot;server&quot;&gt;** et **&lt;script runat =&quot;Server&quot;** &gt;éléments sont envoyés sans modification au navigateur, ce qui explique pourquoi le code ASP.net doit se trouver à l’intérieur d’un élément dont la balise d’ouverture contient l’attribut **runat =&quot;server&quot;** .
4. Ensuite, vous allez ajouter une propriété supplémentaire au contrôle [label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Placez le point d’insertion directement après **asp : label** dans la balise **&lt;asp : label&gt;** , puis appuyez sur la **barre d’espace**.

    Une liste déroulante s’affiche pour afficher la liste des propriétés disponibles que vous pouvez définir pour un contrôle [label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Cette fonctionnalité, appelée **IntelliSense**, vous aide en mode **source** avec la syntaxe des contrôles serveur, des éléments HTML et d’autres éléments de la page. L’illustration suivante montre la liste déroulante **IntelliSense** pour le contrôle [label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .

    ![Attributs IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "Attributs IntelliSense")
5. Sélectionnez **ForeColor** , puis tapez un signe égal.

    IntelliSense affiche une liste de couleurs.

    > [!NOTE] 
    > 
    > Vous pouvez afficher une liste déroulante **IntelliSense** à tout moment en appuyant sur **Ctrl + J** lorsque vous affichez le code.
6. Sélectionnez une couleur pour le texte du contrôle **[label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** . Veillez à sélectionner une couleur qui est suffisamment sombre pour lire un arrière-plan blanc.

    L’attribut **ForeColor** est complété avec la couleur que vous avez sélectionnée, y compris le guillemet fermant.

### <a name="programming-the-button-control"></a>Programmation du contrôle Button

Pour cette procédure pas à pas, vous allez écrire du code qui lit le nom entré par l’utilisateur dans la zone de texte, puis affiche le nom dans le contrôle [label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .

### <a name="add-a-default-button-event-handler"></a>Ajouter un gestionnaire d’événements de bouton par défaut

1. Basculez en mode **Design** .
2. Double-cliquez sur le contrôle [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .

    Par défaut, Visual Studio bascule vers un fichier code-behind et crée un gestionnaire d’événements squelette pour l’événement par défaut du contrôle [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , l’événement [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) . Le fichier code-behind sépare votre balisage de l’interface utilisateur (par exemple, HTML) de votre C#code serveur (tel que).   
   Le curseur est positionné sur le code ajouté pour ce gestionnaire d’événements.

    > [!NOTE] 
    > 
    > Vous pouvez créer des gestionnaires d’événements en double-cliquant sur un contrôle en mode **Design** .
3. Dans le **\_Button1, cliquez sur** gestionnaire d’événements, tapez **Label1** suivi d’un point ( **.** ).

    Quand vous tapez le point après l' **ID** de l’étiquette (**Label1**), Visual Studio affiche une liste des membres disponibles pour le contrôle [label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) , comme indiqué dans l’illustration suivante. Un membre est généralement une propriété, une méthode ou un événement.

    ![IntelliSense en mode Code](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense en mode Code")
4. Terminez le gestionnaire d’événements **Click** pour le bouton afin qu’il soit lu comme indiqué dans l’exemple de code suivant.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Revenez à l’affichage de la vue **source** de votre balisage HTML en cliquant avec le bouton droit sur *FirstWebPage. aspx* dans le **Explorateur de solutions** et en sélectionnant **afficher le balisage**.
6. Faites défiler jusqu’à l’élément **&lt;asp : Button&gt;** . Notez que l’élément **&lt;asp : Button&gt;** a maintenant l’attribut **OnClick =&quot;Button1\_cliquez sur&quot;** .

    Cet attribut lie l’événement [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) du bouton à la méthode de gestionnaire que vous avez codée à l’étape précédente.

    Les méthodes de gestionnaire d’événements peuvent avoir n’importe quel nom ; le nom que vous voyez est le nom par défaut créé par Visual Studio. Le point important est que le nom utilisé pour l’attribut **OnClick** dans le code html doit correspondre au nom d’une méthode définie dans le code-behind.

### <a name="running-the-page"></a>Exécution de la page

Vous pouvez maintenant tester les contrôles serveur sur la page.

### <a name="to-run-the-page"></a>Pour exécuter la page

1. Appuyez sur **CTRL + F5** pour exécuter la page dans le navigateur. Si une erreur se produit, revérifiez les étapes ci-dessus.
2. Entrez un nom dans la zone de texte et cliquez sur le bouton **nom complet** .

    Le nom que vous avez entré est affiché dans le contrôle [label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Notez que lorsque vous cliquez sur le bouton, la page est publiée sur le serveur Web. ASP.NET recrée ensuite la page, exécute votre code (dans ce cas, le gestionnaire d’événements [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) du contrôle [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) s’exécute), puis envoie la nouvelle page au navigateur. Si vous regardez la barre d’État dans le navigateur, vous pouvez voir que la page effectue un aller-retour sur le serveur Web chaque fois que vous cliquez sur le bouton.
3. Dans le navigateur, affichez la source de la page en cours d’exécution en cliquant avec le bouton droit sur la page et en sélectionnant **afficher la source**.

    Dans le code source de la page, vous voyez le code HTML sans code serveur. Plus précisément, vous ne voyez pas les éléments **&lt;asp :&gt;** avec lesquels vous travailliez en mode **source** . Lorsque la page s’exécute, ASP.NET traite les contrôles serveur et restitue des éléments HTML sur la page qui exécute les fonctions qui représentent le contrôle. Par exemple, le contrôle **&lt;asp : Button&gt;** est rendu sous la forme du code HTML **&lt;input type =&quot;envoyer&quot;&gt;** élément.
4. Fermez le navigateur.

## <a name="working-with-additional-controls"></a>Utilisation de contrôles supplémentaires

<a id="sectionToggle2"></a>

Dans cette partie de la procédure pas à pas, vous utiliserez le contrôle [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) , qui affiche les dates mensuelles à la fois. Le contrôle [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) est un contrôle plus complexe que le bouton, la zone de texte et l’étiquette que vous utilisez et qui illustre certaines fonctionnalités supplémentaires des contrôles serveur.

Dans cette section, vous allez ajouter un contrôle [System. Web. UI. WebControls. Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) à la page et le mettre en forme.

### <a name="to-add-a-calendar-control"></a>Pour ajouter un contrôle de calendrier

1. Dans Visual Studio, basculez en mode **Design** .
2. À partir de la section **standard** de la **boîte à outils**, faites glisser un contrôle [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) sur la page et déposez-le sous l’élément **div** qui contient les autres contrôles.

    Le panneau des balises actives du calendrier s’affiche. Le panneau affiche des commandes qui vous permettent d’effectuer facilement les tâches les plus courantes pour le contrôle sélectionné. L’illustration suivante montre le contrôle [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) tel qu’il est rendu en mode **Design** .

    ![Contrôle Calendar dans Mode Création](creating-a-basic-web-forms-page/_static/image13.png "Contrôle Calendar en mode Design")
3. Dans le panneau des balises actives, choisissez **mise en forme automatique**.

    La boîte de dialogue **mise en forme automatique** s’affiche, qui vous permet de sélectionner un schéma de mise en forme pour le calendrier. L’illustration suivante montre la boîte de dialogue **mise en forme automatique** pour le contrôle [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) .

    ![Mise en forme automatique, boîte de dialogue (contrôle Calendar)](creating-a-basic-web-forms-page/_static/image14.png "Boîte de dialogue Mise en forme automatique (contrôle Calendar)")
4. Dans la liste **Sélectionner un schéma** , sélectionnez **simple** , puis cliquez sur **OK**.
5. Basculez en mode **source** .

    Vous pouvez voir l’élément **&lt;asp : Calendar&gt;** . Cet élément est bien plus long que les éléments pour les contrôles simples que vous avez créés précédemment. Il comprend également des sous-éléments, tels que **&lt;WeekEndDayStyle&gt;** , qui représentent différents paramètres de mise en forme. L’illustration suivante montre le contrôle [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) en mode **source** . (Le balisage exact que vous voyez en mode **source** peut différer légèrement de l’illustration.)

    ![Contrôle Calendar en mode Source](creating-a-basic-web-forms-page/_static/image15.png "Contrôle Calendar en mode Source")

### <a name="programming-the-calendar-control"></a>Programmation du contrôle calendrier

Dans cette section, vous allez programmer le contrôle [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) pour afficher la date actuellement sélectionnée.

### <a name="to-program-the-calendar-control"></a>Pour programmer le contrôle calendrier

1. En mode **conception** , double-cliquez sur le contrôle [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) .

    Un nouveau gestionnaire d’événements est créé et affiché dans le fichier code-behind nommé *FirstWebPage.aspx.cs*.
2. Terminez le gestionnaire d’événements [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) avec le code suivant.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    Le code ci-dessus définit le texte du contrôle Label à la date sélectionnée du contrôle Calendar.

### <a name="running-the-page"></a>Exécution de la page

Vous pouvez maintenant tester le calendrier.

### <a name="to-run-the-page"></a>Pour exécuter la page

1. Appuyez sur **CTRL + F5** pour exécuter la page dans le navigateur.
2. Cliquez sur une date dans le calendrier.

    La date sur laquelle vous avez cliqué s’affiche dans le contrôle [label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .
3. Dans le navigateur, affichez le code source de la page.

    Notez que le contrôle [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) a été rendu sur la page sous la forme d’un **tableau**, chaque jour étant un élément **TD** .
4. Fermez le navigateur.

## <a name="next-steps"></a>Étapes suivantes

<a id="nextStepsToggle"></a>

Cette procédure pas à pas a illustré les fonctionnalités de base du concepteur de pages de Visual Studio. Maintenant que vous comprenez comment créer et modifier une page de Web Forms dans Visual Studio, vous souhaiterez peut-être explorer d’autres fonctionnalités. Par exemple, vous souhaiterez peut-être effectuer les opérations suivantes :

- Pour en savoir plus sur ASP.NET Web Forms, suivez la série de didacticiels pas à pas [prise en main avec ASP.NET 4,5 Web Forms et Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- En savoir plus sur les feuilles de style en cascade (CSS). Pour plus d’informations, consultez [utilisation de CSS vue d’ensemble](https://msdn.microsoft.com/library/bb398931.aspx).
