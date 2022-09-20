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
    - 
    
- WORKING WITH SITECORE LAYOUTS, RENDERINGS, & DATA TEMPLATES:
- STUDYING SITECORE:
- WORKING WITH SITECORE UPDATES: