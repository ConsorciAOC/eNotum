
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

Com es comenta en [el punt 1 d'aquest document](https://github.com/ConsorciAOC/eNotum/blob/master/missatgeria/README.MD#11-integració-pci) **eNotum** funciona com a servei dins de la _PCI_, serà per tant necessari treballar amb la missatgeria de la _PCI_, encapsulant la missatgeria específica de **eNotum** dins d'aquesta.

Específicament per a fer ús del servei d'**eNotum** dins de la missatgeria de la _PCI_ és necessari informar els següents elements del missatge _XML_:

* `//Peticion/Atributos/CodigoProducto` el _string_ ENOTUM
* `//Peticion/Atributos/CodigoCertificado` el _string_ ENOTUM
* `//Peticion/Solicitudes/SolicitudTransmision/DatosGenericos/Transmision/CodigoCertificado` el _string_ ENOTUM
* `//Peticion/Solicitudes/SolicitudTransmision/DatosEspecíficos` Petició _XML_ específica d'**eNotum**

Pel que fa a la resta del missatge _PCI_, cal que aquest compleixi amb els requisits definits [al document d'integració de la PCI aqui](https://www.aoc.cat/knowledge-base/plataforma-de-col-laboracio-administrativa-2/idservei/enotum/)

# 3. Missatgeria específica eNotum versió 3.2

A continuació es detallaran els elements comuns a totes les modalitats de consum:

## Petició

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

[Aquí podeu veure la definició completa del esquema _Peticio.xsd_](https://github.com/ConsorciAOC/eNotum/blob/master/missatgeria/v3.2_xsds/Peticio.xsd)

### Usuari

La definició de l'element _UsuariType_, [la podeu trobar a _Usuari.xsd_](https://github.com/ConsorciAOC/eNotum/blob/master/missatgeria/v3.2_xsds/Usuari.xsd)

El tipus usuari està format pels següents elements:

#### Rol
```xml
<xs:element name="Rol">
	<xs:simpleType>
		<xs:restriction base="xs:string">
			<xs:enumeration value="EMPLEAT"/>
			<xs:enumeration value="CIUTADA"/>
		</xs:restriction>
	</xs:simpleType>
</xs:element>
```
Aquest camp és obligatori i contindrà la etiqueta del rol autoritzat de l'usuari que realitza la operació. Hi ha dos rols definits que es descriuen a continuació:
* *EMPLEAT* : L'empleat públic és el rol d'usuari autoritzat per a la creació de notificacions. A banda permet fer les operacions de modalitats de consum de Cerca, Consulta etc. A excepció de les operacions de Practicar i demanar Paraula de pas.
* *CIUTADA* : El ciutadà és el rol d'usuari autoritzat a cercar i acceptar notificacions vinculades al propi usuari.

#### Perfil
```xml
<xs:element name="Perfil" minOccurs="0">
	<xs:simpleType>
		<xs:restriction base="xs:string">
			<xs:enumeration value="PERSONA_JURIDICA"/>
			<xs:enumeration value="EMPRESA"/>
			<xs:enumeration value="PERSONA_FISICA"/>
		</xs:restriction>
	</xs:simpleType>
</xs:element>
```
Opcionalment en cas de `<Rol>` _CIUTADA_ i de que l'usuari presenti el NIF de persona física i el d'empresa, ja sigui al generar la paraula de pas, en el certificat, o indicant-ho en els camps amb autenticació delegada, es podrà indicar un perfil que afectarà directament a les cerques de notificacions. Els possibles valors que pot pendre aquest camp:
* *PERSONA_FISICA* : Cerca les notificacions per a la persona física.
* *EMPRESA* : Cerca les notificacions per a l'empresa.
* *PERSONA_JURIDICA* : Cerca les notificacions com a persona jurídica.

### Autenticació usuari

En cas de `<Rol>` amb valor CIUTADA, s'haurà d'informar només un dels següents blocs en funció del mecanisme d'autenticació emprat per l'usuari.

#### Certificat
```xml
<xs:element name="CertificatCiutada" type="xs:base64Binary"/>
```
En cas d'autenticació amb certificat aquest camp contindrà el certificat del ciutadà que realitza l'operació.

#### Paraula de pas
```xml
<xs:sequence>
	<xs:element name="IdParaulaPas" type="xs:string"/>
	<xs:element name="ParaulaPas" type="xs:string"/>
</xs:sequence>
```
Identificador i paraula de pas respectivament, emprades com a mecanisme d'autenticació del ciutadà que realitza l'operació.

#### Mecanisme d'autenticació extern (Valid o altres)
```xml
<xs:sequence>
	<xs:choice minOccurs="0">
		<xs:element name="NIF" type="NIF"/>
		<xs:element name="PASSAPORT" type="NoEmptyString"/>
	</xs:choice>
	<xs:element name="CIF" type="CIF" minOccurs="0"/>
	<xs:element name="IdAutenticacio" type="xs:string"/>
</xs:sequence>
```
En aquest cas és l'aplicació integrada la que assumeix la responsabilitat d'autenticar el ciutadà utilizant p.e el servei de Valid o qualsevol altre servei extern. Els camps a informar són:

* `/Usuari/NIF` NIF de l'usuari.
* `/Usuari/PASSAPORT` Passaport de l'usuari.
* `/Usuari/CIF` CIF del usuari
* `/Usuari/IdAutenticacio` Identificador intern de l'aplicació integradad per al procés d'autenticació.

## Emissor

La definició de l'element _EmissorType_, [la podeu trobar a _Emissor.xsd_](https://github.com/ConsorciAOC/eNotum/blob/master/missatgeria/v3.2_xsds/Emissor.xsd)

Els elements que conté el tipus Emissor són els següents:

### CodiOrganisme
```xml
<xs:element name="CodiOrganisme" type="xs:string"/>
```
Codi ENS de l'organisme emissor de la notificació

### CodiDepartament
```xml
<xs:element name="CodiDepartament" type="xs:string"/>
```
Codi ENS del departament emissor de la notificació.

### CodiBO
```xml
<xs:element name="CodiBO" type="xs:string" minOccurs="0"/>
```
Codi del Back Office de l'emissor

## Modalitats de consum del _WS_

A continuació és detallaran els elements específics per a les diferents peticions de les modalitats de consum que ofereix el _WS_ d'**eNotum**.

## Petició ProcessarTramsea

[Aquí podeu veure la definició completa del esquema _ProcessarTramesa.xsd_](https://github.com/ConsorciAOC/eNotum/blob/master/missatgeria/v3.2_xsds/PeticioProcessarTramesa.xsd)

Aquesta petició permet la creació de notificacions dins del sistema. La definició d'aquesta petició exten la _peticioType_ afegint l'element `<Tramesa>` que es descriu a continuació.

## Tramesa
```xml
<xs:element name="Tramesa">
	<xs:complexType>
		<xs:sequence>
			<xs:element name="DadesAvisos" minOccurs="0"/>
			<xs:element name="DadesOfici"/>
			<xs:element name="Documents" type="DocumentsType"/>
			<xs:element name="Notificacio" type="NotificacioType" maxOccurs="unbounded"/>
		</xs:sequence>
	</xs:complexType>
</xs:element>

```
La tramesa és l'estructura que permet la creació de notificacions i/o comunicacions i permet agrupar-les. Està formada per els elements que es descriuen a continuació:

### DadesAvisos
```xml
<xs:element name="DadesAvisos" minOccurs="0">
	<xs:complexType>
		<xs:all>
			<xs:element name="Plantilla" type="xs:string" minOccurs="0"/>
			<xs:element name="Email" minOccurs="0"/>
			<xs:element name="SMS" minOccurs="0"/>
			<xs:element name="DiesAvisos" type="DiesAvisosType" minOccurs="0"/>
			<xs:element name="URLs" minOccurs="0"/>
		</xs:all>
	</xs:complexType>
</xs:element>
```	
L'element `<DadesAvisos>` és opcional ja que **eNotum** disposa d'un comportament per defecte per als valors especificats en aquest element, en cas de que vinguin informats al missatge, aquests tindran prioritat per a ser utilitzats en comptes dels que hi hagi establers per defecte. `<DadesAvisos>` està format pels següents elements:
	
##### Plantilla
```xml
<xs:element name="Plantilla" type="xs:string" minOccurs="0"/>
```
Identificador de plantilles de correu i SMS d'avis a utilitzar per a aquesta notificació. Es condició que a **eNotum** existeixi aquest identificador amb les plantilles creades.

#### Email
```xml
<xs:element name="Email" minOccurs="0">
	<xs:complexType>
		<xs:all>
			<xs:element name="Emissor" type="xs:string" minOccurs="0"/>
			<xs:element name="Assumpte" minOccurs="0">
				<xs:simpleType>
					<xs:restriction base="xs:string">
						<xs:maxLength value="100"/>
					</xs:restriction>
				</xs:simpleType>
			</xs:element>
			<xs:element name="Missatge" type="xs:string" minOccurs="0"/>
		</xs:all>
	</xs:complexType>
</xs:element>
```
Aquest element permet canviar la configuració per defecte que rebran els destinataris de la notificació via correu electrònic. Concretament permet indicar els següents camps:

* `/Email/Emissor`
Aquest camp conté l'adreça de correu electrònic que s'utilitzarà com a remitent pels avisos als destinataris de les notificacions.
**eNotum** concatenarà el prefix *NORESPONEU_* al valor indicat per indicar al destinatari que no s'espera cap tipus de resposta de la seva part.
* `/Email/Assumpte`
Assumpte que s'utilitzarà per a l'avís del correu electrònic.
* `/Email/Missatge`
Cos del missatge del avis per correu electrònic. El missatge ha de ser en format de text pla. Aquest missatge s'embolcallarà dins de les plantilles definides al sistema, concretament a les plantilles s'espeficarà l'element `@@EMAIL_BODY@@` ([per més informació podeu veure documentació plantilles](https://www.aoc.cat/knowledge-base/document-dintegracio-e-notum-generacio-de-plantilles-de-correu-electronic/idservei/enotum/)) que serà substituit per aquest missatge en cas d'existir.

#### SMS
```xml
<xs:element name="SMS" minOccurs="0">
	<xs:complexType>
		<xs:all>
			<xs:element name="Missatge" minOccurs="0">
				<xs:simpleType>
					<xs:restriction base="xs:string">
						<xs:maxLength value="512"/>
					</xs:restriction>
				</xs:simpleType>
			</xs:element>
		</xs:all>
	</xs:complexType>
</xs:element>
```
* `/SMS/Missatge`
Aquest element conté un únic camp amb el cos del missatge que s'enviarà com a avis de la notificació.

#### DiesAvisos
```xml
<xs:element name="DiesAvisos" minOccurs="0"/>
	<xs:complexType>
		<xs:sequence>
			<xs:element name="DiaAvis" type="xs:positiveInteger" minOccurs="0" maxOccurs="unbounded"/>
		</xs:sequence>
	</xs:complexType>
</element>
```
* `/DiesAvisos/DiaAvis`
Llista d'elements d'aquest tipus amb els número de dies a comptar a partir del enviament per el qual s'enviaran recordatoris al/s destinatari/s de la notificació mentre aquesta no es trobi en un estat final.

#### URLs
```xml
<xs:element name="URLs" minOccurs="0">
	<xs:complexType>
		<xs:all>
			<xs:element name="AccesNotificacio" type="xs:string" minOccurs="0"/>
			<xs:element name="AccesLlistat" type="xs:string" minOccurs="0"/>
		</xs:all>
	</xs:complexType>
</xs:element>
```
Aquest element igual que la resta de tags de `<DadesAvisos>` són opcionals, ja que **eNotum** disposa de configuració per entitat i per defecte per a aquests camps, només s'han d'usar en cas de voler modificar aquests valors per notificacions concretes.
* `/URLs/AccesNotificacio` Permet informar l'enllaç d'accés directe que desprès es podrà utilitzar des de les plantilles d'avisos de notificacions.
* `/URLs/AccesLlistat` Permet informar l'enllaç d'accés genèric al llistat de notificacions per a l'organisme i departament de la notificació en concret, que desprès igual que en el cas anterior es podrà utilitzar des de les plantilles d'avisos de notificacions.

### DadesOfici

```xml
<xs:element name="DadesOfici">
	<xs:complexType>
		<xs:sequence>
			<xs:choice>
				<xs:element name="CosNotificacio" type="xs:string" nillable="false"/>
				<xs:element name="IdCosNotificacio" type="xs:integer"/>
			</xs:choice>
			<xs:choice>
				<xs:element name="PeuRecurs">
					<xs:simpleType>
						<xs:restriction base="xs:string">
							<xs:maxLength value="4000"/>
						</xs:restriction>
					</xs:simpleType>
				</xs:element>
				<xs:element name="IdPeuRecurs" type="xs:integer"/>
			</xs:choice>
			<xs:element name="OficinaRegistre" type="xs:string" minOccurs="0"/>
			<xs:element name="UnitatOrganitzativa" type="xs:string" minOccurs="0"/>
		</xs:sequence>
	</xs:complexType>
</xs:element>
```
Aquest element permet específicar les dades d'ofici vinculades a la notificació:

* `/DadesOfici/CosNotificacio`
Cos de la notificació
* `/DadesOfici/IdCosNotificacio`
En comptes d'escriure el cos de la notificació, permet indicar l'identificador d'un cos de notificació predeterminat introduït previament al sistema.
* `/DadesOfici/PeuRecurs`
Peu de recurs.
* `/DadesOfici/IdPeuRecurs`
En comptes d'escriure el peu de recurs, permet indicar l'identificador d'un peu de recurs predeterminat introduït previament al sistema.
* `/DadesOfici/OficinaRegistre`
Codi d'oficina de registre a usar per registrar les notificacions. Aquest paràmetre no és obligatori ja que per defecte el sistema usarà la oficina de registre parametritzada per a l'entitat emissora de les notificacions (organisme\departament). Així, només cal informar aquest paràmetre en el cas que s'hagués de realitzar el registre contra una oficina diferent de la habitual.
* `/DadesOfici/UnitatOrganitzativa`
Codi d'unitat organitzativa a usar per registrar les notificacions. Aquest paràmetre no és obligatori ja que per defecte el sistema usarà la unitat organitzativa (codi organisme) parametritzada per a l'entitat emissora de les notificacions (organisme\departament). Així, només cal informar aquest paràmetre en el cas que s'hagués de realitzar el registre contra una oficina diferent de la habitual.

### Documents

`<Documents>` és una seqüencia de entre 1 i 5 elements de tipus `DocumentType`. Només un document de `<Tipus>Resolució</Tipus>` és obligatori, i opcionalment pot anar acompanyat de fins a 4 documents de `<Tipus>Annex</Tipus>`. La definició de `DocumentType` es pot veure a continuació:
```xml
<xs:complexType name="DocumentsType">
	<xs:sequence>
		<xs:element name="Document" maxOccurs="5">
			<xs:complexType>
				<xs:sequence>
					<xs:element name="Nom" type="xs:string"/>
					<xs:element name="SHA1Digest" type="xs:base64Binary" minOccurs="0"/>
					<xs:choice>
						<xs:element name="Dades" type="xs:base64Binary"/>
						<xs:element name="Ruta" type="xs:string"/>
						<xs:element name="IdFicheroPCI" type="xs:string"/>
					</xs:choice>
					<xs:element name="Tipus">
						<xs:simpleType>
							<xs:restriction base="xs:string">
								<xs:enumeration value="Resolució"/>
								<xs:enumeration value="Annex"/>
							</xs:restriction>
						</xs:simpleType>
					</xs:element>
				</xs:sequence>
			</xs:complexType>
		</xs:element>
	</xs:sequence>
</xs:complexType>
```
* `/Documents/Document/Nom`
Nom del fitxer del document, amb extensió. E.g: _holaRepublica.pdf_
* `/Documents/Document/SHA1Digest`
Resum críptografic en [_SHA1_](https://en.wikipedia.org/wiki/SHA-1) del contingut del document com a string codificat en [_base64_](https://en.wikipedia.org/wiki/Base64).
* `/Documents/Document/Dades`
En cas que és vulgui incrustar el document dins del missatge en _base64_. S'ha de tenir en compte que per a documents on la mida superi **1MB**, no recomanem aquest mecanisme, donat que la _PCI_ té una restricció on el missatge _XML_ de la petició complet no pot superar els **2MB**. 
* `/Documents/Document/Ruta`
Ruta del document pujat al FTP. Aquesta ruta és la relativa dins el directori arrel de l'usuari que es connecta al FTP. Només disponible per als ens que s'han donat d'alta amb la integració per aquesta via. 
* `/Documents/Document/IdFicheroPCI`
Permet adjuntar un document mitjançant _MTOM_. Aquest identificador s'ha de correspondre amb el `<Id>` del `<Fichero>` de les `<DatosGenericos>` de la missatgeria de la _PCI_ per a que **eNotum** pugui recuperar correctament el document. En aquest cas el document s'adjunta en la petició _XML_ i s'ha d'específicar mitjançant el tag `<Contendio>` del `<Fichero>` en fromat _MTOM_ dins de la missatgeria de la _PCI_. Per a més informació podeu consultar [la guia d'integració de la _PCI_](https://www.aoc.cat/knowledge-base/plataforma-de-col-laboracio-administrativa-2/)
* `/Documents/Document/Tipus`
Indica el tipus de documents adjuntat. Els possibles valors són:
* *Resolució* : És obligatori adjuntar un document d'aquest tipus per tramesa.
* *Annex* : Opcionalment és poden adjuntar fins a quatre documents d'aquest tipus.

### Notificació

```xml
<xs:complexType name="NotificacioType">
	<xs:all>
		<xs:element name="Titol">
			<xs:simpleType>
				<xs:restriction base="xs:string">
					<xs:maxLength value="100"/>
				</xs:restriction>
			</xs:simpleType>
		</xs:element>
		<xs:element name="Referencia" type="xs:string"/>
		<xs:element name="DadesRegistre" minOccurs="0">
			<xs:complexType>
				<xs:sequence>
					<xs:element name="NumeroRegistre" type="NoEmptyString"/>
					<xs:element name="DataRegistre" type="xs:dateTime"/>
				</xs:sequence>
			</xs:complexType>
		</xs:element>
		<xs:element name="TipusObjecte" type="TipusObjecteType"/>
		<xs:element name="TipusAcces" type="TipusAccesType" minOccurs="0"/>
		<xs:element name="NivellCertificat" type="xs:string" minOccurs="0"/>
		<xs:element name="DiesExpiracio" type="xs:positiveInteger" minOccurs="0"/>
		<xs:element name="Destinataris" type="DestinatarisType"/>
		<xs:element name="Etiquetes" type="EtiquetesType" minOccurs="0"/>
		<xs:element name="AmbitObjecte" type="xs:string" minOccurs="0"/>
		<xs:element name="Idioma" type="Idioma" minOccurs="0"/>
		<xs:element name="IdNotificacioEmissor" type="xs:string" minOccurs="0"/>
		<xs:element name="Canal" type="CanalType" minOccurs="0"/>
		<xs:element name="Expedient" type="xs:string" minOccurs="0"/>
		<xs:element name="Tramit" type="xs:string" minOccurs="0"/>
		<xs:element name="NumeroCas" type="xs:string" minOccurs="0"/>
	</xs:all>
</xs:complexType>	
```

* `/Notificacio/Titol`
Títol de la notificació, màxim 100 caràcters
* `/Notificacio/Referencia`
Cadena d'identificació de la notificació. Tot i que no cal que aquesta sigui única respecte a les demés notificacions existents al sistema, es recomanable utilitzar seqüències úniques de caràcters per facilitar la localització posterior a les cerques.
* `/Notificacio/DadesRegistre/NumeroRegistre`
Número de registre (opcional) pel cas de notificacions que vinguin pre-registrades. Si el paràmetre ve informat, eNOTUM no realitzarà el registre de la notificació corresponent i la dipositarà usant com a dades d'assentament les proporcionades a la petició.
* `/Notificacio/DadesRegistre/DataRegistre`
Data de l'assentament (opcional) pel cas de notificacions que vinguin pre-registrades. Si el paràmetre ve informat, junt amb el de número de registre, eNOTUM no realitzarà el registre de la notificació corresponent i la dipositarà usant com a dades d'assentament les proporcionades a la petició.
* `/Notificacio/TipusObjecte`
Indica si es tracta d'una notificació o d'una comunicació. Els valors possibles per aquest paràmetre son:
* *NOTIFICACIO* : Indica que l'enviament serà de tipus notificació.
* *COMUNICACIO* : Indica que l'enviament serà de tipus comunicació.
* `/Notificacio/TipusAcces`
Indica el tipus d'accés requerit al ciutadà per tal de poder accedir a la notificació. Els possibles valors són:
* *PPAS*
* *CERT*
* `/Notificacio/NivellCertificat`
En cas d'accés requerit amb certificat, és pot ajustar el nivell d'aquest per l'accés a la notificació.
* `/Notificacio/DiesExpiracio`
Dies a partir dels quals la notificació exiprarà, a contar a partir de la data de dipòsit.
* `/Notificacio/Destinataris`
Dades dels destinataris de la notificació.
* `/Notificacio/Etiquetes`
Etiquetes per a agrupar la notificació.
* `/Notificacio/AmbitObjecte`
Àmbit de la notificació.
* `/Notificacio/Idioma`
Idioma de la notificació. Els possibles valors són:
TODO <<<<<<<
* `/Notificacio/IdNotificacioEmissor`
* `/Notificacio/Canal`
* `/Notificacio/Expedient`
* `/Notificacio/Tramit`
* `/Notificacio/NumeroCas`

TODO: Desenvolupar /Notificacio/Destinataris

TODO: Desenvolupar /Notificacio/Etiquetes

## Petició Resum

[Aquí podeu veure la definició completa del esquema _PeticioResum.xsd_](https://github.com/ConsorciAOC/eNotum/blob/master/missatgeria/v3.2_xsds/PeticioResum.xsd)
