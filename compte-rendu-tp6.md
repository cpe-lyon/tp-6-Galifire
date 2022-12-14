*Raphaël DUMAS - 3ICS*

# TP6 - Services Réseau

## Exercice 1 - Addressage IP
    - Réseau : Adresse de sous-réseau | 1e IP | Dernière IP | Broadcast | VLSM?
    - Sous-réseau 1 : 172.16.0.0 | 172.16.0.1 | 172.16.0.38 | 172.16.0.63  | Pas VLSM
    - Sous-réseau 2 : 172.16.0.64 | 172.16.0.65 | 172.16.0.88 | 172.16.0.127 | Pas VLSM
    - Sous-réseau 3 : 172.16.0.128 | 172.16.0.129 | 172.16.0.181 | 172.16.0.191 | Pas VLSM
    - Sous-réseau 4 : 172.16.0.192 | 172.16.0.193 | 172.16.0.228 | 172.16.0.255 | Pas VLSM
    - Sous-réseau 5 : 172.16.1.0 | 172.16.1.1 | 172.16.1.34 | 172.16.1.63 | Pas VLSM
    - Sous-réseau 6 : 172.16.1.64 | 172.16.1.65 | 172.16.1.102 | 172.16.1.127 | Pas VLSM
    - Sous-réseau 7 : 172.16.1.128 | 172.16.1.129 | 172.16.1.154 | 172.16.1.191 | Pas VLSM
35 34 37 25
## Exercice 2 - Préparation de l'environnement

1. Configuration de la VM

2. L'interface loopback est une interface créée par une machine qu'elle peut contacter en boucle. Généralement, cette interface utilise l'ip locale 127.0.0.1.

3. On désinstalle le paquet cloud-init via la commande `sudo apt purge cloud-init -y`

4. On peut modifier le domaine et le nom de la machine grâce à la commande `sudo hostnamectl set-hostname "server.tpadmin.local"`. Puis en l'ajoutant au fichier "/etc/hosts" afin que le changement se perpétue.

## Exercice 3 - Installation du serveur DHCP

1. On installe le paquet isc-dhcp-server via la commande `sudo apt install isc-dhcp-server`

2. Grâce à la commande `sudo addr add 192.168.100.1/24 ens224`, cela permet de configurer une adresse IP pour la nouvelle carte réseau

3. Les lignes "default-lease-time 120;" et "max-lease-time 600;" correspondent au temps de base et maximal de connexion autorisé avant de devoir se reconnecter au réseau.

4. On édite le fichier /etc/default/isc-dhcp-server pour avoir la configuration du serveur DHCP.

5. On redémarre ensuite le serveur DHCP via la commande `sudo systemctl restart isc-dhcp-server`.

6. On doit ensuite modifier le nom de domaine du client ainsi supprimer un package. On entre ainsi les commandes `sudo hostnamectl set-hostname "client.tpadmin.local"` ainsi que `sudo apt purge cloud-init -y`.

7. Les messages affichés dans le fichier syslog résument tout le proccessus du DHCP. DHCPDISCOVER "découvre" une nouvelle adresse IP, DHCPOFFER en propose une sur le sous-réseau, DHCPREQUEST envoie la requête pour confirmer et DHCPACK finalise l'assignement. Le client a bien reçu une adresse dans la plage IP spécifiée dans le fichier de configuration.

8. Le fichier /var/lib/dhcp/dhcpd.leases contient tout l'historique des IP assignées. La commande dhcp-leases-list retourne la liste de toutes les IPs connectées au serveur DHCP, leur addresse MAC ainsi que leur date et heure d'expiration.

9. Lorsque l'on ping le client et le serveur, les 2 répondent, tout fonctionne normalement.

10. On modifie la configuration du serveur DHCP, puis on redémarre le service du côté du client, qui se voit ensuite assigné l'IP statique précisée dans la configuration.

## Exercice 4