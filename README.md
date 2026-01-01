# Architecture Réseau d'Entreprise : Commutation, Routage et WAN

![Cisco Packet Tracer](https://img.shields.io/badge/Technologie-Cisco%20Packet%20Tracer-blue)
![Status](https://img.shields.io/badge/État-Finalisé-success)

## Informations Générales

Ce dépôt héberge les ressources techniques et la documentation d'un projet de conception d'infrastructure réseau.  
Le projet simule un réseau d'entreprise hiérarchique interconnectant un siège social à des sites distants via une liaison WAN.

- **Auteur :** [Boustane Oussama](https://www.linkedin.com/in/oussama-boustane-22a990298/)
- **Contexte :** Module Réseaux Informatiques
- **Année Académique :** 2025 / 2026

---

## Objectifs Techniques

L'objectif principal est de démontrer la mise en œuvre des technologies suivantes :

1. **Commutation (Switching)**  
   - VLANs  
   - Trunking IEEE 802.1Q  
   - Agrégation de liens (EtherChannel – LACP)

2. **Routage Inter-VLAN**  
   - Configuration *Router-on-a-Stick*

3. **Interconnexion WAN**  
   - Liaisons série point-à-point  
   - Routage entre sites distants

4. **Optimisation du Routage**  
   - Résumé de routes (*Route Summarization*)

---

## Topologie et Architecture

Le réseau est organisé autour :
- d’un **routeur central (R1)** assurant le routage inter-VLAN et l’accès WAN,
- d’une **couche distribution / accès** composée de commutateurs interconnectés.

![Schéma de la Topologie Globale](images/topologie_globale.png)

---

## Inventaire du Matériel Simulé

| Équipement | Modèle | Rôle Principal |
|----------|--------|----------------|
| **Routeur R1** | Cisco 2811 | Passerelle LAN & Sortie WAN |
| **Routeur R2** | Cisco 2811 | Routeur de transit (FAI / WAN) |
| **Routeur R3** | Cisco 2811 | Site distant (Simulation Internet) |
| **Switch S1** | Cisco 2960 | Distribution & Agrégation |
| **Switch S2** | Cisco 2960 | Accès Clients & Management |

---

## Plan d’Adressage IP (VLSM)

Un plan d’adressage optimisé a été appliqué afin d’économiser les adresses IPv4, notamment sur les liaisons point-à-point (/30).

| Périphérique | Interface | Adresse IP | Masque | Description |
|-------------|-----------|------------|--------|-------------|
| **R1** | Fa0/0.10 | 172.18.10.14 | /28 | Passerelle VLAN 10 |
| | Fa0/0.20 | 172.18.20.14 | /28 | Passerelle VLAN 20 |
| | Fa0/0.30 | 172.18.30.14 | /28 | Passerelle VLAN 30 |
| | S0/0/0 | 10.0.30.177 | /30 | Liaison vers R2 |
| **R2** | S0/0/0 | 10.0.30.178 | /30 | Liaison vers R1 |
| **R3** | Loopback0 | 10.0.30.129 | /32 | IP cible (Test) |
| **S2** | Vlan60 | 172.18.60.2 | /28 | Interface de gestion |

---

## Points Clés de la Configuration

### 1. Agrégation de Liens (EtherChannel – LACP)

Configuration de l’agrégation de liens entre les commutateurs **S1** et **S2** afin d’assurer la redondance et d’augmenter la bande passante.

```text
! Extrait de configuration S1
interface range FastEthernet0/21-22
 channel-group 1 mode active
!
interface Port-channel1
 switchport mode trunk
 switchport trunk native vlan 50
2. Routage Inter-VLAN (Router-on-a-Stick)
Utilisation de sous-interfaces sur le routeur R1 avec encapsulation IEEE 802.1Q.

text
Copy code
! Extrait de configuration R1
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 172.18.10.14 255.255.255.240
Validation et Tests
Test de Connectivité (Ping & Traceroute)
Les tests de connectivité valident le cheminement complet des paquets depuis le réseau local (VLAN 10) vers le site distant simulé (R3), en passant par le routeur central R1 et le réseau WAN.

Vérification de l’EtherChannel
La commande suivante permet de confirmer :

l’état SU du Port-Channel,

l’agrégation correcte des interfaces via LACP (P).

text
Copy code
show etherchannel summary
Contenu du Dépôt
configs/
Fichiers de configuration brute (.txt) extraits des équipements

images/
Captures d’écran de la topologie et des tests

Architecture_Reseau.pkt
Fichier de simulation Cisco Packet Tracer

Auteur
Projet académique réalisé par Boustane Oussama
© 2025 – 2026
