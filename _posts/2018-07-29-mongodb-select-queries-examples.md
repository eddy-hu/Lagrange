---
layout: post
title: "MongoDB select queries examples"
author: "Eddy Hu"
categories: journal
tags: [mongodb,database]
image: mongodb.jpeg
---
#### MongoDB queries and SQL queris
    db.users.find() select * from users  
    db.users.find({"age" : 27}) select * from users where age = 27  
    db.users.find({"username" : "joe", "age" : 27}) select * from users where "username" = "joe" and age = 27  
    db.users.find({}, {"username" : 1, "email" : 1}) select username, email from users  
    db.users.find({}, {"username" : 1, "_id" : 0}) // no case  
    db.users.find({"age" : {"$gte" : 18, "$lte" : 30}}) select * from users where age >=18 and age <= 30 // $lt(<) $lte(<=) $gt(>) $gte(>=)  
    db.users.find({"username" : {"$ne" : "joe"}}) select * from users where username <> "joe"  
    db.users.find({"ticket_no" : {"$in" : [725, 542, 390]}}) select * from users where ticket_no in (725, 542, 390)  
    db.users.find({"ticket_no" : {"$nin" : [725, 542, 390]}}) select * from users where ticket_no not in (725, 542, 390)  
    db.users.find({"$or" : [{"ticket_no" : 725}, {"winner" : true}]}) select * form users where ticket_no = 725 or winner = true  
    db.users.find({"id_num" : {"$mod" : [5, 1]}}) select * from users where (id_num mod 5) = 1  
    db.users.find({"$not": {"age" : 27}}) select * from users where not (age = 27)  
    db.users.find({"username" : {"$in" : [null], "$exists" : true}}) select * from users where username is null // if passed: find({"username" : null}) then we can find out "no username"results  
    db.users.find({"name" : /joey?/i}) // regex, value is a PCRE regex
    db.food.find({fruit : {$all : ["apple", "banana"]}}) // select from collection
    db.food.find({"fruit.2" : "peach"}) // select from collection
    db.food.find({"fruit" : {"$size" : 3}}) // select from collection  
    db.people.find({"name.first" : "Joe", "name.last" : "Schmoe"}) // nested 
    db.blog.find({"comments" : {"$elemMatch" : {"author" : "joe", "score" : {"$gte" : 5}}}}) // nested 
    db.foo.find({"$where" : "function() { return this.x + this.y == 10; }"}) // $where could uses javascript function  
    db.foo.find().sort({"x" : 1}).limit(1).skip(10); // return index (10, 11], forted by"x".
