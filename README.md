## Initialize the configuration files and certificates
```
**docker-compose run --rm openvpn ovpn_genconfig -u udp://VPN.SERVERNAME.COM**
**docker-compose run --rm openvpn ovpn_initpki**
```
## Start OpenVPN server process

```
**docker-compose up -d openvpn**
```
## You can access the container logs with
```
**docker-compose logs -f**
```
## Generate a client certificate
```
**export CLIENTNAME="your_client_name"**
==> with a passphrase (recommended)
**docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME**
==> without a passphrase (not recommended)
**docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME nopass**
```
## Retrieve the client configuration with embedded certificates
```
**docker-compose run --rm openvpn ovpn_getclient $CLIENTNAME > $CLIENTNAME.ovpn**
```

## Revoke a client certificate
```
- Keep the corresponding crt, key and req files.
**docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME**
- Remove the corresponding crt, key and req files.
**docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME remove**
```
# Debugging Tips

- Create an environment variable with the name DEBUG and value of 1 to enable debug output (using "docker -e").
```
**docker-compose run -e DEBUG=1 -p 1194:1194/udp openvpn**
```
