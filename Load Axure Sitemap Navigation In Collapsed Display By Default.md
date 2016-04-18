_(Originally published on [my wiki](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwjKxLurgo_MAhUBcz4KHYomCkAQFggpMAE&url=http%3A%2F%2Fmatticus.pbworks.com%2Fw%2Fpage%2F58292320%2FLoad%2520Axure%2520Sitemap%2520Navigation%2520In%2520Collapsed%2520Display%2520By%2520Default&usg=AFQjCNHR2OTyU3ivuK2pTjjIEfKwUnNkAg&sig2=UDz2Mhi03bjMKUF9rheSeg) )_

A common request in the Axure forum community is to find a way to load the sitemap within a generated prototype in a collapsed state, instead of expanded, by default.  Ideally, Axure would include this as a native setting in the future, but for now there is a fairly simple workaround that accomplishes the same task.
 
__Notes:__
* These changes were tested on Axure 6.0 and 6.5.  Your mileage will vary with previous / future versions.
 
## Global Change
Making this change globally, will result in all future prototypes generated from your installation reflecting the collapsed behavior.  If you have multiple designers working on the same prototypes, you will need to make this change on each computer / Axure installation that is used to generate prototypes.

* __Navigate to your Axure Installation directory__
  ```
  On Windows 7, the default is: _C:\Program Files (x86)\Axure\Axure RP Pro 6\_
  and if you are on 6.5, _C:\Program Files (x86)\Axure\Axure RP Pro 6.5\_
  ```
 
* __Within your installation directory, navigate to the "sitemap" plugin folder.__
  ```
  Sub-directory: _\DefaultSettings\Prototype_Files\plugins\sitemap_
  ```
 
* __In the "sitemap" folder, make a backup copy of the "sitemap.js" file, in case you want to revert your changes later.__
 
* __Open the "sitemap.js" file with a text editor.__ I recommend [TextPad](https://www.textpad.com/) or [Notepad++](https://notepad-plus-plus.org).
 
* __Around Line 14, you will see a line that configures the toggle order for ".sitemapPlusMinusLink".  Since we are going to change the default behavior, in the next step, to start in a collapsed state, we need to reverse the toggle order.__
  * Here is the original unmodified line, for reference:
  ```javascript
  $('.sitemapPlusMinusLink').toggle(collapse_click, expand_click);
  ```
 
  * Here is the line with the first modification:
  ```javascript
  $('.sitemapPlusMinusLink').toggle(expand_click, collapse_click);
  ```
 
* __Towards the bottom of the file you will find the Javascript function generateNode.  This function is where we need to make the second modification.__
  * Here is the original unmodified function, for reference:
  ```javascript
function generateNode(node) {
     var hasChildren = (node.children && node.children.length > 0);
     if (hasChildren) {
          var returnVal = "<li class='sitemapNode sitemapExpandableNode'><div><a class='sitemapPlusMinusLink'><span class='sitemapMinus'></span></a>";
     } else {
          var returnVal = "<li class='sitemapNode sitemapLeafNode'><div>";
     }
     returnVal += "<a class='sitemapPageLink' nodeUrl='" + node.url + "'><span class='sitemapPageIcon";
     if (node.type == "Flow") { returnVal += " sitemapFlowIcon"; }
     returnVal += "'></span><span class='sitemapPageName'>"
     returnVal += $('<div/>').text(node.pageName).html();
     returnVal += "</span></a></div>";
     if (hasChildren) {
          returnVal += "<ul>";
          for (var i = 0; i < node.children.length; i++) { 
               var child = node.children[i];
               returnVal += generateNode(child);
          } 
          returnVal += "</ul>";
     }
     returnVal += "</li>";
     return returnVal;
}  
 ``` 
  * Now that you have located the generateNode function, you will need to modify the function with the following edits:
  ```javascript
// Edits by Matt Howell, 9/5/2012
// Modified original function to load sitemap in collapsed layout, instead of expanded.
//
function generateNode(node) {
     var hasChildren = (node.children && node.children.length > 0);
     if (hasChildren) {
          var returnVal = "<li class='sitemapNode sitemapExpandableNode'><div><a class='sitemapPlusMinusLink'><span class='sitemapPlus'></span></a>";
     } else {
          var returnVal = "<li class='sitemapNode sitemapLeafNode'><div>";
     }
     returnVal += "<a class='sitemapPageLink' nodeUrl='" + node.url + "'><span class='sitemapPageIcon";
     if (node.type == "Flow") { returnVal += " sitemapFlowIcon"; }
     returnVal += "'></span><span class='sitemapPageName'>"
     returnVal += $('<div/>').text(node.pageName).html();
     returnVal += "</span></a></div>";
     if (hasChildren) {
          returnVal += "<ul style="display:none;'>";
          for (var i = 0; i < node.children.length; i++) { 
               var child = node.children[i];
               returnVal += generateNode(child);
          } 
          returnVal += "</ul>";
     }
     returnVal += "</li>";
     return returnVal;
}  
  ```
 
 * __Save the file.__
 ```
 Note: Depending on your version of Windows, you may need administrative privileges
 ```
  
 * __If you would prefer, I have included an already modified sitemap.js that you can download and drop in.  Download it [here](sitemap.js).__
 
After you generate your next prototype, you may need to clear your browser cache for the change to be visible, if a version of the prototype has been loaded previously. 
 
 
## Prototype-Specific Change
If you only want to make the change for a specific existing prototype, you can make a similar change in the prototype directory, where the exported HTML and resource files reside.
 
* Navigate to the "Destination" folder where you saved your generated prototype.
 
* Navigate to the _/plugins/sitemap_ sub-directory. 
 
* Notice that the same "sitemap.js" file is present.  You can make the same modifications within this file that is outlined above in the Global section.
 
* Save the file, and refresh your browser. 
 
If you implement this change at the "Prototype-Specific" level, you may need to replace the sitemap.js file after each generation, since the Global sitemap.js included in your Axure installation may overwrite it. 
