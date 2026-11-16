🛠️ Étude de Cas Support N1 (GLPI) — Diagnostic d'Incident Réseau

Ce dépôt présente une mise en situation pratique de Technicien Support Informatique Niveau 1 (N1) au sein d'une PME de 60 collaborateurs. L'objectif de cet exercice méthodologique est d'analyser un ticket d'incident entrant sous GLPI et de structurer une démarche de diagnostic rigoureuse avant toute intervention technique sur l'infrastructure.

🏢 1. Contexte Professionnel

Entreprise : PME de 60 employés

Rôle : Technicien Support N1

Outil ITSM : GLPI

Contrainte de l'exercice : Environnement de laboratoire non encore relié au réseau. Le raisonnement doit s'appuyer exclusivement sur l'analyse méthodologique du ticket, sans modification des machines virtuelles.

🎫 2. Ticket d'Incident Entrant (INC-001)

Référence : INC-001 — Accès au dossier partagé impossible

Heure de réception : 09 h 12

Demandeur : Sophie Martin (Service Comptabilité)

Équipement : PC Windows 10

Description de l'utilisateur :

« Bonjour, je n'arrive plus à ouvrir le dossier partagé de la comptabilité. J'en ai besoin pour terminer les factures avant midi. Hier, tout fonctionnait normalement. Merci de m'aider rapidement. »

🎯 3. Objectifs

À l'issue de l'analyse de ce ticket, il faudra :

- Identifier les informations manquantes indispensables avant de chercher une solution technique.

- Formuler plusieurs hypothèses pertinentes en distinguant clairement les symptômes des causes profondes.

- Prioriser les vérifications techniques dans un ordre logique (du plus simple/local au plus global).

- Évaluer la criticité de l'incident (matrice Impact / Urgence).

- Rédiger une réponse de prise en charge claire, rassurante et professionnelle dans le suivi du ticket GLPI.

📋 4. Grille d'Analyse Méthodologique

4.1 Évaluation de la criticité (ITIL)

Impact : Individuel ou collectif ? D'autres collègues de la comptabilité sont-ils touchés ?

Urgence : Élevée, échéance métier critique fixée à 12 h 00 pour la facturation

Priorité résultante : À définir selon la matrice Impact/Urgence.

4.2 Informations manquantes à collecter auprès de l'utilisatrice

Quel est le message d'erreur exact affiché par Windows (ex. : « Accès refusé », « Le chemin réseau n'a pas été trouvé ») ?

Comment accède-t-elle au dossier (lecteur réseau mappé type Z:\, raccourci bureau, chemin UNC \\SERVEUR\Compta) ?

Le PC a-t-il accès à Internet ou à d'autres ressources internes (intranet, messagerie) ?

Ses collègues du service comptabilité rencontrent-ils le même blocage ?

Un redémarrage du poste ou une mise à jour Windows a-t-il eu lieu depuis hier ?

4.3 Hypothèses techniques (Symptôme vs Cause)

Symptôme constaté : Impossibilité d'ouvrir le partage réseau de la comptabilité.

Hypothèses de causes possibles :

Couche physique / liaison : Câble Ethernet débranché, Wi-Fi déconnecté ou basculé sur un réseau invité.

Configuration IP / DHCP : Perte de bail DHCP, adresse APIPA (169.254.x.x), problème de résolution DNS.

Authentification / Session : Expiration du jeton Kerberos, mot de passe expiré ou session Windows verrouillée/désynchronisée avec l'Active Directory.

Côté Serveur / Infrastructure : Serveur de fichiers inaccessible, service SMB arrêté, modification accidentelle des droits NTFS/partage ou saturation disque.

4.4 Plan de vérification logique (Du poste client vers le serveur)

Vérification de la connectivité locale : État de la carte réseau, adresse IP (ipconfig /all), accès à la passerelle.

Test de joignabilité et résolution de noms : ping vers le serveur de fichiers (par nom NetBIOS/FQDN puis par adresse IP).

Vérification du mappage et des identifiants : Test d'accès direct via chemin UNC (\\<IP_SERVEUR>\Compta), vérification du Gestionnaire d'identification Windows.

Escalade / Vérification serveur (si incident collectif) : Contrôle de l'état des services de partage (SMB) et des permissions sur le serveur.

💬 5. Modèle de Prise en Charge (Suivi Ticket GLPI)

Bonjour Madame Martin,

Votre demande concernant l'impossibilité d'accéder au dossier partagé de la comptabilité a bien été prise en compte (Ticket INC-001). Compte tenu de votre impératif de facturation avant midi, votre ticket est traité en priorité.

Afin d'orienter rapidement mon diagnostic, pourriez-vous me préciser :
1. Quel est le message d'erreur exact qui s'affiche lorsque vous tentez d'ouvrir le dossier ?
2. Vos collègues du service comptabilité rencontrent-ils le même problème ce matin ?
3. Avez-vous accès à Internet ou à votre messagerie depuis votre poste ?

Je reste disponible par téléphone au [Numéro interne] ou je peux prendre la main à distance sur votre poste dès votre retour.

Cordialement,
Support Informatique N1
