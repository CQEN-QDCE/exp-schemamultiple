[![img](https://img.shields.io/badge/Cycle%20de%20Vie-Phase%20d%C3%A9couverte-339999)](https://www.quebec.ca/gouv/politiques-orientations/vitrine-numeriqc/accompagnement-des-organismes-publics/demarche-conception-services-numeriques)
[![License](https://img.shields.io/badge/Licence-LiLiQ--R-blue)](LICENSE)
# Exploration sur les schémas de type Anoncreds par rapport aux requêtes de présentation, obtenir des informations d'une ou plusieurs attestations numériques disponibles dans le portefeuille numérique d’un détenteur

## Table des matières

1. [Objectifs](#10-objectifs)

2. [Contexte](#20-contexte)

3. [Environnement d\'expérimentation](#30-environnement-dexpérimentation)

   1. [Conditions initiales et prémisses](#31-conditions-initiales-et-prémisses)

   2. [Médias standard disponibles](#32-médias-standard-disponibles)

4. [Démarche](#40-démarche)

5. [Attestation d\'identité numérique](#50-le-contenu-de-lattestation)

6. [Résultats attendus](#60-résultats-attendus)

7. [Analyse](#70-analyse)

8. [Conclusion](#80-conclusion)

9. [Licence](#90-licence)

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

