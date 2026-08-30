# Technicien 2.0 Studio — binaires du poste de bureau

Ce dépôt ne porte pas de code. Il ne sert qu'à publier les installeurs Windows
du poste de bureau d'études et le manifeste que l'application interroge pour se
mettre à jour. Le code source vit dans un dépôt privé.

Il est public pour une raison précise : le client de mise à jour interroge le
manifeste **sans authentification**, et il le doit — un jeton compilé dans
l'exécutable s'en extrairait. GitHub répondant 404 à un client anonyme sur un
dépôt privé, la publication devait sortir d'ici.

## Télécharger

Tout se trouve dans la [dernière version publiée](../../releases/latest).

| Fichier | À quoi il sert |
|---|---|
| `…_x64-setup.exe` | Installation sur un poste. C'est aussi ce que la mise à jour automatique télécharge. |
| `…_x64.msi` | Déploiement de parc — GPO, Intune, SCCM. |
| `latest.json` | Manifeste de mise à jour, lu par l'application. |
| `*.sig` | Signatures des archives. |

L'installeur embarque WebView2 : il s'installe sans accès réseau.

## Mise à jour

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

## Signature

Chaque archive est signée, et l'application refuse celle dont la signature ne
correspond pas à la clé publique compilée dans l'exécutable. C'est la seule
protection réelle contre un manifeste détourné.

La clé privée ne figure pas ici et n'y figurera jamais : elle vit dans les
secrets du dépôt privé, où la chaîne de publication l'emploie pour signer. Ce
dépôt-ci ne reçoit que du déjà-signé, et n'a besoin d'aucun secret.
