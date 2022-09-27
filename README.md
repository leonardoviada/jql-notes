# Introduzione

## Obiettivi

1. Prendere confidenza con la basi di dati
2. Comprenderne i vantaggi e i potenziali casi d'uso in JEBO
3. Capire come interagire con le entità ed estrarre insight dai dati in modo efficace

### Tipi di dato ed entità nell'audit in JEBO

Dovremo darci una risposta durante le chiacchierate

### OK, ma un DB che cos'è? Perchè non mi faccio un Excel e mi tolgo il problema?

I DB presentano tipologie di persistenza e accesso al dato più efficienti e strutturate, che portano - a seconda del caso d'uso - grandi vantaggi e agevolazione nello sviluppo e la consultazione delle informazioni.

### Quali tipologie di DB esistono? (Storia e cenni nerd)



#### Gerarchici

| **Struttura** | Albero 🌳                    |
| ------------- | ---------------------------- |
| **Esempi**    | IBM DL/1 (Con IMD come DBMS) |

#### Reticolari

| **Struttura** | Grafo ❇️ |
| ------------- | -------- |
| **Esempi**    | Supra    |

#### Relazionali

| **Struttura** | Vedi [ERD](https://cdn-images.visual-paradigm.com/guide/data-modeling/what-is-erd/01-entity-relationship-diagram.png) |
| ------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Esempi**    | DB2, OracleDB, MySQL (MariaDB), SQLServer, PostgreSQL, SQLite - embedded                                              |

_Caratteristiche:_ Utilizzano SQL, le colonne hanno dei [tipi](https://www.w3schools.com/sql/sql\_datatypes.asp)

_Esempio Dati_

<pre><code><strong>id, nome
</strong>1, verdi
2, bianchi
3, rossi</code></pre>

#### Ad Oggetti

| **Struttura** | Può presentarne una qualsiasi delle precedenti |
| ------------- | ---------------------------------------------- |
| **Esempi**    | ZODB                                           |

_Caratteristiche_ Non utilizzano i record, ma gli ogetti, che vengono convertiti grazie ad un [ORM](https://it.wikipedia.org/wiki/Object-relational\_mapping)

#### Documentali

| **Struttura** | Record a composizione variabile |
| ------------- | ------------------------------- |
| **Esempi**    | MongoDB                         |

_Caratteristiche_ Ogni record può presentare campi non in comune con gli altri

_Esempio Dati_

```
  /* doc1 */
  "Viada": {
    "v1": 3
    "v2": 4
    "v3": 2
    "v4": 6
  }

  /* doc2 */
  "Rossi": {
    "v1": 10
    "v2": 6
    "media": 8
  }
```

### Finalmente, SQL

E' standardizzato dalla [ISO/IEC 9075-1:2016](https://www.iso.org/standard/63555.html) (da sapere assolutamente a memoria)

{% code overflow="wrap" lineNumbers="true" %}
```sql
SELECT * from Customers;
```
{% endcode %}

#### Comandi

* Query: `SELECT`
* non-Query: `CREATE`, `UPDATE`, `DROP`, `INSERT`, `ALTER`

#### Tipi Colonne

* Primitivi: `INTEGER/NUMERIC`, `REAL`, `VARCHAR`
* non-Primitivi: `BLOB`, `TIMESTAMP`, `IMAGE`, `DATE`

#### Alcuni comandi eseguiti in chiamata

```sql
SELECT Customers.customer_id, Customers.first_name, last_name, SUM(Orders.amount) as "Totale fatturato"
FROM Customers
JOIN Orders ON Orders.customer_id
WHERE Orders.customer_id=1
```

```sql
SELECT customer_id FROM Customers;
SELECT customer_id,age FROM Customers;
SELECT customer_id as "qualcosaltro",age FROM Customers;
SELECT customer_id as "qualcosaltro",age*2 FROM Customers;
SELECT DISTINCT customer_id as "qualcosaltro",age*2 FROM Customers;
SELECT DISTINCT status FROM Shippings;
SELECT amount from Orders WHERE customer_id=1;
SELECT SUM(amount) from Orders WHERE customer_id=1;
SELECT SUM(amount)/100 from Orders WHERE customer_id=1;
```

```sql
CREATE Table Client (
	client_id int
);
```

#### Ma quanti ne esistono?

{% embed url="https://towardsdatascience.com/the-many-flavours-of-sql-7b7da5d56c1e" %}
Approfondimento sui diversi "gusti" di SQL associati ai principali DB presenti
{% endembed %}

Data la presenza di diverse tipologie di DB all'interno della famiglia dei relazionali, per evitare problemi durante la migrazione dei dati (da un DB ad un altro) conviene **evitare** di utilizzare tipi di dato non primitivi.\
Questo comporta la scrittura di frammenti di codice per il parsing delle colonne del DB che contengono dati non primitivi.\
**E.G.** Per salvare un Array possiamo utilizzare una stringa con un apposito protocollo definito per il parsing - `"item1##item2##item3"`, avremo bisogno quindi una funzione che ci consenta di riportare nel formato desiderato i dati - `mioArray = fieldArray.split('##')`

#### Parallelo con Mongo

{% embed url="https://www.mongodb.com/docs/manual/tutorial/query-documents/" %}

### Addendum

#### Esempio di comunicazione tra client e server nel contesto DB (C)

```c
char* risultato = mySQL_execute_query("SELECT pagamenti
FROM Studenti
WHERE nome_studente = 'Viada'");
printf("soldi ricevuti: %s\n", risultato);

int rc = mySQL_non_query(
"INSERT INTO Studenti
VALUES (3, "Viada", 10000);");
if (rc == 0) printf("tutto bene!"); else return -1;
```

#### Operazioni CRUD

{% embed url="https://en.wikipedia.org/wiki/Create,_read,_update_and_delete" %}

#### Link utili

{% embed url="https://www.programiz.com/sql/online-compiler/" %}
Editor immediato per i primi esperimenti
{% endembed %}

{% embed url="https://towardsdatascience.com/data-lakes-and-sql-49084512dd70" %}
Data lakes e SQL
{% endembed %}

{% embed url="https://www.w3schools.com/SQl/exercise.asp" %}
Esercizi semplici e guidati
{% endembed %}

{% embed url="https://curc.readthedocs.io/en/latest/programming/coding-best-practices.html" %}

#### Lavagna

{% file src=".gitbook/assets/JQL_20220927.pdf" %}

#### Per la prossima volta

Installare sqlite, vedi [SQLite - Installation](https://www.tutorialspoint.com/sqlite/sqlite\_installation.htm)

