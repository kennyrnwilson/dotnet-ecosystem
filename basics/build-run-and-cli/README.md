
# CLI
Everything needed to build, test, and run .net core applications is provided via the .NET Core
Command-line Interface CLI. On windows this executable lives in the following folder. The soruce code for this section is in [Source](./src)

```
C:\Program Files\dotnet
```

The following table shows some useful commands. 

| Task                                   | Command                     |
| ---------------------------------------| ----------------------------|
| Show version of driver                 | *dotnet --version*          |
| List installed SDK versions            | *dotnet --list-sdks*        |
| List Installed Runtime versions        | *dotnet --list-runtimes*    |
| List templates                         | *dotnet new --list*         |
| Build .net project                     | *dotnet build*              |

## Using CLI to build and run a .NET Core Application
We can use the CLI to build a run a .NET application. There are a number of files we need to create in order to get our simple application up and running. Before we start we need to understand the difference between the version of the CLI used to build the application and the version of the runtime targeted by the application. 


### CLI Version used to build the app
By default, commands use the latest installed version of the CLI when **building** applications.
If one does not want to use the latest installed version of the CLI we need to create a
*global.json* file at the root level of the application we are building. This file looks as
follows. 

**global.json**

``` json
{
    "sdk": {
        "version": "6.0.101"
    }
 }

```

### Runtime version used to run the application
The version of the runtime is specified by the **TargetFramework** element of the .csproj file.

**.csproj**
``` csproj
<Project Sdk="Microsoft.NET.Sdk">
	<PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <RootNamespace>CommandLineApp</RootNamespace>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>
</Project>

```


### The Main file 
Using .NET 6.0 we can create really stripped down point of entry files. Note that to get this fragment to work I had to add the **ImplicitUsings** attribute to the .csproj.

**Program.cs**
``` cs
Console.WriteLine(args[0]);
Console.WriteLine(Environment.GetEnvironmentVariable("EnvVar"));
```

### Building the application
We can build our application by running the following command from the folder which contains the .csproj file

```
dotnet build
```

### Running the application
To run the application we can execute the command 

```
dotnet run CommandLineApp
```

### Profiles and launchsettings. 
We can add a file called *Properties/launchSettings.json* to specify profiles that contain command line arguments and environment variables we want to pass to the app.

**Properties/launchSettings.json**
```json
{
    "profiles": {
        "CommandLineApp": {
            "commandName": "Project",
            "environmentVariables": {
                "EnvVar": "World"
            }
        }
    }
}
```

Now we can run the application with this profile as.

```
dotnet run --launch-profile CommandLineApp HelloWorld
```

## CLI Templates

The CLI supports templates which can be usd to create a project based on the specified template. To see a list of available templates run the command

```
dotnet new --list
```

If we wanted to create a project from the xunit template we could run 

```
dotnet new xunit 
```

For more information on templates see [dotnet new](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new)


