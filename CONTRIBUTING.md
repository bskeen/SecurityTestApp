# Contributing to the SecurityTestApp

Thank you for being willing to contribute to this project! Security is a very important topic in any web app,
and in general, the more people look at code the more secure it can be.

The intent of this app is to showcase the [OWASP Top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) and demonstrate different vulnerabilities and their solutions. Other  vulnerabilities could be displayed also, but the focus is on the Top 10. Please contribute if you would like to introduce more security vulnerabilities and their solutions into the project or if you fix any bugs that exist.

# Filing a Bug Report

Please file all bug reports on [GitHub](https://github.com/bskeen/SecurityTestApp/issues). When creating an
issue, please try to include the following:

- A detailed description of the problem you are experiencing.
- Steps to recreate the bug, including what your inputs were.

# Setting up your Development Environment

The web API is built using SQL Server, .Net Core and Entity Framework Core Code first. This requires Visual Studio 2017 to properly edit/build it (it may also work in Visual Studio Code, but I haven't tried it out yet). Once you open the solution in Visual Studio, it should try to automatically attempt to install all necessary nuget packages for you. If it doesn't, you can open the Package Manager Console and type `Update-Package -reinstall` to force it to install/update the missing packages.

If you make any changes to the models used by Entity Framework, you can generate a database migration to automatically update the databases on all other machines this code runs on. You can use the commands described in [Microsoft's Documentation for Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/miscellaneous/cli/powershell) to create these migrations in the Package Manager Console. Some things to keep in mind are:

- Make sure to look over the migration that was created. It is easy to make a mistake when setting up migrations, and they are usually pretty easy to spot if you look at how the migration will be attempting to update the database.
- Keep in mind that this website might run on Azure. The biggest problem this can cause is that Azure SQL does not allow tables to be without a clustered index. If you are making a change to a primary key column that will drop the primary key, you may have to make your own migration script using SQL that will create a copy of the table with the updated schema, copy over all of the values, drop the old table, and rename the new table to match the old table.
- Before creating a pull request, you should merge the development branch into your local branch, just to ensure that there will be no merge conflicts. When doing this, if you created a migration while working on your current branch, then attempt to run the `Update-Database` command. Sometimes, the snapshot used by Entity Framework Code first can get out of sync when migrations are created on separate branches and then they are merged to a central branch. To fix this, create a new migration, ensure that all of the changes listed in the migration happened in past migrations, and then delete all of the code in the `Up` and `Down` methods. This should reset the snapshot without affecting the database.
- In general, be very sensitive to data that is already in the database. If a database schema change you are trying to implement could affect the data already in the table, then make sure that the changes happen in the least damaging way possible (for example, if you renamed a column, make sure the migration is renaming the column instead of dropping and recreating it). If the migration doesn't handle it well enough, then either update it with the right commands to protect the data, or write custom SQL to do so.
- If you create custom SQL to do anything in a migration, then try to make it as idempotent as possible.

The front end website of the app was made using Angular 5 and generated by the Angular CLI. It is in the folder at `SecurityTestApp.Web\app`, and it was originally developed using Visual Studio Code. You should be able to use your favorite text editor to do this, however. the necessary packages can be installed with an `npm install` command. You can add objects by calling the CLI on a terminal using the [commands found in their documentation](https://github.com/angular/angular-cli/wiki). In order to run the app with the server, just navigate to the root folder of the Angular App in a terminal and run `ng build` (if you include the `--watch` option, then it will also automatically rebuild with any changes). Once it is built, then you can run the server like you normally would in Visual Studio. The build process is configured to create the files in the `SecurityTestApp.Web\wwwroot` folder, so they will automatically be used by the server.

To run tests for the front end, navigate to the `SecurityTestApp.Web\app` folder in a terminal, and run `ng test`. Unit tests for the server can be found in each of the different projects whose names end with `Test` (e.g. `SecurityWebApp.Web.Test`), and can be executed by opening the Test Explorer.

# Submitting Pull Requests

All pull requests should be submitted against the development branch. Before creating the pull request, merge the current development branch into your local branch to make sure that all of the changes are applied. If you created a database migration as mentioned above, then make sure to follow the above instructions dealing with migrations.

# Future Tasks for the Project

The primary focus will be to implement security flaws for all of the OWASP Top 10, along with how to mitigate them. Once that is complete, then other security flaws can be added, along with their solutions. Anything that can be added in order to demonstrate security risks and their solutions will be welcome.

Along with the actual security risks themselves, any security risks added to the project should be documented on the wiki. This will allow for the security risks to be easily found and demonstrated. The following should be included in each wiki article:

- A detailed description of what the security risk is, what causes it, some ways it might be exploited and how to protect your website and server against the threat
- A list of files/line numbers in the code where the security risk and its solution can be found
- Possibly a list of links that describe real-life situations where the security risk happened

# Contact Information

Please voice any questions, comments or concerns you have on Gitter:

[![Join the chat at https://gitter.im/SecurityTestApp/Lobby](https://badges.gitter.im/SecurityTestApp/Lobby.svg)](https://gitter.im/SecurityTestApp/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
