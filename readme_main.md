
// starting 

show databases

use fake_school

db 

show collections

db.<name of collection>.find()

db.students.find()

db.students.find().limit(1)

//

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
    'ipo.valuation_amount': 1,
    'ipo.valuation_currency_code':1
}).pretty()


## 2b


db.companies.find({
    "ipo.valuation_currency_code" : "USD",
    "ipo.valuation_amount" :{
        '$gte': 100000000
    },
     
},{
    'name': 1,
    'ipo': 1,
}).pretty()


db.companies.find({
    "ipo.valuation_currency_code" : "USD",
    "ipo.valuation_amount" :{
        '$gte': 100000000
    },
     
},{
    'name': 1,
    'ipo.valuation_currency_code': 1,
    'ipo.valuation_amount' : 1
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


db.inspections.find({
'address.city':"RIDGEWOOD",
'result': {$ne: 'Violation Issued'}
},{
    'business_name': 1,
    'address.city' : 1,
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
   'storeLocation': {'$in':['Denver','Seattle']}
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
// elemMath for nested objects 
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



//try out 2 for ans
// elemMath for nested objects 

db.sales.find({
    'couponUsed':false,
    'items':{
        '$elemMatch':{
            'name':'envelopes',
            'quantity':{
      '$gt':8
      }
     },'$elemMatch':{
            'name':'notpad',
            'quantity':{
      '$gt':1
      }
     }
    }
   },{
      'storeLocation':1,
        'customer':1,
        'couponUsed':1,
        'items.$':1   
}).pretty()

//$and -- expresions
//$or -- values

<!-- /// start example -->
// range greater and less 
db.listingsAndReviews.find({
    'bedrooms':{
        '$gt':3,
        '$lt':6
    }
},{
    'name':1,
    'bedrooms':1,
    'beds':1
}).pretty().limit(8)




// or condition -- convert to  array 
// array 
db.listingsAndReviews.find({
   'amenities':{
     '$in':['Oven','Microwave','Stove']
   }
},{
    'name':1,
    'amenities':1
}).pretty().limit(10)



db.listingsAndReviews.find({
   'amenities':{
     '$in':['TV','Cable TV']
   }
},{
    'name':1,
    'amenities':1
}).pretty().limit(20)






// AND condition use $ all to indicate all elements stated to be found
// array 
db.listingsAndReviews.find({
   'amenities':{
     '$all':['Oven','Microwave','Stove']
   }
},{
    'name':1,
    'amenities':1
}).pretty().limit(20)



// 10006546

db.listingsAndReviews.find({
'_id':ObjectId('10006546')
}).pretty().limit(20)


// nested objects
// $or

db.listingsAndReviews.find({
'$or': [{'address.country': 'Brazil', 
        'bedrooms':{'$gt':3},
        'beds':2},
       {'address.country': 'Canada',
        'bedrooms':{'$lt':3}},
      ]
},{
    'name':1, 'address.country': 1,'bedrooms':1
}).pretty().limit(20)



//elemmatch --- nested dunno how deep is it. 

db.listingsAndReviews.find({
'reviews':{
    '$elemMatch':{
        'reviewer_name':'Octavio'
    }
  }
},{
    'name':1, 'reviews.$':1
}).pretty().limit(20)


//
db.listingsAndReviews.find({
'$and':[
    {'reviews':{'$elemMatch': {'reviewer_name':'Octavio'}}},
    {'reviews':{'$elemMatch': {'reviewer_name':'Alex'}}}
]
},{
    'name':1, 'reviews.$':1
}).pretty().limit(20)

//
db.listingsAndReviews.find({
'first_review':{
    '$lte':ISODate('2018-12-31'),
    '$gte':ISODate('2016-12-31'),

   }
},{
    'name':1,
    'first_review': 1
}).pretty().limit(20)

//
db.listingsAndReviews.find({
'first_review':{
    '$lte':ISODate('2018-12-31'),

   }
},{
    'name':1,
    'first_review': 1
}).pretty().limit(20)



// sort upper and lower case issue 
//regex regula expression

db.listingsAndReviews.find({
    'name':{
        '$regex':'Spacious', '$options':'i'
       }

    },{'name':1}).pretty()

// count
db.listingsAndReviews.find({
    'bedrooms': 8
}).count()


//
db.listingsAndReviews.find({

    'amenities.5':{
        '$exists':true
    }
},{'name':1,'amenities':1})


db.listingsAndReviews.find({

    'amenities.5':{
        '$exists':true
    }
},{'name':1,'amenities':{
    $slice:[5,1]
}})

db.listingsAndReviews.find({
    'address.country':{
        '$not':{
          $in: ['Brazil','Canada']
        }
       }
     },{
        'name': 1,'address.country': 1
     }).pretty()




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


//insert

// if insertMany need to put it in an array []
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
            "Subjects": ['Transfiguration', 'Alchemy','Arithmancy']
        }
})



db.students.updateOne({
    "_id" : ObjectId("62a58868a7061577e2ad3587")
    
    },{
        '$set':{
            "Subjects": ['Defense Against the Dark Arts', 'Charms']
        }
})


db.students.updateOne({
    "_id" : ObjectId("62a58868a7061577e2ad3587")
    
    },{
        '$unset':{
            "Subjects": ""
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
            "Subjects":['Defense Against the Dark Arts', 'Charms']

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



Animals handson
// use animal_shelter

db.animals.insertOne({
    'name':'Fluffy',
    'age': 3,
    'breed':'Golden Retriever',
    'type':'Dog'
})

// updateMany

db.products.updateMany({
    'price':899

},{
    '$set':{
        'price': 1000
    }
})

db.products.deleteMany({'price':1000})



//
HANDS ON FOR CRUD


Add in the following pets:

Name: Jorden,
Age: 15
Breed: Golden Retriever
Species: dog


Name: Dash
Age: 3
Breed: Hamster
Species: Hamster


Name: Carrot
Age: 1.5
Breed: Australian Dwarf
Species: Rabbit



db.pets.insertMany([
{  
    'Name':'Jorden',
    'Age': 3,
    'Breed':'Golden Retriever',
    'Species':'Dog'

},{
    'Name':'Dash',
    'Age': 3,
    'Breed':'Hamster',
    'Species':'Hamster'

},{
    'Name':'Carrot',
    'Age': 1.5,
    'Breed':'Australian Dwarf',
    'Species':'Rabbit'
}

])
