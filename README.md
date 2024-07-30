# nosql-challenge 

## Instructions

The UK Food Standards Agency evaluates various establishments across the United Kingdom, and gives them a food hygiene rating. You've been contracted by the editors of a food magazine, Eat Safe, Love, to evaluate some of the ratings data in order to help their journalists and food critics decide where to focus future articles.
Part 1: Database and Jupyter Notebook Set Up

Use NoSQL_setup_starter.ipynb for this section of the challenge.

    Import the data provided in the establishments.json file from your Terminal. Name the database uk_food and the collection establishments. Copy the text you used to import your data from your Terminal to a markdown cell in your notebook.

    Within your notebook, import the libraries you need: PyMongo and Pretty Print (pprint). 
   
   from pymongo import MongoClient
   import pandas as pd
   from pprint import pprint
    
 # Create an instance of MongoClient
    mongo = MongoClient(port=27017)
    
Confirm that you created the database and loaded the data properly:
 # review the collections in our database
   print(db.list_collection_names())

['establishments']
       
  # assign the collection to a variable
establishments = db['establishments']      
        
        List the databases you have in MongoDB. Confirm that uk_food is listed.
        List the collection(s) in the database to ensure that establishments is there.
        Find and display one document in the establishments collection using find_one and display with pprint.

    Assign the establishments collection to a variable to prepare the collection for use.

Part 2: Update the Database

Use NoSQL_setup_starter.ipynb for this section of the challenge.

The magazine editors have some requested modifications for the database before you can perform any queries or analysis for them. Make the following changes to the establishments collection:

    An exciting new halal restaurant just opened in Greenwich, but hasn't been rated yet. The magazine has asked you to include it in your analysis. Add the following information to the database:

    {
        "BusinessName":"Penang Flavours",
        "BusinessType":"Restaurant/Cafe/Canteen",
        "BusinessTypeID":"",
        "AddressLine1":"Penang Flavours",
        "AddressLine2":"146A Plumstead Rd",
        "AddressLine3":"London",
        "AddressLine4":"",
        "PostCode":"SE18 7DY",
        "Phone":"",
        "LocalAuthorityCode":"511",
        "LocalAuthorityName":"Greenwich",
        "LocalAuthorityWebSite":"http://www.royalgreenwich.gov.uk",
        "LocalAuthorityEmailAddress":"health@royalgreenwich.gov.uk",
        "scores":{
            "Hygiene":"",
            "Structural":"",
            "ConfidenceInManagement":""
        },
        "SchemeType":"FHRS",
        "geocode":{
            "longitude":"0.08384000",
            "latitude":"51.49014200"
        },
        "RightToReply":"",
        "Distance":4623.9723280747176,
        "NewRatingPending":True
    }

    Find the BusinessTypeID for "Restaurant/Cafe/Canteen" and return only the BusinessTypeID and BusinessType fields.

    Update the new restaurant with the BusinessTypeID you found.

    The magazine is not interested in any establishments in Dover, so check how many documents contain the Dover Local Authority. Then, remove any establishments within the Dover Local Authority from the database, and check the number of documents to ensure they were deleted.

    Some of the number values are stored as strings, when they should be stored as numbers.
        Use update_many to convert latitude and longitude to decimal numbers.
        Use update_many to convert RatingValue to integer numbers.

Part 3: Exploratory Analysis

Eat Safe, Love has specific questions they want you to answer, which will help them find the locations they wish to visit and avoid.

Use NoSQL_analysis_starter.ipynb for this section of the challenge.

Some notes to be aware of while you are exploring the dataset:

    RatingValue refers to the overall rating decided by the Food Authority and ranges from 1-5. The higher the value, the better the rating.
        Note: This field also includes non-numeric values such as 'Pass', where 'Pass' means that the establishment passed their inspection but isn't given a number rating. We will coerce non-numeric values to nulls during the database setup before converting ratings to integers.
    The scores for Hygiene, Structural, and ConfidenceInManagement work in reverse. This means, the higher the value, the worse the establishment is in these areas.

Use the following questions to explore the database, and find the answers, so you can provide them to the magazine editors.

Unless otherwise stated, for each question:

    Use count_documents to display the number of documents contained in the result. 


    # Find the establishments with a hygiene score of 20
query = {'scores.Hygiene': 20}
fields = {'BusinessName': 1, 'scores.Hygiene': 1}
# Use count_documents to display the number of documents in the result
document_count = establishments.count_documents(query)

# Display the first document in the results using pprint
results = establishments.find(query, fields)
pprint(results[0])




    Display the first document in the results using pprint.  

    Convert the result to a Pandas DataFrame, print the number of rows in the DataFrame, and display the first 10 rows.
Which establishments have a hygiene score equal to 20?

    # Convert the result to a Pandas DataFrame
result_df = pd.DataFrame(results)

# Display the number of rows in the DataFrame
print("Rows in DataFrame: ", len(result_df))

# Display the first 10 rows of the DataFrame
result_df['HygieneScore'] = result_df['scores'].apply(lambda x: x['Hygiene'])
result_df.drop(columns=['scores'], inplace=True)
result_df.head(10) 

            Rows in DataFrame:  41 

    
    
    Which establishments have a hygiene score equal to 20?
	_id	BusinessName	HygieneScore
0	66a478ffebb4046f8d7cc5dc	The Chase Rest Home	20
1	66a478ffebb4046f8d7cc95d	Brenalwood	20
2	66a478ffebb4046f8d7ccc63	Melrose Hotel	20
3	66a478ffebb4046f8d7cce54	Seaford Pizza	20
4	66a478ffebb4046f8d7cce63	Golden Palace	20
5	66a478ffebb4046f8d7cd802	Ashby's Butchers	20
6	66a478ffebb4046f8d7cda22	South Sea Express Cuisine	20
7	66a478ffebb4046f8d7cef4f	Golden Palace	20
8	66a478ffebb4046f8d7cf394	The Tulip Tree	20
9	66a478ffebb4046f8d7cfba2	F & S	20


    Which establishments in London have a RatingValue greater than or equal to 4?
# Find the establishments with London as the Local Authority and has a RatingValue greater than or equal to 4.
query = {"BusinessName": {'$regex': "London"},
    "RatingValue": {'$gte': 4}}
fields = {'BusinessName': 1, 'RatingValue': 1}

# Use count_documents to display the number of documents in the result
document_count = establishments.count_documents(query)
print("Total document count:", document_count)

# Display the first document in the results using pprint
results = establishments.find(query, fields)
pprint(results[0])

    Total document count: 98
{'BusinessName': 'The London Tavern',
 'RatingValue': 5,
 '_id': ObjectId('66a478ffebb4046f8d7cb151')}  

 


    

    Hint: The London Local Authority has a longer name than "London" so you will need to use $regex as part of your search.

    What are the top 5 establishments with a RatingValue of 5, sorted by lowest hygiene score, nearest to the new restaurant added, "Penang Flavours"?
# Search within 0.01 degree on either side of the latitude and longitude.
# Find the coordinates of "Penang Flavours"
penang_flavors = establishments.find_one({"BusinessName": "Penang Flavours"}, {"geocode.latitude": 1, "geocode.longitude": 1})
degree_search = 0.01
latitude = penang_flavors["geocode"]["latitude"]
longitude = penang_flavors["geocode"]["longitude"]

# Rating value must equal 5    

    query = {
    "RatingValue": 5,
    "geocode.latitude": {"$gte": latitude - degree_search, "$lte": latitude + degree_search},
    "geocode.longitude": {"$gte": longitude - degree_search, "$lte": longitude + degree_search}
}
# Sort by hygiene score
sort = [("scores.Hygiene", 1)]

# Display top 5
limit = 5
# Print the results
results = list(establishments.find(query, {"BusinessName": 1, "RatingValue": 1, "scores.Hygiene": 1}).sort(sort).limit(limit))
for result in results:
    print(result)  

{'_id': ObjectId('66a478ffebb4046f8d7d1e35'), 'BusinessName': 'Volunteer', 'RatingValue': 5, 'scores': {'Hygiene': 0}}
{'_id': ObjectId('66a478ffebb4046f8d7d1e53'), 'BusinessName': 'Plumstead Manor Nursery', 'RatingValue': 5, 'scores': {'Hygiene': 0}}
{'_id': ObjectId('66a478ffebb4046f8d7d1e54'), 'BusinessName': 'Atlantic Fish Bar', 'RatingValue': 5, 'scores': {'Hygiene': 0}}
{'_id': ObjectId('66a478ffebb4046f8d7d1e0f'), 'BusinessName': 'Iceland', 'RatingValue': 5, 'scores': {'Hygiene': 0}}
{'_id': ObjectId('66a478ffebb4046f8d7d1e1e'), 'BusinessName': 'Howe and Co Fish and Chips - Van 17', 'RatingValue': 5, 'scores': {'Hygiene': 0}}

    

# Convert result to Pandas DataFrame
result_df = pd.DataFrame(results)
result_df['HygieneScore'] = result_df['scores'].apply(lambda x: x['Hygiene'])
result_df.drop(columns=['scores'], inplace=True)
result_df  

    _id	BusinessName	RatingValue	HygieneScore
0	66a478ffebb4046f8d7d1e35	Volunteer	5	0
1	66a478ffebb4046f8d7d1e53	Plumstead Manor Nursery	5	0
2	66a478ffebb4046f8d7d1e54	Atlantic Fish Bar	5	0
3	66a478ffebb4046f8d7d1e0f	Iceland	5	0
4	66a478ffebb4046f8d7d1e1e	Howe and Co Fish and Chips - Van 17	5	0


    Hint: You will need to compare the geocode to find the nearest locations. Search within 0.01 degree on either side of the latitude and longitude.

    How many establishments in each Local Authority area have a hygiene score of 0? Sort the results from highest to lowest, and print out the top ten local authority areas.
# Create a pipeline that:
# 1. Matches establishments with a hygiene score of 0
match_query = {"$match": {"scores.Hygiene": 0}}
# 2. Groups the matches by Local Authority
group_query = {"$group": {"_id": "$LocalAuthorityName", "count": {"$sum": 1}}}
# 3. Sorts the matches from highest to lowest
sort_values = {"$sort": {"count": -1, "_id": 1}}
# Print the number of documents in the result
pipeline = [match_query, group_query, sort_values]
results = list(establishments.aggregate(pipeline))
print("Number of documents in the result:", len(results))
# Print the first 10 results
pprint(results[0:10])   


 Hint: You will need to use the aggregation method to answer this.

    The first 5 rows of your resulting DataFrame should look something like this:  
Number of documents in the result: 55
[{'_id': 'Thanet', 'count': 1130},
 {'_id': 'Greenwich', 'count': 882},
 {'_id': 'Maidstone', 'count': 713},
 {'_id': 'Newham', 'count': 711},
 {'_id': 'Swale', 'count': 686},
 {'_id': 'Chelmsford', 'count': 680},
 {'_id': 'Medway', 'count': 672},
 {'_id': 'Bexley', 'count': 607},
 {'_id': 'Southend-On-Sea', 'count': 586},
 {'_id': 'Tendring', 'count': 542}]


# Convert the result to a Pandas DataFrame
result_df = pd.DataFrame(results)
# Display the number of rows in the DataFrame
print("Rows in DataFrame: ", len(result_df))
# Display the first 10 rows of the DataFrame
result_df.head(10) 

    
    Rows in DataFrame:  55 

   
_id	count
0	Thanet	1130
1	Greenwich	882
2	Maidstone	713
3	Newham	711
4	Swale	686
5	Chelmsford	680
6	Medway	672
7	Bexley	607
8	Southend-On-Sea	586
9	Tendring	542 

     Hint: You will need to use the aggregation method to answer this.

    The first 5 rows of your resulting DataFrame should look something like this:  

    

