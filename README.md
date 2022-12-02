# Screen-Capture-Google-Extension

Creating the Screencapture Chrome Extension
Step 1: Creating the manifest file
First, we will need to create the manifest.json file of our chrome extension script. This is a JSON file that contains the information of our extension and the scripts that will be loaded when browsing the sites.

    {
        "manifest_version": 3,
 
        "name": "Simple Screencapture Extension",
        "description": "This extension allows you to Screencapture the site.",
        "version": "1.0",
        "icons": {
            "16": "screencapture-icon.png",
            "48": "screencapture-icon.png",
            "128": "screencapture-icon.png"
        },
        "action": {
         "default_icon": "screencapture-icon.png",
         "default_popup": "popup.html"
        },
        "permissions": ["tabs", "scripting"],
        "content_scripts": [{
            "matches": [
                "<all_urls>"
            ],
            "js": ["page_content_script.js", "html2canvas.min.js"]
        }]
      }
Step 2: Creating the Popup Interface
Next, we will create an HTML file that contains the elements of the extension's popup interface. The interface contains an icon, headings, text, and a button. Save the file as popup.html.

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Simple Screen Capture Extension</title>
    <style>
        body {
            width: 250px;
            padding: 1.5em 1em;
            background: #000;
            box-shadow: 0px 0px 7px #fff;
            margin: 2px;
        }
        .text-center{
            text-align: center;
        }
        .img-holder{
            display: flex;
            width: 100%;
            align-items: center;
            justify-content: center;
        }
        .img-holder>div{
            width: 65px;
            height: 65px;
        }
        .img-holder>div>img{
            width: 100%;
            height: 100%;
            object-fit: cover;
            object-position: center center;
        }
        .text-muted{
            color:#d0d0d0;
        }
    </style>
</head>
<body>
    <div class="img-holder">
        <div>
            <img src="./screencapture-icon.png" alt="ScreenCapture">
        </div>
    </div>
    <h3 class="text-center" style="color:#fff">ScreenCapture</h3>
    <div class="text-center text-muted"><small><i>Capture the site page by clicking the button below</i></small></div>
    <br>
    <div class="text-center">
        <button class="btn-capture" type="button" id="capturePage">Capture</button>
    </div>
    <script src="capture.js"></script>
 
</body>
 
</html>
Step 3: Creating the Extension's Script
Next, create a new JavaScript file and save it as capture.js. This JS file contains the script that will send a message to the page/tab client when the button "Capture" has been clicked. It will send a message that will trigger the screenshot script on the page/tab client.

// Capture Button Event Listener
document.querySelector("#capturePage").addEventListener("click", function(){
    let tab_params = {
        active: true,
        currentWindow: true
    }
    chrome.tabs.query(tab_params, capture)
 
})
 
// Send Message to the Active Page to trigger screen capturing
function capture(e) {
    chrome.tabs.sendMessage(e[0].id, {action:'capture'})
}
Step 4: Creating the Extension's Content Scripts
Next, we will create the extension's content JavaScript file. This JS file will be loaded on the browsed site. It contains the scripts that listen to incoming messages from the extension script. If the button "Capture" has been clicked, this file will receive the message to trigger the screen-capturing script. Save this file as page_content_script.js.

console.log("Screen Capture Extension Script has loaded")
// Listen to the messages sent by the background script
chrome.runtime.onMessage.addListener(ScreenCaptureExtCB)
 
// Screen Capture Extension's Callback [Receiving message]
function ScreenCaptureExtCB(msg) {
    console.log(msg)
    if(msg.action == "capture"){
        html2canvas(document.body).then(canvas => {
            var blob = canvas.toBlob((blob) => {
                url = window.URL.createObjectURL(blob)
                window.open(url)
            }, "image/png")
        });
    }
}
 
Step 5: Download the Libraries Files
Lastly, download and save the HTML2Canvas and jQuery Libraries files into your source code directory. please make sure that the filenames are exactly the same on the script that loads the libraries. For the HTML2Canvas, the file is loaded at the manifest.json file. The jQuery library is loaded in the popup.html file. Please download also the icon/image that you want to use for the extension.

Here are the following links to download the library files

HTML2Canvas
jquery
How to Upload the Extension Source Code in Chrome Browser
Step 1: Go to Chrome's Extensions Page
To upload the extension into your chrome browser, open the browser and browse chrome://extensions


Step 2: Enable Developer Mode
To enable the Developer Mode, toggle/click the Developer Mode toggle located at the upper right of the page.


Step 3: Load Unpack
Next, load the ScreenCapture Extension source code that we created. To do that, click the Load Unpacked button located at the upper left of the page.


That's you can now check the ScreenCapture Extension in the extensions menu of the Chrome Browser. The extensions menu is located beside the address bar.


That's it! You can now test ScreenCapture Extension in your browser. To test it, browse any site and click the Capture button on the extensions popup interface. The captured image will be shown in a new tab of the browser.

Snapshots
Here's the snapshot of the ScreenCapture Extension popup interface.
