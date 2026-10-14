# NF1.UD3 - Usuaris i Grups

Quico Carbonell

---

# 1. Creació de la jerarquia de grups

## Comandes utilitzades

```bash
sudo groupadd devs
sudo groupadd sysadmin
sudo groupadd auditor
```

## Verificació

```bash
tail /etc/group
```

## Resultat

```bash
devs:x:1001:
sysadmin:x:1002:
auditor:x:1003:
```

![cap1]()

---

# 2. Gestió dels perfils d'usuari

## Crear un fitxer de benvinguda

```bash
sudo nano /etc/skel/BENVINGUDA.txt
```

### Contingut de BENVINGUDA.txt

```text
Benvingut al servidor TechData S.L.

Normes d'ús:

- No compartir contrasenyes.
- Respectar les polítiques de seguretat.
- Utilitzar els recursos de forma responsable.
```

![cap2]()

---

## Crear la carpeta Documents

```bash
sudo mkdir /etc/skel/Documents
```

## Verificació

```bash
ls -la /etc/skel
```

![cap3]()

---

## Configurar l'alias ll

Editar el fitxer:

```bash
sudo nano /etc/skel/.bashrc
```

Afegir al final:

```bash
alias ll='ls -la --color=auto'
```

![cap4]()

---

## Comprovar la shell per defecte

```bash
sudo nano /etc/adduser.conf
```

Verificar que existeix:

```text
DSHELL=/bin/bash
```

![cap5]()

---

## Crear usuari de prova

```bash
sudo adduser prova04
```

Iniciar sessió:

```bash
su - prova04
```

Verificar:

```bash
ls
```

```bash
ll
```

![cap6]()

---

## Eliminar usuari de prova

Sortir de la sessió:

```bash
exit
```

Eliminar:

```bash
sudo deluser prova04
```

![cap7]()

---

# Tasca 3. Creació i configuració d'usuaris

## Objectiu

Crear els usuaris definits per l'empresa.

---

## Crear pau_dev

```bash
sudo adduser pau_dev
```

Nom complet:

```text
Pau Garcia
```

```bash
sudo usermod -g devs pau_dev
```

## Captura

📸 Creació de pau_dev.

---

## Crear laura_dev

```bash
sudo adduser laura_dev
```

Nom complet:

```text
Laura Martí
```

Assignar grup principal:

```bash
sudo usermod -g devs laura_dev
```

## Captura

📸 Creació de laura_dev.

---

## Crear marc_sys

```bash
sudo useradd -m -d /home/marc_sys -s /bin/bash -g sysadmin -G devs -c "Marc Soler" marc_sys
```

## Captura

📸 Creació de marc_sys.

---

## Crear auditor

```bash
sudo useradd -m -d /home/auditor -s /bin/bash -g auditor -c "Usuari Auditor" auditor
```

## Captura

📸 Creació d'auditor.

---

## Assignar contrasenyes

```bash
sudo passwd pau_dev
sudo passwd laura_dev
sudo passwd marc_sys
sudo passwd auditor
```

Contrasenya:

```text
Canviam2026!
```

## Captura

📸 Assignació de contrasenyes.

---

## Forçar canvi de contrasenya a pau_dev

```bash
sudo chage -d 0 pau_dev
```

Verificació:

```bash
sudo chage -l pau_dev
```

## Captura

📸 Configuració de chage.

---

## Verificar canvi obligatori

```bash
su - pau_dev
```

Resultat esperat:

```text
You are required to change your password immediately.
```

## Captura

📸 Obligació de canvi de contrasenya.

## Conclusió

L'usuari està obligat a canviar la contrasenya en el primer inici de sessió.

---

# Tasca 4. Inspecció del sistema i fitxers de configuració

## Informació de marc_sys

```bash
id marc_sys
```

### Exemple de resultat

```text
uid=1004(marc_sys)
gid=1002(sysadmin)
groups=1002(sysadmin),1001(devs)
```

## Captura

📸 Sortida de la comanda id.

---

## Informació de pau_dev a /etc/passwd

```bash
grep pau_dev /etc/passwd
```

Exemple:

```text
pau_dev:x:1001:1001:Pau Garcia:/home/pau_dev:/bin/bash
```

### Informació que conté

- Nom d'usuari
- UID
- GID
- Nom complet
- Directori personal
- Shell

## Captura

📸 Entrada de passwd.

---

## Informació de pau_dev a /etc/shadow

```bash
sudo grep pau_dev /etc/shadow
```

Exemple:

```text
pau_dev:$y$j9T$xxxxxxxxxxxxxxxxxx
```

### Informació que conté

- Hash de la contrasenya
- Caducitat
- Bloquejos
- Política de contrasenyes

## Captura

📸 Entrada de shadow.

---

## Permisos de /etc/shadow

```bash
ls -l /etc/shadow
```

Exemple:

```text
-rw-r----- 1 root shadow
```

### Explicació

Només root i el grup shadow poden accedir al fitxer perquè conté informació sensible sobre les contrasenyes dels usuaris.

## Captura

📸 Permisos del fitxer.

---

## Directori plantilla

```text
/etc/skel
```

Verificació:

```bash
ls -la /etc/skel
```

També:

```bash
grep SKEL /etc/adduser.conf
```

## Explicació

Quan es crea un nou usuari, el contingut de `/etc/skel` es copia automàticament al seu directori personal.

## Captura

📸 Contingut de `/etc/skel`.

## Conclusió

El directori plantilla permet definir configuracions comunes per a tots els usuaris nous.

---

# Tasca 5. Manteniment, bloqueig i eliminació

## Bloquejar el compte de pau_dev

```bash
sudo passwd -l pau_dev
```

## Captura

📸 Bloqueig del compte.

---

## Comprovar el bloqueig

```bash
sudo grep pau_dev /etc/shadow
```

Resultat:

```text
pau_dev:!$y$...
```

### Explicació

El símbol `!` davant del hash indica que el compte està bloquejat.

## Captura

📸 Entrada bloquejada al fitxer shadow.

---

## Desbloquejar el compte

```bash
sudo passwd -u pau_dev
```

Comprovació:

```bash
sudo grep pau_dev /etc/shadow
```

## Captura

📸 Compte desbloquejat.

---

## Eliminar l'usuari auditor

```bash
sudo userdel -r auditor
```

Comprovació:

```bash
grep auditor /etc/passwd
```

No hauria d'aparèixer cap resultat.

## Captura

📸 Eliminació de l'usuari auditor.

## Conclusió

L'usuari s'ha eliminat correctament juntament amb el directori personal.

---

# Tasca 6. Personalització de l'entorn d'usuari

## Instal·lar Fish Shell

Actualitzar repositoris:

```bash
sudo apt update
```

Instal·lar Fish:

```bash
sudo apt install fish -y
```

## Captura

📸 Instal·lació de fish.

---

## Comprovar la ruta de fish

```bash
which fish
```

Resultat habitual:

```text
/usr/bin/fish
```

## Captura

📸 Ruta de fish.

---

## Canviar la shell de laura_dev

```bash
sudo usermod -s /usr/bin/fish laura_dev
```

## Captura

📸 Modificació de la shell.

---

## Verificar el canvi a /etc/passwd

```bash
grep laura_dev /etc/passwd
```

Exemple:

```text
laura_dev:x:1002:1001:Laura Martí:/home/laura_dev:/usr/bin/fish
```

## Captura

📸 Verificació de passwd.

---

## Iniciar sessió amb laura_dev

```bash
su - laura_dev
```

Comprovar la shell:

```bash
echo $SHELL
```

Resultat:

```text
/usr/bin/fish
```

També es pot comprovar:

```bash
fish --version
```

## Captura

📸 Sessió iniciada amb fish.

## Conclusió

L'usuari laura_dev utilitza correctament la shell Fish.

---

# Conclusions Finals

Durant aquesta pràctica s'ha configurat l'estructura d'usuaris i grups del servidor Ubuntu Server de TechData S.L.

S'han creat grups i usuaris, s'han configurat perfils per defecte mitjançant `/etc/skel`, s'han analitzat els fitxers `/etc/passwd`, `/etc/group` i `/etc/shadow`, s'han realitzat tasques de manteniment de comptes i s'ha personalitzat l'entorn de treball mitjançant Fish Shell.

Aquesta activitat ha permès conèixer els principals mecanismes de gestió d'usuaris, grups i seguretat bàsica en sistemes Linux.
