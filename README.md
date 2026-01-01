cat << 'EOF' > README.md
# 🌐 Architecture Réseau d'Entreprise : Switching, Routing & WAN

![Cisco Packet Tracer](https://img.shields.io/badge/Technologie-Cisco%20Packet%20Tracer-blue?logo=cisco&logoColor=white)
![Status](https://img.shields.io/badge/Etat-Finalise-success)

## 📖 Description du Projet

Ce projet, réalisé dans le cadre du module **Réseaux Informatiques**, consiste en la conception et le déploiement d'une infrastructure réseau complète simulant un siège social connecté à des sites distants.

L'objectif est de démontrer la maîtrise des protocoles de **Commutation (Switching)**, de **Routage Inter-VLAN** et d'**Interconnexion WAN**.

* **Auteur :** [Boustane Oussama](https://www.linkedin.com/in/oussama-boustane-22a990298/)
* **Année Académique :** 2025/2026

---

## 🏗️ Topologie et Architecture

Le réseau s'articule autour d'un cœur de réseau (R1) et d'une couche distribution/accès.

![Topologie du Réseau](images/topologie_globale.png)

### Inventaire Technique

| Equipement | Modèle | Rôle Principal |
| :--- | :--- | :--- |
| **Routeurs (x3)** | Cisco 2811 | Cœur de réseau, Passerelle WAN & Internet |
| **Switchs (x2)** | Cisco 2960 | Distribution, Accès & Agrégation |
| **Clients** | PC Génériques | Simulation des départements (VLANs) |

---

## 📊 Plan d'Adressage IP (VLSM)

Optimisation de l'espace d'adressage pour les liaisons WAN et LAN.

| Périphérique | Interface | IP / Masque | Description |
| :--- | :--- | :--- | :--- |
| **R1 (Cœur)** | Fa0/0.10 | 172.18.10.14 /28 | Passerelle VLAN 10 |
| | S0/0/0 | 10.0.30.177 /30 | Liaison vers FAI (R2) |
| **R2 (FAI)** | S0/0/0 | 10.0.30.178 /30 | Liaison vers R1 |
| **R3 (Internet)**| Loopback0| 10.0.30.129 /32 | Serveur Distant (Test) |

---

## ⚙️ Implémentation Technique

### 1. Commutation Avancée (Switching)
* **VLANs :** Segmentation (10, 20, 30, 50, 60).
* **EtherChannel (LACP) :** Agrégation de bande passante entre S1 et S2.

```text
! Extrait Config S1 (LACP)
interface range FastEthernet0/21-22
 channel-group 1 mode active
!
interface Port-channel1
 switchport mode trunk
