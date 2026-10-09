TP02-Gestion des utilisateurs, groupes et permissions Linux



1. Objectif



Mettre en pratique les commandes Linux permettant de gérer les utilisateurs, les groupes et les permissions d'un dossier partagé.



Ce TP simule un besoin courant en entreprise : permettre à plusieurs techniciens d'accéder à un espace de travail commun.



2. Environnement



\* Système : Debian GNU/Linux 13 (Trixie)

\* Machine virtuelle : VirtualBox

\* Nom de la machine : `debian1`

\* Utilisateur d'administration : `yentest1`



3. Création du groupe et de l'utilisateur



Création du groupe `techniciens` :



```bash

sudo groupadd techniciens

```



Création de l'utilisateur de test :



```bash

sudo useradd -m -s /bin/bash techuser

```



Ajout de l'utilisateur au groupe secondaire `techniciens` :



```bash

sudo usermod -aG techniciens techuser

```



Vérification de l'appartenance au groupe :



```bash

id techuser

getent group techniciens

```



4. Création du dossier partagé



Création du dossier :



```bash

sudo mkdir -p /srv/partage-techniciens

```



Attribution du propriétaire et du groupe :



```bash

sudo chown root:techniciens /srv/partage-techniciens

```



Configuration des permissions :



```bash

sudo chmod 770 /srv/partage-techniciens

```



Les permissions `770` accordent tous les droits au propriétaire et au groupe, et aucun droit aux autres utilisateurs.



5. Test d'accès



Création d'un fichier de test en tant que `techuser` :



```bash

sudo -u techuser touch /srv/partage-techniciens/test.txt

```



Vérification du contenu :



```bash

sudo ls -la /srv/partage-techniciens

```



Le fichier `test.txt` a été créé avec succès.



Lors des tests, l'utilisateur `yentest1` ne pouvait initialement pas accéder au dossier, car il n'appartenait pas au groupe `techniciens`. Après son ajout au groupe, il a fallu actualiser la session pour prendre en compte cette appartenance.



6. Commandes étudiées



| Commande       | Utilité                                                      |

| -------------- | ------------------------------------------------------------ |

| `groupadd`     | Créer un groupe                                              |

| `useradd`      | Créer un utilisateur                                         |

| `usermod -aG`  | Ajouter un utilisateur à un groupe secondaire                |

| `id`           | Afficher les UID, GID et groupes                             |

| `getent group` | Consulter les informations d'un groupe                       |

| `mkdir`        | Créer un dossier                                             |

| `chown`        | Modifier le propriétaire et le groupe                        |

| `chmod`        | Modifier les permissions                                     |

| `ls -l`        | Afficher les informations des fichiers et dossiers           |

| `sudo -u`      | Exécuter une commande sous l'identité d'un autre utilisateur |



7. Compétences mises en pratique



\* Création et gestion de comptes Linux

\* Gestion des groupes secondaires

\* Attribution des droits sur un dossier partagé

\* Vérification des permissions

\* Diagnostic d'un problème d'accès

\* Utilisation des commandes d'administration système



8. Captures de preuve



Les captures associées à ce TP sont stockées dans le dossier `captures/`.



\* `id-techuser` : vérification du compte de test

\* `groupe-techniciens : vérification du groupe

\* `permissions-dossier` : propriétaire, groupe et permissions

\* `test-partage.png` : vérification du fichier de test



9. Bilan



Ce TP m'a permis de comprendre comment organiser les accès à un dossier partagé sous Linux en utilisant les utilisateurs, les groupes et les permissions. J'ai également appris à diagnostiquer un refus d'accès lié aux droits du dossier et à l'appartenance aux groupes.



