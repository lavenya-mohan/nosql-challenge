# UK Food Establishments Analysis

The UK Food Standards Agency evaluates various establishments across the United Kingdom and assigns them food hygiene ratings. This project involves setting up a MongoDB database with the provided data and performing data analysis to help the editors of the food magazine **Eat Safe, Love** decide where to focus future articles.

## Project Overview

The project is divided into three main parts:
1. **Database and Jupyter Notebook Setup**
2. **Database Updates**
3. **Exploratory Data Analysis**

---

## Part 1: Database and Jupyter Notebook Setup

**Use the `NoSQL_setup_starter.ipynb` notebook for this section.**

### Steps:
1. **Import Data into MongoDB**
   - Import the data from `establishments.json` into MongoDB.
   - Use the following command in your terminal:
     ```bash
     mongoimport --type json -d UK_food -c establishments --drop --jsonArray establishments.json
     ```
     This command imports the data into a database named `UK_food` and a collection named `establishments`.

2. **Set Up Jupyter Notebook**
   - Import necessary libraries: `PyMongo` and `pprint`.
   - Create an instance of `MongoClient`.
   - Confirm the database and collection have been created:
     - List all databases and ensure `UK_food` is listed.
     - List the collections in the `UK_food` database and ensure `establishments` is there.
     - Use `find_one` to display one document from the `establishments` collection.
   - Assign the `establishments` collection to a variable for further use.

---

## Part 2: Update the Database

**Continue using the `NoSQL_setup_starter.ipynb` notebook.**

The magazine editors have requested some modifications to the database before analysis.

### Tasks:
1. **Add a New Establishment**
   - Add the following new restaurant to the `establishments` collection:
     ```json
     {
       "BusinessName": "Penang Flavours",
       "BusinessType": "Restaurant/Cafe/Canteen",
       "BusinessTypeID": "",
       "AddressLine1": "Penang Flavours",
       "AddressLine2": "146A Plumstead Rd",
       "AddressLine3": "London",
       "AddressLine4": "",
       "PostCode": "SE18 7DY",
       "Phone": "",
       "LocalAuthorityCode": "511",
       "LocalAuthorityName": "Greenwich",
       "LocalAuthorityWebSite": "http://www.royalgreenwich.gov.uk",
       "LocalAuthorityEmailAddress": "health@royalgreenwich.gov.uk",
       "scores": {
         "Hygiene": "",
         "Structural": "",
         "ConfidenceInManagement": ""
       },
       "SchemeType": "FHRS",
       "geocode": {
         "longitude": "0.08384000",
         "latitude": "51.49014200"
       },
       "RightToReply": "",
       "Distance": 4623.9723280747176,
       "NewRatingPending": true
     }
     ```

2. **Update BusinessTypeID**
   - Find the `BusinessTypeID` for "Restaurant/Cafe/Canteen".
   - Update the new restaurant ("Penang Flavours") with the correct `BusinessTypeID`.

3. **Remove Establishments in Dover**
   - Check how many documents have `LocalAuthorityName` as "Dover".
   - Remove all establishments with `LocalAuthorityName` equal to "Dover".
   - Verify the removal by checking the number of documents left.

4. **Correct Data Types**
   - Some fields are incorrectly stored as strings instead of numbers.
   - **Convert `latitude` and `longitude` to decimal numbers**:
     - Use `update_many` to convert these fields in all documents.
   - **Convert `RatingValue` to integer numbers**:
     - First, coerce any non-numeric `RatingValue` to `null`.
     - Then, use `update_many` to convert the field to integers.

---

## Part 3: Exploratory Data Analysis

**Use the `NoSQL_analysis_starter.ipynb` notebook for this section.**

The editors have specific questions to help them decide where to focus future articles.

### Notes:
- **RatingValue**: Ranges from 1-5 (higher is better). Some values may be non-numeric (e.g., 'Pass'), which should be set to `null`.
- **Scores for Hygiene, Structural, and ConfidenceInManagement**: The higher the score, the worse the establishment is in these areas.

### Questions:
1. **Establishments with a Hygiene Score of 20**
   - Find establishments where `scores.Hygiene` equals 20.

2. **Establishments in London with RatingValue >= 4**
   - Find establishments in London with a `RatingValue` greater than or equal to 4.
   - *Hint*: The `LocalAuthorityName` might not be exactly "London"; use a regular expression to match.

3. **Top 5 Establishments near "Penang Flavours"**
   - Find the top 5 establishments with a `RatingValue` of 5, sorted by the lowest hygiene score, and nearest to "Penang Flavours".
   - *Hint*: Use the geocode of "Penang Flavours" and search within ±0.01 degrees of latitude and longitude.

4. **Local Authorities with the Most Zero Hygiene Scores**
   - Find how many establishments in each `LocalAuthorityName` have a `scores.Hygiene` of 0.
   - Sort the results from highest to lowest.
   - Display the top ten local authority areas.

### For Each Question:
- Use `count_documents` to display the number of documents in the result.
- Display the first document in the results using `pprint`.
- Convert the results to a Pandas DataFrame.
  - Print the number of rows in the DataFrame.
  - Display the first 10 rows.

---

## Conclusion

This project sets up a MongoDB database from provided data, performs necessary data cleaning and updates, and conducts exploratory data analysis to answer specific questions posed by the editors of **Eat Safe, Love**. The insights gained will help the magazine focus their future articles on areas and establishments of interest.
