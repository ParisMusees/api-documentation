# Structures des données
La structure des données est disponible via le schéma GraphQL, disponible à cette adresse : http://apicollections.parismusees.paris.fr/explorer, dans la colonne de droite « Documentation explorer » (ouvrir le panneau « Docs »)

Vous trouverez cependant ci-dessous un descriptif des principales entités pouvant être requêtées.

La structure des données est disponible via le schéma GraphQL, disponible à cette adresse : http://apicollections.parismusees.paris.fr/explorer, dans la colonne de droite « Documentation explorer » (ouvrir le panneau « Docs »)

Vous trouverez cependant ci-dessous un descriptif des principales entités pouvant être requêtées.

Les conventions de nommage de GraphQL sont les suivantes :

* Les champs et les propriétés utilisent le format camelCase. Cela signifie que la propriété field_image dans le site devient fieldImage dans GraphQL et que la propriété revision_log devient revisionLog.
* Les types d'entités et les ensembles utilisent le format camelCase avec la première lettre en majuscule. Taxonomy_term devient alors TaxonomyTerm.

## Musée
Les données des musées de la Ville de Paris sont disponibles via l'API, comme ce sont des termes de taxonomie vous devez faire précéder l'appel des champs de "... on TaxonomyTermMusee"
voir l'exemple de l'explorer sur la liste des musées.
Vous pouvez retrouver : 

| label | description | type | exemple |
| ----- | ----------- | ---- | ------- |
| entityId | numéro de l'entité, celui ci peut servir pour effectuer des requêtes sur un musée précis | entier | 10 |
| fieldMuseeTitreCourt | Le nom du musée | texte brut | "Maison de Balzac" |
| fieldMuseeLogo { url } | l'url de l'image du logo du musée | url | "http://parismuseescollections.paris.fr/sites/default/files/filefield_paths/logo_musee_balzac.png" |
| fieldAdresse { <br> &nbsp; countryCode <br> &nbsp; locality <br>&nbsp; postalCode <br>&nbsp; addressLine1 <br>&nbsp; addressLine2 <br> } | adresse postale du musée | texte | "fieldAdresse": { <br> &nbsp; &nbsp; "countryCode": "FR", <br> &nbsp; &nbsp; "locality": "Paris", <br> &nbsp; &nbsp; "postalCode": "75016", <br> &nbsp; &nbsp; "addressLine1": "47, rue Raynouard", <br> &nbsp; &nbsp; "addressLine2": "" <br>} |
| fieldGeolocation {<br> &nbsp; lat<br> &nbsp; lng<br> &nbsp; latSin<br> &nbsp; latCos<br> &nbsp; lngRad<br> &nbsp; data<br>} | informations de géolocalisation | entier | "fieldGeolocation": {<br> &nbsp; &nbsp; "lat": 48.8555508,<br> &nbsp; &nbsp; "lng": 2.2795915, <br> &nbsp; &nbsp; "latSin": 0.75305318380701,<br> &nbsp; &nbsp; "latCos": 0.6579596510107,<br> &nbsp; &nbsp; "lngRad": 0.039786377275476,<br> &nbsp; &nbsp; "data": [] <br>} |

## Oeuvre
Les oeuvres sont les contenus principaux du portail des collections, un exemple d'interrogation d'une oeuvre est disponible sur l'explorer depuis le bouton "Soleil couchant de Monet"
il est possible de récupérer une oeuvre directement depuis son id en mentionnant nodeById(id: "XXXXX") avec XXXXX le nid de l'oeuvre souhaitée, veuillez préciser ... on NodeOeuvre pour la récuperation des champs

| label | description | type | exemple |
| ----- | ----------- | ---- | ------- |
| entityId | numéro de l'entité, celui ci peut servir pour effectuer des requêtes sur un musée précis | entier | 10 |
| fieldMuseeTitreCourt | Le nom du musée | texte brut | "Maison de Balzac" |
| fieldMuseeLogo { url } | l'url de l'image du logo du musée | url | "http://parismuseescollections.paris.fr/sites/default/files/filefield_paths/logo_musee_balzac.png" |
| fieldAdresse { <br> &nbsp; countryCode <br> &nbsp; locality <br>&nbsp; postalCode <br>&nbsp; addressLine1 <br>&nbsp; addressLine2 <br> } | adresse postale du musée | texte | "fieldAdresse": { <br> &nbsp; &nbsp; "countryCode": "FR", <br> &nbsp; &nbsp; "locality": "Paris", <br> &nbsp; &nbsp; "postalCode": "75016", <br> &nbsp; &nbsp; "addressLine1": "47, rue Raynouard", <br> &nbsp; &nbsp; "addressLine2": "" <br>} |
| fieldGeolocation {<br> &nbsp; lat<br> &nbsp; lng<br> &nbsp; latSin<br> &nbsp; latCos<br> &nbsp; lngRad<br> &nbsp; data<br>} | informations de géolocalisation | entier | "fieldGeolocation": {<br> &nbsp; &nbsp; "lat": 48.8555508,<br> &nbsp; &nbsp; "lng": 2.2795915, <br> &nbsp; &nbsp; "latSin": 0.75305318380701,<br> &nbsp; &nbsp; "latCos": 0.6579596510107,<br> &nbsp; &nbsp; "lngRad": 0.039786377275476,<br> &nbsp; &nbsp; "data": [] <br>} |

## Personnes (Auteur, Donateur...)
Les objets "personnes" peuvent être récupérés au travers des oeuvres, il s'agit de terme de la taxonomie "Personne institution Personnage" dont le nom machine est "pip", ces champs sont donc récupérables en utilisant ... on TaxonomyTermPip

| label | description | type | exemple |
| ----- | ----------- | ---- | ------- |
| tid | numéro de l'entité | entier | 217291 |
| name | Nom de la personne | texte brut | Salmon, Marie-José |
| fieldPipDateNaissance | date de naissance de la personne | objet date | "startYear": 1929 |
| fieldPipDateDeces | date de décès de la personne | objet date | "startYear": 2001 |
| fieldPipLieuNaissance | Lieu de naissance | texte brut | "Soissons" |
| fieldLieuDeces | Lieu de décès | texte brut | "Senlis" |

## Images et autres media
Les images disponibles pour les utilisateurs de l'API publique sont les images libres de droit et les images au petit format des oeuvres (vignette)

voici ce qu'il faut ajouter à la requête pour la recuperation de la vignette :
```graphql
fieldVisuels {
  entity {
    vignette
  }
}
```
Les images non libres et les media sont disponible uniquement sur l'API privée

#
1. [Documentation de l'API du portail des collections](README-fr.md#documentation-de-lapi-du-portail-des-collections)
2. [Se connecter à l'API](se-connecter.md#se-connecter-à-l'API)
3. [Récupérer des données](recuperer-donnees.md#récupérer-des-données)
4. [Structure des données](structure-donnees.md#structures-des-données)