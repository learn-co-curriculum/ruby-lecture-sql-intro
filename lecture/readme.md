why databases? why do we need to learn sql?
with the playlister you had to parse some files each time
these essentially served as your database (an in memory database)
we use a database because it’s faster to store and retrieve data, and easier to search for specific bits of data

basic structure of an application -> data, behavoir, UI
tell the story of methods, classes, objects, web framework, persistence

goal is to create a system where we can persist our data, and represent that data using objects, and then interact with the data

-Databases use the client/server model
-Local dev mainly on your own computer
-Production mainly connecting to a remote server
-Similar to local web dev/production dev

networking, TCP, Unix Socket, SSH
any computer on the internet can communicate with any other computer
loopback interface (“localhost” vs 127.0.0.1 vs 0.0.0.0) “/etc/hosts"
just need the ip address and the port, one computer is the server (listener) and one is the client
there are many clients to a database (mysql from the command line, psequel, pgadmin, sequel pro, sqlite database browser, ruby, python)
SQL is the language we use to talk to a database
there are many implementations of sql (sqlite, postgres, mysql, oracle, ms sequel server)
there are also NoSql databases (cassandra, voldemort, riak, mongodb)
where are the files?
i couldn’t figure out where my database was...
 http://vimeo.com/36573119 (sqlite has an actual file, everything else is binary)
how do i move files (mysqldump)

Rails apps are CRUD apps
create = INSERT
read = SELECT
update = UPDATE
delete = DELETE

-server = ps aux | grep postgres

remote connection from command line
psql -h localhost -d fis_sql_book

create db from command line
psql -c "CREATE DATABASE fis_sql_book"

command line
createdb magical (my symlinks are messed up)
psql -d magical -f 01_create_wizards.sql (run a sql file against a db)
psql -d magical (login to specific database)
\d wizards (show table)

connect and type help

SQL servers have multiple dbs.
Each db has multiple tables
Tables have multiple columns and rows
Tables are like spreadsheets

postgres specific commands
\dt (list tables)
\l

Ok to forget the syntax, remember the concepts
CRUD
You'll never use any of this but select (ORMs write the rest)
-Create table

CREATE TABLE wizards(
  id SERIAL PRIMARY KEY,
  name TEXT,
  age INTEGER
);

TYPES!!!!!!

everyone add your favorite wizard

-Drop table

-Alter table

- insert
INSERT INTO wizards (name, age, color) VALUES ('Bigby', 40, 'Yellow');

- Select

select *
what if we have multiple people with the same name
select distinct name, age (distinct for multiple attributes)

- Where
select * from wizards where name = 'harry' and age > 10

-Update
(make sure you use a where clause)
UPDATE wizards SET age=86 WHERE name = 'Eriadna';

-Delete
(make sure you use a where clause)
DELETE FROM wizards WHERE name = 'Bigby'

Relations/Joins/Modeling Data
Let's add powers (power 1-10)
If we add powers to the wizards table what problems would we have?
-potentially a lot of null values for wizards with only one power
-what if powers have attributes themselves (power_1_damage)
-how would we look up all wizards who have a certain power?

move powers to new table with foreign key

CREATE TABLE powers(
  id SERIAL PRIMARY KEY,
  name TEXT,
  damage INTEGER,
  wizard_id INTEGER,
  FOREIGN KEY (wizard_id) REFERENCES wizards (id)
);

Insert your favorite power

How can we get the powers for each wizard
SELECT *
FROM wizards
INNER JOIN powers
ON wizards.id = powers.wizard_id;

What if a wizard has no power
-left join (common) (opposite of right join?)

What if a power has no wizard?
-right join(uncommon)

What if multiple wizards have the same power?
(many to many)
(need a join table)

Show any powers not belonging to any wizard?
Show wizards that don't have a power?

GROUP BY
Show only wizards with more than one power?


select wizards.name, count(wizards.name) as times from wizards
join all_powers
on wizards.id = all_powers.wizard_id
group by wizards.name
having count(wizards.name) > 1