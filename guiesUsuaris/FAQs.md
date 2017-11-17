<h1> Preguntes freqüents </h1>

<h3> Quina és la mida màxima dels documents de resolució/annexos per adjuntar a una tramesa?</h3>

Els documents de resolució/annexos que és poden adjuntar a una tramesa de diverses formes:

* Directament com a _base64_ dins del missatge.
* Indicant una ruta FTP (els ens que estan integrats mitjançant aquesta via).
* Adjuntant el document a través d'alguns dels mecanismes previstos per la _PCI_ (`MTOM`,...)

En una tramesa és obligatori adjuntar un document de tipus _Resolució_ i opcionalment és poden adjuntar fins a un màxim de 4 documents més de tipus _Annex_. Fent un total màxim de 5 documents.

En el cas d'adjuntar el/s document/s directament dins del missatge, la mida màxima permesa per un document és de **2MB**.
Per a la resta de casos la mida màxima permesa per un document és de **6MB**.
Aquesta és la restricció màxima. A nivell d'ens és pot configurar un límit més restrictiu.
A través de l'aplicació d'eNotum dins [_d'EACAT_](www.eacat.cat), anant a configuració i seleccionant l'ens en qüestió:

![grandariaMaximaEnsPortletEACAT](https://github.com/ConsorciAOC/eNotum/blob/master/guiesUsuaris/imgs/grandariaMaximaEnsPortletEACAT.png)

Aquest camp aplica sempre i quan sigui més restrictiu que els **2MB** en cas d'adjuntar via missatgeria, o de **6MB** per a les altres formes. En cas de setejar amb un valor superior no és tindrà en compte.

**NOTA ADICIONAL**: Degut a una restricció futura de la PCI, proximament en cas d'ajuntar el/s document/s en _base64_ dins del missatge, el total del missatge (document/s i resta de tags inclosos) no podrà superar els **2MB**. Es recomana per tant utilitzar una dels altres mecanismes en cas de voler adjuntar document/s pesats.
