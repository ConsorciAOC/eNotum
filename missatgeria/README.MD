
<h1> ESBORRANY </h1>

# 1. Introducció

A continuació és descriu el funcionament i les diferents modalitats de consum de la versió de missatgeria _3.2_ d'**eNotum**.

Les versions antigues de missatgeria segueixen sent compatibles i funcionen correctament, tot i que disposen de menys funcionalitats i són susceptibles de ser discontinuades en un futur, si comenceu ara la integració haurieu de fer-ho amb aquesta versió. 

De totes formes podeu consultar les versions antigues de missatgeria i altres documents d'integració a través del [nostre portal de suport a la integració](https://www.aoc.cat/portal-suport/e-notum-base-coneixement/idservei/enotum/#integracio).

## 1.1. Integració PCI

Tal i com es reflecteix a la següent figura, el Backend d'**eNotum** s'integra dins de l'arquitectura de la Plataforma de Col·laboració Interadministrativa (en endavant _PCI_) a mode d'un nou servei accessible a través de la MTI. 

Per tant els integradors que vulguin accedir a l'**eNotum** ho hauran de fer a través de la missatgeria de la _PCI_ utilitzant l'element `<DatosEspecificos>` d'aquesta, per a més informació podeu consultar [el document d'integració de la _PCI_ aqui](https://www.aoc.cat/knowledge-base/plataforma-de-col-laboracio-administrativa-2/idservei/enotum/)

![eNotum_integracio_pci](https://github.com/ConsorciAOC/eNotum/blob/master/missatgeria/imgs/eNotum_arquitectura_pci.png)

## 1.2. Cicle de vida de les notificacions

### 1.2.1. Diagrama de cicle de vida

### 1.2.2. Descripció dels estats de les notificacions

#### 1.2.2.1. Pendent de processar

#### 1.2.2.2. Registrada

### TODO Resta estats...

## 1.3. Cicle de vida de les comunicacions

## 1.4. Codificació dels estats eNOTUM

# 2. Missatgeria

Com es comenta en [el punt 1 d'aquest document](https://github.com/ConsorciAOC/eNotum/blob/master/missatgeria/README.MD#11-integració-pci) **eNotum** funciona com a servei dins de la _PCI_, serà per tant necessari treballar amb la missatgeria de la _PCI_, encapsulant la missatgeria específica de **eNotum** dins d’aquesta.

Específicament per a fer ús del servei d'**eNotum** dins de la missatgeria de la _PCI_ és necessari informar els següents elements del missatge _XML_:

* `//Peticion/Atributos/CodigoProducto` el _string_ ENOTUM
* `//Peticion/Atributos/CodigoCertificado` el _string_ ENOTUM
* `//Peticion/Solicitudes/SolicitudTransmision/DatosGenericos/Transmision/CodigoCertificado` el _string_ ENOTUM
* `//Peticion/Solicitudes/SolicitudTransmision/DatosEspecíficos` Petició _XML_ específica d'**eNotum**

Pel que fa a la resta del missatge _PCI_, cal que aquest compleixi amb els requisits definits [al document d'integració de la PCI aqui](https://www.aoc.cat/knowledge-base/plataforma-de-col-laboracio-administrativa-2/idservei/enotum/)

# 3. Missatgeria específica eNotum versió 3.2

A continuació es detallaran els elements comuns a totes les modalitats de consum:

### 3.1. Petició

Totes les modalitats de consum _WS_ que ofereix **eNotum** extenen el següent esquema de petició:

```xml
<xs:complexType name="PeticioType" abstract="true">
	<xs:sequence>
		<xs:element name="Usuari" type="UsuariType"/>
		<xs:element name="Emissor" type="EmissorType"/>
	</xs:sequence>
</xs:complexType>
```

Per tant com és pot veure totes les peticions, a banda dels elements específics de cada modalitat de consum, tenen els següents elements:

* `//PeticioType/Usuari` Dades de l'usuari, es descriuen a continuació.
* `//PeticioType/Emissor` Dades de l'emissor. es descriuen a continuació.

[Aquí podeu veure la definició completa del esquema _Peticio.xsd](https://github.com/ConsorciAOC/eNotum/blob/master/missatgeria/v3.2_xsds/Peticio.xsd)

### 3.2. Usuari

La definició de l'element _UsuariType_, [la podeu trobar a_Usuari.xsd](https://github.com/ConsorciAOC/eNotum/blob/master/missatgeria/v3.2_xsds/Usuari.xsd)

Aquest tipus està format pels següents elements:

CHOICE un dels 3.
* `/Usuari/CertificatCiutada`
Certificat del ciutadà per les operacions amb rol CIUTADA.
* `/Usuari/IdParaulaPas`
Identificador de la paraula de pas
* `/Usuari/ParaulaPas`
Paraula de pas
	<xs:annotation>
						<xs:documentation>Dades per l'autenticació amb Valid. Identificador de l'autenticació del client al que el deleguem (no cal q sigui el token de valid) sobre aquest id no es fa cap comprovació.</xs:documentation>
					</xs:annotation>
					<xs:choice minOccurs="0">
						<xs:element name="NIF" type="NIF"/>
						<xs:element name="PASSAPORT" type="NoEmptyString"/>
					</xs:choice>
					<xs:element name="CIF" type="CIF" minOccurs="0"/>
					<xs:element name="IdAutenticacio" type="xs:string"/>
          
          
          
OPCIONALMENT * `/Usuari/Perfil`
Perfil de l’usuari que realitza la consulta. Existiran 3 possibles valors per aquest perfil, que incidiran en la forma de la cerca de notificacions.
* `/Usuari/Rol`
Rol autoritzat de l’usuari:
 EMPLEAT
 CIUTADA
* 

A continuació és detallaran els elements específics per a cadascúna de les modalitats de consum:

ProcessarTramesa
Consula
Cerca 

....





