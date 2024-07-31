# WireGuard

# Schéma de l'infrastructure WireGuard
```mermaid
graph TD;
    subgraph Configuration
        direction TB
        Ansible[[Ansible]]
        ServerScripts[[Serveur de Scripts]]
        Systemd-networkd[[Systemd-networkd]]
        Netdev+network[[netdev+network]]
    end
    subgraph Serveurs
        direction TB
        style Serveurs fill:,stroke:#333,stroke-width:10px;
        subgraph Central[Serveur Central]
            direction TB
            VPN[VPN WireGuard]
            Firewall[Firewall Port Forward]
        end
        S1((Serveur 1))
        S2((Serveur 2))
        S3((Serveur 3))
    end
   
    subgraph Utilisateurs
        direction TB
        U1[Utilisateur 1]
        U2[Utilisateur 2]
    end
    %% Connexion initiale des utilisateurs au serveur central
    U1 -. Connecté à .-> VPN
    U2 -. Connecté à .-> VPN
    %% Accès aux ports spécifiques via le serveur central et le firewall
    VPN --> Firewall
    Firewall --|Accès à port 5432|--> S1
    Firewall --|Accès à port SSH|--> S1
    Firewall --|Accès à port 5432|--> S2
    Firewall --|Accès à port 443|--> S3
    %% Configuration et scripts
    Serveurs -- configuré par --> Ansible
    Utilisateurs -- configuré par --> Ansible
    ServerScripts -- Fournit des scripts à --> Ansible
    Ansible -- configure --> Systemd-networkd
    Systemd-networkd -- configure --> Netdev+network
```

## Description

Ce projet contient les scripts nécessaires pour configurer et gérer un serveur WireGuard en utilisant systemd-networkd.

## Prérequis

Avant de commencer, assurez-vous d'avoir les éléments suivants :

- Un serveur Linux avec une distribution prise en charge
- Les privilèges d'administration sur le serveur
- Une connexion Internet stable

## Installation

1. Clonez ce dépôt sur votre serveur :

  ```bash
  git clone https://github.com/ArisLegend/Ansible-wireguard-systemd-network.git
  ```

2. Accédez au répertoire du projet :

  ```bash
  cd wireguard
  ```

3. Exécutez le script d'installation qui est dans playbooks/wireguard :

  ```
  ansible-playbook -i inventory.yml wireguard.yml
  ```

