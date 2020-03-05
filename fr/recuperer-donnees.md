# Récupérer des données

Récuperer les données dont vous avez besoin pour vos projet grâce à l'API,  le résultat retourné est au format JSON.

L'un des grands avantages de GraphQL réside dans le caractère intuitif de la syntaxe de la requête et des réponses correspondantes. La réponse aura le même format que la requête spécifiée, avec les valeurs correspondantes.

## GraphQL

### Terminologie

La documentation ci-dessous reflète l’utilisation la plus commune de l’API GraphQL.
Pour une documentation plus complète et exhaustive : https://graphql.org/learn/

#### Schema

Le schéma décrit l’ensemble des données auxquelles un client peut accéder. Les différentes requêtes sont validées et exécutées par rapport au schéma. Vous pouvez retrouver le schéma à cette adresse : http://apicollections.parismusees.paris.fr/explorer, dans la colonne de droite « Documentation explorer »

#### Query / Requête

Par défaut GraphQL permet 2 types d’opérations, les « query » qui correspondent à des requêtes GET pour de la consultation, et les « mutations » qui correspondant à des requêtes POST/DELETE pour de la mise à jour.
Seules les requêtes de consultation sont autorisées sur PM-API

#### Object / Entité

Les objets dans GraphQL représentent les ressources auxquelles vous pouvez accéder. Un objet peut contenir une liste de champs spécifiquement typés

#### Fields / Champs

Un champ est une donnée que vous pouvez extraire d’un objet. Une requête GraphQL consiste essentiellement à sélectionner des champs sur les objets
Les champs peuvent faire référence à des objets. Il faudra alors faire une sous sélection de champs pour cet objet. Les requêtes GraphQL peuvent traverser les objets mentionnés, donnant la possibilité au client de récupérer beaucoup de données en une seule requête.

#### Arguments

Un argument est un ensemble clé-valeur rattaché à un champ. C’est ce qui va permettre de filtrer votre requête

#### Fragment

Si vous interrogez un champ qui fait référence à un objet, vous devrez utiliser des segments pour accéder aux données du type concret sous-jacent. Le champ du fragment ne sera alors retourné que si l’objet correspond au type spécifié par le fragment.

### Construire une requête

L'un des grands avantages de GraphQL réside dans le caractère intuitif de la syntaxe de la requête et des réponses correspondantes. La requête ressemble beaucoup à la façon dont vous voulez que la réponse apparaisse, mais sans les valeurs.

Les requêtes GraphQL ne renvoient que les données spécifiées. Pour former une requête, vous devez spécifier des champs dans les champs (également appelés sous-champs imbriqués) jusqu'à ce que vous ne retourniez que des scalaires.

Toutes les opérations GraphQL doivent spécifier leurs sélections jusqu'à des champs qui renvoient des valeurs scalaires pour assurer une réponse de forme non ambiguë. Cela signifie que si vous essayez de renvoyer un champ qui n'est pas un scalaire, la validation du schéma générera une erreur. Vous devez ajouter des sous-champs imbriqués jusqu'à ce que tous les champs renvoient des scalaires.

Les différentes étapes dans la construction de votre requête :

* Définir le type de requêt, ex:
    * nodeById
    * nodeQuery
    * taxonomyTermQuery
    * etc... <br>
    La liste exhaustive est disponible dans la doc de l’exporer (http://apicollections.parismusees.paris.fr/explorer)
    Le requête peut s’accompagner d’argument, qui permettront de filter les résultats, ex pour la requête nodeQuery :
    * filter
    * sort
    * offset
    * limit
    * revisions
* Définir les champs « communs » à tous les objets à remonter dans les résultats
* Définir les fragments pour récupérer les champs spécifiques à certains objets
* Définir les champs imbriqués jusqu’à ce que tous les champs renvoient des scalaires.

Exemple avec la requête ci-dessous :
```graphql
{
  nodeById(id: "226737") {
    entityCreated,
    entityBundle,
    ... on NodeOeuvre {
      title
      absolutePath
      fieldUrlAlias
      fieldOeuvreThemeRepresente     {
        entity {
          name
        }
      }
      fieldLieuxConcernes {
        entity {
          name
        }
      }
      fieldDonateurs {
        entity {
          name
        }
      }
    }
  }
}
```
« nodeById » => le type de requête pour récupérer un objet par son identifiant<br>
« id : » => l’argument qui permet de spécifier l’ID à récupérer<br>
« entityCreated », « entityBundle » => les champs communs à remonter. Ces champs sont disponibles pour tous les objets d'entité.<br>
« … on NodeOeuvre » => le fragment pour définir spécifiquement l’objet NodeOeuvre. nodebyId peut renvoyer n'importe quel nœud, pas seulement Oeuvre, nous devons donc envelopper les champs spécifiques dans un fragment GraphQL.<br>
« title », « absolutePath », « fieldOeuvreThemeRepresente » => les champs spécifiques à l’objet NodeOeuvre. Ce sont des champs qui ont été ajouté au bundle oeuvre. Ils sont uniquement disponible dans le type NodeOeuvre.<br>
« fieldOeuvreThemeRepresente > entity > name » => les champs imbriqués jusqu’à obtenir une valeur

Cela donnerait un résultat similaire à :
```graphql
{
  "data": {
    "nodeById": {
      "entityCreated": "2019-10-16T23:43:16+0200",
      "entityBundle": "oeuvre",
      "title": "Soleil couchant sur la Seine à Lavacourt, effet d'hiver",
      "absolutePath": "http://parismuseescollections.paris.fr/node/226737",
      "fieldUrlAlias": "petit-palais/oeuvres/soleil-couchant-sur-la-seine-a-lavacourt-effet-d-hiver",
      "fieldOeuvreThemeRepresente": [
        {
          "entity": {
            "name": "Paysage" 
          }
        }
      ],
      "fieldLieuxConcernes": [
        {
          "entity": {
            "name": "Seine" 
          }
        },
        {
          "entity": {
            "name": "Lavacourt" 
          }
        }
      ],
      "fieldDonateurs": [
        {
          "entity": {
            "name": "Brandus, Edward" 
          }
        }
      ]
    }
  }
}
```
Les données récupérées correspondent exactement aux champs que nous avons demandés, dans ce cas, quelques informations sur une oeuvre bien précise, ciblée par son identifiant.
Il est possible d'interroger différentes entités Drupal, et nous allons voir comment interroger différentes parties du portail des collections.

## Autres Ressources
Pour plus d'information sur graphql vous pouvez consulter la documentation officielle sur https://graphql.org/

## L'outil de test explorer et documentation autogénérée
L'explorer, disponible à cette adresse : http://apicollections.parismusees.paris.fr/explorer, permet avant tout de tester vos requêtes avant leur implémentation et ainsi éviter de dépasser le quota avec des requêtes erronées.

La structure des données est également disponible via le schéma GraphQL dans la colonne de droite « Documentation explorer » (ouvrir le panneau « Docs »)

Vous trouverez cependant ci-dessous un descriptif des principales entités pouvant être requêtées.

Les conventions de nommage de GraphQL sont les suivantes :

* Les champs et les propriétés utilisent le format camelCase. Cela signifie que la propriété field_image dans le site devient fieldImage dans GraphQL et que la propriété revision_log devient revisionLog.
* Les types d'entités et les ensembles utilisent le format camelCase avec la première lettre en majuscule. Taxonomy_term devient alors TaxonomyTerm.

### Requêtes d'exemple
Sur la page de l'explorer nous avons ajouté quelque requêtes d'exemples disponibles en cliquant sur les boutons de la partie haute de la page.

Ces exemples proposent des requêtes pour recuperer des oeuvres d'un auteur, des liste d'oeuvres par musées etc...

#
1. [Documentation de l'API du portail des collections](README-fr.md#documentation-de-lapi-du-portail-des-collections)
2. [Se connecter à l'API](se-connecter.md#se-connecter-à-l'API)
3. [Récupérer des données](recuperer-donnees.md#récupérer-des-données)
4. [Structure des données](structure-donnees.md#structures-des-données)