---
uid: whitepapers/ms03-32-issue
title: Correction de l’erreur « application serveur non disponible » après l’application de la mise à jour de sécurité pour Internet Explorer | Microsoft Docs
author: rick-anderson
description: Ce document décrit le correctif qui résout un problème lié à la mise à jour de sécurité MS03-32 pour Internet Explorer qui affecte les applications ASP.NET 1,0 s’exécutant sur un réseau Wi...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: e0b6776cbfe22e341ac7105f03daac5074b480fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574185"
---
# <a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Correctif pour l’erreur « Application serveur non disponible » après application de la mise à jour de sécurité pour Internet Explorer

> Ce document décrit le correctif qui résout un problème lié à la mise à jour de sécurité MS03-32 pour Internet Explorer qui affecte les applications ASP.NET 1,0 s’exécutant sur Windows XP professionnel.
> 
> S’applique à ASP.NET 1,0 et Windows XP professionnel.

Microsoft a identifié un problème avec la mise à jour de sécurité MS03-32 pour le correctif de sécurité Internet Explorer et ASP.NET 1,0 s’exécutant sur Windows XP. Ce correctif peut être installé manuellement ou en obtenant des mises à jour critiques récentes à partir du site Windows Update.

Ce problème est dû au fait qu’après l’installation du correctif sur un ordinateur Windows XP, toutes les demandes adressées aux applications ASP.NET qui s’exécutent sur le serveur Web IIS 5,1 local provoquent un message d’erreur indiquant que l’application serveur n’est pas disponible. Les demandes adressées à des serveurs Web distants ne sont pas affectées.

Ce problème n’affecte que les installations exécutant ASP.NET 1,0 sur Windows XP. Elle n’affecte pas les ordinateurs qui exécutent Windows 2000 ou Windows Server 2003. Il n’affecte pas non plus les ordinateurs exécutant Windows XP avec ASP.NET 1,1 installé.

Notez que ce problème **n’est pas** un bogue de sécurité avec ASP.net. Il **n’ouvre pas** ou n’autorise pas les attaques malveillantes sur une application ou un serveur ASP.net. Au lieu de cela, il s’agit simplement d’un bogue fonctionnel provoqué par le correctif lui-même.

Nous travaillons en dur sur une solution permanente pour résoudre ce problème. En attendant, vous pouvez exécuter le fichier de commandes suivant en guise de solution de contournement pour le problème. Le fichier de commandes effectue les opérations suivantes :

1. Arrête les services d’État IIS et ASP.NET
2. Supprime et recrée le compte ASPNET avec un mot de passe temporaire connu
3. Utilise la commande de `runas` Windows pour lancer un fichier exécutable qui crée un profil utilisateur ASPNET
4. Réinscrit ASP.NET. Cela crée un nouveau mot de passe aléatoire pour le compte et applique les paramètres de contrôle d’accès ASP.NET par défaut pour celui-ci.
5. Redémarre le service IIS.

Le fichier de commandes contient un mot de passe temporaire codé en dur «<strong>1pass\@Word</strong>», auquel vous serez invité à entrer pour la commande runas lors de l’exécution du fichier de commandes. Une fois la commande runas terminée, le mot de passe du compte ASPNET est recréé avec une valeur aléatoire forte. Notez que le fichier de commandes peut échouer si le mot de passe codé en dur ne respecte pas les exigences de complexité de mot de passe dans votre environnement. Si c’est le cas, vous pouvez le remplacer par une autre valeur appropriée pour votre environnement.

*> [!IMPORTANT]* Si vous avez ajouté des paramètres de contrôle d’accès personnalisés ou des autorisations de compte de base de données pour le compte ASPNET, vous devez les recréer une fois ce fichier de commandes terminé. En effet, lorsque le compte est recréé, il obtient un nouvel identificateur de sécurité (SID).

*> [!IMPORTANT]* Si vous exécutez le processus de travail ASP.NET avec un compte personnalisé autre que le compte ASPNET, vous ne devez pas exécuter ce fichier de commandes. Au lieu de cela, vous devez vous connecter de manière interactive ou utiliser la commande runas avec ce compte, qui créera un profil utilisateur pour ce compte.

Le fichier de commandes est inclus dans l’archive à extraction automatique ci-dessous. Pour l’utiliser :

1. Vous devez exécuter en tant que compte avec des privilèges d’administrateur
2. [Télécharger et ouvrir le fichier exécutable à extraction automatique](ms03-32-issue/_static/fixup1.exe)
3. Extrayez le contenu dans c:\
4. Sélectionnez Exécuter... dans le menu Démarrer, entrez `cmd.exe`
5. Dans les fenêtres de commande ouvertes, tapez `c:\fixup.cmd`.
6. Quand vous y êtes invité, entrez <strong>1pass\@mot</strong> comme mot de passe.
7. Si vous avez déjà des paramètres de contrôle d’accès personnalisés ou des autorisations de compte de base de données pour le compte ASPNET, vous devez réappliquer ces paramètres maintenant.

De nombreuses excuses pour les désagréments occasionnés. Nous publions des informations supplémentaires dès qu’elles seront disponibles.

Le tableau ci-dessous détaille les plateformes et les versions affectées par ce problème.

| .NET Framework | Plate-forme | Subi |
| --- | --- | --- |
| Version 1.0 | Windows 2000 Professionnel | Non |
| Version 1.0 | Windows 2000 Server | Non |
| Version 1.0 | Windows XP Professionnel | Oui |
| Version 1.0 | Windows Server 2003 | Non |
| Version 1.0 | Windows XP famille avec Cassini | Non |
| Version 1.1 | Windows 2000 Professionnel | Non |
| Version 1.1 | Windows 2000 Server | Non |
| Version 1.1 | Windows XP Professionnel | Non |
| Version 1.1 | Windows Server 2003 | Non |
| Version 1.1 | Windows XP famille avec Cassini | Non |

Avec tous nos remerciements,   
 L’équipe ASP.NET
