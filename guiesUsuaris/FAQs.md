# Preguntes freqüents

## Quina és la mida màxima dels documents de resolució/annexos per adjuntar a una tramesa?

Els documents de resolució/annexos que és poden adjuntar a una tramesa de diverses formes:

* Directament com a _base64_ dins del missatge.
* Indicant una ruta FTP (els ens que estan integrats mitjançant aquesta via).
* Adjuntant el document al missatge través de la _PCI_ fent ús de [`MTOM`](https://en.wikipedia.org/wiki/Message_Transmission_Optimization_Mechanism). Per a més informació respecte a l'ús d'aquest mecanisme podeu consultar [la guia d'integració de la _PCI_](https://suport.aoc.cat/ca-es/article/?servei=integracio&id=KA-07379_documentacio-generica-per-a-integrar-se-a-la-pci)

En una tramesa és obligatori adjuntar un document de tipus _Resolució_ i opcionalment és poden adjuntar fins a un màxim de 4 documents més de tipus _Annex_. Fent un total màxim de 5 documents.

En el cas d'adjuntar el/s document/s directament dins del missatge, la mida màxima permesa per un document és de **2MB**.
Per a la resta de casos la mida màxima permesa per un document és de **6MB**.

**NOTA ADDICIONAL**: Degut a una restricció futura de la _PCI_, proximament en cas d'ajuntar el/s document/s en _base64_ dins del missatge, el total del missatge (document/s i resta de contingut a excepció unicament dels tags) no podrà superar els **2MB**. Es recomana per tant utilitzar una dels altres mecanismes en cas de voler adjuntar document/s pesats.

# Es pot notificar a telefons estrangers?

**eNotum** accepta números de telèfons estrangers. Els servei web concretament fa els següents passos amb aquest ordre per tal de validar els telèfons:
* Normalitza el telèfon:
  * Si el telèfon hi ha un caràcter `+` el remplaça per `00`.
  * Si el telèfon comença per `0034` elimina aquests caràcters perquè és el prefix d'Espanya.

# Quines validacions es realitzen als telefons?

  * Validem la correctesa de qualsevol número de teléfon de país o regió del mon mitjançant la llibreria https://github.com/googlei18n/libphonenumber
  * Validem que, si el número de telèfon mòbil és espanyol, comenci per 6 o 71, 72, 73, 74 segons el [Plan Nacional de Numeración](https://avance.digital.gob.es/es-ES/Servicios/Numeracion/Documents/Guia_Numeracion.pdf)
