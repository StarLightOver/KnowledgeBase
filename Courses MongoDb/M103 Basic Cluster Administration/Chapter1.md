# lec 1

Launch mongod in the first shell:
`mongod`

Connect to the Mongo shell in the second shell:
`mongo`

To create a new collection
`db.createCollection("employees")`

Try to connect back to Mongo shell, without specifying a port:
```
use admin
db.shutdownServer()
exit
```

# lab 1

Role: readWrite on applicationData database
Authentication source: admin
Username: m103-application-user
Password: m103-application-pass

```javascript
db.createUser(
    { user: "m103-application-user",
      pwd: "m103-application-pass",
      roles: [ { db: "applicationData", role: "readWrite" } ]
    })
```

mongoimport --port 27000 -u m103-application-user -p m103-application-pass --authenticationDatabase "admin" /dataset/products.json
