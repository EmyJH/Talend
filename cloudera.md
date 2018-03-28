# cloudera vm
```Linux
ifconfig
```
inet adress : 10.0.2.15

Dans windows
C:\Windows\System32\drivers\etc\hosts
192.168.56.101		quickstart.cloudera

hadoop dfsadmin -safemode leave

dans masque de fichier *.csv pour put tous les fichiers csv

# pig
```pig
salarie = LOAD '/user/cloudera/tp_talend/salarie_no_doublons.csv' 
USING PigStorage(';') 
AS (numero:int, nom:chararray, prenom:chararray,civilite:int, CP:int, date_naissance:datetime);

nomEtPrenom = FOREACH salaries GENERATE nom, prenom;
DUMP nomEtPrenom;

```

```pig
salarie = LOAD '/user/cloudera/tp_talend/salarie_no_doublons.csv' 
USING PigStorage(';') 
AS (numero:int, nom:chararray, prenom:chararray,civilite:int, CP:int, date_naissance:chararray);

nomEtPrenom= FOREACH salarie GENERATE nom, ToDate (birthday, 'dd/MM/yyyy') as datenaiss;
salarieciv2 = FILTER salarie BY civilite == 2 ;
STORE salarieciv2 INTO '/user/cloudera/tp_talend/pig1.csv' USING PigStorage (';');
DUMP salarie;
```

```pig
salarie = LOAD '/user/cloudera/tp_talend/salarie_no_doublons' 
USING PigStorage(';') AS (numero : int, nom : chararray, prenom : chararray,
civilite : int , code_postal : int, birthday : chararray);

indvd = FOREACH salarie GENERATE nom, ToDate (birthday, 'dd/MM/yyyy')
AS datenaiss ;

salarieFormate = FOREACH indvd GENERATE numero, nom, ToString 
(datenaiss, 'dd/MM/yyyy'),YearsBetween(CurrentTime(),datenaiss) as age;
STORE salarieFormate INTO '/user/cloudera/tp_talend/pig2.csv' USING PigStorage (';');

DUMP indvd;
```

EEE MMM dd HH:mm:ss CET yyyy

setxkbmap fr pour passer clavier en azerty