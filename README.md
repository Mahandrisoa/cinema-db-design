

# BASE DE DONNEES XML
Le but de ce projet est de réaliser une modélisation d'une base de données relationnelle en Xml, d'en tirer les DTD ou [Document Type Definition](https://fr.wikipedia.org/wiki/Document_type_definition)
pour la validation mais aussi de créer des implémentations Xsd ou [Xml Schema Definition](https://fr.wikipedia.org/wiki/XML_Schema) qui ont été conçu afin de pallier à un certain nombre de lacunes observées dans les DTD.   
Par la suite, des instances de données seront élaborées ainsi que des reqêtes Xquery pour exploiter notre nouvelle base de données Xml.


## Modelisation de la base de données
Notre modèle physique de données de notre base de départ est comme suit:   
-  **Personne**( __id_personne__ :int, nom_personne: str, prenom_personne: str, date_naissance: date),  
-  **Film**( __id_film__ :int, nom_film:str, date_sortie: str),   
-  **Genre**( __id_genre__ : int, nom_genre: str),   
-  **Acteur**( __#id_film__ : int, __#id_personne__: int),   
-  **Realisateur**( __#id_film__ : int, __#id_personne__: int),   
-  **Film_genre**( __#id_film__ : int, __#id_genre__: int),
-  **Critique**( __#id_personne__ : int, __#id_flim__: int, note: float, commentaire: text)


Voici une illustration montrant notre modèle Entité relation:     
  
  
![Modèle Entité-Relation!](https://www-apps.univ-lehavre.fr/forge/bda_group/bda/raw/master/modele_entite_relation.jpg "Modèle Entité Relation")  
   
## Création de la DTD  
Notre DTD est présenté dans le fichier [cinema.dtd](https://www-apps.univ-lehavre.fr/forge/bda_group/bda/blob/master/cinema.dtd)


## Création du XML Schema  
Par lequel on soutire notre Xml Schema [cinema.xsd](https://www-apps.univ-lehavre.fr/forge/bda_group/bda/blob/master/cinema.xsd):  


## Fichiers d'instances  
Les instances de notre Xml Schema qui serviront pour tester notre modèle sont disponibles dans les fichiers xmls suivants:  
1.  [personnes.xml](https://www-apps.univ-lehavre.fr/forge/bda_group/bda/blob/master/instances/personnes.xml)
2.  [realisateurs.xml](https://www-apps.univ-lehavre.fr/forge/bda_group/bda/blob/master/instances/realisateurs.xml)
3.  [acteurs.xml](https://www-apps.univ-lehavre.fr/forge/bda_group/bda/blob/master/instances/acteurs.xml)
4.  [genres.xml](https://www-apps.univ-lehavre.fr/forge/bda_group/bda/blob/master/instances/genres.xml)
5.  [films.xml](https://www-apps.univ-lehavre.fr/forge/bda_group/bda/blob/master/instances/films.xml)
6.  [film_genres.xml](https://www-apps.univ-lehavre.fr/forge/bda_group/bda/blob/master/instances/film_genre.xml)
  

## Requêtes XQueries  
Xquery est un langage de requête informatique permettant non seulement d'extraire des informations d'un document XML, ou d'une collection de documents XML, mais également d'effectuer des calculs complexes à partir des informations extraites et de reconstruire de nouveaux documents ou fragments XML.

Voici quelques exemples appliqués sur notre base de données XML:  
* Liste du nombre de films par réalisateurs  
`element film_realisateurs {         
  for $id in distinct-values(data(realisateurs/realisateur/@id_personne))  
  return  
  element film_realisateur {    
    personnes/personne[@id_personne=$id],  
   element nbFilms { count(films/film/realisateur[@id_personne=$id]) }     
   }  
}`  

*  Liste du nombre d'acteurs par films  
`element films_acteurs {
    for $film in films/film 
    return element film_acteur {
        element film {
            element id_film {data($film/@id_film)},
            element nom {data($film/@nom_film)},
            element date_sortie {data($film/@date_sortie)}
        }, 
        element nombre_acteurs {
            count(acteurs/acteur[@id_film=$film/@id])
        } 
    }
}`

*  Liste du nombre de films par genre   
`
element genre_films {
    for $genre in genres/genre
    return element genre_film {
        $genre, 
        element nombre_films {
            count (film_genres/filmgenre[@id_genre= $genre/@id_genre] )           
        }
    }
}
`

*  La moyenne des notes de critiques de chaque films
`
element moyenne_critiques {
    for $film in films/film 
    return     
    element moyenne_critique { 
        element film { data($film/@nom_film) },
        element note_moyenne { 
            avg($film/critique/note)
        }
    }      
}`  

## Bilan général  
Nous avons vu comment implémenter notre modèle avec les bases de données Xml, nous avons pu créer des contraintes et validations de
nos elements en DTD puis plus détaillé en Xml Schema. A la fin nous avons interroger notre base de donnée avec les requêtes Xqueries.  
XQuery et SQL sont chacun des langages de requêtes, l'un pour XML et l'autre pour SQL.
Ces deux langages sont très différents. En effet, ils portent sur des modèles de données différents (hiérarchique vs. relationnel). Bien que SQL soit plus ancien, il se contente de porter des requêtes alors que, nous l'avons vu, XQuery est un réel langage de programmation.  
Par contre, il est possible de faire des modifications de données avec SQL alors que le XQuery 1.0 ne propose que d'exploiter les données. 