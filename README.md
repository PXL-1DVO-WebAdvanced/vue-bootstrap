# vue-bootstrap

In dit labo gebruiken we Bootstrap in combinatie met Sass (Scss) in een Vue.js applicatie. We starten met een basis Vue applicatie die enkel een root-component bevat (voorlopig zonder opmaak):

![screenshot applicatie](/opgave/before.png)

## Setup

- Installeer Sass en Bootstrap door onderstaande commando's uit te voeren in de rootfolder van dit project
```
npm install -D sass
npm install -D bootstrap@5.3.3
```
- Maak een **scss** folder aan in de root van het project met de volgende inhoud:
```
scss
|-  components
    |- _footer.scss
    |- _nav.scss
|-  _global.scss
|-  _variabels.scss
|-  main.scss
```
- Plaats onderstaande inhoud in het ***main.scss*** bestand. 
```
@import 'variables'; 
@import '../node_modules/bootstrap/scss/bootstrap';
@import 'global';
@import 'components/nav';
@import 'components/footer';
```
> [!CAUTION] 
> De volgorde is hier van groot belang! De opmaak van Bootstrap wordt grotendeels bepaald door variabelen. Door eerst onze eigen variabelen te importeren kunnen we de waardes van de Bootstrap variabelen beïnvloeden waardoor we onze eigen "touch" kunnen geven aan de Bootstrap componenten. Nadat Bootstrap wordt geïmporteerd kunnen we in onze partials de Bootstrap css-classes nog overschrijven/uitbreiden.

> [!IMPORTANT]
> Omdat Bootstrap geen ondersteuning heeft voor *@use* moeten we ook voor onze eigen partials ***@import*** gebruiken. Een combinatie van de 2 is helaas niet mogelijk
- Importeer het main.scss bestand samen met alle Bootstrap scripts door onderstaande toe te voegen bovenaan in het main.js bestand:
``` main.js
// Import custom scss
import '../scss/main.scss'

// Import all of Bootstrap's JS
import 'bootstrap'
```
- Omdat Bootstrap (nog) niet 100% compatibel is met de laatste versie van Sass krijg je een aantal waarschuwingen tijdens het transpilen van de scss-code. Om de terminal "proper" te houden kan je deze waarschuwingen best onderdrukken door onderstaande eigenschap toe te voegen aan de defineConfig functie in het ***vite.config.js*** bestand:
```
  css: {
    preprocessorOptions: {
      scss: {
        api: 'modern-compiler',
        silenceDeprecations: ["mixed-decls", "import", "color-functions","global-builtin"],
      },
    },
  },
```
- Start de applicatie met ```npm run dev```
  De applicatie wordt nu weergegeven in de standaard "Bootstrap-stijl"

![screenshot applicatie](/opgave/bootstrap.png)

## Customize
> [!TIP]
> Alle informatie die je nodig hebt om Bootstrap volledig naar je hand te zetten vind je terug in het hoofdstuk [Customize](https://getbootstrap.com/docs/5.3/customize/overview/) van de Bootstrap documentatie.

### Variabelen
Op onze webpagina's willen we natuurlijk niet altijd het "Bootstrap-blauw" gebruiken als primaire kleur. Bootstrap maakt gebruik van variabelen om deze kleur te bepalen.

- Voeg onderstaande code toe aan het **_variables.scss** bestand en bekijk het resultaat in je browser!
```
$primary: #33658A;
$secondary: #F26419;
```
- Elk Bootstrap component heeft ook een aantal specifieke variabelen waarmee de opmaak bepaald wordt. Deze vind je meestal terug helemaal onderaan in de documentatie van het component (bv [Accordion](https://getbootstrap.com/docs/5.3/components/accordion/#sass-variables) of [Navbar](https://getbootstrap.com/docs/5.3/components/navbar/#sass-variables)). Aan de hand van de naam kan je meestal vrij goed inschatten waarvoor de variabele gebruikt wordt.
Voeg nu onderstaande code toe aan de **_variables.scss** partial om de opmaak van het Navbar- en Accordion-component aan te passen.
```
$navbar-brand-font-size: 3rem;
$navbar-light-brand-color: $secondary;

$accordion-button-bg: $primary;
$accordion-button-active-bg: $secondary;
```
### Custom class
- We kunnen natuurlijk ook nog steeds zelf de opmaak bepalen door nieuwe css classes te maken! Voeg onderstaande code toe aan het **_global.scss** bestand om de structuur van de pagina te bepalen. Het div-element met deze css-class (zie template App.vue) zal vanaf nu de volledige hoogte innemen van het browser venster (100vh). Van de 3 child-elementen (nav, main en footer) zal het 2de element alle vrije witruimte benutten waardoor de pagina altijd het volledige scherm zal innemen.
```
.grid {
    height: 100vh;
    display: grid;
    grid-template-rows: auto 1fr auto;
} 
```
### Bootstrap class
- Voeg onderstaande code toe aan de **components/_nav.scss** partial om de bestaande Bootstrap classes *navbar-brand* en *navbar-nav* uit te breiden met onze eigen opmaak.
```
@import '../variables';

nav {
    .navbar-brand {
        display: flex;
        align-items: baseline;
        gap: 1rem;
    }

    .navbar-nav {
        width: 100%;
        justify-content: end;
        gap: 2rem;
    }
}
```
> [!CAUTION]
> Ook in de partials moet gebruik gemaakt worden van @import in plaats van @use! De functionaliteit van beide functies is echter hetzelfde.

## Summary
Er zijn verschillende mogelijkheden om Bootstrap naar je hand te zetten. Een belangrijke tool die je hiermee kan helpen zijn de DevTools van je browser. Hiermee kan je eenvoudig een element op je pagina selecteren en zo bepalen door welke css classes en variabelen de opmaak van dat element bepaald wordt! **Ga hier zelf nu mee aan de slag en geef de footer wat meer glans!**


## Resultaat
### Mobile
![screenshot applicatie](/opgave/customized-mobile.png)

### Desktop
![screenshot applicatie](/opgave/customized-desktop.png)