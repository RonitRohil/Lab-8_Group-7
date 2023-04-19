# Lab-8_Group-7

### IT314-Software Engineering Lab-8
### Unit Testing with JUnit

#### Group - 7 Location Sharing System

#### Members

 1. 202001063 - Bhalodiya Hem Pareshbhai
 2. 202001066 - Japan Vijay Bhatt
 3. 202001068 - Dhrupal Kukadia
 4. 202001078 - Shashank Didwania
 5. 202001081 - Ronit Jain
 6. 202001083 - Patel Vedant Vipulbhai
 7. 202001093 - Parmar Dhruv Jayeshbhai
 8. 202001106 - Hardi Sanghani
 9. 202001115 - Aditya Kothari


Source: https://www3.ntu.edu.sg/home/ehchua/programming/java/JavaUnitTesting.html

# Java Unit Testing - JUnit & TestNG

### Goal:

The goal of this lab is to learn how to use JUnit to write unit tests for Java programs.

### Preliminary – Learn about JUnit in Eclipse:

The primary goal of unit testing is to take the smallest piece of testable software in an application, isolate it from the remainder of the code, and determine whether or not it behaves the way you expect it to behave. Each unit is tested separately before integrating it into the rest of the program. In other words, classes should be tested in isolation from other classes (test the methods of a class before you use the class elsewhere). Unit testing has proven its value in that a large percentage of defects are identified during unit testing.


# class code

```
const express = require('express')
const User = require('../models/user.model')
const router = express.Router()
const jwt = require("jsonwebtoken")

router.post('/signup',async(req,res)=>{
    const {email,password} = req.body;

  try {

    if(await User.findOne({email}))
    {
      res.status(400).json({
                message:'Email is already registered'
            })   
    }
    else
    {
        const userDoc = await User.create({
        email,
        password,
        });
        res.json(userDoc);
    }
    
  } catch (e) {
    res.status(500).json({error : e.message});
  }
    
})


router.post('/signin', async(req,res)=>{
  try{const {email,password} = req.body;
  const userDoc = await User.findOne({email});

  if (userDoc) {
    if (password == userDoc.password) {
        const token = jwt.sign({id:userDoc._id}, "passwordKey");
        res.status(200).json({token, ...userDoc._doc})
    } 
    else {
      res.status(400).json({msg : "Password is Wrong"});
    }
  } 
  else {
     return res.status(400).json({msg : "User with this email does not exist !"});
  }}
  catch(e)
  {
    res.status(500).json({error : e.message});
  }
})
module.exports = router
```



# Test Case Code
```
const chai = require("chai");
const app = require("../routes/user.route");
const chaiHttp = require('chai-http');

chai.use(chaiHttp);

describe('http://10.200.8.251:8080', ()=>{
    describe('POST /signin', ()=>{
  
it('Should success if credential is valid', async() => {
        const res = await chai.request(app)
        .post('/signin')
        .send({email:"abc@gmail.com",
                password:"12345"});
        expect(res).to.have.status(400);
    }); 
   }); 

   describe('POST /signin', ()=>{
  
it('Should success if credential is valid', async() => {
        const res = await chai.request(app)
        .post('/signin')
        .send({email:"abc@gmail.com",
                password:"1234"});
        expect(res).to.have.status(200);
    }); 
   });

   describe('POST /signup', ()=>{
  
it('Should create new User', async() => {
        const res = await chai.request(app)
        .post('/signup')
        .send({email:"abcd@gmail.com",
                password:"1234"});
        expect(res).to.have.status(200);
    }); 
   });

   

   
});
```

# result

![image](https://user-images.githubusercontent.com/99037272/233171809-8343099e-a8c7-464a-bf1d-32f41c52ba2c.png)

![image](https://user-images.githubusercontent.com/99037272/233206044-30802480-ca6f-4d07-b187-2c48d35053a1.png)

