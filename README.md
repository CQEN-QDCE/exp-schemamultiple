[![img](https://img.shields.io/badge/Cycle%20de%20Vie-Phase%20d%C3%A9couverte-339999)](https://www.quebec.ca/gouv/politiques-orientations/vitrine-numeriqc/accompagnement-des-organismes-publics/demarche-conception-services-numeriques)
[![License](https://img.shields.io/badge/Licence-LiLiQ--R-blue)](LICENSE)
# Exploration sur les schémas de type Anoncreds par rapport aux requêtes de présentation, obtenir des informations d'une ou plusieurs attestations numériques disponibles dans le portefeuille numérique d’un détenteur

## Table des matières

1. [Objectifs](#10-objectifs)

2. [Contexte](#20-contexte)

3. [Environnement d\'expérimentation](#30-environnement-dexpérimentation)

   1. [Conditions initiales et prémisses](#31-conditions-initiales-et-prémisses)

4. [Démarche](#40-démarche)

5. [Schémas et attestations](#50-schémas-et-attestations)

6. [Requêtes de présentation](#60-requêtes-de-présentation)

7. [Résultats attendus](#70-résultats-attendus)

8. [Analyse](#80-analyse)

9. [Conclusion](#90-conclusion)

10. [Détails techniques à considérer](#100-détails-techniques-à-considérer)

11. [Références](#110-références)

12. [Licence](#120-licence)

---

## 1.0 Objectifs

- Analyser la faisabilité d’obtenir des informations (attributs), via des requêtes de présentation, contenues dans une ou plusieurs attestations numériques disponibles dans le portefeuille numérique d’un détenteur (holder) .

- Tester le cas d'utilisation où l’on a plusieurs attestations d'identité différentes, mais qui partagent un même ensemble d’attributs de base (given_name, family_name, birthdate_dateint) et des attributs spécifiques dans chaque attestation. 

- Tester les implications d'avoir deux attestations pour le Québec (identité avec photo et adresse séparées).

- Évaluer différents formats/syntaxes à utiliser dans la construction des requêtes de présentation.

## 2.0 Contexte

Dans les travaux pancanadiens concernant le Person Credential, on essaie de définir un ensemble d'attributs de base d'une identité numérique. Les trois provinces participantes ont des besoins, des enjeux juridiques et des sources de données qui complexifient la tâche d'arriver à une entente à propos d'un schéma unique. 

Cette preuve de concept vise à obtenir une requête de présentation qui favorise l'interopérabilité, pour autant qu’on soit capable de trouver un terrain d'entente entre les trois provinces pour l'ensemble des attributs de base.

Nous prenons ici comme hypothèse que si chaque province définit son propre schéma sans une normalisation des attributs communs, les requêtes de présentation seront beaucoup plus complexes ou encore la complexité serait transférée aux consommateurs des attestations qui devront s'adapter selon la provenance de l'identité de chaque individu, complexifiant l'interopérabilité.

## 3.0 Environnement d'expérimentation

### Prérequis 
* [Git](https://git-scm.com/)
* [docker](https://www.docker.com)
* [von-network](https://github.com/bcgov/von-network)
* [Hyperledger Aries Cloud Agent Python (ACA-Py)](https://github.com/hyperledger/aries-cloudagent-python)

### Facultatifs
* [VSCode](https://code.visualstudio.com)

### Déploiement

Pour la réalisation de cette POC, on a utilisé des instances docker locales d'un registre distribué Indy VON Network et 3 instances de l’agent ACA-PY. Pour l’interaction entre les agents ACA-PY, on a utilisé les interfaces CLI et Swagger de chaque agent.

Il est très recommandable d’avoir suivi la formation [Becoming a Hyperledger Aries Developer](https://www.edx.org/course/becoming-a-hyperledger-aries-developer) préalablement afin d’arriver rapidement à déployer l’infrastructure nécessaire pour cette POC.

### Déploiement VON Network

Accéder à une ligne de commande Bash et cloner le référentiel von-network:

```sh
git clone https://github.com/bcgov/von-network
cd von-network
```

Une fois le dépôt de code cloné , exécuter les commandes suivantes pour déployer les instances docker (4 nœuds et un serveur web):

```sh
./manage build
./manage start 
```
Vous pouvez accéder à l’interface web du registre en allant sur http://localhost:9000.
Pour plus de détails sur le déploiement d’un registre distribué indy VON Network cliquer [ici](https://github.com/cloudcompass/ToIPLabs/blob/main/docs/LFS173xV2/vonNetwork.md).

### Déploiement des Agents ACA-PY 

Accéder à une ligne de commande Bash et cloner le référentiel ACA-PY. La version d’ACA-PY utilisée pour cette preuve de concept est `v0.7.4-rc2`.

```sh
git clone https://github.com/hyperledger/aries-cloudagent-python.git
```
Pour cette POC, on va déployer les 3 agents Demo (Faber, Alice et Acme) disponibles dans le [dépôt de code GitHub de l’agent ACA-PY](https://github.com/hyperledger/aries-cloudagent-python):

1. **Faber** en tant qu'émetteur des schémas et des attestations.
2. **Alice** en tant que détenteur des attestations.
3. **Acme** en tant que consommateur des attestations.

Il est recommandé d’ouvrir un terminal pour chaque agent (instance docker) à déployer afin d’avoir accès aux interfaces CLI offertes par chaque agent. 

Terminal 1:
```sh
cd aries-cloudagent-python/demo
./run_demo Faber
```

Terminal 2:
```sh
cd aries-cloudagent-python/demo
./run_demo Alice
```

Terminal 3:
```sh
cd aries-cloudagent-python/demo
./run_demo Acme
```

> :information_source: Lorsqu’on lance un agent Demo sans spécifier un fichier Genesis ou l’URL du registre distribué, par défaut il va s’attacher au registre local von-network roulant sur  http://localhost:9000.

Par défaut, les agents Faber et Acme ont créé une invitation de connexion (voir le terminal respectif de chaque agent). Vous pouvez utiliser l’interface CLI ou Swagger (API: POST /out-of-band/receive-invitation) d’Alice pour accepter les invitations de Faber et Acme. 

Si tout s’est déroulé sans problème vous devrez avoir accès aux interfaces Swagger de chaque agent:

* Faber : http://localhost:8021/api/doc
* Alice :  http://localhost:8031/api/doc
* Acme : http://localhost:8041/api/doc

### 3.1 Conditions initiales et prémisses

- Un registre distribué von-network est en place.
- Un émetteur (Faber), consommateur (Acme) et détenteur(Alice) des attestions est en place.
- Une connexion entre l’agent émetteur (Faber) et le détenteur (Alice) a été établie.
- Une connexion entre l’agent consommateur (Acme) et le détenteur (Alice) a été établie.

## 4.0 Démarche

1. Déployer le registre distribué von-network
2. Déployer les 3 agents ACA-PY (Faber, Acme et Alice).
3. L'émetteur (Faber) enregistre des schémas et les respectives “credential definitions” dans le registre von-network. Pour plus de détails sur les schémas voir la section 5.0 Schémas et attestations.
4. Établir une connexion a été établie entre l'émetteur (Faber) et le détenteur(Alice). 
5. Établir une connexion a été établie entre le consommateur(Acme) et le détenteur (Alice). 
6. L'émetteur (Faber) émets des attestations à Alice , et Alice les a accepté. Pour plus de détails sur les schémas voir la section 5.0 Schémas et attestations
7. Le consommateur (Acme) envoie des requêtes de présentation au détenteur (Alice)

## 5.0 Schémas et attestations

### Schémas

Les schémas suivants sont inscrits par Faber dans le registre local  VON Network via Swagger

```json 
Swagger:
API: POST /schemas
```
#### Schéma 1 : 
```json 
{
  "attributes": [
    "given_names",
    "family_name",
    "birthdate_dateint",
    "parent_1_full_name",
    "parent_2_full_name",
    "photo",
    "issuing_jurisdiction"
  ],
  "schema_name": "schema_1",
  "schema_version": "1.0"
}
```
#### Schéma 2 : 
```json 
{
  "attributes": [
    "given_names",
    "family_name",
    "birthdate_dateint",
    "photo",
    "full_adress"
  ],
  "schema_name": "schema_2",
  "schema_version": "1.0"
}
```
#### Schéma 3 : 
```json 
{
  "attributes": [
    "given_names",
    "family_name",
    "birthdate_dateint"
  ],
  "schema_name": "schema_3",
  "schema_version": "1.0"
}
```
#### Schéma 4 : 
```json 
{
  "attributes": [
    "full_adress"
  ],
  "schema_name": "schema_adress",
  "schema_version": "1.0"
}
```

#### Schéma 5 : 
```json 
{
  "attributes": [
    "full_adress",
    "adress_type"
  ],
  "schema_name": "schema_adress_2",
  "schema_version": "1.0"
}
```
> :blue_book: Dans le Schéma 5, le champ adress_type sera rempli avec l’information « R » (résidentielle) ou avec l’information « C » (correspondance).

#### Attestations
Faber émets 5 attestations à Alice:

- 1 attestation associée au Schéma 1;
- 1 attestation associée au Schéma 2;
- 1 attestation associée au Schéma 4 ;
- 2 attestations associées au Schéma 5.

> :warning: Il n’y a pas d’attestation associée au Schéma 3 dans le portefeuille d’Alice. 

Les attestation suivants sont émises par Faber à Alice via l’interface Swagger. 

```sh
Swagger:
API:POST /issue-credential-2.0/send
```
> :information_source: Lorsque vous allez utiliser les exemples d’attestions ici-bas, n’oubliez pas de mettre à jour les variables `connection_id` et `cred_def_id` avec celles propres à votre environnement de test.

1. Attestation liée au Schéma 1
```json 
{
   "connection_id":"59ace7d0-8bfc-46c5-8107-9fdb32b6f75a",
   "comment":"commentaires",
   "auto_remove":false,
   "credential_preview":{
      "@type":"https://didcomm.org/issue-credential/2.0/credential-preview",
      "attributes":[
         {
            "name":"given_names",
            "value":"Alice"
         },
         {
            "name":"family_name",
            "value":"Smith"
         },
         {
            "name":"birthdate_dateint",
            "value":"19880801"
         },
         {
            "name":"parent_1_full_name",
            "value":"Jhon"
         },
         {
            "name":"parent_2_full_name",
            "value":"Alexandra"
         },
		 {
            "name":"photo",
            "value":"CodeXYZ"
         },
		 {
            "name":"issuing_jurisdiction",
            "value":"1659381214"
         }
      ]
   },
   "filter":{
      "indy":{
         "cred_def_id":"E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"
      }
   },
   "trace":false
}
```

2. Attestation liée au Schéma 2
```json 
{
   "connection_id":"59ace7d0-8bfc-46c5-8107-9fdb32b6f75a",
   "comment":"commentaires",
   "auto_remove":false,
   "credential_preview":{
      "@type":"https://didcomm.org/issue-credential/2.0/credential-preview",
      "attributes":[
         {
            "name":"given_names",
            "value":"Alice"
         },
         {
            "name":"family_name",
            "value":"Smith"
         },
         {
            "name":"birthdate_dateint",
            "value":"19880801"
         },         
		 {
            "name":"photo",
            "value":"CodeXYZ"
         },
		 {
            "name":"full_adress",
            "value":"1500 Rue Cyrille-Duquet, Québec, QC G1N 2E5"
         }
      ]
   },
   "filter":{
      "indy":{
         "cred_def_id":"E3UVPCktrmwnQWyqY8XSbe:3:CL:17:schema_2"
      }
   },
   "trace":false
}
```

3. Attestation liée au Schéma 4
```json 
{
   "connection_id":"59ace7d0-8bfc-46c5-8107-9fdb32b6f75a",
   "comment":"commentaires",
   "auto_remove":false,
   "credential_preview":{
      "@type":"https://didcomm.org/issue-credential/2.0/credential-preview",
      "attributes":[
         {
            "name":"full_adress",
            "value":"1500 Rue Cyrille-Duquet, Québec, QC G1N 2E5"
         }
      ]
   },
   "filter":{
      "indy":{
         "cred_def_id":"E3UVPCktrmwnQWyqY8XSbe:3:CL:22:schema_adress"
      }
   },
   "trace":false
}
```

4. Attestation liée au Schéma 5 (1)
```json 
{
   "connection_id":"59ace7d0-8bfc-46c5-8107-9fdb32b6f75a",
   "comment":"commentaires",
   "auto_remove":false,
   "credential_preview":{
      "@type":"https://didcomm.org/issue-credential/2.0/credential-preview",
      "attributes":[
         {
            "name":"full_adress",
            "value":"1500 Rue Cyrille-Duquet, Québec, QC G1N 2E5"
         },
		 {
            "name":"adress_type",
            "value":"R"
         }
      ]
   },
   "filter":{
      "indy":{
         "cred_def_id":"E3UVPCktrmwnQWyqY8XSbe:3:CL:182:schema_adress_2"
      }
   },
   "trace":false
}
```

5. Attestation liée au Schéma 5 (2)
```json 
{
   "connection_id":"59ace7d0-8bfc-46c5-8107-9fdb32b6f75a",
   "comment":"commentaires",
   "auto_remove":false,
   "credential_preview":{
      "@type":"https://didcomm.org/issue-credential/2.0/credential-preview",
      "attributes":[
         {
            "name":"full_adress",
            "value":"200 Rue Cyrille-Duquet, Québec, QC G1N 2E5"
         },
		 {
            "name":"adress_type",
            "value":"C"
         }
      ]
   },
   "filter":{
      "indy":{
         "cred_def_id":"E3UVPCktrmwnQWyqY8XSbe:3:CL:182:schema_adress_2"
      }
   },
   "trace":false
}
```

## 6.0 Requêtes de présentation

Dans cette section on affiche les requêtes de présentation ayant un statut de réussi. C’est-à-dire que tant l’agent émetteur de la requête (Acme) comme l’agent destinataire de la requête (Alice) ont accepté le format et la syntaxe utilisés pour la constructions des la requêtes.

> :information_source: Les requêtes de présentation suivantes ont été testées sur la base du protocole [Aries RFC 0454: Present Proof Protocol 2.0](https://github.com/hyperledger/aries-rfcs/tree/eace815c3e8598d4a8dd7881d8c731fdb2bcc0aa/features/0454-present-proof-v2)

Les requêtes de présentation sont envoyés par l’agent Acme à l’agent Alice via Swagger.

```sh
Swagger:
API: POST /present-proof-2.0/send-request
```
> :information_source: Lorsque vous allez utiliser les requêtes de présentation ici-bas, n’oublie pas de mettre à jour les variables `connection_id` et `cred_def_id` avec celles propres à votre environnement de test.


#### Requête 1.a : Un attribut dans un ensemble de 3 attestations en utilisant l’id du “credential definition”

```json 
{
  "connection_id": "3f6e173f-ad71-4e81-a9cf-23d52b89e3e2",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "0_given_names_uuid": {
          "name": "given_names",
          "restrictions": [
            {"cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:18:schema_3"},
            {"cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:17:schema_2"},
            {"cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"}
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

#### Requête 1.b : Un attribut dans un ensemble de 3 attestations en utilisant le nom du schéma

```json
{
  "connection_id": "3f6e173f-ad71-4e81-a9cf-23d52b89e3e2",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "0_given_names_uuid": {
          "name": "given_names",
          "restrictions": [
            {"schema_name": "schema_2"},{"schema_name": "schema_3"},{"schema_name": "schema_1"}
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

> :white_check_mark: Résultats:

- Pour l’attribut given_names, 2 attestations ont été trouvées dans le portefeuille d’Alice, schema_1 et schema_2.


#### Requête 2 : Plusieurs attributs individuels dans des ensembles de 3 attestations

```json
{
  "connection_id": "f9d52241-0d02-47b3-aeae-604d8df53289",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "0_given_names_uuid": {
          "name": "given_names",
          "restrictions": [
            {"cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:18:schema_3"},
            {"cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:17:schema_2"},
            {"cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"}
          ]
        },
       "0_parent_1_name_uuid": {
          "name": "parent_1_full_name",
          "restrictions": [
            {"cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:18:schema_3"},
            {"cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:17:schema_2"},
            {"cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"}
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

> :white_check_mark: Résultats:

- Pour l’attribut given_names, 2 attestations ont été trouvées dans le portefeuille d’Alice, schema_1 et schema_2;

- Pour l’attribut parent_1_full_name, 1 attestation a été trouvée dans le portefeuille d’Alice, schema_1.


#### Requête  3 : Un attribut dans une attestation qu’Alice ne possède pas

```json
{
  "connection_id": "3f6e173f-ad71-4e81-a9cf-23d52b89e3e2",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "0_given_names_uuid": {
          "name": "given_names",
          "restrictions": [
            {"cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:18:schema_3"}
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

> :white_check_mark: Résultats:

- Aucune attestation trouvée dans le portefeuille d’Alice.


#### Requête 4 : Des attributs individuels dans 2 attestations

```json
{
  "connection_id": "3f6e173f-ad71-4e81-a9cf-23d52b89e3e2",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "137388449251487884310892013251656523020",
      "requested_attributes": {
        "0_given_names_uuid": {
          "name": "given_names",
          "restrictions": [
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"
            }
          ]
        },
        "0_birthdate_dateint_uuid": {
          "name": "birthdate_dateint",
          "restrictions": [
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"
            }
          ]
        },
        "0_full_adress_uuid": {
          "name": "full_adress",
          "restrictions": [
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:22:schema_adress"
            }
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

> :white_check_mark: Résultats:

- Deux attestations trouvées dans le portefeuille d’Alice.



#### Requête 5 : Des attributs regroupés dans un ensemble de 3 attestations

```json
{
  "connection_id": "6cffd43d-9474-4783-bc0e-e8c9bfb0cd3f",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "attr_1": {
          "names": [
            "given_names",
            "family_name",
            "birthdate_dateint"
          ],
          "restrictions": [
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:18:schema_3"
            },
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:17:schema_2"
            },
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"
            }
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

> :white_check_mark: Résultats:

- Pour le regroupement d’attributs , deux attestations trouvées dans le portefeuille d’Alice, schema_1 et schema_2.


#### Requête 6 : Des attributs regroupés  dans un ensemble de 3 attestations

```json
{
  "connection_id": "6cffd43d-9474-4783-bc0e-e8c9bfb0cd3f",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "attr_1": {
          "names": [
            "given_names",
            "family_name",
            "birthdate_dateint",
			"parent_1_full_name",
			"parent_2_full_name"
          ],
          "restrictions": [
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:18:schema_3"
            },
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:17:schema_2"
            },
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"
            }
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

> :white_check_mark: Résultats:

- Pour le regroupement d’attributs attr_1, une seule attestation trouvée dans le portefeuille d’Alice, schema_1. 


#### Requête 7.a :  Deux regroupements d’attributs dans deux attestations différentes

```json
{
  "connection_id": "6cffd43d-9474-4783-bc0e-e8c9bfb0cd3f",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "attr_1": {
          "names": [
            "given_names",
            "family_name",
            "birthdate_dateint"			
          ],
          "restrictions": [           
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"
            }
          ]
        },
		"attr_2": {
          "names": [
            "photo",
            "full_adress"			
          ],
          "restrictions": [
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:17:schema_2"
            }
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

#### Requête 7.b :  Trois regroupements d’attributs dans 3 attestations différentes

```json
{
  "connection_id": "6cffd43d-9474-4783-bc0e-e8c9bfb0cd3f",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "attr_1": {
          "names": [
            "given_names",
            "family_name",
            "birthdate_dateint"			
          ],
          "restrictions": [           
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"
            }
          ]
        },
		"attr_2": {
          "names": [
            "photo",
            "full_adress"			
          ],
          "restrictions": [
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:17:schema_2"
            }
          ]
        },
		"attr_3": {
          "name": "full_adress",
          "restrictions": [
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:22:schema_adress"
            }
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

> :white_check_mark: Résultats:

- Les attestations associées à chaque regroupement ont été trouvées dans le portefeuille d’Alice.


#### Requête 8 : Un regroupement d’attributs dans un ensemble des 3 attestations et un regroupement d’attributs dans une attestation spécifique.

```json
{
  "connection_id": "f9d52241-0d02-47b3-aeae-604d8df53289",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "attr_1": {
          "names": [
            "given_names",
            "family_name",
            "birthdate_dateint"
          ],
          "restrictions": [
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:18:schema_3"
            },
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:17:schema_2"
            },
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"
            }
          ]
        },
		"attr_2": {
          "names": [
            "parent_1_full_name",
			"parent_2_full_name",
            "photo"
          ],
          "restrictions": [            
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"
            }
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

> :white_check_mark: Résultats:

- Deux attestations trouvées pour le regroupement attr_1 , schema_1 et schema_2;

- Une attestation trouvées pour le regroupement attr_2, schema_1.


> :information_source: Un des élément important à repérer concernant le format et la syntaxe des requêtes de présentation précédentes c’est la clé (key)  restrictions . Dans le champ restrictions le Consommateur interroge des attributs du portefeuille d’un détenteur en indiquant explicitement  soit la cred_def_id  ou le schema_name .  Dans  les requêtes suivantes, on va montrer la syntaxe pour interroger des attributs du portefeuille d’un détenteur en indiquant explicitement la valeur spécifique d’un attribut dans le champ restrictions .


#### Requête 9.1 :  Un attribut dans les restrictions

```json
{
  "connection_id": "f9d52241-0d02-47b3-aeae-604d8df53289",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "attr_1": {
          "names": [
            "full_adress",
            "adress_type"
          ],
          "restrictions": [            
            {
              "attr::adress_type::value":"R"
            }
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

> :white_check_mark: Résultats:

- Une attestation trouvée (associée au schéma schema_adress_2 ) dans le portefeuille d’Alice


#### Requête 9.1 :  Un attribut dans les restrictions (Alice a 2 attestations qui correspondent à l’attribut cherché)

```json
{
  "connection_id": "f9d52241-0d02-47b3-aeae-604d8df53289",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "attr_1": {
          "names": [
            "given_names",
            "family_name",
            "birthdate_dateint"
          ],
          "restrictions": [            
            {
              "attr::family_name::value":"Smith"
            }
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

> :white_check_mark: Résultats:

- Deux attestations trouvées (associée au schéma schema_1 et schema_2) dans le portefeuille d’Alice.


#### Requête 10 :  Un cred_def_id et un attribut dans les restrictions

```json
{
  "connection_id": "f9d52241-0d02-47b3-aeae-604d8df53289",
  "presentation_request": {
    "indy": {
      "name": "Proof of Identity",
      "version": "1.0",
      "nonce": "93166826414932296800727076347518098347",
      "requested_attributes": {
        "attr_1": {
          "names": [
            "full_adress",
            "adress_type"
          ],
          "restrictions": [            
            {
              "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:182:schema_adress_2",
              "attr::adress_type::value":"R"
            }
          ]
        }
      },
      "requested_predicates": {}
    }
  }
}
```

> :white_check_mark: Résultats:

- Une attestation trouvée (associée au schéma schema_adress_2 ) dans le portefeuille d’Alice.


## 7.0 Résultats attendus

Les résultats de l’exécution de chaque requête de présentation sont affichés à la fin de chaque requête de  présentation de la section précédente.

## 8.0 Analyse
- La requête de présentation peut être considérée comme une recherche faite par le Consommateur dans le portefeuille d’un Détenteur.  Un attribut, ou un groupe d’attributs, est recherché dans une attestation ou un ensemble d’attestations que le Détenteur possède;

- Lorsqu’on construit une requête de présentation, et plus spécifiquement le champ  requested_attributes, il faut faire attention aux détails suivants:

  • Utiliser name  pour la récupération des attributs individuels et names pour la récupération de groupes d’attributs;
  
  • Dans le champ restrictions on peut essayer de récupérer des attributs d’un portefeuille en utilisant la valeur d’un attribut. La syntaxe suivante a été testée:

```json 
  "restrictions": [            
            {
              "attr::nom_attribut::value":"Valeur de l'attribut"
            }
          ]	  
 ```	
  
 - Le format de la réponse du Détenteur à la requête du Consommateur change légèrement si la requête comprend des attributs individuels ou des regroupements d’attributs. La section 10.0 Détails techniques à considérer montre des extraits de la réponse d’un Détenteur;
 
 - Dans le cas d’un regroupement des attributs, la totalité des attributs doit se trouver dans au moins une des attestations dans le portefeuille du Détenteur afin que la recherche soit un succès. 
 

## 9.0 Conclusion

L’expérimentation a permis de  :

- Démontrer qu'un vérificateur est capable, avec une seule requête de présentation, d’obtenir un ensemble d’informations provenant d’attestations d’identité basées sur des schémas différents qui partagent les mêmes noms d’attributs communs (given_name, family_name, birthdate_dateint);

- Démontrer qu’avec une seule requête de présentation, on peut interroger le portefeuille d’un détenteur et obtenir des attributs stockés dans des attestations différentes (ANIG, adresse, photo, etc.). 

- Acquérir la connaissance relative à la construction de requête de présentation, sa syntaxe et format. On a pu constater qu’en plus d’interroger le portefeuille d’un détenteur sur la base du cred_def_id ou schema_name, on peut également le faire en utilisant la valeur spécifique d’un attribut, "attr::nom_attribut::value":"Valeur de l'attribut"

- Nous familiariser avec les enregistrement de blocs dans le registre distribué von-network et les agents ACA-PY


## 10.0 Détails techniques à considérer

Extraits de la réponse (message) provenant d’Alice à la Requête 1 fait par Acme

- La Requête 1 comprend des attributs individuels;

- État de la requête de présentation (côté Acme): presentation-received.

```json 
  {
  "requested_attributes": {
    "0_given_names_uuid": {
      "name": "given_names",
      "restrictions": [
        {
          "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:18:schema_3"
        },
        {
          "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:17:schema_2"
        },
        {
          "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"
        }
      ]
    }
  }
}
 ```

 
 ```json 
{
  "requested_proof": {
    "revealed_attrs": {
      "0_given_names_uuid": {
        "sub_proof_index": 0,
        "raw": "Alice",
        "encoded": "27034640024117331033063128044004318218486816931520886405535659934417438781507"
      }
    },
    "self_attested_attrs": {},
    "unrevealed_attrs": {},
    "predicates": {}
  }
}	  
 ```	
  

Extraits de la réponse (message) provenant d’Alice à la Requête 7.a fait par Acme

- La Requête 7.a comprend des attributs regroupés;

- État de la requête de présentation (côté Acme): presentation-received.

```json 
{
  "requested_attributes": {
    "attr_1": {
      "names": [
        "given_names",
        "family_name",
        "birthdate_dateint"
      ],
      "restrictions": [
        {
          "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:16:schema_1"
        }
      ]
    },
    "attr_2": {
      "names": [
        "photo",
        "full_adress"
      ],
      "restrictions": [
        {
          "cred_def_id": "E3UVPCktrmwnQWyqY8XSbe:3:CL:17:schema_2"
        }
      ]
    }
  }
}
 ``` 

```json 
{
  "requested_proof": {
    "revealed_attrs": {},
    "revealed_attr_groups": {
      "attr_1": {
        "sub_proof_index": 1,
        "values": {
          "given_names": {
            "raw": "Alice",
            "encoded": "27034640024117331033063128044004318218486816931520886405535659934417438781507"
          },
          "family_name": {
            "raw": "Smith",
            "encoded": "72066417326714592639357157411494099840335319789392267512864384015597754702049"
          },
          "birthdate_dateint": {
            "raw": "19880801",
            "encoded": "19880801"
          }
        }
      },
      "attr_2": {
        "sub_proof_index": 0,
        "values": {
          "photo": {
            "raw": "CodeXYZ",
            "encoded": "78976342904209055345930199669889437607013257267866167915837498975775753679980"
          },
          "full_adress": {
            "raw": "501 rue 34 Québec",
            "encoded": "44814093327120579746678040803316506220761275506333990637459898229352725026090"
          }
        }
      }
    },
    "self_attested_attrs": {},
    "unrevealed_attrs": {},
    "predicates": {}
  }
}
 ``` 

> :blue_book: Pour la réalisation de cette POC des modifications mineurs ont été apportées à l’agent demo Acme afin de visualiser en console les attributs, schémas et credential definitions recuperés du portefeuille d’Alice

À remplacer dans le fichier acme.py (ligne 99) [aries-cloudagent-python/acme.py at main · hyperledger/aries-cloudagent-python :](https://github.com/hyperledger/aries-cloudagent-python/blob/main/demo/runners/acme.py)

```json 
async def handle_present_proof_v2_0(self, message):       
        state = message["state"]
        pres_ex_id = message["pres_ex_id"]
        self.log(f"Presentation: state = {state}, pres_ex_id = {pres_ex_id}")

        if state == "presentation-received":
            log_status("#27 Process the proof provided by X")
            log_status("#28 Check if proof is valid")
            proof = await self.admin_POST(
                f"/present-proof-2.0/records/{pres_ex_id}/verify-presentation"
            )
            self.log("Proof = ", proof["verified"])

            
            # check values received
            pres_req = message["by_format"]["pres_request"]["indy"]
            pres = message["by_format"]["pres"]["indy"]
            is_proof_of_identity = (
                pres_req["name"] == "Proof of Identity"
            )
            if is_proof_of_identity:
                log_status("#28.1 Received proof of identity, check claims")
                for (referent, attr_spec) in pres_req["requested_attributes"].items():
                    if "names" in attr_spec:
                        for name in attr_spec['names']:
                            self.log(
                                f"{name}: "
                                f"{pres['requested_proof']['revealed_attr_groups'][referent]['values'][name]['raw']}"
                            )
                    if "name" in attr_spec:
                        self.log(
                            f"{attr_spec['name']}: "
                            f"{pres['requested_proof']['revealed_attrs'][referent]['raw']}"
                        )        

                for id_spec in pres["identifiers"]:
                    # just print out the schema/cred def id's of presented claims
                    self.log(f"schema_id: {id_spec['schema_id']}")
                    self.log(f"cred_def_id {id_spec['cred_def_id']}")
                # TODO placeholder for the next step
            else:
                # in case there are any other kinds of proofs received
                self.log("#28.1 Received ", message["presentation_request"]["name"])
            pass	
```

## 11.0 Références

[Becoming a Hyperledger Aries Developer - Part 7: Present Proof | Laurence de Jong](https://ldej.nl/post/becoming-a-hyperledger-aries-developer-part-7-present-proof/)

## 12.0 Licence
Distribué sous Licence Libre du Québec – Réciprocité (LiLiQ-R). Voir [LICENCE](LICENSE) pour plus d'informations.


