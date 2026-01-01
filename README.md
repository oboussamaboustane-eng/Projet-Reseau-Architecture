# 🌐 Architecture Réseau d'Entreprise : Switching, Routing & WAN

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue?logo=cisco&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success)

## 📖 Description du Projet
Ce projet, réalisé dans le cadre du module **Réseaux Informatiques**, consiste en la conception et le déploiement d'une infrastructure réseau complète simulant un siège social connecté à des sites distants.

L'objectif est de démontrer la maîtrise des protocoles de **Commutation (Switching)**, de **Routage Inter-VLAN** et d'**Interconnexion WAN**.

**Étudiant :** [TON NOM PRÉNOM]
**Année :** 2025/2026

---

## 🏗️ Topologie et Architecture

Le réseau est constitué de :
* **3 Routeurs Cisco 2811** (Zone WAN et Cœur de réseau).
* **2 Switchs Cisco 2960-24TT** (Zone LAN Access/Distribution).
* **Postes Clients** répartis sur différents VLANs.

![Topologie du Réseau](images/topologie_globale.png)

---

## ⚙️ Fonctionnalités Configurées

### 1. Commutation (Switching)
* **VLANs :** Segmentation du réseau en 5 VLANs (10, 20, 30, 50, 60).
* **EtherChannel (LACP) :** Agrégation de liens entre S1 et S2.
* **Trunking (802.1Q) :** Transport des VLANs.

### 2. Routage (Routing)
* **Router-on-a-Stick :** Configuration de sous-interfaces sur R1.
* **Routage WAN :** Liaisons séries avec encapsulation HDLC.
* **Routage Statique & Résumé :** Optimisation des tables de routage.

---

## 📸 Preuves de Fonctionnement

### Test de Connectivité WAN (Ping/Traceroute)
Le traceroute ci-dessous démontre que les paquets traversent correctement le réseau local, le routeur central (R1) pour atteindre la cible distante sur Internet (simulée).

![Test Traceroute](images/test_wan_tracert.png)

### Vérification EtherChannel (LACP)
Configuration validée sur le Switch S1 (Flags SU et P).

![Preuve EtherChannel](images/preuve_etherchannel.png)

---

## 📂 Structure du Dépôt

* `/configs` : Fichiers de configuration (Show running-config).
* `/images` : Captures d'écran du projet.
* `Architecture_Reseau.pkt` : Le fichier de simulation Packet Tracer.

---
*Projet académique.*
