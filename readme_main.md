HANDS ON
Create a new mongodb database with the name fake_school
Create a new collection name students
Add to the students collection the following documents:

Name: Jane Doe
Age: 13
Subjects: Defense Against the Dark Arts, Charms, History of Magic
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




db.students.insertOne({
    'name':'Fluffy',
    'age': 3,
    'subjects':'divine coding method',
    'Date_Enrolled':'2009'
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

//update
db.students.updateOne({
       "_id" : ObjectId("6217329f4fe17b3c734240d2")
}, {
    '$set': {
        'Age':'67'
    }
})