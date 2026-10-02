# 🌐 Laboratoire réseau local filaire et Wi‑Fi sécurisé

> Projet personnel réalisé dans le cadre de ma préparation à une **alternance TSSR — Technicien Supérieur Systèmes et Réseaux**.

<p align="center">
  <img src="https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white" alt="Cisco Packet Tracer">
  <img src="https://img.shields.io/badge/Projet-Laboratoire%20TSSR-2E7D32?style=for-the-badge" alt="Laboratoire TSSR">
  <img src="https://img.shields.io/badge/Réseau-192.168.10.0%2F24-1565C0?style=for-the-badge" alt="Réseau IPv4">
  <img src="https://img.shields.io/badge/Sécurité-WPA2%20%2B%20RADIUS-C62828?style=for-the-badge" alt="Sécurité">
</p>

---

## 📄 Documentation complète

Le portfolio PDF présente le laboratoire **étape par étape**, avec les captures d’écran, les configurations, les tests et les explications associées.

### 👉 [Consulter le portfolio PDF](./portfolio_reseau_packet_tracer_TSSR.pdf)

---

## 🎯 Objectif du projet

Mettre en pratique la conception, la configuration, la sécurisation et la validation d’un petit réseau d’entreprise sous **Cisco Packet Tracer**.

Le laboratoire couvre :

- la création d’une topologie réseau complète ;
- l’adressage IPv4 ;
- la configuration d’un serveur DHCP dédié ;
- la mise en place d’un Wi‑Fi interne sécurisé avec **WPA2 Enterprise / RADIUS** ;
- la création d’un réseau Wi‑Fi visiteurs en **WPA2 Personal** ;
- la sécurisation de l’accès local au switch ;
- la validation par des tests de connectivité ;
- le diagnostic et la correction d’une anomalie DHCP.

---

## 🗺️ Architecture du laboratoire

| Équipement               | Quantité | Rôle                             |
| ------------------------ | -------: | -------------------------------- |
| 📶 Routeur Wi‑Fi         |        1 | Wi‑Fi interne et visiteurs       |
| 🔀 Switch manageable     |        1 | Interconnexion du réseau filaire |
| 🖥️ Serveurs              |        3 | DHCP/DNS, RADIUS, stockage       |
| 💻 PC filaires           |        8 | Postes utilisateurs              |
| 🖨️ Imprimantes           |        3 | Ressources réseau                |
| 💻 Laptop interne        |        1 | Client Wi‑Fi interne             |
| 📱 Tablettes internes    |        3 | Clients Wi‑Fi internes           |
| 📷 Caméra IP             |        1 | Équipement Wi‑Fi interne         |
| 👥 Équipements visiteurs |        3 | Clients du Wi‑Fi visiteurs       |

### Réseau utilisé

```text
192.168.10.0/24
```

---

## 🧮 Plan d’adressage

| Équipement            | Adresse IPv4                       | Type     |
| --------------------- | ---------------------------------- | -------- |
| Routeur Wi‑Fi         | `192.168.10.1`                     | Statique |
| SRV-DHCP-DNS          | `192.168.10.10`                    | Statique |
| SRV-RADIUS            | `192.168.10.11`                    | Statique |
| SRV-STOCKAGE          | `192.168.10.12`                    | Statique |
| Imprimante accueil    | `192.168.10.20`                    | Statique |
| Imprimante publique 1 | `192.168.10.21`                    | Statique |
| Imprimante publique 2 | `192.168.10.22`                    | Statique |
| Clients               | `192.168.10.50` à `192.168.10.229` | DHCP     |

### Paramètres distribués par DHCP

```text
Masque       : 255.255.255.0
Passerelle   : 192.168.10.1
DNS          : 192.168.10.10
```

Le service DHCP intégré au routeur est désactivé afin de conserver **une seule source DHCP** sur le réseau.

---

## 📡 Wi‑Fi interne

<p>
  <img src="https://img.shields.io/badge/SSID-WIFI--INTERNE-1565C0?style=flat-square">
  <img src="https://img.shields.io/badge/Sécurité-WPA2%20Enterprise-C62828?style=flat-square">
  <img src="https://img.shields.io/badge/Chiffrement-AES-6A1B9A?style=flat-square">
  <img src="https://img.shields.io/badge/Auth-RADIUS-2E7D32?style=flat-square">
</p>

```text
SSID              : WIFI-INTERNE
Diffusion SSID    : désactivée
Bandes            : 2,4 GHz et 5 GHz
Sécurité          : WPA2 Enterprise
Chiffrement       : AES
Serveur RADIUS    : 192.168.10.11
```

Le serveur RADIUS contient un compte dédié pour chaque équipement Wi‑Fi interne du laboratoire.

> 🔒 Les mots de passe, le secret partagé RADIUS et les autres données sensibles ne sont pas publiés dans ce dépôt.

---

## 👥 Wi‑Fi visiteurs

<p>
  <img src="https://img.shields.io/badge/SSID-WIFI--VISITEURS-F57C00?style=flat-square">
  <img src="https://img.shields.io/badge/Sécurité-WPA2%20Personal-E65100?style=flat-square">
  <img src="https://img.shields.io/badge/Chiffrement-AES-6A1B9A?style=flat-square">
  <img src="https://img.shields.io/badge/Isolation-Guest%20Network-2E7D32?style=flat-square">
</p>

```text
SSID             : WIFI-VISITEURS
Diffusion SSID   : activée
Bandes           : 2,4 GHz et 5 GHz
Sécurité         : WPA2 Personal
Chiffrement      : AES
```

L’isolation des visiteurs repose ici sur la fonction **Guest Network** du routeur Packet Tracer.

> Ce laboratoire n’utilise pas de VLAN visiteurs séparé.

---

## 🛡️ Sécurisation du switch

L’accès local au switch a été protégé avec :

- une bannière d’avertissement ;
- un compte administrateur local ;
- `login local` pour la console ;
- un `enable secret` distinct ;
- `service password-encryption` ;
- une sauvegarde de la configuration dans la `startup-config`.

### Logique de connexion

```text
Connexion console
      ↓
Bannière d’avertissement
      ↓
Username / Password
      ↓
Switch>
      ↓
enable
      ↓
Second mot de passe
      ↓
Switch#
```

---

## 🧪 Tests réalisés

### ✅ Serveur vers routeur

```bash
ping 192.168.10.1
```

Résultat :

```text
4 paquets envoyés
4 réponses reçues
0 % de perte
```

### ✅ Wi‑Fi interne vers imprimante

```bash
ping 192.168.10.20
```

Résultat :

```text
4 paquets envoyés
4 réponses reçues
0 % de perte
```

### ⛔ Wi‑Fi visiteurs vers serveur interne

```bash
ping 192.168.10.10
```

Résultat attendu et obtenu :

```text
0 réponse
100 % de perte
```

Le même test depuis le Wi‑Fi interne reçoit bien des réponses.

---

## 🧰 Compétences mises en pratique

- conception d’une topologie réseau ;
- adressage IPv4 ;
- configuration d’adresses statiques et dynamiques ;
- configuration et diagnostic DHCP ;
- compréhension du rôle de la passerelle et du DNS ;
- WPA2 Enterprise ;
- authentification RADIUS ;
- Wi‑Fi visiteurs ;
- sécurisation d’un switch Cisco en CLI ;
- tests avec `ping` ;
- diagnostic réseau ;
- documentation technique.

---

## 🚧 Limites du laboratoire

Ce laboratoire ne comprend pas :

- VLAN ;
- routage inter‑VLAN ;
- pare-feu dédié ;
- ACL ;
- accès distant SSH au switch ;
- portail captif ;
- supervision réseau ;
- haute disponibilité.

Ces éléments constituent des pistes d’amélioration pour de futurs laboratoires.

---

## ✅ Bilan

Ce laboratoire m’a permis de pratiquer une démarche complète :

```text
Concevoir
   ↓
Configurer
   ↓
Sécuriser
   ↓
Tester
   ↓
Diagnostiquer
   ↓
Documenter
```

L’objectif n’était pas seulement d’obtenir un réseau fonctionnel, mais aussi de comprendre **pourquoi chaque configuration est utilisée**, de savoir **vérifier son fonctionnement** et d’être capable **d’expliquer mes choix simplement**.

---

## 👤 Auteur

Projet personnel réalisé dans le cadre de ma préparation à une **alternance TSSR — Technicien Supérieur Systèmes et Réseaux**.
