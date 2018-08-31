# WAX ExpressTrade Tutorial
# Youtube tutorial video (step-by-step) here: www.youtube.com/watch?v=1742zi5Pxh4
# Or you can follow here on github (step-by-step) tutorial below:

## 1. Create the server

Create a new VPS droplet in DigitalOcean (you can use my affiliate link to receive $10 FREE: https://m.do.co/c/1fb7d9628800)

Create a VPS with OS: Ubuntu 14.04.5 x64.

The password will be sent through your e-mail/gmail registered to your DigitalOcean.

## 2. Open PuTTy

Download PuTTy from the link: (https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

Open PuTTy server. Login with your IP Address and with the password from your e-mail/gmail.

Then you will be input to write your old password (from e-mail/gmail) and then asked to write your new password.

After the new password has been created, write these following commands:

```shell
apt-get update
```

```shell
apt-get install apache2 mysql-server phpmyadmin php5
```

## 3. Open FileZilla (File Server Browser)

Download FileZilla Client: https://filezilla-project.org/download.php?type=client and open it.

Connect to the server by:

* `Host` IP address of server
* `User` root
* `Pass` The password of VPS
* `Port` 22

In `/root` folder create folder  `/contest` and inside the folder, create a file called **server.js**

## 4. Return to PuTTy

To install `nodejs` we will write this command in PuTTy: `curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -`

After the `nodejs` got all files, install by this command: `sudo apt-get install -y nodejs`

## 5. Create the variables on `server.js` file

Open `server.js` file and write following variables:

```javascript
var app = require('https').createServer();
var io = require('socket.io')(app);
var fs = require('fs');

app.listen (8080);
```

## 6. Now we're creating the registering of tradeurl on our server

Open `server.js` file and write following variables and functions:

```javascript
var users = {};

io.on('connection', function(socket) {
    socket.on('connected', function(tradeurl) {
        users.push(tradeurl);
        socket.emit('message', "You have registered your UID to our website!");
    });
});
```

## 7. Creating the `index.php` file.

Return to FileZilla, go to folder `/var/www/html` and inside the folder make a file called **index.php**

## 8. Adding code to `index.php` file

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>WAX ExpressTrade - Echo Tutorial</title>
    <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css">
</head>
<body>
    <h1>WAX ExpressTrade - Echo Tutorial</h1>
    <div class="settings">
        <imput type="text" id="your_tradeurl" placeholder="your trade url">
        <button type="button" id="registerBtn">Register</button>
    </div>
</body>
</html>
```
Add now the `script` tags:

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>WAX ExpressTrade - Echo Tutorial</title>
</head>
<body>
    <h1>WAX ExpressTrade - Echo Tutorial</h1>
    <div class="settings">
        <input type="text" id="your_tradeurl" placeholder="your trade url">
        <button type="button" id="registerBtn">Register</button>
    </div>
</body>
</html>

<script src="https://code.jquery.com/jquery-3.3.1.js" integrity: "sha384-oqVuAfHRKap7fdgcCY5iykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC" crossorigin= "@guiolmar"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/1.4.5/socket.io.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>
```

Now add the `script` tags for `socket.io` connection and some user functions:

```php
<script>
    var socket = null;

    if(socket == null) {
        socket = io('yourip:8080')                  // EXAMPLE: 123.123.123.0:8080

        socket.on('connect', function() {
            toastr.success('Connected to the server!')

            user_functions();
        });
    }
    
    function user_function() {
        $('#registerBtn').click(function() {
            var $tradeurl = $('#your_tradeurl').val();

            if($tradeurl.includes('https://') && $tradeurl.includes('trade.opskins.com')) {
                socket.emit('register', $tradeurl);
                socket.emit('user inventory', $tradeurl);
            } else toastr.error('Wrong format of trade url!');
        });
    }
```
        
## 9. Installing module dependencies for `NodeJs`

Write the following commands on PuTTy to install the module dependencies:

* `cd /root/contest` - *you will get to the folder where `server.js` is*
* `npm install expresstrade socket.io@1.7.3 fs` - *you will install the npm modules*

After the npm modules were installed, start the `server.js` with the command `node server.js`.

## 10. Editing the `server.js`.

From old `server.js` to this `server.js`:

```javascript
var users = {};

io.on('connection', function(socket) {
    socket.on('connected', function(tradeurl) {
        users[tradeurl.split('/')[4]] = tradeurl;
        socket.emit('message', "You are now registered to the website!")
    });
});
```

## 11. Adding new scripts to `index.php` file

The *index.php* file will look like this:

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>WAX ExpressTrade - Echo Tutorial</title>
    <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css">
</head>
<body>
    <h1> WAX ExpressTrade - Echo Tutorial
    <div class="settings">
        <imput type="text" id="your_tradeurl" placeholder="your trade url">
        <button type="button" id="registerBtn">Register</button>
    </div>
</body>
</html>

<script src="https://code.jquery.com/jquery-3.3.1.js" integrity: "sha384-oqVuAfHRKap7fdgcCY5iykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC" crossorigin= "@guiolmar"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/1.4.5/socket.io.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>


<script>
    var socket = null;

    if(socket == null) {
        socket = io('yourip:8080')

        socket.on('connect', function() {
            toastr.success('Connected to server!')

            user_functions();
        });
    }

    function user_function() {
        $('#registerBtn').click(function() {
            var $tradeurl = $('#your_tradeurl').val();

            if($tradeurl.includes('https://') && $tradeurl.includes('trade.opskins.com')) {
                socket.emit('register', $tradeurl);
                socket.emit('user inventory', $tradeurl);
            } else toastr.error('Wrong format of trade url!');
        });
    }
```
Now you can test the tradeurl thing by stopping and starting again your `server.js` with the following commands: `CTRL+C` *to stop your current server* and `node server.js` *to start again the server.js file*.

## 12. Update the global variables from `server.js` file

The server.js will look like this:

```javascript
var users = {};

io.on('connection', function(socket) {
    socket.on('connected', function(tradeurl) {
        if(users.hasOwnProperty(tradeurl.split('/')[4])) return socket.emit('message', 'You have already registered your UID to our website!');
        users[tradeurl.split('/')[4]] = tradeurl;
        socket.emit('message', 'You have registered your UID to our website!');
    });
});
```

## 13. Time to work with endpoints from https://github.com/OPSkins/trade-opskins-api

To work with OPSkins endpoints we will use this module, so it will be easy to use https://github.com/TheTimmaeh/node-expresstrade

The `server.js` will look like this:

```javascript
var app = require('https').createServer();
var io = require('socket.io')(app);
var fs = require('fs');

var ExpressTrade = require('expresstrade');

var ET = new ExpressTrade({
    apikey: 'Your OPSkins API Key',
    twofactorsecret: 'Your OPSkins 2FA Secret',
    pollInterval: 5000
  })

app.listen(8080);
```

*Your OPSkins API Key* can be found on `https://opskins.com > Account Settings > Advanced Options > API Key`.

*Your OPSkins 2FA Secret* can be found at enabling your 2FA on `https://opskins.com` or you can simply re-enable it from `https://opskins.com > Account Settings > Account Security > Disable Two-Factor Authentication`, then when you re-enable the 2FA, below the *QR image scanner* you will find a secret code like this `"HGPS SHFO HS6A G2U7"`.

After everything is done, it will look like this:

```javascript
var app = require('https').createServer();
var io = require('socket.io')(app);
var fs = require('fs');

var ExpressTrade = require('expresstrade');

var ET = new ExpressTrade({
    apikey: '92nsns01hw0sn101bsiidas',         // EXAMPLE of apikey
    twofactorsecret: '29SN2ISN2JSN2SF',                // EXAMPLE of twofactorsecret
    pollInterval: 5000
  })

app.listen(8080);
```

The variable `ET` contains confidential data, do not give to others, otherwise they can steal your items from bot.

## 14. Let's create the bot functions

This will be the `server.js`:

```javascript
var users = {};

io.on('connection', function(socket) {
    socket.on('logged', function() {
        botInventory(function(items) {
            socket.emit('bot inventory', items)
        });
    });

    socket.on('register', function(tradeurl) {
        if(users.hasOwnProperty(tradeurl.split('/')[4])) return socket.emit('message', 'You have already registered your UID to our website!');
        users[tradeurl.split('/')[4]] = tradeurl;
        socket.emit('message', 'You have registered your UID to our website!');
    });
});

function botInventory(callback) {
    ET.IUser.GetInventory({
        app_id: 1
    }
        (err, body) => {
        if(err) console.log(err);

        callback(body.responde.items);
    });
}
```

Now return to `index.php` and replace with this:

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>WAX ExpressTrade - Echo Tutorial</title>
    <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css">
</head>
<body>
    <h1>WAX ExpressTrade - Echo Tutorial</h1>
    <div class="settings">
        <imput type="text" id="your_tradeurl" placeholder="your trade url">
        <button type="button" id="registerBtn">Register</button>
    </div>
    <div class='bot invemtory'>
            
    </div>
</body>
</html>
```
And the `scripts` section will look like this:

```php
<script src="https://code.jquery.com/jquery-3.3.1.js" integrity: "sha384-oqVuAfHRKap7fdgcCY5iykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC" crossorigin= "@guiolmar"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/1.4.5/socket.io.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>


<script>
    var socket = null;

    if(socket == null) {
        socket = io('yourip:8080')

        socket.on('connect', function() {
            toastr.success('Connected to server!')

            socket.emit('logged');

            user_functions();
        });
    }

    function user_functions() {
        $('#registerBtn').click(function() {
            var $tradeurl = $('#your_tradeurl').val();

            if($tradeurl.includes('https://') && $tradeurl.includes('trade.opskins.com')) {
                socket.emit('register', $tradeurl);
                socket.emit('user inventory', $tradeurl);
            } else toastr.error('Wrong format of trade url!');
        });

        socket.on('message', function(msg) {
            toastr.info(msg);
        });
        socket.on('bot inventory', function(items) {
            for(var i in items) {
                $('.bot_inventory').append(`
                    <div class="item" data-id="` + items[i].id + `">
                        <ing src="` + items[i].image['100px'] + `"> 
                        ` + items[i].name + `<br>
                        $` + parseFloat(items[i].suggested_price_floor/100).toFixed(2) + '
                    </div>            
                ')
            }
        });
    }
```
Now if you restart the `server.js` file you can see the bot inventory!

## 15. Now we will create the withdraw button

In `index.php` we need to edit:

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>WAX ExpressTrade - Echo Tutorial</title>
    <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css">
</head>
<body>
    <h1>WAX ExpressTrade - Echo Tutorial</h1>
    <div class="settings">
        <imput type="text" id="your_tradeurl" placeholder="your trade url">
        <button type="button" id="registerBtn">Register</button>
    </div>
    <div class='bot invemtory'>
            
    </div>

    <button type="button" id="withdrawItems">Withdraw</button>
    <span class=items></span>
</body>
</html>
```


## 16. Now add styles to bot inventory and items

Add style on **index.php** file:

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>WAX ExpressTrade - Echo Tutorial</title>
    <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css">

    <style>
        .bot_inventory {
            margin-top: 100px;
            display: flex;
        }

        .item {
            cursor: pointer;
            width: 100px;
            background-color: black;
            color: white;
            margin-right: 10px;
        }

        .item img {
            width: 100px;
        } 

        .items {
            margin-top: 100px;
            display: flex;
        }
    </style>

</head>
<body>
    <h1>WAX ExpressTrade - Echo Tutorial</h1>
    <div class="settings">
        <imput type="text" id="your_tradeurl" placeholder="your trade url">
        <button type="button" id="registerBtn">Register</button>
    </div>
    <div class='bot invemtory'>
            
    </div>
    
    <button type="button" id="withdrawItems">Withdraw</button>
    <div class=items></span>
</body>
</html>
```

The `scripts` section will look like this:

```php
<script>
    var withdraw_items = [];
    var socket = null;

    if(socket == null) {
        socket = io('yourip:8080')

        socket.on('connect', function() {
            toastr.success('Connected to server!')

            socket.emit('logged');

            user_functions();
        });
    }
    function user_functions() {
        $('body').on('click', '.bot_inventory .items', function() {
            var &id = $(this).attr('data-id';)

            var $item = $(this).html();
            $(this).remove();

            withdraw.items.push($id);

            $('.items').append(`
                <div class="item" data-id="` + $id + `">
                    ` + $item + `
                </div>
            `);
        });

        $('body').on('click', '.items .items', function() {
            var &id = $(this).attr('data-id';)

            var $item = $(this).html();

            $(this).remove();

            withdraw_items.splice(withdraw_items.indexDF($id), 1);

            $('.bot_inventory').append(`
                <div class="item" data-id="` + $id + `">
                    ` + $item + `
                </div>
            `);
        });

        $('#registerBtn').click(function() {
            var $tradeurl = $('#your_tradeurl').val();

            if($tradeurl.includes('https://') && $tradeurl.includes('trade.opskins.com')) {
                socket.emit('register', $tradeurl);
                socket.emit('user inventory', $tradeurl);
            } else toastr.error('Wrong format of trade url!');
        });


        socket.on('message', function(msg) {
            toastr.info(msg);
        });

        socket.on('bot inventory', function(items) {
            for(var i in items) {
                $('.bot_inventory').append(`
                    <div class="item" data-id="` + items[i].id + `">
                        <ing src="` + items[i].image['100px'] + `"> 
                        ` + items[i].name + `<br>
                        $` + parseFloat(items[i].suggested_price_floor/100).toFixed(2) + '
                    </div>            
                ')
            }
        });
    }
```

## 17. Now let's create the withdraw function

Code with the withdraw function done:
**server.js**
```javascript
var app = require('https').createServer();
var io = require('socket.io')(app);
var fs = require('fs');

var ExpressTrade = require('expresstrade');

var ET = new ExpressTrade({
    apikey: 'Your OPSkins API Key',
    twofactorsecret: 'Your OPSkins 2FA Secret',
    pollInterval: 5000
  })

app.listen(8080);

//global
var users = {};

io.on('connection', function(socket) {
    socket.on('logged', function() {
        botInventory(function(items) {
            socket.emit('bot inventory', items)
        });
    });

    socket.on('register', function(tradeurl) {
        if(users.hasOwnProperty(tradeurl.split('/')[4])) return socket.emit('message', 'You have already registered your UID to our website!');
        users[tradeurl.split('/')[4]] = tradeurl;
        socket.emit('message', 'You have registered your UID to our website!');
    });

    socket.on('withdraw items', function(items, tradeurl) {
        if(tradeurl == '') return socket.emit('message', 'You need to input your tradelink!');
        if(items.length == 0) return socket.emit('message', 'You need to select some items to withdraw!');

        ET.ITrade.SendOffer({
            trade_url: tradeurl,
            items: items.join(','),
            message: 'This is a trade offer from the ExpressTrade website contest by Echo (withdraw).'
        }, (err, body) => {
            if(err) {
                socket.emit('message', err);
                return;
            }

            if(!body.hasOwnProperty('response') && body.hasOwnProperty('message')) return socket.emit('message', body.message);

            if(body.response.offer.state == 2) {
                socket.emit('trade offer', body.response.offer.id, 'withdraw');
            }

        });
    });
});

function botInventory(callback) {
    ET.IUser.GetInventory({
        app_id: 1
    }
        (err, body) => {
        if(err) console.log(err);

        callback(body.responde.items);
    });
}
```

**index.php**

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>WAX ExpressTrade - Echo Tutorial</title>
    <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css">

    <style>
        .bot_inventory {
            margin-top: 100px;
            display: flex;
        }

        .item {
            cursor: pointer;
            width: 100px;
            background-color: black;
            color: white;
            margin-right: 10px;
        }

        .item img {
            width: 100px;
        } 

        .items {
            margin-top: 100px;
            display: flex;
        }
    </style>

</head>
<body>
    <h1>WAX ExpressTrade - Echo Tutorial</h1>
    <div class="settings">
        <imput type="text" id="your_tradeurl" placeholder="your trade url">
        <button type="button" id="registerBtn">Register</button>
    </div>
    <div class='bot invemtory'>
            
    </div>
    <button type="button" id="withdrawItems">Withdraw</button>
    <div class=items></span>

    <br></br>
    
        <h4>Your history trade</h4>
    <div class='trade'>

    </div>
</body>
</html>

<script src="https://code.jquery.com/jquery-3.3.1.js" integrity: "sha384-oqVuAfHRKap7fdgcCY5iykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC" crossorigin= "@guiolmar"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/1.4.5/socket.io.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>
<script>
    var withdraw_items = [];
    var socket = null;

    if(socket == null) {
        socket = io('yourip:8080')

        socket.on('connect', function() {
            toastr.success('Connected to server!')

            socket.emit('logged');

            user_functions();
        });
    }

    function user_functions() {
        $('body').on('click', '.bot_inventory .items', function() {
            var &id = $(this).attr('data-id';)

            var $item = $(this).html();
            $(this).remove();

            withdraw.items.push($id);

            $('.items').append(`
                <div class="item" data-id="` + $id + `">
                    ` + $item + `
                </div>
            `);
        });

        $('body').on('click', '.items .items', function() {
            var &id = $(this).attr('data-id';)

            var $item = $(this).html();

            $(this).remove();

            withdraw_items.splice(withdraw_items.indexDF($id), 1);

            $('.bot_inventory').append(`
                <div class="item" data-id="` + $id + `">
                    ` + $item + `
                </div>
            `);
        });

        $('#registerBtn').click(function() {
            var $tradeurl = $('#your_tradeurl').val();

            if($tradeurl.includes('https://') && $tradeurl.includes('trade.opskins.com')) {
                socket.emit('register', $tradeurl);
                socket.emit('user inventory', $tradeurl);
            } else toastr.error('Wrong format of trade url!');
        });
        
        $('#withdrawItems').click(function() {
            socket.emit('withdraw items', withdraw_items, $('#your_tradeurl').val());
        
        });

        socket.on('message', function(msg) {
            toastr.info(msg);
        });

        socket.on('tarde offer', function(tid) {
            $('.trade').append(`
                <span class color: purple; font-weight: bold; "Trade #` + tid + ` has been created!</span>
                <a href="https://trade.opskins.com/trade-offers/` + tid + `" target=" blank">click here to accept</a>
            `);
        });

        socket.on('bot inventory', function(items) {
            for(var i in items) {
                $('.bot_inventory').append(`
                    <div class="item" data-id="` + items[i].id + `">
                        <ing src="` + items[i].image['100px'] + `"> 
                        ` + items[i].name + `<br>
                        $` + parseFloat(items[i].suggested_price_floor/100).toFixed(2) + '
                    </div>            
                ')
            }
        });
    }
```

Now restart the `server.js` and you will see you can withdraw items from bot inventory!

## 18. Now let's create the deposit button and functions

Edit the `index.php` file:

The new code with new `index.php` file with deposit functions and button:
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>WAX ExpressTrade - Echo Tutorial</title>
    <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css">

    <style>
        .bot_inventory {
            margin-top: 100px;
            display: flex;
        }

        .user_inventory {
            margin-top: 100px;
            display: flex;
        }

        .item {
            cursor: pointer;
            width: 100px;
            background-color: black;
            color: white;
            margin-right: 10px;
        }

        .item img {
            width: 100px;
        }

        .items {
            margin-top: 100px;
            display: flex;
        }
    </style>

</head>
<body>
    <h1>WAX ExpressTrade - Echo Tutorial</h1>
    <div class="settings">
        <input type="text" id="your_tradeurl" placeholder="your trade url">
        <button type="button" id="registerBtn">Register</button>
    </div>
<br>

    <div class="user_inventory">

    </div><br>
    <button type="button" id="depositItems">Deposit</button>
    <div class="items2"></div>

    <br><br>
    <div class="bot_inventory">

    </div>
    <button type="button" id="withdrawItems">Withdraw</button>
    <div class="items"></div>

    <br><br>

        <h4>Your trades here</h4>
    <div class="trade">

    </div>
</body>
</html>

<script src="https://code.jquery.com/jquery-3.3.1.js" integrity="sha256-2Kok7MbOyxpgUVvAk/HJ2jigOSYS2auK4Pfzbm7uH60=" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/1.4.5/socket.io.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>


<script>
    var withdraw_items = [];
    var deposit_items = [];
    var socket = null;

    if(socket == null) {
        socket = io('yourip:8080');

        socket.on('connect', function() {
            toastr.success('Connected to server!');

            socket.emit('logged');

            user_functions();
        });
    }

    function user_functions() {

        $('body').on('click', '.bot_inventory .item', function() {
            var $id = $(this).attr('data-id');

            var $item = $(this).html();
            $(this).remove();

            withdraw_items.push($id);

            $('.items').append(`
                <div class="item" data-id="` + $id + `">
                    ` + $item + `
                </div>
            `);
        });

        $('body').on('click', '.items .item', function() {
            var $id = $(this).attr('data-id');

            var $item = $(this).html();

            $(this).remove();

            withdraw_items.splice(withdraw_items.indexOf($id), 1);

            $('.bot_inventory').append(`
                <div class="item" data-id="` + $id + `">
                    ` + $item + `
                </div>
            `);
        });

        $('#registerBtn').click(function() {
            var $tradeurl = $('#your_tradeurl').val();

            if($tradeurl.includes('https://') && $tradeurl.includes('trade.opskins.com')) {
                socket.emit('register', $tradeurl);
                socket.emit('user inventory', $tradeurl);
            } else toastr.error('Wrong format of trade url!');
        });

        socket.on('user inventory', function(items) {
            for(var i in items) {
                $('.user_inventory').append(`
                    <div class="item" data-id="` + items[i].id + `">
                        <img src="` + items[i].image["600px"] + `">
                        ` + items[i].name + `<br>
                        $` + parseFloat(items[i].suggested_price_floor/100).toFixed(2) + `
                    </div>
                `)
            }
        });

        $('body').on('click', '.user_inventory .item', function() {
            var $id = $(this).attr('data-id');

            var $item = $(this).html();
            $(this).remove();

            deposit_items.push($id);

            $('.items2').append(`
                <div class="item" data-id="` + $id + `">
                    ` + $item + `
                </div>
            `);
        });

        $('body').on('click', '.items2 .item', function() {
            var $id = $(this).attr('data-id');

            var $item = $(this).html();

            $(this).remove();

            deposit_items.splice(deposit_items.indexOf($id), 1);

            $('.user_inventory').append(`
                <div class="item" data-id="` + $id + `">
                    ` + $item + `
                </div>
            `);
        });

        $('#withdrawItems').click(function() {
            socket.emit('withdraw items', withdraw_items, $('#your_tradeurl').val());
        });

        $('#depositItems').click(function() {
            socket.emit('deposit items', deposit_items, $('#your_tradeurl').val());
        });

        socket.on('message', function(msg) {
            toastr.info(msg);
        });

        socket.on('trade offer', function(tid, type) {
            $('.trade').append(`
                <span class="color: orange; font-weight: bold;">Trade #` +  tid + ` for ` + type + ` has been created!</span>
                <a href="https://trade.opskins.com/trade-offers/` + tid + `" target="_blank">click here to accept</a><br>
            `);
        });

        socket.on('bot inventory', function(items) {
            for(var i in items) {
                $('.bot_inventory').append(`
                    <div class="item" data-id="` + items[i].id + `">
                        <img src="` + items[i].image["600px"] + `">
                        ` + items[i].name + `<br>
                        $` + parseFloat(items[i].suggested_price_floor/100).toFixed(2) + `
                    </div>
                `)
            }
        });
    }
</script> 
```

And the `server.js` with the deposit functions

```javascript
var app = require('http').createServer();
var io = require('socket.io')(app);
var fs = require('fs');

var ExpressTrade = require('expresstrade');

var ET = new ExpressTrade({
    apikey: 'Your OPSkins API Key',
    twofactorsecret: 'Your OPSkins 2FA Secret',
    pollInterval: 5000
  });

app.listen(8080);

// global variables
var users = {};

io.on('connection', function(socket) {
    socket.on('logged', function() {
        botInventory(function(items) {
            socket.emit('bot inventory', items);
        });
    });


    socket.on('register', function(tradeurl) {
        if(users.hasOwnProperty(tradeurl.split('/')[4])) return socket.emit('message', 'You have already registered your UID to our website!');
        users[tradeurl.split('/')[4]] = tradeurl;
        socket.emit('message', 'You have registered your UID to our website!');
    });

    socket.on('withdraw items', function(items, tradeurl) {
        if(tradeurl == '') return socket.emit('message', 'You need to input your tradelink!');
        if(items.length == 0) return socket.emit('message', 'You need to select some items to withdraw!');

        ET.ITrade.SendOffer({
            trade_url: tradeurl,
            items: items.join(','),
            message: 'This is a trade offer from the ExpressTrade website contest by Echo (withdraw).'
        }, (err, body) => {
            if(err) {
                socket.emit('message', err);
                return;
            }

            if(!body.hasOwnProperty('response') && body.hasOwnProperty('message')) return socket.emit('message', body.message);

            if(body.response.offer.state == 2) {
                socket.emit('trade offer', body.response.offer.id, 'withdraw');
            }

        });
    });

    socket.on('deposit items', function(items, tradeurl) {
        if(tradeurl == '') return socket.emit('message', 'You need to input your tradelink!');
        if(items.length == 0) return socket.emit('message', 'You need to select some items to deposit!');

        ET.ITrade.SendOffer({
            trade_url: tradeurl,
            items: items.join(','),
            message: 'This is a trade offer from the ExpressTrade website contest by Echo (deposit).'
        }, (err, body) => {
            if(err) {
                socket.emit('message', err);
                return;
            }

            if(!body.hasOwnProperty('response') && body.hasOwnProperty('message')) return socket.emit('message', body.message);

            if(body.response.offer.state == 2) {
                socket.emit('trade offer', body.response.offer.id, 'deposit');
            }

        });
    });

    socket.on('user inventory', function(tradeurl) {
        userInventory(tradeurl.split('/')[4], function(items) {
            socket.emit('user inventory', items);
        });
    });
});

function botInventory(callback) {
    ET.IUser.GetInventory({
        app_id: 1
    },
        (err, body) => {
        if(err) return console.log(err);

        callback(body.response.items);
    });
}

function userInventory(uid, callback) {
    ET.ITrade.GetUserInventory({
        app_id: 1,
        uid: uid
    }, (err, body) => {

        if(err) return console.log(err);

        callback(body.response.items);
    });
}
```
If you get ``ERR PORT IN USE``, write the following commands: `1. killall node`, `2. node server.js`.

## Tutorial made by Echo
## Twitter @AlaDyn172
