## A simple role to configure Wireguard VPN-server on Ubuntu Focal/Jammy with enabled source NAT and ipv4 routing.
First of all you need to create key pairs for all your client devices. Here is an example for linux:
```
$ wg genkey 
CIKUAseFZd5YQX7NewXbufd1se1czqgrttMGNsoH/Gw=
$ echo CIKUAseFZd5YQX7NewXbufd1se1czqgrttMGNsoH/Gw= | wg pubkey 
ADfHS+4vxkV0+0tq0cALoqCqb08hg2A9Q7r2BwncdTo=
```
The first string is your private key (client private key), the second one is your public key, you gonna need to put it in "wg_vpn_server_clients" variable.<br />
### Process overview
- Install wireguard, iptables-persistent and netfilter-persistent;
- Make server private and public keys, store them on the remote machine (in /root) and show them in the output;
- Make wireguard (wg0 tun interface) and iptables configs based on the templates;
- Enable wireguard service (wg-quick) and ipv4 packet forwarding;
- Generate client config examples (stored on ansible host in the project directory) based on the provided pubkeys. You will have to insert your private keys there;
- Reboot VPN-server.

### Client configuration
For a client configuration you can take an example config file from the project directory, put there your private key, then save it into a file (for example /etc/wireguard/my-vpn.conf) and then enable the service:
```
sudo systemctl enable --now wg-quick@my-vpn
```
That's all.
