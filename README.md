#  Logs Analysis Project

 **[Full Stack Web Developer Nanodegree's first project](#)** 

This is a task which aims to create a reporting tool that prints out reports (in plain text) based on the data in the database. This reporting tool is a Python program using the psycopg2 module to connect to the database.

### Project Requirements
* Reporting tool should answer the following questions:
           1) What are the most popular three articles of all time?
           2) Who are the most popular article authors of all time?
           3) On which days did more than 1% of requests lead to errors?

* Project follows good SQL coding practices: Each question should be answered
 with a single database query.
* code should be written with good Python style. The [PEP8 style guide ](https://www.python.org/dev/peps/pep-0008/) is an excellent standard to follow. 



### Installation

To start on this project, you'll need database software [(provided by a Linux virtual machine)](#) 
and the data to analyze.

*Here the programs that shoud be installed in your pc :*

* **News database** schema [click here](https://d17h27t6h515a5.cloudfront.net/topher/2016/August/57b5f748_newsdata/newsdata.zip) to download .

* **Virtual Box** to manage virtual machines [click here ](https://www.virtualbox.org/wiki/Downloads) to download .

* **Vagrant** development environment [click here](https://www.vagrantup.com/) to download .

* **Python** Programming Language version 3 [click here](https://www.python.org/) to download .

* **PostgreSQL** Database [click here](https://www.postgresql.org/) to download .


### Quick Start

After downloading the **news database file** extract it, you will find **_newsdata.sql_** file 
add it to your project directory.
* Open your shell terminal
* run `cd Downloads/FSND-Virtual-Machine/Vagrant/`to change dierctory to vagrant .
* run `vagrant up` to start the virtual machine .
* then `vagrant ssh` to login to virtual machine .
* run `cd /vagrant`.
* Then go to the project directory by typing `cd loganalysis/` in your terminal .

Now time to use the **PostgreSQL Database** .
* Type `psql -d news -f newsdata.sql` into your terminal to prepare the _database_.

 Implementation of queries **Log Analysis Program** .
* Type `python3 log_analysis.py` in your terminal to run the program.

this will give you the **OUTPUT.txt** file modified with the output.


### Output

       
        ** 1) The most popular 3 articles of all time ** 

With Title: 'Candidate is jerk, alleges rival' --->  338647  Views

With Title: 'Bears love berries, alleges bear' --->  253801  Views 

With Title: 'Bad things gone, say good people' --->  170098  Views

       ** 2) The most popular articles authors of all time ** 
       
Author Name: Ursula La Multa ---> 507594 Views 
 
Author Name: Rudolf von Treppenwitz ---> 423457 Views 
 
Author Name: Anonymous Contributor ---> 170098 Views 
 
Author Name: Markoff Chaney ---> 84557 Views

       ** 3) Days in which  more than 1% requests lead to errors **

  Day : July 17, 2016' --->  2.3 % 



### Queries Used

*  Most popular three articles of all time ?
```sql
select articles.title, count(log.ip) as views
from log join articles
on log.path = concat('/article/', articles.slug)
group by articles.title
order by views desc limit 3;
```


* Most popular article authors of all time ?
```sql
select authors.name, count(log.ip) as views
from log, authors, articles
where articles.author = authors.id
and CONCAT('/article/',articles.slug) = log.path
group by authors.name order by views desc limit 3;
```

* On which days did more than 1% of requests lead to error ?
```sql
select to_char(date, 'FMMonth FMDD, YYYY'),
        (err/total) * 100 as ratio
from (select time::date as date, count(*) as total,
        sum((status != '200 OK')::int)::float as err
from log group by date) as errors
where err/total > 0.01;
```


