# CALAMITA_Text-to-SQL
This repository contains the instruction for evaluating Text-to-SQL performances achieved by a language model on Termite Dataset

# Database Import Procedure:

- you should download the Termite folder wich is a zip file that needs to be decrypted with password "youshallnotpass".
- inside the Termite folder there is another folder called "database" containing another folder for each database of the dataset.
- each database folder contains 2 files named *db.sql, no_insert_*db.sql) where *db is the name of each database. There are 10 databases included (bowling, centri, coronavirus, voli ...)
- each *db.sql file contains the dump that you need to load on your sql environment. The *db.sql file include the creation of the schema (table,columns and keys) and a set of data insert.
- each no_insert_*db.sql file contains the dump without the data insert. The content of this file will be used to generate the prompt.
- the reason why the prompt contains just the schema creation without the data insert is explained in the paper.

# Text-to-SQL Prompt Template:

- inside 
- as explained in "Database Import Procedure" each folder contained in /Termite/database/ contains a no_insert_*db.sql which is an sql dump used for creating just the database schema (tables,columns and keys) without inserting any data.
- you shold use the content fo
