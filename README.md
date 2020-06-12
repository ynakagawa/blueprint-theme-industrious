# DF Blueprint - Converting a Free HTML Template into an AEM Theme

This is the code for the DF Blueprint Reference skin: 

# Lessons
1. Creating an AEM Project
2. Arranging the HTML for easier AEM Import
3. Converting HTML libraries to AEM Clientlibs
4. Configuring your AEM Page template
5. Container Component
6. Embed Component

##LESSON 1 - Setup AEM using DF Blueprint Archetype

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

##Lesson 2
1. Download HTML template-industrious & unpack

2. rename assets folder to resources

3. move html files & images into resources folder

4. De-compile & modify font-awesome.min.css (I used https://unminify.com, sample provided)
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
 
 
###Lesson 3

1. Update Template thumbnail
   * go to Hammer > Templates > BP > Content Page : Properties & upload new image (LESSON3_template-thumb.png)
2. Edit Page Template > Page Policy: Add clientlib theme.templated-industrious
3. Edit Text Component config:  Enable show HTML Source
4. Edit Container COmponent config: 
    * Enable background image
    * Add Style Group:  Render
    * Save as policy: theme-industrious
5. Edit Header XF
    * Edit Template & allow for bp.structure components
    * Remove existing component:  Navigation, Language Navigation, Search
    * Add Container Component wtih ID: Header-top
    * Add embed component inside Container Component
       * ID:  header
       * HTML should be taken from HTML sample, code within <header> tags
       * make sure to change link to /content/bp/us/en/home.html
5. Update Nav (account for AEM changing DIV to NAV)
    * modify demo HTML files replacing NAV with DIV within Head
    * make coresponding update to resources/sass/layout_header.scss (line 51) 
    * compile sass by running 

 
      




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

