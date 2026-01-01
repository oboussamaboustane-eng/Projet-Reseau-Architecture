# Architecture Reseau d'Entreprise : Commutation, Routage et WAN

![Cisco Packet Tracer](https://img.shields.io/badge/Technologie-Cisco%20Packet%20Tracer-blue)
![Status](https://img.shields.io/badge/Etat-Finalise-success)

## Informations Generales

Ce depot heberge les ressources techniques et la documentation d'un projet de conception d'infrastructure reseau. Le projet simule un reseau d'entreprise hierarchique interconnectant un siege social a des sites distants via une liaison WAN.

* **Auteur :** [Boustane Oussama](https://www.linkedin.com/in/oussama-boustane-22a990298/)
* **Contexte :** Module Reseaux Informatiques
* **Annee Academique :** 2025/2026

---

## Objectifs Techniques

L'objectif principal est de demontrer la mise en oeuvre des technologies suivantes :
1. **Commutation (Switching) :** Segmentation par VLANs, protocoles de trunking (802.1Q) et agregation de liens (LACP).
2. **Routage Inter-VLAN :** Configuration "Router-on-a-Stick" pour la communication entre sous-reseaux.
3. **Interconnexion WAN :** Liaisons series et protocoles de routage pour l'acces aux sites distants.
4. **Optimisation :** Resume de routes (Route Summarization) pour alleger les tables de routage.

---

## Topologie et Architecture

Le reseau s'articule autour d'un coeur de reseau (R1) gerant le routage inter-VLAN et l'acces WAN, et d'une couche distribution/acces assuree par des commutateurs interconnectes.

![Schema de la Topologie Globale](images/topologie_globale.png)

### Inventaire du Materiel Simule

| Equipement | Modele | Role Principal |
| :--- | :--- | :--- |
| **Routeur R1** | Cisco 2811 | Passerelle LAN & Sortie WAN |
| **Routeur R2** | Cisco 2811 | Routeur de transit (FAI/WAN) |
| **Routeur R3** | Cisco 2811 | Destination distante (Simulation Internet) |
| **Switch S1** | Cisco 2960 | Distribution & Agregation |
| **Switch S2** | Cisco 2960 | Acces Clients & Management |

---

## Plan d'Adressage IP (VLSM)

Un adressage optimise a ete applique pour economiser les adresses IPv4, notamment sur les liaisons point-a-point (/30).

| Peripherique | Interface | Adresse IP | Masque (CIDR) | Description |
| :--- | :--- | :--- | :--- | :--- |
| **R1** | Fa0/0.10 | 172.18.10.14 | /28 | Passerelle VLAN 10 |
| | Fa0/0.20 | 172.18.20.14 | /28 | Passerelle VLAN 20 |
| | Fa0/0.30 | 172.18.30.14 | /28 | Passerelle VLAN 30 |
| | S0/0/0 | 10.0.30.177 | /30 | Liaison vers R2 |
| **R2** | S0/0/0 | 10.0.30.178 | /30 | Liaison vers R1 |
| **R3** | Loopback0 | 10.0.30.129 | /32 | IP Cible (Test) |
| **S2** | Vlan60 | 172.18.60.2 | /28 | Interface de Gestion |

---

## Points Cles de la Configuration

### 1. Agregation de Liens (EtherChannel)
Configuration du protocole LACP (Link Aggregation Control Protocol) entre les commutateurs S1 et S2 pour assurer la redondance et augmenter la bande passante.


! Extrait de configuration S1
interface range FastEthernet0/21-22
 channel-group 1 mode active
!
interface Port-channel1
 switchport mode trunk
 switchport trunk native vlan 50

### 2. Routage Inter-VLAN
Utilisation de sous-interfaces sur le routeur R1 avec encapsulation 802.1Q.

! Extrait de configuration R1
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 172.18.10.14 255.255.255.240
---
### Validation et Tests
Test de Connectivite (Ping & Traceroute)
Le test ci-dessous valide le cheminement complet des paquets depuis le reseau local (VLAN 10) jusqu'au reseau distant simule (R3), traversant la passerelle R1 et le reseau WAN.

Verification de l'EtherChannel
La commande show etherchannel summary confirme que le Port-Channel est actif (SU) et que les ports physiques sont agreges via le protocole LACP (P).

Contenu du Depot
configs/ : Fichiers de configuration brute (.txt) extraits des equipements.

images/ : Captures d'ecran justificatives de la topologie et des tests.

Architecture_Reseau.pkt : Fichier de simulation Cisco Packet Tracer.

---
Projet academique - Boustane Oussama.
