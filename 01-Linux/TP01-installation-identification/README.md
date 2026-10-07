# TP01 — Installation et identification d'une machine Linux

## 🎯 Objectif

Installer une machine virtuelle Debian et effectuer les premières opérations d'identification et de vérification du système.

L'objectif est de disposer d'une première machine Linux fonctionnelle qui servira de base au laboratoire TSSR.

## 🖥️ Environnement

| Élément          | Configuration                |
| ---------------- | ---------------------------- |
| Machine hôte     | Windows 11 Famille           |
| Hyperviseur      | VirtualBox                   |
| Système invité   | Debian GNU/Linux 13 (Trixie) |
| Mémoire VM       | 2 Go RAM                     |
| Disque VM        | 19 Go                        |
| Réseau           | NAT VirtualBox               |
| Interface réseau | enp0s3                       |

## 🌐 Configuration réseau

La machine Debian utilise actuellement le réseau NAT de VirtualBox.

Adresse IPv4 :

`##.#.#.##/##`

La passerelle et les serveurs DNS sont fournis par l'environnement réseau VirtualBox.

## 🔎 Vérifications effectuées

Plusieurs commandes ont été utilisées afin d'identifier et de vérifier le système :

```bash
hostnamectl
```

Cette commande permet d'obtenir des informations sur le nom de la machine et le système.

```bash
ip addr
```

Cette commande permet d'afficher les interfaces réseau et les adresses IP.

```bash
ip route
```

Cette commande permet d'afficher les routes réseau et notamment la passerelle par défaut.

```bash
cat /etc/resolv.conf
```

Cette commande permet d'afficher la configuration DNS utilisée par la machine.

## 🧪 Tests réseau

Les tests suivants ont été réalisés :

* Vérification de la connectivité réseau
* Test de la passerelle
* Test d'une adresse IP externe
* Test de résolution DNS
* Test d'accès à Internet

Les tests sont fonctionnels.

## 📌 Notions retenues

Cette première manipulation m'a permis de comprendre le rôle de plusieurs éléments :

* **Adresse IP :** identifie la machine sur le réseau.
* **Passerelle :** permet de communiquer avec des réseaux extérieurs au réseau local.
* **DNS :** permet de traduire un nom de domaine en adresse IP.
* **Interface réseau :** permet à la machine de communiquer sur le réseau.

## ⚠️ Difficultés rencontrées

La commande `resolvectl status` n'était pas disponible dans cette installation Debian.

La configuration DNS a donc été vérifiée directement avec :

```bash
cat /etc/resolv.conf
```

## ✅ Résultat

La machine Debian est fonctionnelle et dispose d'une connectivité réseau opérationnelle.

Elle constitue la première machine du laboratoire TSSR et pourra maintenant être utilisée pour les prochains travaux pratiques.

## 🧠 Compétences travaillées

* Installation d'un système Linux
* Identification d'un système
* Analyse d'une configuration réseau
* Utilisation de commandes Linux
* Diagnostic réseau de base
* Compréhension de DNS et de la passerelle
* Documentation technique
