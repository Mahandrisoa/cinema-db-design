# BASE DE DONNEES XML
Le but de ce projet est de réaliser une modélisation d'une base de données relationnelle en Xml, d'en tirer les DTD ou [Document Type Definition](https://fr.wikipedia.org/wiki/Document_type_definition)
pour la validation mais aussi de créer des implémentations Xsd ou [Xml Schema Definition](https://fr.wikipedia.org/wiki/XML_Schema) qui ont été conçu afin de pallier à un certain nombre de lacunes observées dans les DTD.   
Par la suite, des instances de données seront élaborées ainsi que des reqêtes Xquery pour exploiter notre nouvelle base de données Xml.


## Modelisation de la base de données
Notre modèle physique de données de notre base de départ est comme suit:   
-  **Personne**(__id_personne__:int, nom_personne: str, prenom_personne: str, date_naissance: date),  
-  **Film**(__id_film__:int, nom_film:str, date_sortie: str),   
-  **Genre**(__id_genre__: int, nom_genre: str),   
-  **Acteur**(__#id_film__: int, __#id_personne__: int),   
-  **Realisateur**(__#id_film__: int, __#id_personne__: int),   
-  **Film_genre**(__#id_film__: int, __#id_genre__: int),
-  **Critique**(__#id_personne__: int, __#id_flim__: int, note: float, commentaire: text)


Voici une illustration montrant notre modèle Entité relation:     
  
  
![Modèle Entité-Relation!](https://www-apps.univ-lehavre.fr/forge/bda_group/bda/raw/master/modele_entite_relation.png "Modèle Entité Relation")  