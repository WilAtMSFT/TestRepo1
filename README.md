## Battleship basic webapp tutorial: Learning the basics of javascript and express framework.

I wanted to learn some basic javascript and build a webapp. I thought a Battleship like game would be straight forward. I am a backend developer mainly working in C# and SQL, so my gaol was to not use either of those. After asking around, using the express[6] framework on node.js seemed the best approach so from there i just started digging. I started by following the tutorial at closebrace[1] but I skipped all the mongoDb stuff as I just wanted a quick intro to the framework and getting started. After that it was a bit of web searching and experimenting.

The following are the steps I used to create my Battleship app with corresponding commits. This repo alone will not be enough as it does not contain the node_modules and you need to install node/npm, but it does have the files so you can just copy them over after doing the initial express setup.

### Tutorial

First install [npm](https://www.npmjs.com/) so you can install other stuff. This will install node.js

Next install express by runing the following from the command line `npm install -g express-generator`

Create a base project by running `express BattleShip --view=jade`. This will create a folder Battleship with the express framework all setup using the jade templating engine[3]. You'll see this output on the command line.

```   create : BattleShip
   create : BattleShip/package.json
   create : BattleShip/app.js
   create : BattleShip/routes
   create : BattleShip/routes/index.js
   create : BattleShip/routes/users.js
   create : BattleShip/public
   create : BattleShip/views
   create : BattleShip/views/index.jade
   create : BattleShip/views/layout.jade
   create : BattleShip/views/error.jade
   create : BattleShip/bin
   create : BattleShip/bin/www
   create : BattleShip/public/images
   create : BattleShip/public/javascripts
   create : BattleShip/public/stylesheets
   create : BattleShip/public/stylesheets/style.css

   install dependencies:
     > cd BattleShip && npm install

   run the app:
     > SET DEBUG=battleship:* & npm start
```

Now cd into BattleShip and run `npm install`.

What have we done so far? From what I understand, we know have a basic web server framework ready for requests to `http://localhost:3000/`. Express provides an easy routing and rendering framework. I considered writing this app without express and just using html but I started down express and I really liked the templating of views and the ease of rendering[7]. Yes, you'll need to learn some Jade[3] but that is not that bad. I'll explain more in a bit.

Run `npm start` in the BattleShip folder and then hit `http://localhost:3000/` in the browser. We have a server responding! How is this stuff working. First open app.js. You'll see a bunch of require('blah') statements. This is loading up node modules[5].

The routes and views folders are where we will spend most of our time. Two routes are defined in app.js. First is for the index which is the root route of `localhost:3000/` and the second is the users route at `localhost:3000/users`. There are index.js and users.js in the routes folder. Both those js files export modules. In app.js, you'll see `var index = require('./routes/index');` and another for users. This makes those modules avaliable to app.js. The calls to `localhost:3000/` and `localhost:3000/users` get wired with `app.use('/', index);` and 
`app.use('/users', users);` (see [6]). So when those endpoints are hit our server executes index.js or users.js. Users.js is simple as it just returns with a string. Index.js is more interesting as it response with a jade templated view.

The views folder currently has 3 views. Layout.jade is going to be what the other views extend/inherit from. The error.jade view is used in the app.js when things go wrong. The index.jade view is what express is rendering for you when you hit `localhost:3000`. You'll see in the index.js route file `res.render('index', { title: 'Express' });` which means change this jade template into html and sub in Express when you see the token #{title} - this render API[7] is the main reason I stuck with express. Go to `localhost:3000` in chrome and hit F12 key. In the Elements sections, you will see the resulting html. You'll see where the layout.jade produced the header and the index.jade extended that filling out the body of the html with the title token replaced.

Now we have a rough idea of how this all works. This is the state for the first commit a5dc46e25346139ffceec465d81336127df47034

Lets clean it up and get ride of users stuff. Lets add a battleship route and view. This is basic it just says welcome to battleship if you go to http://localhost:3000/battleship. commit 0f20e4d69b46546945fe595740686fd36ece696f

Lets add a place for user to give us a coordinate. We will use a form (https://www.w3schools.com/tags/tag_form.asp) and send the input back with a fixed result message. I am not a formatting expert so it looks crude but it accomplished what I was aiming for. commit 0a6432a8cf7eebce6bb8082f2f39f30a67ddc9af

Now we need a spot to save some data. To keep it simple for now, we'll add a hidden read-only input to contain the current board state. We will also pre-populate a small 3 by 3 board. The basic design for the board is a 2D array for the cells (only cells that have a ship or have been fired on), a list of ships in an array where we reserver the 0 index for null so i don't have to do -1 shifts everywhere. So ships[1] is the first ship and the value is number of hits it has left. board also has the count of remaining ships (shipsRemaining) and an indicator if the game is over. This starts to get into more javascript work so look at ?? as a reference. Also, I added the commented out line "//let result = fire(x,y,board);" which foreshadows my plan for updating the data. I recommend running the server at this time and going to our battleship enpoint. Hit F12 in the browser and expand the html, you will see the rendered html from the jade template and you will see our hidden json board. commit 73d236bef7dc96726d93efdc82cd699960c30994

Now lets add a grid. Stylesheet gets loaded by the layout.jade template. I am not a css pro and at this point I only modified (7) a little to get fixed size cells. commit 6b64db3522c16c28d0c06390638f1dfd1abf1a7d

Cool. Time to build some battleship logic. Added java script for the fire method and result returned by this method. Watch it in action in the browser with F12 to see the board changes. commit 5d94ca4b277461d0915fd27811411386b0dd208f

Lets make the grid work and make the messaging on the page nicer. commit ???

now we have a working battleship with the one fixed board.

Next steps?? random generated board with the 5 standard ships. Save and load from file system [5]. Stop sending back board and instead save it. Create 2 players.

Resources:
[1]: https://closebrace.com/tutorials/2017-03-02/the-dead-simple-step-by-step-guide-for-front-end-developers-to-getting-up-and-running-with-nodejs-express-and-mongodb
[2]: https://javascript.info/
[3]: https://scalate.github.io/scalate/documentation/jade-syntax.html
[4]: https://www.w3schools.com/tags/
[5]: https://www.w3schools.com/nodejs/default.asp
[6]: http://expressjs.com
[7]: http://expressjs.com/en/api.html#res.render
[8]: https://codepen.io/eddyerburgh/pen/RaBgxq
