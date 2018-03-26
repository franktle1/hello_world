# Documentation

# Commands for running the program.
## If a new version is downloaded:
1. Go to the root folder where the package.json file is located and open up your terminal in the folder
2. "npm install" - this will create the node modules folder that will contain your dependencies 
3. "npm run build:dev" - this will compile your code, make a dev folder and dev/public folder and pull up the server
4. "npm run start" - if no changes were made and code is already compiled

# Notable Tech Stack/API for the Project:
- Node: platform tool
- Webpack: for compiling scripts
- Babel: for compiling new syntax
- ExpressJS: for routing
- node-fetch: for HTTP requests
- EJS template engine: for views/GUI
- Passport: for routing, authorization, and authentication

# Files and directories to consider
## webpack.config.dev.js
- contains the webpack configurations that will neatly bundle all ejs, html, css files into the dev and dev/public folder
- it also tells the system to use babel to compile code into ES5 script
- if you want to add new "views"/webpages, webpage partial views, or JS files that the views use, this is where you need to make edits. A view is a GUI that the user sees

## ./buildScripts/devServer.js
- This is what the node server will read first (settings were written in the package.json file)
- Opens the sign-in page
- Contains APIs and Middleware that the project will be using, and defines which view template we are using.  
- Defines the root endpoints for all our views (i.e. localhost/admin/settings --> /admin will be the root for the admin router)

# In the ./src directory
## /config/local.strategy.js
Establishes credentials in the HTTP header.

header variables: `user.nav` and `user` can be found here
- `user.nav`: an array of objects that contain all the endpoints (and associated text value) that the admin or user can access; This will be used to display as our navigation bar.

- `user`: used to check if a user accessing the endpoint is logged in as a user

## /routes
- Contain instructions that tell what view to load at which endpoint.
- Contains instructions on which variables to load into the view/ejs file
- Includes authorizing user permissions with the user variable (req.user) and redirects any non-users to the signin page
-Each file in this folder contains a router with a specific root (defined in the devServer page.)

### Overview of current files
- authRoutes.js: Defines login/logout endpoints; 
- homeRoutes.js: Home Page after signing in
- companyDataRoutes.js: The search page
- adminRoutes.js: Contains endpoints visible only to admin including settings (changing labels), and usermanagement (adding/removing users)

## /service
- Contains the file with functions that will send/retrieve HTTP requests from the Java API
- Enables access to public methods that can be used in JS files associated with views to retrieve/send data.

## /views
- Contains the files that the client would see. Essentially the GUI or webpages that will be displayed
- All files are ejs format which combines 

## /common.js
- Used to import libraries like BootStrap and jQuery into the views

## /app.css
- Contains global css styles.

## package.json  
- contains all the libraries, dev dependencies, dependencies that your web app will use.

# For Windows Users, make sure to replace the current script commands with these scripts inside the package.json file. 
```
"scripts": {
"make-dev-dir": "mkdir dev ",
"make-dev-pub": "mkdir dev\\public ",
"test": "echo \"Error: no test specified\" && exit 1",
"clean-dist": "npm-run-all --sequential delete-dev-dir make-dev-dir make-dev-pub",
"start": "npm-run-all --parallel security-check run-server lint:watch",
"share": "npm-run-all --parallel run-server localtunnel",
"delete-dev-dir": "rimraf ./dev",
"localtunnel": "lt --port 3000",
"security-check": "nsp check",
"run-server": "babel-node buildScripts/devServer.js",
"lint": "esw webpack.config.* src buildScripts --color",
"lint:watch": "npm run lint -- --watch",
"prebuild:dev": "npm run clean-dist",
"build:dev": "babel-node buildScripts/devBuild.js",
"postbuild:dev": "npm run start"
}
``` 

# Making Modifications to the Web App
    
## Adding a new view or a new partial view:
1. Create the ejs in the view folder
2. (Adding JS Files) Any JavaScript file associated with the view are currently placed in the /src folder
3. (If adding a JS files for a view) Make edits in the same webpackconfigfile to the "entry section" if you want to add a new js file for a view
```
entry: {
settings: path.resolve(__dirname, 'src/settings'),
webpack: 'webpack-hot-middleware/client',
common: path.resolve(__dirname, 'src/common'),
examplejsfile: path.resolve(__dirname, 'src/examplejsfile') <---add it like this one
}
```
4. Modify the webpack.config file; Any new or partial view requires a new HtmlWebPackPlugin section. Any js files
- any JS files added in entry should have the same name as the file in chunks array;
```
new HtmlWebpackPlugin({
    template: '!!raw-loader!src/views/example.ejs', // The view ejs file you made
    chunks: ['common'], // ['common', 'examplejsfile'] if you want to add a JS file for that view 
    hash: true, 
    inject: true, 
    filename: 'example.ejs'}), //the view ejs file you made
```

5. (Only if you are making another endpoint) Setup a new route in the routes directory; if you're adding onto an existing root endpoint (ie say you want to add /admin/newpage to /admin), then just append to an existing JS file.     



