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

El connector només disposa d'una modalitat de consum que és detalla a continuació:

## PeticioNotificacionsDetallades

A continuació podeu veure l'esquema de la petició de notificacions detallades:

```xml
<element name="PeticioNotificacionsDetallades">
  <complexType>
    <sequence>
      <element name="documentIdentitatDestinatari" type="pciage:DocumentIdentitatDestinatariType"/>
      <element maxOccurs="1" minOccurs="0" name="cognomDestinatari" type="pciage:Cognom"/>
      <element name="idp" type="pciage:idpType"/>
      <element name="nivellSeguretatAccess" type="pciage:Qaa"/>
      <element maxOccurs="1" minOccurs="0" name="dataDisposicioInici" type="dateTime"/>
      <element maxOccurs="1" minOccurs="0" name="dataDisposicioFi" type="dateTime"/>
      <element name="tipusEnviament" type="pciage:TipusEnviament"/>
      <element maxOccurs="1" minOccurs="0" name="estat" type="pciage:Estat"/>
      <element maxOccurs="1" minOccurs="0" name="ambit" type="pciage:Ambit"/>	
      <element maxOccurs="1" minOccurs="0" name="codiOrganisme" type="pciage:Organisme"/>							
    </sequence>
  </complexType>
</element>
```

[Aquí podeu veure la definició completa del esquema _Peticio.xsd_](https://github.com/ConsorciAOC/eNotum/blob/master/connectorCarpeta/xsds/peticioNotificacionsDetallades.xsd)

Aquesta és la petició que recupera el detall de les notificacions i està format pels següents elements:

* `documentIdentitatDestinatari` : Es tracta d'un `xsd:choice` que ens permet espeficiar el NIF o el CIF del destinatari de les notificacions a cercar.
* `cognomDestinatari`: Camp *opcional* per indicar el cognom del destinatari
* `idp`: Especifica dins de l'enumeració `idpType` del mateix esquema un dels següents valors que es correspon al nivell d'accés requerit per a les notificacions a cercar:
** *certificat*
** *idcat-mobil*
** *valid*
** *clave-pin24*
** *clave-segsoc*



