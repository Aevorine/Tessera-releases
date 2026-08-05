# Tessera — coffre-fort chiffré + nettoyage PC pour Windows

[English](README.md) · [简体中文](README.zh-CN.md) · **Français** · [Español](README.es.md) · [Русский](README.ru.md) · [العربية](README.ar.md)

[![Télécharger](https://img.shields.io/badge/t%C3%A9l%C3%A9charger-derni%C3%A8re%20version-brightgreen.svg)](../../releases/latest)
[![Plateforme](https://img.shields.io/badge/plateforme-Windows%2010%2F11%20x64-lightgrey.svg)]()
[![Android](https://img.shields.io/badge/android-application%20compagnon-green.svg)](../../releases/latest)
[![Langues](https://img.shields.io/badge/langues-6-blue.svg)]()

**Vos fichiers, mots de passe, presse-papiers et notes derrière un seul déverrouillage — plus
une suite de nettoyage qui garde la machine en dessous en ordre.** Rien n'est téléversé, rien
ne « téléphone à la maison », et rien ne quitte votre ordinateur sauf si vous le configurez
explicitement.

Gratuit. Hors ligne d'abord. Windows 10/11 64 bits, avec une application Android compagnon
pour le presse-papiers et les fichiers.

## Téléchargement

**→ [Obtenir la dernière version](../../releases/latest)**

| Fichier | Pour qui |
| --- | --- |
| `Tessera-Setup.exe` | Programme d'installation Windows — choix de l'emplacement, entrée de désinstallation. **La plupart des gens veulent celui-ci.** |
| `Tessera.exe` | Portable. Fonctionne depuis n'importe où, y compris une clé USB, ne touche pas au registre. |
| `Tessera-CrossDevice-<version>.apk` | Téléphone ou tablette Android (presse-papiers, fichiers, synchronisation). |

Une fois installé, vous n'avez plus besoin de revenir ici — l'application trouve les nouvelles
versions toute seule.

## Ce que vous obtenez

### Un coffre-fort chiffré

- **Fichiers** — chiffrez n'importe quoi en un seul `.ivault`. Post-quantique par défaut
  (AES-256-GCM pour le contenu, la clé de chaque fichier étant en plus enveloppée par
  ML-KEM-1024), ou AES-256-GCM classique / ChaCha20-Poly1305. Les noms de fichiers et les
  sommes de contrôle vivent *à l'intérieur* de l'en-tête chiffré : un conteneur volé ne révèle
  ni l'un ni l'autre.
- **Mots de passe** — gestionnaire complet, import depuis un CSV Chrome/Edge/Firefox, et le
  CSV en clair est supprimé de façon sécurisée ensuite.
- **Historique du presse-papiers** — classé automatiquement (URL / e-mail / téléphone /
  formule / code / texte), épinglage, recherche, liste noire par application, et un panneau
  global `Ctrl+Maj+V`.
- **Notes** — Markdown avec images, catégories imbriquées, recherche plein texte. Chaque note
  vous ramène là où vous vous étiez arrêté. Une note de 4 Mo affiche son premier écran en
  environ 40 ms.
- **Cinq façons de déverrouiller, une seule suffit** — mot de passe · code e-mail ·
  authentificateur (TOTP) · Windows Hello (visage / empreinte / code PIN) · clé de récupération
  à usage unique.

### Une suite de nettoyage qui dit la vérité

- Fichiers inutiles, caches de navigateurs, traces de confidentialité, résidus de pilotes,
  doublons et gros fichiers, allègement du disque C:, gestionnaire de démarrage, menu
  contextuel, blocage des pop-ups, et 42 petits utilitaires.
- **Trois profondeurs d'analyse pour le bilan en un clic** — *Analyse rapide* (les 4 catégories
  les plus rapides, environ 2 s), *Bilan standard* (10 catégories, ~15 s), *Analyse approfondie*
  (les mêmes 10, mais l'allègement du disque interroge DISM sur le magasin de composants : plus
  lent, et exact plutôt qu'estimé). **21 scanners au total** ; les 11 autres — doublons, gros
  fichiers, espace par dossier, logiciels installés, menu contextuel, données de navigateur,
  pilotes remplacés et le reste — vivent sur leurs propres panneaux, pour qu'un bilan ne vous
  impose jamais un balayage complet du disque que vous n'avez pas demandé.
- **Rien d'irréversible n'est coché à votre place.** Ce que l'application ne peut pas récupérer
  *et* qui coûte un vrai effort à reconstruire est listé et sélectionnable, mais jamais
  pré-sélectionné — le chemin en un clic doit être le chemin sûr.
- **Une simulation parcourt le vrai chemin de code sans toucher un seul octet** et indique ce
  que chaque disque gagnerait réellement.
- **Chaque catégorie précise quel genre de « vide » elle a trouvé** — réellement propre, bloqué
  par les permissions, ou jamais réellement exécuté — et combien d'emplacements ont été
  vérifiés. « Rien trouvé » et « ça n'a jamais tourné » n'ont pas le droit de se ressembler.
- **« Libéré » veut dire libéré.** Les fichiers envoyés à la corbeille sont comptés
  *séparément* de l'espace réellement récupéré, et l'application remesure le disque ensuite
  pour que vous puissiez vérifier son arithmétique face à ce que Windows annonce.
- Cinq jugements sur chaque élément — *Windows en a besoin · un pilote · quelque chose que vous
  utilisez · optionnel · une publicité* — avec la raison écrite en toutes lettres. Ce dont
  Windows a besoin est verrouillé, et le back-end le refuse même si on le lui demande.
- Les clés de registre, entrées de démarrage et gestionnaires de menu sont **désactivés avec une
  sauvegarde**, jamais supprimés. Si l'export de la sauvegarde échoue, rien n'est modifié.

### Téléphone ↔ PC, sur votre propre réseau local

Copiez sur votre téléphone, collez sur votre PC, et inversement — texte, texte enrichi, code,
formules mathématiques et images conservent leur mise en forme. Envoyez des fichiers ou des
dossiers entiers, avec reprise et chiffrement de bout en bout. Appairage par code à six
chiffres, QR code, lien, ou depuis une liste d'appareils à proximité. Le trafic reste sur votre
réseau local.

### Interface

Un seul langage visuel sur tous les écrans. Huit couleurs d'accent, clair/sombre/système, un
mode Simple ou Professionnel, et un mode confort visuel qui décale la température de couleur
**sans** toucher aux couleurs d'état — le confort ne doit pas vous coûter la capacité de voir
ce qui a échoué.

**Six langues** — English, 简体中文, Français, Español, Русский, العربية — commutables partout,
y compris sur l'écran de connexion.

## Vous cherchez une alternative à…

Tessera couvre, dans une seule application hors ligne, ce pour quoi on installe d'habitude trois ou quatre outils :

- **CCleaner / BleachBit / Wise Disk Cleaner** — fichiers inutiles, données de navigation,
  restes de registre, gestionnaire de démarrage, doublons, allègement du disque C:.
  Chaque entrée explique ce qu'elle est et ce que vous perdez ; une simulation montre
  d'abord ce qui se passerait.
- **KeePass / Bitwarden / 1Password** — gestionnaire de mots de passe local avec TOTP intégré.
  Pas de compte, pas de serveur, pas d'abonnement.
- **VeraCrypt / 7-Zip AES** — chiffrement de fichiers et dossiers, post-quantique par défaut.
- **Ditto / ClipClip** — historique du presse-papiers avec recherche, épinglage et raccourci global.
- **Obsidian / prise de notes rapide** — notes Markdown avec images et recherche plein texte, chiffrées au repos.
- **AirDroid / LocalSend** — synchronisation presse-papiers et fichiers en réseau local avec Android.

Gratuit et open source (AGPL-3.0), Windows 10/11 64 bits.

## À l'installation : ce que Windows et Android vont demander

**Windows.** Les builds sont signés avec un certificat auto-signé, donc SmartScreen peut
avertir (« Windows a protégé votre ordinateur »). Cliquez sur **Informations complémentaires →
Exécuter quand même**. Cet avertissement porte sur le fait que le certificat n'a pas été acheté
auprès d'une autorité commerciale — pas sur une altération du fichier.

Les mises à jour intégrées utilisent la même clé de signature : après téléchargement,
l'application vérifie que la signature est bien *cette* clé, et **supprime le fichier au lieu de
l'installer** si ce n'est pas le cas. Il n'existe aucun chemin « continuer quand même ».

**Android.** La première mise à jour intégrée vous demandera d'autoriser l'installation
d'applications inconnues pour Tessera. Android n'autorise jamais une application chargée
latéralement à installer quoi que ce soit en silence — cette confirmation finale appartient
toujours au système. Les mises à niveau exigent une signature identique, imposée par Android
lui-même : personne d'autre ne peut publier un build se faisant passer pour cette application.

## Vérifier ce que vous avez téléchargé

GitHub affiche un SHA-256 pour chaque fichier sur la page de version. Les mises à jour
intégrées le comparent automatiquement ; si vous avez téléchargé à la main :

```powershell
Get-FileHash .\Tessera-Setup.exe -Algorithm SHA256
```

```bash
sha256sum Tessera-CrossDevice-*.apk
```

Vous pouvez aussi faire un clic droit sur l'exe → **Propriétés → Signatures numériques**.

## Configuration requise

Windows 10 ou 11, 64 bits. Plusieurs fonctions sont liées aux API Windows (Windows Hello,
intégration au shell, raccourcis globaux). Application Android compagnon : Android 8.0 ou plus
récent.

## Questions

**Le code source est-il public ?** Pas pour le moment. Ce dépôt ne contient que les builds
publiés.

**Est-ce que ça « téléphone à la maison » ?** Non. La seule requête sortante que l'application
émet d'elle-même est la vérification de mise à jour vers ce dépôt, et vous pouvez la passer en
manuel.

**Où vivent mes données ?** Dans une base de données chiffrée, dans un dossier que vous
choisissez lors de la configuration. Les mises à jour n'y touchent jamais.

**J'ai perdu mon mot de passe.** Utilisez la clé de récupération à usage unique fournie lors de
la configuration. Sans elle, et sans aucune des quatre autres méthodes de déverrouillage, le
coffre ne peut pas être ouvert — c'est précisément son intérêt.

## Licence

AGPL-3.0, avec une licence commerciale également disponible. Les conditions complètes sont
livrées avec l'application et visibles dans sa page À propos.
