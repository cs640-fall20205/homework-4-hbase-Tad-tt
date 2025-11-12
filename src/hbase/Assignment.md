# Assignment 4 - Columnar Databases and HBase

## 7in7 - HBase - Day 1

### Part 1 - Interactive Reading

Read Day 1 and work through the examples in the chapter.
Save your final database in a directory `day1` as follows.

```
./save.bash wiki day1
```

You can use the same script to save your database between work sessions.
But you cannot save over an existing save. So use temporary names like:
`day1a`, `day1b`, etc. You can use the `./load.bash` script to restore
an empty database from a saved state.

Here are some additional tips:

* Skip **Configuring HBase**.
* Begin with the last command on p57: `version`.
* p59 - Do not run these in the shell, which is Ruby.
    These are illustrative examples in python.
* p64 - The last example is split over 2 lines. `hbase>` and `hbase*` are
    not part of the command.
* I have not been able to use the colons inside of quotes in the hbase commands. If they don't work with the colon, try them without.
* The command to allow unlimited  versions is:

`alter 'wiki', {NAME => 'text', VERSIONS => 2147483647 }`

### Part 2 - 7in7 - HBase Day 1 - Find

1. Figure our how to use the shell to do the following:

    * Delete individual column values in a row:

        ```
        delete 'wiki', 'rev1', 'text:comment'

        ```

    * Delete an entire row

        ```
        deleteall 'wiki', 'rev1'

        ```

`delete` removes single cell or column,
`deleteall` removes entire row or cell.


2. Bookmark the HBase API documentation for the version of HBase you’re using.

    ```
    https://hbase.apache.org/1.2/apidocs/

    ```

### Part 3 - Create a family database

#### Step 1 - Create an hbase table that represents a family.

Specifically, you should have column families for the following:
* personal information: names of family members and birthdays
* favorites: foods and vacation locations
* location information: addresses including street, city, state, and zip. and phone numbers

Place the Hbase code to create the families below:

```
create 'family',
  { NAME => 'personal',  VERSIONS => 1 },
  { NAME => 'favorites', VERSIONS => 1 },
  { NAME => 'location',  VERSIONS => 1 }

```

#### Step 2 - Load five rows of data.
Make sure to have at least one row with more than one favorite food and at least one row with more than one favorite vacation location. Place the Hbase code below:
```
put 'family','susan','personal:name','Susan Johnson'
put 'family','susan','personal:birthday','1999-04-12'
put 'family','susan','favorites:food1','pizza'
put 'family','susan','favorites:food2','Ramen'
put 'family','susan','favorites:vac1','Paris'
put 'family','susan','location:street','45 Birchwood Ln'
put 'family','susan','location:city','Springfield'
put 'family','susan','location:city','Springfield'
put 'family','susan','location:state','MA'
put 'family','susan','location:zip','01109'
put 'family','susan','location:phone','413-555-2134'

put 'family','dexter','personal:name','Dexter Johnson'
put 'family','dexter','personal:birthday','1996-12-01'
put 'family','dexter','favorites:food1','Burger'
put 'family','dexter','favorites:vac1','Japan'
put 'family','dexter','location:street','228 Elm Terrace'
put 'family','dexter','location:city','Springfield'
put 'family','dexter','location:state','MA'
put 'family','dexter','location:zip','01105'
put 'family','dexter','location:phone','413-556-7134'

put 'family','carol','personal:name','Carol Johnson'
put 'family','carol','personal:birthday','2001-02-20'
put 'family','carol','favorites:food1','Seafood'
put 'family','carol','favorites:vac1','Hawaii'
put 'family','carol','favorites:vac2','New York'
put 'family','carol','location:street','18 Brookfield Ave'
put 'family','carol','location:city','Springfield'
put 'family','carol','location:state','MA'
put 'family','carol','location:zip','01103'
put 'family','carol','location:phone','416-568-3055'

put 'family','eva','personal:name','Eva Johnson'
put 'family','eva','personal:birthday','2003-11-03'
put 'family','eva','favorites:food1','Steak'
put 'family','eva','favorites:food2','pizza'
put 'family','eva','favorites:vac1','Spain'
put 'family','eva','location:street','72 Pine Hollow Rd'
put 'family','eva','location:city','Springfield'
put 'family','eva','location:state','MA'
put 'family','eva','location:zip','01108'
put 'family','eva','location:phone','433-785-5778'

put 'family','dave','personal:name','Dave Johnson'
put 'family','dave','personal:birthday','1994-06-13'
put 'family','dave','favorites:food1','BBQ'
put 'family','dave','favorites:vac1','Bangkok'
put 'family','dave','location:street','59 Oakwood Dr'
put 'family','dave','location:city','Springfield'
put 'family','dave','location:state','MA'
put 'family','dave','location:zip','01107'
put 'family','dave','location:phone','422-666-4990'

```

#### Step 3 – Create HBase queries for the items below.

Place the Hbase code **and the results** after each query.

**Query 1:** Get complete information for a specific family member.
```
get 'family','susan'

COLUMN                                        CELL
 favorites:food1                              timestamp=1762906097258, value=pizza
 favorites:food2                              timestamp=1762906116990, value=Ramen
 favorites:vac1                               timestamp=1762906135020, value=Paris
 location:city                                timestamp=1762906288640, value=Springfield
 location:phone                               timestamp=1762906306180, value=413-555-2134
 location:state                               timestamp=1762906295509, value=MA
 location:street                              timestamp=1762906266971, value=45 Birchwood Ln
 location:zip                                 timestamp=1762906299862, value=01109
 personal:birthday                            timestamp=1762906087177, value=1999-04-12
 personal:name                                timestamp=1762906074572, value=Susan Johnson
10 row(s) in 0.2320 seconds

```

**Query 2:** View only personal information for all family members.
```
scan 'family', { COLUMNS => 'personal' }

ROW                                           COLUMN+CELL
 carol                                        column=personal:birthday, timestamp=1762906491210, value=2001-02-20
 carol                                        column=personal:name, timestamp=1762906482565, value=Carol Johnson
 dave                                         column=personal:birthday, timestamp=1762906782890, value=1994-06-13
 dave                                         column=personal:name, timestamp=1762906763073, value=Dave Johnson
 dexter                                       column=personal:birthday, timestamp=1762906326016, value=1996-12-01
 dexter                                       column=personal:name, timestamp=1762906311896, value=Dexter Johnson
 eva                                          column=personal:birthday, timestamp=1762906672314, value=2003-11-03
 eva                                          column=personal:name, timestamp=1762906656370, value=Eva Johnson
 susan                                        column=personal:birthday, timestamp=1762906087177, value=1999-04-12
 susan                                        column=personal:name, timestamp=1762906074572, value=Susan Johnson
5 row(s) in 0.0420 seconds

```

**Query 3:** Get the name, favorite foods, and vacation locations for one family member.
```
get 'family', 'dexter', { COLUMNS => ['personal:name', 'favorites:food1', 'favorites:vac1'] }

COLUMN                                        CELL
 favorites:food1                              timestamp=1762906357540, value=Burger
 favorites:vac1                               timestamp=1762906380721, value=Japan
 personal:name                                timestamp=1762906311896, value=Dexter Johnson
3 row(s) in 0.2480 seconds

```

**Query 4:** Get a range of at least two family members.
```
scan 'family', { STARTROW => 'carol', STOPROW => 'eva' }

ROW                                           COLUMN+CELL
 carol                                        column=favorites:food1, timestamp=1762906505781, value=Seafood
 carol                                        column=favorites:vac1, timestamp=1762906529561, value=Hawaii
 carol                                        column=favorites:vac2, timestamp=1762906537972, value=New York
 carol                                        column=location:city, timestamp=1762906584679, value=Springfield
 carol                                        column=location:phone, timestamp=1762906612451, value=416-568-3055
 carol                                        column=location:state, timestamp=1762906589722, value=MA
 carol                                        column=location:street, timestamp=1762906548322, value=18 Brookfield Ave
 carol                                        column=location:zip, timestamp=1762906595145, value=01103
 carol                                        column=personal:birthday, timestamp=1762906491210, value=2001-02-20
 carol                                        column=personal:name, timestamp=1762906482565, value=Carol Johnson
 dave                                         column=favorites:food1, timestamp=1762906796165, value=BBQ
 dave                                         column=favorites:vac1, timestamp=1762906819165, value=Bangkok
 dave                                         column=location:city, timestamp=1762906829424, value=Springfield
 dave                                         column=location:phone, timestamp=1762906864944, value=422-666-4990
 dave                                         column=location:state, timestamp=1762906833637, value=MA
 dave                                         column=location:street, timestamp=1762906825183, value=59 Oakwood Dr
 dave                                         column=location:zip, timestamp=1762906838370, value=01107
 dave                                         column=personal:birthday, timestamp=1762906782890, value=1994-06-13
 dave                                         column=personal:name, timestamp=1762906763073, value=Dave Johnson
 dexter                                       column=favorites:food1, timestamp=1762906357540, value=Burger
 dexter                                       column=favorites:vac1, timestamp=1762906380721, value=Japan
 dexter                                       column=location:city, timestamp=1762906425775, value=Springfield
 dexter                                       column=location:phone, timestamp=1762906463252, value=413-556-7134
 dexter                                       column=location:state, timestamp=1762906432643, value=MA
 dexter                                       column=location:street, timestamp=1762906417100, value=228 Elm Terrace
 dexter                                       column=location:zip, timestamp=1762906439174, value=01105
 dexter                                       column=personal:birthday, timestamp=1762906326016, value=1996-12-01
 dexter                                       column=personal:name, timestamp=1762906311896, value=Dexter Johnson
3 row(s) in 0.0950 seconds

```

**Query 5:** Get the addresses for all family members.
```
scan 'family', { COLUMNS => ['location:street','location:city','location:state','location:zip'] }

ROW                                           COLUMN+CELL
 carol                                        column=location:city, timestamp=1762906584679, value=Springfield
 carol                                        column=location:state, timestamp=1762906589722, value=MA
 carol                                        column=location:street, timestamp=1762906548322, value=18 Brookfield Ave
 carol                                        column=location:zip, timestamp=1762906595145, value=01103
 dave                                         column=location:city, timestamp=1762906829424, value=Springfield
 dave                                         column=location:state, timestamp=1762906833637, value=MA
 dave                                         column=location:street, timestamp=1762906825183, value=59 Oakwood Dr
 dave                                         column=location:zip, timestamp=1762906838370, value=01107
 dexter                                       column=location:city, timestamp=1762906425775, value=Springfield
 dexter                                       column=location:state, timestamp=1762906432643, value=MA
 dexter                                       column=location:street, timestamp=1762906417100, value=228 Elm Terrace
 dexter                                       column=location:zip, timestamp=1762906439174, value=01105
 eva                                          column=location:city, timestamp=1762906723705, value=Springfield
 eva                                          column=location:state, timestamp=1762906728780, value=MA
 eva                                          column=location:street, timestamp=1762906717940, value=72 Pine Hollow Rd
 eva                                          column=location:zip, timestamp=1762906734407, value=01108
 susan                                        column=location:city, timestamp=1762906288640, value=Springfield
 susan                                        column=location:state, timestamp=1762906295509, value=MA
 susan                                        column=location:street, timestamp=1762906266971, value=45 Birchwood Ln
 susan                                        column=location:zip, timestamp=1762906299862, value=01109
5 row(s) in 0.0940 seconds

```

**Query 6:** Get the names of family members who like a specific favorite food (e.g., pizza).
```
scan 'family', {  COLUMNS => ['personal:name','favorites:food1','favorites:food2'],  FILTER  => "ValueFilter(=,'substring:pizza')"}

ROW                                           COLUMN+CELL
 eva                                          column=favorites:food2, timestamp=1762906696022, value=pizza
 susan                                        column=favorites:food1, timestamp=1762906097258, value=pizza
2 row(s) in 0.0510 seconds

```

**Query 7:** Create a vacation preference list with names.
```
scan 'family', { COLUMNS => ['personal:name','favorites:vac1','favorites:vac2'] }

ROW                                           COLUMN+CELL
 carol                                        column=favorites:vac1, timestamp=1762906529561, value=Hawaii
 carol                                        column=favorites:vac2, timestamp=1762906537972, value=New York
 carol                                        column=personal:name, timestamp=1762906482565, value=Carol Johnson
 dave                                         column=favorites:vac1, timestamp=1762906819165, value=Bangkok
 dave                                         column=personal:name, timestamp=1762906763073, value=Dave Johnson
 dexter                                       column=favorites:vac1, timestamp=1762906380721, value=Japan
 dexter                                       column=personal:name, timestamp=1762906311896, value=Dexter Johnson
 eva                                          column=favorites:vac1, timestamp=1762906709275, value=Spain
 eva                                          column=personal:name, timestamp=1762906656370, value=Eva Johnson
 susan                                        column=favorites:vac1, timestamp=1762906135020, value=Paris
 susan                                        column=personal:name, timestamp=1762906074572, value=Susan Johnson
5 row(s) in 0.0300 seconds

```

### Part 4 - 7in7 - Day 3 - Wrap Up

Read Day 3 - Wrap Up. Then answer the following.

1. List the pros of HBase as described in our text.

    ```
    Can handle large aount of data and good scalibility.

    System provide built in versioning and compressing which is good for keeping data history.

    Rich resources

    ```

2. List the cons of HBase as described in our text.

    ```
    More challenging to manage data compared to other relational databases.

    Wordings such as like “colum” and “table” might be confusing at first time using.

    strict setup for better performance

    ```


### 7in7 - Day 2 - OPTIONAL

OPTIONAL - You may safely skip Day 2.

This section contains a code-heavy example of loading a large amount
of data into your HBase database. If you feel like a challenge and are
interested, feel free to work through it.

If you do this section, please **do not** use ./save.bash to save
your database, because it may become very large.

So if you are ready for the challenge, below are some instructions to
help you along. Good luck!

1. Download and extract the source code for the text: https://pragprog.com/titles/pwrdata/seven-databases-in-seven-weeks-second-edition/

2. Drag the following files into 02_hbase/local/scripts on GitPod.
    * source_code/02_hbase/import_from_wikipedia.rb
    * source_code/02_hbase/create_wiki_schema.rb

3. Start your database.

4. Run the following to create the wiki table.

    ```
    ./shell.bash create_wiki_schema.rb
    ```

5. Now you should be able to run the command below.
    BEFORE YOU DO... be ready to press CTRL+C to stop the process. This
    command will load a lot of data very fast.

    ```
    curl https://dumps.wikimedia.org/enwiki/latest/enwiki-latest-pages-articles.xml.bz2 | bzcat | ./shell.bash import_from_wikipedia.rb
    ```

6. After it appears to be working, press CTRL+C to stop it.

7. Connect to your database.

8. Run the command below to count the number of
    rows in your 'wiki' table.

    ```
    count 'wiki'
    ```

    Copy and paste the output of this command below.

9. Run the command below to get information about your database's regions.

    ```
    scan 'hbase:meta',{FILTER=>"PrefixFilter('wiki')"}
    ```

    Copy and past the output of this command below.
