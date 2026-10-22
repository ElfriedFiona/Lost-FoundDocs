# Lost&Found Docs

Application mobile de restitution de documents officiels, académiques, administratifs et professionnels (Douala), développée en **.NET MAUI** (C#, XAML, MVVM).

> ## ⚠️ Frontend uniquement
> Aucun backend, aucune API, aucune base de données, aucune authentification réelle, aucune notification push, aucune messagerie entre appareils. Les données sont **fictives et gardées en mémoire** ; elles disparaissent à la fermeture de l'application. Toutes les simulations sont signalées par `// SIMULATION` dans le code et par un bandeau « Mode démonstration » dans l'interface.

## Prérequis
- Windows 10/11, **Visual Studio Community 2022 (17.14 ou plus récent)**
- Charge de travail **« Développement d'applications multiplateformes .NET »** (Visual Studio Installer), avec le SDK **.NET 9**
- Pour Android : SDK Android et un émulateur (Android Device Manager) ou un téléphone en mode développeur
- Pour Windows : mode développeur activé (Paramètres > Confidentialité et sécurité > Pour les développeurs)

Cibles : `net9.0-android` et `net9.0-windows10.0.19041.0` (iOS et Mac Catalyst ne sont pas ciblés).

## Ouvrir et lancer
1. Ouvrir `LostFoundDocs.sln` dans Visual Studio.
2. Choisir la cible **Windows Machine** ou **Android Emulator** dans la barre d'outils.
3. Lancer avec **F5**.

En ligne de commande :
```
dotnet build LostFoundDocs/LostFoundDocs.csproj -f net9.0-android
dotnet build LostFoundDocs/LostFoundDocs.csproj -f net9.0-windows10.0.19041.0
```

## Compte de démonstration
| | |
|---|---|
| Téléphone | `6 77 12 48 21` (préfixe +237) |
| Mot de passe | `Demo@2026` |
| Code SMS « mot de passe oublié » | `123456` |

Le bouton « Utiliser le compte de démonstration » de l'écran de connexion remplit les champs. Un compte créé via l'inscription est vide (permet de tester les états vides) et n'existe que pendant la session.

## Architecture
```
LostFoundDocs/
  Models/            modèles purs (Report, ReportDraft, UserAccount, règles de statut)
  Services/
    Abstractions/    interfaces (IAuthService, IReportService, INavigationService, IDialogService)
    Mock/            implémentations fictives en mémoire (SIMULATION) + données de démonstration
    Navigation/      routes, navigation Shell, dialogues
    Validation/      règles de saisie, masquage, formats de date
  ViewModels/        état et commandes (MVVM sans dépendance externe)
  Views/             écrans (Auth, Home, Shared) et Components (composants réutilisables)
  UI/                icônes, accès aux ressources, présentation
  Resources/         Colors.xaml, Styles.xaml, police d'icônes Lucide
```
Pour brancher un backend plus tard : remplacer les deux lignes `MockAuthService` / `MockReportService` de `MauiProgram.cs` par des implémentations HTTP des mêmes interfaces.

## Icônes
Police d'icônes **Lucide** (licence ISC), fichier `Resources/Fonts/lucide.ttf`, alias `Lucide`. Les glyphes sont déclarés dans `UI/Icons.cs`. Voir `THIRD_PARTY_NOTICES.md`.

## Règles garanties par le frontend
- **Un signalement n'est enregistré qu'une fois** : un brouillon (`ReportDraft`) devient un `Report` à la validation, un double appui renvoie le même signalement ; ensuite seul son **statut** évolue (transitions contrôlées dans `ReportStatusRules`).
- **Dates cohérentes** : toutes les dates fictives sont calculées à partir de la date du jour ; `DemoDataValidator` vérifie les invariants (résultat dans la fenêtre Sortie en Debug).
- **Date de naissance facultative**, numéro de document jamais obligatoire.

## État d'avancement
- Phase A : thème, composants, services, connexion, inscription, mot de passe oublié, accueil, navigation par onglets.
- Phase B : formulaires de déclaration, recherche, correspondances, signalements, messages, notifications, restitution, preuve, profil complet.
- Phase C : interactions, états vides, filtres, parcours complets.

## Simulations actuelles
| Fonction | Simulation |
|---|---|
| Connexion / inscription | comptes en mémoire, mot de passe en clair en mémoire |
| Mot de passe oublié | code fixe affiché, aucun SMS |
| « Recherche active vérifiée il y a 12 min » | heure fictive |
| Signalements du compte de démonstration | jeu de données généré (dates relatives) |

Le backend devra fournir, pour chacune : authentification sécurisée (hachage, jetons), envoi réel de SMS, moteur de correspondance, stockage persistant.
