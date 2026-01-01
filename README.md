# Architecture Réseau d'Entreprise : Commutation, Routage et WAN

![Cisco Packet Tracer](https://img.shields.io/badge/Technologie-Cisco%20Packet%20Tracer-blue)
![Status](https://img.shields.io/badge/Etat-Finalise-success)

## Informations Générales

Ce dépôt héberge les ressources techniques et la documentation d'un projet de conception d'infrastructure réseau. Le projet simule un réseau d'entreprise hiérarchique interconnectant un siège social à des sites distants via une liaison WAN.

* **Auteur :** [Boustane Oussama](https://www.linkedin.com/in/oussama-boustane-22a990298/)
* **Contexte :** Module Réseaux Informatiques
* **Année Académique :** 2025/2026

---

## Objectifs Techniques

L'objectif principal est de démontrer la mise en œuvre des technologies suivantes :
1.  **Commutation (Switching) :** Segmentation par VLANs, protocoles de trunking (802.1Q) et agrégation de liens (LACP).
2.  **Routage Inter-VLAN :** Configuration "Router-on-a-Stick" pour la communication entre sous-réseaux.
3.  **Interconnexion WAN :** Liaisons séries et protocoles de routage pour l'accès aux sites distants.
4.  **Optimisation :** Résumé de routes (Route Summarization) pour alléger les tables de routage.

---

## Topologie et Architecture

Le réseau s'articule autour d'un cœur de réseau (R1) gérant le routage inter-VLAN et l'accès WAN, et d'une couche distribution/accès assurée par des commutateurs interconnectés.

![Schéma de la Topologie Globale](images/topologie_globale.png)

### Inventaire du Matériel Simulé

| Équipement | Modèle | Rôle Principal |
| :--- | :--- | :--- |
| **Routeur R1** | Cisco 2811 | Passerelle LAN & Sortie WAN |
| **Routeur R2** | Cisco 2811 | Routeur de transit (FAI/WAN) |
| **Routeur R3** | Cisco 2811 | Destination distante (Simulation Internet) |
| **Switch S1** | Cisco 2960 | Distribution & Agrégation |
| **Switch S2** | Cisco 2960 | Accès Clients & Management |

---

## Plan d'Adressage IP (VLSM)

Un adressage optimisé a été appliqué pour économiser les adresses IPv4, notamment sur les liaisons point-à-point (/30).

| Périphérique | Interface | Adresse IP | Masque (CIDR) | Description |
| :--- | :--- | :--- | :--- | :--- |
| **R1** | Fa0/0.10 | 172.18.10.14 | /28 | Passerelle VLAN 10 |
| | Fa0/0.20 | 172.18.20.14 | /28 | Passerelle VLAN 20 |
| | Fa0/0.30 | 172.18.30.14 | /28 | Passerelle VLAN 30 |
| | S0/0/0 | 10.0.30.177 | /30 | Liaison vers R2 |
| **R2** | S0/0/0 | 10.0.30.178 | /30 | Liaison vers R1 |
| **R3** | Loopback0 | 10.0.30.129 | /32 | IP Cible (Test) |
| **S2** | Vlan60 | 172.18.60.2 | /28 | Interface de Gestion |

---

## Points Clés de la Configuration

### 1. Agrégation de Liens (EtherChannel)
Configuration du protocole LACP (Link Aggregation Control Protocol) entre les commutateurs S1 et S2 pour assurer la redondance et augmenter la bande passante.

```text
! Extrait de configuration S1
interface range FastEthernet0/21-22
 channel-group 1 mode active
!
interface Port-channel1
 switchport mode trunk
 switchport trunk native vlan 50
