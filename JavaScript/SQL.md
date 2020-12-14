SQL
-

````SQL
    /*Création d'une base de donnée*/
    CREATE DATABASE nom_de_la_base ;
    CREATE DATABASE IF NOT EXISTS nom_de_la_base;

    /*Suppression d'une base de donnée*/
    DROP DATABASE nom_de_la_base ;
    DROP DATABASE IF EXISTS nom_de_la_base;

    /*Création d'une table*/
    CREATE TABLE nom_table(
    nom_colonne1 type_données1,
    nom_colonne2 type_données2,
    nom_colonne3 type_données32
    ….
    nom_colonneN type_donnéesN
    );

    /*Modifier une table*/
    ALTER TABLE nom_table
    Instructions;
    /*Instructions possible :
        ADD, MODIFY, DROP, DROP COLUMN, CHANGE
    */

    /*Création d'une cléé étrangère*/
    ALTER TABLE nom_table
    ADD CONSTRAINT nom_contrainte FOREIGN KEY(nom_cle_etrangere) REFERENCE
    nom_table_reference(nom_champ_reference)

    /*Insersion de données dans une table*/
    INSERT INTO nom_table (nom_colonne1, nom_colonne2, nom_colonne3, nom_colonne_4,….)
    VALUES ("valeur_colonne1", "valeur_colonne2", "valeur_colonne3", "valeur_colonne4","….");

    /*Suppression d'une table*/
    DROP TABLE nom_table;
    DROP TABLE IF EXISTS nom_table;

    /*Mettre à jour des donnéés dans une table*/
    UPDATE nom_table
    SET nom_colonne = valeur
    WHERE condition;

    /*Supprimer des lignes dans une table*/
    DELETE FROM nom_table
    WHERE condition;

    /*Supprimer des lignes dans une table avec réinitialisation de l'auto incrémentation !*/
    TRUNCATE TABLE nom_table;

     
````








