# Recap des actions pour TP2

## 1. Anatomy of a program


### A. `file`

🌞 **Utiliser `file` pour déterminer le type de :**
- la commande `ls`
- la commande `ip`
- un fichier `.mp3` que vous aurez téléchargé sur le disque de la VM

[giz@localhost /]$``` file /sbin/ip
/sbin/ip: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=77a2f5899f0529f27d87bb29c6b84c535739e1c7, for GNU/Linux 3.2.0, stripped```

[giz@localhost /]$ ``` file /bin/ls
/bin/ls: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=1afdd52081d4b8b631f2986e26e69e0b275e159c, for GNU/Linux 3.2.0, stripped```

[giz@localhost ~]$ ```file random-mp3.mp3
random-mp3.mp3: Audio file with ID3 version 2.4.0, contains:MPEG ADTS, layer III, v1, 64 kbps, 44.1 kHz, Stereo```

### B. `readelf`

🌞 **Utiliser `readelf` sur le programme `ls`**

Commande :
```bash
readelf -h /bin/ls

En-tête ELF:
  Magique:                             7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  Classe:                              ELF64
  Données:                             complément à 2, système à octets de poids faible d'abord (little endian)
  Version:                             1 (actuelle)
  OS/ABI:                              UNIX - System V
  Version ABI:                         0
  Type:                                DYN (fichier objet partagé)
  Machine:                             Advanced Micro Devices X86-64
  Version:                             0x1
  Adresse du point d'entrée:          0x6b10
  Début des en-têtes de programme:    64 (octets dans le fichier)
  Début des en-têtes de section:      139032 (octets dans le fichier)
  Fanions:                             0x0
  Taille de cet en-tête:              64 (octets)
  Taille de l'en-tête du programme:   56 (octets)
  Nombre d'en-têtes du programme:     13
  Taille des en-têtes de section:     64 (octets)
  Nombre d'en-têtes de section:       30
  Table d'index des chaînes d'en-tête de section: 29

readelf -S /bin/ls

Il y a 30 en-têtes de section, débutant à l'adresse de décalage 0x21f18 :

mais ducoup celle-ci que tu voulais -> 

[15] .text PROGBITS 0000000000004d50 00004d50 0000000000012532 0000000000000000 AX 0 0 16
````

### C. `ldd`

🌞 **Utiliser `ldd` sur le programme `ls`**

```libc.so.6 => /lib64/libc.so.6 (0x00007f2c4e200000)```

## 2. Syscalls basics

### A. Syscall list

🌞 **Donner le nom ET l'identifiant unique d'un syscall qui permet à un processus de...**

- lire un fichier stocké sur disque
- écrire dans un fichier stocké sur disque
- lancer un nouveau processus

- Lire un fichier stocké sur disque  
  - **Nom** : `read`  
  - **ID** : `0`  

- Écrire dans un fichier stocké sur disque  
  - **Nom** : `write`  
  - **ID** : `1`  

- Lancer un nouveau processus  
  - **Nom** : `execve`  
  - **ID** : `59`  


🌞 **Utiliser `objdump`** sur la commande `ls`

Commande utilisée :
```bash
objdump -d -j .text /bin/ls | less
``` 

et résultat : 
```bash
0000000000004d50 <_obstack_begin@@Base-0xb090>:
    4d50:       50                      push   %rax
    4d51:       e8 da f9 ff ff          callq  4730 <abort@plt>
    4d56:       e8 d5 f9 ff ff          callq  4730 <abort@plt>
    4d5b:       e8 d0 f9 ff ff          callq  4730 <abort@plt>
    4d60:       e8 cb f9 ff ff          callq  4730 <abort@plt>
    4d65:       e8 c6 f9 ff ff          callq  4730 <abort@plt>
    4d6a:       e8 c1 f9 ff ff          callq  4730 <abort@plt>
    4d6f:       e8 bc f9 ff ff          callq  4730 <abort@plt>
    4d74:       e8 b7 f9 ff ff          callq  4730 <abort@plt>
    4d79:       e8 b2 f9 ff ff          callq  4730 <abort@plt>
``` 

🌞 **Utiliser `objdump`** sur la librairie Glibc

```bash
ldd /bin/ls

libc.so.6 => /lib64/libc.so.6 (0x00007f21aa712000)
??? pas trouvé le syscall :( y'en avais trop... ????
````


# Part II : Observe

🌞 **Utiliser `strace` pour tracer l'exécution de la commande `ls`**

### Commande utilisée :
```bash
strace ls /etc

write(1, "passwd\ngroup\nhostname\nhosts\n...\n", 100) = 100
```

🌞 **Utiliser `strace` pour tracer l'exécution de la commande `cat`**

Commande utilisée :
```bash
strace cat /etc/passwd
```
appel pour open le file
```bash
openat(AT_FDCWD, "/etc/passwd", O_RDONLY) = 3
```
appel pour ecrire dans le file si jai bien compris
```bash
write(1, "root:x:0:0:root:/root:/bin/bash\n"..., 1953root:x:0:0:root:/root:/bin/bash
```

🌞 **Utiliser `strace` pour tracer l'exécution de `curl example.org`**


### Commande utilisée :
```bash
strace -c curl google.com
```

## 2. sysdig

### B. Use it

🌞 **Utiliser `sysdig` pour tracer les *syscalls*  effectués par `ls`**

quand je  sysdig -s2000 evt.type=write and proc.name=ls.

J'ai un blank return jsp pourquoi :/ ...

🌞 **Utiliser `sysdig` pour tracer les *syscalls*  effectués par `cat`**

disons je cat "cat ````/etc/ostree/ostree-remotes.conf````
```bash
openat(AT_FDCWD, "/etc/ostree/ostree-remotes.conf", O_RDONLY) = 3
write(1, "remote \"centos\" { ... }\n", 30) = 30
```


🌞 **Utiliser `sysdig` pour tracer les *syscalls*  effectués par votre utilisateur**

sudo sysdig -p"%evt.datetime %proc.name %evt.args" user.giz

 🌞 **Livrez le fichier `curl.scap` dans le dépôt git de rendu**

la meme blank quand j'essaye ````sudo sysdig -w curl_capture.scap proc.name=curl and evt.arg.url=google.com````
pas de retour du terminal relou jsp.... ptn

# Part III : Service Hardening

## 1. Install NGINX
# OK & Config

🌞 **Tracer l'exécution du programme NGINX**

```bash
81094 res=1
81095 flags=0
81102 fd=12(<4t>127.0.0.1:53862->127.0.0.1:80) tuple=127.0.0.1:53862->127.0.0.1:80 queuepct=0 queuelen=0 queuemax=511
81104
81105
81106 maxevents=512
81107 res=1
81108 fd=12(<4t>127.0.0.1:53862->127.0.0.1:80) size=1024
81109 res=73 data=GET / HTTP/1.1..Host: 127.0.0.1..User-Agent: curl/7.76.1..Accept: */*.... tuple=127.0.0.1:53862->127.0.0.1:80
81110
81111 res=0 dirfd=-100(AT_FDCWD) path=/usr/share/nginx/html/index.html flags=0
81112 dirfd=-100(AT_FDCWD) name=/usr/share/nginx/html/index.html flags=65(O_NONBLOCK|O_RDONLY) mode=0
81113 fd=13(<f>/usr/share/nginx/html/index.html) dirfd=-100(AT_FDCWD) name=/usr/share/nginx/html/index.html flags=65(O_NONBLOCK|O_RDONLY) mode=0 dev=FD00 ino=17754904
81114 fd=13(<f>/usr/share/nginx/html/index.html)
81115 res=0
81116
81117 res=0 fd=12(<4t>127.0.0.1:53862->127.0.0.1:80) level=2(SOL_TCP) optname=0(UNKNOWN) val=.... optlen=4
81118 fd=12(<4t>127.0.0.1:53862->127.0.0.1:80) size=240
81119 res=240 data=HTTP/1.1 200 OK..Server: nginx/1.20.1..Date: Sun, 06 Apr 2025 01:20:33 GMT..Cont
81120 out_fd=12(<4t>127.0.0.1:53862->127.0.0.1:80) in_fd=13(<f>/usr/share/nginx/html/index.html) offset=0 size=7620
81121 res=7620 offset=7620
81122 fd=5(<f>/var/log/nginx/access.log) size=91
81123 res=91 data=127.0.0.1 - - [06/Apr/2025:03:20:33 +0200] "GET / HTTP/1.1" 200 7620 "-" "curl/7
81124 fd=13(<f>/usr/share/nginx/html/index.html)
81125 res=0
81126
81127 res=0 fd=12(<4t>127.0.0.1:53862->127.0.0.1:80) level=2(SOL_TCP) optname=0(UNKNOWN) val=.... optlen=4
81128 maxevents=512
81130 next=0 pgft_maj=0 pgft_min=305 vm_size=15556 vm_rss=5560 vm_swap=0
84483 res=1
84484 fd=12(<4t>127.0.0.1:53862->127.0.0.1:80) size=1024
84485 res=0 data=NULL tuple=127.0.0.1:53862->127.0.0.1:80
84486 fd=12(<4t>127.0.0.1:53862->127.0.0.1:80)
84487 res=0
84488 maxevents=512
84489 next=0 pgft_maj=0 pgft_min=305 vm_size=15556 vm_rss=5560 vm_swap=0
```
