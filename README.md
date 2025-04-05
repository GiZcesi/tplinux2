# Recap des actions pour TP2


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


