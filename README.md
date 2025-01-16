# CHROMEX
cxhrom extension by mym
[Block_URL_Paths_Extension.pdf](https://github.com/user-attachments/files/18444695/Block_URL_Paths_Extension.pdf)








Chrome Extension to Block URL Paths
Below is the code for a Chrome extension that blocks websites based
on their URL paths.
The extension checks the URL of the current tab and blocks the page
if it matches a predefined list of paths.
1. manifest.json
Defines the extension's metadata and permissions.
{
 "manifest_version": 3,
 "name": "Block URL Paths",
 "version": "1.0",
 "description": "Blocks websites based on specific URL paths.",
 "permissions": [
 "tabs",
 "webNavigation",
 "scripting"
 ],
 "host_permissions": [
 "<all_urls>"
 ],
 "background": {
 "service_worker": "background.js"
 },
 "content_scripts": [
 {
 "matches": ["<all_urls>"],
 "js": ["content.js"]
 }
 ],
 "action": {
 "default_popup": "popup.html",
 "default_icon": "icon.png"
 }
}
2. background.js
This service worker listens for navigation events and checks the URL
path.
const blockedPaths = ["/blocked-path", "/example-path"]; // Add your
blocked paths here.
chrome.webNavigation.onBeforeNavigate.addListener(details => {
 const url = new URL(details.url);
 if (blockedPaths.includes(url.pathname)) {
 chrome.scripting.executeScript({
 target: { tabId: details.tabId },
 func: () => {
 document.body.innerHTML = "<h1>Blocked</h1><p>This page
has been blocked by the extension.</p>";
 document.body.style.textAlign = "center";
 document.body.style.marginTop = "20%";
 }
 });
 }
});
3. content.js
(Optional) Allows for injecting scripts or interacting with page content
if needed.
console.log("Content script running...");
4. popup.html
Provides a simple popup UI for the extension.
<!DOCTYPE html>
<html>
<head>
 <title>Block URL Paths</title>
 <style>
 body {
 font-family: Arial, sans-serif;
 margin: 0;
 padding: 20px;
 }
 input, button {
 margin-top: 10px;
 }
 </style>
</head>
<body>
 <h1>Blocked Paths</h1>
 <p>Add or remove paths to block:</p>
 <ul id="pathList"></ul>
 <input type="text" id="pathInput" placeholder="/example-path" />
 <button id="addPath">Add Path</button>
 <script src="popup.js"></script>
</body>
</html>
5. popup.js
Handles adding and removing blocked paths dynamically.
const pathInput = document.getElementById("pathInput");
const addPathButton = document.getElementById("addPath");
const pathList = document.getElementById("pathList");
let blockedPaths = ["/blocked-path", "/example-path"];
function updatePathList() {
 pathList.innerHTML = "";
 blockedPaths.forEach(path => {
 const listItem = document.createElement("li");
 listItem.textContent = path;
 const removeButton = document.createElement("button");
 removeButton.textContent = "Remove";
 removeButton.style.marginLeft = "10px";
 removeButton.onclick = () => {
 blockedPaths = blockedPaths.filter(p => p !== path);
 updatePathList();
 };
 listItem.appendChild(removeButton);
 pathList.appendChild(listItem);
 });
}
addPathButton.onclick = () => {
 const newPath = pathInput.value.trim();
 if (newPath && !blockedPaths.includes(newPath)) {
 blockedPaths.push(newPath);
 updatePathList();
 pathInput.value = "";
 }
};
updatePathList();
6. Icon (Optional)
Add an icon.png to serve as the extension's icon.
Installation:
1. Save the files in a folder (e.g., block-url-path-extension).
2. Open Chrome and navigate to chrome://extensions/.
3. Enable "Developer mode" in the top-right corner.
4. Click "Load unpacked" and select the folder containing your
extension.
Now, the extension will block websites based on the paths you
specify!
