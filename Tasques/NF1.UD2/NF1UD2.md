# Activitat 1 - Administració d'Ubuntu Server 

## Alumne
**Nom:** Quico  
**Curs:** 2n SMX  
**Sistema Operatiu:** Ubuntu Server 

---

# 1. Connexió remota amb SSH

## 1.1 Obtenció de l'adreça IP

### Comanda

```bash
ip a
```

### Explicació

He executat la comanda `ip a` per visualitzar totes les interfícies de xarxa disponibles. He identificat la segona adreça IP, corresponent a la interfície Host-Only, que serà la utilitzada per connectar-me remotament des del sistema Windows.

![Imatge Ip Servidor](/Tasques/NF1.UD2/img/1.png)

---

## 1.2 Connexió SSH

### Comanda

```bash
ssh usuari@adreca_ip
```

### Explicació

Des del Terminal de Windows he establert una connexió remota amb el servidor Ubuntu utilitzant el protocol SSH.

![Conexio ssh](/Tasques/NF1.UD2/img/2.png)

---


# 2. Actualitzacions del sistema

## 2.1 Comprovació d'actualitzacions

### Comanda

```bash
sudo apt update
```

### Explicació

He actualitzat la informació dels repositoris per comprovar si existeixen paquets pendents d'actualitzar.

![Comprovacio Actualitzacio](/Tasques/NF1.UD2/img/3.png)
---


## 2.2 Actualització del sistema


```bash
sudo apt upgrade 
```

### Explicació

S'han actualitzat tots els paquets instal·lats a les versions més recents disponibles als repositoris.

### Captura

![Actualitzacio](/Tasques/NF1.UD2/img/4.png)

---

# 3. Canvi del nom de l'equip

## 3.1 Comprovació del nom actual

### Comanda

```bash
hostnamectl
```

### Captura

![Canvi Nom](/Tasques/NF1.UD2/img/5.png)

---

## 3.2 Canvi del hostname

### Comanda

```bash
sudo hostnamectl set-hostname sox-qcv
```

### Explicació

He canviat el nom permanent de l'equip utilitzant les meves inicials.

### Captura

![Canvi Nom](/Tasques/NF1.UD2/img/6.png)

---

## 3.3 Configuració del nom descriptiu

### Comanda

```bash
sudo hostnamectl set-icon-name "Servidor de Quico"
```

### Explicació

He afegit una descripció identificativa per al servidor.

### Captura

![Canvi Nom](/Tasques/NF1.UD2/img/7.png)

---

## 3.4 Verificació dels canvis

### Comanda

```bash
hostnamectl
```

### Captura

![Canvi Nom](/Tasques/NF1.UD2/img/8.png)

---

## 3.5 Diferència entre hostname i hostnamectl

### Comanda

```bash
hostname
```

### Explicació

La comanda `hostname` només mostra el nom curt del servidor. En canvi, `hostnamectl` proporciona informació més completa sobre el sistema i la seva configuració.

### Captura

![Modificar Fitxer](/Tasques/NF1.UD2/img/9.png)
---

## 3.6 Modificació del fitxer hosts

### Comanda

```bash
sudo nano /etc/hosts
```

### Contingut

```text
127.0.0.1 localhost
127.0.1.1 sox-qcc.sox.test sox-qcc
```

### Explicació

He actualitzat el fitxer perquè el servidor resolgui correctament el nou nom configurat.

### Captura

![Modificar Fitxer](/Tasques/NF1.UD2/img/10.png)

---

## 3.7 Hostname complet

### Comanda

```bash
hostname -f
```

### Explicació

La comanda mostra el nom complet de domini (FQDN), mentre que `hostname` només mostra el nom curt.

### Captura

![Hostname complet](/Tasques/NF1.UD2/img/11.png)

---

# 4. Canvi de contrasenya

## 4.1 Modificació de la contrasenya

### Comanda

```bash
passwd
```

### Explicació

He canviat la contrasenya de l'usuari administrador per una de nova més segura.

### Captura

![Canvi de contrasenya](/Tasques/NF1.UD2/img/12.png)

---

# 5. Gestió de la instal·lació d'aplicacions

## 5.1 Cerca del paquet btop

### Comanda

```bash
apt search btop
```

### Captura

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/13.png)

---

## 5.2 Informació del paquet btop

### Comanda

```bash
apt show btop
```

### Explicació

He consultat informació sobre el paquet, com la versió disponible, dependències i descripció.

### Captura

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/14.png)

---

## 5.3 Instal·lació de btop

### Comanda

```bash
sudo apt install btop -y
```

### Verificació

```bash
btop
```

### Captura

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/15.png)

---

## 5.4 Instal·lació de lsd

### Comandes

```bash
apt search lsd
```

```bash
sudo apt install lsd -y
```

### Verificació

```bash
lsd
```

### Captura

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/16lsd.png)

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/17lsd.png)

---

## 5.5 Instal·lació d'Apache

### Comanda

```bash
sudo apt install apache2 -y
```

### Captura

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/18.png)

---

### Verificació

```bash
systemctl status apache2
```

### Captura

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/19.png)

---

## 5.6 Desinstal·lació d'Apache

### Comanda

```bash
sudo apt purge apache2 -y
```

### Captura

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/20.png)

---

## 5.7 Instal·lació de Micro

### Comanda

```bash
sudo snap install micro --classic
```

### Captura

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/21.png)

---

### Verificació

```bash
snap list
```

### Captura

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/22.png)

## 5.8 Actualització de Micro

### Comanda

```bash
sudo snap refresh micro
```

### Captura

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/23.png)

---

## 5.9 Eliminació de Micro

### Comanda

```bash
sudo snap remove micro
```

### Captura

![Gestió de la instal·lació d'aplicacions](/Tasques/NF1.UD2/img/24.png)

---

# 6. Configuració d'hora, teclat i idioma

## 6.1 Zona horària actual

### Comanda

```bash
timedatectl
```

### Captura

![Configuració d'hora, teclat i idioma](/Tasques/NF1.UD2/img/25.png)

---

## 6.2 Configuració de la zona horària

### Comanda

```bash
sudo timedatectl set-timezone Europe/Madrid
```

### Explicació

He configurat el servidor perquè utilitzi la zona horària d'Espanya.

### Captura

![Configuració d'hora, teclat i idioma](/Tasques/NF1.UD2/img/26.png)
---

## 6.3 Configuració del teclat

### Comanda

```bash
sudo dpkg-reconfigure keyboard-configuration
```

### Captura

![Configuració d'hora, teclat i idioma](/Tasques/NF1.UD2/img/27.png)

---

## 6.4 Configuració de l'idioma

### Comandes

```bash
locale
```

```bash
sudo dpkg-reconfigure locales
```

### Captura

![Configuració d'hora, teclat i idioma](/Tasques/NF1.UD2/img/28.png)

---

# 7. Explorant arxius de configuració

## 7.1 Cerca de fitxers YAML

### Comanda

```bash
find /etc -type f -name "*.yaml"
```

### Captura

![Explorant arxius de configuració](/Tasques/NF1.UD2/img/29.png)

---

## 7.2 Cerca de ca*petes SSH

### Comanda

```bash
find /etc -type d -name "*ssh*"
```

### Captura

![Explorant arxius de configuració](/Tasques/NF1.UD2/img/30.png)

---

*# 7.3 Fitxers més grans de 10 MB

*## Comanda

```bash
find /var/log *type f -size +10M
```

### Captura*

![Explorant arxius de configuració](/Tasques/NF1.UD2/img/31.png)

---

## 7.4 Configuració SSH sense comentaris

### Comanda

```bash
grep -vE '^\s*#|^\s*$' /etc/ssh/sshd_config
```

### Captura

![Explorant arxius de configuració](/Tasques/NF1.UD2/img/32.png)
---

# 8. Configuració de xarxa

## 8.1 Canvi a Adaptador Pont

### Explicació

He modificat la configuració de VirtualBox canviant la xarxa NAT per Adaptador Pont.

### Captura

![Configuració de xarxa](/Tasques/NF1.UD2/img/33.png)

---

## 8.2 Configuració IP fixa

### Comanda

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

### Captura

![Configuració de xarxa](/Tasques/NF1.UD2/img/34.png)

---

## 8.3 Aplicació dels canvis

### Comanda

```bash
sudo netplan apply
```

### Captura

![Configuració de xarxa](/Tasques/NF1.UD2/img/35.png)

---

## 8.4 Verificació

### Comanda

```bash
ip a
```

### Captura

![Configuració de xarxa](/Tasques/NF1.UD2/img/36.png)

---

## 8.5 Connectivitat

### Comanda

```bash
ping 8.8.8.8
```

### Captura

![Configuració de xarxa](/Tasques/NF1.UD2/img/37.png)

---

# 9. Gestió de serveis

## 9.1 Estat del servei SSH

### Comanda

```bash
systemctl status ssh
```

### Captura

![Gestió de serveis](/Tasques/NF1.UD2/img/38.png)

---

## Aturar el servei

### Comanda

```bash
sudo systemctl stop ssh
```

---

## Iniciar el servei

### Comanda

```bash
sudo systemctl start ssh
```

### Captura

![Gestió de serveis](/Tasques/NF1.UD2/img/39.png)

---

# 10. Exportació de la màquina virtual

## Exportació a OVA

### Explicació

He exportat la màquina virtual des de VirtualBox utilitzant l'opció **Fitxer → Exporta servei virtualitzat**, generant un fitxer `.ova` per disposar d'una còpia completa del sistema.

### Captura

![Exportació a OVA](/Tasques/NF1.UD2/img/40.png)

