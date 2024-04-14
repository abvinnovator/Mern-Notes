
# MERN Stack Notes

This repository contains my personal notes on the MERN (MongoDB, Express.js, React.js, Node.js) stack.

## COURSE NAME
MERN

## Cheat codes 🧑‍💻

### Cheat Sheet for using Mongoose with Express.js

**MongoDB & Mongoose:**
- `find`: Used to query multiple documents.
- `findOne`: Used to query a single document.
- `create`: Used to create a new document.
- `insertOne`: Used to insert a single document.
- `insertMany`: Used to insert multiple documents.
- `updateOne`: Used to update a single document matching the filter.
- `updateMany`: Used to update multiple documents matching the filter.
- `deleteOne`: Used to delete a single document matching the filter.
- `deleteMany`: Used to delete multiple documents matching the filter.

To delete all documents in a collection, pass in an empty document (`{}`):
```bash
db.collection_name.deleteMany({})
```

### Code Snippet

```javascript
const express = require('express');
const router = express.Router();
const YourModel = require('../models/YourModel');

router.get('/', async (req, res) => {
  try {
    const documents = await YourModel.find();
    res.json(documents);
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
});

router.post('/', async (req, res) => {
  const document = new YourModel({
    // Set properties based on req.body or other sources
  });

  try {
    const newDocument = await document.save();
    res.status(201).json(newDocument);
  } catch (err) {
    res.status(400).json({ message: err.message });
  }
});

router.patch('/:id', async (req, res) => {
  // Update document based on req.body
});

router.delete('/:id', async (req, res) => {
  // Delete document based on req.params.id
});

module.exports = router;
```

**HTTP Methods:**
- GET (Read operations)
- POST (Create operations)
- PUT/PATCH (Update operations)
- DELETE (Delete operations)

## Connecting Mongoose to Express and Displaying Data in React

1. Create a sample collection in Mongoose using the Mongo shell:
   ```bash
   db.collection_name.insertMany([
     {"name":"vamsi","age":17},
     {"name":"example","age":20}
   ]);
   ```

2. Set up the backend using the following commands:
   ```bash
   npm init -y
   npm install mongoose express nodemon cors
   ```

3. Create a middleware and connect to Mongoose:
   ```javascript
   const express = require('express');
   const cors = require('cors');
   const mongoose = require('mongoose');
   const ProjectModel = require('./Models/Project')

   const app = express();
   app.use(express.json());
   app.use(cors());

   mongoose.connect('mongodb://127.0.0.1:27017/user');

   app.get('/', (req, res) => {
     res.json({ message: "hello i am server" });
   });

   app.get("/user", async (req, res) => {
     try {
       const model = await ProjectModel.find();
       res.json(model);
     } catch (err) {
       console.log(err);
     }
   });

   app.listen(5000, () => {
     console.log("server is running");
   });
   ```

4. In the frontend (React):
   - Install `axios` and `react-router-dom` (optional).
   - Create a component to fetch and display the data.
   - Use `useEffect` and `useState` to manage the state.
   - Render the data using the `map` function.

   ```javascript
   import React, { useEffect, useState } from 'react'
   import axios from "axios";

   const Extract = () => {
     const [users, setUsers] = useState([])

     useEffect(() => {
       axios.get("http://localhost:5000/user")
         .then(res => setUsers(res.data))
         .catch(err => console.log(err));
     }, [])

     return (
       <div>
         {users.map((user, index) => (
           <li key={index}>{user.given_name}</li>
         ))}
       </div>
     )
   }

   export default Extract
   ```

## Creating Data from Frontend and Storing in MongoDB

1. Update the server-side code to handle POST requests:

   ```javascript
   // server.js
   app.post('/api/items', async (req, res) => {
     try {
       const newItem = new Item(req.body);
       await newItem.save();
       res.status(201).json(newItem);
     } catch (error) {
       res.status(500).json({ error: 'Error saving item to database' });
     }
   });

   app.get('/api/items', async (req, res) => {
     try {
       const items = await Item.find();
       res.status(200).json(items);
     } catch (error) {
       res.status(500).json({ error: 'Error retrieving items from database' });
     }
   });
   ```

2. In the React frontend, create a form component to submit new items:

   ```javascript
   // src/components/Form.js
   const Form = () => {
     const [formData, setFormData] = useState({ name: '', description: '' });

     const handleSubmit = async (e) => {
       e.preventDefault();
       try {
         await axios.post('/api/items', formData);
       } catch (error) {
         console.error('Error saving data:', error);
       }
     };

     return (
       <form onSubmit={handleSubmit}>
         {/* Form inputs */}
         <button type="submit">Submit</button>
       </form>
     );
   };
   ```

3. Create a component to display the stored items:

   ```javascript
   // src/components/ItemList.js
   const ItemList = () => {
     const [items, setItems] = useState([]);

     useEffect(() => {
       const fetchData = async () => {
         try {
           const response = await axios.get('/api/items');
           setItems(response.data);
         } catch (error) {
           console.error('Error fetching data:', error);
         }
       };
       fetchData();
     }, []);

     return (
       <ul>
         {items.map((item) => (
           <li key={item._id}>
             {item.name} - {item.description}
           </li>
         ))}
       </ul>
     );
   };
   ```
## We can Find More detailed Version Here in my Docs:
[Mern-Notes](https://docs.google.com/document/d/1tXgiDJbzQgC7U07ZoZH7-fPg1bk5TAdRJQ8_UNK3Lrw/edit?usp=sharing)
