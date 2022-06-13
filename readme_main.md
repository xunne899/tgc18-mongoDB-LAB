HANDS ON
Create a new mongodb database with the name fake_school
Create a new collection name students
Add to the students collection the following documents:

Name: Jane Doe
Age: 13
Subjects: Defense Against the Dark Arts, Charms, History of Magic
Date Enrolled: 13th May 2016

//updated
Name: Jane Doe Jr
Age: 11
Subjects: Defense Against the Dark Arts, Charms
Date Enrolled: 13th May 2016
 
Name: James Verses
Age: 14
Subjects: Transfiguration, Alchemy
Date Enrolled: 15th June 2015
 
Name: Jonathan Goh
Age: 12
Subjects: Divination, Study of Ancient Runes
Date Enrolled: 16th April 2017


// handson 2

HANDS ON
Change the age of James Verses to 13
Change the student with the name of "Jane Doe" to "Jane Doe Jr" and her age to 11.
Remove Jonathan Goh from the system


// handson 3

HANDS ON
Add Arithmancy to James Verses' subjects
Remove History of Magic from Jane Doe Jr's subjects.



show databases

use fake_school

db 

show collections

db.<name of collection>.find()

db.students.find()

//insert
db.students.insertMany([{
    'Name':'Roger',
    'Age': 33,
    'Subjects':'fast coding method',
    'Date_Enrolled':'27th April 2009'
},{
    'Name':'David',
    'Age': 40,
    'Subjects':'superb coding programme',
    'Date_Enrolled':'20th June 2003'
    }])



// insert one 
db.students.insertOne({
    'Name': 'Jonathan Goh',
    'Age': 12,
   'Subjects': 'Divination, Study of Ancient Runes'
   'Date_Enrolled': '16th April 2017'
})



db.students.insertOne({
    'Name': 'Jane Doe Jr',
    'Age': 11,
    'Subjects': 'Defense Against the Dark Arts, Charms',
    'Date_Enrolled': '13th May 2016'
})



// delete
Delete object 
db.students.deleteMany({
   "_id" : ObjectId("62a54889a7f5a872b74738e9"),
   "_id" : ObjectId("62a54889a7f5a872b74738ea")
})

db.students.deleteOne({
   "_id" : ObjectId("62a54889a7f5a872b74738e9"),
 
})

// delete jonathan 
db.students.deleteOne({
    "_id" : ObjectId("6217329f4fe17b3c734240d2")
})

//update
db.students.updateOne({
       "_id" : ObjectId("6217329f4fe17b3c734240d2")
}, {
    '$set': {
        'Age':'67'
    }
})


// for array
db.perfume.updateOne({
    "_id" : ObjectId("62a4984b1e43fabae663e858")
    },{
       '$push': {
       "capacity": '200ml'
    }

})

db.students.updateOne({
    "_id" : ObjectId("621732884fe17b3c734240d1")
    
    },{
        '$set':{
            "Subjects": "Transfiguration, Alchemy,Arithmancy"
        }
})



db.perfume.updateOne({
        '_id':ObjectId("623cbd2db6366ca99675dde3")
}, {
    '$set': {
        'capacity':'150ml'
    }
})

## remove array attributes to  perfume 
db.perfume.updateOne({
    '_id':ObjectId("623cbd2db6366ca99675dde2"),
}, {
    '$pull': {
        'capacity': '100ml'
    }
})

//Remove History of Magic from Jane Doe Jr's subjects.
db.students.updateOne({
    "_id" : ObjectId("621732644fe17b3c734240d0")
    },{ 
        '$set': {
            "Subjects":"Defense Against the Dark Arts, Charms"

    }
})




db.students.updateOne({
    "_id" : ObjectId("621732644fe17b3c734240d0")
    },{ 
        '$set': {
           'Age': 13,
           'Subjects': 'Defense Against the Dark Arts, Charms',
           'Date_Enrolled': '13th May 2016'

    }
})




// remove a key from document 
db.animals.update({
    '_id':ObjectId("5f33acfbbf91d0dd5c1440df")
}, {
    '$unset': {
        'date':""
    }
})

// non array
.remove()






Use the sample_training database


Project the company name and year founded and find by the criteria below:


All companies founded in the year 2006,
All companies founded after the year 2000
All companies founded between the year 1900 and 2010


Project the company name, the valuation amount and the valuation currency of its IPO, and find by the criteria below
All companies with valuation amount higher than 100 million
All companies with valuation amount higher than 100 million and with the currency being 'USD'


## 1a
db.companies.find({
    'founded_year': 2006
},{
    'name':1,'founded_year':1
}).pretty()

## 1b

db.companies.find({
    'founded_year': {
        '$gte': 2000
    }
},{
    'name': 1,
    'founded_year':1
}).pretty()


## 1c

db.companies.find({
    'founded_year':{
        '$gte': 1900,
        '$lte': 2010
    }
},{
    'name':1,
    'founded_year':1
}).pretty()


## 2a 
db.companies.find({
    "ipo.valuation_amount" :{
        '$gte': 100000000
    }
},{
    'name': 1,
    'ipo': 1
}).pretty()


## 2b


db.companies.find({
    "ipo.valuation_currency_code" : "USD",
    "ipo.valuation_amount" :{
        '$gte': 100000000
    },
     
},{
    'name': 1,
    'ipo': 1
}).pretty()

db.companies.find({
      "ipo.valuation_currency_code" : "USD",
}).pretty()
 <!-- "valuation_currency_code" : "USD" -->

//handson

Use the inspections collection from the sample_training database for the questions below
Find all businesses which has violations issued
Find all business which has violations, and are in the city of New York.
Count how many businesses there in the city of New York
Count how many businesses there are in the city of Ridgewood and does not have violations (hint: google for "not equal" in Mongo)

 ## 1
db.inspections.find({
'result': 'Violation Issued'
},{
    'business_name': 1,
    'result':1
}).pretty()

## 2
db.inspections.find({
'address.city':"NEW YORK",
'result': 'Violation Issued'
},{
    'business_name': 1,
    'address.city' : 1,
    'result':1
}).pretty()

## 3


db.inspections.find({
    'address.city':"NEW YORK"
},{
    'business_name': 1,
    'address.city' : 1
}).pretty().count()


db.inspections.find({
    'address.city':"NEW YORK"
},{
    'business_name': 1,
    'address.city' : 1
}).count()


## 4

db.inspections.find({
'address.street':"RIDGEWOOD PL",
'result': {$ne: 'Violation Issued'}
},{
    'business_name': 1,
    'address.street' : 1,
    'result':1
}).pretty()



// handson
Use the accounts document from the sample_analytics database and answer the following questions:
Find all accounts that have the InvestmentStock product
Find all accounts that have both the Commodity and InvestmentStock product
Find all accounts that have either Commodity OR CurrencyService product
Find all accounts that does not have CurrencyService product
Find all products have a limit of more than 1000, and offer both InvestmentStock and InvestmentFund products




## 1 
db.accounts.find({
   'products': 'InvestmentStock'
    },{
        'account_id':1,
        'products':1
      
}).pretty()

## 2

db.accounts.find({
   'products': { '$all':['InvestmentStock','Commodity']}
    },{
        'account_id':1,
        'products':1
      
}).pretty()

## 3

db.accounts.find({
   'products': { '$in':['CurrencyService','Commodity']}
    },{
        'account_id':1,
        'products':1
      
}).pretty()


## 4 
db.accounts.find({
   'products': { '$ne':'CurrencyService'}
    },{
        'account_id':1,
        'products':1
      
}).pretty()


## 5

db.accounts.find({
    'limit':{
        '$gt':1000
    },
   'products': { '$all':['InvestmentStock','Commodity']}
    },{
        'account_id':1,
        'limit':1,
        'products':1
      
}).pretty()


db.listingsAndReviews.find({ 'amenities':{ '$all':['Oven', 'Microwave', 'Stove', 'Dishes and silverware'] } },{ 'name':1, 'amenities':1 }).pretty()


Use the sales collection from the sample_supplies and answer the following questions:
Show the items sold from the stores at Denver and Seattle.
Show the items sold from the stores at Denver and where the customer's satisfaction is at least 3.
Show all onlines sales made at Denver and sales made through phone at Seattle
Show all sales that does not use a coupon
Show all envelopes sales where more than 8 envelopes are sold and no coupon are used.

## 1 

db.sales.find({
   'storeLocation': { '$in':['Denver','Seattle']}
    },{
        'items':1,
        'storeLocation':1,
        'customer':1
      
}).pretty()


## 2
db.sales.find({
   'customer.satisfaction' : 3,
   'storeLocation': 'Denver'
    },{
        'items':1,
        'storeLocation':1,
        'customer':1
      
}).pretty()

## 3 

db.sales.find({ 
    '$or':[ 
        {'storeLocation':'Denver', 'purchaseMethod':'Online' }, 
        {'storeLocation':'Seattle', 'purchaseMethod':'Phone'} ] 
    },{ 
       'storeLocation':1,
        'customer':1,
        'purchaseMethod':1

    }).pretty()



## 4 

db.sales.find({
  'couponUsed':false

},{
    'storeLocation':1,
        'customer':1,
        'purchaseMethod':1,
        'couponUsed':1
}).pretty()

## 5

<!-- db.sales.find({
   'couponUsed':false,
  'items.name':'envelopes',
  'items.quantity':{
      '$gt':8
  }
  
},{
        'storeLocation':1,
        'customer':1,
        'items.name' :1,
        'items.quantity':1,
        'couponUsed':1
}).pretty()


db.sales.find({
   'items.name': { '$in':['envelopes']},
    'couponUsed':false,
    },{
        'items':1,
        'storeLocation':1,
        'customer':1
      
}).pretty() -->


```
db.sales.find({
    'couponUsed':false,
    'items':{
        '$elemMatch':{
            'name':'envelopes',
            'quantity':{
      '$gt':8
      }
     }
    }
   },{
      'storeLocation':1,
        'customer':1,
        'couponUsed':1,
        'items.$':1   
}).pretty()

