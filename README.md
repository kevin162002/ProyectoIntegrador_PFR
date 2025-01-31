# ProyectoIntegrador_PFR
Repositorio del proyecto integrador - Programación funcional y reactica

# LIMPIEZA 
Limpieza realizada en datagrip

CREATE DATABASE complete02;
USE complete02;

SELECT DISTINCT * FROM complete_sin_espacios_delimitado_por_comas;

CREATE TABLE compTable AS SELECT distinct * FROM complete_sin_espacios_delimitado_por_comas;

################################################
################################################
################################################

DROP TABLE IF EXISTS auxBelongs_to_collection;

CREATE TABLE auxBelongs_to_collection AS
SELECT
    id,
    TRIM(compTable.belongs_to_collection) AS belongs_to_collection
FROM
    compTable;


SELECT * FROM auxBelongs_to_collection;

SELECT id, belongs_to_collection,
       JSON_VALID(belongs_to_collection)
FROM auxBelongs_to_collection;

## UPDATE general para pasr de apostrofe a doble comilla en los json
UPDATE auxBelongs_to_collection
set belongs_to_collection = REPLACE(belongs_to_collection,'\'','"');

UPDATE auxBelongs_to_collection
SET belongs_to_collection = '{}'
WHERE TRIM(belongs_to_collection) = '';

UPDATE auxBelongs_to_collection
SET belongs_to_collection = REPLACE(belongs_to_collection, 'None','null');

UPDATE auxBelongs_to_collection
set belongs_to_collection = CONCAT(belongs_to_collection,'"}')
WHERE belongs_to_collection NOT LIKE ('%}');

################################################
################################################
################################################

#VALIDACION DE JSON Y CORRECCION DE ERRORES
DROP TABLE IF EXISTS auxGenres;

CREATE TABLE auxGenres AS SELECT id, compTable.genres FROM compTable;

SELECT * FROM auxGenres;

SELECT id, genres,
       JSON_VALID(genres)
FROM auxGenres;

## UPDATE general para pasr de apostrofe a doble comilla en los json
UPDATE auxGenres
set genres = REPLACE(genres,'\'','"');

##UPDATE especifico para el id 105001: falta corchete al final de genres.
UPDATE auxGenres
set genres = REPLACE(genres,'{"id": 35, "name": "Comedy"},','{"id": 35, "name": "Comedy"}]')
WHERE id = 105001;

UPDATE auxGenres
set genres = CONCAT(genres,']')
WHERE id = 100089;

UPDATE auxGenres
set genres = REPLACE(genres,'"Romance"}, {"id"','"Romance"}]')
WHERE id = 110899;

################################################
################################################
################################################

DROP TABLE IF EXISTS auxProduccionCompanies;

CREATE TABLE auxProduccionCompanies AS SELECT id, production_companies FROM compTable;

SELECT * FROM auxProduccionCompanies;

SELECT id, production_companies,
       JSON_VALID(production_companies)
FROM auxProduccionCompanies
/WHERE id = 108869/;

## UPDATE general para pasar de apostrofe a doble comilla en los json
UPDATE auxProduccionCompanies
SET production_companies = REPLACE(production_companies, '{\'','{\"');

UPDATE auxProduccionCompanies
SET production_companies = REPLACE(production_companies, '\': \'','\": \"');

UPDATE auxProduccionCompanies
SET production_companies = REPLACE(production_companies, '\', \'','\", \"');

UPDATE auxProduccionCompanies
SET production_companies = REPLACE(production_companies, ', \'',', \"');

UPDATE auxProduccionCompanies
SET production_companies = REPLACE(production_companies, '\': ','\": ');

UPDATE auxProduccionCompanies
SET production_companies = REPLACE(production_companies, '\'}','\"}');

## UPDATE PARA QUE EN CADA NULL PONGA UNA LISTA VACIA
UPDATE auxProduccionCompanies
SET production_companies = COALESCE(production_companies,'[]');
# IDS MODIFICADOS = 108869  109516  110671

## UPDATE ESPECIFICO PARA REMPLAZAR DOBLE COMILLA POR APOSTROFE
UPDATE auxProduccionCompanies
set production_companies = REPLACE(production_companies,'"ZRF "Syrena""','"ZRF Syrena"')
WHERE id = 101362;

################################################
################################################
################################################

DROP TABLE IF EXISTS auxProduccionCounties;

CREATE TABLE auxProduccionCounties AS SELECT id, compTable.production_countries from compTable;

SELECT * FROM auxProduccionCounties;

SELECT id,
       production_countries,
       JSON_VALID(production_countries)
FROM auxProduccionCounties;

UPDATE auxProduccionCounties
SET production_countries = REPLACE(production_countries, '\'','"');

UPDATE auxProduccionCounties
SET production_countries = '[]'
WHERE production_countries IS NULL;

#UPDATES PARTICULARES

UPDATE auxProduccionCounties
SET production_countries = REPLACE(production_countries,'m"}, {"iso_3166_1"','m"}]')
WHERE id = 116;

UPDATE auxProduccionCounties
SET production_countries = CONCAT(production_countries,'ca"}]')
WHERE id = 1123;

UPDATE auxProduccionCounties
SET production_countries = REVERSE(REPLACE(REVERSE(production_countries),REVERSE(', {"iso_3166_1": "E'),']'))
WHERE production_countries LIKE '%, {"iso_3166_1": "E' AND id = 1116;

UPDATE auxProduccionCounties
SET production_countries = CONCAT(production_countries,'a"}]')
WHERE id = 110415;

UPDATE auxProduccionCounties
SET production_countries = CONCAT(production_countries,'ame": "France"}]')
WHERE id = 1075;

UPDATE auxProduccionCounties
SET production_countries = CONCAT(production_countries,'me": "Belgium"}]')
WHERE id = 105763;

UPDATE auxProduccionCounties
SET production_countries = CONCAT(production_countries,', "name": "Germany"}]')
WHERE id = 101006;

################################################
################################################
################################################

DROP TABLE IF EXISTS auxSpokenLanguages;

CREATE TABLE auxSpokenLanguages AS SELECT id, compTable.spoken_languages from compTable;

SELECT * FROM auxSpokenLanguages;

SELECT id,
       spoken_languages,
       JSON_VALID(spoken_languages)
FROM auxSpokenLanguages;

UPDATE auxSpokenLanguages
SET spoken_languages = REPLACE(spoken_languages, '\'','"');

UPDATE auxSpokenLanguages
SET spoken_languages = '[]'
WHERE spoken_languages IS NULL;

#UPDATES PARTICULARES

UPDATE auxspokenlanguages
set spoken_languages = CONCAT(spoken_languages,'}]')
where id = 105965;

UPDATE auxspokenlanguages
set spoken_languages = REPLACE(spoken_languages,'l"}, {"','l"}]')
where id = 105833;

################################################
################################################
################################################

DROP TABLE IF EXISTS auxKeywords;

CREATE TABLE auxKeywords AS SELECT id, compTable.keywords from compTable;

SELECT * FROM auxKeywords;

SELECT id,
       keywords,
       JSON_VALID(keywords)
FROM auxKeywords;

UPDATE auxKeywords
SET keywords = REPLACE(keywords, '{\'','{\"');

UPDATE auxKeywords
SET keywords = REPLACE(keywords, '\': \'','\": \"');

UPDATE auxKeywords
SET keywords = REPLACE(keywords, '\', \'','\", \"');

UPDATE auxKeywords
SET keywords = REPLACE(keywords, ', \'',', \"');

UPDATE auxKeywords
SET keywords = REPLACE(keywords, '\': ','\": ');

UPDATE auxKeywords
SET keywords = REPLACE(keywords, '\'}','\"}');

UPDATE auxKeywords
SET keywords = '[]'
WHERE keywords IS NULL;

#UPDATES PARTICULARES

UPDATE auxKeywords
set keywords = REPLACE(keywords,'waiterss\\xa0','waiterss')
where id = 107246;

################################################
################################################
################################################

DROP TABLE IF EXISTS auxCast;

CREATE TABLE auxCast AS SELECT id, compTable.cast from compTable;

SELECT * FROM auxCast;

SELECT id,
       cast,
       JSON_VALID(cast)
FROM auxCast;

UPDATE auxCast
SET cast = REPLACE(cast, '"','\'');

UPDATE auxCast
SET cast = REPLACE(cast, '{\'','{\"');

UPDATE auxCast
SET cast = REPLACE(cast, '\': \'','\": \"');

UPDATE auxCast
SET cast = REPLACE(cast, '\', \'','\", \"');

UPDATE auxCast
SET cast = REPLACE(cast, ', \'',', \"');

UPDATE auxCast
SET cast = REPLACE(cast, '\': ','\": ');

UPDATE auxCast
SET cast = REPLACE(cast, '\'}','\"}');


UPDATE auxCast
SET cast = REPLACE(cast, 'None','null');

UPDATE auxCast
SET cast = '[]'
WHERE cast IS NULL;

#UPDATE PARTICULAR

UPDATE auxcast
SET cast = concat(cast,'ofile_path": null}]')
where id = 1116;

UPDATE auxcast
SET cast = REPLACE(cast,', {"cast_id": 73, "character": "Mosep',']')
where id = 11;

UPDATE auxcast
SET cast = REPLACE(cast,'"The smallest one''','''The smallest one''')
where id = 108512;

#'O\\\'Mbrogli', 'O\'Mbrogli'

#UPDATE auxcast
#SET cast = REPLACE(cast,"'O\'Mbrogli'",'O\' Mbrogli')
#where id = 107052;

#{"cast_id": 8, "character": "Jordan Belfort", "credit_id": "52fe4a6dc3a36847f81cd4e7", "gender": 2, "id": 6193, "name": "Leonardo DiCaprio", "order": 0, "profile_path": "/jToSMocaCaS5YnuOJVqQ7S7pr4Q.jpg"}
#{"cast_id": 110, "character": "Maitre d' Hector", "credit_id": "567aec0ac3a3684be9000276", "gender": 0, "id": 1, "name": null, "order": null, "profile_path": null}]
#{"cast_id": 110, "character": "Maitre d' Hector", "credit_id": "567aec0ac3a3684be9000276", "gender": 0, "id": 1

UPDATE auxcast
SET cast = concat(cast,', "name": null, "order": null, "profile_path": null}]')
where id = 106646;

UPDATE auxcast
SET cast = REPLACE(cast,'69, "profile_path": null}, {','69, "profile_path": null}]')
where id = 1018;

#{"cast_id": 8, "character": "Jordan Belfort", "credit_id": "52fe4a6dc3a36847f81cd4e7", "gender": 2, "id": 6193, "name": "Leonardo DiCaprio", "order": 0, "profile_path": "/jToSMocaCaS5YnuOJVqQ7S7pr4Q.jpg"}
#{"cast_id": 76, "character": "Inventor No. 2", "credit_id": "545f72ed0e0a2
#", "gender": null, "id": null, "name": null, "order": null, "profile_path": null}]

UPDATE auxcast
SET cast = concat(cast,'", "gender": null, "id": null, "name": null, "order": null, "profile_path": null}]')
where id = 100042;

################################################
################################################
################################################

DROP TABLE IF EXISTS auxCrew;

CREATE TABLE auxCrew AS SELECT id, compTable.crew from compTable;

SELECT * FROM auxCrew;

SELECT id,
       crew,
       JSON_VALID(crew)
FROM auxCrew;

UPDATE auxCrew
SET crew = REPLACE(crew, '"','\'');

UPDATE auxCrew
SET crew = REPLACE(crew, '{\'','{\"');

UPDATE auxCrew
SET crew = REPLACE(crew, '\': \'','\": \"');

UPDATE auxCrew
SET crew = REPLACE(crew, '\', \'','\", \"');

UPDATE auxCrew
SET crew = REPLACE(crew, ', \'',', \"');

UPDATE auxCrew
SET crew = REPLACE(crew, '\': ','\": ');

UPDATE auxCrew
SET crew = REPLACE(crew, '\'}','\"}');


UPDATE auxCrew
SET crew = REPLACE(crew, 'None','null');

UPDATE auxCrew
SET crew = '[]'
WHERE crew IS NULL;

#UPDATE PARTICULAR

#{"credit_id": "52fe4a6dc3a36847f81cd53f", "department": "Camera", "gender": 2, "id": 275, "job": "Director of Photography", "name": "Rodrigo Prieto", "profile_path": "/uf5OnYiLnHSiAoeqK6lpWK8O0M8.jpg"}
#{"credit_id": "56817c77c3a3684be9010a6b", "departme
#nt": null, "gender": null, "id": null, "job": null, "name": null, "profile_path": null}]

UPDATE auxCrew
SET crew = concat(crew,'nt": null, "gender": null, "id": null, "job": null, "name": null, "profile_path": null}]')
where id = 106646;

################################################
################################################
################################################

DROP TABLE IF EXISTS auxRatings;

CREATE TABLE auxRatings AS SELECT id, compTable.ratings from compTable;

SELECT * FROM auxRatings;

SELECT id,
       ratings,
       JSON_VALID(ratings)
FROM auxRatings;

UPDATE auxRatings
SET ratings = REPLACE(ratings, '{\'','{\"');

UPDATE auxRatings
SET ratings = REPLACE(ratings, '\': \'','\": \"');

UPDATE auxRatings
SET ratings = REPLACE(ratings, '\', \'','\", \"');

UPDATE auxRatings
SET ratings = REPLACE(ratings, ', \'',', \"');

UPDATE auxRatings
SET ratings = REPLACE(ratings, '\': ','\": ');

UPDATE auxRatings
SET ratings = REPLACE(ratings, '\'}','\"}');

UPDATE auxRatings
SET ratings = '[]'
WHERE ratings IS NULL;

