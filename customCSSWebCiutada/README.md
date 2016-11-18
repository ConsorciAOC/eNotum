# Personalització estils *WebCiutada*

L'objectiu d'aquest readme és la de guiar en la generació d'un fitxer d'estils `CSS` per a la personalització del *WebCiutada* d'eNotum. Aquesta personalització és totalment opcional i en cas de no dur-se a terme el full d'estils que s'aplicacarà per al *WebCiutada* del ens en questió serà el estil per defecte, que podeu trobar [aqui](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/webCiutadaPlantilla.css) i que també us pot servir de template a l'hora de fer la personalització.

# Estils

Tots els estils que ara veureu estàn definits al [webCiutadaPlantilla.css](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/webCiutadaPlantilla.css) i s'explicarà un a un el seu efecte sobre *webCiutada*. Evidentment es poden modificar propietats dels estils diferents als especificats a la plantilla base, l'únic que es recomana fer-ho amb cura i validar-ho bé per evitar possiblese **efectes indesitjats**.

# General

La modificació dels estils dins d'aquest apartat, afectarà als elements dins del _cos_ de la web, no afectarà ni la _capçalera_ ni el _peu_:

![headerBodyFooter](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/headerBodyFooter.jpg)

### Color dels enllaços

Per als enllaços es permet definir el *color* per defecte i el *color* quan el cursor passa per sobre dels mateixos, en l'estil base el color és el mateix per ambdos casos:

```css
/* Color enllaços */
a {color: #0d8ed1;}
a:hover {color: #0d8ed1;}
```
Com hem comentat abans això tindrà afectació per a tots els enllaços del _cos_ de la web, com a exemple mostrarem el enllaços del llistat de notificacions:

![ColorLinkBase](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/ColorLinkBase.png)

Si es modifiquen el valor del _color_ en el `CSS` per exemple per:

```css
/* Color enllaços */
a {color:purple;}
a:hover {color:green;}
```
El resultat en aquests mateixos enllaços de la notificació seria:

![ColorLinkModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/ColorLinkModificat.png)

I amb el cursor a sobre:

![ColorLinkHoverModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/ColorLinkHoverModificat.png)

### Color de fons

Aquesta és la propietat que s'aplica com a base per al color de fons:

```css
/* Color fons pàgina */
body {background: #f1f1f1;}
```
![BackgroundColorBase](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/BackgroundColorBase.png)

Si es modifica per exemple per el següent:

```css
/* Color fons pàgina */
body {background: greenyellow;}
```

![BackgroundColorModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/BackgroundColorModificat.png)

### Tipus de lletra

També es pot modificar el tipus de lletra de l'aplicació modificant la següent propietat:

```css
/* Color fons pàgina */
body {font-family: Helvetica, Arial, Verdana, sans-serif;}
```


# Capçalera

Els següents estils permeten la modificació de la _capçalera_.

### Capçalera logotip del ens

Els següent estil permet modificar el color de fons de la part de la capçalera que conté el logotip del ens:

```css
/* Barra blanca logotip */
#header { background-color: #FFF;}  /* Color fons */
```

![HeaderColorDefecte](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/HeaderColorDefecte.png)

Si es modifica per:

```css
/* Barra blanca logotip */
#header { background-color: rebeccapurple;}  /* Color fons */
```
![HeaderColorModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/HeaderColorModificat.png)

### Enapçalament

El següent estil permet modificar la barra negra de la _capçalera_ més concretament el color del titol, el color de fons de la barra, així com la mida, el color i el tipus del marge inferior d'aquesta:

```css
/* Barra negra encapçalament */
h2 {color: #fff;} /* Color títol */
#headerNotifica{
	background-color: #333333;/* Color fons */
	border-bottom: 1px solid #d9d9d9; /* Mida, tipus i color marge inferior */
}
```

![HeaderColorDefecte](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/HeaderColorDefecte.png)

Si es modifica per:

```css
/* Barra negra encapçalament */
h2 {color: blue;} /* Color títol */
#headerNotifica{
	background-color: red;/* Color fons */
	border-bottom: 10px dotted green; /* Mida, tipus i color marge inferior */
}
```

![BarNotificacionsModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/BarNotificacionsModificat.png)

# Notificacions

Els següents estils tenen afectació sobre el llistat de notificacions i de la plana de detall de les mateixes.

### Titols

Els següent estil permet modificar el color del títol en la plana de detall de notificacions i en el llistat

```css
/* Títol detall i del llistat de notificacions */
.box_mid_header h3 { color: #000; }
```
![TitleColorLlistatNoti](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/TitleColorLlistatNoti.png)
![TitleColorDetallNoti](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/TitleColorDetallNoti.png)
![TitleColorPreviNoti](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/TitleColorPreviNoti.png)

I si es modifica per:

```css
/* Títol detall i del llistat de notificacions */
.box_mid_header h3 { color: violet; }
```

![TitleColorLlistatNotiModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/TitleColorLlistatNotiModificat.png)
![TitleColorDetallNotiModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/TitleColorDetallNotiModificat.png)
![TitleColorPreviNotiModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/TitleColorPreviNotiModificat.png)

### Títol cerca

El següent estil permet modificar el color dels titol del camp de cerca:

```css
/* Títol Cerca */
.box_small_header h3 {color: #4b4b4b;} /* Color text */
```
![TitleCercaColor](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/TitleCercaColor.png)

I si es modifica per:

```css
/* Títol Cerca */
.box_small_header h3 {color: violet;} /* Color text */
```

![TitleCercaColorModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/TitleCercaColorModificat.png)

### Filtres

El següent estil defineix el color de fons i el color del text dels filtres aplicats:

```css
/* Etiqueta filtres seleccionats */ 
.etiqueta {background-color: #333;} /* Color fons*/
.etiqueta a {color: #fff;} /* Color text */
```
![Filtre](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/Filtre.png)

I si es modifica per:

```css
/* Etiqueta filtres seleccionats */ 
.etiqueta {background-color: blue;} /* Color fons*/
.etiqueta a {color: yellow;} /* Color text */
```

![FiltresModificats](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/FiltresModificats.png)

### Barra torneu a la bústia

Aquests estils defineixen el color de fons, i la mida, tipus i color del marge inferior de la barra de torneu a la bústia:

```css
/* Barra Torneu a la bústia */
.breadcrumb{
	border-bottom:1px solid #ebebeb; /* Mida, tipus i color marge inferior */
	background:#f8f8f8; /* Color fons barra */
}
```

![BarraTornar](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/BarraTornar.png)

I si es modifica per:

```css
.breadcrumb{
	border-bottom:3px dashed blue; /* Color bord inferior */
	background:violet; /* Color fons barra */
}
```

![BarraTornarModificat](https://github.com/ConsorciAOC/eNotum/blob/master/customCSSWebCiutada/img/BarraTornarModificat.png)
