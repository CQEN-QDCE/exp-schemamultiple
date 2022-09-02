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

Accéder à une ligne de commande Bash et cloner le référentiel ACA-PY. La version d’ACA-PY utilisée pour cette preuve de concept est v0.7.4-rc2 .

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

Lorsqu’on lance un agent Demo sans spécifier un fichier Genesis ou l’URL du registre distribué, par défaut il va s’attacher au registre local von-network roulant sur  http://localhost:9000.

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

## 5.0 Schémas et attestations

## 6.0 Requêtes de présentation

## 7.0 Résultats attendus

## 8.0 Analyse

## 9.0 Conclusion

## 10.0 Détails techniques à considérer

## 11.0 Références 

[Becoming a Hyperledger Aries Developer - Part 7: Present Proof | Laurence de Jong](https://ldej.nl/post/becoming-a-hyperledger-aries-developer-part-7-present-proof/)

## 12.0 Licence
Distribué sous Licence Libre du Québec – Réciprocité (LiLiQ-R). Voir [LICENCE](LICENSE) pour plus d'informations.
