Fluent Command Line Parser
==========================
A simple, strongly typed .NET C# command line parser library using a fluent easy to use interface.
### Download

See what's new in [v1.1](https://github.com/fclp/fluent-command-line-parser/wiki/Roadmap).

You can download the latest release from [CodeBetter's TeamCity server](http://teamcity.codebetter.com/project.html?projectId=project314)

You can also install using [NuGet](http://nuget.org/packages/FluentCommandLineParser/)
```
PM> Install-Package FluentCommandLineParser
```
### Usage
Commands such as `updaterecord.exe -r 10 -v="Mr. Smith" --silent` can be captured using
```
static void Main(string[] args)
{
  var p = new FluentCommandLineParser();

  p.Setup<int>("r")
   .Callback(record => RecordID = record)
   .Required();

  p.Setup<string>("v")
   .Callback(value => NewValue = value)
   .Required();

  p.Setup<bool>("s", "silent")
   .Callback(silent => InSilentMode = silent)
   .SetDefault(false);

  p.Parse(args);
}
```
### Parser Option Methods

`.Setup<int>("r")` Setup an option using a short name, 

`.Setup<int>("r", "record")` or short and long name.

`.Required()` Indicate the option is required and an error should be raised if it is not provided.

`.Callback(val => Value = val)` Provide a delegate to call after the option has been parsed

`.SetDefault(int.MaxValue)` Define a default value if the option was not specified in the args

`.WithDescription("Execute operation in silent mode without feedback")` Specify a help description for the option

### Parsing To Collections

Many arguments can be collected as part of a list. Types supported are `string`, `int`, `double` and `bool`

For example arguments such as

`--filenames C:\file1.txt C:\file2.txt C:\file3.txt`

can be automatically parsed to a `List<string>` using
```
static void Main(string[] args)
{
   var p = new FluentCommandLineParser();

   var filenames = new List<string>();

   p.Setup<List<string>>("f", "filenames")
    .Callback(items => filenames = items);

   p.Parse(args);

   Console.WriteLine("Input file names:");

   foreach (var filename in filenames)
   {
      Console.WriteLine(filename);
   }
}
```
output:
```
Input file names
C:\file1.txt
C:\file2.txt
C:\file3.txt
```
### Supported Syntax
`[-|--|/][switch_name][=|:| ][value]`

Supports boolean names
```
example.exe -s  // enable
example.exe -s- // disabled
example.exe -s+ // enable
```
Supports combined (grouped) options
```
example.exe -xyz  // enable option x, y and z
example.exe -xyz- // disable option x, y and z
example.exe -xyz+ // enable option x, y and z
```
### Development
Fclp is in the early stages of development. Please feel free to provide any feedback on feature support or the Api itself.

If you would like to contribute, you may do so to the [develop branch](https://github.com/fclp/fluent-command-line-parser/tree/develop).