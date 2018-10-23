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
### Espèces
Les espèces sont décrites conformément aux taxons Darwin Core (https://dwc.tdwg.org/terms/#taxon). [Alytes obstetricans](https://lepus.unine.ch/carto/index.php?nuesp=70110&rivieres=on&lacs=on&hillsh=on&data=on&year=2000), le crapaud accoucheur, est comme décrit plus bas dans notre sortie rdf. Les données sont disponibles à l’adresse : https://lepus.unine.ch/api/species/70110.

```rdf
<?xml version="1.0" encoding="UTF-8"?>
<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:dwc="http://rs.tdwg.org/dwc/terms/">
  <dwc:Taxon rdf:about="https://lepus.unine.ch/api/species/70110">
    <dwc:taxonID>70110</dwc:taxonID>
    <dwc:family>Alytidae</dwc:family>
    <dwc:genus>Alytes</dwc:genus>
    <dwc:specificEpithet>obstetricans</dwc:specificEpithet>
    <dwc:scientificName>Alytes obstetricans (Laurenti, 1768)</dwc:scientificName>
    <dwc:taxonRank>species</dwc:taxonRank>
    <dwc:vernacularName xml:lang="fr">Crapaud accoucheur</dwc:vernacularName>
    <dwc:vernacularName xml:lang="de">Geburtshelferkröte</dwc:vernacularName>
    <dwc:vernacularName xml:lang="it">Rospo ostetrico</dwc:vernacularName>
    <dwc:vernacularName xml:lang="en">Midwife Toad</dwc:vernacularName>
  </dwc:Taxon>
</rdf:RDF>
```

* __taxonID__ : identifiants de taxon info fauna (NUESP dans nos bases de données). Ce numéro est unique dans nos systèmes. Il ne l’est pas globalement.
* __family__ : famille taxonomique.
* __genus__ : genre taxonomique.
* __specificEpithet__ : espèce taxonomique.
* __taxonRank__ : rang du taxon (famille, genre, espèce, etc.)
* __vernacularName__ : noms vernaculaires. A notre connaissance, le format ne permet pas de spécifier la langue.

Une requête SPARQL sur la base de ces données peut par exemple prendre la forme suivante :

```sparql
PREFIX dwc: <http://rs.tdwg.org/dwc/terms/>
SELECT ?nuesp ?family ?genus ?species
WHERE
  { ?x dwc:taxonID ?nuesp .
    ?x dwc:family ?family .
    ?x dwc:genus ?genus .
    ?x dwc:specificEpithet ?species }
```

### Distribution selon la grille 5x5 km
La distrbution des espèces est données selon une grille de 5x5 km. La description de chaque localité est conforme aux standards de Darwin Core (https://dwc.tdwg.org/terms/#location). Pour le crapaud accoucheur, les données, pour le centre de la Ville de Berne sont disponibles à l'adresse: https://lepus.unine.ch/api/distribution/70110/grid/600200.


```rdf
<?xml version="1.0" encoding="UTF-8"?>
<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:dc="http://purl.org/dc/terms/" xmlns:dwc="http://rs.tdwg.org/dwc/terms/" xmlns:dwciri="http://rs.tdwg.org/dwc/iri/">
  <rdf:Description rdf:about="https://lepus.unine.ch/api/distribution/70110/grid/600200">
    <dc:language>fr</dc:language>
      <dwc:taxonID>https://lepus.unine.ch/api/species/70110</dwc:taxonID>
      <dwc:countryCode>CH</dwc:countryCode>
      <dwc:decimalLatitude>202500</dwc:decimalLatitude>
      <dwc:decimalLongitude>602500</dwc:decimalLongitude>
      <dwc:geodeticDatum>EPSG:21781</dwc:geodeticDatum>
  </rdf:Description>
</rdf:RDF>
```

* __taxonID__ : lien vers l'identification de taxon décrite plus haut.
* __countraCode__ : code pays (ici généralement CH).
* __decimalLatitude__ : coordonnées Y du centre du carré 5x5km.
* __decimalLongitude__ : coordonnées X du centre du carré 5x5km.
* __geodeticDatum__ : Code [EPSG](http://spatialreference.org/ref/epsg/) des coordonnées (projection utlisée), le 21781 correspond à la projection suisse CH1903 / LV03.
