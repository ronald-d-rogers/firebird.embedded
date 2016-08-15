#Firebird.Embedded

![Firebird Logo](http://firebirdsql.org/file/about/firebird-logo-48.png)

Ez-pz embedding of the fantastic [Firebird SQL RDBMS database](http://firebirdsql.org/).

Simply reference Firebird.Embedded in nuget and you're good to go, the required Firebird libraries and files will automatically be copied to your output directory on build.

To start an embedded instance create a connection string like so:

```c#
var connectionString = new FbConnectionStringBuilder
{
    Database = @"C:\Example\Path\To\File.fdb,
    ServerType = FbServerType.Embedded,
    UserID = ...,
    Password = ...
}.ToString();
```

Note that the value for the `Database` option can also be a relative path, but it is relative to the user directory of the user running the application, and not relative to the directory in which the executing process resides (as might be expected).

```c#
var currentDirectory = System.IO.Path.GetDirectoryName(Process.GetCurrentProcess().MainModule.FileName);

var connectionString = new FbConnectionStringBuilder
{
    Database = Path.Combine(currentDirectory, "File.fdb"),
    ServerType = FbServerType.Embedded,
    UserID = ...,
    Password = ...
}.ToString();
```

Create the database:

```c#
new FbConnection.CreateDatabase(connectionString, false);
```

Connect to the database:

```c#
new FbConnection(connectionString);
```

You might want tables also, here's how to do it:

```c#
using (var connection = new FbConnection(connectionString))
using (var command = new FbCommand(
    @"
    EXECUTE BLOCK AS BEGIN

    IF (NOT EXISTS(SELECT 1 FROM rdb$relations WHERE rdb$relation_name = 'EXAMPLE_TABLE')) THEN 
        execute statement 'create table example_table (
            id int not null primary key,
            ...
        );';

    END",
    connection))
{

    if (connection.State == ConnectionState.Closed)
        connection.Open();

    command.ExecuteNonQuery();
}

```

## ASP.Net Core
ASP.Net does not respect nuget package build targets so you have to copy the files manually.

In your `project.json` add the following:

```json
{
    "scripts": {
        "postcompile": ["robocopy /E %USERPROFILE%\\.nuget\\packages\\Firebird.Embedded.3.0.0.32485\\build\\x64\\ %compile:RuntimeOutputDir%\\"]
    }
}
```

Note that if you're not building for the x64 architecture you have to change it to build for the x86 architecture:

`robocopy /E %USERPROFILE%\\.nuget\\packages\\Firebird.Embedded.3.0.0.32485\\build\\x86\\ %compile:RuntimeOutputDir%\\`

If you are referencing Firebird.Embedded in another, non .xproj project and not the .xproj project itself then you want the following path instead:

`robocopy /E ..\\packages\\Firebird.Embedded.3.0.0.32485\\build\\x64\\ %compile:RuntimeOutputDir%\\`

## More Information
http://firebirdsql.org/en/net-examples-of-use/

Don't be frightened by the hideous website, its actually a great database!
