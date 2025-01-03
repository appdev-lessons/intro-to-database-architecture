# Intro to Database Architecture: Must See Movies

## Slides

Here are [the slides I will be going through today](https://firstdraft.slides.com/d/236NZyU/live).

[Here is a link to the non-live version of the slides for you to look through.](https://firstdraft.slides.com/raghubetina/appdev-03-intro-to-database-architecture?token=9RBQRn_H)

Below are written notes which you should also review.

## Databases

A "database" is a piece of software that stores and retrieves information. We'll be using a type of database known as a _relational database_, or RDBMS (relational database management system).

Relational databases are the workhorses of computing. There are other kinds of databases for specialized purposes (graph databases, vector databases, etc); but relational databases drive the main functionality of most cloud-based software.

Don't let the fancy name throw you — relational databases are nothing more than a set of tables, like you'd find in an almanac. Each table has headings and entries, or columns and rows; and we perform lookups to find information that we're interested in. I don't want you to think about databases as software at all. Try to think of them as just paper, like almanacs or clipboards.

In order to write our apps, we first have to be able to describe the manual routine we would go through to do these lookups, as humans, if we had the tables printed out on paper. Computers are faster at doing them than us, but computers can only do the same, simple operations that we can — filtering based on criteria, sorting, counting, and cross-referencing rows in tables.

## One strategy for database architecture

There are many approaches that database designers use to come up with a database design (or "schema") for a given problem space. Here is a common one:

- We figure out the main _things_, or _nouns_, in our problem space. These things are candidates to be **tables**.
- For each thing, we figure out which _attributes_ we need to keep track of. These attributes are candidates to be columns in that table.
- We draw out the tables and columns we identified. Then we simulate user actions and make sure that our design can support them all.

In a relational database, all user actions translate to creating, reading, updating, or deleting **rows** in the tables that the developers have created.

We say "create, read, update, or delete" so often — the fundamental 4 operations that all user actions map to — that, in the industry, we abbreviate it to **CRUD**.

## Database design constraints

### Constraint one

Users actions can trigger CRUDing **rows** within existing tables, but **cannot trigger adding new tables or columns**. 

We, the developers, will create all tables and columns up front, when we design and deploy the application.

User actions can CRUD a million **rows** per second. But they can't add _any_ tables or columns.

### Constraint two

**We can only store one value per cell.**

It's tempting sometimes to want to store multiple values within one cell, perhaps separated by commas or slashes — but it makes filtering hard.

The value in one cell can be a very long piece of text, like a bio; but it can't be _multiple_ bios for different people.

- Choose all that are true:
- A relational database is a prerequisite for most CRUD apps.
  - Yes!
- We can think of a database much like a set of tables in a spreadsheet software.
  - Yes! This is a helpful way to think about it.
- The "things" or "nouns" in our problem space are candidates for tables; and their attributes are candidates for columns.
  - Yes!
- CRUD stands for **C**reate, **R**ead, **U**pdate, and **D**elete
  - Yes!
- Users are allowed to add columns in our database.
  - Not quite. Re-read the constraints.
- We can store more than one value per cell in our database tables. 
  - Not quite. Re-read the constraints.
{: .choose_all #database_quiz title="Database quiz" points="4" answer="[1,2,3,4]" }

## Must See Movies

Okay, this is all pretty abstract. Let's look at some examples.

Lets examine [this application](https://msm-associations.matchthetarget.com/), which is one that we'll build in class. It's a very simplified version of the iMDB Top 250 movies list (this is where we scraped the data from).

Click around it for a minute and then try to imagine — what are the tables and columns powering this application? How many tables are required? What columns are in each table? I'll show you a solution in a moment, but take a guess.

Remember:

- We figure out the main _things_, or _nouns_, in our problem space. These things are candidates to be **tables**.
- For each thing, we figure out which _attributes_ we need to keep track of. These attributes are candidates to be columns in that table.
- We draw out the tables and columns we identified. Then we simulate user actions and make sure that our design can support them all.

### One big table: Movies

"Movie" seems like an important noun in this app's problem domain. Let's make a table to keep track of movies. To guide us in identifying the attributes that are being stored, let's look at [a movie's details page](https://msm-associations.matchthetarget.com/movies/24).

![](assets/movies-table-1.png)

We add columns for all of the attributes of a movie (title, duration, description, etc — there's not enough room on the slide for them all but pretend they are there).

The "id" column is automatically added to every table, and the db assigns a unique value for each record. We don't get to pick the name of the column or assign values.

![](assets/movies-table-2.png)

Now imagine that we start adding columns for the director info that we see on the movie details page, like the director's name. But there is also more information about each director [on their own details page](https://msm-associations.matchthetarget.com/directors/2662) — where should we store that information?

We could continue storing more and more director attributes (like bio or dob) within movies, but we will start to get a lot of redundancy But, it works and doesn't violate either of our two db design constraints.

![](assets/movies-table-3.png)

Now imagine that we start adding columns to keep track of the actors who star in a movie. We run into a problem: we've violated Constraint Two — we can't store more than one value in a single cell.

![](assets/movies-table-4.png)

One reason for not adding multiple values in a single cell — when the values are long there will be a lot of redundancy. More importantly, filtering rows by a criteria is a lot easier when you don't have to look through a list.

### More than one table

So: if we only have one table, we'll have to break Constraint Two. Let's try multiple tables instead. What are other important nouns in the problem domain that might deserve their own table?

Wherever we observed redundancy, or if we were tempted to store more than 1 value in a cell, that's a candidate for an entity that requires its own table.

#### One-to-many relationships

As we build it out we will find that we need some **one-to-many relationships** between tables. This will make use of _primary keys_ and _foreign keys_ to associate rows to one another.

![](assets/one-to-many-1.png)

#### Many-to-many relationships

And we will also find that we need some **many-to-many relationships** between tables. This will require us to put two primary keys in a new _join table_.

![](assets/many-to-many-1.png)

And we may will also find that there are real names to describe the relationships in such many-to-many join tables!

![](assets/many-to-many-2.png)

<div class="alert alert-primary">

### Must See Movies Solution

The full database architecture solution to [Must See Movies](https://msm-associations.matchthetarget.com/) can be found below. Try not to peek at the solution until you've tried to write it out yourself by hand or in a spreadsheet software.

[**Solution PDF file**](https://res.cloudinary.com/dmxgp9oq2/image/upload/v1711038623/appdev-lessons/intro-to-database-architecture/main/Must%20See%20Movies%20database%20architecture%20solution.pdf)
</div>

---

[Here is a link to the non-live version of the slides.](https://firstdraft.slides.com/raghubetina/appdev-03-intro-to-database-architecture?token=9RBQRn_H)

---

- Approximately how long (in minutes) did this lesson take you to complete?
{: .free_text_number #time_taken title="Time taken" points="1" answer="any" }
