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
  http://docs.sequelizejs.com/variable/index.html
  * Specifying model options
  http://docs.sequelizejs.com/manual/installation/getting-started.html#application-wide-model-options

   * Getters & Setters (aka virtuals)
       * _Getters/Setters_
   http://docs.sequelizejs.com/manual/tutorial/models-definition.html#getters-setters
       *  _Virtuals_
  http://docs.sequelizejs.com/variable/index.html#static-variable-DataTypes
```javascript
    route: {
        type: Sequelize.VIRTUAL,
        get () {
            return '/wiki/' + this.getDataValue('urlTitle')
        }
    },
    renderedContent: {
        type: Sequelize.VIRTUAL,
        get () {
            return marked(this.getDataValue('content'))
        }
    }
})
```
   * Hooks, e.g. beforeValidate
  http://docs.sequelizejs.com/manual/tutorial/hooks.html#hooks
   ```javascript
   Page.beforeValidate(function (page) {
  if (page.title) {
    page.urlTitle = page.title.replace(/\s+/g, '_').replace(/\W/g, '');
  }
})
   ```
   * Class methods
   * Instance methods

### _This is an instance method..._
```javascript
const thing = new Thing('fred');
thing.jump();
```
### _This is a class method..._
```javascript
const thing = new Thing('george');
Thing.findByName('george');
```
### _This is how we define an instance method..._
```javascript
function Thing (name) {
  this.name = name;
}
Thing.prototype.jump = function () {
  this.status = 'happy';
};
```
   * *this* value in custom methods
     * getters: the instance
     * hooks: the model (instance is 1st arg of the hook func)
     * instance methods: instance
     * Class methods: class
 * Associating models, e.g. hasOne, belongsTo, etc
 http://docs.sequelizejs.com/manual/tutorial/associations.html
  * Which model has the foreignKey
  http://docs.sequelizejs.com/manual/advanced/legacy.html#foreign-keys
  * Which Sequelize model is given new methods
 * Synchronizing models with db.sync() -- what does the option force: true do?
     *Force true clears the database*
 ```javascript
 User.sync()
    .then(function () {
        return Page.sync();
    })
    .then(function () {
        app.listen(3001, function () {
            console.log('Server is listening on port 3001!');
        });
    });
 ```




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
