# Guia de Tasca: Configuració d'un Servidor i Client NFS

Aquesta guia descriu els passos necessaris per configurar un entorn de compartició de fitxers mitjançant el protocol **NFS (Network File System)**. Aprendrem a preparar un servidor (Ubuntu Server) i un client (Zorin OS) per compartir una carpeta de manera segura i coherent.


---

## Taula de continguts

[Preparació de l'entorn](#preparació-de-lentorn)


---

##  Preparació de l'entorn

Per realitzar aquesta pràctica necessitarem dues màquines virtuals connectades per la mateixa xarxa:

*   **Servidor NFS:** Ubuntu Server
*   **Client NFS:** Zorin OS

---

##  Pas 1: Instal·lació dels paquets necessaris

Abans de començar, assegureu-vos que les màquines tenen accés a Internet i actualitzeu els repositoris.

### 1.1. Al Servidor (Ubuntu Server)

Instal·lem el paquet que proporciona el dimoni del servidor NFS:

```bash
sudo apt update
sudo apt install nfs-kernel-server -y
```

### 1.2. Al Client (Zorin OS)

Instal·lem les utilitats comunes per poder muntar recursos NFS:

```bash
sudo apt update
sudo apt install nfs-common -y
```

---

##  Pas 2: Configuració del Servidor NFS

En aquest pas prepararem el recurs que volem compartir, assignant els permisos adequats i configurant el servei per exportar-lo a la xarxa.

### 2.1. Creació de l'usuari, grup i carpeta compartida

Crearem un usuari i un grup específics per a la pràctica, i els assignarem com a propietaris de la carpeta que compartirem.

```bash
# Creem el grup i l'usuari que serà propietari del recurs (UID i GID 1100)
sudo groupadd -g 1100 grup_smx
sudo useradd -u 1100 -m -g grup_smx usuari_smx

# Crear una carpeta per compartir
sudo mkdir -p /srv/nfs/compartit

# Canviar propietari i grup de la carpeta
sudo chown usuari_smx:grup_smx /srv/nfs/compartit

# Establir permisos d'accés a la carpeta (rwx per al propietari, rx per al grup)
sudo chmod 750 /srv/nfs/compartit
```

### 2.2. Configuració de l'exportació (`/etc/exports`)

El fitxer `/etc/exports` defineix quines carpetes es comparteixen, amb qui i amb quins permisos.

Editem el fitxer:

```bash
sudo nano /etc/exports
```

Afegim la següent línia al final del fitxer (permetem accés a tothom `*` amb lectura/escriptura i sincronització de dades):

```text
/srv/nfs/compartit *(rw,sync,no_subtree_check)
```

> **Explicació de les opcions:**
> - `rw`: Permet lectura i escriptura.
> - `sync`: Força el servidor a esperar que els canvis s'escriguin al disc físic abans de respondre al client (integritat de dades).
> - `no_subtree_check`: Desactiva la comprovació de subàrbres, millorant el rendiment.

### 2.3. Aplicar canvis i reiniciar el servei

Perquè els canvis tinguin efecte, exportem les noves regles i reiniciem el servei:

```bash
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

Per verificar que el servei està funcionant correctament:

```bash
sudo systemctl status nfs-kernel-server
```

---

##  Pas 3: Configuració del Client NFS

### 3.1. Creació d'usuari i grup (Coherència UID/GID)

```bash
sudo groupadd -g 1100 grup_smx
sudo useradd -u 1100 -m -g grup_smx usuari_smx -s /bin/bash
```

### 3.2. Muntatge manual del recurs

Creem un directori local on "connectarem" la carpeta remota i la muntem.

```bash
sudo mkdir -p /mnt/compartit
sudo mount -t nfs servidor_nfs:/srv/nfs/compartit /mnt/compartit
```

### 3.3. Comprovació de funcionament

Verifiquem que l'usuari creat pot interactuar amb el recurs compartit:

```bash
su - usuari_smx

cd /mnt/compartit

touch fitxer_prova.txt

ls -l fitxer_prova.txt
```

*Si el fitxer es crea sense errors de permisos, la configuració és correcta.*

Un cop fetes les proves, sortim de l'usuari (`exit`) i desmuntem la carpeta:

```bash
sudo umount /mnt/compartit
```

---

##  Pas 4: Muntatge permanent (`/etc/fstab`)

El muntatge anterior és temporal (es perd en reiniciar). Per fer-lo automàtic en arrencar el sistema, editarem el fitxer `/etc/fstab` del **client**.

```bash
sudo nano /etc/fstab
```

Afegim la següent línia al final (substituint `IP_servidor_nfs` per la IP real):

```text
IP_servidor_nfs:/srv/nfs/compartit /mnt/compartit nfs defaults 0 0
```

**Explicació de l'entrada:**

| Camp | Descripció |
|------|------------|
| `IP_servidor_nfs:/srv/nfs/compartit` | Ruta del recurs compartit al servidor |
| `/mnt/compartit` | Punt de muntatge local al client |
| `nfs` | Tipus de sistema de fitxers |
| `defaults` | Opcions de muntatge per defecte |
| `0` | No es farà verificació del sistema de fitxers (`dump`) |
| `0` | No es seguirà un ordre específic de muntatge (`fsck`) |

Per provar que l'entrada és correcta sense reiniciar, executem:

```bash
sudo mount -a
```

Si no apareix cap error, el muntatge s'ha configurat correctament.

---

##  Comandes útils de verificació

| Comanda | Descripció |
|---------|------------|
| `showmount -e IP_servidor` | Mostra els recursos exportats pel servidor |
| `mount \| grep nfs` | Llista els recursos NFS muntats actualment |
| `df -h` | Mostra l'espai disponible dels sistemes de fitxers muntats |
| `cat /etc/exports` | Mostra la configuració d'exportació del servidor |

---
