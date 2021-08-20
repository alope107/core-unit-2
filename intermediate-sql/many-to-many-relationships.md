## Many To Many Relationships

As covered in the Database Relationships lesson, building a many to many relationship requires a table called a **join** table. Consider a database in which a book can be classified in many genres, and each genre will have many books in it.  To create a relationship like this:


![Many to Many ERD diagram between books and genres](../assets/intermediate-sql_establishing-relationships_many-to-many.svg)
*Fig. Many to many relationship between books and genres*

We create a join table with the following SQL:

```sql
CREATE TABLE books_genres (
  book_id INT,
  FOREIGN KEY (book_id) REFERENCES books(id),
  genre_id INT,
  FOREIGN KEY (genre_id) REFERENCES genres(id),
  PRIMARY KEY (book_id, genre_id)
);
```

This SQL creates a join table connecting the `books` and `genres` tables.  This table is different from prior tables in a few ways.

1.  The table lacks an `id` field as the primary key.
1.  The table **only** has two foreign key fields.
1.  The `books_genres` table uses a combination of **two** columns as a primary key.
    *  This means that no two rows can exist with identical `book_id` and `genre_id` values.
1.  We *choose* to name the join table `books_genres` a combination of the two parent table names. SQL does not require any particular name. As a convention at Ada, when naming a join table, we will combine the parent table names alphabetically.

### Two Column Primary Keys

We could have created the `books_genres` table with an `id` primary key, but using a two-field primary key has an advantage. Using the combination of `book_id` and `genre_id` prevents duplicate entries. In this case no book can be listed in the same genre twice. A book can be in multiple different genres, and a genre can have multiple different books. But only one entry can link an individual book to a particular genre.

If our `books_genres` table contained these entries:

| book_id | genre_id |
|--- |--- |
| 1  | 1  |
| 2  | 1  |

The following SQL code would generate the subsequent error.

```sql
INSERT INTO books_genres (book_id, genre_id)
VALUES(1, 1);
ERROR:  duplicate key value violates unique constraint "books_genres_pkey"
DETAIL:  Key (book_id, genre_id)=(1, 1) already exists.
```