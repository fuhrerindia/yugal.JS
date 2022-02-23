# Yugal.JS
Yugal.JS is a frontend Single Page framework, you can easily build your Single Page App with very basic knowledge of Javascript and html.
File size of Yugal.JS is just 16kb.


## Author

 - [Paurush Sinha](https://www.instagram.com/sinha.paurush/)

## Coverting Existing Bare Project to Yugal.JS
- Add the following code to your `<head>` tag.
```
 <link rel="stylesheet" href="https://yugaljs.netlify.app/yugal.css">
```
- Add comments below at the end of `<head>` tag.
 ```
<!-- ADD ANY YOUR CUSTOM <HEAD> CODE ABOVE, DON'T ADD ANY CUSTOM HEAD CODE BELOW.  -->
<!-- DO NOT DELETE THIS COMMENT! THIS COMMENT IS VERY IMPORTANT FOR YUGAL TO WORK! -->
 ```
 So that code must look like
 ```html
  <head>
   ...
    <link rel="stylesheet" href="https://yugaljs.netlify.app/yugal.css">
   ...
   <!-- ADD ANY YOUR CUSTOM <HEAD> CODE ABOVE, DON'T ADD ANY CUSTOM HEAD CODE BELOW.  -->
   <!-- DO NOT DELETE THIS COMMENT! THIS COMMENT IS VERY IMPORTANT FOR YUGAL TO WORK! -->
  </head>
 ```
- Add follwing code to `<body>` tag.
  ```html
  <div id="yugal-root"></div>
```
- Add following code at the end of `<body>`
 ```html
 <script src="https://yugaljs.netlify.app/yugal.js"></script>
```
- Create a file with name `.htaccess` in your root folder of project. Follow the instructions below. 
**Remove all other code from `<body>`, and convert it in the following code format.**
## How to create an App with Yugal.JS?
- Open `index.html` file in root, and customize site title in `<title>` and in `content` of `<meta name="title">` for better SEO.
- Customize `app.js` file.
- Make sure that `app.js` is included in `index.html` after calling `./yugal/yugal.js` at the end of `body` tag. 
- Importing `./yugal/yugal.js` is very important. And make sure calling it before calling your other `js` scripts.
### Creating First Page
- Pages in `Yugal.JS` are methods. You are needed to define a simple `js` method which return an `object` with `render` parameter. `render` parameter accepts the code to render into the body. Below is the example.
```javascript
    function HomePage(){
        return{
            render: `<h1>HELLO WORLD</h1>`
        };
    }
```
### Rendering Page to Screen
To render `code` into the page, you are needed to call a method from `yugal`. `yugal.navigate` method is used to render code to page. Pass method defined for page above into `yugal.navigate`.
**Example**:
```javascript
    yugal.navigate(HomePage());
```
### Updating Page Title
To update page title, Yugal.JS has one line solution to it, just call `yugal.title` and pass new title into this method. If no new title is defined into this method, it will return current `title`.

**Example:**
```javascript
    yugal.title("SOME NEW TITLE"); //UPDATES TITLE AND RESPECTIVE META TAGS
    yugal.title(); //RETURNS CURRENT TITLE
```
### Custom `<head>` code for YUGAL.JS Pages
If you want to add some custom code to `<head>` which should be applicable for custom pages only, you can use `yugal.header` method. Pass your custom `head` to `yugal.header` and it will render it to the `head`.

*THIS WILL UPDATE `<head>` WITHOUT REMOVING DEFAULT CODE WHICH ARE ABOVE THE IMPORTANT COMMENT IN INDEX.HTML.*
```javascript
    yugal.header(`
        <meta name="something" content="something" />
    `); //THIS WILL UPDATE <head> WITHOUT REMOVING DEFAULT CODE WHICH ARE ABOVE THE IMPORTANT COMMENT IN INDEX.HTML.
```
You can also use `yugal.header` with empty strings to remove custom `head` code.
### Page Routing
By routing pages you can easily navigate between methods with custom urls. This requires some additional configurations.
- Make sure that in `.htaccess` file, `ErrorDocument 404` is pointed to `index.html`.
- You need to pass `uri` parameter in page methods for custom `urls`.
*Example for Defining URI parameter in Page Method*
```javascript
    function HomePage(){
        return{
            render: `<h1>HELLO WORLD</h1>`,
            uri: 'home.html' //THIS WILL BE CUSTOM PAGE NAME FOR URLS
        };
    }
``` 

**HOW TO USE `yugal.router`?**

`yugal.router` must be called after the page is completely loaded, so that there is no error. For this `addEventListener` must be used.
**Example**
```javascript
    window.addEventListener("load", ()=>{
        yugal.router();
    });
```
However in above example, blank parameter could cause some error, so this code will not work, refer to examples below.

**USAGE**

`yugal.router` accepts an `object` as parameter, with `initial`, `error404` and `screens` as parameters.
- `initial`: accepts the page method which must be renderred if no pointed page is defined in URL
- `error404`: accepts the page method which must be renderred when router can't find any page method with URI parameter which is called by URL. (OPTIONAL)
- `screens`: accepts an array of all page methods.

**Example**
```javascript
    function ErrorPage(){
        return{
            render: `<h1>ERROR!</h1>`
        };
    }
    function AboutPage(){
        return{
            render: `<h1>ABOUT PAGE</h1>`,
            uri: 'about'
        };
    }
    function HomePage(){
        return{
            render: `<h1>HELLO WORLD</h1>`,
            uri: 'index'
        };
    }
    window.addEventListener("load", ()=>{
        yugal.router({
            initial: ()=>HomePage(),
            error404: ()=>ErrorPage(),
            screens: [
                HomePage,
                AboutPage
            ]
        });
    });
```
Don't use `yugal.navigate` barely to initially render a method, as this is handled by `yugal.router` itself.

**`yugal.router` MUST BE RAN ONLY ONCE IN WHOLE PROJECT**

## Life Cycle Methods
Yugal has `2` Life Cycle method for page methods.
- `willMount` : This method runs before renderring code to page.
- `onMount`: This method runs after mounting code to page.
Both are passed as `parameter` in `object` which page method returns.
**Example**
```javascript
    function HomePage(){
        return{
            render: `<h1>SOME CODE!</h1>`,
            uri: 'index',
            onMount: ()=>{
                console.log('AFTER MOUNTING');
            },
            willMount: ()=>{
                console.log('BEFORE MOUNTING');
            }
        };
    }
``` 
## Dynamically Importing Scripts to Page
To import some `js` file Dynamically, use `yugal.include` method and pass file path, relative to `index.html` in it.
However, Yugal don't have a method to remove the Dynamically imported script. This can be done manually.
**Example**
```javascript
    yugal.include('./abc.js');
```
`./abc.js` will be imported at the end of `<body>` tag.
## Production App or Site
Before deploying your project, add `yugal.production()` in anywhere in your `JS` code. This will do following.
- Disable `console.log`
- Disable `console.warn`
- Disable `console.error`
- Disable Yugal Error Reports
- Add a warning in console
This makes sure, that it hides any logs or error from or in code.

## A Simple Yugal App Example
**`index.html`**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="./yugal/yugal.css">
    <title>Yugal App</title>
    <meta name="title" content="Yugal App" />
    <!-- ADD ANY YOUR CUSTOM <HEAD> CODE ABOVE, DON'T ADD ANY CUSTOM HEAD CODE BELOW.  -->
    <!-- DO NOT DELETE THIS COMMENT! THIS COMMENT IS VERY IMPORTANT FOR YUGAL TO WORK! -->
</head>
<body>
    <div id="yugal-root"></div>
    <script src="./yugal/yugal.js"></script>
    <script src="./app.js"></script>
</body>
</html>
```
**`app.js`**
```javascript
const {navigate, router, header, title, production} = yugal;
production();
const navBar = `
    <ul>
        <li onclick="">HOME</li>
        <li onclick="">ABOUT</li>
    </ul>
`;
function HomePage(){
    return{
        render: `
            ${navBar}
            <h1>HOME PAGE</h1>
        `,
        uri: 'index.html',
        onMount: ()=>{
            let value = localStorage.getItem("something");
            alert(value);
            header("<meta name='abc' content='def' />");
            title("HOME PAGE");
        },
        willMount:()=>{
            localStorage.setItem("something", "HELLO");
            header("");
        }
    };
}
function AboutPage(){
    return{
        render: `
            ${navBar}
            <h1>ABOUT PAGE</h1>
        `,
        uri: 'about.html',
        onMount: function(){
            console.log("PAGE MOUNTED!");
            title("ABOUT PAGE");
        },
        willMount: function(){
            console.log("PAGE IS GOING TO MOUNT");
            header("");
        }
    }
}
function error404(){
    return{
        render: 'ERROR!'
    };
}
window.addEventListener("load", ()=>{
    router({
        initial: HomePage,
        error404: error404,
        screens: [
            HomePage,
            AboutPage
        ]
    });
});
```
In above code `const {navigate, router, header, title, production} = yugal;`, this line is extracting `yugal.navigate`, `yugal.router`, `yugal.header`, `yugal.title`, `yugal.production` as `navigate`, `router`, `header`, `title`, `production` respectively.

**`.htaccess`**
```.htaccess
ErrorDocument 404 /index.html
```
