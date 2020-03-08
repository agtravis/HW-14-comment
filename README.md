# HW-14-comment

_In directory and file order:_

# config > middleware > isAuthenticated.js

This is a middleware file that is called when the path `/members` is routed. It is called in the HTML-routes.js file, and runs as a callback function BEFORE a request is made. If `req.user` _exists_ as a property (i.e. if the user is signed in), the `next()` function is called, which in this case calls the callback function to route the user to the original request of `/members`. If `req.user` does not exist, the response is redirected to the root (`/`).

# config > config.json

This is a JSON file which contains instructions on which host, and therefore which database, should be connected to. There are three objects, one for developement, one for testing, and then one for production. When ready to upload to a remote server, this file would be reconfigured to access the database using an environment process variable (for deploying to Heroku with JawsDB for example).

# config > passport.js

This module calls in two requirements, `passport` and `passport-local`, along with the models (databse table representations). The model used here is the one that stores the username/email and encrypted password. Essentially what is going on here is it takes the user's unput and checks it against what is stored in the table. First it looks for the user, and if it doesn't exist, returns a message saying so. If not returned, it then checks the password (the decryption is done in the called in module `db.User`). Again if this doesn't match, the user is returned unsuccessful. If both matches, the same function `done()` is called but this time it is passed the response data instead of `false`. Finally there is some obligatory code essential to keeping the authentication through CRUD operations.

# models > index.js

This is the file generated upon Sequelize CLI command `init`. It is just a set of instructions that tells Sequelize a few things, like which database to interaction with based on the environment, and also runs on each of the model directory's .js files to enable them to be able to interact with the database tables they represent.

# models > user.js

Already touched on this file a little. This is where the encryption is handled. First it makes sure the user info follows the required constraints of SQL generally and applies additional validation. Then it runs on the password so it is stored with encryption in the database. Finally, although 'finally' only applies to the position of the block of code and not the actual sequence of events here, a hook runs. This is basically an instruction that says right before a specific operation occurs (in this case `beforeCreate` - so crucially _after_ validation), to run something. This here runs some code that creates a hash syncronously to be able to compare the actual password to the encrypted.

# node_modules

This is just a directory containing all the modules that have been utilized in this app. This should be ignored if uploading with git.

# public >

This directory is a way to hold static files and deliver them without being routed. Or rather, to route them collectively behind the scenes. This is done via the middleware in `app.use(express.static('public'))`.

# public > js > login.js

This file contains the AJAX functions that take the information from the form and send to be routed, in this case to log in. Based on the API responses, the user will be directed to either a member page or to sign in again if unsuccessful.

# public > js > members.js

This file sets content on the page based on an API response, in this case it checks to see which user is logged in.

# public > js > signup.js

Another form and API handling for a new user. This will then either add the user to the database, or respond that the credentials are invalid for whatever reason (fail validation or already exist).

# public > stylesheets > style.css

This is any styling that has been applied to the front end.

# public > [login/members/signup].html

These three HTML files are what content will be delivered to the browser based on API responses called in the JavaScript.

# routes > [api-routes/html-routes].js

These files contain instructions as to what to do when certain routes are called upon. They are split apart for the reason that they essentially do two different things via the same methods. The HTML delivers views to the browser (in this case static HTML files), and the API handles database CRUD interactions.

# .eslintignore/.gitignore

These two files list other files that certain things should ignore. `.eslintignore` tells the linter which files to not check, and `.gitignore` tells git which files to not commit or push.

# .eslint.json

This file tells the sibling files and folders and children of those folders which rules to enfore as far as linting goes. These rules are stored as JSON.

# package-lock.json

This contains a load of options and is created after any modules are installed. It also helps to align older versions of modules if newer versions have been released, to allow for guaranteed functionality.

# package.json

This is created by running `npm init` in the CLI before any modules are installed, and is crucial in allowing other users besides the original creator to simply use the code. It contains a list of modules that must be downloaded for operation, in order to save the creator from uploading those files (that are always the same in any instance - version aside) and storing them each time. (certain modules, such as `fs` are so common that they have long been a part of Node.Js).

# server.js

This is the entry point for the app. It basically sets up everything already detailed, via middleware, and then finally opens a port up for listening so that the app can be run. Since this app uses Sequelize, it also syncronizes the database and models before allowing any user interaction.
