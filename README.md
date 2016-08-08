#Firebird.Embedded

Ez-pz embedding of the fantastic [Firebird SQL RDMS database](http://firebirdsql.org/).

Simply reference Firebird.Embedded in nuget and you're good to go, the required Firebird libraries and files will automatically be copied to your output directory.

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

More here:
http://firebirdsql.org/en/net-examples-of-use/

Don't be frightened by the hideous website, its actually a pretty good database!
