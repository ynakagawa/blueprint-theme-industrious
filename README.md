# DF Blueprint - Converting a Free HTML Template into an AEM Theme

This is the code for the DF Blueprint Reference skin: 

## Lessons
1. Creating an AEM Project using the DF Blueprint Archetype
2. Arranging the HTML for easier AEM Import & creating the AEM Clientlibs
3. Configuring your AEM Page template
4. Container Component
5. Embed Component

## Lesson 1 - Setup AEM using DF Blueprint Archetype

```
mvn archetype:generate \
  -DarchetypeGroupId=com.adobe.granite.archetypes \
  -DarchetypeArtifactId=aem-project-archetype \
  -DarchetypeVersion=23 \
  -DgroupId=com.adobe.aem.blueprint \
  -Dversion=0.0.1-SNAPSHOT \
  -DappsFolderName=bp \
  -DartifactId=aem-guides-bp \
  -Dpackage=com.adobe.aem.guides.bp \
  -DartifactName=Digital\ Foundation\ Blueprint \
  -DcomponentGroupName=BP \
  -DconfFolderName=bp \
  -DcontentFolderName=bp \
  -DcssId=bp \
  -DisSingleCountryWebsite=y \
  -Dlanguage_country=en_us \
  -DaemVersion=cloud \
  -DoptionDispatcherConfig=cloud \
  -DoptionIncludeErrorHandler=y \
  -DoptionIncludeExamples=y \
  -DoptionIncludeFrontendModule=y \
  -DpackageGroup=bp \
  -DsiteName=DF\ Blueprint\ Site \
  -DappId=bp \
  -DappTitle=bp
```

## Lesson 2 - Arranging HTML for easier AEM Import

1. Download HTML template-industrious & unpack [html zip](https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:c2227230-ccda-47b5-82f2-9192b55e1286)


  _For this tutorial, i went to [Templated.io](https://templated.co/) & downloaded a free HTML template.  Templated provides a collection of simple HTML5 & Responsive site templates, released for free under the Creative Commons._
  _The template i chose was called [Industrous](https://templated.co/industrious)_

2. Rename assets folder to resources

3. move html files & images into resources folder

   _AEM Clientlib feature AllowProxy property will expose the folder "resource"_ 
   https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet

4. De-compile & modify font-awesome.min.css _I used https://unminify.com_ [Sample provided](https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:6fc2fdaf-75a5-4d53-bde5-81949b4cab86)

   * create file font-awesome.css & paste unminified css & place in resources/css
   * delete file font-awesome.min.css
   * create file font-awesome.scss & place in resources/sass/
   * change resources/sass/font-awesome.scss line #7 - #9
        * ../fonts --> ../../resources/fonts
   * comment out reference in resources/sass/main.scss to the correct scss file (line 8)
        *//  @import 'font-awesome.scss';
       
4. Modify index.html file references
   * remove all instances of assets/ 
   * change header element from nav to div
   * change NAV ID menu's child div class from links to cmp-navigation__group
   * change image path to images/
   * fix bad references to .png files by changing to .jpg
   
5. Modify generic.html files
   * remove all instances of assets/ 
   * change header element from nav to div
   * change NAV ID menu's child div class from links to cmp-navigation__group
   * change image path to images/

6. Modify elements.html files
   * remove all instances of assets/ 
   * change header element from nav to div
   * change NAV ID menu's child div class from links to cmp-navigation__group
   * change image path to images/
   * fix bad references to .png files by changing to .jpg

7. Modify resources/sass/layout_menu.scss
   * line 30 .links to .cmp-navigation__group
 
8. Modify resources/sass/layout/_banner.scss
   * background-image: url('../images/banner.jpg');

9. Modify resources/sass/layout/_cta.scss
   * modify image reference to ../images/cta01.jpg
      
10. Compile with sass running sass --no-source-map sass:css        

11. Create AEM Clientlib structure & import theme
 
    * under ui.apps/src/main/content/jcr_root/apps/bp/clientlibs/ , create folder clientlib-themes
    * under ui.apps/src/main/content/jcr_root/apps/bp/clientlibs/clientlib-themes , create folder templated-industrious
    * under ui.apps/src/main/content/jcr_root/apps/bp/clientlibs/clientlib-themes/templated-industrious , create folder resources
    * create file css.txt & js.txt under ui.apps/src/main/content/jcr_root/apps/bp/clientlibs/clientlib-themes/templated-industrious
         * css.txt add references to css files
```
#base=resources/css
main.css
font-awesome.css 
```

   * jx.txt add references to js files

```
#base=resources/js
jquery.min.js
browser.min.js
breakpoints.min.js
util.js
main.js
```
 
   * create a file .content.xml to define AEM node structure under ui.apps/src/main/content/jcr_root/apps/bp/clientlibs/clientlib-themes/templated-industrious
         * add property jcr:primaryType="cq:ClientLibraryFolder"
         * add property allowProxy="{Boolean}true"
         * add property categories="[theme.templated-industrious]"


```
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:ClientLibraryFolder"
          allowProxy="{Boolean}true"
          categories="[theme.templated-industrious]" />
s
```
 
     
 9. Navigate to http://localhost:4502/etc.clientlibs/bp/clientlibs/clientlib-themes/templated-industrious/resources/generic.html to validate HTML working with demo files
    
 10. Navigate to http://localhost:4502/crx/de/index.jsp#/apps/bp/clientlibs/clientlib-themes/templated-industrious to validate AEM Clientlib creation
 
_**To skip the above & download the reformatted files [here](https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:6fc2fdaf-75a5-4d53-bde5-81949b4cab86)**_


## Lesson 3 - Configuring your AEM Page template
[Lesson 3 AEM CRX Package](https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:4b6d8170-ab40-4692-92bd-a3ecf174a469)
  
1. Update Template thumbnail
   * go to Hammer > Templates > BP > Content Page : Properties & upload new image (LESSON3_template-thumb.png)
2. Edit Page Template > Page Policy: Add clientlib theme.templated-industrious
3. Edit Text Component config:  Enable show HTML Source
4. Edit Container Component config: 
    * Enable background image
    * Add Style Group:  Render
    * Save as policy: theme-industrious
5. Edit Header XF
    * Edit Template & allow for bp.structure components
    * Remove existing component:  Language Navigation, Search
    * Add embed component inside Container Component
       * ID:  header
       * HTML should be taken from HTML sample, code within <header> tags
       * make sure to change link to /content/bp/us/en/home.html
    * Update Navigation component & add menu to ID field
6. Create HomePage to validate clientLib through top Menu Nav
    * Location: /content/sites/bp/us/en
    * Page properties: Title= Home, name= home
    * You can use AEM Preview to view this or you can remove "editor.html" --> http://localhost:4502/content/bp/us/en/home.html
       * Make sure the Top Nav appears & the menu scrolls out when clicking Menu
7. Edit Footer XF
    * Remove the text component containing copyright dummy data
    * Add Container Component with footer as the ID value
    * Add 3 text components (We will use CSS to convert into columns)
    * *If you wish to use AEM's layout feature to resize so they appear as columns, then do not use a Container component
    * Edit Page Template & Configure XF text components to allow for HTML source
    * Add html content for all three text components (using the HTML Source option)
    * Footer will appear vertical, fix this by updating CSS to map to AEM
        1. Change .content to .cmp-container (line 16,46,63)
        2. Change section to .text (line 19,49)
        3. Change section to .div (line 64)
  
  [Lesson 3 AEM Content Package](https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:4b6d8170-ab40-4692-92bd-a3ecf174a469)
  
     
## Lesson 4 - Container Components

[Lesson 4 AEM Content Package ] (https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:bf30d08e-8cab-4bbe-8532-26d02e163b17)


1. CTA Style for Container Component

    * Make sure Text has HTML Source enabled (use Template Policy Editor)
    * Open Home page & Add Container Component.  Within the Container, add a Text & Teaser (add dummy content)
    * View Source to see how the page rendered, notice the CTA ID & the class=cmp-container (we will replace CTA wil style system)
    * Open template editor & click on Container to configure:
    * Add a style "CTA" under group "Render".  Value should be cmp-container-cta.  (Publish template when done)
    * In the Home page, edit mode, use the Style System selector & choose CTA as the render option
    * In Preview, inspect the page:  You will notice that cmp-container-cta DIV was added on top of the cta div.
    * Open file resources/sass/layout/_cta.scss, change #cta to .cmp-container-cta .cmp-container
    * recompile sass with:  sass --no-source-map sass:css & push into AEM
    * Make corresponding updates to resources/index.html
       * add DIV with class=cmp-container-cta above DIV with ID=cta
       * remove ID=CTA & replace with class=cmp-container
    * In the Edit Mode of Home page, toggle CTA on/off to view experience


2. Heading Style for Container Component

   * Add style sytem option for Heading wtih value .cmp-container-heading (in Template Policy Editor)
   * In the Elements page, add a container component with ID header
   * Inside the Container Component, add a text and|or title component
   * (if the CSS causes the AEM component bars to get misplaced, remove the ID heading & add it back in after the nested component placements) 
   * Inspect the Elements page & notice the DIV with the ID heading
   * Use the Style System to select Heading & inspect again:  Notice again the addition of the DIV with class cmp-container-heading
   * Modify file resources/sass/layout/_heading.scss, change #heading to .cmp-container-heading .cmp-container
   * Also copy h1 CSS to h2 to allow for component flexibility (Title uses H2)
   * To add support for Text Components, add > .text
   * recompile sass with:  sass --no-source-map sass:css & push into AEM
   * Make corresponding updates to resources/generic.html & elements.html
      * add DIV with class=cmp-container-cta above DIV with ID=cta
      * remove ID=CTA & replace with class=cmp-container
    * In Edit Mode of Elements or Generic page, toggle Heading on/off to view experience

3. Main Style for Container Component

   * Open Generic Page, add Separator component, then a Container component with a nested Text Component with dummy data
   * Open template policy editor & make the following updates:
      * Change "Reader" header to "background layouts"
      * Create a new heading caled "Content Layouts"
      * Change "CTA" to "Scrolling" & Heading to "Fixed"
      * Add a new entry under "Content Layouts" called "main" with value "cmp-container-main"
  * Open the sass file resources/sass/layout/_main.scss and modify:
      * change #main to .cmp-container-main .cmp-container
      * comment out the nested .content class to allow for any component within this main Container component
  * In Edit Mode of Generic, toggle on main to view experience
  * The title appear's smaller then the HTM version in all instances.  HTML is using H1 where AEM components start at H2
      * edit resources/sass/base/_typography.scss & make H2 match H1
      
      
4. Highlights Style for Container Component

   * Open template policy editor & make the following updates:
      * Add a new entry under "Content Layouts" called "Highlights" with value "cmp-container-highlights"
   * Open Edit Page, add Separator component
   * Within the Container components that should be supported in this experience:
      * Add Title Component with dummy data
      * Add Text Component with dummy data
      * Add List Component with dummy data
   * Use the Style System to select "Highights"
   * Open sass file resources/sass/components/_highights.scss & modify:
      * Change .highlights to .cmp-controller-highlights .cmp-controller
      * for every component you wish to support, add a class entry for that component.  i added .text, .list, .teaser
      * copy/paste & adjuste from class .content

[Lesson 4 AEM Content Package ] (https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:bf30d08e-8cab-4bbe-8532-26d02e163b17)

##Lesson 5 - Embed Component

[Lesson 5 AEM Content Package ] (https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:f86d3601-23d5-435a-a40e-845326271f43)

1. Create an embeddable stub
   * Open CRX DE Lite, copy /apps/core/wcm/components/embed/v1/embed/embeddable to /apps/bp/components/embed
   * copy /apps/core/wcm/components/embed/v1/embed/cq:dialog to /apps/bp/components/embed/cq:dialog
   * change the node /apps/bp/components/embed/embeddable/youtube to video
   * change the underlying file from youtube.html to video.html
   * change the jcr:title property on the video node from Youtube to Video
   * place the entire html fragment into the video.html file
2. Validate the experience, then make the video path authorable by creating a dialog entry
3. Configure the Template policy to only allow my video embed
   
[Lesson 5 AEM Content Package ] (https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:f86d3601-23d5-435a-a40e-845326271f43)
 
## Modules

The main parts of the project are:

* core: Java bundle containing all core functionality like OSGi services, listeners or schedulers, as well as component-related Java code such as servlets or request filters.
* ui.apps: contains the /apps (and /etc) parts of the project, ie JS&CSS clientlibs, components, templates, runmode specific configs as well as Hobbes-tests
* ui.content: contains sample content using the components from the ui.apps
* ui.tests: Java bundle containing JUnit tests that are executed server-side. This bundle is not to be deployed onto production.
* ui.launcher: contains glue code that deploys the ui.tests bundle (and dependent bundles) to the server and triggers the remote JUnit execution

## How to build

To build all the modules run in the project root directory the following command with Maven 3:

    mvn clean install

If you have a running AEM instance you can build and package the whole project and deploy into AEM with  

    mvn clean install -PautoInstallPackage

Depending on your maven configuration, you may find it helpful to force the resolution of the Adobe public repo with

    mvn clean install -PautoInstallPackage -Padobe-public
    
Or to deploy it to a publish instance, run

    mvn clean install -PautoInstallPackagePublish
    
Or alternatively

    mvn clean install -PautoInstallPackage -Daem.port=4503

Or to deploy only the bundle to the author, run

    mvn clean install -PautoInstallBundle

## Testing

There are three levels of testing contained in the project:

* unit test in core: this show-cases classic unit testing of the code contained in the bundle. To test, execute:

    ```
    mvn clean test
    ```

* server-side integration tests: this allows to run unit-like tests in the AEM-environment, ie on the AEM server. To test, execute:

    ```
    mvn clean verify -PintegrationTests
    ```

* client-side Hobbes.js tests: JavaScript-based browser-side tests that verify browser-side behavior. To test, go in the browser, open the page in 'Developer mode', open the left panel and switch to the 'Tests' tab and find the generated 'MyName Tests' and run them.


## Maven settings

The project comes with the auto-public repository configured. To setup the repository in your Maven settings, refer to:

    http://helpx.adobe.com/experience-manager/kb/SetUpTheAdobeMavenRepository.html

