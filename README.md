# mongoose-private-paths [![Build Status](https://travis-ci.org/yamadapc/mongoose-private-paths.png?branch=master)](https://travis-ci.org/yamadapc/mongoose-private-paths)

A simple mongoose plugin to provide private Schema keys

## Getting Started
Install the module with: `npm install mongoose-private-paths`

```javascript
var mongoose = require('mongoose'),
    privatePaths = require('mongoose-private-paths');

var TestSchema = new mongoose.Schema({
  public:       { type: String },
  _private:     { type: String },
  also_private: { type: String, private: true },
  not_private:  { type: String, private: false },
  _even:        { type: String, private: false },
  works_too:   [{ type: String, private: false }]
});

TestSchema.plugin(privatePaths);

var Test = mongoose.model('Test', Testschema);

var test = new Test({
  public:       'all keys are public by default!',
  _private:     'keys prefixed with an "_" will be private - except "_id"',
  also_private: 'stuff with a "private" field set to true will be... private!',
  not_private:  'stuff with a "private" field set to false will be... NOT private!',
  _even:        'if they are prefixed with an "_"',
  works_too:    'array-like fields should work too'
});

test.toJSON(); // =>
// {
//   _id:          '1234',
//   public:       'all keys are public by default!',
//   _not_private: 'stuff with a "private" field set to false will be... NOT private!',
//   _even:        'if they are prefixed with an "_"'
// }
```

## Documentation

As of right now there're only two options available, to be passed when calling
`Schema.plugin(privatePaths, options)`:

- **ignore** => an Array of keys to ignore (they'll all be public)
- **prefix** => a different private key prefix (to be used instead of "_")

## Contributing
Just lint and test using [Grunt](http://gruntjs.com/).

## License
Copyright (c) 2013 . Licensed under the MIT license.
