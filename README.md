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
  # review the collections in our new database
pprint(db.establishments.find_one())

   {'AddressLine1': 'The Bay',
 'AddressLine2': 'St Margarets Bay',
 'AddressLine3': 'Kent',
 'AddressLine4': '',
 'BusinessName': 'The Coastguard Inn',
 'BusinessType': 'Pub/bar/nightclub',
 'BusinessTypeID': 7843,
 'ChangesByServerID': 0,
 'Distance': 4587.347174863443,
 'FHRSID': 1034540,
 'LocalAuthorityBusinessID': 'PI/000078691',
 'LocalAuthorityCode': '182',
 'LocalAuthorityEmailAddress': 'publicprotection@dover.gov.uk',
 'LocalAuthorityName': 'Dover',
 'LocalAuthorityWebSite': 'http://www.dover.gov.uk/',
 'NewRatingPending': False,
 'Phone': '',
 'PostCode': 'CT15 6DY',
 'RatingDate': '2022-08-17T00:00:00',
 'RatingKey': 'fhrs_5_en-gb',
 'RatingValue': '5',
 'RightToReply': '',
 'SchemeType': 'FHRS',
 '_id': ObjectId('66a478ffebb4046f8d7ca7c2'),
 'geocode': {'latitude': '51.152225', 'longitude': '1.387974'},
 'links': [{'href': 'https://api.ratings.food.gov.uk/establishments/1034540',
            'rel': 'self'}],
 'meta': {'dataSource': None,
          'extractDate': '0001-01-01T00:00:00',
          'itemCount': 0,
          'pageNumber': 0,
          'pageSize': 0,
          'returncode': None,
          'totalCount': 0,
          'totalPages': 0},
 'scores': {'ConfidenceInManagement': 0, 'Hygiene': 0, 'Structural': 5}}

# assign the collection to a variable
establishments = db['establishments']

   
   print(db.list_collection_names())
['admin', 'autosaurus', 'classDB', 'config', 'fruits_db', 'local', 'met', 'petsitly_marketing', 'travel_db', 'uk_food']

    # assign the uk_food database to a variable name
    db = mongo['uk_food'] 

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


    new_restaurant ={
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
	# Create a dictionary for the new restaurant data
new_restaurant ={
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
    

    Update the new restaurant with the BusinessTypeID you found. 

# Insert the new restaurant into the collection
establishments.insert_one(new_restaurant)   

   InsertOneResult(ObjectId('66a47d7a9f68a68f51f3dacd'), acknowledged=True)


# Check that the new restaurant was inserted
query = {"BusinessName": "Penang Flavours"}
results = establishments.find(query)
for result in results:
    pprint(result)  

{'AddressLine1': 'Penang Flavours',
 'AddressLine2': '146A Plumstead Rd',
 'AddressLine3': 'London',
 'AddressLine4': '',
 'BusinessName': 'Penang Flavours',
 'BusinessType': 'Restaurant/Cafe/Canteen',
 'BusinessTypeID': '',
 'Distance': 4623.972328074718,
 'LocalAuthorityCode': '511',
 'LocalAuthorityEmailAddress': 'health@royalgreenwich.gov.uk',
 'LocalAuthorityName': 'Greenwich',
 'LocalAuthorityWebSite': 'http://www.royalgreenwich.gov.uk',
 'NewRatingPending': True,
 'Phone': '',
 'PostCode': 'SE18 7DY',
 'RightToReply': '',
 'SchemeType': 'FHRS',
 '_id': ObjectId('66a47d7a9f68a68f51f3dacd'),
 'geocode': {'latitude': '51.49014200', 'longitude': '0.08384000'},
 'scores': {'ConfidenceInManagement': '', 'Hygiene': '', 'Structural': ''}}
    

# Find the BusinessTypeID for "Restaurant/Cafe/Canteen" and return only the BusinessTypeID and BusinessType fields
query = {"BusinessType": "Restaurant/Cafe/Canteen"}
fields = {"BusinessTypeID": 1, "BusinessType": 1}  # Include only desired fields
results = establishments.find(query, fields).limit(10)
for result in results:
    pprint(result)

{'BusinessType': 'Restaurant/Cafe/Canteen',
 'BusinessTypeID': 1,
 '_id': ObjectId('66a478ffebb4046f8d7ca7c5')}
{'BusinessType': 'Restaurant/Cafe/Canteen',
 'BusinessTypeID': 1,
 '_id': ObjectId('66a478ffebb4046f8d7ca7c6')}
{'BusinessType': 'Restaurant/Cafe/Canteen',
 'BusinessTypeID': 1,
 '_id': ObjectId('66a478ffebb4046f8d7ca7c7')}
{'BusinessType': 'Restaurant/Cafe/Canteen',
 'BusinessTypeID': 1,
 '_id': ObjectId('66a478ffebb4046f8d7ca7c8')}
{'BusinessType': 'Restaurant/Cafe/Canteen',
 'BusinessTypeID': 1,
 '_id': ObjectId('66a478ffebb4046f8d7ca7ca')}
{'BusinessType': 'Restaurant/Cafe/Canteen',
 'BusinessTypeID': 1,
 '_id': ObjectId('66a478ffebb4046f8d7ca7cb')}
{'BusinessType': 'Restaurant/Cafe/Canteen',
 'BusinessTypeID': 1,
 '_id': ObjectId('66a478ffebb4046f8d7ca7d6')}
{'BusinessType': 'Restaurant/Cafe/Canteen',
 'BusinessTypeID': 1,
 '_id': ObjectId('66a478ffebb4046f8d7ca7d8')}
{'BusinessType': 'Restaurant/Cafe/Canteen',
 'BusinessTypeID': 1,
 '_id': ObjectId('66a478ffebb4046f8d7ca7d9')}
{'BusinessType': 'Restaurant/Cafe/Canteen',
 'BusinessTypeID': 1,
 '_id': ObjectId('66a478ffebb4046f8d7ca7db')}



    

    The magazine is not interested in any establishments in Dover, so check how many documents contain the Dover Local Authority. Then, remove any establishments within the Dover Local Authority from the database, and check the number of documents to ensure they were deleted.

	# Find how many documents have LocalAuthorityName as "Dover"
count = establishments.count_documents({"LocalAuthorityName": 'Dover'})
print(f"Number of documents that have LocalAuthorityName 'Dover': {count}") 
	Number of documents that have LocalAuthorityName 'Dover': 994

# Delete all documents where LocalAuthorityName is "Dover"
query = {"LocalAuthorityName": "Dover"}
results = establishments.delete_many(query)   

# Check if any remaining documents include Dover
query = {"LocalAuthorityName": "Dover"}
count = establishments.count_documents(query)
if count > 0:
    results = establishments.find(query)
    for result in results:
        print(result)
else:
    print("No documents found for LocalAuthorityName 'Dover'")

No documents found for LocalAuthorityName 'Dover' 



# Check that other documents remain with 'find_one'
query = {"LocalAuthorityName": {"$ne": "Dover"}} # Query all LocalAuthorityName not equal to "Dover"
fields = {"BusinessName": 1, "LocalAuthorityName": 1, "_id": 0}
result = establishments.find_one(query, fields)
pprint(result)   

{'BusinessName': 'The Pavilion', 'LocalAuthorityName': 'Folkestone and Hythe'}  
	
 	Some of the number values are stored as strings, when they should be stored as numbers.
	Use update_many to convert latitude and longitude to decimal numbers.
# Change the data type from String to Decimal for longitude and latitude
establishments.update_many({}, [
    {"$set": {
        "geocode.latitude": {"$toDouble": "$geocode.latitude"},
        "geocode.longitude": {"$toDouble": "$geocode.longitude"} 
    }}
])

UpdateResult({'n': 38786, 'nModified': 38786, 'ok': 1.0, 'updatedExisting': True}, acknowledged=True) 


 
        Use update_many to convert RatingValue to integer numbers.
# Set non 1-5 Rating Values to Null
non_ratings = ["AwaitingInspection", "Awaiting Inspection", "AwaitingPublication", "Pass", "Exempt"]
establishments.update_many({"RatingValue": {"$in": non_ratings}}, [ {'$set':{ "RatingValue" : None}} ])   

	UpdateResult({'n': 4091, 'nModified': 4091, 'ok': 1.0, 'updatedExisting': True}, acknowledged=True) 

# Change the data type from String to Integer for RatingValue
establishments.update_many({}, [
    {"$set": {
        "RatingValue": {"$toInt": "$RatingValue"}
    }}
]) 

	UpdateResult({'n': 38786, 'nModified': 34695, 'ok': 1.0, 'updatedExisting': True}, acknowledged=True) 
 
# Check that the coordinates and rating value are now numbers
    sample_document = establishments.find_one()
# Loop through the fields in the sample document to check their data types
   for field, value in sample_document.items():
    data_type = type(value).__name__
    print(f"{field}: {data_type}")   

	id: ObjectId
FHRSID: int
ChangesByServerID: int
LocalAuthorityBusinessID: str
BusinessName: str
BusinessType: str
BusinessTypeID: int
AddressLine1: str
AddressLine2: str
AddressLine3: str
AddressLine4: str
PostCode: str
Phone: str
RatingValue: int
RatingKey: str
RatingDate: str
LocalAuthorityCode: str
LocalAuthorityName: str
LocalAuthorityWebSite: str
LocalAuthorityEmailAddress: str
scores: dict
SchemeType: str
geocode: dict
RightToReply: str
Distance: float
NewRatingPending: bool
meta: dict
links: list
    

 

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
	{'BusinessName': 'The Chase Rest Home',
 '_id': ObjectId('66a478ffebb4046f8d7cc5dc'),
 'scores': {'Hygiene': 20}}  

## Convert the result to a Pandas DataFrame,  print the number of rows in the DataFrame, and display the first 10 rows.
     result_df = pd.DataFrame(results)

# Convert the result to a Pandas DataFrame
    result_df = pd.DataFrame(results) 
    result_df['HygieneScore'] = result_df['scores'].apply(lambda x: x['Hygiene'])
    result_df.drop(columns=['scores'], inplace=True)
    result_df 

# Display the number of rows in the DataFrame
    print("Rows in DataFrame: ", len(result_df))  

	_id	BusinessName	RatingValue	HygieneScore
0	66a478ffebb4046f8d7d1e35	Volunteer	5	0
1	66a478ffebb4046f8d7d1e53	Plumstead Manor Nursery	5	0
2	66a478ffebb4046f8d7d1e54	Atlantic Fish Bar	5	0
3	66a478ffebb4046f8d7d1e0f	Iceland	5	0
4	66a478ffebb4046f8d7d1e1e	Howe and Co Fish and Chips - Van 17	5	0


# Display the first 10 rows of the DataFrame
    result_df['HygieneScore'] = result_df['scores'].apply(lambda x: x['Hygiene'])
    result_df.drop(columns=['scores'], inplace=True)
    result_df.head(10) 

            Rows in DataFrame:  41 
		
## Which establishments have a hygiene score equal to 20?
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


## Which establishments in London have a RatingValue greater than or equal to 4?
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

 # Convert the result to a Pandas DataFrame
    result_df = pd.DataFrame(results)

# Display the number of rows in the DataFrame
    print("Rows in DataFrame: ", len(result_df))

# Display the first 10 rows of the DataFrame
    result_df.head(10)


    

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
    {'_id': ObjectId('66a478ffebb4046f8d7d1e1e'), 'BusinessName': 'Howe and Co Fish and Chips - Van 17', 'RatingValue': 5, 'scores': {'Hygiene':      0}}

    

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

     
## Collaboration 
Omid Khan - omidk414@gmail.com - omidk414 
Gursimran Kaur - kaursimran081999@gmail.com
    

    

