# POC - Requête de présentation - tester dans le portefeuille numérique gouvernementale

## Table des matières

1. [Objectifs](#10-objectifs)

2. [Prérequis](#20-prérequis)

3. [Tester la requête 4 sans attestation dans le portefeuille](#30-tester-la-requête-4-sans-attestation-dans-le-portefeuille)

2. [Émettre une attestation schéma 1 et tester la requête 4](#40-émettre-une-attestation-schéma-1-et-tester-la-requête-4)

3. [Émettre une attestation schéma 1 et l\'attestation schéma adresse et tester la requête 8](#50-émettre-une-attestation-schéma-1-et-lattestation-schéma-adresse-et-tester-la-requête-8)

4. [Émettre une attestation schéma 1 et tester la requête 8](#60-émettre-une-attestation-schéma-1-et-tester-la-requête-8)

5. [Émettre une attestation schéma 2 et tester la requête 8](#70-émettre-une-attestation-schéma-2-et-tester-la-requête-8)

6. [Émettre une attestation schéma 2 et schéma 1 et tester la requête 8](#80-émettre-une-attestation-schéma-2-et-schéma-1-et-tester-la-requête-8)

7. [Émettre 2 attestations schema adress et tester la requête 10](#90-émettre-2-attestations-schema-adress-et-tester-la-requête-10)



## 1.0 Objectifs

- Savoir le comportement des requêtes de présentation dans le portefeuille
	
- Utiliser l'outil SSI_Lab développé par Martin St-Pierre pour tester les requêtes de la POC [précédente](https://github.com/CQEN-QDCE/exp-schemamultiple) avec le portefeuille numérique.
	

## 2.0 Prérequis

- Portefeuille numérique version 1.0.3-82

-  [SSI-Studio](https://ssi-studio.apps.exp.openshift.cqen.ca)

-  [Agent emetteur](https://aries-issuer-admin.apps.exp.openshift.cqen.ca)

## 3.0 Tester la requête 4 sans attestation dans le portefeuille

 Se connecter au portefeuille avec NIP de 6 caractères

 ![image](https://user-images.githubusercontent.com/120060804/206300257-418e8440-4fb0-4e50-b943-4b46c5beed67.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206300483-a3e8b79e-dd31-424b-b651-509bb2f0ceab.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206300524-0efac93d-6d55-4417-8ebe-ca011e4e8b40.png)

 Une fois connecté, l’utilisateur peut utiliser ‘Lire un code QR’ soit pour recevoir une attestation, soit pour vérifier une attestation

![image](https://user-images.githubusercontent.com/120060804/206300586-09a4e9fd-e722-4f43-80d4-fe9e0d47a423.png)

 Se connecter sur SSI-STUDIO pour paramétrer les agents, les schémas, les attestations et les requêtes de présentations

 ![image](https://user-images.githubusercontent.com/120060804/206300666-1d29658c-3f45-4ed0-a119-ecd0cff3105b.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206300715-7afe859c-6334-4f36-95e5-0b51af7884d3.png)

#### Paramétrages des agents

 ![100](https://user-images.githubusercontent.com/120060804/206742828-54f94f39-2e1f-4658-945d-edefdb8caf75.PNG)

 ![101](https://user-images.githubusercontent.com/120060804/206743000-edde8a81-54f8-4160-8927-5b86aa20a1d9.PNG)

 ![image](https://user-images.githubusercontent.com/120060804/206300759-5a6b167d-b8ab-477c-91d9-0f264f156fcd.png)

#### Paramétrages des schémas et émissions des attestions via le bouton « Offre »

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

 ![102](https://user-images.githubusercontent.com/120060804/206743119-3d0b129b-7045-4004-9951-213542d42ff4.PNG)

 ![103](https://user-images.githubusercontent.com/120060804/206743346-39bd3aa0-6c9d-4adc-bc97-7f609a0de9e0.PNG)

 ![image](https://user-images.githubusercontent.com/120060804/206300803-65936a3d-8d1f-421e-bcd7-6a591c758c01.png)

#### Paramétrages des requêtes de présentation 

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

 ![104](https://user-images.githubusercontent.com/120060804/206744823-5c6928cf-44e9-4592-8db3-f962d629be4c.PNG)

 ![105](https://user-images.githubusercontent.com/120060804/206744868-75e36d1c-be7b-4245-94e9-bab1e176f93b.PNG)

 ![106](https://user-images.githubusercontent.com/120060804/206744896-576a3486-3ebc-49f7-85f6-4f9a03648cbd.PNG)

 ![107](https://user-images.githubusercontent.com/120060804/206744943-796ec93c-2cf2-43d7-aed3-bdb716a0ce81.PNG)

 ![image](https://user-images.githubusercontent.com/120060804/206300881-e4aa2693-dbfc-4b05-9230-cf41ab456e52.png)

#### Vérifier le schéma 1 à travers la requête 4 au niveau du portefeuille  

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

 ![image](https://user-images.githubusercontent.com/120060804/206301824-1ce94189-4b61-4bd7-88e5-6f104ba6cd30.png)

 ![image](https://user-images.githubusercontent.com/120060804/206301869-f4d61102-fb13-4cf8-9602-dc942da4773f.png)
 
 > :white_check_mark: Résultats:

 > - L'attestation de schéma 1 n'a pas été trouvée dans le portefeuille numérique.
 
 ![image](https://user-images.githubusercontent.com/120060804/206301924-aaec1aca-1a27-425c-87f4-d857c8ddc365.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206301991-16eec79d-5f3e-412a-99ee-43b54530bcc5.png)

## 4.0 Émettre une attestation schéma 1 et tester la requête 4

#### Émission de l’attestation du schéma 1

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

 ![image](https://user-images.githubusercontent.com/120060804/206302384-125400be-b502-4fa7-8e2f-f50cfb0dc94f.png)

 ![image](https://user-images.githubusercontent.com/120060804/206302423-bcde734c-0e25-4492-85fc-ebfe3b134629.png)

 ![image](https://user-images.githubusercontent.com/120060804/206302457-28ec0d10-a861-4371-b4c7-877a1e93adc7.png)

 ![image](https://user-images.githubusercontent.com/120060804/206302491-9d02830a-ec3f-47da-bdcc-6ca48a0b90ad.png)

 ![image](https://user-images.githubusercontent.com/120060804/206302516-d83e815b-6acd-4df2-8b7a-4c9180d93595.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206302553-e00d8c96-4b8b-4dcb-b170-76daf3bae1f0.png)

 ![image](https://user-images.githubusercontent.com/120060804/206302586-8d2d6bec-4e38-4a5f-a9f2-4b78617edf52.png)

#### Vérifier le schéma 1 à travers la requête 4 au niveau du portefeuille  

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

![image](https://user-images.githubusercontent.com/120060804/206302887-aa510fb6-fae5-4c07-8f7d-19fc63fa4271.png)

> :white_check_mark: Résultats:

> - L'’attribut « Full Adress » n’est pas défini et par conséquent non disponible au niveau du portefeuille numérique.

![image](https://user-images.githubusercontent.com/120060804/206302911-10669f50-9e54-4545-885a-bb5bd8aa8a57.png)

![image](https://user-images.githubusercontent.com/120060804/206302949-f099cf25-d929-48a7-ae02-b8db2a62104d.png)
 
## 5.0 Émettre une attestation schéma 1 et l'attestation schéma adresse et tester la requête 8

#### Émission de l’attestation du schéma 1

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

 ![image](https://user-images.githubusercontent.com/120060804/206303108-33951956-356a-4864-b00e-724c5c79b02d.png)

 ![image](https://user-images.githubusercontent.com/120060804/206303155-22831942-5e64-4c99-b999-19b44128d7cd.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206303210-61d4dff7-807a-40e4-b9fa-5a2ed3fa8693.png)

 ![image](https://user-images.githubusercontent.com/120060804/206303235-2f818f36-31b4-4815-9e26-6fe32ce839c3.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206303267-31c11843-08ee-44b8-a5e9-a948c68d9447.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206303301-3b286522-c225-48dc-a7a4-4ccd5de3d3e3.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206303332-115330d0-faf3-4fe7-8f86-8658d5b56444.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206303363-50f7caad-c7f2-4859-9269-fa6971a8b17f.png)
 
#### Émission de l’attestation du schéma d’adresse

```json
{
  "attributes": [
    "full_adress"
  ],
  "schema_name": "schema_adress",
  "schema_version": "1.0"
}
```

 ![image](https://user-images.githubusercontent.com/120060804/206303508-9d7d0515-f5f7-42bb-9210-71c5f97e79fa.png)

 ![image](https://user-images.githubusercontent.com/120060804/206303533-8e246ca9-d59b-40c0-beca-5149130c8c38.png)

 ![image](https://user-images.githubusercontent.com/120060804/206303557-8b951c7d-7486-4dc2-972c-fc798bb2e14b.png)

 ![image](https://user-images.githubusercontent.com/120060804/206303580-8501b746-d9f6-40eb-9d7c-9b2a14ca3dca.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206303608-c82ec318-cf13-4319-9c23-3e5aa2eb8e4c.png)

 ![image](https://user-images.githubusercontent.com/120060804/206303631-fcf2d971-af09-4dae-a48f-3227fa4e2ced.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206303658-acab08d8-c33d-414c-ada5-6e9c6358517c.png)

#### Vérifier le schéma 1 et le schéma adress à travers la requête 8 au niveau du portefeuille  

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

 ![image](https://user-images.githubusercontent.com/120060804/206303717-d7791fc3-f181-4ebc-981b-b724c343664e.png)
 
 > :white_check_mark: Résultats:

 > - Une attestation trouvée pour le regroupement attr_1 et attr_2 du schéma 1.

 ![image](https://user-images.githubusercontent.com/120060804/206303751-6a444773-93a6-4087-a840-f5eed2cc74fc.png)

 ![image](https://user-images.githubusercontent.com/120060804/206303779-d62d44fe-9281-4b9a-a17f-826acd57d054.png)

## 6.0 Émettre une attestation schéma 1 et tester la requête 8

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

 ![image](https://user-images.githubusercontent.com/120060804/206304011-6733ccd3-2c5e-4951-9803-609ed88c9789.png)

 ![image](https://user-images.githubusercontent.com/120060804/206304035-9df97aa2-527c-4c26-9e13-ebe2210ce8ae.png)

 ![image](https://user-images.githubusercontent.com/120060804/206304065-a2be2f26-0e12-40b0-a69d-6ff9dead8fcb.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206304107-7e766b30-afae-4caf-8b80-3b169b725cd9.png)

 ![image](https://user-images.githubusercontent.com/120060804/206304138-67bd77df-f826-4342-9da6-bc06f21188cf.png)

 ![image](https://user-images.githubusercontent.com/120060804/206304164-42810250-13e3-4773-96ca-c285125c01b6.png)

 ![image](https://user-images.githubusercontent.com/120060804/206304207-df702a24-2e36-4743-ae03-6053ea009f98.png)
 
#### Vérifier le schéma 1 à travers la requête 8

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

![image](https://user-images.githubusercontent.com/120060804/206469828-e32d055a-26cf-4152-93b5-7ae44936fe9c.png)

> :white_check_mark: Résultats:

> - L'’attribut « Full Adress » n’est pas défini et par conséquent non disponible au niveau du portefeuille numérique.

![image](https://user-images.githubusercontent.com/120060804/206469924-f24ad411-5a79-4609-bfe9-8ea8e7f0d0cb.png)

![image](https://user-images.githubusercontent.com/120060804/206469990-3d9369f0-eec8-438f-8fd7-e044d7416518.png)

## 7.0 Émettre une attestation schéma 2 et tester la requête 8

#### Émission de l’attestation du schéma 2

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

![image](https://user-images.githubusercontent.com/120060804/206541712-c06d4e4f-7467-427c-b0d6-76856dbeeda6.png)

![image](https://user-images.githubusercontent.com/120060804/206541740-e5d21d31-37f7-4861-b656-7babc77be699.png)

![image](https://user-images.githubusercontent.com/120060804/206541769-3282787f-2ce2-491a-8e87-e214b8cd447c.png)

![image](https://user-images.githubusercontent.com/120060804/206541787-28392db2-b77e-4bac-a38a-15f37649fd3c.png)

![image](https://user-images.githubusercontent.com/120060804/206541863-ee376237-bc10-4fde-9cd8-cc09bfa73fac.png)

![image](https://user-images.githubusercontent.com/120060804/206541904-598500e3-3fe8-42bb-960a-4bb4b8757850.png)

![image](https://user-images.githubusercontent.com/120060804/206541932-0b6af2a3-6609-4ba5-b470-5befacfdc1d9.png)

![image](https://user-images.githubusercontent.com/120060804/206541958-080a91e6-2dad-4017-8b08-47e45f7ef1a7.png)
 
#### Vérifier le schéma 2 à travers la requête 8

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

![image](https://user-images.githubusercontent.com/120060804/206542639-ab4fd215-db90-4761-8161-720b0f4103be.png)

![image](https://user-images.githubusercontent.com/120060804/206542691-81735aed-7486-4ccb-9ed7-6f730e4e21ed.png)

> :white_check_mark: Résultats:

> - Les attributs Parent 1 et Parent 2 ainsi que Photo ne sont pas disponibles au niveau du portefeuille car l’attestation du schéma 1 n’a pas été reçue.

![image](https://user-images.githubusercontent.com/120060804/206543173-b4dee240-ca92-49e8-8d79-2eed4dde1f49.png)

![image](https://user-images.githubusercontent.com/120060804/206543223-9c47f6f6-ae4b-4657-9d95-6dce120490da.png)

## 8.0 Émettre une attestation schéma 2 et schéma 1 et tester la requête 8
 
#### Émission d’attestation de schéma 2

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

 ![image](https://user-images.githubusercontent.com/120060804/206543301-4563b2de-4f12-4d55-b0ba-b284473a9d98.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206543332-bd8ca042-a7e5-4441-955b-5f83f5396f2d.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206543383-ab839565-e056-422d-9163-67f1dc7f8bbc.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206543419-2b5b94a1-035c-4823-aacc-4e375b8334df.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206543445-5d915794-1aa3-4ea7-aec1-8ebb4f120c89.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206543469-28263319-3590-498b-97ff-caf5a5f991fb.png)
 
 ![image](https://user-images.githubusercontent.com/120060804/206543499-a4022024-5e9d-4d7f-9dc7-7490bc1966dc.png)
 
#### Émission de l’attestation du schéma 1

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

![image](https://user-images.githubusercontent.com/120060804/206543750-ce8a8a11-6c58-4ce0-a68e-299b35b2bf9f.png)

![image](https://user-images.githubusercontent.com/120060804/206543781-72c50191-bd74-47e5-a518-f6bea86838a6.png)

![image](https://user-images.githubusercontent.com/120060804/206548171-b2486189-258a-40a9-9221-b9343e11484d.png)

![image](https://user-images.githubusercontent.com/120060804/206573640-a191edec-a9cc-4c4c-8142-709330c154cd.png)

![image](https://user-images.githubusercontent.com/120060804/206573705-32059723-d097-451a-a2c2-8be8db9063f1.png)

![image](https://user-images.githubusercontent.com/120060804/206573849-e2bf1379-1c12-4a3e-9a1d-84fb8c727bfe.png)

![image](https://user-images.githubusercontent.com/120060804/206573893-d16d614f-b732-4228-a67e-6d259cb0680b.png)

![image](https://user-images.githubusercontent.com/120060804/206573931-5389735e-bc58-4045-83f1-ffc6eafeae28.png)

#### Vérifier le schéma 1 et schéma 2 à travers la requête 8

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

![image](https://user-images.githubusercontent.com/120060804/206574212-f106c038-a516-470f-a8c4-537c6d303d3c.png)

> :white_check_mark: Résultats:

> - Une attestation trouvée pour le regroupement attr_1 et attr_2 du schéma 1.

![image](https://user-images.githubusercontent.com/120060804/206574251-73cee692-9fb8-4790-a892-879f6b6a7ded.png)

![image](https://user-images.githubusercontent.com/120060804/206574300-fce82425-a67f-4baf-b43c-b3e0cf416601.png)

## 9.0 Émettre 2 attestations schema adress et tester la requête 10

#### Émission des 2 attestations du schémas d’adress

### Émission de l'attestation du schémas d’adress de type R

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

![image](https://user-images.githubusercontent.com/120060804/206574453-55b948d0-dbdb-4d0d-a9af-39dc29a47241.png)

![image](https://user-images.githubusercontent.com/120060804/206574483-11c1d7a9-0cbc-4607-a631-62ba0b19669f.png)

![image](https://user-images.githubusercontent.com/120060804/206574508-1729962a-cfe4-4bc7-b6c8-096ce3398b98.png)
 
![image](https://user-images.githubusercontent.com/120060804/206574552-17afa454-09b9-46a8-bcbf-69af7e04a176.png)

![image](https://user-images.githubusercontent.com/120060804/206574591-a97f4cd5-e4f7-4975-af59-f278319f9368.png)

![image](https://user-images.githubusercontent.com/120060804/206574639-d711e74c-d541-4858-9c07-76d7c65613fe.png)

### Émission de l'attestation du schémas d’adress de type C

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

![image](https://user-images.githubusercontent.com/120060804/206574679-dd6fbc06-9dda-4429-9cfa-afd254d67a48.png)

![image](https://user-images.githubusercontent.com/120060804/206574706-3349f730-6666-46a7-b58c-71a076a73c54.png)

![image](https://user-images.githubusercontent.com/120060804/206574731-21c054a3-a6a9-4224-9ae8-ac3a834fee31.png)

![image](https://user-images.githubusercontent.com/120060804/206574759-fa1f2774-f7cb-4783-af71-144856e5a1f8.png)

![image](https://user-images.githubusercontent.com/120060804/206574793-077b7eec-84b4-4a0b-83e8-d35c7941f45b.png)

![image](https://user-images.githubusercontent.com/120060804/206574833-3779a424-20ba-49c0-a4aa-5bfae6657cc6.png)
 
#### Vérifier le schéma adress à travers la requête 10

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

![image](https://user-images.githubusercontent.com/120060804/206575354-268859cb-06f6-4d75-9ee1-f5af3fde18fe.png)

> :white_check_mark: Résultats:

> - Une attestation trouvée (associée au schéma schema_adress_2 ) dans le portefeuille numérique.

![image](https://user-images.githubusercontent.com/120060804/206575392-d36ef43e-e07f-45e6-839d-1883213ccd7b.png)



