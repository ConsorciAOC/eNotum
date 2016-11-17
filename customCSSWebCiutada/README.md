# Personalització estils *WebCiutada*

L'objectiu d'aquest readme és la de guiar en la generació d'un fitxer d'estils `CSS` per a la personalització del *WebCiutada* d'eNotum. Aquesta personalització és totalment opcional i en cas de no dur-se a terme el full d'estils que s'aplicacarà per al *WebCiutada* del ens en questió serà el estil per defecte, que podeu trobar [aqui](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/webCiutadaPlantilla.css) i que també us pot servir de template a l'hora de fer la personalització.

# Estils

Tots els estils que ara veureu estàn definits al [webCiutadaPlantilla.css](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/webCiutadaPlantilla.css) i s'explicarà un a un el seu efecte sobre *webCiutada*. Tocar estils diferents als especificats a la plantilla pot provocar **efectes indesitjats**, aleshores penseu que les modificacions són **at your own risk!**

# General

La modificació dels estils dins d'aquest apartat, afectarà als elements dins del _cos_ de la web, no afectarà ni la _capçalera_ ni el _peu_:

![headerBodyFooter](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/headerBodyFooter.jpg)

### Color dels enllaços

Per als enllaços es permet definir *color* per defecte i quan el cursor passa per sobre dels mateixos, en l'estil base el color és el mateix per ambdos:

```css
/* Color enllaços */
a { color: #0d8ed1; }
a:hover { color: #0d8ed1; }
```
Com hem comentat abans això tindrà afectació per a tots els enllaços del _cos_ de la web, com a exemple mostrarem el enllaços del llistat de notificacions:

![ColorLinkBase](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/ColorLinkBase.png)

Si es modifiquen el valor del _color_ en el `CSS` per exemple per:

```css
/* Color enllaços */
a { color: purple; }
a:hover { color: green; }
```
El resultat en aquests mateixos enllaços de la notificació seria:

![ColorLinkModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/ColorLinkModificat.png)

I amb el cursor a sobre:

![ColorLinkHoverModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/ColorLinkHoverModificat.png)

### Color de fons

Aquesta és la propietat que s'aplica com a base per al color de fons:

```css
/* Color fons pàgina */
body { background: #f1f1f1; }
```
![BackgroundColorBase](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/BackgroundColorBase.png)

Si es modifica per exemple per el següent:

```css
/* Color fons pàgina */
body { background: greenyellow; }
```

![BackgroundColorModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/BackgroundColorModificat.png)

# Capçalera

Els següent estil permet modificar el color de fons de la part de la capçalera que conté el logotip del ens i del color del títol, del font, del text i del bord inferior de la subcapçalera:

```css
/* Barra blanca logotip */
#header { background-color: #FFF;}  /* Color fons */

/* Barra negra encapçalament */
h2 {color: #fff;} /* Color títol */
#headerNotifica{
	background-color: #333333;/* Color fons */
	color:#FFF;/* Color text */
	border-bottom: 1px solid #d9d9d9;/* Color bord inferior */
}
```

Si es modifica per:

```css
/* Barra blanca logotip */
#header { background-color: red;}  /* Color fons */

/* Barra negra encapçalament */
h2 {color: blue;} /* Color títol */
#headerNotifica{
	background-color: red;/* Color fons */
	color:orange;/* Color text */
	border-bottom: 1px solid #d9d9d9;/* Color bord inferior */
}
```


