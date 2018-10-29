# infofauna-api
L'API info fauna permet d'accéder de manière électronique aux informations mises à disposition par info fauna (anciennement Centre suisse de cartographie de la faune). Les données actuellement disponibles sont encore limitées, puisque le projet est encore en phase de test.

L'adresse pour l'accès aux données est https://lepus.unine.ch/api.

## Formats
Les données sont actuellement disponibles au format [rdf](https://www.w3.org/RDF/). Le format [json](https://www.json.org/) pourrait également être mis à disposition dans le futur. Les données qui concernent la distribution seront également mises à disposition sous forme cartographique via notre serveur [WM(T)S](https://fr.wikipedia.org/wiki/Web_Map_Tile_Service).

Info fauna met à disposition un _endpoint_ pour effectuer des requêtes [SPARQL](https://www.w3.org/TR/rdf-sparql-query/) à l'adresse https://lepus.unine.ch/api/sparql.php. A l'heure actuelle, le nombre de données accessibles via ce point d'accès est très limité.

## Déontologie et accès aux données
Les données mises à disposition peuvent être utilisées sans restriction. Info fauna ne met toutefois pas à disposition les données brutes issues des bases de données, mais des données agrégées conformément à la [déontologie](http://www.cscf.ch/cscf/home/datenverwaltung/datenschutzrichtlinien.html) adoptée par les [centres nationaux de données](https://www.infospecies.ch/fr/).

## Description format rdf
Les propositions du [Darwin Core](http://rs.tdwg.org/dwc/) concernant la présentation standardisée de données liées à la biodiversité seront utilisées pour décrire les données distribuées par info fauna sous forme RDF. Darwin Core est un _vocabulaire_ pour la description de ressources dans le domaine de la biodiversité.

## Données disponibles
### Specimens
Les specimens sont décrites conformément aux éléments de [Dublin Core](http://dublincore.org/) et de [Darwin Core](https://dwc.tdwg.org/terms/#taxon). La donnée https://lepus.unine.ch/api/specimen/1 est ainsi décrite par les informations contenues dans la sortie ci-dessous.

```rdf
<?xml version="1.0" encoding="UTF-8"?>
<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:dwc="http://rs.tdwg.org/dwc/terms/" xmlns:dc="http://purl.org/dc/terms/">
<rdf:Description rdf:about="https://lepus.unine.ch/api/specimen/114">
  <dc:creator>info fauna, Neuchâtel, Switzerland</dc:creator>
  <dc:created>2018-10-29T15:22:37+01:00</dc:created>
</rdf:Description>
<rdf:Description rdf:about="https://lepus.unine.ch/api/specimen/114">
  <dc:type>Specimen</dc:type>
  <dwc:basisOfRecord>Occurrence</dwc:basisOfRecord>
  <dwc:recordedBy>Inventar SO Finder S</dwc:recordedBy>
  <dwc:taxonID>70127</dwc:taxonID>
  <dwc:family>Ranidae</dwc:family>
  <dwc:genus>Rana</dwc:genus>
  <dwc:specificEpithet>temporaria</dwc:specificEpithet>
  <dwc:scientificName>Rana temporaria Linnaeus, 1758</dwc:scientificName>
  <dwc:taxonRank>species</dwc:taxonRank>
  <dwc:vernacularName xml:lang="fr">Grenouille rousse</dwc:vernacularName>
  <dwc:vernacularName xml:lang="de">Grasfrosch</dwc:vernacularName>
  <dwc:vernacularName xml:lang="it">Rana temporaria</dwc:vernacularName>
  <dwc:vernacularName xml:lang="en">Grass Frog</dwc:vernacularName>
  <dwc:country>Switzerland</dwc:country>
  <dwc:countryCode>CH</dwc:countryCode>
  <dwc:stateProvince>SO</dwc:stateProvince>
  <dwc:municipality>OBERBUCHSITEN</dwc:municipality>
  <dwc:decimalLatitude>47.332788209119</dwc:decimalLatitude>
  <dwc:decimalLongitude>7.8024639786892</dwc:decimalLongitude>
  <dwc:geodeticDatum>WGS84</dwc:geodeticDatum>
</rdf:Description>
</rdf:RDF>
```
* __type__ : type de données, selon Dublin Core, généralement un specimen.
* __basisOfRecord__ : la nature de l'information. Ici, une occurrence, l'existence d'un origanisme à un moment et dans un lieu donné.
* __taxonID__ : identifiants de taxon info fauna (NUESP dans nos bases de données). Ce numéro est unique dans nos systèmes. Il ne l’est pas globalement.
* __recordedBy__ : personne ayant récolté la donnée.
* __family__ : famille taxonomique.
* __genus__ : genre taxonomique.
* __specificEpithet__ : espèce taxonomique.
* __taxonRank__ : rang du taxon (famille, genre, espèce, etc.)
* __vernacularName__ : noms vernaculaires, peuvent-être multiples, la langue est donnée par le préfixe xml_lang.
* __country__ : pays.
* __countryCode__ : code pays (ici généralement CH).
* __stateProvince__ : code province, le canton en Suisse.
* __municipality__ : le nom de la commune dans laquelle l'observation a été trouvée.
* __decimalLatitude__ : latitude (du centre du carré 5x5km).
* __decimalLongitude__ : longitude (du centre du carré 5x5km).
* __geodeticDatum__ : Projection utilisée, ici WGS84

Une requête SPARQL permettant d'extraire la liste des espèces prend par exemple la forme suivante :

```sparql
PREFIX dwc: <http://rs.tdwg.org/dwc/terms/>
SELECT DISTINCT ?family ?genus ?species
WHERE
  { ?x dwc:family ?family .
    ?x dwc:genus ?genus .
    ?x dwc:specificEpithet ?species }
 ```

Le résultat de la requête peut être [consulté ici](https://lepus.unine.ch/api/sparql.php?query=PREFIX+dwc%3A+%3Chttp%3A%2F%2Frs.tdwg.org%2Fdwc%2Fterms%2F%3E%0D%0ASELECT+DISTINCT+%3Ffamily+%3Fgenus+%3Fspecies%0D%0AWHERE%0D%0A++%7B+%3Fx+dwc%3Afamily+%3Ffamily+.%0D%0A++++%3Fx+dwc%3Agenus+%3Fgenus+.%0D%0A++++%3Fx+dwc%3AspecificEpithet+%3Fspecies+%7D&output=htmltab&jsonp=&key=2d4d2f1d50ad9429e48f51ab3338e14a&show_inline=1).

Une requête SPARQL permettant d'extraire la liste des coordonnées (les carrés 5x5km) dans lesquelles on trouve Rana temporaria prend par exemple la forme suivante :

```sparql
PREFIX dwc: <http://rs.tdwg.org/dwc/terms/>
SELECT DISTINCT ?latitude ?longitude
WHERE
  { ?x dwc:decimalLatitude ?latitude .
    ?x dwc:decimalLongitude ?longitude .
    ?x dwc:genus "Rana" .    
    ?x dwc:specificEpithet "temporaria" }
 ```

Le résultat de la requête peut être [consulté ici](https://lepus.unine.ch/api/sparql.php?query=PREFIX+dwc%3A+%3Chttp%3A%2F%2Frs.tdwg.org%2Fdwc%2Fterms%2F%3E%0D%0ASELECT+DISTINCT+%3Flatitude+%3Flongitude%0D%0AWHERE%0D%0A++%7B+%3Fx+dwc%3AdecimalLatitude+%3Flatitude+.%0D%0A++++%3Fx+dwc%3AdecimalLongitude+%3Flongitude+.%0D%0A++++%3Fx+dwc%3Agenus+%22Rana%22+.++++%0D%0A++++%3Fx+dwc%3AspecificEpithet+%22temporaria%22+%7D&output=htmltab&jsonp=&key=2d4d2f1d50ad9429e48f51ab3338e14a&show_inline=1).
