# Windows-Beefluid-releases — canal public de publication de BeeFluid

Les installeurs du poste de bureau d'études, l'APK de l'application de terrain et le manifeste de mise à jour, sans aucun code.

| Famille | Plateforme | Publié sur | État |
|---|---|---|---|
| BeeFluid | Windows (poste de bureau) · Android (APK) | Releases de ce dépôt | En service |

## Sommaire

- [En bref](#en-bref)
- [Fonctionnement](#fonctionnement)
- [Démarrer](#démarrer)
- [Vérifier](#vérifier)
- [Publier](#publier)
- [Configuration](#configuration)
- [Organisation du dépôt](#organisation-du-dépôt)
- [Sécurité](#sécurité)
- [Dépôts liés](#dépôts-liés)
- [Licence](#licence)

## En bref

Ce dépôt ne porte pas de code. Il ne sert qu'à publier les installeurs Windows
du poste de bureau d'études (anciennement « Technicien 2.0 Studio »), l'APK de
l'application Android et le manifeste que le poste interroge pour se mettre à
jour. Le code source vit dans un dépôt privé.

Il est public pour une raison précise : le client de mise à jour interroge le
manifeste **sans authentification**, et il le doit — un jeton compilé dans
l'exécutable s'en extrairait. GitHub répondant 404 à un client anonyme sur un
dépôt privé, la publication devait sortir d'ici.

## Fonctionnement

### Télécharger

Tout se trouve dans la [dernière version publiée](../../releases/latest).

| Fichier | À quoi il sert |
|---|---|
| `…_x64-setup.exe` | Installation sur un poste. C'est aussi ce que la mise à jour automatique télécharge. |
| `…_x64.msi` | Déploiement de parc — GPO, Intune, SCCM. |
| `latest.json` | Manifeste de mise à jour, lu par l'application. |
| `*.sig` | Signatures des archives. |
| `BeeFluid-<version>.apk` | Application de terrain, à poser à la main sur le téléphone (Releases `M.…`). |

Jusqu'à la version 0.4.6, l'installeur embarquait WebView2 et s'installait sans
accès réseau. Depuis la 0.4.8, il ne l'embarque plus et le télécharge s'il
manque : WebView2 est présent sur Windows 11 et sur Windows 10 à jour, mais une
machine **sans réseau et sans WebView2** ne peut plus installer le poste.

L'APK s'installe par-dessus la version précédente en gardant les données ;
**désinstaller l'application efface ses interventions et ses relevés**, qui ne
vivent que sur le téléphone.

### Mise à jour

À partir de la version 0.4.0, l'application vérifie ce dépôt au démarrage et
propose la version disponible. Elle n'installe jamais d'elle-même : un
ingénieur au milieu d'une note de calcul ne doit pas voir son application
redémarrer sous lui.

Les postes qui portent une version antérieure ne verront pas les nouvelles.
L'adresse du manifeste est compilée dans l'exécutable, et elle désignait un
autre endroit : une installation manuelle, une fois, suffit à rejoindre le
canal.

L'état du canal se lit dans l'application, à *Administration → Canal de mise à
jour* : version installée, adresse interrogée et résultat de la dernière
interrogation.

L'application Android n'a pas de mise à jour automatique : une nouvelle version
se télécharge ici et se pose par-dessus la précédente.

### Signature

Chaque archive est signée, et l'application refuse celle dont la signature ne
correspond pas à la clé publique compilée dans l'exécutable. C'est la seule
protection réelle contre un manifeste détourné.

La clé privée ne figure pas ici et n'y figurera jamais : elle vit dans les
secrets du dépôt privé, où la chaîne de publication l'emploie pour signer. Ce
dépôt-ci ne reçoit que du déjà-signé, et n'a besoin d'aucun secret.

## Démarrer

Sans objet : il n'y a rien à installer ni à lancer, ce dépôt ne contient que
des fichiers publiés.

## Vérifier

Sans objet : aucun code ne s'exécute ici. Le canal se vérifie en ouvrant
`releases/latest/download/latest.json` de ce dépôt dans une fenêtre de
navigation privée : si le JSON s'affiche, les postes le liront aussi.

## Publier

Les Releases sont créées par la chaîne de publication du dépôt source, après
ses tests et sa signature ; rien ne se dépose ici à la main.

## Configuration

Sans objet : ce dépôt n'a ni secret, ni variable, ni workflow.

## Organisation du dépôt

```
README.md, SECURITY.md, AGENTS.md, CLAUDE.md   Présentation et règles communes
.editorconfig, .gitattributes                  Mise en forme
.github/                                       Gabarits de pull request et d'issues
```

Les binaires ne sont pas dans l'arbre : ils sont attachés aux Releases.

## Sécurité

Voir [SECURITY.md](SECURITY.md). Ce dépôt ne détient aucun secret ; seules les
signatures, vérifiées par l'application, garantissent ce qui s'y publie.

## Dépôts liés

Le même tableau figure dans chaque dépôt. Un dépôt qui s'ajoute s'ajoute partout.

| Famille | Dépôt | Rôle |
|---|---|---|
| Foody | [Mobile-App-Foody](https://github.com/App-Foody/Mobile-App-Foody) | Application de suivi alimentaire (Expo) ; tient le SQL du projet Supabase commun |
| Foody | [Website-Foody](https://github.com/App-Foody/Website-Foody) | Site public, compte, réinitialisation, suppression de compte |
| Foody | [Website-FoodySupport](https://github.com/App-Foody/Website-FoodySupport) | Console des agents du support |
| Impera | [Mobile-App-Impera](https://github.com/Impera-App/Mobile-App-Impera) | Application de concentration, Android (Expo) |
| Impera | [Windows-App-Impera](https://github.com/Impera-App/Windows-App-Impera) | Application Windows (Electron) et service ImperaGuard |
| Impera | [MacOS-App-Impera](https://github.com/Impera-App/MacOS-App-Impera) | Application macOS (Swift) et filtre réseau |
| Impera | [Website-Impera](https://github.com/Impera-App/Website-Impera) | Site public, compte, paiement ; Edge Functions Stripe |
| Impera | [Website-ImperaSupport](https://github.com/Impera-App/Website-ImperaSupport) | Console des agents du support |
| Impera | [Windows-Impera-Releases](https://github.com/Impera-App/Windows-Impera-Releases) | Canal public de mise à jour de l'app Windows |
| BeeFluid | [Windows-Beefluid](https://github.com/BeeFluid/Windows-Beefluid) | Application terrain (Expo) et poste de bureau d'études (Tauri) |
| BeeFluid | [Windows-Beefluid-releases](https://github.com/BeeFluid/Windows-Beefluid-releases) | Canal public de publication du poste et de l'APK |
| BeeFluid | [Mobile-Beefluid](https://github.com/BeeFluid/Mobile-Beefluid) | Vide : l'application mobile vit dans Windows-Beefluid |
| Lorderis | [capsa](https://github.com/LordEris/capsa) | Gestion de patrimoine : web, API Node, schéma Supabase `capsa` |
| Lorderis | [HealthChecks](https://github.com/LordEris/HealthChecks) | Alfred, assistant personnel : web, Electron, Android |
| Lorderis | [Lineup](https://github.com/LordEris/Lineup) | Playlists de groupe (Next.js, Appwrite) |
| Lorderis | [Fleetly](https://github.com/LordEris/Fleetly) | Gestion de garage et de flotte (Expo) |
| Lorderis | [Website-Kikiladi](https://github.com/LordEris/Website-Kikiladi) | Jeu « Kikiladi ? » (statique + PHP) |
| Lorderis | [Website-LordEris](https://github.com/LordEris/Website-LordEris) | Site vitrine Lorderis et pages des projets |

## Licence

Aucune licence publiée : tous droits réservés.
