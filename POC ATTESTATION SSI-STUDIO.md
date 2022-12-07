Tester la requête 4 sans attestation dans le portefeuille 

Se connecter au portefeuille avec NIP de 6 caractères

 
 
 

Une fois connecté, l’utilisateur peut utiliser ‘Lire un code QR’ soit pour recevoir une attestation, soit pour vérifier une attestation


 



Se connecter sur SSI-STUDIO pour paramétrer les agents, les schémas, les attestations et les requêtes de présentations

 

Paramétrages des agents

 

Paramétrages des schémas et émissions des attestions via le bouton « Offre »

 















Paramétrages des requêtes de présentation 

 
 



























Vérifier le schéma 1 à travers la requête 4 au niveau du portefeuille  

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


 














L’attestation du schéma 1 non disponible du portefeuille

 


















 

 



















Émettre une attestation schéma 1 et tester la requête 4

Émission de l’attestation du schéma 1

 






 

 




 



Vérifier le schéma 1 à travers la requête 4 au niveau du portefeuille  
Il faut noter que l’attribut « Full Adress » n’est pas défini et par conséquent non disponible au niveau du portefeuille numérique

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

















 





































 
Émettre une attestation schéma 1 et l'attestation schéma adresse et tester la requête 8

Émission de l’attestation du schéma 1

 





 


























 















 


 

















 
 
 
Émission de l’attestation du schéma d’adresse

 









 



 



 


 


 
 

Vérifier le schéma 1 et le schéma adress à travers la requête 8 au niveau du portefeuille  

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



 



 


 



















Émettre une attestation schéma 1 et tester la requête 8

 


 


 
 


 


 


 
Vérifier le schéma 1 à travers la requête 8

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


 

 


 



















Émettre une attestation schéma 2 et tester la requête 8

Émission de l’attestation du schéma 2

  
 



 
 


 




 



 
 
Vérifier le schéma 2 à travers la requête 8

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


 


















Les attributs Parent 1 et parent 2 ainsi que Photo ne sont pas disponibles au niveau du portefeuille car l’attestation du schéma 1 n’a pas été reçu.


 


 



 



















Émettre une attestation schéma 2 et schéma 1 et tester la requête 8 (on veut savoir si le portefeuille donne le choix à l'utilisateur de sélectionner l'attestation qu'il veut)
 
Émission d’attestation de schéma

 



 



 

 

 

 
 
Émission de l’attestation du schéma 1

 







 








 

 

 

 

 
Vérifier le schéma 1 et schéma 2 à travers la requête 8

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


 


 

 
















Émettre 2 attestations schema_adress (1 type d'adresse R et 1 type d'adresse C), tester la requête 10
Émission des 2 attestations du schémas d’adress

 




 

 

 

 


 
 

 


 

 

 

 
Vérifier le schéma adress à travers la requête 10
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

 

 

