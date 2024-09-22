# CALAMITA_Text-to-SQL
This repository contains the instruction for evaluating Text-to-SQL performances achieved by a language model on Termite Dataset.

# Database Import Procedure:

- you should download the Termite folder wich is a zip file that needs to be decrypted with password "youshallnotpass".
- inside the Termite folder there is another folder called "database" containing another folder for each database of the dataset.
- each database folder contains 2 files named *db.sql, no_insert_*db.sql) where *db is the name of each database. There are 10 databases included (bowling, centri, coronavirus, voli ...)
- each *db.sql file contains the dump that you need to load on your sql environment. The *db.sql file include the creation of the schema (table,columns and keys) and a set of data insert.
- each no_insert_*db.sql file contains the dump without the data insert. The content of this file will be used to generate the prompt.
- the reason why the prompt contains just the schema creation without the data insert is explained in the paper.

# Text-to-SQL Prompt Template:

- inside Termite folder there is a json file queries.json.
- queries.json contains a series of items featured by 3 fields ("db_id","original","query").
- each item of query.json includes an query in Italian ("original" field), describing a specific data retrieval task, and the corresponding query in SQL (also known as gold query) executable on a relational database referred on "db_id" field.
- in order to build the Text-to-SQL prompt you just need the original natural language queries.
- as explained in "Database Import Procedure" each folder contained in /Termite/database/ contains a no_insert_*db.sql which is an sql dump used for creating just the database schema (tables,columns and keys) without inserting any data.
- before prompting the model for the Text-to-SQL translation, the model needs a context in which the database schema is considered.
- a first prompt consist in providing the schema dump of the database to which the model has to refer in translating nl queries.
- the template of this prompt is the following: "Considera il seguente database: {dump=no_insert_*db.sql}" where no_insert_*db.sql is the content of the file which is the sql code used to build the schema of the database.
- the model should return a consistent description of the database provided through the sql dump.
- now, maintaining the context on a specific database, the translation prompt can be submitted.
- one by one, the translation prompts are submitted and model response processed.
- the translation prompt template is the following: "Traduci in SQL la seguente query in linguaggio naturale : {original}" where original is a natural language query related to the database considered in the context (found in queries.json by looking at fields "db_id" and "original") 
- the model should return a text containing the sql query.

# Memorize generated queries:
- after the submission of each translation prompt the model should return a text containing the sql query.
- the element to be evaluated is just the sql query, so it's needed a method (possibly a python script) to extract the just the sql code from the text response. This method can vary from one model to another.
- in this case all generated queries needs to be stored inside a txt files in the following folder \evaluation\db_dir\db_name as the name predict.txt.

# Text-to-SQL Evaluation:
- after the submission of each translation prompt the model should return a text containing the sql query.
- the element to be evaluated is just the sql query, so it's needed a method (possibly a python script) to extract the just the sql code from the text response. This method can vary from one model to another.
- each sql query generated needs to be evaluated whether it is correct or not.
- the evaluation metric adopted is Execution Accuracy.
- Execution Accuracy method consists in verifying whether the records returned by the generated query are the same returned by the gold query.
- in order to perform this method over generated queries it is needed a python script that automatically execute both the gold and generated sql query (whether it is executable in absence of syntax errors) and compare the records returned.
- evaluation.py do that for us.
- evaluation.py is a python script that fundamentally works over a set of database stored in local environment to which it connects and over a set 2 text files.
- hence , given a db stored in local environment, the evaluation script will compare the records returned by the gold sql queries file and the records returned by the generated sql queries file (that is basically Execution Accuracy).
- the script will report at the end the execution accuracy over all the database and the execution accuracy for each database.  
  







