# Code

## Snowflake


### Latest Event Record

```sql
SELECT * FROM DATABASE1.SCHEMA1.TABLE1
QUALIFY ROW_NUMBER() OVER (PARTITION BY MID ORDER BY EVENT_DATE DESC) = 1
```

### Window Functions

```sql

```

### Column Functions

```sql
ALTER TABLE DATABASE1.SCHEMA1.TABLE1
ADD COLUMN LAST_UPDATED_TIMESTAMP_TZ TIMESTAMP_TZ AS TIMESTAMP_TZ_FROM_PARTS(
    YEAR(LAST_UPDATED_TIMESTAMP),
    MONTH(LAST_UPDATED_TIMESTAMP),
    DAY(LAST_UPDATED_TIMESTAMP),
    HOUR(LAST_UPDATED_TIMESTAMP),
    MINUTE(LAST_UPDATED_TIMESTAMP),
    SECOND(LAST_UPDATED_TIMESTAMP),
    DATE_PART(NANOSECOND, LAST_UPDATED_TIMESTAMP),
    'Australia/Sydney'
);

ALTER TABLE DATABASE1.SCHEMA1.TABLE2
ADD COLUMN LAST_UPDATED_TIMESTAMP_EPOCH_MILLIS NUMBER(38, 0) AS DATE_PART('EPOCH_MILLISECOND', LAST_UPDATED_TIMESTAMP_TZ);
```

## CSS

### Dark Mode

```css
@media (prefers-color-scheme: dark) { }
```

### CSS3

```css
// 700ms to change width when element with “tip” class changes width
.tip { transition: width 700ms; }

// 5 seconds to run through notify keyframe. Remove then add .tip class again
.tip { animation: notify 5s; }

@keyframes notify {
    from { height: 0; }
    20% { height: 60px; }
    80% { height: 60px; }
    to { height: 0; }
}
```

### Flexbox

```css
.container {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
}

.item {
  order: 1;
  flex-grow: 2;
}
```

## Javascript

### Extension methods

startsWith for string object

```javascript
String.prototype.startsWith = function (value) {
    return this.indexOf(value) != -1;
};
```

### Grunt.js

```javascript
grunt.registerTask 'adel', -> grunt.log.write 'Hello world!'
> grunt adel
```

### IIFE and Binding

```javascript
var iifeApproach = function () {
  for (var i = 0; i < 3; i++) {
    (function (it) {
      setImmediate(function () {
        console.log('iife approach:', it);
      });
    })(i);
  }
};

var bindApproach = function () {
  for (var i = 0; i < 3; i++) {
    setImmediate(function (it) {
      console.log('bind approach:', it);
    }.bind(null, i));
  }
};
```

### Require.js

```javascript
// decorator.js

define(['require', 'jquery'], function (require, $) {
return {
    decoratorProperty: "Hello World",
    decoratorFunction: function (parameter) {
        alert(parameter);
    }
}
});

// main.js

require(['jquery', 'decorator'], function ($, decorator) {
var d = decorator.decoratorProperty;
decorator.decoratorFunction("Hello World");
});
```

### Promises

```javascript
var promiseA = Promise.resolve('success');
var promiseB = Promise.reject('fail');

promiseA.then((result) => { return result; });
promiseA.then((result) => { return result; });
promiseB.catch((result) => { return result; });

promiseA
    .then((result) => { return result; })
    .then((result) => { return result; })
    .catch((result) => { return result; });

var promises = Promise.all[promiseA, promiseB];
promises.then((result) => { return result; });

var promises = Promise.race[promiseA, promiseB];
promises.then((result) => { return result; });
```

## Authentication

```javascript
OAuth = require('oauth20-provider');
oAuth = new OAuth({log: {level: 2}});
server.use(oAuth.inject());
server.post('/token', oAuth.controller.token);
server.post('/authorization', isAuthorized, oAuth.controller.authorization);
server.get('/authorization', isAuthorized, oAuth.controller.authorization, function(req, res) {
    res.render('authorization', {layout: false});
});
function isAuthorized(req, res, next) {
    if (req.session.authorized) next();
    else {
        var params = req.query;
        params.backUrl = req.path;
        res.redirect('/login?' + query.stringify(params));
    }
};
```

## PowerShell

```powershell
> Get-Command | Get-Member
> Get-Process -Name chrome
> Get-Process | Get-Member -Name "wait*"
> Get-Service | Where-Object { $_.Status -eq "Running" -and $_.Name -like "MS*" }
> $note = Get-Process -Name notepad
> Remove-Variable note
> powershell -Command "& { cd C:\Work\project; ./go Prepare; echo ‘hello’; }"
```
