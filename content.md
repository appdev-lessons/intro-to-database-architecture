# Intro to Database Architecture

## The first writers

In 2012, I gave [a talk at TEDxUChicago](https://www.youtube.com/watch?v=rtl9QG4qe6g), in which I discussed the following idea:

When the printing press was invented, the foundation of the world was the written word.

![](assets/scribe-1.png)

![](assets/printing-press.png)

The foundation of the world _today_ is software.

I want to extend the analogy a bit more...

### What were they writing?

Back in the early times, what were people writing? On this tablet, is the _very first_ name in recorded history (the author's signature):

![](assets/first-tablet.png)

Do you know what is written here? Try to guess before you click.

<button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#firstWrittenName">
  Click for answer
</button>

<div class="modal fade" id="firstWrittenName" tabindex="-1" aria-labelledby="firstWrittenNameLabel" aria-hidden="true">
  <div class="modal-dialog">
    "29,086 measures barley; 37 months — Kushim"
  </div>
</div>

---

<div style="height: 250px;"></div>

### What were they printing?

> "It is telling that the first recorded name in history belongs to an accountant, rather than a prophet, a poet, or a great conqueror."
> — Yuval Noah Harari, Sapiens: A Brief History of Humankind

After the bible, what was the best-selling book in the 17th century? Try to guess before you click.

<button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#earlyBestSeller">
  Click for answer
</button>

<div class="modal fade" id="earlyBestSeller" tabindex="-1" aria-labelledby="earlyBestSellerLabel" aria-hidden="true">
  <div class="modal-dialog">
    Almanacs!
  </div>
</div>

---

<div style="height: 250px;"></div>

An almanac is an annual publication that includes information such as weather forecasts, farmers' planting dates, tide tables, and tabular information often arranged according to the calendar.

### My Claim

The killer application of writing, and printing, was _recordkeeping_.

The killer application of computing _is also_ recordkeeping.

The heart of the ultra-valuable cloud-based apps that have "eaten the world" is the information they are keeping track of. Users, tweets, and who-follows-who in Twitter. Listings, bookings, and messages in Airbnb. Venues, reviews, and ratings in Yelp. Etc.

We keep track of this information as records in plain old tables — just like they did in almanacs in the 17th century. One of the most important parts of developing an app to solve a problem is figuring out _what information it needs to keep track of_. And then, designing a set of tables to organize that information effectively. This is part of the process known as _database architecture_.

Database architecture is part of a larger process known as _domain modeling_, wherein a developer becomes a mini-domain-expert in the area they're building software for.

Domain modeling involves conducting interviews, reading reference manuals and textbooks, discovering existing processes, gathering sample data, making mockups, and other research.

Only after you've done this research can you begin to implement a solution. We'll focus on the database architecture part of domain modeling, for now.

## Request/Response Cycle

![](assets/request-response-cycle.png)
{: .bleed-full }

It is, of course, important to provide a usable interface; but the interface by itself is not useful without the information.

<aside>
For most cloud-based software. If you're writing a non-cloud based game, then the interface itself may indeed be the valuable part.
</aside>

The core value of cloud-based apps is the ever-changing information it keeps track of.

airbedandbreakfast.com, circa 2009:

![old airbnb](assets/old-airbnb.png)

We've only scratched the surface of HTML and CSS, but it's enough for our purposes for now.

Today we're going to start learning how to store and retrieve users' information — ultimately, the value of most cloud-based apps.

## Databases

A "database" is a piece of software that stores and retrieves information. We'll be using a type of database known as a _relational database_, or RDBMS (relational database management system).

Relational databases are the workhorses of computing. There are other kinds of databases for specialized purposes (graph databases, vector databases, etc); but relational databases drive the main functionality of most cloud-based software.

Don't let the fancy name throw you — relational databases are a set of tables — like an almanac. Each table has headings and entries, or columns and rows; and we perform lookups to find information that we want. I don't want you to think about databases as software at all. Try to think of them as just paper, like almanacs or clipboards.

We have to be able to describe the routine we would go through to do these lookups manually, as humans, if we had the tables printed out on paper. Computers are faster at doing them than us, but computers can only do the same, simple operations that we can — filtering based on criteria, sorting, counting, and cross-referencing rows in tables.

## One strategy for database architecture

**We** (the developers) figure out the main _things_, or _nouns_, in our problem space while domain modeling. These things are candidates to be **tables**.

For each thing, we figure out which _attributes_ we need to keep track of. These attributes are candidates to be columns in that table.

We (the developers) create the tables and columns we identified.
