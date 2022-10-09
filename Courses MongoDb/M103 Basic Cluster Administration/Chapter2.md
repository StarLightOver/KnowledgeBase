# lec 1

```powershell
# Creating the keyfile and setting permissions on it:
sudo mkdir -p /var/mongodb/pki/
sudo chown vagrant:vagrant /var/mongodb/pki/
openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile
chmod 400 /var/mongodb/pki/m103-keyfile

#Creating the dbpath for node1:
mkdir -p /var/mongodb/db/node1

#Starting a mongod with node1.conf:
mongod -f node1.conf

#Copying node1.conf to node2.conf and node3.conf:
cp node1.conf node2.conf
cp node2.conf node3.conf

#Editing node2.conf using vi:
vi node2.conf

#Saving the file and exiting vi:
:wq

mkdir /var/mongodb/db/{node2,node3}
mongod -f node2.conf
mongod -f node3.conf

mongo --port 27011
```

# lab 1

```powershell
mongo --host "localhost:27004" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
```

# lec 2

```yaml
replication:
  replSetName: m103-csrs
security:
  keyFile: /var/mongodb/pki/m103-keyfile
net:
  bindIp: localhost,192.168.103.100
  port: 26001
systemLog:
  destination: file
  path: /var/mongodb/db/csrs1.log
  logAppend: true
processManagement:
  fork: true
storage:
  dbPath: /var/mongodb/db/csrs1
```

```powershell
mongod -f csrs_1.conf
mongod -f csrs_2.conf
mongod -f csrs_3.conf

mongo --port 26001
rs.initiate()
```

```javascript
use admin

db.createUser(
    {
        user: "m103-admin",
        pwd: "m103-pass",
        roles: [{role: "root", db: "admin"}]
    })

db.auth("m103-admin", "m103-pass")
```

```javascript
rs.add("192.168.103.100:26002")
rs.add("192.168.103.100:26003")
```

```yaml
sharding:
  configDB: m103-csrs/192.168.103.100:26001,192.168.103.100:26002,192.168.103.100:26003
security:
  keyFile: /var/mongodb/pki/m103-keyfile
net:
  bindIp: localhost,192.168.103.100
  port: 26000
systemLog:
  destination: file
  path: /var/mongodb/db/mongos.log
  logAppend: true
processManagement:
  fork: true
```

```powershell
mongos -f mongos.conf

mongo --port 26000 --username m103-admin --password m103-pass --authenticationDatabase admin
```

```yaml
sharding:
  clusterRole: shardsvr
storage:
  dbPath: /var/mongodb/db/node1
wiredTiger:
  engineConfig:
    cacheSizeGB: .1
net:
  bindIp: 192.168.103.100,localhost
  port: 27011
security:
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node1/mongod.log
  logAppend: true
processManagement:
  fork: true
replication:
  replSetName: m103-repl
```

```powershell
mongo --port 27012 -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
```

```javascript
use admin
db.shutdownServer()
```

```powershell
mongod -f node2.conf
```

```javascript
rs.stepDown()
sh.addShard("m103-repl/192.168.103.100:27012")
```

# lab 2


# lec 3


```powershell
use config
show collections
```
```javascript
db.chunks.findOne()
db.settings.save({_id: "chunksize", value: 2})
sh.status()
```
```powershell
mongoimport /dataset/products.part2.json --port 26000 -u "m103-admin" -p "m103-pass" --authenticationData
```