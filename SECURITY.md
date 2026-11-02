# Sécurité — BeeFluid, canal de publication

Ce fichier est commun à tous les dépôts (Foody, Impera, BeeFluid, Capsa,
Alfred, Lineup, Fleetly, Kikiladi) et tenu à l'identique ; seule la section
« Propre à ce dépôt » change.

## Signaler une faille

Ne pas ouvrir d'issue publique. Passer par **Security → Report a
vulnerability** sur ce dépôt (signalement privé GitHub). Le signalement doit
dire ce qui est atteint, comment le reproduire, et ce qu'un attaquant y gagne.

## Les règles communes

- **Aucun secret dans le dépôt.** Les clés publiques (clé `anon` /
  `publishable` de Supabase, identifiants de projet Appwrite, ID clients OAuth)
  ne sont pas des secrets : elles voyagent dans chaque requête du navigateur.
  Tout le reste — clé `service_role`, mots de passe SFTP, clés de signature,
  jetons de publication — vit dans les secrets GitHub Actions ou dans ceux des
  Edge Functions. Le bloc « Secrets et clés » du `.gitignore` est le même
  partout. Un secret committé une fois se **change** : le retirer du dépôt ne
  le retire pas de l'historique.
- **Les workflows lisent, sauf exception écrite.** `permissions: contents:
  read` par défaut ; une tâche qui pousse un commit ou une étiquette le dit en
  tête de fichier. Les actions sont épinglées par SHA, avec la version en
  commentaire, et Dependabot les tient à jour.
- **Une valeur saisie ou un secret passe par `env:`**, jamais par `${{ }}` dans
  le corps d'un script : l'interpolation y colle le texte tel quel.
- **SFTP : la clé d'hôte est vérifiée** (`FTP_KNOWN_HOSTS`), jamais acceptée
  d'office. Sans elle, le déploiement échoue au lieu d'envoyer les
  identifiants à un serveur non vérifié.
- **Les droits se tiennent côté serveur.** RLS et fonctions `SECURITY
  DEFINER` (avec `search_path` fixé) pour Supabase, règles de document pour
  Appwrite. Ce que l'interface cache n'est pas protégé pour autant.
- **Un échec ne se tait pas, une donnée absente n'est pas favorable** : ce
  sont les règles de chaque dépôt, et elles valent pour la sécurité aussi.

## Propre à ce dépôt

- Dépôt public sans code : il ne porte que des Releases — installeur et MSI du
  poste de bureau, APK de l'application Android, signatures et manifeste
  `latest.json`. Il ne détient **aucun secret** et ne doit jamais en recevoir.
- Les archives sont signées avant d'arriver ici ; l'application refuse celle
  dont la signature ne correspond pas à la clé publique compilée dans
  l'exécutable. La clé privée n'est pas dans ce dépôt et n'y sera jamais.
- Un fichier publié ici qu'aucune chaîne de publication n'a produit, ou un
  manifeste qui désigne autre chose que les fichiers de sa Release, se
  signale comme une faille, par le canal ci-dessus.
