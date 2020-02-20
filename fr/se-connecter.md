# Se connecter à l'API

En creant votre compte, vous etes identifié en tant qu'utilisateur de l'API publique

## S'inscrire sur l'API

### Création d'un compte

L'utilisation de l'API nécessite la création d'un compte sur le site http://apicollections.parismusees.paris.fr/ afin d'obtenir un token d'authentification nécessaire aux différentes requêtes.

Une fois votre compte créé, vous pouvez générer des tokens pour vos applications.

Pour créer un compte :

![Créer un compte](../images/creer-un-compte.png)

1. survoler le bouton "Connexion" et cliquer sur le bouton "Créer un nouveau compte".
1. renseigner les informations pour votre compte et valider en cliquant sur le bouton "Créer un nouveau compte".
1. un email est envoyé à l'adresse indiquée dans votre compte.
1. cliquer sur le lien dans l'email reçu pour vous connecter à votre compte et renseigner les informations complémentaires (mot de passe, nom, prenom).

### Se connecter

La connexion au site de l'api permet de générer ses tokens, ses identifiants et de tester ses requêtes dans l'explorer avant leur utilisation
voir http://apicollections.parismusees.paris.fr/explorer

## Utilisation des Tokens
### Générer un Token

Pour générer un token:

1. se connecter sur le site http://apicollections.parismusees.paris.fr/
1. cliquer sur le bouton "Mon compte" puis "Modifier".
1. dans la rubrique "Auth Tokens" du formulaire de compte, cliquer sur le bouton "Add new auth token".
1. renseigner un titre (obligatoire) et une description (facultative) pour le nouveau token.
1. cliquer sur le bouton "Create auth token", le token apparait dans la colonne "API KEY".
1. cliquer sur le bouton "Enregistrer" pour valider le formulaire et la liste des tokens créés.

### Construction de l'appel à l'API
L'appel est réalisé sur le endpoint http://apicollections.parismusees.paris.fr/graphql avec les paramètres suivants :

* 2 paramètres dans le header de la requête :
    * Content-Type : "application/json".
    * ~~auth-token : le token généré sur le compte utilisateur.~~ (déprécié)
    * Authorization: Bearer $token (recommandé)
* 1 paramètre post de la requête :
    * query : la requête graphql. .(voir chapitre 4.2 – Construire une requête)

Le token généré sur un compte utilisateur du site de l'API s'utilise dans le header auth-token de la requête. <br> 
Lorsque le paramètre auth-token est passé à la requête, les droits du compte utilisateur lié à ce token sont appliqués.

Exemple de requête avec cUrl :
```php
<?php

$query = '
{
  nodeById(id: "226737") {
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
}';
$data_string = json_encode(["query" => $query]);
$headers = [
  'Content-Type: application/json',
  'Authorization: Bearer 18d3127b-5c77-4b03-9049-2b3ea85bb74b',
];

$chObj = curl_init(http://apicollections.parismusees.paris.fr/graphql');
curl_setopt($chObj, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($chObj, CURLOPT_POST, TRUE);
curl_setopt($chObj, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($chObj, CURLOPT_HTTPHEADER, $headers);

$response = curl_exec($chObj);

return json_decode($response);
```

### Limitations
Si vous utilisez l'API publique, il y a un quota de 1000 requêtes par jour à ne pas dépasser, vous êtes également limité à 5 tokens pour 5 applications distinctes. <br>
Si votre projet nécessite d'effectuer plus de requêtes ou d'exploiter d'autres données disponibles dans l'API privée, nous vous invitons à demander une mise à jour de votre compte en utilisant le formulaire de contact http://parismusees.paris.fr/fr/contact

### Récuperation de données
Le résultat retourné est au format JSON.

L'un des grands avantages de GraphQL réside dans le caractère intuitif de la syntaxe de la requête et des réponses correspondantes. La réponse aura le même format que la requête spécifiée, avec les valeurs correspondantes.

## API Privée
Si vous souhaitez effectuer un plus grand nombre de requete ou utiliser les données des oeuvres qui ne sont pas libres de droit, veuillez contacter le service numérique des musées de la ville de Paris : http://www.parismusees.paris.fr/fr/contact

# 
1. [Documentation de l'API du portail des collections](README-fr.md#documentation-de-lapi-du-portail-des-collections)
2. [Se connecter à l'API](se-connecter.md#se-connecter-à-l'API)
3. [Récupérer des données](recuperer-donnees.md#récupérer-des-données)
4. [Structure des données](structure-donnees.md#structures-des-données)