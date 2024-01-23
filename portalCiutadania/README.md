# API per a la recollida automatitzada de notificacions 

1. [Introducció](#1-introducció)
   1. [Autenticacio](#autenticació)
   2. [Representacions](#representacions)
   3. [Entorns](#entorns)
   4. [Com demanar l'alta al servei](#com-demanar-lalta-al-servei)
2. [Operacions](#2-operacions)
   1. [Cerca Notificacions](#21-cerca-notificacions)
   2. [Consultar Notificació](#22-consulta-notificació)
   3. [Practicar Notificació](#23-practicar-notificació)
   4. [Obtenir document d'una notificació](#24-obtenir-document-duna-notificació)
   5. [Obtenir justificant PDF de la pràctica d'una notificació](#25-obtenir-justificant-pdf-de-la-pràctica-duna-notificació)
   6. [Resum de notificacions](#26-resum-de-notificacions)
   7. [Informació del servei](#27-informació-del-servei)
   8. [Model de dades dels recursos](#model-de-dades-dels-recursos)
3. [Respostes en cas d'error](#3-respostes-en-cas-derror)

# 1. Introducció

A continuació es descriu l'API que proporciona **eNotum** per la recollida automatitzada de notificacions per part dels destinataris o dels representants d'aquests.

Aquesta API va dirigida a tant a destinataris que reben moltes notificacions com a les plataformes de recollida de notificacions de tercers que automatitzen la recollida periòdica de les notificacions de múltiples destinataris.

L'API de recollida de notificacions és una API de tipus REST amb missatges en format JSON. L'autenticació dels usuaris es farà via autenticació mútua de certificats i es podrà operar en nom d'un altre destinatari de les notificacions a través de les representacions donades d'alta a Representa o amb un certificat de representant.

Aquesta API segueix el principi `Open/Closed` de manera que en un futur es podran introduir modificacions a la API garantint la compatibilitat sempre que els canvis que s'introdueixin es restringeixin exclusivament a afegir nous camps sense modificar els existents o a afegir noves operacions. De manera que els integradors han de garantir que les seves implementacions no fallen si en algun moment s'afegeix un camp extra en una resposta. En cas que sigui necessari fer un canvi en una operació existent que pugui trencar les integracions realitzades es prendrà la decisió de versionar l'operació o fer el canvi sobre l'operació existent. En aquest últim cas s'avisarà amb temps als integradors.

El servei només estarà obert per les aplicacions que s'hagin donat d'alta previament (veure apartat [Com demanar l'alta al servei](#com-demanar-lalta-al-servei)). Per garantir-ne el bon ús es realitzarà un filtrat per IP i s'aplicaran quotes de volum de peticions.

## Autenticació

Les aplicacions que vulguin utilitzar l'API de recollida de notificacions s'han d'autenticar seguint el protocol *Mutual TLS* (mTLS) i presentant el certificat de la persona de la que volen consultar les notificacions o del representant d'aquesta.

> :information_source: Els certificats de representant d'entitat amb personalitat jurídica o els certificats de representant d'entitat sense personalitat jurídica son considerats certificats de persona física i només es podrà operar sobre les notificacions i comunicacions de la persona física. Si voleu consultar les notificacions i comunicacions de l'entitat representada s'ha de demanar explicitament com s'indica al punt següent.

## Representacions

L'API de recollida de notificacions permet als usuaris actuar com a representants d'una tercera persona física o jurídica. La representació s'acredita via la utilització d'un certificat de representant o donant d'alta una representació al servei Representa (més informació del servei [aquí](https://suport-representa-ciutadania.aoc.cat/hc/ca)).

Per invocar les operacions de l'API com a representant d'una tercera persona s'ha d'afegir la capçalera `x-enotum-representat` amb el tipus i identificador de la persona a representar separats per un guió. Per exemple una capçalera vàlida per representar una persona física seria:

`x-enotum-representat: NIF-12345678Z`

o per representar una persona jurídica:

`x-enotum-representat: CIF-R0599999J`

Els tipus vàlids per persona física són: `NIF`, `PASSAPORT`, `IDC` (identificador de persona física d'un país de la unió europea).

Els tipus vàlids per persona jurídica són: `CIF`, `VAT` (identificador de persona jurídica d'un país de la unió europea).

## Entorns

L'adreça base de les operacions per cadascún dels entorns és la següent:
 * **PRE**: https://api-pre.enotum.cat:8443
 * **PRO**: https://api.enotum.cat:8443

> :information_source: Remarcar l'ús del port 8443 que és el que permet realitzar l'autenticació mTLS.

## Com demanar l'alta al servei

Per demanar l'alta al servei s'ha d'omplir el següent formulari (Pendent de publicar) i obrir un tiquet a [Suport](https://suport-enotum-ciutadania.aoc.cat/hc/ca).

# 2. Operacions

L'API proporciona totes les funcions necessaries per operar amb notificacions de les persones físiques i jurídiques que reben notificacions.

Les operacions que proporciona l'API estan detallades en el fitxer en format OpenAPI v3.0 que podeu [descarregar](openapi.yml).

## 2.1 Cerca Notificacions

La cerca de notificacions proporciona el llistat de notificacions i comunicacions del ciutadà. Aquesta llista inclou les notificacions de **totes** les entitats emissores de notificacions adherides a eNOTUM.

> :information_source: Per defecte es cerca per notificacions i comunicacions dipositades durant els últims 30 dies.

### **GET** `/ciutadania/notificacions`

### Paràmetres de consulta

* **dataInicial**: Data inicial del filtre per data de dipòsit. En format: `YYYY-MM-DD`
* **dataFinal**: Data final del filtre per data de dipòsit. En format: `YYYY-MM-DD`
* **estat**: Filtre per estat. Valors possibles:
  * DIPOSITADA
  * ACCEPTADA
  * REBUTJADA
  * EXPIRADA
* **tipus**: Tipus d'enviament. Pot agafar els valors:
  * NOTIFICACIO
  * COMUNICACIO
* **organisme**: Filtre per codi d'organisme emissor
* **departament**: Filtre per codi de departament de l'organisme emissor.
* **etiqueta**: Filtre per etiqueta.
* **noLlegides**: Filtre per notificacions no llegides.
* **ordre**: Ascendent o descendent. Valors possibles:
  * ASC
  * DESC
* **pagina**: Pàgina del llistat a recuperar. Per defecte: 1

### Paràmetres de capçalera

* **x-enotum-representat**: Identificador de la persona representada de la que consultar les notificacions. A l'apartat [Representacions](#representacions) es detalla aquest camp.

### Resposta

La resposta té els seguents camps:

* **notificacions**: array de `Notificacions`. Es descriu amb detall [aquí](#Notificacio). Aquest llistat no inclou els documents de la notificació. 
* **pagina**: númeor de pàgina actual del llistat retornat.
* **totalPagines**: número total de pagines del llistat.
* **totalResultats**: número total de notificacions del llistat.

## 2.2 Consulta Notificació

Proporciona el detall d'una notificació o una comunicació. Inclosos els documents d'aquesta.

> :information_source: En el cas de les comunicacions no caldrà acceptar-les amb l'operació [Practicar Notificació](#23-practicar-notificació) sinó que simplement fent-ne la consulta ja passaran a estar acceptades i es podran recuperar els documents.

### **GET** `/ciutadania/notificacions/{id}`

### Paràmetres a l'adreça

* **id**: Identificador de la notificació a consultar.

### Paràmetres de capçalera

* **x-enotum-representat**: Identificador de la persona representada de la que consultar les notificacions. A l'apartat [Representacions](#representacions) es detalla aquest camp.

### Resposta

La resposta és del tipus `Notificacio`. Es descriu amb detall [aquí](#Notificacio). 


## 2.3 Practicar Notificació

Aquesta operació realitza la pràctica de la notificació a eNotum. Es realitza canviant l'estat del recurs Notificacio a ACCEPTADA o REBUTJADA.

> :information_source: Per les comunicacions no s'ha de realitzar aquesta operació ja que passen automàticament a ser acceptades quan es realitza l'[Operació Consultar](#22-consulta-notificació) sobre aquestes.

### **PATCH** `/ciutadania/notificacions/{id}`

### Paràmetres del cos de la petició

* **estat**: Pot prendre els valors:
  * ACCEPTADA
  * REBUTJDADA 

### Paràmetres de capçalera

* **x-enotum-representat**: Identificador de la persona representada de la que consultar les notificacions. A l'apartat [Representacions](#representacions) es detalla aquest camp.

### Resposta

La resposta no retorna cap missatge. Només es retornen els codis HTTP:

* **200**: Si l'operació ha anat correctament.
* **400**: Si els paràmetres d'entrada no són vàlids.

## 2.4 Obtenir document d'una notificació

L'operació de detall d'una notificació retorna el llistat de documents d'aquesta. En el model de [Document](#document) el camp ``uuid`` indica el valor a utilitzar per tal de recuperar el document.

### **GET** `/ciutadania/notificacions/{id}/documents/{uuid}/contingut`

### Paràmetres a l'adreça

* **id**: Identificador de la notificació a la que pertany el document.
* **uuid**: Identificador del document a descarregar.

### Paràmetres de capçalera

* **x-enotum-representat**: Identificador de la persona representada de la que consultar les notificacions. A l'apartat [Representacions](#representacions) es detalla aquest camp.

### Resposta

Es retorna un codi HTTP 307 (*Temporary Redirect*) amb l'adreça temporal per efectuar la descàrrega. En funció de la llibreria utilitzada per l'integrador aquesta redirecció serà automàtica o s'haurà de recuperar de la capçalera ``Location``.

## 2.5 Obtenir justificant PDF de la pràctica d'una notificació

Permet obtenir el justificant PDF de la pràctica d'una notificació.

### **GET** `/ciutadania/notificacions/{id}/evidencia/contingut`

### Paràmetres a l'adreça

* **id**: Identificador de la notificació de la que es vol recuperar el justificant.

### Paràmetres de capçalera

* **x-enotum-representat**: Identificador de la persona representada de la que consultar les notificacions. A l'apartat [Representacions](#representacions) es detalla aquest camp.

### Resposta

Es retorna un codi HTTP 307 (*Temporary Redirect*) amb l'adreça temporal per efectuar la descàrrega. En funció de la llibreria utilitzada per l'integrador aquesta redirecció serà automàtica o s'haurà de recuperar de la capçalera ``Location``.

## 2.6 Resum de notificacions

Aquesta operació retorna el recompte de notificacions pendents de practicar i total del ciutadà. Aquest recompte es retorna segregat per entitat emissora i agregat en un total.

Per defecte es mostren les dades de les notificacions dipositades durant els últims 30 dies.

### **GET** `/ciutadania/resum`

### Paràmetres de consulta

* **dataInicial**: Data inicial del filtre per data de dipòsit. En format: `YYYY-MM-DD`
* **dataFinal**: Data final del filtre per data de dipòsit. En format: `YYYY-MM-DD`

### Paràmetres de capçalera

* **x-enotum-representat**: Identificador de la persona representada de la que consultar les notificacions. A l'apartat [Representacions](#representacions) es detalla aquest camp.

### Resposta

La resposta té els seguents camps:

* **totalNotificacions**: Numero total de notificacions del ciutadà.
* **totalNotificacionsPendents**: Número total de notificacions pendents de practicar del ciutadà.
* **entitats**: Recompte segregat per entitats. Conté els següents camps:
  * **codiOrganisme**: Codi de l'organisme emissor de les notificacions.
  * **nomOrganisme**: Nom de l'organisme emissor de les notificacions.
  * **codiDepartament**: Codi del departament de l'organisme emissor de les notificacions.
  * **nomDepartament**: Nom del departament de l'organisme emissor de les notificacions.
  * **notificacions**: Número de notificacions del ciutadà emeses pel departament.
  * **notificacionsPendents**: Número de notificacions pendents de practicar del ciutadà emeses pel departament.

## 2.7 Obtenir dades de l'usuari autenticat

Aquesta operació retorna les dades de l'usuari autenticat. Entre les dades que es retorna hi ha el llistat de persones representades segons les credencials de l'usuari. 

> :information_source: Aquest llistat de representats inclou tant les representacions donades d'alta a **Representa** com la representació present al **certificat digital** si aquest és un certificat de representació.

### **GET** `/ciutadania/usuari`

No accepta paràmetres d'entrada. Només és necessari que l'usuari s'hagi autenticat.

### Resposta

* **nom**: Nom de l'usuari.
* **cognoms**: Cognoms de l'usuari.
* **tipusDocument**: Tipus de document de l'usuari. Pot tenir els valors:
  * NIF
  * PASSAPORT
  * IDC
  * CIF
  * VAT
* **document**: Número de document identificatiu.
* **nivellSeguretat**: Nivell de seguretat de les credencials presentades. L'usuari només podrà consultar la notifiació i els documents de les notificacions que tinguin un nivell de seguretat igual o inferior al de l'usuari. Pot tenir els valors:
  * ALT
  * SUBSTANCIAL
  * BAIX

* **representats**: Array amb el llistat de representats de l'usuari. Aquests son els únics identificadors que es podran passar a la capçalera **x-enotum-representat**. Conté els camps:
  * **nom**: Nom de l'usuari representat.
  * **tipusDocument**: Tipus del document identificador de l'usuari representat.
  * **document**: Numero del document identificatiu de l'usuari representat.

## 2.7 Informació del servei

Retorna informació de la versió de l'API desplegada.

### **GET** `/ciutadania/info`

### Resposta

* **nom**: Nom del component que serveix de l'API.
* **versio**: Versió del component que serveix l'API.

## Model de dades dels recursos

Hi ha una serie de recursos comuns entre les diferents operacions. A continuació se'n detalla la definició:

### Notificacio

* **id**: Identificador de la notificació
* **tipus**: Tipus de l'enviament. Pot tenir els valors:
  * NOTIFICACIO
  * COMUNICACIO
* **emissor**: Dades de l'enitat emissora de la notificació. Conté els camps:
  * **codiOrganisme**: Codi de l'organisme emissor de les notificacions.
  * **nomOrganisme**: Nom de l'organisme emissor de les notificacions.
  * **codiDepartament**: Codi del departament de l'organisme emissor de les notificacions.
  * **nomDepartament**: Nom del departament de l'organisme emissor de les notificacions.
* **dadesOfici**: Dades d'ofici de la notificació. Conté els camps:
  * **cos**: Cos de la notificació
  * **peu**: Peu de recurs de la notificació.
* **titol**: Títol de la notificació.
* **referencia**: Referència indicada per l'emissor de la notificació.
* **nivellAcces**: Nivell de seguretat de la notificació. Pot tenir els valors:
  * ALT
  * SUBSTANCIAL
  * BAIX
* **dadesRegistre**: Dades de registre de la notificació. Conté els camps:
  * **numero**: Número de registre
  * **data**: Data de registre en format ISO 8601.
* **dataPublicacio**: Data de publicació de la notificacio. En format ISO 8601.
* **dataExpiracioPrevista**: Data d'expiració prevista de la notificació en format ISO 8601. Només per les notificacions que encara no han estat practicades.
* **dataPracticada**: Data de la pràctica de la notificació. En format ISO 8601.
* **estat**: Estat de la notificació. Pot prendre els valors:
  * DIPOSITADA
  * ACCEPTADA
  * REBUTJADA
  * EXPIRADA
* **diesExpiracio**: Retorna els dies que la notificació romandrà dipositada abans d'expirar.
* **consultada**: Indica si la notificació ha estat consultada previament.
* **canal**: Indica si la notificació és complementaria o exclusivament digital. Valors possibles:
  * DIGITAL
  * PAPER
* **expedient**: Identificador de l'expedient associat a la notificació.
* **tramit**: Identificador del tramit associat a la notificació.
* **numeroCas**: Número de cas associat a la notificació.
* **documents**: Documents associats a la notificació. Conté els camps:
  * **resolucio**: Document de resolució.
  * **annexos**: Array dels documents annexos a la notificació. Fins a un màxim de 5.

### Document

* **nom**: Nom del document.
* **uuid**: Identificador únic del document.
* **mida**: Mida en bytes del document.

# 3. Respostes en cas d'error

El servei pot respondre amb els següents codis HTTP:

* **200**: L'operació s'ha processat correctament. De manera que no es tracta d'un error.
* **400**: Algun dels paràmetres d'entrada no és correcte.
* **500**; Error intern del servei.

El payload de la resposta tindrà el següents camps:

* **codi**: Codi d'error de negoci.
* **missatge**: Missatge d'error.
* **parametresInvalids**: Array amb els paràmetres invàlids (en cas de repostes HTTP 400). Conté els camps:
  * **nom**: Nom del paràmetre invàlid.
  * **motiu**: Motiu pel qual el paràmetre és invàlid.

