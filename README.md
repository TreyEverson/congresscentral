# congresscentral
Full stack website made in CS 331E


To run on MacOS
1. virtualenv venv
2. source venv/bin/activate

Design and RESTful API
Database Design and Implementation:
The database was first designed by creating an Entity Relationship Diagram (ERD), after which a relational schema was created in order to determine how the PostgreSQL database should be set up. The mapping algorithm was used to convert the ERD to the relational schema. Using the relational schema, the models.py and create_db.py files were created in order to create the PostgreSQL database. JSON files generated from the external APIs were used to populate the database. A PostgreSQL database was set up on GCP for our backend API to pull data from.
The following image illustrates the ERD used to design the database.

Figure 1: ERD for designing the database.
The following image illustrates the relational schema used to create the database.

Figure 2: Relational schema for creating the database.
APIs:
The Twitter, ProPublica, and OpenSecrets APIs were used to obtain information to populate our PostgreSQL database. The Python file, API_Data_Scraping.py, was used to scrape these external APIs. Once JSON files were created using API_Data_Scraping.py, they were fed into create_db.py in order to populate our database. The frontend of our website used our backend API from the main.py file to populate our dynamic HTMLs.
DB Models:
To explain the principle of 'congress' database, we set all the tables of 'congress' as below. We categorized main entities(tables): 'committee', 'member', 'legislation' and weak entities(tables): 'twitter_account', 'financial_information', 'industry_contributor', 'organ_contributor', 'sector_contributor', 'subcommittee', 'hearing', 'action', 'are_part_of', 'Is_Pushed_Through', and 'discuss'. The below tables sequentially represent the main and weak entities.
Note: VARCHAR means character varying with string data type

Main Entities
committee : 5 attributes



Attributes
Information




id
VARCHAR(10), NOT NULL, Primary Key


name
VARCHAR(80), NOT NULL


website
VARCHAR(255), NOT NULL


branch
VARCHAR(40), NOT NULL


chair_id
VARCHAR(80), NOT NULL


ranking_id
VARCHAR(80), NOT NULL



member : 13 attributes
Terminology: fname = first name, lname = last name, mname = middle name, namesuffix = suffix, vote_w_party = vote with party, vote_against_party = vote against party



Attributes
Frame Information




id
VARCHAR(20), NOT NULL, Primary Key


fname
VARCHAR(20), NOT NULL


lname
VARCHAR(20), NOT NULL


mname
VARCHAR(20), NOT NULL


namesuffix
VARCHAR(20)


party
VARCHAR(10), NOT NULL


state
VARCHAR(10), NOT NULL


state_rank
VARCHAR(10), NOT NULL


phone
VARCHAR(20)


mailing_address
VARCHAR(255)


url
VARCHAR(255), NOT NULL


votes_w_party
Integer, NOT NULL


votes_against_party
Integer, NOT NULL



legislation : 7 attributes
Terminology: sponsor_id = id in members



Attributes
Information




bill_id
VARCHAR(20), NOT NULL, Primary Key


cosponsors
Integer, NOT NULL


summary
VARCHAR(1000), NOT NULL


type
VARCHAR(10)


date_introduced
VARCHAR(20), NOT NULL


sponsor_id
VARCHAR(20), Foreign Key for member's id


bill_number
VARCHAR(20), NOT NULL




Weak Entities
twitter_account : 6 attributes



Attributes
Information




twitter_handle
VARCHAR(40), Primary Key


member_id
VARCHAR(20), Foreign Key for id in member


number_following
Integer


number_followers
Integer


number_tweets
Integer


timeline_html
VARCHAR(255)



financial_information : 2 attributes



Attributes
Information




crp_id
VARCHAR(40), Primary Key


member_id
VARCHAR(20), Foreign Key for id in member



industry_contributor : 4 attributes



Attributes
Information




industry_name
VARCHAR(100), Primary Key


crp_id
VARCHAR(20), Foreign Key for id in member, Primary Key


member_id
VARCHAR(20), Foreign Key for id in member


total
VARCHAR(20)



organ_contributor : 4 attributes
Terminology: organ_name = organization name



Attributes
Information




organ_name
VARCHAR(100), Primary Key


crp_id
VARCHAR(20), Foreign Key for id in member, Primary Key


member_id
VARCHAR(20), Foreign Key for id in member


total
VARCHAR(20)



sector_contributor : 4 attributes



Attributes
Information




sector_name
VARCHAR(100), Primary Key


crp_id
VARCHAR(20), Foreign Key for id in member, Primary Key


member_id
VARCHAR(20), Foreign Key for id in member


total
VARCHAR(20)



subcommittee : 2 attributes



Attributes
Information




name
VARCHAR(150), Primary Key


committee_id
VARCHAR(10), Foreign Key for id in committee



hearing : 5 attributes



Attributes
Information




date
VARCHAR(20), Primary Key


committee_id
VARCHAR(10), Primary Key


time
VARCHAR(20), Primary Key


location
VARCHAR(20), Primary Key


description
VARCHAR(4000)



action : 4 attributes



Attributes
Information




action_id
VARCHAR(20), Primary Key


bill_id
VARCHAR(80), Foreign Key for bill_id in legislation, Primary Key


date
VARCHAR(20)


description
VARCHAR(1500)



are_part_of : 2 attributes



Attributes
Information




committee_id
VARCHAR(10), Primary Key


member_id
VARCHAR(20), Primary Key



is_pushed_through : 2 attributes



Attributes
Information




committee_id
VARCHAR(10), Primary Key


bill_id
VARCHAR(80), Primary Key



discuss : 4 attributes



Attributes
Information




hearing_date
VARCHAR(20), Primary Key


hearing_time
VARCHAR(20), Primary Key


hearing_location
VARCHAR(20), Primary Key


bill_id
VARCHAR(80), Primary Key




The following image illustrates all of the relationships in the database.

Figure 3: The relationships of each entity in the database.
Search Capability
Search functionality was implemented on the website using a text box and submit button. Both of these features required being placed within input tags and both were contained inside one form tag. The form tag was specified to have the POST method, and its action was directed to the URL in the flask app where it would be searching from. The flask app would check if the request method was a POST request, and if so, it would filter through the database for the specified user input.
RESTful API
Our website also features a RESTful API which is documented with Postman. This allows others to use our data for their own needs. The endpoints for the API are as follows.

[GET] Get all congress member names and IDs
[GET] Get all committees and subcommittees
[GET] Get all legislation bills
[GET] Get all information on a specific member

No authentication is needed to access the API. The API can be reached at these corresponding endpoints.

www.congresscentral.me/members/json/
www.congresscentral.me/committees/json/
www.congresscentral.me/legislation/json/

www.congresscentral.me/members/<member_id>/json/

The API is documented in detail in the postman.json file within the repository. It will allow you to open the API in Postman to interact with it.
