# Insert data in external database

This example illustrates how to collect data from a user in a Bonita form (more precisely a process instantiation form) and store them in a an external database (in this example a PostgreSQL database).

Note that it is usually recommended to use Bonita Business Data Model (BDM) feature whenever possible for better integration with all Bonita solution components.

## Download

In order to download the example, go to the [release section](https://github.com//Bonitasoft-Community/insert-external-database/releases) and [download the .bos file](https://github.com//Bonitasoft-Community/insert-external-database/releases/latest/download/insert-external-database.bos).

## Install and run

### Database

In order to successfully run this process you need to have a PostgreSQL server up and running.  
The process is configured to connect to a local server with username/password: antoine/antoine.

It also expects to have a database schema named: `my_database`:

```SQL
CREATE DATABASE my_database
  WITH OWNER = antoine
       ENCODING = 'UTF8'
       TABLESPACE = pg_default
       CONNECTION LIMIT = -1;

COMMENT ON DATABASE my_database
  IS 'A database to store some data write by Bonita connectors and read by Bonita REST API extensions (and optionnal connectors)';
```

One table named `todo` is also required:

```SQL
CREATE TABLE public.todo
(
  id integer NOT NULL DEFAULT nextval('todo_id_seq'::regclass),
  description text NOT NULL,
  validated boolean
)
WITH (
  OIDS=FALSE
);
ALTER TABLE public.todo
  OWNER TO antoine;
```

### Import in Bonita Studio

The easiest way to run this example is to import it in Bonita Studio and deploy it in the test environment embedded in Bonita Studio.

To import the process in Bonita Studio go in the `File` menu -> `Import` -> `BOS archive...` and select the .bos file you previously [download](#download).

### Run

Simply click on the run button from the toolbar. Once you submit the instantiation form you should be able to use a tool such as pgAdmin to see the content input in the form stored in the database.

## Known limitations

For better performance it would be better to use connection pooling. This can be achieved by configuring a connection pool in the application server used for Bonita web application deployment in conjunction with Bonita data source database connector (instead of PostgreSQL connector).



