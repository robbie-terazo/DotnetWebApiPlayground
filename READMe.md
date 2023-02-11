# Playground API

## Local Development

### Start the Server

```
dotnet run
```

- Start the server and load the Swagger UI endpoint at https://localhost:7174/swagger.
- Or

```
dotnet watch
```

- Using watch will start the server while enabling hot reload to watch the directory for changes and automatically restart the server.

### Create the SQLite Database

```
dotnet ef database update
```

- See below in the Getting Started section for steps on installing the .NET Entity Framework tools.

### Configure Development Secrets

https://learn.microsoft.com/en-us/aspnet/core/security/key-vault-configuration?view=aspnetcore-6.0

We will be using Microsoft's Azure Key Vault for configuration of secrets in deployment. Configure the development environment using the Dotnet Secret Manager tool.

#### Add User Secrets to the Project

https://learn.microsoft.com/en-us/aspnet/core/security/app-secrets?view=aspnetcore-6.0&tabs=linux

```
dotnet user-secrets init
```

- Adds the `UserSecretsId` element within a `PropertyGroup` of the project file and assigns a unique GUID to the project.
- User secrets are stored in plaintext in the user's home directory organized by the GUID.

```sh
# Windows
%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json

# Linux / MacOS
~/.microsoft/usersecrets/<user_secrets_id>/secrets.json
```

You can add and remove secrets one at a time or replace the values in `secrets.json` in the root of the project directory and pipe the output to the set command of the user-secrets tool.

```
cat secrets.json | dotnet user-secrets set
```

## Getting Started

https://learn.microsoft.com/en-us/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-6.0

Many of the these steps have already been performed, but are included here as a quickstart guide to ensure you have the required software to begin development. Keep reading if this is your first time setting up this project.

### Install the .NET SDK

https://dotnet.microsoft.com/en-us/download

For this project we are using .NET 6.0, which is the current LTS.

For MacOS, it might be easier to manage your installation with Homebrew.

```
brew install dotnet@6
```

- Make sure to follow the instructions to make sure your install location is added to your path.
- Verify your install by running this command in a new terminal.

```
dotnet --info
```

### Restore Project from a Template

https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-web-api?view=aspnetcore-6.0&tabs=visual-studio-code

This project was initiated from dotnet webapi template.

```
dotnet new webapi -o <Projec Name>
```

- The DbContext and Models were created in the Models directory.

### Install ASPNET Code Generator To Scaffold A Controller

https://learn.microsoft.com/en-us/aspnet/core/fundamentals/tools/dotnet-aspnet-codegenerator

```
dotnet tool install -g dotnet-aspnet-codegenerator
```

```
dotnet aspnet-codegenerator controller -name ResourceController -async -api -m Resource -dc DbContext -outDir Controllers
```

- Scaffolds a controller for the selected DbContext and Model and outputs to the Controllers directory.

### Add Nuget Packages to the Project

```
dotnet add package Microsoft.EntityFrameworkCore.Sqlite --version 6.0
```

- Many packages require you to include the --version flag to ensure you install the version that matches your SDK.
- These are saved as project settings in MosaicApi.csproj.
- Dotnet will automatically install required packages when building the project.

### Install .NET Entity Framework Tools

https://learn.microsoft.com/en-us/ef/core/cli/dotnet

```
dotnet tool install --global dotnet-ef
```

- Command line tools for working with Microsoft's ORM, Entity Framework.

### Create Database Migrations with Entity Framework Tool

https://learn.microsoft.com/en-us/ef/core/cli/dotnet#dotnet-ef-migrations-add

```
dotnet ef migrations add InitialCreate
```

- Saves the Database Migration steps from the latest Models into the Migrations directory.
- Commit these files to produce repeatable steps to rebuild the database.
