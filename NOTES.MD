## Getting Started With Sitecore 9 & Helix

- COURSE OVERVIEW:

- LEARNING SITECOURSE:
    - Version check: Sitecore 9.2.
    - Sitecore: A customer experience management platform providing a web content management system, CMS, and multi-channel marketing automation.
    - CMS: Content with presentation.
    - Helix: A set of overall design principles and conventions for Sitecore development.
    - Helix architecture is required. Primer is a tool that can assist in creating the helix architecture.
    - Layouts, renderings, and data templates.

- CONFIGURAING A DEVELOPMENT ENVIRONMENT FOR SITECORE:
    - .NET Framework 4.7.2
    - Solr 7.5: Default search and index provider.
        - Windows Services: SOLR service.
    - Powershell:
    ```javascript
        Set-ExecutionPolicy Unrestricted -Scope LocalMachine
    ```
    - (Install 7-zip.)
    - Sitecore 9.2
    - SUMMARY:
        - Windows 10 O/S. VS 2017. SQL Server 2017. SOLR 7.5.

- INSTALLING SITECORE:
    - Sitecore Dev Portal (Installation Files.)
    - Developer license. (No free developer trial program.)
    - Sitecore Install Assistant. (SIA.)
    - Sitecore Installation Framework. (SIF.)
    - On Premises Download. 9.2. Install everything on one server. (Content management. Content delivery. Reporting & Analytics. SOLR search.)
    - setup.exe.config:
        - SqlServer, SqlAdminUser="sa", SqlAdminPassword.
        - SolrUrl, SolrRoot
        - Web Platform Installer. IIS. SQL PowerShell Module. Sqlcmd Utility.
    - Verify the installation:
        - Via IIS. Three Webs. e.g.: .dev.local. e.g.: http://92sc.dev.local/sitecore
    - SIF:
        - PowerShell 5.1 is required.
        ```javascript
            $PSVersionTable.PSVersion
            Register-PSRepository -Name SitecoreGallery -SourceLocation https://sitecore.myget.org/F/sc-powershell/api/v2 -InstallationPolicy Trusted
            Install-Module SitecoreInstallFramework
            Get-Module SitecoreInstallFramework -ListAvailable
            Install-SitecoreConfiguration -Path .\Prerequisites.json
        ```
        - C:\Program Files\WindowsPowerShell\Modules\SitecoreInstallFramework
        - Open file: XP0-SingleDeveloper.ps1.
        ```javascript
            $Prefix="sc92sif"
            $SitecoreAdminPassword="xyz"
            $ClientSecret=""
        ```
        ```javascript
            ./XP0-SingleDeveloper
        ```
    - What was installed?
        - IIS as administrator. Three Webs.
            - Identity server: Authentication.
            - Public site. Content management & presentation.
            - xconnect: A service layer between xdb and queries, writes betweem xdb. Personalization and analytics reporting.
        - File system folders. (Default inet pub location.) Visual Studio to publish here.
            - Also: App-Data\logs. App_Config with connection strings, access rights.
        - SQL databases. core sitecore interface settings. e.g.: Custom menu options. security authentication. web: The live site.
        - SOLR: Dashboard. master & web proivide the search indexes. One SOLR per core.
    - Make a backup of the Web root.

- SETTING UP A SITECORE MVC VISUAL STUDIO PROJECT USING HELIX:
    - Building a Sitecore Helix Archicture from scratch.
        - Create folder outside of the Web root. Create ASP.NET Web Application (.NET Framework.)
        - Adhere to Sitecore/VS Studio project. Do not "Place solution and project in same directory."
        - Ensure .NET Framework 4.7.2. Create empty project template. Delete all.
        - Three folders: Three layers of the Sitecore Helix architecture.
            - 1. Project. (Presentation. Layouts and page types. Features with presentation to create the Web.)
            - 2. Feature. (Components and functionality.)
            - 3. Foundation. (House projects that are at the core of the Sitecore project. e.g.: Configuration. Bootstrap.)
        - Create Foundation/Configuration folder and add an ASP.NET Web Application (.NET Framework.) "92sc.Foundation.Configuration."
        - src/layer/project/code.
        - Delete and set copy local to false. Replace Web.config with Sitecore root. Set 'copy to output directory' = copy if newer.
        - Add APP-Config folder. Create ConnectionStrings.config. And copy contents from Web root. Set 'copy to output directory' = copy if newer.
        - RE: Publish. IIS as target. File system. Target is Web root. Set debug as configuration.
            - Exclude files from App-Data folder.
        - Use Web one-click publish. Validate Web still loads.
    - Foundation layer is core functionality. Can also include third-party tools, libraries, and frameworks.
        - Foundation.Configuration includes Web.config & ConnectionStrings.config.
    - Feature project: Feasture/PageContent. Create a new ASP.NET Web application. \src\Feature\PageContent\code. 
        - And choose the (optionless) MVC project template.
        - Install Sitecore NuGet packages. First, add SitecorePackageSource with https://sitecore.myget.org/F/sc-packages/api/v3/index.json
            - Sitecore.Kernel e.g.: 9.2.0.
            - Sitecore.Mvc (same package.)
            - Select all references and set 'copy local' to false.
            - <add namespace="Sitecore.Mvc" /> to <namespaces>
        - Feature layer: Components and functionality. Project references Feature. Feature references foundation.
    - Project/Website with new ASP.NET Web Application and an emplty MVC template.
        - And ensure build action is set to none and copy to output directory is set to do not copy with both remaining configururation files.
        - Wire NuGet and obtain the same two Sitecore packages.
        - Presentation. This produces the final Web. This is the top of the foodchain. References both Feature & Foundation.
    - Discuss the Helix layers.
        - Dependency management. High coehision. Low coupling.
        - Feature layer can be dependent upon the foundation layer.
    - Configure new Sitecore Helix Visual Studio projects.
    - SUMMARY:
        - Three layers. Sitecore NuGet packages.

- WORKING WITH SITECORE LAYOUTS, RENDERINGS, & DATA TEMPLATES:
    - Build a Layout & Page Type Template:
        - From WebForms to MVC.
        - Sitecore desktop. Start. Content editor.
        - Page Type Template combines Presentation and content. Every variation should have a Page Type. 
        - Layout is required as presentation.
        - Template standard values (default values.)
    - Publish Sitecore Content:
        - Publish changes from master database to web database.
        - Use smart publish as it validates differences between source and target database.
        - Republish. Overrides all items within the target database. Incremental publishes only items that are denoted within db.
    - Template inheritance:
        - e.g.: Title. Body copy.
        - Field section. Group together similiar fields. e.g.: Title. single line text. Body copy: rich text.
        - Use inheritance to avoid having templates to contain the very same fields.
        - Now use the content editor to populate. Rich text editor.
    - Add a View Rendering:
        - Body copy view rendering. Create Page Content.
        ```html
            @model Sitecore.Mvc.Presentation.RenderingModel
            @using Sitecore.Mvc
            <h1 class="contentTitle">@Html.Sitecore().Field("Title")</h1>
        ```
        - And then within the parent:
        ```html
            @model Sitecore.Mvc.Presentation.RenderingModel
            @using Sitecore.Mvc
            <div>@Html.Sitecore().Placeholder("main")</div>
        ```
        - Always set up within __Standard Values.
        - Sitecore experience editor. Rendering model melds content with presentation.
    - Create a global datasource.
        - Content comes from Sitecore and is not static.
        - Content - Inseret from template. Create a Global Datasources folder.
        - Browse. Upload media. Destination /Media Library/Images. Add alt text.
    - Work with Images in the Media Library:
        - Via an image view rendering:
        ```html
            @model Sitecore.Mvc.Presentation.RenderingModel
            @using Sitecore.Mvc
            @{
                Sitecore.Data.Fields.ImageField img = Model.Item.Fields["Hero Image"];
                string url = Sitecore.Resources.Mediq.MediaManager.GetMediaUrl(img.MediaItem);
            }
            <div id="Header" style="background:white url('@url') no-repeat;background-size:1170px 402px;">
                @Html.Sitecore().Field("Hero Logo", Model.Item, new { @id="scLogo" } )
            </div>
        ```
        - Data Source: Browse...
    - SUMMARY:
        - Layout and Page Type. Presentation.
        - View Rendering. Component.
        - Data Template. Content Definition.
        - Context Item versus Datasource.

- STUDYING SITECORE:
    - Valuable resources: 
        - doc.sitecore.net
        - sitecore.stackexchange.com
        - siteco.re/sitecoreslack
        - community.sitecore.net
    - MVP & developer resources.
        - Jeremy Davis. jewrmdavis.wordpress.com
        - Peter Prochazka. tothecore.sk
        - Mark Cassidy.

- WORKING WITH SITECORE UPDATES:
    - 9.2 versus 9.3. Noteable changes.
    - SIA has improved greatly, with a SOLR installation automatically. Extract and run setup.exe. And a valid license. licanse.xml
    - Windows service and path prefix. e.g.: sc93-
    - SQL Server instance name. sa & password. SOLR instance should already be running. And verify Windows service.
    - Lucene depreciated.