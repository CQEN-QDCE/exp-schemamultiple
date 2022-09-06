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

> Lorsqu’on lance un agent Demo sans spécifier un fichier Genesis ou l’URL du registre distribué, par défaut il va s’attacher au registre local von-network roulant sur  http://localhost:9000.

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

> :grey_exclamation: Il n’y a pas d’attestation associée au Schéma 3 dans le portefeuille d’Alice. 

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

## 7.0 Résultats attendus

Les résultats de l’exécution de chaque requête de présentation sont affichés à la fin de chaque requête de  présentation de la section précédente.

## 8.0 Analyse

## 9.0 Conclusion

## 10.0 Détails techniques à considérer

## 11.0 Références 

[Becoming a Hyperledger Aries Developer - Part 7: Present Proof | Laurence de Jong](https://ldej.nl/post/becoming-a-hyperledger-aries-developer-part-7-present-proof/)

## 12.0 Licence
Distribué sous Licence Libre du Québec – Réciprocité (LiLiQ-R). Voir [LICENCE](LICENSE) pour plus d'informations.


