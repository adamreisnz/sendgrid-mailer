# sendgrid-mailer

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

## License
(MIT License)

Copyright 2016-2017, [Adam Reis](http://adam.reis.nz)
