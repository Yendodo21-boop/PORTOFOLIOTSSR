Besoin : isoler les services et les équipements reconditionnés du reste du réseau. 
Objectif : reproduire 1 schéma en créant six VLAN, distribuer les adresses par DHCP, router entre VLAN et prouver les communications par des ping.
#1 Choix d'architecture
Le schéma montre un seul câble entre le switch (2960, couche 2) et le routeur : le routage inter-VLAN se fait donc en router-on-a-stick sur l'interface Gig0/0 du routeur. Le serveur DHCP est le seul serveur du schéma, dans le VLAN 60 : les VLAN 10 à 50 n'étant pas dans son réseau, un relais DHCP est configuré sur leurs sous-interfaces (ip helper-address 192.168.60.1). Le serveur n'a pas de pool pour le VLAN 60, car son adresse est fixe. Chaque passerelle est la dernière adresse utilisable (.254).
#2 Réalisation
1.  Maquette : routeur R-GTS (2911), switch SW-GTS (2960-24TT), serveur SRV-DHCP et cinq PC (RH, COMPTA, INFO, DG, STOCK). [Q4-01]
2.  Switch : VLAN 10 RH, 20 COMPTA, 30 INFO, 40 DG, 50 STOCKAGE et 60 SERVEURS ; Fa0/1 à Fa0/5 en access dans les VLAN 10 à 50 ; Gig0/1 en access VLAN 60 ; Gig0/2 en trunk autorisant les VLAN 10 à 60.
3.  Routeur : Gig0/0 activée, sous-interfaces .10 à .60 en encapsulation dot1Q avec l'adresse .254 de chaque réseau, et ip helper-address 192.168.60.1 sur les cinq premières.
4.  Serveur SRV-DHCP : adresse fixe 192.168.60.1/24, passerelle 192.168.60.254, DNS 192.168.60.10, service DHCP activé avec un pool par VLAN 10 à 50 (passerelle .254, première adresse .10, 200 adresses).
5.  Les PC passent en DHCP et reçoivent une adresse de leur réseau avec la passerelle .254 ; des ping réussissent entre PC de VLAN différents et vers le serveur.
#3 Vérifications observées
Contrôle
Résultat
show vlan brief
Fa0/1 dans VLAN 10, Fa0/2 dans 20, Fa0/3 dans 30, Fa0/4 dans 40, Fa0/5 dans 50, Gig0/1 dans 60
show interfaces trunk
Gig0/2 en trunking, 802.1q, VLAN 10 à 60 autorisés et actifs
show ip interface brief (routeur)
Gig0/0 et les six sous-interfaces up/up, adresses en .254
show interfaces gig0/0.60
encapsulation 802.1Q, VLAN 60, 192.168.60.254/24
show mac address-table
adresse du serveur en VLAN 60 sur Gig0/1
SRV-DHCP
adresse statique 192.168.60.1/24, passerelle .254
Captures : Q4-02 à Q4-07.
#4 Difficultés rencontrées et corrections
• La configuration du routeur a d'abord été saisie dans le CLI du switch : interface gig0/0 inexistante sur un 2960 (Invalid interface type), nom du switch changé en R-GTS. Correction : restauration du nom SW-GTS et saisie dans le CLI du routeur (invite Router).
• Gig0/0 du routeur en down/down alors que sa configuration était correcte : le câble n'était pas sur la bonne interface. Symptômes : ping du routeur vers le serveur à 0 %, aucune adresse MAC du routeur sur Gig0/2, table ARP du routeur vide. Correction : câbles replacés sur Gig0/2 (routeur) et Gig0/1 (serveur) ; show interfaces trunk valide alors le trunk.
• Un PC resté en adresse 169.254.x.x (APIPA) : son câble était sur un port non configuré, donc en VLAN 1, sans relais ni pool. Correction : câble déplacé. Le PC reçoit ensuite une adresse de son réseau.
• La commande show ip arp sur le switch est vide : c'est normal, un switch de couche 2 n'a pas d'adresse IP. La table MAC est son équivalent.
#5 Limites et sécurité
• Un ping réussi ne prouve pas la sécurisation du réseau : en entreprise, les flux entre VLAN seraient limités par des ACL.
• Les ports inutilisés (Fa0/6 à Fa0/24) restent dans le VLAN 1 et actifs ; on les passerait en shutdown.
• Le DHCP et le DNS 192.168.60.10 sont simulés ; le VLAN natif reste le VLAN 1.
• Sauvegarder la configuration (copy running-config startup-config) et le fichier Packet Tracer.
#6 Bilan
Les six réseaux sont représentés, segmentés par VLAN, routés par un router-on-a-stick et alimentés par un DHCP relayé. Les postes reçoivent leur adresse et communiquent entre eux.
