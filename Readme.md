Documentation

Notable Tech Stack/API for the Project:
    Node: platform tool
    Webpack: for compiling scripts
    Babel: for compiling new syntax
    ExpressJS: for routing
    node-fetch: for HTTP requests
    EJS template engine: for views/GUI
    Passport: for routing, authorization, and authentication


Files and directories to consider
webpack.config.dev.js
    contains the webpack configurations that will neatly bundle all ejs, html, css files into the dev and dev/public folder

    it also tells the system to use babel to compile code into ES5 script
    
    if you want to add new "views"/webpages, webpage partial views, or JS files that the views use, this is where you need to make edits. A view is a GUI that the user sees

./buildScripts/
    devServer.js
        This is what the node server will read first (settings were written in the package.json file)

        Opens the sign-in page

        Contains APIs and Middleware that the project will be using, and defines which view template we are using.  

        Defines the root endpoints for all our views (i.e. localhost/admin/settings --> /admin will be the root for the admin router)

./src
    /config/local.strategy.js
        Establishes credentials in the HTTP header.

        header variables: user.nav and user can be found here
            USER.NAV: an array of objects that contain all the endpoints (and associated text value) that the admin or user can access; This will be used to display as our navigation bar.

            USER: used to check if a user accessing the endpoint is logged in as a user

    /routes
        Contain instructions that tell what view to load at which endpoint.

        Contains instructions on which variables to load into the view/ejs file

        Includes authorizing user permissions with the user variable (req.user) and redirects any non-users to the signin page

        Each file in this folder contains a router with a specific root (defined in the devServer page.)

        Overview of current files
            authRoutes.js: Defines login/logout endpoints; 
            homeRoutes.js: Home Page after signing in
            companyDataRoutes.js: The search page
            adminRoutes.js: Contains endpoints visible only to admin including settings (changing labels), and usermanagement (adding/removing users)

    /service
        Contains the file with functions that will send/retrieve HTTP requests from the Java API

        Enables access to public methods that can be used in JS files associated with views to retrieve/send data.

    /views
        Contains the files that the client would see. Essentially the GUI or webpages that will be displayed

        All files are ejs format which combines 

    /common.js
        Used to import libraries like BootStrap and jQuery into the views

    /app.css
        Contains global css styles.



package.json  
    contains all the libraries, dev dependencies, dependencies that your web app will use.

    For Windows Users, make sure to replace the current script commands with these scripts. 
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

Making Modifications to the Web App:
    
    Adding a new view or a new partial view:
        1. Create the ejs in the view folder
        2. (Adding JS Files) Any JavaScript file associated with the view are currently placed in the /src folder
        3. Modify the webpack.config file; Any new or partial view requires a new HtmlWebPackPlugin section

        ```
        new HtmlWebpackPlugin({
          template: '!!raw-loader!src/views/example.ejs', // The view ejs file you made
          chunks: ['common'], // the JS files you want to apply on the view in src; ie, if you want to add a file ['common', 'anotherjsfile']
          hash: true, // We do this to bust cache.
          inject: true, 
          filename: 'example.ejs'}),
        ```

        4. (Adding JS files for a view) Make edits in the same webpackconfigfile to the "entry section" if you want to add a new js file for a view
            entry: {
            settings: path.resolve(__dirname, 'src/settings'),
            webpack: 'webpack-hot-middleware/client',
            common: path.resolve(__dirname, 'src/common'),
            examplejsfile: path.resolve(__dirname, 'src/examplejsfile'),},



