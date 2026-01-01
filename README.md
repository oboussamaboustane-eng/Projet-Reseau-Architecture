cat << 'EOF' > README.md
<div align="center">

# 📡 Architecture Réseau d'Entreprise
### Switching • Routing Inter-VLAN • WAN

![Cisco Packet Tracer](https://img.shields.io/badge/Technologie-Cisco%20Packet%20Tracer-005571?style=for-the-badge&logo=cisco&logoColor=white)
![Status](https://img.shields.io/badge/État-Finalisé-2ea44f?style=for-the-badge)
![License](https://img.shields.io/badge/Module-Réseaux-333333?style=for-the-badge)

<br>

<p align="center">
  <b>Conception et déploiement d'une infrastructure réseau hiérarchique<br>simulant un siège social connecté à des sites distants.</b>
</p>

[**Boustane Oussama**](https://www.linkedin.com/in/oussama-boustane-22a990298/)
<br>
*Année Académique : 2025/2026*

</div>

---

## 🔹 Topologie et Architecture

Le réseau s'articule autour d'un **Cœur de réseau (Core)** routé et d'une couche **Distribution/Accès** commutée.

<div align="center">
  <img src="images/topologie_globale.png" alt="Topologie Globale" width="800">
</div>

### ➤ Inventaire Matériel

| Equipement | Modèle | Rôle Principal | Zone |
| :--- | :--- | :--- | :--- |
| **R1** | Cisco 2811 | Passerelle LAN & Sortie WAN | Core |
| **R2** | Cisco 2811 | Routeur de Transit (FAI) | WAN |
| **R3** | Cisco 2811 | Site Distant (Internet) | Remote |
| **S1** | Cisco 2960 | Distribution & Agrégation | LAN |
| **S2** | Cisco 2960 | Accès Clients | LAN |

---

## 🔹 Plan d'Adressage (VLSM)

Optimisation de l'espace d'adressage pour garantir l'évolutivité.

| Périphérique | Interface | Adresse IP / Masque | Description |
| :--- | :--- | :--- | :--- |
| **R1** | Fa0/0.10 | `172.18.10.14 /28` | GW VLAN 10 (RH) |
| | S0/0/0 | `10.0.30.177 /30` | Vers R2 (FAI) |
| **R2** | S0/0/0 | `10.0.30.178 /30` | Vers R1 (Siège) |
| **R3** | Loopback0| `10.0.30.129 /32` | Serveur Test |
| **S2** | Vlan60 | `172.18.60.2 /28` | Management |

---

## 🔹 Implémentation Technique

### 1. Commutation (Switching)
> Segmentation logique et agrégation de bande passante.

* **VLANs :** Déploiement des VLANs 10, 20, 30, 50, 60.
* **EtherChannel (LACP) :** Lien redondant entre S1 et S2.
* **Trunking (802.1Q) :** Transport multi-VLANs.

```text
! Extrait Config S1 (LACP)
interface range FastEthernet0/21-22
 channel-group 1 mode active
!
interface Port-channel1
 switchport mode trunk
2. Routage (Routing)
Interconnexion des réseaux locaux et distants.

Router-on-a-Stick : Routage inter-VLAN via sous-interfaces.

Protocole WAN : Encapsulation HDLC sur liaisons séries.

Plaintext

! Extrait Config R1 (ROAS)
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 172.18.10.14 255.255.255.240
🔹 Preuves de Fonctionnement
1. Connectivité WAN (Traceroute)
Le test valide le cheminement : LAN ➤ R1 ➤ WAN (R2) ➤ Cible (R3).

2. Validation EtherChannel
Le statut SU (Layer2 / In Use) et le protocole P (LACP) confirment le succès de l'agrégation.

📂 Contenu du Dépôt
configs/ : Fichiers de configuration brute (.txt).

images/ : Captures d'écran justificatives.

Architecture_Reseau.pkt : Fichier source Packet Tracer.
