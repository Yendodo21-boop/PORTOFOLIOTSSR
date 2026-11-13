# TP03 — Permissions avancées Linux : SGID et umask

## 1. Objectif du TP

Comprendre et appliquer les permissions avancées sous Linux afin de gérer correctement un dossier partagé entre plusieurs techniciens.

Les objectifs sont :
- Comprendre le fonctionnement du bit SGID.
- Vérifier l'héritage du groupe sur les nouveaux fichiers et sous-dossiers.
- Comprendre le rôle de `umask`.
- Tester les droits d'accès de plusieurs utilisateurs.
- Corriger les permissions pour permettre le travail collaboratif.

## 2. Environnement

- Système : Debian GNU/Linux 13 (Trixie)
- Utilisateur principal : `yentest1`
- Second utilisateur : `techuser`
- Groupe partagé : `techniciens`
- Dossier partagé : `/srv/partage-techniciens`

## 3. Vérification initiale

Le dossier partagé possédait les permissions suivantes :

```bash
ls -ld /srv/partage-techniciens
```

Résultat observé :

```text
drwxrwx--- root techniciens /srv/partage-techniciens
```

Les membres du groupe `techniciens` disposaient des droits de lecture, d'écriture et de traversée du dossier.

Les premiers tests ont montré que les fichiers créés avant l'activation du SGID n'héritaient pas automatiquement du groupe `techniciens`.

## 4. Activation du bit SGID

Commande utilisée :

```bash
sudo chmod g+s /srv/partage-techniciens
```

Vérification :

```bash
ls -ld /srv/partage-techniciens
```

Résultat observé :

```text
drwxrws--- root techniciens /srv/partage-techniciens
```

Le `s` dans les permissions du groupe indique que le bit SGID est activé.

Les nouveaux fichiers et sous-dossiers créés dans ce répertoire héritent du groupe `techniciens`.

## 5. Vérification de l'héritage du groupe

Un fichier a été créé par `yentest1` après l'activation du SGID :

```bash
touch /srv/partage-techniciens/fichier2-yentest1.txt
ls -l /srv/partage-techniciens
```

Résultat observé :

```text
-rw-rw-r-- yentest1 techniciens fichier2-yentest1.txt
```

Un sous-dossier a ensuite été créé :

```bash
mkdir /srv/partage-techniciens/projet-test
```

Un fichier a été créé par `techuser` dans ce sous-dossier :

```bash
sudo -u techuser touch /srv/partage-techniciens/projet-test/fichier-techuser.txt
```

Résultat observé :

```text
-rw-rw-r-- techuser techniciens fichier-techuser.txt
```

Ces tests confirment l'héritage du groupe `techniciens`, y compris dans le sous-dossier.

## 6. Comprendre umask

Commande utilisée :

```bash
umask
```

Valeur observée :

```text
0002
```

Une valeur de `umask` détermine les permissions qui sont retirées lors de la création des fichiers et des répertoires.

Un test temporaire a été effectué avec la valeur `0022` :

```bash
( umask 022; touch /srv/partage-techniciens/fichier-umask022.txt )
ls -l /srv/partage-techniciens/fichier-umask022.txt
```

Résultat observé :

```text
-rw-r--r-- yentest1 techniciens fichier-umask022.txt
```

Le fichier héritait bien du groupe `techniciens`, mais le groupe ne disposait pas du droit d'écriture.

## 7. Correction des permissions

Pour autoriser l'écriture au groupe sur ce fichier, la commande suivante a été utilisée :

```bash
chmod g+w /srv/partage-techniciens/fichier-umask022.txt
```

Vérification :

```bash
ls -l /srv/partage-techniciens/fichier-umask022.txt
```

Résultat observé :

```text
-rw-rw-r-- yentest1 techniciens fichier-umask022.txt
```

Un test avec `techuser` a ensuite confirmé que l'écriture était autorisée :

```bash
sudo -u techuser sh -c '[ -w /srv/partage-techniciens/fichier-umask022.txt ] && echo "écriture autorisée" || echo "écriture refusée"'
```

Résultat :

```text
écriture autorisée
```

## 8. Bilan

Ce TP m'a permis de comprendre la différence entre le bit SGID et `umask`.

- Le SGID permet aux nouveaux fichiers et sous-dossiers d'hériter du groupe du répertoire parent.
- `umask` influence les permissions attribuées lors de la création des fichiers et répertoires.
- La commande `chmod g+w` ajoute le droit d'écriture au groupe.
- Les permissions doivent être vérifiées avec `ls -l` et `ls -ld`.
- Les fichiers créés avant l'activation du SGID ne changent pas automatiquement de groupe.

Ces manipulations sont utiles pour administrer des dossiers partagés dans un environnement Linux professionnel.

## 9. Captures d'écran

Les captures d'écran des commandes et des résultats seront ajoutées dans le dossier `captures/` du TP03.
