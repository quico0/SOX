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

![cap1](/media/NF1.UD3/1.png)

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

![cap2](/media/NF1.UD3/2.png)

---

## Crear la carpeta Documents

```bash
sudo mkdir /etc/skel/Documents
```

## Verificació

```bash
ls -la /etc/skel
```

![cap3](/media/NF1.UD3/3.png)

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

![cap4](/media/NF1.UD3/4.png)

---

## Comprovar la shell per defecte

```bash
sudo nano /etc/adduser.conf
```

Verificar que existeix:

```text
DSHELL=/bin/bash
```

![cap5](/media/NF1.UD3/5.png)

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

![cap6](/media/NF1.UD3/6.png)

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

![cap7](/media/NF1.UD3/7.png)

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

![cap8](/media/NF1.UD3/8.png)


```bash
sudo usermod -g devs pau_dev
```

![cap9](/media/NF1.UD3/9.png)

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

![cap10](/media/NF1.UD3/10.png)

---

## Crear marc_sys

```bash
sudo useradd -m -d /home/marc_sys -s /bin/bash -g sysadmin -G devs -c "Marc Soler" marc_sys
```

![cap11](/media/NF1.UD3/11.png)

---

## Crear auditor

```bash
sudo useradd -m -d /home/auditor -s /bin/bash -g auditor -c "Usuari Auditor" auditor
```

![cap12](/media/NF1.UD3/12.png)

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

![cap13](/media/NF1.UD3/13.png)

---

## Forçar canvi de contrasenya a pau_dev

```bash
sudo chage -d 0 pau_dev
```

Verificació:

```bash
sudo chage -l pau_dev
```

![cap14](/media/NF1.UD3/14.png)

---

## Verificar canvi obligatori

```bash
su - pau_dev
```

Resultat esperat:

```text
You are required to change your password immediately.
```

![cap15](/media/NF1.UD3/15.png)

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

![cap16](/media/NF1.UD3/16.png)

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

![cap17](/media/NF1.UD3/17.png)

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

![cap18](/media/NF1.UD3/18.png)

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

![cap19](/media/NF1.UD3/19.png)

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

![cap20](/media/NF1.UD3/20.png)

## Conclusió

El directori plantilla permet definir configuracions comunes per a tots els usuaris nous.

---

# Tasca 5. Manteniment, bloqueig i eliminació

## Bloquejar el compte de pau_dev

```bash
sudo passwd -l pau_dev
```

![cap21](/media/NF1.UD3/21.png)

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

![cap22](/media/NF1.UD3/22.png)

---

## Desbloquejar el compte

```bash
sudo passwd -u pau_dev
```

Comprovació:

```bash
sudo grep pau_dev /etc/shadow
```

![cap23](/media/NF1.UD3/23.png)

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

![cap24](/media/NF1.UD3/24.png)

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

![cap25](/media/NF1.UD3/25.png)

---

## Comprovar la ruta de fish

```bash
which fish
```

Resultat:

```text
/usr/bin/fish
```

![cap26](/media/NF1.UD3/26.png)

---

## Canviar la shell de laura_dev

```bash
sudo usermod -s /usr/bin/fish laura_dev
```

![cap27](/media/NF1.UD3/27.png)

---

## Verificar el canvi a /etc/passwd

```bash
grep laura_dev /etc/passwd
```

![cap28](/media/NF1.UD3/28.png)

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

![cap29](/media/NF1.UD3/29.png)

## Conclusió

L'usuari laura_dev utilitza correctament la shell Fish.

