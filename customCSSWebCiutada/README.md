# Personalització estils *WebCiutada*

L'objectiu d'aquest readme és la de guiar en la generació d'un fitxer d'estils `CSS` per a la personalització del *WebCiutada* d'eNotum. Aquesta personalització és totalment opcional i en cas de no dur-se a terme el full d'estils que s'aplicacarà per al *WebCiutada* del ens en questió serà el estil per defecte, que podeu trobar [aqui](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/webCiutadaPlantilla.css) i que també us pot servir de template a l'hora de fer la personalització.

# Estils

Tots els estils que ara veureu estàn definits al [webCiutadaPlantilla.css](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/webCiutadaPlantilla.css) i s'explicarà un a un el seu efecte sobre *webCiutada*. Tocar estils diferents als especificats a la plantilla pot provocar **efectes indesitjats**, aleshores penseu que les modificacions són **at your own risk!**

# General

Comencem doncs, la modificació dels estils dins d'aquest apartat, afectarà als elements dins del _cos_ de la web, no afectarà ni la _capçalera_ ni el _peu_:

![headerBodyFooter](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/headerBodyFooter.jpg)

### Color dels enllaços

Per als enllaços es permet definir *color* per defecte i quan el cursor passa per sobre dels mateixos, en l'estil base el color és el mateix per ambdos:

```css
/* Color enllaços */
a { color: #0d8ed1; }
a:hover { color: #0d8ed1; }
```
Per exemple els enllaços del llistat:

![ColorLinkBase](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/ColorLinkBase.png)





