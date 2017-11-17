# Express/Sequelize Checkpoint-Study Guide

## Models

* Creating a db instance with *new Sequelize*
* Creating models with db.define(modelname, fields, options)
  * Specifying schema fields(attributes)
  * Specifying attribute types, e.g. Sequelize.STRING
  * Specifying attribute validations, e.g. allowNull
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
