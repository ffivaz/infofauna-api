# infofauna-api
L'API info fauna permet d'accéder de manière électronique aux informations mises à disposition par info fauna (anciennement Centre suisse de cartographie de la faune). Les données actuellement disponibles sont encore limitées, puisque le projet est encore en phase de test.

L'adresse pour l'accès aux données est https://lepus.unine.ch/api.

## Formats
Les données sont actuellement disponibles au format [rdf](https://www.w3.org/RDF/). Le format [json](https://www.json.org/) sera également mis à disposition. Les données qui concernent la distribution seront mises à disposition sous forme cartographique via notre serveur [WM(T)S](https://fr.wikipedia.org/wiki/Web_Map_Tile_Service).

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

### Distribution selon la grille 5x5 km
La distrbution des espèces est données selon une grille de 5x5 km. La description de chaque localité est conforme aux standards de Darwin Core (https://dwc.tdwg.org/terms/#Location). Pour le crapaud accoucheur, les données, pour le centre de la Ville de Berne sont disponibles à l'adresse: https://lepus.unine.ch/api/distribution/70110/grid/600200.


```rdf
<?xml version="1.0" encoding="UTF-8"?>
<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:dc="http://purl.org/dc/terms/" xmlns:dwc="http://rs.tdwg.org/dwc/terms/">
  <rdf:Description rdf:about="https://lepus.unine.ch/api/distribution/70110/grid/600200">
    <dc:language>fr</dc:language>
  </rdf:Description>
  <dwc:Taxon rdf:about="https://lepus.unine.ch/api/distribution/70110/grid/600200">
    <dwc:taxonID>70110</dwc:taxonID>
    <dwc:family>Alytidae</dwc:family>
    <dwc:genus>Alytes</dwc:genus>
    <dwc:specificEpithet>obstetricans</dwc:specificEpithet>
    <dwc:scientificName>Alytes obstetricans  (Laurenti, 1768)</dwc:scientificName>
    <dwc:taxonRank>species</dwc:taxonRank>
  </dwc:Taxon>
  <dc:Location rdf:about="https://lepus.unine.ch/api/distribution/70110/grid/600200">
    <dwc:countryCode>CH</dwc:countryCode>
    <dwc:decimalLatitude>202500</dwc:decimalLatitude>
    <dwc:decimalLongitude>602500</dwc:decimalLongitude>
    <dwc:geodeticDatum>EPSG:21781</dwc:geodeticDatum>
  </dc:Location>
</rdf:RDF>
```
