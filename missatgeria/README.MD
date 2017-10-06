
# 1 Introducció

A continuació és descriu el funcionament i les diferents modalitats de consum de la versió de missatgeria _3.2_ d'eNotum.

Les versions antigues de missatgeria segueixen sent compatibles i funcionen correctament, tot i que disposen de menys funcionalitats i són susceptibles de ser discontinuades en un futur, si comenceu ara la integració haurieu de fer-ho amb aquesta versió. 

De totes formes podeu consultar les versions antigues de missatgeria i altres documents d'integració a través del [nostre portal de suport a la integració](https://www.aoc.cat/portal-suport/e-notum-base-coneixement/idservei/enotum/#integracio).

# 1.1 Integració PCI

Tal i com es reflecteix a la següent figura, el Backend d'_eNotum_ s'integra dins de l'arquitectura de la PCI a mode d'un nou servei accessible a través de la MTI. 

Per tant els integradors que vulguin accedir a l'_eNotum_ ho hauran de fer a través de la missatgeria de la PCI utilitzant l'element `<DatosEspecificos>` d'aquesta, per a més informació podeu consultar [el document d'integració de la PCI aqui](https://www.aoc.cat/knowledge-base/plataforma-de-col-laboracio-administrativa-2/idservei/enotum/)


