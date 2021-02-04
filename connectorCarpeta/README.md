# 1. Introducció

A continuació és descriu el funcionament i les diferents modalitats de consum del connector de carpeta ciutadana del projecte **eNotum**.

Aquesta modalitat és independent del producte, i no implica la integració previa amb el mateix ja que funcionen de forma independent.

## 1.1. Integració PCI

Aquest servei s'integra dins de l'arquitectura de la Plataforma de Col·laboració Interadministrativa (en endavant _PCI_) a mode d'un nou servei accessible a través de la MTI. 

Per tant els integradors que vulguin accedir al connector de **carpeta ciutadana d'eNotum** ho hauran de fer a través de la missatgeria de la _PCI_ utilitzant l'element `<DatosEspecificos>` d'aquesta, per a més informació podeu consultar [el document d'integració de la _PCI_ aqui](https://www.aoc.cat/knowledge-base/plataforma-de-col-laboracio-administrativa-2/idservei/enotum/)

# 2. Missatgeria

Com es comenta en [el punt 1 d'aquest document](#11-integració-pci) el connector de **carpeta ciutadana d'eNotum** funciona com a servei dins de la _PCI_, serà per tant necessari treballar amb la missatgeria de la _PCI_, encapsulant la missatgeria específica del connector a dins d'aquesta.

Específicament per a fer ús del servei dins de la missatgeria de la _PCI_ és necessari informar els següents elements del missatge _XML_:

* `//Peticion/Atributos/CodigoProducto` el _string_ ENOTUM_CARPETA
* `//Peticion/Atributos/CodigoCertificado` el _string_ ENOTUM
* `//Peticion/Solicitudes/SolicitudTransmision/DatosGenericos/Transmision/CodigoCertificado` el _string_ ENOTUM
* `//Peticion/Solicitudes/SolicitudTransmision/DatosEspecíficos` Petició _XML_ específica el connector de **carpeta ciutadana d'eNotum**

Pel que fa a la resta del missatge _PCI_, cal que aquest compleixi amb els requisits definits [al document d'integració de la PCI aqui](https://www.aoc.cat/knowledge-base/plataforma-de-col-laboracio-administrativa-2/idservei/enotum/)

# 3. Missatgeria específica connector carpeta ciutadana d'eNotum

El connector disposa de dues modalitats de consum que es detallen a continuació:

## PeticioNotificacionsDetallades

A continuació podeu veure l'esquema de la petició de notificacions detallades:

```xml
<element name="PeticioNotificacionsDetallades">
  <complexType>
    <sequence>
      <element name="documentIdentitatDestinatari" type="pciage:DocumentIdentitatDestinatariType"/>
      <element name="cognomDestinatari" type="pciage:Cognom" minOccurs="0" />
      <element name="idp" type="pciage:idpType"/>
      <element name="nivellSeguretatAccess" type="pciage:Qaa"/>
      <element name="dataDisposicioInici" type="dateTime" minOccurs="0" />
      <element name="dataDisposicioFi" type="dateTime" minOccurs="0" />
      <element name="tipusEnviament" type="pciage:TipusEnviament" minOccurs="0" />
      <element name="estat" type="pciage:Estat" minOccurs="0" />
      <element name="ambit" type="pciage:Ambit" minOccurs="0" />
      <element name="codiOrganisme" type="pciage:Organisme" minOccurs="0" />
    </sequence>
  </complexType>
</element>
```

Aquesta és la petició que recupera el detall de les notificacions i està format pels següents elements:

* `documentIdentitatDestinatari` : Permet espeficiar el NIF i/o CIF del destinatari de les notificacions a cercar.
* `cognomDestinatari`: Permet indicar el cognom del destinatari. *Opcional*
* `idp`: Especifica dins de l'enumeració `idpType` del mateix esquema un dels següents valors que es correspon al nivell d'accés utilitzat per l'usuari.
  * *certificat*
  * *idcat-mobil*
  * *valid*
  * *clave-pin24*
  * *clave-segsoc*
* `nivellSeguretatAccess`: Especifica el nivell d'accés requerit de les notificacions que es volen cercar amb els valors del 1 al 4 definits per de la següent forma:
  * *1*: Baix
  * *2*: Baix amb registre
  * *3*: Substancial
  * *4*: Alt
* `dataDisposicioInici`: Data mínima de diposit de les notificacions a cercar. Només és tornaran les notificacions amb data de dipósit superior a l'especificada. *Opcional*
* `dataDisposicioFi`: Data màxima de diposit de les notificacions a cercar. Només és tornaran les notificacions amb data de dipósit inferior a l'especificada. *Opcional*
* `tipusEnviament`: Enumeració que permet especificar el tipus d'enviaments a cercar. *Opcional*
  * *NOTIFICACIO* : Enviaments de tipus notificació
  * *COMUNICACIO* : Enviaments de tipus comunicació
* `estat`: Enumeració que permet especificar l'estat en el que és troba la notificació a cercar. *Opcional*
  * *EN TERMINI* : L'enviament és troba dins del termini i encara es pot practicar per part de l'usuari.
  * *FINAL* : L'enviament ha finalitzat el seu cicle de vida i es troba en un estat final.
* `ambit`: Enumeració per especificar l'ambit de l'enviament. *Opcional*
  * *GENERALITAT* : Notificacions enviades per alguns dels organismes vinculats a la generalitat, que es troben a **eNotum**
  * *ENS LOCALS* : Notificacions enviades per a la resta d'ens dins d'**eNotum** que no són generalitat.
  * *ESTATAL* : Cerca a les notificacions que no és troben a **eNotum** contra el connector que ofereix l'AGE **(:warning: de moment no disponible)**
* `organisme`: Permet filtar per el INE10 de l'organisme pel qual es volen recuperar les notificacions. *Opcional*

[Aquí podeu veure la definició completa del esquema _peticioNotificacionsDetallades.xsd_](https://github.com/ConsorciAOC/eNotum/blob/master/connectorCarpeta/xsds/peticioNotificacionsDetallades.xsd)

## RespostaNotificacionsDetallades

A continuació podeu veure l'esquema de la resposta de notificacions detallades:

```xml
<element name="RespostaNotificacionsDetallades">
  <complexType>
    <choice>
      <element name="Errors" type="pciage:ErrorsType" /> 
      <sequence>
        <element name="enviaments" type="pciage:Enviaments" />
        <element name="enviamentsEspecials" type="pciage:EnviamentsEspecials" />
        <element name="mesResultats" type="boolean" />
        <element name="missatges" type="pciage:Missatges" minOccurs="0" />
      </sequence>
    </choice>
  </complexType>
</element>
```
La RespostaNotificacionsDetallades és l'estructura que retorna l'informació dels enviaments o un error en cas que hi hagi algun problema amb el tractament de la petició, està formada per els elements que es descriuen a continuació:

[Aquí podeu veure la definició completa del esquema _respostaNotificacionsDetallades.xsd_](https://github.com/ConsorciAOC/eNotum/blob/master/connectorCarpeta/xsds/respostaNotificacionsDetallades.xsd)

### Errors

En cas que hi hagi hagut un problema en el tractament de la petició, és retornara només l'element error sense cap detall sobre els enviaments, l'element té la següent forma:

```xml
<complexType name="ErrorsType">
  <sequence>
    <element name="Error">
      <complexType>
        <all>
          <element name="Codi" type="integer"/>
          <element name="Descripcio" type="string"/>
        </all>
      </complexType>
    </element>
  </sequence>
</complexType>
```

* `\\Error\Codi`: Valor númeric amb el codi de l'error (TODO: Veure apartat amb els codis d'error)
* `\\Error\Descripcio`: Descripció de la causa de l'error.

### Resposta correcte

En cas que la petició s'hagi processat correctament ens trobarem amb la següent `<xsd:sequence>`:

```xml
<sequence>
  <element name="enviaments" type="pciage:Enviaments" />
  <element name="enviamentsEspecials" type="pciage:EnviamentsEspecials" />
  <element name="mesResultats" type="boolean" />
  <element name="missatges" type="pciage:Missatges" minOccurs="0" />
</sequence>
```
* `\\mesResultats`: Valor boolea, que indica que en el cas que els filtres especificats a la cerca retornin més de 100 enviaments, només es retorna la informació dels 100 primers i s'indica mitjançant el valor `true` que hi ha més resultats.
* `\\missatges`: Missatges retornats per el connector d'AGE. Només pot aplicar en peticions on s'hagi informat `<ambit>ESTATAL</ambit>`.

La resta d'elements es descriuren a continuació:

#### Enviaments

```xml
<complexType name="Enviament">
  <sequence>
    <element name="codiOrganisme" type="pciage:Organisme" />
    <element name="descripcioOrganisme" type="pciage:Descripcio" />
    <element name="identificador" type="pciage:Identificador" />
    <element name="concepte" type="pciage:Concepte" />
    <element name="estat" type="pciage:Estat" minOccurs="0" />
    <element name="dataDisposicioInici" type="dateTime" />
    <element name="dataDisposicioFi" type="dateTime" />
    <element name="tipusEnviament" type="pciage:TipusEnviament" />
    <element name="canalEnviament" type="pciage:Canal" minOccurs="0" />
    <element name="linkAcces" type="anyURI" />
  </sequence>
</complexType>
```

* `\Enviament\codiOrganisme`: Contindrà dos subelements amb el codi ine10 i dir3 de l'emissor de l'enviament.
* `\Enviament\descripcioOrganisme`: Camp de text amb la descripció o nom de l'organisme
* `\Enviament\identificador`: Identificador de l'enviament.
* `\Enviament\concepte`: Titol o concepte descriptiu de l'enviament
* `\Enviament\estat`: Estat en el que és troba l'enviament, és una enumeració entre els següents valors:
  * *EN TERMINI* : L'enviament es troba dins del termini.
  * *FINAL* : L'enviament ha finalitzat.
* `\Enviament\dataDisposicioInici`: Data en que l'enviament es va posar a diposició de l'usuari.
* `\Enviament\dataDisposicioFi`: Data en que l'enviament finalitza el termini.
* `\Enviament\tipusEnviament`: Enumeració del tipus d'enviament retornat.
  * *NOTIFICACIO* : Enviaments de tipus notificació
  * *COMUNICACIO* : Enviaments de tipus comunicació
* `\Enviament\canalEnviament`: Mitja per el qual es va realitzar l'enviament, enumeració d'un dels següents valors:
  * *ELECTRONIC* : Enviament es va realitzar a través de mitjants electronics (email o telefon)
  * *POSTAL* : Enviament es va realitzar via correu postal.
  * *SEUE* : Enviament es va posar a disposició a través de la seu electrònica de l'organisme emissor.
* `\Enviament\linkAcces` : Enllaç web que permet al destinatari accedir al contingut de l'enviament.
  
#### EnviamentsEspecials

Aquests elements no existeixen a **eNotum** i només es tornaran en respostes a peticions no hi hagi informat `<ambit>ESTATAL</ambit>`.

```xml
<complexType name="EnviamentEspecial">
  <sequence>
    <element name="codiOrganismo" type="pciage:Organisme" />
    <element name="descripcioOrganismo" type="pciage:Descripcio" />
    <element name="estat" type="pciage:Estat" minOccurs="0" />
    <element name="tipusEnviament" type="pciage:TipusEnviament" />
    <element name="existeixEnviament" type="boolean" />
    <element name="linkAcces" type="anyURI" />
  </sequence>
</complexType>
```

* `\EnviamentsEspecial\existeixEnviament`: Camp propi de la missatgeria del AGE.

Els camps del element `enviamentsEspecials` tenen el mateix significat que els camps de l'element `enviament`.

## PeticioNotificacionsAgrupades

A continuació podeu veure l'esquema de la petició de notificacions detallades:

```xml
<element name="PeticioNotificacionsAgrupades">
  <complexType>
    <sequence>
      <element name="documentIdentitatDestinatari" type="pciage:DocumentIdentitatDestinatariType"/>
      <element name="cognomDestinatari" type="pciage:Cognom" minOccurs="0" />
      <element name="idp" type="pciage:idpType"/>
      <element name="nivellSeguretatAccess" type="pciage:Qaa"/>
      <element name="dataDisposicioInici" type="dateTime" minOccurs="0" />
      <element name="dataDisposicioFi" type="dateTime" minOccurs="0" />
      <element name="tipusEnviament" type="pciage:TipusEnviament" minOccurs="0" />
      <element name="estat" type="pciage:Estat" minOccurs="0" />
      <element name="ambit" type="pciage:Ambit" minOccurs="0" />
      <element name="codiOrganisme" type="pciage:Organisme" minOccurs="0" />
    </sequence>
  </complexType>
</element>
```

Aquesta és la petició que recupera el detall de les notificacions i està format pels següents elements:

* `documentIdentitatDestinatari` : Permet espeficiar el NIF i/o CIF del destinatari de les notificacions a cercar.
* `cognomDestinatari`: Permet indicar el cognom del destinatari. *Opcional*
* `idp`: Especifica dins de l'enumeració `idpType` del mateix esquema un dels següents valors que es correspon al nivell d'accés utilitzat per l'usuari.
  * *certificat*
  * *idcat-mobil*
  * *valid*
  * *clave-pin24*
  * *clave-segsoc*
* `nivellSeguretatAccess`: Especifica el nivell d'accés requerit de les notificacions que es volen cercar amb els valors del 1 al 4 definits per de la següent forma:
  * *1*: Baix
  * *2*: Baix amb registre
  * *3*: Substancial
  * *4*: Alt
* `dataDisposicioInici`: Data mínima de diposit de les notificacions a cercar. Només és tornaran les notificacions amb data de dipósit superior a l'especificada. *Opcional*
* `dataDisposicioFi`: Data màxima de diposit de les notificacions a cercar. Només és tornaran les notificacions amb data de dipósit inferior a l'especificada. *Opcional*
* `tipusEnviament`: Enumeració que permet especificar el tipus d'enviaments a cercar. *Opcional*
  * *NOTIFICACIO* : Enviaments de tipus notificació
  * *COMUNICACIO* : Enviaments de tipus comunicació
* `estat`: Enumeració que permet especificar l'estat en el que és troba la notificació a cercar. *Opcional*
  * *EN TERMINI* : L'enviament és troba dins del termini i encara es pot practicar per part de l'usuari.
  * *FINAL* : L'enviament ha finalitzat el seu cicle de vida i es troba en un estat final.
* `ambit`: Enumeració per especificar l'ambit de l'enviament. *Opcional*
  * *GENERALITAT* : Notificacions enviades per alguns dels organismes vinculats a la generalitat, que es troben a **eNotum**
  * *ENS LOCALS* : Notificacions enviades per a la resta d'ens dins d'**eNotum** que no són generalitat.
  * *ESTATAL* : Cerca a les notificacions que no és troben a **eNotum** contra el connector que ofereix l'AGE **(:warning: de moment no disponible)**
* `organisme`: Permet filtar per el INE10 (el camp té una longitud màxima de 9 caràcters) del organisme per el qual es volen recuperar les notificacions. *Opcional*

[Aquí podeu veure la definició completa del esquema _peticioNotificacionsAgrupades.xsd_](https://github.com/ConsorciAOC/eNotum/blob/master/connectorCarpeta/xsds/peticioNotificacionsAgrupades.xsd)

## RespostaNotificacionsAgrupades

A continuació podeu veure l'esquema de la resposta de notificacions agrupades:

```xml
<element name="RespostaNotificacionsAgrupades">
  <complexType>
    <choice>
      <element name="Errors" type="pciage:ErrorsType" />
      <sequence>
        <element name="organismesEmisors" type="pciage:OrganismesEmisors" />
        <element name="estats" type="pciage:Estats" />
        <element name="missatges" type="pciage:Missatges" minOccurs="0" />
      </sequence>
    </choice>
  </complexType>
</element>
```
La RespostaNotificacionsAgrupades és l'estructura que retorna l'informació dels enviaments o un error en cas que hi hagi algun problema amb el tractament de la petició, està formada per els elements que es descriuen a continuació:

[Aquí podeu veure la definició completa del esquema _respostaNotificacionsAgrupades.xsd_](https://github.com/ConsorciAOC/eNotum/blob/master/connectorCarpeta/xsds/respostaNotificacionsAgrupades.xsd)

### Errors

En cas que hi hagi hagut un problema en el tractament de la petició, és retornara només l'element error sense cap detall sobre els enviaments, l'element té la següent forma:

```xml
<complexType name="ErrorsType">
  <sequence>
    <element name="Error">
      <complexType>
        <all>
          <element name="Codi" type="integer"/>
          <element name="Descripcio" type="string"/>
        </all>
      </complexType>
    </element>
  </sequence>
</complexType>
```

* `\\Error\Codi`: Valor númeric amb el codi de l'error (TODO: Veure apartat amb els codis d'error)
* `\\Error\Descripcio`: Descripció de la causa de l'error.

### Resposta correcte

En cas que la petició s'hagi processat correctament ens trobarem amb la següent `<xsd:sequence>`:

```xml
<sequence>
  <element maxOccurs="1" minOccurs="1" name="organismesEmisors" type="pciage:OrganismesEmisors"/>
  <element maxOccurs="1" minOccurs="1" name="estats" type="pciage:Estats"/>
  <element maxOccurs="1" minOccurs="0" name="missatges" type="pciage:Missatges"/>
</sequence>
```
* `\\organismesEmissors`: Llista en la que es mostren agrupades per organisme emissor les notificacions i comunicacions que té el destinatari.
* `\\estats`: Llista en la que es mostren per estats les notificacions i comunicacions que té el destinatari.
* `\\missatges`: Missatges retornats per el connector d'AGE. Només pot aplicar en peticions on s'hagi informat `<ambit>ESTATAL</ambit>`.

##### OrganismesEmisors
```xml
<complexType name="OrganismeEmisor">
  <sequence>
    <element name="codiOrganisme" type="pciage:Organisme" />
    <element name="descripcioOrganisme" type="pciage:Descripcio" />
    <element name="notificacionsPendents" type="pciage:Enviament" minOccurs="0" />
    <element name="notificacionsFinalitzades" type="pciage:Enviament" minOccurs="0" />
    <element name="comunicacions" type="pciage:Enviament" minOccurs="0" />
  </sequence>
</complexType>
```
* `\\codiOrganisme`: Contindrà el codi INE10 de l'organisme emissor.
* `\\descripcioOrganisme`: Camp de text amb la descripció o nom de l'organisme.
* `\\notificacionsPendents`: Contindrà els enviaments de les notificacions en estat pendent. No s'informa si no n'hi ha.
* `\\notificacionsFinalitzades`: Contindrà els enviaments de les notificacions que ja han estat finalitzades. No s'informa si no n'hi ha.
* `\\comunicacions`: Contindrà els enviaments de les comunicacions. No s'informa si no n'hi ha.

##### Estats
```xml
<complexType name="Estats">
  <sequence>
    <element name="notificacionsPendents" type="pciage:Enviament" minOccurs="0" />
    <element name="notificacionsFinalitzades" type="pciage:Enviament" minOccurs="0" />
    <element name="comunicacions" type="pciage:Enviament" minOccurs="0" />
  </sequence>
</complexType> 
```
* `\\notificacionsPendents`: Contindrà el total dels enviaments de les notificacions en estat pendent. No s'informa si no hi ha enviaments.
* `\\notificacionsFinalitzades`: Contindrà el total dels enviaments de les notificacions que ja han estat finalitzades. No s'informa si no hi ha enviaments.
* `\\comunicacions`: Contindrà el total dels enviaments de les comunicacions. No s'informa si no hi ha enviaments.

##### Enviament
```xml
<complexType name="Enviament">
  <sequence>
    <element name="existeixenEnviament" type="boolean" />
    <element name="numeroEnviaments" type="integer" minOccurs="0" />
    <element name="mesResultats" type="boolean" />
    <element name="linkAcces" type="anyURI" />
  </sequence>
</complexType>
```
* `\\existeixenEnviament`: Booleà indicant si hi ha enviaments per l'organisme o l'estat.
* `\\numeroEnviaments`: Indica el número d'enviaments. Només s'informa si el camp `existeixenEnviament` és cert.
* `\\mesResultats`: Indica si hi ha més resultats a banda dels que indica el número d'enviaments.
* `\\linkAcces`: Enllaç web que permet al destinatari accedir al contingut de l'enviament.

# 4. Codis i missatges d'error


| CODI   | DESCRIPCIO                        | 
| ------ | --------------------------------- | 
| 101    | El xml no compleix l'esquema      |
| 102    | Error executant l'operació        |
| 103    | Error en la cerca                 |
| 104    | Error en la petició a l'AGE       |
