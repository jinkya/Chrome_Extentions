# Chrome_Extentions

## Section 1: Intro

#### PROJECT SETUP
1. Install nodejs (npm will be also installed)
2. Install VS Code
3 VS Code Terminal - 

      npm init
      npm i types @types/chrome

 
  [install types for project, as the suggestions will be auto available in chrome]
  
4. chrome::extensions (just bookmark it for frequest visits)
   [Turn on the developer mode]
5. Some knowledge of js and jQuery required.

What is an extention?
Small /w prog which customizes browsing experience of user.
Enhance or add features to browser
The xtention mainly consist 4 different types/components
1]the background script 2]content scripts 3]optionspage
and 4] the UI element
The final extention is just a bundle of HTML, CSS, JS and
images if you have any and is zipped to .crx file


## Section 2: Development

#### MANIFEST FILE
manifest.json

    {
    "name": "my first extention",
    "vesrion":"1.0.0",
    "description":"jinkyas first extention",
    "manifest_version":2
    }

.
Load unpacked in extntion
NavigatE to root directory and hit okay and your 
first extention can be seen.
In documnetation their are a lot of fields for manifest
https://developer.chrome.com/extentions/manifest

#### BACKGROUND SCRIPT
Fundamentals of google chrome extentions-
the background scripts. The extentions are based on 
events. The events are browser triggers such as closing
the tab, opening the tab, navigatingor clicking.
The background script is only loaded when required and 
goes idle when not in use.
Register the background script in the manifest

    {
    ...
    "background":{
    	"scripts": ["background.js"],
    	"persistent": false
    	}
    }

Create background.js

    chrome.runtime.onInstall.addListener(()=>{
    	console.log("installed");
    })

Reload the extention
You can inspect the extention and head to console 
to see result.
The background scripts are usually used when we want
to install a context menu.
Create a ilstener when bookmark is created
background.js

    ...
    chrome.bookmarks.onCreated.addListener(()=>{
    	alert('Bookmark saved');
    })

You can see the error in the the chrome extention tab.
The reason you get an error is you need to add 
permission to access the bookmarks
manifest.json

    {
    ...
    "permissions":[
    	"bookmarks"
    ]
    }

Try to bookmark any page to test this.
Most of the  chrome APIs are available through the
background script only, background script is the one
which is responsible for reacting to the events which
occurs over time such as on installed, on cerated, bokmarks 
and all these interesting things.

#### CONTENT SCRIPTS
Background and Content script ususally goes hand in hand 
where the backgroud script is responsible for reacting to 
events and adding listener to various events the content 
script runs in the context of the web pages. They are able 
to read the details of the web page you visit, but also make 
changes and pass information to the background script. 
To create a content script,first add metadat to manifest
manifest.json

    {
    ...
    "content_scripts":[
    	{ 
    	  "js": ["content.js"],
    	  "matches":["https://*.youtube.com/*"]    // read @ glob pattern
              "exclude_matches": ["https://*youtube.com/watch*"],
    	  "run_at":"document.js"
    	}
     ]
    }

Save and refresh extention and verify if button enjected
Check in developer tools source for the content.js

Create content.js
For instance we want to add an additional button to youtune
content.js
//Check the youtube button DOM HTML, as it can frequently change

    window.onload = ()=>{
    	const button = document.crateElement('button');
    	button.id="darkModeButton";
    	button.textContent="MAKE IT DARK";
    	document.querySelector('#end').prepend("Dark Mode");
    	button.addEventListener('click',()=> enableDarkMode());
    }
    function enableDarkMode(){
    	document.getElementsByTagName('ytd-app')[0].style.backgroundColor="black";
     }

Its a local development but for deployed you should always
add an layer of protection for extenal communication via 
HTTPS, to filter for a cross site scripting attacks.

#### EXTENTION UI
icons and bar icons etc.
manifest.json

    {
    ...
    "icons":{
    	"16": "darkIcon.png",
    	"48": "darkIcon.png",
    	"128": "darkIcon.png",
    	},
    "browser_action": {
    	"default_title":"Created by jinkya, enjoy!",
    	"default_popup":"popup.html"
    }
    }

popup.html

    <html> 
    <head></head> 
    <body>
    <img src="./darkIcon.png" width="40">
    Go to YouTube and a new button will appear
    <a href="https://google.com" target="_blank">
    <button>Visit HomePage</button>
    </a>
    </body>
    <script src="popup.js"></script>
    </html>

The extnetion at the bookmark bar can only be shown in the "matches" path

BONUS: PERSITING DATA IN STORAGE
https://developer.chrome.com/apps/storage
chrome.storage API to store, retrieve, and track changes to
user data
