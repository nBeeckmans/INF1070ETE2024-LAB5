# INF1070ETE2024-LAB5

# 0. Notes :

```sh
abc
...
```
`...` indique que l'affichage continue mais est trop long !

# 1. Setup : 

Pour simplifer le travail, nous allons créer une variable qui permet de raccourcir les commandes : 

```sh 
dict="/usr/share/dict/words"
```

ainsi :

```sh
cat $dict 
```

devrait afficher les mots du dictionnaire !

## 2. Questions :

1. 
```sh 
wc -l $dict
346200 /usr/share/dict/words
```
`-l` : spécifie qu'on veut le nombre de lignes. 

2. 
```sh 
grep "with" $dict
```
(Le dictionnaire français ne contient pas de mots avec "with", ainsi je vais prendre chat comme patron à chercher)

```sh 
grep "chat" $dict
achat
achats
chat
chateaubriand
chateaubriands
....
```
(Alternative avec Chat.)

3. 
```sh 
grep ^chat $dict
chat
chateaubriand
chateaubriands
chat-huant
chatière
chatières
chatoie
...
```

4. 
```sh
 grep chat$ $dict
achat
chat
crachat
entrechat
exarchat
galuchat
...
```

5. 
```sh 
grep \'s$ $dict
man's
traveller's
```
```sh 
grep "'s$" $dict
man's
traveller's
```

6.
```sh
grep ^chat $dict | column -c 100 ­> chat.txt
chat                    chatonne                chatouillassions        chatoyâmes
chateaubriand           chatonné                chatouillât             chatoyant
chateaubriands          chatonnée               chatouillâtes           chatoyante
...
```
`-c 100` : spécifie que la longueur d'une ligne est de 100 characters maximum ! 

7. 
```sh 
grep ^[A-Z] $dict > noms-propres.txt 
```

```sh 
cat noms-propres.txt
```

Il n'y a pas de noms propres dans le dictionnaire... 
Mais vous pouvez faire ça avec des chats :D 


## 3. Base de données : 

### 1. Setup 

Téléchargements : 
```sh 
wget http://download.geonames.org/export/dump/CA.zip
```

```sh 
wget http://download.geonames.org/export/dump/countryInfo.txt
```

```sh 
wget http://download.geonames.org/export/dump/readme.txt
```

Décompression : 
```sh
unzip CA.zip 
Archive:  CA.zip
replace readme.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: n
  inflating: CA.txt     
```

```sh 
wc -lc CA.txt 
  315245 35021172 CA.txt
```

### 2. Clean Up 
```sh
wget https://gitlab.info.uqam.ca/lord_mel/inf-1070-labs-perso/-/raw/master/labo05/clean
python3 clean CA.txt > CA-clean.txt
python3 clean countryInfo.txt > countryInfo-clean.txt 
```
Ce sont des scripts python donc il faut exécuter les scripts avec python3 !

### 3. Clean up 2
```sh 
grep -v '^#' countryInfo-clean.txt > countryInfo-clean-clean.txt 
```

`-v` : indique qu'on fait une "anti-recherche", ainsi on prend les lignes qui ne commence _pas_ par "#" 

### 4. Recherche sur les Pays
1. 
```sh
cut -f 2,8 countryInfo-clean-clean.txt | sort -k2 -nr | cut -f 1 | head -10
CHN     
IND     
USA     
IDN     
PAK     
BRA     
NGA     
BGD     
RUS     
JPN     
```
`-f 2,8` : spécifie de couper les colonnes 2 et 8. 
`-k2` : spécifie la 2e colonne.
`-nr` : par ordre numérique à l'envers.

Alternative : 
```sh 
cut -f 2-8 countryInfo-clean-clean.txt | cut --complement -f 2-6 | sort -k2 -nr | cut -f 1 | head -10
CHN     
IND     
USA     
IDN     
PAK     
BRA     
NGA     
BGD     
RUS     
JPN     
```

2. 
```sh
cut -f 2,7 countryInfo-clean-clean.txt | sort -k2 -nr | cut -f 1 | head -20
RUS     
ATA     
CAN     
USA     
CHN     
BRA     
AUS     
IND     
ARG     
KAZ     
DZA     
COD     
GRL     
MEX     
SAU     
IDN     
SDN     
LBY     
IRN     
MNG     
```

3. 
```sh
cut -f 12 countryInfo-clean-clean.txt | sort | uniq | wc -l
81
```
`sort | uniq` : permet d'extraire les lignes uniques seulement ! 

4. 
```sh
cut -f 9 countryInfo-clean-clean.txt | sort | uniq -c 
     58 AF
      5 AN
     51 AS
     54 EU
     42 NA
     28 OC
     14 SA
```
`-c ` : permet de compter les lignes qui se répètent.
grep "Longueuil" CA-clean.txt | wc -l
18

### 5. CA 

1. 
```sh
cut -f 18 CA-clean.txt | sort | uniq -c
   1129 America/Atikokan
   1808 America/Blanc-Sablon
   1633 America/Cambridge_Bay
     54 America/Chicago
    126 America/Creston
     22 America/Dawson
   1165 America/Dawson_Creek
     58 America/Denver
...
```

2. 
```sh
 grep "Longueuil" CA-clean.txt 
 5893322	Baronnie de Longueuil	Baronnie de Longueuil	N/A	45.40008	-73.34916	LAREA	CA	N/A	10	N/A	N/A	N/A	0	N/A	20	America/Toronto	2006-01-18
... 
```

```sh
 grep "Longueuil" CA-clean.txt | wc -l
 18
```

3. 
```sh
grep "2018-[0-9][0-9]-[0-9][0-9]$" CA-clean.txt 
3424953	Virgin Rocks	Virgin Rocks	N/A	46.42886	-50.81995	U	RFUCA	N/A	05	N/A	N/A	N/A	0	N/A	-9999	N/A	2018-02-20
...
```

4. 
En 4 étapes : 

```sh
cut -f 5 CA-clean.txt | sort -n | tail -1
83.12224
```

```sh
cut -f 5 CA-clean.txt | sort -n | head -1
41.65444
```

```sh
cut -f 6 CA-clean.txt | sort -n | tail -1
123.13881
```

```sh
cut -f 6 CA-clean.txt | sort -nr | tail -1
-141.00542
```



