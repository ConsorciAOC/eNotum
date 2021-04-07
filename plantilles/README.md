# 1. Introducció

L'objectiu d'aquest document és el de donar un patró per a la generació de plantilles per l'enviament de missatges als destinataris
de notificacions i/o comunicacions. 

## 1.1. Requisits
Per poder desenvolupar les funcionalitats presentades en aquest document, cal tenir coneixements : 
*	eNOTUM
*	HTML

## 1.2. Antecedents
L'acte de posada en coneixement d'una notificació o comunicació a un destinatari concret es fa mitjançant l'enviament de un correu 
electrònic i, si s'ha informat el telèfon mòbil, d'un SMS. El format del correu és fix i és possible modificar el contingut de certes 
parts del correu, així com el missatge enviat per SMS. 

Es desenvolupa una nova funcionalitat que permet als integradors introduir el seu propi format de correu, identificable a nivell 
d'identificador propi o a nivell d’entitat, tipus de notificació/comunicació e idioma. També existeixen plantilles per defecte a 
nivell d'aplicació per tipus de notificació e idioma, aquestes plantilles estan revisades i mantingudes i disposen d'un breu resum de 
les dades més importants del enviament així com el resum de les dades necessaries per al seu accés, per això si no teniu unes
necessitats molt concretes **us recomenem sempre l'ús de les plantilles per defecte**.

## 1.3.	Requeriments
A través del frontal d'empleat públic accessible via [EACAT](https://www.eacat.cat/) es crea un nou manteniment de plantilla, on 
l'usuari pot introduir la seva plantilla, fer una previsualització i comprovar la seva correctesa. 

Per a tot enviament, es podrà informar la plantilla a emprar i l'idioma per a cada destinatari. En cas que aquests camps no s'informin, 
o no es trobi una plantilla de correu electrònic per a l'idioma especificat corresponent a l'entitat que envia la notificació, es farà 
la cerca de la plantilla informada per l'entitat com a plantilla per defecte (no es tindrà en compte l'idioma del destinatari). Si aquesta
plantilla no es trobés es cercarà la plantilla per defecte de l’aplicació. 

# 2.	Missatgeria
Opcionalment en cas de que l'ens tingui configurades diverses plantilles és podra fer la especificació de la plantilla de correu electrònic 
a emprar i del idioma de la mateixa que rebrà el destinatari a través de l'operació de [_processarTramesa_](../missatgeria/README.md#petició---peticioprocessartramesa), amb la incorporació dels camps
plantilla i idioma. 

El camp `<Idioma>` el podeu trobar en l'objecte [`<Notificacio>`](../missatgeria/README.md#notificació) aquest aplicaria a tots els destinataris de la mateixa, a nivell de [`<Destinatari>`](../missatgeria/README.md#destinataris) també existeix i permet sobreescriure el de la `<Notificacio>` en cas que per exemple una notificació tingui varis destinataris en un mateix idioma i un en un idioma diferent. Per defecte si no s'especifica idioma, la plataforma utilitza el català.

El camp `<Plantilla>` dins de les [`<DadesAvisos>`](../missatgeria/README.md#dadesavisos) permet especificar l'identificador específic de la plantilla que es vol utilitzar.
 
Aquests valors no s'incorporen als resultats de les cerques, consultes i reports dels enviaments.
 
# 3. Generació de plantilla HTML
La plantilla HTML per correu electrònic és de lliure definició dins de unes restriccions de sistema. Dins aquesta plantilla es podran incloure una sèrie de camps, que seran substituïts pels seus valors corresponents en el moment de enviar el correu electrònic al destinatari. 

## 3.1.	Aplicació de la plantilla
S'han de generar 4 plantilles per tipus i idioma. La plantilla d'avís de notificació i de recordatori per al destinatari, i les plantilles d'avís i recordatori per a les persones d'avís. 

Els missatges d'avis i recordatori de notificació via SMS també apliquen les plantilles, la diferència és que en aquest cas no es poden utilitzar elements HTML però si que es podrà utilitzar la nomenclatura necessària per a la traducció dels valors dels camps.

## 3.2.	Nomenclatura de camps

En les versions anteriors els camps que es volien utilitzar s’especificaven amb el prefix i el sufix `@@`, amb el nom en majúscules, com per exemple `@@ID_NOTIFICACIO@@`

Per temes de retrocompatibilitat el camp `@@EMAIL_BODY@@` seguirà funcionant d’aquesta forma, la resta s’ha modificat el comportament com s'explica a continuació.

En el cas del tag `@@EMAIL_BODY@@` es traduirà per el camp [`//*:Tramesa/DadesAvisos/Email/Missatge`](../missatgeria/README.md#dadesavisos) de la petició de [_processarTramesa_](../missatgeria/README.md#petició---peticioprocessartramesa) o en el cas que no vingui informat al missatge (el camp es opcional) s'omplirà amb el camp missatge configurat a nivell d’entitat.

### 3.2.1	Avaluació d’expressions a la nova plantilla

En aquesta nova versió s'ha volgut afegir funcionalitats a les plantilles per dotar-les de la possibilitat d’incorporar més lògica fent així les plantilles més expressives i donant més flexibilitat al integrador a l'hora de definir-les. El cas és que ara les expressions s'hauran d'escriure de la següent forma: `${ expressio }`

La nova forma a diferència de l'anterior no només permet simplement reemplaçar els tags pel valor concret de l'enviament sinó que ens permet afegir lògica avaluable en temps de generació de plantilles. Per exemple podem afegir condicions de la següent forma, utilitzant l'operador ternari per a fer una condició: `${ condicio ? “compleix la condició” : “ooohhhh no” }`

En el cas de l'exemple si la condició es compleix, a la plantilla quedarà el text: **compleix la condició** si la condició no és certa a la plantilla s’afegirà el text: **ooohhhh no**.

Dins d'aquests tags el codi que s'avalua potser *java* o *groovy*, amb el que dona flexibilitat a l'integrador per a fer coses com podria ser aplicar mascares a l’hora d'ensenyar informació sensible, com per exemple mostrar només els 3 últims dígits del NIF: 

`${ String nif = "12345678Z";"******"+nif.substring(nif.length()-3); }`

Aquesta expressió a la plantilla un cop avaluada quedaria com `******78Z`

Dins del context de les expressions s'afegeixen els següents objectes per accedir a les seves propietats amb dot notation:
*	notificacio
*	destinatari
*	entitat

## 3.3	Camps accessibles


| EXPRESSIÓ | DESCRIPCIÓ |
| --------- | ---------- |
|`${notificacio.id}`|	Identificador de la notificació.|
|`${notificacio.idNotificacioEmissor}`|	Identificador de la notificació creat per l'emissor de la mateixa.|
|`${notificacio.titol}`|	Títol de la notificació.|
|`${notificacio.referencia}`|	Referència de la notificació de l'emissor.|
|`${notificacio.numeroRegistre}`|	Número de registre de la notificació.|
|`${notificacio.dataCreacio}`|	Data de creació de la notificació.|
|`${notificacio.dataRegistre}`|	Data de registre de entrada de la notificació.|
|`${notificacio.dataPublicacio}`|	Data de publicació de la notificació (ja és accessible per el destinatari).|
|`${notificacio.dataExpiracio}`|	Data en la qual expira la notificació.|
|`${notificacio.diesExpiracio}`|	Número de dies de vida de la notificació.|
|`${notificacio.tipusAcces}`|	El tipus d'accés per als enviaments són BAIX, SUBS, ALT.|
|`${notificacio.tipus}`|	Tipus de notificació.0 = notificació.1 = comunicació.|
|`${notificacio.idioma}`|	Idioma de la notificació amb el que s'ha enviat al client. ca = català, es = castellà, oc = aranès, en = anglès.|
|`${notificacio.msgTipusAcces}`|	Missatge ja traduït en funció del idioma de la notificació per a definir l'accés a la notificació.|
|`${notificacio.msgTipusEnviament}`|	Missatge ja traduït en funció del idioma de la notificació per a definir el tipus d'aquesta.|
|`${notificacio.linkAccesBustia}`|	Enllaç per a accedir a la bústia de l'entitat a la que pertany la notificació.|
|`${notificacio.linkAccesNotificacio}`|	Enllaç per a accedir directament a la notificació.|
|`${destinatari.email}`|	Correu electrònic del destinatari al que s'ha enviat la notificació.|
|`${destinatari.mobil}`|	Telèfon mòbil del destinatari al que s'ha enviat la notificació. És opcional i per tant pot ser buit.|
|`${destinatari.nomDestinatari}`|	Nom del destinatari de la notificació.|
|`${destinatari.cognom1}`|	Primer cognom del destinatari de la notificació.|
|`${destinatari.cognom2}`|	Segon cognom del destinatari de la notificació.|
|`${destinatari.raoSocial}`|	Raó social de l'empresa a la que pertany el destinatari en cas de que sigui una persona jurídica.|
|`${destinatari.idDocumentPF}`|	Identificador del document de la persona física **anonimitzat**. Per exemple: per un NIF amb número 12345678A mostrarà ******78A.|
|`${destinatari.tipusDocumentPF}`|	Tipus del document de la persona física. 0 = NIF, 1 = Passaport.|
|`${destinatari.idDocumentPJ}`|	Identificador del document de la persona jurídica **anonimitzat**. Per exemple: per un NIF amb número Q0801175A mostrarà ******75A.|
|`${destinatari.tipusDocumentPJ}`|	Tipus del document de persona jurídica. 0 = CIF, 1 = VAT Number.|
|`${entitat.nomEntitat}`|	Nom de l'organisme i el nom del departament que han enviat la notificació separats per un slash.|
|`${entitat.departament}`|	Nom del departament que ha enviat la notificació.|
|`${entitat.organisme}`|	Nom de l’organisme que ha enviat la notificació.|
|`${entitat.idEns}`|	Identificador del ens que ha enviat la notificació.|

A banda d'aquesta objectes dins del context de les plantilles hi ha carregats aquests altres dos objectes un per aplicar si es desitja mascares a dades sensibles com poden ser el documents identificatius, telèfons o les adreces de correu. L'altre és per a donar format a les dates:
* utils
* dateFormat

| EXPRESSIÓ |	DESCRIPCIÓ |
| --------- | ---------- |
|`${utils.getNumberMask(<Numero>) }`|	Aplica la mascarà al ‘<numero>’. Es pot aplicar a nif, telèfons, passaports; retorna el número substituint tots els números per * deixant només visibles els 3 últims. Per exemple: 666777888 retorna ******888, Q0801175A retorna ******75A.|
|`${utils.getEmailMask(<email>)}`|	Aplica la mascara per al ‘<email>’. Deixa el primer caràcter, el caràcter ‘@’ i el domini. Del primer caràcter a la ‘@’ sempre substitueix per cinc ‘*’. Exemple: localpart@domain.cat retorna l*****@domain.cat |
|`${dateFormat.format(<date)}`|	Formata la data en el format dd/MM/yyyy HH:mm:ss |
                            
### 3.3.1	Altres

Els mètodes addicionals de l'objecte `utils` i el `dateFormat` s’han afegit només per a facilitar les tasques que creiem més comuns a l’hora de generar les plantilles, però com s’explica al principi del document la nova forma de tractar les plantilles ens permet afegir codi dins de les expressions i generar per exemple els nostres propis formats per a la data (o per la mascara com s'explica al punt 3.2), per exemple si volem generar un format diferent ho podríem fer de la següent forma:

```${ new java.text.SimpleDateFormat(\"dd.MMM.yyyy\").format(new Date())}```

D'aquesta forma el format de la data per el dia 03/03/2016 seria: **03.mar.2016**

### 3.3.2	Consideracions addicionals

:warning: Aquesta nova forma de generar plantilles dona més flexibilitat, però també pot provocar més errors degut a expressions que s'avaluïn de forma incorrecta, per tant es recomanable que davant de modificacions susceptibles d'aportar molts canvis a les plantilles **es provin primer aquestes a l'entorn de PRE abans de fer el canvi a l’entorn de PRO.**

## 3.4

A tall d'exemple, a continuació podeu trobar els 4 tipus de plantilles customitzables per idioma i tipus que actualment s'utilitzen com a plantilles per defecte a l'aplicació:

* Català
  * [Comunicació](ca/comunicacio)
  * [Notificació](ca/notificacio)
* Aranés occità 
  * [Comunicació](oc/comunicacio)
  * [Notificació](oc/notificacio)
* Castellà
  * [Comunicació](es/comunicacio)
  * [Notificació](es/notificacio)
* Anglès
  * [Comunicació](en/comunicacio)
  * [Notificació](en/notificacio)
 
## 3.5	Restriccions de sistema

Les plantilles han de complir els següents requisits:

*	Grandària màxima  : 64Kb
*	Sense enllaços externs a javascript o css. Tot element necessari haurà de ser autocontingut a la plantilla.
*	Tota codificació serà estàndard HTML (http://www.w3.org/MarkUp/html-spec/html-spec_13.html) i el missatge estarà codificat en UTF-8, d'altre manera hi poden haver errors en el renderitzat d'accents i d'altres caràcters especials.
*	Addicionalment, es recomana passar el comprovant de pàgina de [*W3C*](http://validator.w3.org/).
*	Aquesta nomenclatura afecta a les plantilles de correu electrònic i de missatges SMS, i pot ser utilitzada també dins de la petició de procesarTramesa en els següents camps:
     * `//*:Tramesa/DadesAvisos/Email/Missatge`
     * `//*:Tramesa/DadesAvisos/SMS/Missatge`
*	Els enllaços d´accés a la bústia i notificació, s'ha de seguir utilitzant els antics tags amb la notació `@@TAG@@` per a fer ús del identificador de l'enviament o del codi del ens per exemple, tant els definits a la configuració de l'entitat com els que es poden enviar a través de la missatgeria als camps:
     * `//*:Tramesa/DadesAvisos/URLs/AccesNotificacio`
     * `//*:Tramesa/DadesAvisos/URLs/AccesLlistat` 
