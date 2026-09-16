# osTicket Lab

This lab will set up and use osTicket on `lab-server-1`. I will walk through the setup of osTicket and 
it's prerequisites.

### Lab's Prerequisites 

#### [Azure Lab Setup](https://github.com/woodgavin/azurelab)

#### [Active Directory Setup](https://github.com/woodgavin/adlab)

### osTicket Requirements

osTicket requires certain software in order to run, as well as some Windows configuration. These steps
must be done in order to prevent an issue with osTicket's install process. Within `lab-server-1`, follow
these steps.

> #### Enable IIS
> 
> * Open Server Manager and click `Add roles and features`.
> * Click Next until `Server Roles`.
> * Enable `Web Server (IIS)`.
> * Click Next until `Role Services`.
> * Enable `CGI`, `ISAPI Extensions`, and `ISAPI Filters` under `Application Development`.
![](/imgs/1.png)
> * Click Next and Install.

> #### Install PHP Manager for IIS
>
> * Download PHPManager via the official [Github](https://github.com/phpmanager/phpmanager/releases). The filename should be `PHPManagerForIIS_x64.msi`.
> * Open and Install PHP Manager.

> #### Install URL Rewrite Module 2.1
> 
> Similar to PHP Manager, download and install URL Rewrite from [iis.net](https://www.iis.net/downloads/microsoft/url-rewrite)

> #### Install PHP
>
> * Create a new directory in `C:\\` called `PHP`. 
> * Download [PHP](https://downloads.php.net/~windows/releases/archives/php-8.5.10-Win32-vs17-x64.zip), and extract the ZIP file into `C:\\PHP`.

> #### Install C++
>
> Download and install [VC_redist.x64.exe](https://aka.ms/vc14/vc_redist.x64.exe).

> #### Install MySQL
>
> * Download and Install [MySQL](https://cdn.mysql.com//Downloads/MySQLInstaller/mysql-installer-web-community-8.0.46.0.msi).
> * Choose `Server Only` for the setup type.
> * On Type and Networking, choose `Server Computer`.
> * On Authentication Method, choose `Legace Authentication`.
> * Set the root password. For this lab, I will use `root` to simplify the process.
> * Click Next/Execute until the Installalation process is complete.

> #### Install HeidiSQL
>
> HeidiSQL will be used for managing the database for osTicket. This will store both user information, ticket information, and any configuration done inside osTicket.
> 
> * [Download HeidiSQL](https://github.com/HeidiSQL/HeidiSQL/releases/download/v12.21/HeidiSQL_12.21.0.7344_Setup.exe) and install with all default options.
> * Create a new session with user/password: `root`/`root`.
> * In the left pane in the new window, create a new database with the name osTicket. (`New > database`)

### Configuring IIS

IIS is the program that manages PHP servers on Windows. This will allow a web browser to communicate to osTicket and vice versa.

* In the search bar, search `IIS`, and run it as Administrator.
* In the left pane, click `lab-server-1`.
* Click `PHP Manager`.
* Click `Register new PHP version`.
* Copy and paste or navigate to `C:\\PHP\\php-cgi.exe`.
* Go back to `lab-server-1` and restart the IIS server.
![](/imgs/2.png)

### Installing osTicket

[osTicket Download](https://github.com/osTicket/osTicket/releases)

* After downloading osTicket, extract and move the `upload` folder to `C:\\inetpub\\wwwroot`.
* Rename `upload` to `osTicket`. (This will be the path in the URL for osTicket.)
* Restart the IIS server again.
* In the left pane, Navigate to `lab-server-1 > Sites > Default Web Sites > osTicket`
* Open PHP Manager.
* Click `Enable or disable an extension`.
* Enable `php_intl.dll`.
* Within `C:\\inetpub\\wwwroot\\osTicket\\include`, rename `ost-sampleconfig.php` to `ost-config.php`.
* Right click and edit properties of `ost-config.php`.
* Under `Security`, Click `Advanced`.
* Click `Change Permissions`, then `Disable Inheritence`.
* Remove all inherited permissions.
* Click Add.
* Click `Select a principal`.
* Type `Everyone`.
* Enable all permissions.
* Apply and Ok.

### Configuring osTicket

* On `lab-server-1`, navigate to http://labserver.com/osTicket.
* Under system settings, name the helpdesk `Helpdesk` and set the email to `help@labserver.com`.
* For admin user, input the same details as the admin account `Jane Doe`.
![](/imgs/3.png)
* Under `Database Settings`, input `osTicket`, `root`, `root` into the `Database`, `Username`, and `Password` fields.
![](/imgs/4.png)
* Click `Install now`. This will create the databases osTicket needs in order to function.

You should now have 2 links. The first is [http://labserver.com/osTicket/scp/login.php](http://labserver.com/osTicket/scp/login.php) for admin access. The other is [http://labserver.com/osTicket/](http://labserver.com/osTicket/) for users to create and view tickets.

> After setup is complete, delete `C:\inetpub\wwwroot\osTicket\setup` folder and change permissions on `C:\inetpub\wwwroot\osTicket\include\ost-config.php` to `Read Only`. 