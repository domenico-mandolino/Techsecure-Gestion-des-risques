# Techsecure-Gestion-des-risques
Gestion des risques
TechSecure

Introduction du sujet
"TechSecure" est une PME spécialisée dans le développement de solutions
logicielles de cybersécurité. Elle dispose d'une équipe de 50 employés et gère
des données sensibles pour ses clients.
• Infrastructure : Serveurs cloud, bases de données clients, site web
dématérialisé.
• Données critiques : Informations clients, codes sources de logiciels de
sécurité.
• Menaces potentielles : Phishing, ransomware, attaque DDoS, vol de
données internes.

Vous avez pour mission d’évaluer les risques , pour cela « TechSecure » fournis
le schéma de l’infrastructure de son réseau , ainsi que la configuration des
équipements réseau ( voir annexes , les mots de passes ont volontairement
été changés )


Vous devez évaluer la probabilité et l'impact des risques en utilisant une
matrice des risques .
- Identifier les faiblesses potentielles dans l'architecture réseau
(segmentation inadéquate, points de défaillance uniques, etc.).
- Créer une liste des menaces spécifiques à cette infrastructure, classées par
type d'attaquant (interne, externe, sophistiqué, opportuniste).
- Évaluation des contrôles de sécurité : Analysez les configurations pour
identifier les contrôles manquants ou mal configurés (règles de pare-feu trop
permissives, mauvaise ségrégation des VLANs, etc.).
- Plan de remédiation : Création d'un plan hiérarchisé pour corriger les
vulnérabilités identifiées.
- Étude d'impact business : Identifiez les actifs critiques sur ce réseau et
évaluez l'impact business en cas de compromission.
- Élaboration d'un plan de continuité : Conception de stratégies pour
maintenir les services essentiels en cas d'incident.

- Analyse de conformité : Vérifiez si la configuration respecte les normes
pertinentes ( RGPD, etc.).
- Élaboration de règles adaptées à cette infrastructure.
- Identification des points de surveillance et des alertes à implémenter.
- Analysez les configurations pour identifier les contrôles manquants ou mal
configurés (règles de pare-feu trop permissives, mauvaise ségrégation des
VLANs, etc.).


Annexe Configuration Équipement

1) Adressage IP
Réseau principal : 10.0.0.0/16
VLAN 10 (Administration) : 10.0.10.0/24
VLAN 20 (Développement) : 10.0.20.0/24
VLAN 30 (Commercial) : 10.0.30.0/24
VLAN 40 (Serveurs internes) : 10.0.40.0/24
VLAN 99 (Management) : 10.0.99.0/24
DMZ : 172.16.0.0/24

2) Routeur R1 (Cisco 2911)
enable
configure terminal
hostname R1
enable secret TechSecure@2025
interface GigabitEthernet0/0
description Connexion WAN
ip address dhcp
no shutdown

interface GigabitEthernet0/1
description Connexion LAN
ip address 10.0.0.1 255.255.0.0
no shutdown

ip nat inside source list 1 interface GigabitEthernet0/0 overload
access-list 1 permit 10.0.0.0 0.0.255.255
ip route 172.16.0.0 255.255.255.0 10.0.0.2


3) Pare-feu ASA 5506-X (FW1)
enable
configure terminal
hostname FW1

interface GigabitEthernet1/1
nameif outside
security-level 0
ip address 10.0.0.2 255.255.0.0
interface GigabitEthernet1/2
nameif inside
security-level 100
ip address 10.0.1.1 255.255.0.0
interface GigabitEthernet1/3
nameif dmz
security-level 50
ip address 172.16.0.1 255.255.255.0

3) Switch Principal (S1 - L3)
enable
configure terminal
hostname S1
vlan 10
name Administration

vlan 20
name Developpement

vlan 30
name Commercial



vlan 40
name Serveurs_Internes

vlan 99
name Management

4) Configuration des Points d’Accès (AP1 et AP2 VLAN30)
enable
configure terminal
hostname AP1 ou AP2
interface Dot11Radio0
ssid TechSecure-WiFi
authentication open
encryption mode ciphers aes-ccm
wpa-psk ascii TechSecure@2025!
interface GigabitEthernet0
switchport mode access
switchport access vlan 30
no shutdown

5) Configuration du Serveur MySQL/MariaDB (VLAN 40)
CREATE USER 'dev_user'@'10.0.20.%' IDENTIFIED BY 'DevPass123!';
GRANT ALL PRIVILEGES ON *.* TO 'dev_user'@'10.0.20.%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
