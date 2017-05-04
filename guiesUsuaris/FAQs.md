<h1> Preguntes freqüents </h1>

<h3> Quina és la mida màxima dels documents de resolució/annexos per adjuntar a una tramesa?</h3>

Els documents de resolució/annexos que és poden adjuntar a una tramesa de diverses formes:

* Directament com a _base64_ dins del missatge.
* Indicant una ruta FTP (els ens que estan integrats mitjançant aquesta via).
* Indicant una `URL` de descarrega del document.
* Adjuntant el document a través d'alguns dels mecanismes previstos per la _PCI_ (`MTOM`,...)

En el cas d'adjuntar el document directament dins del missatge, la mida màxima permesa del mateix és de **2MB**.
Per a la resta de casos la mida màxima permesa és de **6MB**.
Aquesta és la restricció màxima, a nivell d'ens és pot configurar un límit més restrictiu.
A través de l'aplicació d'eNotum dins [_d'EACAT_](www.eacat.cat), anant a configuració i seleccionant l'ens en qüestió:

IMG

Aquest camp aplica sempre i quan sigui més restrictiu que els **2MB** en cas d'adjuntar via missatgeria, o de **6MB** per a les altres formes. En cas de setejar amb un valor superior no és tindrà en compte.

