# Tessera

[English](README.md) · [简体中文](README.zh-CN.md) · **Français** · [Español](README.es.md) · [Русский](README.ru.md) · [العربية](README.ar.md)

[![Licence: AGPL-3.0](https://img.shields.io/badge/licence-AGPL--3.0-blue.svg)](LICENSE) [![Commercial licence available](https://img.shields.io/badge/commercial%20licence-available-brightgreen.svg)](LICENSE-COMMERCIAL.md) [![Platform: Windows](https://img.shields.io/badge/platform-Windows%2010%2F11%20x64-lightgrey.svg)]()

**Tessera** — un coffre-fort chiffré multi-facteurs pour Windows qui embarque
aussi un gestionnaire de mots de passe, un historique de presse-papiers, des notes, un
assistant IA et un gestionnaire de fichiers rapide. Tout vit derrière un seul
déverrouillage ; rien ne quitte votre machine sauf si vous le configurez explicitement.

> Statut : `0.1.12`, en développement actif. Windows 10/11 x64 uniquement — plusieurs
> fonctionnalités (Windows Hello, intégration à l'explorateur, raccourcis fichiers
> globaux) dépendent directement des API Windows.

---

## Sommaire

- [Fonctionnalités](#fonctionnalités)
- [Prise en main](#prise-en-main)
- [Premier lancement : l'assistant de configuration](#premier-lancement--lassistant-de-configuration)
- [Configurer la connexion par code e-mail](#configurer-la-connexion-par-code-e-mail)
- [Configurer l'application d'authentification](#configurer-lapplication-dauthentification)
- [Configurer le déverrouillage biométrique](#configurer-le-déverrouillage-biométrique)
- [Clé de récupération — à lire absolument](#clé-de-récupération--à-lire-absolument)
- [Utilisation au quotidien](#utilisation-au-quotidien)
- [Synchronisation multi-appareils](#synchronisation-multi-appareils)
- [Modèle de sécurité](#modèle-de-sécurité)
- [Où vivent vos données](#où-vivent-vos-données)
- [Développement](#développement)
- [Construire une version](#construire-une-version)
- [Licence](#licence)

---

## Fonctionnalités

| Fonctionnalité | Ce qu'elle fait |
|---|---|
| **Chiffrement de fichiers** | Chiffrez/déchiffrez n'importe quel fichier vers un seul fichier `.ivault`. Trois profils : **post-quantique** (par défaut — AES-256-GCM pour le contenu, la clé de chaque fichier étant en plus encapsulée par ML-KEM-1024, si bien qu'il faut casser les deux constructions), AES-256-GCM classique, ou ChaCha20-Poly1305. Le profil s'applique de façon identique à une sélection multiple de fichiers et à un dossier entier. Sélection par lot de plusieurs fichiers à la fois. Noms de fichiers et sommes de contrôle vivent dans un en-tête *chiffré* : un `.ivault` volé ne révèle ni l'un ni l'autre. Le déchiffrement dispose de sa propre entrée pour les dossiers, à côté de celle des fichiers, et ouvrir un dossier chiffré affiche son contenu sans déchiffrer le conteneur au préalable : la liste ne lit que les quelques centaines de Ko où se trouve l'index de l'archive, si bien qu'un dossier de 10 Go s'ouvre aussi vite qu'un petit, et l'extraction écrit chaque fichier une seule fois. |
| **Suppression sécurisée + recherche de résidus** | Écrase puis supprime en option l'original après chiffrement, et recherche les résidus en clair — sauvegardes Office/WPS et copies homonymes laissées ailleurs sur le disque. |
| **Gestionnaire de mots de passe** | CRUD complet, masqué par défaut, import depuis un CSV de mots de passe Chrome/Edge/Firefox (le CSV en clair est supprimé de façon sécurisée après import), export au format Chrome/Edge. |
| **Historique du presse-papiers** | Historique chiffré avec classification automatique (URL / e-mail / téléphone / **formule** / code / texte), épinglage, recherche, défilement virtuel, liste noire par application, corbeille, icône de zone de notification, et un panneau global via `Ctrl+Shift+V`. Cliquez sur une miniature pour ouvrir l'image entière (plein écran ou fenêtre flottante, au choix) avec zoom, déplacement et clic pour copier. Export de tout / des éléments épinglés / masqués / de la seule sélection, avec sélection multiple et tout sélectionner ; import de plusieurs fichiers à la fois, les originaux étant ensuite supprimés de façon sécurisée. Un import ne laisse jamais un second exemplaire de ce que vous avez déjà : une entrée identique est fusionnée plutôt que dupliquée, et ses marques épinglé et masqué sont conservées. Pour les doublons déjà présents, un nettoyage dresse la liste de ce qu'il a trouvé et ne fusionne qu'après votre confirmation ; ce qu'il écarte part à la corbeille. |
| **Notes** | Notes Markdown avec images, catégories et recherche plein texte, stockées dans une base de données chiffrée. Les catégories se trient par nom, par nombre de notes, par dernière utilisation — ou dans l'ordre que vous arrangez vous-même. Quatre compteurs surmontent la liste — Total, Actif, Caché, Supprimé — et chacun ouvre ce qu'il compte : cliquez sur Caché pour voir ces notes, sur Supprimé pour ouvrir la corbeille, et à nouveau pour revenir. Les deux qui feraient apparaître des notes masquées demandent d'abord une vérification d'identité, avec le même déverrouillage que partout ailleurs ; une note masquée mise à la corbeille reste elle aussi derrière ce contrôle, et la corbeille indique combien d'éléments elle retient, pour qu'une liste courte ne ressemble jamais à des données perdues. Chaque note retient l'endroit où vous vous êtes arrêté de lire ou d'écrire et vous y ramène à la réouverture, y compris dans les notes assez grandes pour être paginées. Une grande note s'affiche en quelques millisecondes et reste réactive pendant la frappe : le rendu se fait bloc par bloc sur un fil d'arrière-plan, si bien que le premier écran n'attend pas le reste — une note de 4 Mo montre son début en environ 40 ms au lieu d'une seconde et demie. |
| **Assistant IA** | Discutez avec n'importe quel point de terminaison compatible OpenAI (DeepSeek par défaut). La liste des modèles est récupérée en direct auprès du fournisseur et affichée avec la date de publication de chacun, du plus récent au plus ancien ; les modèles inconnus de l'application restent visibles et sélectionnables, et ceux qui sont retirés sont signalés au lieu d'échouer en silence. Le modèle choisi s'applique à toutes les fonctions IA de l'application. « Compétences » importables utilisant le même format `SKILL.md` que Claude Code. L'historique des conversations est stocké chiffré. |
| **Gestionnaire de fichiers** | Copier / couper / coller / supprimer à haute vitesse, interopérable avec le presse-papiers natif de l'explorateur, avec une carte de progression en direct. Raccourcis globaux `Ctrl+V` / `Suppr` / `Maj+Suppr`. |
| **Test de vitesse réseau** | Lancez et visualisez des tests de connexion, avec historique. Le débit est lu comme le lisent les tests de débit publics : la connexion est échantillonnée par tranches d'un quart de seconde et le chiffre retenu est le 90e centile du régime établi, si bien que la montée en charge de TCP et les creux isolés ne tirent pas le résultat sous ce que la ligne délivre réellement. La latence mesure un aller-retour TCP plutôt qu'une requête HTTPS complète, ce qui en exclut le temps de traitement du serveur, et la gigue est la variation moyenne entre échantillons consécutifs - la grandeur dont dépend la tenue d'un appel ou d'une partie. Chaque résultat s'accompagne d'un indice de stabilité indiquant de combien les moments rapides et lents du test se sont écartés, ce qui répond à la question de savoir si un chiffre différent au test suivant veut dire quelque chose. |
| **Mise à jour automatique signée** | La release contient **deux fichiers : un installateur et un exe portable**, un par modalité. La confiance vient de la signature Authenticode intégrée à l'intérieur, vérifiée par rapport à un nom d'éditeur et une empreinte de clé publique inscrits dans la compilation — un certificat auto-signé gratuit suffit donc ; un certificat d'autorité payant ne fait qu'éliminer l'avertissement SmartScreen. Un téléchargement non signé, altéré ou signé avec une autre clé est supprimé, et une compilation sans signataire configuré refuse toute mise à jour — jamais « autoriser si non configuré ». Une release contenant deux builds de la *même* modalité est rejetée plutôt que devinée. L'installation est transactionnelle : la signature est revérifiée après la fermeture de l'application, l'ancien exe est conservé, et une nouvelle version en échec est automatiquement restaurée. Votre coffre, vos identifiants et vos réglages vivent hors de l'exe et ne sont jamais touchés par une mise à jour. |
| **Synchronisation multi-appareils** | Copiez sur votre téléphone, collez sur votre PC — et inversement. Texte, texte enrichi, code, formules mathématiques et symboles physiques conservent leur mise en forme ; les images circulent aussi. Envoyez n'importe quel fichier ou dossier à un appareil ou à plusieurs à la fois, avec reprise après interruption et chiffrement de bout en bout. Quatre façons d'appairer — QR code, code d'appairage, choix de l'appareil dans la liste des appareils à proximité, ou saisie manuelle d'une adresse — une dizaine de secondes dans tous les cas. Quand la liste reste vide, une analyse active du réseau trouve les appareils que la découverte multicast n'atteint pas, et un diagnostic de connexion nomme l'étape qui échoue. Le partage depuis une autre application Android passe par le menu de partage du système : appui long sur une photo ou un fichier, choisir Tessera, puis désigner les appareils appairés destinataires. Les fichiers ne vont qu'aux appareils choisis — un ou plusieurs, désignés explicitement avant chaque envoi — tandis que le presse-papiers atteint toujours tous les appareils appairés. Une fois l'appairage fait, un appareil qui réapparaît sur le réseau se reconnecte seul en une seconde environ — et l'appairage ne se fait qu'une fois : des appareils appairés se retrouvent sur un autre réseau ou après un changement d'adresse IP, sans dépendre de la découverte multicast que beaucoup de routeurs bloquent. |
| **Nettoyage du PC** | Cinq panneaux qui s'occupent de la machine elle-même. Un bilan analyse d'un coup les fichiers inutiles, les caches des navigateurs, les traces d'utilisation, les modules, les programmes au démarrage, les sources de fenêtres publicitaires et les références obsolètes du registre — et se remplit au fur et à mesure plutôt que de vous faire regarder un écran figé. L'espace disque couvre l'allègement du disque système, les gros fichiers, l'espace par dossier, les doublons (comparés par taille, puis sur un échantillon de chaque fichier, puis intégralement) et une optimisation qui exécute TRIM sur un SSD au lieu de le défragmenter. Démarrage et menus liste ce qui se lance à l'ouverture de session — registre, dossier Démarrage, tâches planifiées et services — ainsi que chaque entrée du menu contextuel, avec un bloqueur de fenêtres publicitaires qui agit sur les deux fronts. Logiciels installés recense tout, et une boîte à outils regroupe quarante-deux petits utilitaires, avec recherche et épinglage. Avant un nettoyage, une simulation emprunte le vrai chemin du code sans toucher un octet et indique ce que chaque disque gagnerait et ce qui serait irrécupérable. Les données du navigateur sont détaillées par navigateur et par profil — cookies, historique, formulaires, stockage des sites — les mots de passe et les favoris n'étant jamais listés ni supprimables d'ici. Les paquets de pilotes remplacés par des versions plus récentes sont souvent le plus gros poids mort du disque système, et le nettoyage de disque de Windows n'y touche pas. Dossiers vides, raccourcis pointant dans le vide et fichiers de zéro octet sont rassemblés depuis le bureau, les documents et les téléchargements. Les logiciels installés sont classés par taille *et* par durée d'inactivité, car quatre gigaoctets ne deviennent une raison de désinstaller qu'une fois qu'on sait ne pas y avoir touché depuis un an. L'action de nettoyage est en haut de la page, pas en bas. Chaque catégorie précise quel genre de « vide » elle a trouvé — vraiment rien à nettoyer, bloqué par les permissions, ou jamais réellement exécuté — et combien d'emplacements elle a vérifiés. Les panneaux s'ouvrent sur le résultat précédent pendant qu'une nouvelle analyse tourne derrière. Vous pouvez ajouter des règles visant vos propres dossiers — jamais la racine d'un disque, un dossier système ou les données du coffre — et laisser une passe planifiée regarder sans rien supprimer. |
| **Savoir ce qu'on peut retirer** | Chaque élément porte l'un de cinq jugements — Windows en a besoin, un pilote, quelque chose que vous utilisez, facultatif, une publicité — avec la raison écrite en toutes lettres. Ce dont Windows a besoin est verrouillé et ne peut pas être sélectionné ; le backend le refuse même si on le lui demande. Tout ce qui n'est pas reconnu est classé facultatif, jamais déchet : rater un fichier de cache coûte des mégaoctets, supprimer un pilote coûte un après-midi. |
| **Rien de retiré que vous ne puissiez récupérer** | Les caches et fichiers temporaires sont supprimés définitivement — c'est ce qui libère la place. Vos propres fichiers vont à la Corbeille. Les clés de registre, les programmes au démarrage et les gestionnaires du menu contextuel sont *désactivés avec une sauvegarde*, jamais supprimés. Une sauvegarde du registre est exportée avant toute modification, et si l'export échoue, rien n'est modifié. |
| **Interface et apparence** | Un seul langage visuel sur tous les écrans — même en-tête de page, mêmes cartes de section, mêmes couleurs d'état partout, rien qui semble rapporté après coup. Huit couleurs d'accentuation, deux styles visuels (Natif enrichi / Luxe technologique oriental), système/clair/sombre, et un mode Simple ou Professionnel qui masque ou révèle la profondeur de diagnostic. L'application Android compagnon reprend la même palette et la même organisation en sections. |
| **6 langues** | English, 简体中文, Français, Español, Русский, العربية — modifiable partout, y compris sur l'écran de connexion. |

Cinq façons de déverrouiller, **une seule suffit** : mot de passe · code e-mail ·
application d'authentification (TOTP) · Windows Hello · clé de récupération

Windows Hello désigne ici la méthode que vous avez enregistrée — **visage, empreinte ou code PIN**.
La fenêtre est celle du système : un PC doté d'une caméra Hello mais sans lecteur d'empreintes se
déverrouille par reconnaissance faciale.

L'écran de connexion **s'ouvre sur Windows Hello et lance la vérification immédiatement**, sans clic.
Sur une machine où Hello n'a jamais été associé à ce coffre, il bascule silencieusement vers le champ
mot de passe, sans message d'erreur : la tentative ne coûte rien.
à usage unique.

---

## Prise en main

### Option A — télécharger une version publiée

Deux builds sur la page [Releases](../../releases). C'est la même application ; prenez
celui qui correspond à votre façon de travailler.

| | `Tessera.Setup.exe` | `Tessera.exe` |
|---|---|---|
| | **Programme d'installation** | **Portable** |
| Emplacement d'installation | vous le choisissez | là où vous placez le fichier |
| Raccourci menu Démarrer / bureau | oui | non |
| Apparaît dans *Applications et fonctionnalités* | oui | non |
| Droits administrateur | pas nécessaires (installation par utilisateur) | pas nécessaires |
| Fonctionne depuis une clé USB | non | oui |
| La mise à jour peut revenir en arrière | non | oui |

Aucun des deux n'écrit quoi que ce soit en dehors de votre profil utilisateur.
**La désinstallation ne supprime pas votre coffre** — voir
[Où vivent vos données](#où-vivent-vos-données).

> Au premier lancement, Windows affichera « Windows a protégé votre ordinateur ». C'est
> SmartScreen qui réagit à un certificat auto-signé, pas un avertissement de virus.
> Cliquez sur **Informations complémentaires → Exécuter quand même**. Cela n'arrive
> qu'une fois ; les mises à jour intégrées ne le déclenchent jamais.

Les deux builds se mettent à jour eux-mêmes, chacun restant dans sa catégorie.

### Option B — lancer depuis les sources

```bash
git clone <this repo>
cd Tessera

# Côté Python — toutes les dépendances d'exécution sont déclarées dans pyproject
pip install -e .

# Côté UI (node_modules n'est pas versionné)
cd modules/file_vault/ui && npm install && cd ../../..

# Lancer
python scripts/run.py run file_vault
```

Cette dernière commande exécute `npm run dev` dans `ui/`, qui démarre Vite plus une
fenêtre Electron. Le processus principal Electron lance `backend_server.py` comme
processus enfant Python de longue durée et communique avec lui en NDJSON sur
stdin/stdout ; fermer la fenêtre l'arrête.

Extra optionnel — le déverrouillage par empreinte nécessite les liaisons Windows Hello :

```bash
pip install winrt-Windows.Security.Credentials.UI winrt-Windows.Foundation
```

---

## Premier lancement : l'assistant de configuration

Au premier lancement, l'assistant vous guide, dans l'ordre :

1. **Définir un mot de passe** — il enveloppe votre clé maîtresse d'identité.
2. **Lier une application d'authentification** — scannez un QR code, confirmez un code.
3. **Lier le déverrouillage par empreinte** — *ignoré automatiquement* si ce PC n'a pas
   Windows Hello.
4. **Configurer l'e-mail** — optionnel ici, ajoutable plus tard dans les Paramètres. Voir
   ci-dessous.
5. **Générer une clé de récupération** — affichée une seule fois. Notez-la.

Ensuite, chaque lancement ouvre l'écran de connexion ; validez un seul facteur et vous
êtes dedans.

---

## Configurer la connexion par code e-mail

C'est le chemin de déverrouillage « envoie-moi un code à 6 chiffres par e-mail ». Il
fonctionne en envoyant du courrier **depuis votre propre boîte mail, via votre propre
compte SMTP** — il n'y a aucun serveur Tessera au milieu, c'est pourquoi vous devez
fournir des identifiants SMTP.

### Étape 1 — obtenir un mot de passe spécifique à l'application auprès de votre
fournisseur de messagerie

Vous ne pouvez presque certainement pas utiliser votre mot de passe de connexion normal.
Les fournisseurs exigent un mot de passe d'application dédié (Gmail) ou un code
d'autorisation (QQ, 163) pour les clients tiers :

| Fournisseur | Où obtenir l'identifiant |
|---|---|
| **Gmail** | Activez la validation en 2 étapes, puis Compte Google → Sécurité → Mots de passe des applications. Vous obtenez un code de 16 caractères ; les espaces affichés par Google sont là pour la lisibilité et **ne font pas partie** du mot de passe. Google a retiré « l'accès moins sécurisé des applications » en 2022, donc un mot de passe d'application (ou OAuth) est le seul moyen. |
| **QQ Mail** | Paramètres → Compte → activez le service POP3/IMAP/SMTP, puis générez un 授权码 (code d'autorisation). Utilisez-le, pas votre mot de passe QQ. |
| **163 Mail** | Paramètres → POP3/SMTP/IMAP → activez le service et définissez un 授权码. Même principe. |
| **Outlook.com / Microsoft 365** | **Non pris en charge.** Microsoft a désactivé l'authentification de base pour SMTP AUTH en 2026 ; ces comptes exigent désormais OAuth 2.0 (XOAUTH2), que cette application n'implémente pas. Utilisez une autre boîte mail pour le code, ou reposez-vous sur les quatre autres méthodes de déverrouillage. |

### Étape 2 — remplir les paramètres

Dans l'étape *Configuration de l'e-mail et du courrier sortant* de l'assistant, ou plus
tard via **Paramètres → Mettre à jour l'e-mail et le courrier sortant** :

| Champ | Signification | Valeur typique |
|---|---|---|
| Adresse e-mail de récupération | Où le code est envoyé (peut être la même boîte) | `vous@exemple.com` |
| Serveur SMTP | L'hôte sortant de votre fournisseur | `smtp.gmail.com` · `smtp.qq.com` · `smtp.163.com` |
| Port SMTP | `465` pour SSL, `587` pour STARTTLS | `465` ou `587` |
| Compte d'envoi | L'adresse complète depuis laquelle le courrier est envoyé | `vous@gmail.com` |
| Mot de passe spécifique à l'application | L'identifiant de l'étape 1 — *pas* votre mot de passe de connexion | mot de passe d'application à 16 caractères / 授权码 |
| Utiliser SSL | Cochez pour le port **465** ; laissez décoché pour **587** (STARTTLS) | — |

> Dans les Paramètres, laisser le champ mot de passe vide conserve celui déjà enregistré.

### Étape 3 — envoyer un e-mail de test

Cliquez sur **Envoyer un e-mail de test** et confirmez qu'il arrive bien. S'il n'arrive
pas, l'erreur est affichée textuellement — les causes habituelles sont la mauvaise
combinaison port/SSL, l'utilisation du mot de passe de connexion au lieu du mot de passe
d'application, ou le SMTP pas encore activé sur le compte.

### Comportement à la connexion

Choisissez l'onglet **E-mail** sur l'écran de connexion et demandez un code. Le code fait
6 chiffres, valable **10 minutes**, utilisable **une seule fois**, et n'est conservé que
dans la mémoire du processus backend — il n'est jamais écrit sur le disque.

---

## Configurer l'application d'authentification

Scannez le QR code de l'assistant avec Google Authenticator, Microsoft Authenticator,
Authy, ou toute application TOTP standard, puis saisissez le code à 6 chiffres actuel
pour confirmer la liaison. La graine peut aussi être saisie manuellement si vous ne
pouvez pas scanner. La graine est stockée dans le Gestionnaire d'identification Windows,
pas dans le dépôt.

## Configurer le déverrouillage biométrique

Ceci appelle le vrai capteur Windows Hello. Nécessite les deux paquets `winrt-*` ci-dessus
plus un PC avec Windows Hello configuré. Si Hello n'est pas disponible, l'assistant ignore
cette étape et les quatre autres méthodes ne sont pas affectées.

Tessera demande à Windows quels capteurs biométriques cette machine possède réellement et
retient le premier disponible dans l'ordre **visage → empreinte → iris**. L'onglet est
libellé et illustré en conséquence : une machine dotée d'une caméra infrarouge affiche
« Visage », une machine avec un simple lecteur affiche « Empreinte ». Dessiner un visage sur
une machine sans caméra laisse l'utilisateur attendre une invite qui ne viendra jamais.

**Partout où un mot de passe est demandé, l'invite biométrique démarre d'elle-même** :
l'écran de connexion, la boîte de réauthentification qui protège les Réglages, et les
vérifications à l'entrée de chaque module. Vous n'avez rien à cliquer d'abord. Si cette
machine ne peut pas utiliser la biométrie, ou si ce coffre n'a aucune liaison biométrique,
aucune invite n'apparaît et l'onglet mot de passe s'ouvre à la place : pas de message
d'erreur pour une fonction que vous n'avez jamais configurée, et pas d'invite Hello vouée à
l'échec.

Windows Hello ne permet pas à une application de choisir *quelle* modalité utiliser — le
système décide selon ce que vous avez enregistré. Cette chaîne détermine ce que Tessera vous
montre, quel facteur elle démarre automatiquement, et vers quoi elle se replie.

## Clé de récupération — à lire absolument

La clé de récupération s'affiche **une seule fois**, pendant la configuration.
Sauvegardez-la en lieu sûr et *pas* à côté de vos fichiers chiffrés.

Si vous perdez votre mot de passe, perdez le téléphone avec votre authentificateur, et
que la machine avec votre empreinte liée meurt — la clé de récupération est le seul
moyen d'entrer qui reste. Si elle aussi est perdue, **les fichiers ne peuvent pas être
récupérés.** C'est la conception voulue, pas un bug.

---

## Utilisation au quotidien

- **Chiffrer** — choisissez un ou plusieurs fichiers, choisissez un algorithme, appuyez
  sur Chiffrer. Un seul fichier ouvre une boîte de dialogue Enregistrer sous ; plusieurs
  fichiers demandent un dossier cible et écrivent `<nom>.ivault` pour chacun. Un échec
  sur un fichier n'annule pas les autres ; vous obtenez un résumé à la fin.
- **Supprimer l'original** — désactivé par défaut pour que vous ne perdiez pas de
  données par accident. Mais le chiffrement ne sert à rien si le texte en clair reste à
  côté du texte chiffré, donc cochez **supprimer l'original de façon sécurisée après
  chiffrement** quand le but est la confidentialité. Cette suppression écrase d'abord
  avec des octets aléatoires et contourne la Corbeille. (Best-effort : le nivellement
  d'usure des SSD signifie que ce n'est pas une garantie contre la récupération
  légale.)
- **Recherche de résidus** — après chiffrement, l'application recherche les résidus en
  clair : sauvegardes d'éditeur (`~$…`, `.bak`, `.wbk`, dossiers d'auto-sauvegarde WPS)
  et copies homonymes ailleurs sur le disque. Les éléments liés au fichier source sont
  précochés ; les correspondances homonymes à l'échelle du disque ne sont **jamais**
  précochées, car l'une d'elles pourrait être un fichier que vous vouliez garder. Vous
  pouvez relancer ceci à tout moment sur un fichier `.ivault`.
- **Verrouiller** — fermer la fenêtre la cache dans la zone de notification. La clé
  maîtresse déverrouillée n'existe que dans la mémoire du processus backend et meurt
  avec lui.

---

## Nettoyer l'ordinateur

Cinq panneaux sous **Nettoyage du PC** dans la barre latérale. Ils fonctionnent avec les
permissions d'un utilisateur ordinaire ; seules quelques opérations demandent les droits
administrateur, et un petit processus assistant apparaît alors, fait ce travail précis et
se ferme. Tessera lui-même ne s'exécute jamais en tant qu'administrateur — il détient du
matériel de clé déchiffré, et un processus élevé de longue durée le détenant serait une
prise bien plus intéressante qu'un processus ordinaire.

### Bilan

Un bouton analyse neuf catégories à la fois et affiche chacune dès qu'elle arrive. Sur une
machine normale, les premiers résultats sont à l'écran en bien moins d'une seconde ; les
tâches planifiées prennent de cinq à neuf secondes et arrivent en dernier, parce qu'un
processus non élevé ne peut pas aller plus vite — Windows ne le laisse même pas lister le
dossier.

Deux choses restent délibérément derrière leur propre bouton plutôt que dans cette analyse :
l'analyse du magasin de composants et la mesure de chaque dossier de premier niveau du
disque système. Chacune prend environ une minute, et les inclure dans le bilan en un clic
en faisait une attente de deux minutes.

Sous **Options d'analyse**, vous réglez depuis combien de temps un fichier doit être
inutilisé pour compter comme inutile. Un jour par défaut, volontairement prudent : une
installation en cours écrit dans le dossier temporaire. Sur une machine qui vient de
redémarrer, régler sur *Sans condition d'âge* trouve généralement plusieurs fois plus.

### Espace disque

- **Disque système** — fichier de mise en veille prolongée, magasin de composants, points
  de restauration, `Windows.old`, et ce qu'occupe le fichier d'échange. Ce dernier est
  affiché pour information et ne peut pas être sélectionné : le désactiver fait planter
  les programmes quand la mémoire manque, au lieu de les ralentir.
- **Gros fichiers** — les plus gros d'abord, dans les dossiers et au-dessus de la taille
  que vous choisissez.
- **Espace par dossier** — quels dossiers occupent la place, un niveau à la fois.
- **Doublons** — comparés par taille, puis sur un échantillon pris en tête et en queue de
  chaque fichier, puis intégralement. Dans chaque groupe, la copie au chemin le plus court
  est conservée pour vous et le reste est présélectionné.
- **Disques** — une optimisation qui lit d'abord le type de média. Tessera refuse de
  défragmenter un SSD même si on le lui demande : il n'y a rien à y gagner et cela l'use.

### Démarrage et menus

Tout ce qui se lance à l'ouverture de session, rassemblé depuis quatre endroits : le
registre, le dossier Démarrage, les tâches planifiées et les services. Les outils qui ne
lisent que le registre annoncent une poignée d'entrées alors que la machine met toujours
une minute à démarrer.

Rien n'est supprimé ici. Les entrées de registre sont déplacées vers une clé de sauvegarde,
les raccourcis de démarrage renommés, les tâches planifiées désactivées via le planificateur,
et les services passés en démarrage manuel plutôt que désactivés — en désactiver un fait
échouer au démarrage tout ce qui en dépend. Chacun revient en un clic.

Le bloqueur de fenêtres publicitaires agit à deux niveaux : il désactive ce qui les produit
au démarrage, et ferme par `WM_CLOSE` les fenêtres correspondant à une règle. Il ne termine
jamais un processus, ne touche jamais la fenêtre dans laquelle vous travaillez, et
journalise tout ce qu'il a fermé.

### Logiciels installés

Tout ce qui est installé, lu depuis les trois emplacements du registre — 64 bits, 32 bits
et par utilisateur. Ne lire que le premier fait manquer plus de la moitié.

Les tailles sont mesurées depuis le dossier d'installation plutôt que reprises de ce que
l'installateur a déclaré. La désinstallation lance le programme de désinstallation du
logiciel ; Tessera ne supprime jamais un dossier d'installation lui-même, ce qui laisserait
derrière services, pilotes et entrées de registre.

L'onglet registre dit clairement que nettoyer les références obsolètes **ne rendra pas
votre PC plus rapide**. C'est utile pour la propreté. La seule catégorie qui aide vraiment
est celle des enregistrements de composants pointant vers des DLL disparues.

### Boîte à outils

Dix-huit petits utilitaires, chacun avec une ligne disant quand vous en auriez besoin :
vider le cache DNS, voir quel programme occupe un port, découvrir ce qui bloque un fichier,
redémarrer l'Explorateur, reconstruire le cache d'icônes, trouver les dossiers vides,
trouver les chemins trop longs pour être copiés, renommer en lot avec un aperçu préalable.

Le fichier hosts est affiché en lecture seule. Le modifier permet de rediriger n'importe
quel site, et ce n'est pas une capacité qu'un coffre chiffré doit détenir en votre nom.


## Synchronisation multi-appareils

Votre téléphone, votre tablette et ce PC partagent un presse-papiers et un point de dépôt de
fichiers. Copiez une formule sur le téléphone, collez-la dans Word sur le PC. Déposez une
vidéo de 4 Go sur le PC, récupérez-la sur la tablette. Rien ne transite par un serveur : les
appareils dialoguent directement sur votre réseau local, chiffrés de bout en bout.

### Appairer un appareil

Ouvrez **Multi-appareils → Ajouter un appareil**, puis au choix :

- **Afficher mon code** — un QR code apparaît. Scannez-le depuis l'autre appareil.
- **Saisir leur code** — collez le code affiché sur l'autre appareil.
- **Appareils à proximité** — la liste des appareils non appairés sur ce réseau. Choisissez-en un
  et appairez, sans transmettre le moindre code. Les noms y sont marqués *non vérifié* : un nom
  d'appareil est ce que son propriétaire a saisi ; c'est la vérification du code de sécurité
  ci-dessous qui établit réellement l'identité.
- **Saisir une adresse** — la solution de repli quand rien d'autre ne marche, et la plus fiable.
  L'onglet affiche l'adresse de cet ordinateur (`192.168.1.7:52140`) à dicter, et un champ pour
  celle de l'autre. Saisissez `IP:port` ou uniquement une IP : dans ce dernier cas, Tessera
  interroge la balise de découverte de l'appareil pour connaître son port courant. Deux appareils
  qui se joignent par IP peuvent s'appairer ainsi : même sous-réseau non requis, et cela fonctionne
  là où le multicast est bloqué.

**Si la liste des appareils à proximité reste vide, appuyez sur « Analyser le réseau » plutôt que
de réappuyer sur « Actualiser ».** Actualiser ne fait que relire ce qui a déjà été entendu, et
beaucoup de routeurs suppriment les paquets multicast dont dépend la découverte automatique : dans
ces réseaux, actualiser cent fois n'affiche toujours rien. L'analyse interroge directement chaque
adresse du sous-réseau et contourne le problème.

Un bandeau jaune en haut de la page signifie que le pare-feu Windows bloque le service de
synchronisation : les autres appareils ne voient pas cet ordinateur. Appuyez sur **Autoriser** —
une seule élévation, et la règle couvre tous les profils réseau de Windows, mais seulement le
programme de synchronisation de Tessera : il répond uniquement aux sondes de découverte et aux
négociations QUIC chiffrées, et sans appairage rien ne peut être lu. Windows supprime ces paquets
en silence : aucune erreur, aucune entrée de journal, rien qu'une liste vide — d'où l'intérêt de
l'afficher plutôt que de laisser deviner. Le **diagnostic de connexion** sous la liste parcourt
chaque étape — écoute, adresses, annonce, port de réponse, découverte, analyse active — et indique
laquelle échoue et quoi faire.

Les deux écrans affichent ensuite les mêmes six chiffres et six émojis. **Comparez-les.**
S'ils correspondent, confirmez des deux côtés ; s'ils diffèrent, quelqu'un s'est interposé —
choisissez « Ils ne correspondent pas » et l'appairage est abandonné. Un code d'appairage ne
sert qu'une fois et expire au bout d'une minute : une capture d'écran d'un ancien code ne sert
à personne.

À l'appairage, vous choisissez de quel type d'appareil il s'agit :

| | Permanent | Temporaire |
|---|---|---|
| Destiné à | Vos propres appareils | Ceux de quelqu'un d'autre — un ami, un PC de salle de classe |
| Reconnexion automatique | Oui | Non |
| Synchronisation du presse-papiers | Oui | Non |
| Expiration | Jamais, jusqu'à révocation | 10 min / 30 min / 1 heure / 1 jour |
| Identifiants sur le disque | Stockés, chiffrés | Jamais écrits |

Révoquez n'importe quel appareil à tout moment. La révocation est immédiate et définitive :
les anciens identifiants cessent de fonctionner dès la tentative de connexion suivante, pas au
prochain redémarrage.

### Copier et coller

Tout ce que vous copiez part vers vos appareils permanents et atterrit dans leur presse-papiers
système, prêt à être collé. Texte, texte enrichi et images voyagent dans la représentation que
chaque application réceptrice sait utiliser ; les formules et symboles physiques restent intacts
— `E = mc²`, `m/s²`, `θ̇`, `∑∫√`. Seul le dernier élément du presse-papiers est synchronisé : une
ancienne copie ne peut donc pas arriver en retard et remplacer ce que vous venez de copier.
Windows vérifie toutes les 200 ms ; sous Android 10 et suivants, le système n'autorise Tessera à
lire le presse-papiers qu'au premier plan : après une copie dans une autre application Android,
ouvrez Tessera une fois pour la synchroniser. Le contenu reçu est toujours écrit dans le
presse-papiers Android.

Les grandes images ne sont pas poussées ; l'autre appareil ne les récupère que si vous collez
réellement.

### Envoyer des fichiers

Sélectionnez un ou plusieurs appareils dans la liste, puis **Envoyer des fichiers**. Vous pouvez
choisir explicitement une ou plusieurs destinations ; fichiers, dossiers, images et paquets
d'application sont identifiés dans la liste des transferts. Chaque appareil obtient son propre
transfert indépendant : un téléphone lent ne retarde jamais un PC rapide, et un appareil qui
refuse ou échoue n'annule pas les autres.

Chaque transfert actif affiche son nom, sa progression, sa vitesse et le temps restant estimé.
Les deux côtés peuvent le mettre en pause, le reprendre ou l'annuler. La pause conserve les blocs
déjà vérifiés ; après une interruption ou une reconnexion, le transfert repart de là, sans tout
recommencer. Chaque fichier est vérifié à l'arrivée — un transfert dont la vérification échoue est
signalé comme échoué, jamais comme « terminé ».

Les exécutables reçus (`.exe`, `.msi`, `.apk`) sont enregistrés comme de simples fichiers.
Tessera n'exécute et n'installe jamais ce qu'il reçoit.

### Rester connecté

Les appareils qui décrochent — téléphone verrouillé, portable en veille, changement de Wi-Fi,
routeur redémarré — reviennent d'eux-mêmes. Pas de bouton « reconnecter », pas besoin de
rescanner quoi que ce soit.

La reconnexion essaie toutes les routes en même temps (dernière adresse connue, adresses vues
précédemment, et ce que la découverte du réseau local rapporte à l'instant) et garde celle qui
répond en premier. Sur un réseau local, cela prend généralement bien moins d'une seconde. Comme
la découverte est vivante, un appareil qui a changé d'adresse IP — nouveau Wi-Fi, nouveau bail
DHCP, autre sous-réseau — est retrouvé quand même.

### Ce que contient cette version

La partie Windows est complète et incluse dans cette publication : le service de
synchronisation est intégré à l'exe et démarre avec l'application.

**L'application Android est construite à partir du même cœur de protocole**, compilé en
bibliothèque native (`tesseracore.aar`) via `gomobile bind` : l'appairage, le chiffrement, la
machine à états de reconnexion et la reprise de transfert sont littéralement le même code que
sous Windows, et non une réimplémentation qui finira par diverger. Le côté Android n'ajoute que
ce qui est réellement spécifique à la plateforme : un service de premier plan pour garder le
processus vivant, un verrou multicast Wi-Fi pour que la découverte mDNS continue de fonctionner
écran éteint, et la lecture/écriture du presse-papiers système.

L'énumération des interfaces réseau fait partie de ces éléments propres à la plateforme. Android
bloque l'appel noyau que la bibliothèque standard de Go utilise pour cela ; le client Android lit
donc la liste via l'API Java et la transmet au cœur partagé avant le démarrage du nœud, puis à
chaque changement de réseau. Sans cette étape, tous les chemins de découverte — appareils à
proximité, balayage du réseau local, adresse de cet appareil pour l'appairage manuel — reviendraient
vides sans rien pour l'expliquer. Le diagnostic de connexion sous **Appareils à proximité** affiche
le nombre d'interfaces en premier pour la même raison : c'est la première chose à écarter.

L'écran est composé de sections repliables. Ce dont vous ne vous servez pas — le code d'appairage,
les mises à jour, le verrou de l'application, le journal des incidents — se réduit à une ligne de
titre et le reste à la prochaine ouverture. La liste des appareils appairés et les actions d'envoi
ne se replient pas : l'écran existe pour elles. Des mesures en direct sont également disponibles —
appareils en ligne, latence du lien, vitesse actuelle, totaux de la session, port d'écoute, adresses
locales, interfaces disponibles — lues dans le flux d'événements qui arrive déjà, sans interrogation
supplémentaire en arrière-plan, et le minuteur ne tourne pas tant que la section est fermée.

Le client Android couvre l'appairage par scan de QR code, la vérification du code de sécurité,
la liste des appareils, l'envoi/réception du presse-papiers, le choix d'une ou plusieurs
destinations pour les fichiers, les contrôles de transfert et le choix de l'emplacement où
enregistrer les fichiers reçus. Il permet aussi de sélectionner et de révoquer plusieurs appareils
appairés à la fois. Les appareils proches déjà appairés et en ligne sont signalés comme tels, sans
demander de les appairer de nouveau. La reconnaissance des codes-barres utilise un modèle embarqué
plutôt que téléchargeable : scanner pour appairer arrive le plus souvent sur un téléphone fraîchement
installé et pas encore connecté — précisément quand un modèle téléchargé à la première utilisation
est indisponible.

L'application Android vérifie aussi ses propres mises à jour auprès des releases de ce dépôt. La
vérification automatique est activée par défaut : elle a lieu une fois à l'ouverture et ne vous
sollicite que si une version plus récente existe réellement. Désactivez-la et rien n'est vérifié
tant que vous n'appuyez pas sur **Vérifier les mises à jour**. Seule la *vérification* est
automatique : Android n'autorise pas une application installée hors magasin à installer un paquet
silencieusement, donc la dernière étape reste toujours la boîte de dialogue du système, que vous
confirmez vous-même. Le téléchargement n'est accepté que depuis github.com, et c'est le système
qui impose que le nouveau paquet porte la même clé de signature que celui déjà installé.

Il a été compilé et signé mais **pas** exécuté sur un appareil physique ici : considérez la
première installation comme un essai.

### Sécurité

Chaque appareil détient sa propre paire de clés à long terme, générée au premier lancement et
conservée dans la DPAPI de Windows — jamais un numéro de série matériel, un IMEI ou une adresse
MAC, car ceux-là ne peuvent pas être révoqués. Être sur le même réseau n'accorde rien : chaque
connexion revérifie l'identité par rapport à la clé que vous avez confirmée lors de
l'appairage, et un appareil dont la clé change soudainement est refusé bruyamment plutôt
qu'accepté en silence.

Chaque connexion négocie des clés jetables neuves : enregistrer le trafic d'aujourd'hui puis
voler une clé d'appareil plus tard ne permet toujours pas de le déchiffrer. Les chemins des
fichiers entrants sont contrôlés avant toute écriture — les échappements `../`, les chemins
absolus et les noms réservés de Windows sont rejetés d'emblée.

---

### Connecter vos appareils

Sur l'ordinateur, ouvrez **Multi-appareils → Appareils et synchro → Générer un code
d'appairage** ; sur le téléphone, touchez **Scanner pour appairer** et visez le QR code.
Les deux écrans affichent alors les mêmes six chiffres et six émojis — vérifiez qu'ils
correspondent, confirmez des deux côtés, c'est fait.

Les deux extrémités peuvent choisir où enregistrer les fichiers reçus, et renommer un appareil
se propage à tous les appareils appairés en une dizaine de secondes, sans reconnexion.

Si un scan semble bloquer, la cause habituelle est que les deux appareils ne sont pas sur le
même réseau, ou que le Wi-Fi applique l'isolation AP (fréquent en entreprise, sur les campus
et à l'hôtel). Un partage de connexion depuis le téléphone est le moyen le plus rapide de
trancher. Guide complet :
[guide de connexion](modules/file_vault/crossdevice/docs/06-连接指南.md).

### Progression des transferts

Une carte de progression apparaît dès qu'un transfert démarre : nom du fichier, appareil
cible, pourcentage, octets transférés sur le total, une ligne par fichier. Elle se rafraîchit
toutes les 250 ms plutôt qu'à chaque bloc : un bloc fait 4 Mo, soit des dizaines par seconde
en gigabit, et repeindre à chaque bloc fait saccader la barre au lieu de la faire avancer.

### Journal de diagnostic

Le journal de diagnostic a sa propre entrée dans la barre de navigation, et le même journal
figure aussi dans les réglages : un seul interrupteur, un seul contenu, accessible des deux
côtés. Il est **désactivé par défaut**. Activé, il
n'enregistre que ce qui a échoué : un raccourci resté sans effet, un appareil injoignable,
un service qui n'a pas démarré. Il n'enregistre jamais ce que vous faisiez : ni
presse-papiers, ni noms de fichiers, ni chemins, ni clés.

Il est délibérément en clair plutôt que chiffré, car le cas qui réclame le plus un journal
est celui où le coffre refuse de s'ouvrir — et un journal chiffré est alors précisément
illisible. C'est aussi pourquoi il doit être désactivé par défaut. Un bouton efface tout.

### Guides

- [Guide bureau](docs/电脑端操作指南.md) — tous les raccourcis, appairage, envoi par glisser-déposer, dépannage
- [Guide Android](docs/安卓端操作指南.md) — appairage, menu de partage, survie en arrière-plan, verrouillage de l'application
- [Référence connexion](modules/file_vault/crossdevice/docs/06-连接指南.md) — ce qu'une connexion exige et comment diagnostiquer

### Ce dont la connexion a besoin

Les deux appareils doivent être sur le même réseau local — même Wi-Fi, ou un ordinateur en
Ethernet derrière le même routeur. Rien d'autre n'est requis : pas de compte, pas de cloud,
pas de redirection de port.

Le port d'écoute n'est pas figé. Si celui par défaut est déjà pris, le service se rabat sur un
port attribué par le système ; le code d'appairage et l'enregistrement mDNS annoncent tous deux
le port réellement utilisé, donc rien en aval ne s'en soucie.

Un VPN ou un proxy sur l'ordinateur ne pose pas de problème : les interfaces sans multicast
(ce qu'est précisément un tunnel VPN) sont ignorées lors de l'annonce des adresses, si bien que
le téléphone ne reçoit jamais une adresse de tunnel qu'il ne peut pas joindre.

Ce que l'application ne peut pas contourner, c'est **l'isolation AP** : beaucoup de réseaux
d'entreprise, de campus et d'hôtel interdisent aux appareils d'un même Wi-Fi de communiquer.
Un partage de connexion depuis le téléphone est le moyen le plus rapide de le confirmer, et
aussi de le contourner.


## Modèle de sécurité

Chiffrement en enveloppe à deux couches :

```
                 ┌─ mot de passe ──────┐
                 ├─ code e-mail ───────┤
Clé Maîtresse    ├─ TOTP ──────────────┤  chacun enveloppe l'IMK indépendamment
d'Identité (IMK) ┼─ Windows Hello ─────┤
                 └─ clé de récupération┘
                          │
                          ▼
   clé par fichier (FK), nouvelle pour chaque fichier, chiffre le contenu par blocs
   la FK est ensuite enveloppée par l'IMK et stockée dans l'en-tête du .ivault
```

- La couche identité (enveloppe de l'IMK, emplacements de clés) est toujours
  AES-256-GCM ; votre choix d'algorithme n'affecte que le contenu des fichiers.
- Les emplacements du mot de passe et de la clé de récupération dérivent leur clé
  d'enveloppe avec **scrypt à N=2¹⁷, r=8** (le minimum actuellement recommandé par
  l'OWASP) : environ 134 Mo de RAM par tentative, ce qui rend une ferme de GPU ou
  d'ASIC impraticable — des milliers de cœurs parallèles ne servent à rien si chacun
  réclame ses propres 134 Mo. Chaque emplacement enregistre le coût avec lequel il a
  été scellé : relever ce coût plus tard ne verrouille donc jamais un coffre existant,
  et un emplacement resté sur un ancien coût est ré-enveloppé silencieusement lors du
  déverrouillage suivant.
- Tous les sels et toutes les clés sont générés sur la machine de l'utilisateur
  (`os.urandom`) lors de la configuration — **aucun secret n'est intégré à l'exe** :
  le même téléchargement entre mille mains produit mille coffres sans aucun lien.
- Les bases de données presse-papiers / notes / historique IA sont en SQLCipher, avec
  une clé dérivée par HKDF de l'IMK — pas de second mot de passe à retenir, mais elles
  sont illisibles tant que le coffre est verrouillé.
- Le processus de rendu n'a aucun accès Node ni système de fichiers
  (`contextIsolation: true`, `nodeIntegration: false`) ; il ne peut appeler le backend
  que via une API preload étroite.
- **Un seul facteur suffit à déverrouiller**, donc la force globale est celle du facteur
  le plus faible. Un mot de passe divulgué contourne les quatre autres. Exiger plusieurs
  facteurs simultanément n'est pas pris en charge dans cette version.

---

## Où vivent vos données

| Quoi | Où |
|---|---|
| Identité, bases de données du coffre, pièces jointes | `%LOCALAPPDATA%\Tessera\data` *(configurable — voir ci-dessous)* |
| Graine TOTP, clé e-mail, mot de passe SMTP | Gestionnaire d'identification Windows |
| Clé maîtresse déverrouillée | Mémoire du processus backend uniquement — jamais sur disque |

> **Vous venez d’une version nommée « Ideal1 File Vault » ?** Votre coffre migre tout seul. Au premier lancement, l’application cherche l’ancien dossier `%LOCALAPPDATA%\Ideal1 File Vault\` et le migre avec la même procédure transactionnelle que tout autre déplacement : instantanés SQLite cohérents, vérification puis renommage atomique, et **l’ancien dossier reste intact**. Un emplacement personnalisé choisi lors de l’ancienne installation est également repris.

Rien n'est téléversé nulle part. Les seules connexions sortantes sont celles que vous
configurez : votre propre serveur SMTP pour les codes de connexion, votre point de
terminaison IA choisi si vous utilisez l'assistant, les cibles du test de vitesse, et
GitHub Releases pour les vérifications de mise à jour.

Les données d'identité sont liées à une seule machine et ne se transfèrent pas
automatiquement. `data/` est ignoré par git et jamais versionné.


### Choisir l'emplacement du coffre

L'emplacement par défaut convient à la plupart des gens. Vous pouvez le placer ailleurs
— un disque que vous sauvegardez vraiment, un volume chiffré, un second disque — de
trois façons, par ordre de priorité :

1. **La variable d'environnement `IDEAL1_FILE_VAULT_DATA_DIR`.** Prioritaire sur tout,
   valable pour cette exécution seulement, et elle désactive tout déplacement
   automatique : la définir signifie que vous gérez ce dossier vous-même.
2. **L'installateur.** `Tessera.Setup.exe` demande un dossier de données sur
   sa propre page, juste après avoir demandé où installer le programme. Ce sont deux
   décisions distinctes : réinstaller ailleurs est trivial, déplacer un coffre rempli
   depuis six mois ne l'est pas.
3. **Paramètres → À propos → Changer d'emplacement.** Choisissez un dossier et
   redémarrez. Le déplacement a lieu au démarrage suivant, *avant* que quoi que ce soit
   n'ouvre le coffre : copier une base SQLite chiffrée alors que ses connexions sont
   ouvertes perdrait silencieusement ce qui reste dans le journal d'écriture anticipée.
   L'ancien dossier reste intact ; un déplacement raté ne coûte rien.

La désinstallation ne supprime jamais le dossier de données.

---

## Développement

```
core/       infrastructure partagée : config, journalisation, registre de modules, aides subprocess
modules/    modules de fonctionnalités enfichables — un dossier = un scénario autonome
scripts/    point d'entrée CLI : run.py (list / run <module>)
docs/       notes d'architecture, spécifications de conception, procédure de release
tests/      tests de fumée
```

Règles maison — la version complète est dans [`CLAUDE.md`](CLAUDE.md) et
[`modules/README.md`](modules/README.md) :

- Python ≥ 3.9. Utilisez `python -X utf8 …` quand la sortie n'est pas en ASCII.
- Les modules ne s'importent jamais entre eux ; ils ne dépendent que de `core/`. La
  logique partagée est promue vers `core/`.
- Les modules peuvent mélanger du JavaScript ou du C, invoqués via `run_command()` de
  `core/proc.py`.

Ajouter un scénario : copiez `modules/_template/` → `modules/<name>/`, éditez
`MODULE_META`, implémentez, puis confirmez avec `python scripts/run.py list`.

```bash
python -m pytest tests/                        # tests de fumée
python -m pytest modules/file_vault/tests/     # suite du coffre
cd modules/file_vault/ui && npm run lint       # lint UI
```

Plus de détails : [`docs/architecture.md`](docs/architecture.md) ·
[`modules/file_vault/README.md`](modules/file_vault/README.md)

---

## Construire une version

Les builds doivent tourner sur **Windows x64** — PyInstaller n'est pas un compilateur
croisé.

```powershell
cd modules\file_vault\ui
npm install
npm run build:standalone
```

Cela compile les aides C natives, gèle deux cibles PyInstaller, puis exécute
`tsc` + `vite build` + `electron-builder`, produisant
`ui/release/<version>/` — both `Tessera.exe` (portable) and
`Tessera.Setup.exe` (installer).

La signature est gérée par la compilation elle-même : elle crée le certificat au premier lancement, inscrit sa clé publique dans `signer-policy.json`, garde la clé privée **non exportable** dans votre magasin de certificats et écrit une sauvegarde protégée par mot de passe hors du dépôt. Rien à faire à la main ici. (`secure-signing-key.cmd`, à la racine du dépôt, peut reprotéger cette sauvegarde avec une phrase secrète à vous, si vous souhaitez qu'elle survive à la perte de cette machine.)

Pousser un tag `v*` lance le même build en CI — voir
[`.github/workflows/build-windows.yml`](.github/workflows/build-windows.yml). Le
workflow téléverse un exe **non signé** vers une release **draft** et s'arrête là. La
signature de code reste sur votre propre machine **par conception** : une clé de signature
conservée dans les secrets CI peut être utilisée par quiconque peut modifier un workflow,
ce qui réduirait la signature à une décoration.

Avant de publier votre propre fork, vous devez remplir deux emplacements réservés :

- `REPO` dans `modules/file_vault/ui/electron/updateService.ts` → votre `owner/repo`

C'est le seul à modifier à la main. Le certificat de signature est **créé pour vous** au
premier `npm run build:standalone`, et l'empreinte de sa clé publique est écrite dans
`signer-policy.json`, que vous n'avez plus qu'à committer. Chaque build ultérieur vérifie
que le certificat sur le point de signer est toujours celui qui est épinglé, et s'arrête
sinon — sans quoi une reconstruction sur une autre machine produirait silencieusement une
version que tous les clients installés refusent, sans que rien ne semble anormal avant
qu'un utilisateur ne lance la mise à jour.

**Le certificat n'a pas besoin de coûter de l'argent.** Intégrer la signature dans l'exe
et payer une autorité de certification sont deux choses indépendantes : le format
Authenticode ne se soucie pas de l'émetteur du certificat, et épingler la clé publique
côté client est en réalité *plus fort* que faire confiance à une AC — un attaquant aurait
besoin de votre clé privée, et non d'un certificat au même nom obtenu auprès de l'une des
centaines d'AC publiques. Ce que l'argent achète, c'est la réputation SmartScreen,
c'est-à-dire l'absence d'avertissement « Windows a protégé votre ordinateur ».

Si vous laissez `SIGNER_POLICY` vide, l'application fonctionne toujours — elle refuse
simplement de se mettre à jour et l'indique clairement.

Procédure complète, y compris ce qui se passe à l'expiration du certificat et pourquoi la
release en contient exactement deux :
[`docs/standalone-exe-release.md`](docs/standalone-exe-release.md)

---

## Licence

Tessera est sous **double licence**. Vous en choisissez une ; les deux ne sont
pas cumulatives.

| Vous êtes… | Licence | Coût |
|---|---|---|
| Utilisateur personnel, ou en interne dans votre entreprise, sans redistribution | **AGPL-3.0** | Gratuit |
| Lecteur, forkeur, contributeur publiant son fork | **AGPL-3.0** | Gratuit |
| Distributeur du logiciel (ou de code dérivé) dans un produit **propriétaire** | **Commerciale** | Payant |
| Fournisseur d'un **service hébergé** sans publier votre source | **Commerciale** | Payant |

**Édition communautaire — [GNU AGPL-3.0](LICENSE).** Utilisable pour tout usage, y
compris commercial. Seule obligation : si vous distribuez le logiciel, une version
modifiée, ou si vous en permettez l'usage **via un réseau**, ces utilisateurs doivent
pouvoir obtenir le code source complet correspondant, sous AGPL également. L'usage
personnel et l'usage interne en entreprise ne déclenchent ni l'un ni l'autre.

**Édition commerciale — [conditions](LICENSE-COMMERCIAL.md).** Le même logiciel, sans
l'obligation de divulgation du code, avec des garanties et un canal de support dont
l'édition AGPL se dégage explicitement. Contact : `<your-licensing-email>`.

L'arbre de décision, les conditions de contribution qui rendent la double licence
possible, et les deux points de l'AGPL que tout le monde comprend de travers :
[`LICENSING.md`](LICENSING.md).

Les composants tiers (Electron, React, SQLCipher, la bibliothèque Python `cryptography`,
tesseract.js et d'autres) conservent leurs propres licences — aucune des deux licences
ci-dessus ne les modifie, et rien ici ne restreint les droits que vous détenez à ce
titre. Inventaire complet : [`THIRD-PARTY-NOTICES.md`](THIRD-PARTY-NOTICES.md),
également consultable dans l'application sous **Paramètres → À propos**.

Les licences couvrent le code, pas le nom du projet ni son logo. Les forks sont les
bienvenus — donnez-leur un autre nom.
⁢‌‌​​‌‌‌⁠‌‌‌⁡‌​⁡‌​⁡​⁡‌⁡⁡​‌‌‌‌​⁡​⁠‌​‌⁠‌⁡​⁡‌⁠‌​‌​‌⁡‌‌‌⁠‌⁠⁠⁡‌‌⁠​​⁡​‌​⁡⁠​‌⁠⁠​‌⁠⁠⁡‌⁠⁡‌‌​⁡​‌​⁡​‌⁠​‌‌⁡‌⁠​⁡‌⁠‌‌⁠⁠‌‌​⁠‌⁡​‌‌‌‌​‌​‌⁠‌‌⁠​‌⁠‌⁡‌​​⁠‌​​‌‌⁡​​‌⁠⁡‌‌⁡‌⁠‌‌⁠⁠‌⁡​⁡‌​⁡⁡​⁠⁡⁡‌​⁡​​⁡​⁡‌⁠⁡​‌‌​‌‌‌​​‌​‌⁠‌⁠⁠​​⁡‌​‌​​⁠‌⁡⁠‌‌​​⁠‌‌​⁡​⁡⁠‌​⁡‌⁡‌‌​​‌‌‌​​⁡​⁠‌​⁡​‌⁠⁠⁡‌​​⁠‌​​‌‌​​⁡​⁡‌​‌‌​‌‌​⁠​‌⁠‌​‌⁠⁠⁡‌⁡⁠​‌⁡​⁡‌‌‌‌‌⁡‌⁡‌‌‌⁠‌⁡​​‌⁠​‌​⁡‌​‌‌‌​​⁡‌⁠​⁡​‌‌‌⁠​‌​⁠​‌⁠​⁡​⁡​​‌‌⁠​‌‌⁠‌‌⁠​⁡​⁡⁠​‌⁠⁠​‌‌‌⁠‌‌⁠​‌​⁠⁡​⁡‌⁡‌⁠‌⁡‌⁠⁠​‌​⁠⁠‌⁡‌‌‌⁠⁠⁡‌⁡‌‌‌⁠⁡​‌⁠⁠​‌‌⁠​‌​‌‌‌​‌‌​⁡⁠​‌‌⁠‌‌⁡‌​‌⁠‌‌‌⁠‌⁠‌⁠⁡‌‌​⁠⁠‌⁡⁠⁠‌‌‌⁠‌⁠⁡⁡‌‌​⁠‌⁠⁠⁡‌⁡‌​‌⁡​⁡‌‌​​‌⁡⁠​‌​​‌‌⁡​‌‌​‌⁠‌‌‌‌‌​⁡‌‌​⁡‌​⁡‌⁡‌⁠⁠⁡‌‌⁠​​⁠⁡⁡‌​‌​‌⁠‌​‌‌‌⁡‌‌​‌‌⁠​⁡​⁡‌‌‌​⁡⁡‌⁠⁡⁠‌⁠⁡​‌​​⁡​⁡‌​‌​​‌‌⁡⁠⁠‌⁠⁠‌‌⁠⁠​‌⁠⁡⁡​⁠⁡⁡‌​‌‌‌⁠⁠‌​⁡‌​‌​​⁡​⁡‌‌‌⁠⁡​‌​⁠⁡​⁡​⁠‌⁠⁡‌‌​⁡​‌⁡​​​⁡⁠​‌‌‌‌‌​⁠​‌⁠⁠⁠‌​‌⁡​⁠⁠⁡‌​⁠⁠‌​​⁠‌​​⁠‌⁠⁡⁠‌​⁡⁡‌⁠⁡​‌⁠‌​‌⁠‌⁠​⁡​‌​⁠⁡⁡‌​‌​​⁡‌⁡​⁡⁠‌‌⁠​⁠‌⁡​⁡‌⁠​‌​⁡​⁠​⁡​⁡‌⁡‌‌‌‌​‌‌‌​⁡‌​⁡‌‌‌​⁠‌⁡‌⁠‌⁡​‌‌⁠​⁠‌⁠‌‌‌​‌‌‌⁠⁡​‌​​⁠‌​⁠‌‌⁡‌‌‌⁡‌⁡‌⁠‌​‌‌​⁠​⁡​⁡‌⁠‌‌‌‌⁠‌​⁡⁠​‌⁠⁠⁠‌⁡​⁡‌‌‌​‌⁠⁠⁠‌​⁠‌‌⁠⁠⁡‌⁠​⁠‌⁠‌​​⁡‌‌‌⁠⁠⁠‌⁠⁠⁠‌⁠‌‌‌⁡‌​​⁡‌​‌⁠​⁡‌‌‌‌‌⁡‌​‌​⁠​‌⁡​‌‌​⁡​‌​⁡⁠‌⁠⁠⁠‌⁡⁠⁠‌⁡​​‌⁠⁠‌‌⁠‌​​⁡‌⁡‌‌‌⁠‌‌⁠​​⁡​⁡​⁡‌‌‌​⁡⁡​⁡‌⁠‌⁡⁠​‌⁠​⁡‌​⁡​​⁡​⁡‌​‌⁠‌‌⁠​‌⁠⁡⁠‌​​‌‌⁠‌⁠‌⁠‌​‌⁠⁠​​⁡‌​‌⁡​⁠​⁡‌‌​⁠⁠⁡‌​​‌‌⁠⁠‌​⁡​‌​⁡​​‌⁠⁠⁡‌⁠​‌​⁡‌⁡‌​‌⁠​⁡​⁡‌‌⁠‌‌⁡‌‌‌‌‌⁠​⁡​⁠‌⁠⁠​‌​⁠​‌‌​‌‌⁠⁡⁠‌⁠‌​‌⁡⁠⁠​⁡‌⁠‌⁡​​‌​⁠⁡​⁡​⁡​⁡‌⁡​⁡⁠​‌⁠⁠⁠‌​⁠‌‌⁠​⁠​⁠⁠⁡​⁡‌⁠‌​⁠​‌​‌​‌‌​⁡‌⁡‌⁠‌⁡‌​‌⁠​⁠​⁡‌‌​⁡​‌‌‌‌‌‌​​⁠‌⁡​‌‌‌⁠⁠‌‌⁠⁠‌⁡⁠​‌⁡​⁠‌‌​⁡‌​​⁡‌⁡⁠‌‌‌‌​‌⁡⁠‌‌⁡‌‌‌⁠⁠⁠‌⁠⁠⁡‌‌​⁠‌‌‌⁡‌⁠⁡⁠‌⁠​⁡‌⁡​⁡‌⁠​⁠‌⁠​⁠‌‌⁠​​⁠⁡⁡‌‌⁠​‌‌​​‌⁠​⁠‌⁡‌⁡‌⁠‌⁠‌​‌‌‌⁡⁠‌‌⁠⁠⁡‌⁠​⁡‌⁡‌⁡‌‌‌⁠‌​⁠⁠‌‌‌⁠‌‌⁠‌‌⁡‌‌‌⁠⁠​‌⁠⁠​‌‌​​​⁠⁡⁡‌‌‌​​⁡⁠‌​⁠⁠⁡​⁡⁠‌‌⁡‌‌‌⁠⁡⁠‌⁠⁡⁡‌⁠⁡​‌​‌​‌⁡​​‌‌​⁡‌​​⁠‌⁠‌⁠‌⁡​⁡‌​‌⁡‌‌‌⁠‌‌​​‌​‌⁡​⁡⁠‌‌​⁡‌‌​⁠⁠‌⁠‌⁡‌​⁠‌‌⁠​⁠‌⁡‌‌‌​​‌‌⁠​⁡‌⁠⁡⁡‌​⁠⁡​⁡​‌‌⁠‌⁡​⁡‌⁡​⁡⁠‌‌​⁡⁠‌‌​‌​⁡​​‌​⁠​​⁠⁠⁡‌‌⁠‌‌⁠​⁡‌⁠‌‌‌‌‌‌‌⁠⁡⁠‌​⁠​‌⁠⁠⁠‌⁠⁡⁠‌⁡⁠​‌​⁡​‌⁡⁠⁠‌⁠​⁡‌‌‌‌‌⁡​⁠‌​‌⁡‌⁡‌⁠‌‌​⁡​⁡⁠‌‌​​⁠‌​⁡⁠‌​‌⁡‌​‌⁠​⁡⁠​‌‌‌⁡​⁡‌‌‌⁠​⁠‌‌​‌​⁡​⁠​⁡‌⁠‌​⁡​‌⁡⁠‌‌⁠⁠​​⁡​⁡‌​‌⁠​⁡‌‌‌​​‌‌⁡‌⁡‌‌‌​‌​⁠⁠‌‌​​​⁡‌⁡‌​⁠‌​⁡‌⁡‌⁠⁠‌‌⁡⁠‌‌⁡⁠⁠‌​⁡​‌‌​⁠‌‌​⁡‌⁠⁡⁠‌⁡‌​‌⁠⁠⁠‌‌⁠​​⁠⁡⁡‌⁠‌‌‌‌⁠⁠‌​‌⁡‌​‌⁡‌​⁡‌‌⁠‌‌‌​⁡​‌⁠⁠⁠‌⁠⁡​‌⁠⁡⁡‌⁠⁠⁠​⁠⁡⁡‌​⁡‌‌⁡​​‌⁠​⁡‌⁠‌⁠‌⁡‌⁠‌⁠⁡⁠‌​⁡‌​⁡​​‌​⁡⁠‌⁡​​‌​⁡⁠‌⁡‌‌‌⁡​⁠‌⁡⁠⁠‌⁡⁠​‌⁠⁠​‌⁠​‌‌⁡‌⁠‌‌​⁠‌​⁠⁠‌⁡⁠​‌‌‌‌​⁡‌⁡‌⁠‌​‌⁠​‌‌⁡‌⁠​⁡​​​⁡‌⁡​⁡​⁠‌​​⁠‌‌⁠‌​⁡‌⁡‌⁡‌⁡‌⁡‌⁡‌‌⁠​‌⁡​⁡‌⁡​⁠​⁡⁠​​⁡​⁡​⁡​⁡‌⁡‌⁡‌⁠‌​‌‌⁠⁠‌⁠‌‌‌‌⁠‌‌​‌‌‌‌​⁠​⁡‌​‌‌​⁡‌⁠⁡⁠‌​⁠‌‌​‌⁡‌⁠⁠⁠‌⁡⁠⁠‌​​⁡‌​⁠‌‌‌​⁠‌​⁠⁠‌‌⁠⁠‌⁠‌⁡‌⁠‌‌‌⁠​‌‌‌​​‌⁡‌‌‌​​⁡‌⁠⁡‌‌​⁠‌‌​⁠⁡‌⁡​⁡‌‌​⁠‌⁠​‌‌⁠​‌​⁡‌​‌‌⁠‌‌‌‌⁡‌⁠‌⁠‌⁠⁠⁡‌​‌‌‌​‌⁡​⁡‌⁠‌‌‌⁡​⁡​⁠​⁡​​‌⁠⁠⁡‌⁡‌‌‌​⁡⁠‌⁠⁠⁡‌‌​⁠‌​​‌‌‌⁠​‌⁡‌​‌⁡‌⁠‌⁡​​​⁡⁠‌‌⁡​⁡‌​‌‌‌​⁠⁡‌⁠⁡‌‌⁡​‌‌⁠​‌‌‌​​​⁡‌‌‌⁠⁡​‌‌‌‌‌⁡​⁡‌⁠⁡⁡​⁡‌‌‌‌​⁡‌⁠​⁡‌⁡​‌‌⁠⁠⁠‌⁡⁠‌‌​⁡⁠​⁡​⁡‌⁠⁠⁡​⁡‌⁠‌⁠‌⁡‌‌‌​‌⁠⁠⁡‌⁠⁡⁡‌​⁡⁠‌‌​‌‌⁡​‌‌‌‌⁡​⁡⁠‌‌​⁡⁡‌⁠⁠⁡‌⁡⁠⁠‌‌​⁡‌‌‌⁠‌​​⁡​⁡​‌‌‌⁠⁠‌‌⁠​‌‌‌⁡‌⁠⁠​‌⁡​⁡‌​⁡​‌⁠‌⁠‌​⁡⁠‌​⁡‌‌​⁡​​⁡⁠‌‌⁡⁠​‌​⁡⁡​⁡‌⁡‌‌​​‌​​⁠‌⁡⁠‌‌‌⁠⁠‌​​⁡‌​⁠⁡‌​⁡⁠​⁡​⁠‌​⁠⁡​⁡‌​‌⁡‌⁠‌​⁠​​⁠⁠⁡‌​⁠‌‌⁠‌​‌⁠⁡‌​⁡‌‌‌​⁠​‌‌⁠⁠‌​⁠​‌​⁠⁠​⁡⁠‌‌‌‌​‌⁠​⁡‌⁡‌⁠‌​⁡⁡‌⁠​‌‌⁠​‌‌⁠⁠⁡‌⁠​⁡​⁡‌​‌​‌‌​⁡​⁠‌⁠⁠‌‌​⁡⁠‌⁠⁠​‌⁡⁠⁠‌​⁠⁠‌⁡​‌‌​​⁠​⁡‌⁡‌⁠⁠‌‌​⁠‌​⁡⁠‌‌⁠‌⁡‌⁠⁠​‌‌​⁡‌⁡‌⁡‌⁠⁡⁠‌⁠⁡⁠‌​⁠​‌​​‌‌​‌‌‌‌​‌‌⁡‌⁠‌‌​⁠‌⁠⁠‌‌‌​‌‌​⁡​‌⁡⁠‌‌⁠‌⁠‌⁠​‌‌‌​​‌⁠‌‌‌⁠⁡​​⁡​‌​⁡‌⁠‌⁡⁠⁠‌‌⁠‌‌⁡⁠​‌​‌⁠​⁡‌‌​⁡‌​​⁠⁠⁡‌‌‌​‌⁡‌⁡‌⁠⁡‌​⁡⁠​‌⁠⁡​‌‌‌⁠‌⁠⁡‌‌⁠​⁠‌‌​​‌‌​⁡​⁠⁠⁡‌‌‌‌‌​‌​‌⁠⁠​‌⁡⁠⁠​⁡⁠​‌⁠⁡‌​⁡​⁠‌​‌‌​⁡​‌​⁡​‌‌⁠⁠​‌​⁠‌​⁡​‌‌⁡‌‌‌‌​​‌⁠⁠⁠‌⁠​‌‌​‌⁡​⁡‌⁠‌​⁡‌‌⁠⁡‌​⁡‌⁠‌⁠⁡⁡​⁡⁡‌⁢