# Express/Sequelize Checkpoint-Study Guide Notes

## Models

* Creating a db instance with *new Sequelize*
```javascript
var Sequelize = require('sequelize');
var db = new Sequelize('postgres://localhost:5432/wikistack', { logging: false });
```

* Creating models with db.define(modelname, fields, options)
  * Specifying schema fields(attributes)
  * Specifying attribute types, e.g. Sequelize.STRING
  * Specifying attribute validations, e.g. allowNull
  ```javascript
  var User = db.define('user', {
    name: {
        type: Sequelize.STRING,
        allowNull: false
    },
    email: {
        type: Sequelize.STRING,
        allowNull: false,
        unique: true,
        validate: {
            isEmail: true
        }
    }
});
  ```
  * Specifying attribute defaultValues
  * Specifying model options
   * Getters & Setters (aka virtuals)
   * Hooks, e.g. beforeValidate
   * Class methods
   * Instance methods
   * *this* value in custom methods
     * getters: the instance
     * hooks: the model (instance is 1st arg of the hook func)
     * instance methods: instance
     * Class methods: class
 * Associating models, e.g. hasOne, belongsTo, etc
  * Which model has the foreignKey
  * Which Sequelize model is given new methods
 * Synchronizing models with db.sync() -- what does the option force: true do?



## Routes

* app.use vs app.all vs app.get vs app.post vs app.put vs app.delete
* How to interact with data from the request
 * req.param vs req.query vs req.body (URI match vs ? that Express parses vs body-parser, respectively)
 * Using a model within a route to query, creations, updates or deletions
* Model.create, instance.destroy, etc.
* More complex queries
 * Using $in, $ne, and other operators
 * Using Eager Loading (the include syntax) to do joins (i.e. populate assocations)
* Handling asynchronicity (from Sequelize methods, but really any promises)
 * Using next with error handling
 * Sending a response once the async data is fetched
 * Error handling middleware (4 parameters - error, request, response, next)
