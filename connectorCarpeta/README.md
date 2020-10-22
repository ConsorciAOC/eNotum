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

