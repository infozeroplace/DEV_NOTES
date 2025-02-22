## **How to maintain VPS local MongoDB**

**If you're inserting data into a local MongoDB instance on a VPS, there are several better ways to do it instead of using the terminal:**

### 1. Use MongoDB Compass (GUI)

- Install MongoDB Compass on your local machine.
- Connect to your VPS MongoDB instance remotely.
- Insert data easily using the GUI.

How to connect?

- Ensure your MongoDB instance is listening on 0.0.0.0 instead of 127.0.0.1.
- Open "/etc/mongod.conf" and modify:

```
bindIp: 0.0.0.0
```

- Restart MongoDB:

```
sudo systemctl restart mongod
```

- Allow remote access through the firewall:

```
sudo ufw allow 27017/tcp
```

- Use MongoDB Compass to connect via:

```
mongodb://your-vps-ip:27017
```

### 2. Use a Web-Based Admin Panel

- Mongo Express – Lightweight and easy to set up.
- AdminMongo – More features for database management.

- Install Mongo Express on VPS:

```
npm install -g mongo-express
```

- Run it:

```
mongo-express --url mongodb://localhost:27017
```

- Access via Browser:

```
http://your-vps-ip:8081
```

(Default port: 8081, you can change it in the config.)

### 3. Use a Node.js Script

- If you have structured data (JSON, CSV), write a simple Node.js script to insert data.

- Example:

```
const { MongoClient } = require("mongodb");

async function insertData() {
    const client = new MongoClient("mongodb://localhost:27017");
    await client.connect();
    const db = client.db("your_database");
    const collection = db.collection("your_collection");

    const data = [
        { name: "Product A", price: 10 },
        { name: "Product B", price: 20 }
    ];

    await collection.insertMany(data);
    console.log("Data inserted successfully!");

    client.close();
}

insertData().catch(console.error);
```

- Run it:

```
node insert.js
```

### 4. Use MongoDB Import Tool (for Bulk Data)

If you have data in a JSON or CSV file, use mongoimport:

```
mongoimport --db your_database --collection your_collection --file data.json --jsonArray
```

- Example data.json:

```
[
    { "name": "Item 1", "price": 100 },
    { "name": "Item 2", "price": 200 }
]
```

### 5. Use a Remote MongoDB Cluster (Alternative)

If managing MongoDB on a VPS is annoying, you can use MongoDB Atlas (fully managed cloud MongoDB).

- Create a free tier at MongoDB Atlas.
- Connect to it from your VPS without worrying about database maintenance.

### Which One is Best for You?

- For manual entry: Use MongoDB Compass or Mongo Express.
- For bulk insert: Use mongoimport.
- For automation: Use a Node.js script.
- For hassle-free management: Use MongoDB Atlas.

Let me know which method you prefer, or if you need help setting it up! 🚀

## **How to Secure MongoDB**

### 1. Create an Admin User in MongoDB

- Connect to MongoDB Shell

- Run:

```
mongosh
```

- or (if using the old mongo shell):

```
mongo
```

- Switch to the Admin Database:

```
use admin
```

- Create an Admin User with a Password:

```
db.createUser({
    user: "myAdminUser",
    pwd: "MySecurePassword123",
    roles: [{ role: "root", db: "admin" }]
})
```

Use a strong password! Don’t use simple passwords like "1234".

- Exit the MongoDB Shell:

```
exit
```

### 2. Enable Authentication in MongoDB Config

- Edit the MongoDB configuration file:

```
sudo nano /etc/mongod.conf
```

- Find the security section and enable authorization::

```
security:
  authorization: enabled
```

If the security: section doesn’t exist, add it at the bottom.

Save and exit (CTRL + X, then Y, then ENTER).

### 3. Restart MongoDB

- Apply the changes by restarting MongoDB:

```
sudo systemctl restart mongod
```

### 4. Test the Authentication

- Now, try connecting without a username:

```
mongosh
```

You should get an authentication error.

- Now, connect with authentication:

```
mongosh -u "myAdminUser" -p "MySecurePassword123" --authenticationDatabase "admin"
```

### 5. Update Your Connection String

Since authentication is now enabled, your connection string must include the

- username and password:

```
mongodb://myAdminUser:MySecurePassword123@your-vps-ip:27017/admin
```

- If your app uses MongoDB, update the MongoDB URI in your .env file:

```
MONGODB_URI=mongodb://myAdminUser:MySecurePassword123@your-vps-ip:27017/admin
```

### 6. Restrict External Access (Extra Security)

**(A) Bind MongoDB to Localhost (Recommended)**

- If you don’t need external access, modify /etc/mongod.conf:

```
bindIp: 127.0.0.1
```

- Restart MongoDB:

```
sudo systemctl restart mongod
```

Now, MongoDB can only be accessed from the VPS itself.

**(B) Allow Only Specific IPs**

- If you need remote access but want to allow only your IP, use UFW Firewall:

```
sudo ufw allow from YOUR_IP to any port 27017
```

- To check UFW rules:

```
sudo ufw status
```

- To enable UFW (if not already enabled):

```
sudo ufw enable
```

Now Your MongoDB is Secure! ✅
Authentication is required to connect.
Remote access is restricted to specific IPs (if configured).
Attackers can't access MongoDB without a password.
Let me know if you need more help! 🚀
