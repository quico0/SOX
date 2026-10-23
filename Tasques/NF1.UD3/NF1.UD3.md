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

![cap1](/Tasques/NF1.UD3/img/1.png)

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

![cap2](/Tasques/NF1.UD3/img/2.png)


---

## Crear la carpeta Documents

```bash
sudo mkdir /etc/skel/Documents
```

## Verificació

```bash
ls -la /etc/skel
```

![cap3](/Tasques/NF1.UD3/img/3.png)
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

![cap4](/Tasques/NF1.UD3/img/4.png)

---

## Comprovar la shell per defecte

```bash
sudo nano /etc/adduser.conf
```

Verificar que existeix:

```text
DSHELL=/bin/bash
```

![cap5](/Tasques/NF1.UD3/img/5.png)

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

![cap6](/Tasques/NF1.UD3/img/6.png)

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

![cap7](/Tasques/NF1.UD3/img/7.png)

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

![cap8](/Tasques/NF1.UD3/img/8.png)


```bash
sudo usermod -g devs pau_dev
```

![cap9](/Tasques/NF1.UD3/img/9.png)
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

![cap10](/Tasques/NF1.UD3/img/10.png)

---

## Crear marc_sys

```bash
sudo useradd -m -d /home/marc_sys -s /bin/bash -g sysadmin -G devs -c "Marc Soler" marc_sys
```

![cap11](/Tasques/NF1.UD3/img/11.png)

---

## Crear auditor

```bash
sudo useradd -m -d /home/auditor -s /bin/bash -g auditor -c "Usuari Auditor" auditor
```

![cap12](/Tasques/NF1.UD3/img/12.png)

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

![cap13](/Tasques/NF1.UD3/img/13.png)

---

## Forçar canvi de contrasenya a pau_dev

```bash
sudo chage -d 0 pau_dev
```

Verificació:

```bash
sudo chage -l pau_dev
```

![cap14](/Tasques/NF1.UD3/img/14.png)

---

## Verificar canvi obligatori

```bash
su - pau_dev
```

Resultat esperat:

```text
You are required to change your password immediately.
```

![cap15](/Tasques/NF1.UD3/img/15.png)

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

![cap16](/Tasques/NF1.UD3/img/16.png)

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

![cap17](/Tasques/NF1.UD3/img/17.png)

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

![cap18](/Tasques/NF1.UD3/img/18.png)

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

![cap19](/Tasques/NF1.UD3/img/19.png)

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

![cap20](/Tasques/NF1.UD3/img/20.png)

## Conclusió

El directori plantilla permet definir configuracions comunes per a tots els usuaris nous.

---

# Tasca 5. Manteniment, bloqueig i eliminació

## Bloquejar el compte de pau_dev

```bash
sudo passwd -l pau_dev
```

![cap21](/Tasques/NF1.UD3/img/21.png)

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

![cap22](/Tasques/NF1.UD3/img/22.png)

---

## Desbloquejar el compte

```bash
sudo passwd -u pau_dev
```

Comprovació:

```bash
sudo grep pau_dev /etc/shadow
```

![cap23](/Tasques/NF1.UD3/img/23.png)

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

![cap24](/Tasques/NF1.UD3/img/24.png)

---

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

![cap25](/Tasques/NF1.UD3/img/25.png)


---

## Comprovar la ruta de fish

```bash
which fish
```

Resultat:

```text
/usr/bin/fish
```

![cap26](/Tasques/NF1.UD3/img/26.png)


---

## Canviar la shell de laura_dev

```bash
sudo usermod -s /usr/bin/fish laura_dev
```

![cap27](/Tasques/NF1.UD3/img/27.png)

---

## Verificar el canvi a /etc/passwd

```bash
grep laura_dev /etc/passwd
```

![cap28](/Tasques/NF1.UD3/img/28.png)

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

![cap29](/Tasques/NF1.UD3/img/29.png)

