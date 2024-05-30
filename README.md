# INF1070ETE2024-LAB5

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

6.
```sh
grep ^chat $dict | column -c 100
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
cut -f 2,8 countryInfo-clean-clean.txt | sort -k2 -nr | head -10
CHN     1411778724
IND     1352617328
USA     327167434
IDN     267663435
PAK     212215030
BRA     209469333
NGA     195874740
BGD     161356039
RUS     144478050
JPN     126529100
```
`-f 2,8` : spécifie de couper les colonnes 2 et 8. 
`-k2` : spécifie la 2e colonne.
`-nr` : par ordre numérique à l'envers.

Alternative : 
```sh 
cut -f 2-8 countryInfo-clean-clean.txt | cut --complement -f 2-6 | sort -k2 -nr | head -10
CHN     1411778724
IND     1352617328
USA     327167434
IDN     267663435
PAK     212215030
BRA     209469333
NGA     195874740
BGD     161356039
RUS     144478050
JPN     126529100
```

2. 
```sh
cut -f 2,7 countryInfo-clean-clean.txt | sort -k2 -nr | head -20
RUS     17100000
ATA     14000000
CAN     9984670
USA     9629091
CHN     9596960
BRA     8511965
AUS     7686850
IND     3287590
ARG     2766890
KAZ     2717300
DZA     2381740
COD     2345410
GRL     2166086
MEX     1972550
SAU     1960582
IDN     1919440
SDN     1861484
LBY     1759540
IRN     1648000
MNG     1565000
```

3. 
```sh
cut -f 11 countryInfo-clean-clean.txt | sort | uniq | wc -l
155
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

### 5. CA 

1. 
```sh
cut -f 18 CA-clean.txt | sort | uniq | grep -v "N/A" | wc -l
38
```

2. 
```sh
grep "Longueuil" CA-clean.txt | wc -l
18
```

3. 
```sh
cut -f 19 CA-clean.txt | grep "2018" | wc -l
26871
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



