# sendgrid-mailer

[![npm version](https://img.shields.io/npm/v/sendgrid-mailer.svg)](https://www.npmjs.com/package/sendgrid-mailer)
[![node dependencies](https://david-dm.org/adamreisnz/sendgrid-mailer.svg)](https://david-dm.org/adamreisnz/sendgrid-mailer)
[![github issues](https://img.shields.io/github/issues/adamreisnz/sendgrid-mailer.svg)](https://github.com/adamreisnz/sendgrid-mailer/issues)

## DEPRECATED
This package is now **deprecated** in favour of the new official Sendgrid library [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail). This new library, which I've worked on in collaboration with Sendgrid, follows almost the exact same API as this package. As such, it will work as a drop-in replacement with only a few minor changes needed:

```js
//Instead of:
const sendgrid = require('sendgrid-mailer');
//Use:
const sgMail = require('@sendgrid/mail');

//Instead of:
sendgrid.config(SENDGRID_API_KEY);
//Use:
sgMail.setApiKey(SENDGRID_API_KEY);

//Instead of:
sendgrid.send(emails);
//Use:
sgMail.send(emails);
//Or:
sgMail.sendMultiple(emails);
```

That's it! The structure of your emails data can remain unchanged. In addition, the new library supports all advanced API options as documented in the [API v3 documentation](https://sendgrid.com/docs/API_Reference/Web_API_v3/Mail/index.html) as well.

Please use [the new Sendgrid library](https://www.npmjs.com/package/@sendgrid/mail) going forward, as this package will **no longer be maintained or supported**.

## Introduction
A simple wrapper around the official Sendgrid library to make sending emails easy again.

This wrapper is not meant as a full blown replacement of the
[official Sendgrid library](https://www.npmjs.com/package/sendgrid). Instead,
it is meant for those use cases where you just want to send one or more emails *quickly*
with a few lines of code, without needing to use elaborate helper classes or remember the Sendgrid Request JSON structure by heart.

## Installation
Use `npm` or `yarn` to install:

```
npm install sendgrid-mailer --save
```

```
yarn add sendgrid-mailer
```

## Requirements
Written in ES6 for Node 6+, requires [sendgrid](https://www.npmjs.com/package/sendgrid) as a peer dependency.

## Usage

### Configure mailer

Configure the API key somewhere once:
```js
require('sendgrid-mailer').config(API_KEY);
```

### Simplest example
```js
//Load mailer and set API key (only needed once)
const mailer = require('sendgrid-mailer').config(API_KEY);

//Create email data
const email = {
  to: 'a@example.org',
  from: 'b@example.org',
  subject: 'Hello world',
  text: 'Hello plain world!',
  html: '<p>Hello HTML world!</p>',
};

//Send away
mailer.send(email); //Returns promise
```

### Further details

The `send` method returns a `Promise`, so you can handle success and capture errors:

```js
mailer.send(email)
  .then(() => {
    //Celebrate
  })
  .catch(error => {
    //Do something with the error
  });
```

It accepts both a single email or an array of emails (sent individually):

```js
//Construct an array of emails
const emails = [email1, email2, email3];

//Send them all
mailer.send(emails);
```

It also accepts Sendgrid helper `Mail` instances:

```js
//Load helper class
const Mail = require('sendgrid').mail.Mail;

//Instantiate
const sgMail = new Mail();
//...compose email...

//Send using mailer
mailer.send(sgMail);
```

The `to` and `from` fields are flexible:

```js
const email = {

  //Simple email address string
  to: 'someone@example.org',

  //Email address with name
  to: 'Some One <someone@example.org>',

  //Object with name/email
  to: {
    name: 'Some One',
    email: 'someone@example.org',
  },

  //A Sendgrid Email instance
  to: new Email('someone@example.org', 'Some One'),

  //Or an array of emails
  to: ['someone@example.org', 'else@example.org'],
};
```

You can provide a reply-to address:

```js
const email = {
  replyTo: 'no-reply@example.org',
}
```

You can provide a template ID and substitutions:

```js
const email = {
  templateId: 'sendgrid-template-id',
  substitutions: {
    '{{name}}': 'Some One',
    '{{id}}': '123',
  },
}
```

Or an array of substitutions if you have multiple recipients:

```js
const email = {
  templateId: 'sendgrid-template-id',
  to: ['someone@example.org', 'else@example.org'],
  substitutions: [
    {
      '{{name}}': 'Some One',
      '{{id}}': '123',
    },
    {
      '{{name}}': 'Else',
      '{{id}}': '456',
    },
  ],
}
```

If needed, you can get direct access to the underlying Sendgrid instance:

```js
//Do stuff with the Sendgrid instance
const emptyRequest = mailer.sg.emptyRequest();
```

You can use the mailer to quickly create Sendgrid `Mail` instances:

```js
//Create email data
const email = {
  to: 'someone@example.org',
  from: 'else@example.org',
  subject: 'Hello world',
  text: 'Hello plain world!',
  html: '<p>Hello HTML world!</p>',
};

//Create a Sendgrid `Mail` instance
const sgMail = mailer.createMail(email);
```

Or `Email` identity instances:

```js
//Create a Sendgrid `Email` instance
const sgEmail = mailer.createEmail('Some One <someone@example.org>');
```

Or even to create Sendgrid `Request` instances so you can send them yourself for more control:

```js
//Create a Sendgrid `Request` instance
const sgRequest = mailer.createRequest(email);

//Send it using the underlying API
mailer.sg.API(request);
```

Lastly, you can overwrite the promise implementation you want the mailer to use. Defaults to ES6 `Promise`:

```js
mailer.Promise = require('bluebird');
```

### Contributing
Pull requests or suggestions for improvement welcome!

## License
(MIT License)

Copyright 2016-2017, [Adam Reis](http://adam.reis.nz)
