#Firebird.Embedded

Ez-pz embedding of the fantastic [Firebird SQL RDMS database](http://firebirdsql.org/).

Simply reference Firebird.Embedded in nuget and you're good to go, the required Firebird libraries and files will automatically be copied to your output directory on build.

To start an embedded instance create a connection string like so:

```c#
var connectionString = new FbConnectionStringBuilder
{
    Database = ...,
    ServerType = FbServerType.Embedded,
    UserID = ...,
    Password = ...
}.ToString();
```

Creating the database:

```c#
new FbConnection.CreateDatabase(connectionString, false);
```

Connecting to the database:
```c#
new FbConnection(connectionString);
```

## ASP.Net Core
ASP.Net core will not respect nuget's target file so you have to copy the files manually.

In you `project.json` add the following:

```json
{
    "scripts": {
        "postcompile": ["robocopy /E %USERPROFILE%\\.nuget\\packages\\Firebird.Embedded.3.0.0.32483\\build\\x64\\ %compile:RuntimeOutputDir%\\"]
    }
}
```

Note that if you're not building for the x64 architecture you will have to change it to build to x86 manually.
`%USERPROFILE%\\.nuget\\packages\\Firebird.Embedded.3.0.0.32483\\build\\x86\\ %compile:RuntimeOutputDir%\\`

If you are referencing Firebird.Embedded in another, non .xproj project then you want the following path instead:
`..\\packages\\Firebird.Embedded.3.0.0.32483\\build\\x64\\ %compile:RuntimeOutputDir%\\`

## More Information
http://firebirdsql.org/en/net-examples-of-use/

Don't be frightened by the hideous website, its actually a great database!
