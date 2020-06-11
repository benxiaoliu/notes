Overview 

The QP architecture is split between the backend and the frontend. The two communicate using JSON POST requests. 

 

Backend 

The backend runs server-side and provides a data API for the frontend. It is broken down into units called "Data Providers." Each data provider communicates with a Bing service, parses the data, and translates it to a JSON string. 

 

Frontend 

The frontend renders the views that users see. Each tab on http://qp/ is what we consider a frontend view. Each view has 4 files: 

XML manifest - provide metadata for the view, such as title and owner 

HTML - this provides some basic markup for your view, and possibly some templates for commonly used page elements (like a single document on a SERP) 

CSS - a stylesheet to define your view's style, like layout, font color, font size, margins, etc. 

JavaScript/Typescript - this actually runs your view. It handles platform events, like query changes. It retrieves data from the backend, and it renders that data. 

 

So what do I need to do? 

If you want to write a plugin, you will definitely need to write a frontend view. Again, this is the actual tab that the user sees. 

 

You may or may not need to write a backend data provider. The best way to figure that out is to talk to qpbel, and discuss with us what data you need. We can compare this with the data currently available, and decide what new data is needed. It is possible we will add this data to an existing data provider, or you may need to implement a new data provider for your data. 

As a way of getting our partners off the ground with QP plug-in development, we have created Visual Studio templates that together implement a complete end-to-end implementation of a plug-in: from the front-end view to the backend data source and back. 

 

The implementation is minimal but it provides a solid foundation for creating a plug-in of any complexity.  Without writing a single line of code, the user gets a plug-in that sends a message to its backend data source and the data source returns the message (plus some more text appended to it.)  The plug-in also demonstrates that it is aware of changes to the Analysis Set: every time that an item is added/removed from the set, the plug-in displays the count of the items in the set. 

 





Unit Tests 
All major components are expected to be covered by unit tests.  A unit test is in this case defined as an isolated, standalone test that validates a single component quickly and reliably.  They are designed to rapidly validate whether a change has caused a regression against the expected behavioral contract for the impacted component. 
Unit tests should take no external dependencies.  As a rule of thumb, any individual unit test should take just a few milliseconds to run and be fully deterministic. 
Where necessary, unit tests will make use of mocks/dependency injection to achieve these goals. 

Functional Tests 
A functional test in this case is defined as a non-isolated test that validates a single component.  These are not designed to exercise the entire stack, but rather to validate a single feature, but without its dependencies isolated.  For example, a component that takes a dependency on an external production service may have a functional test that makes direct use of that service. 

Integration Tests 
An integration test in this case is defined as one which exercises a large number of components interacting with one another.  For example, UI automation tests will interact with the UI and exercise all dependent portions of the system in the process. 


QP test bed: https://qptest/ is on EAP and very familiar with the production environment. Some of the features cannot test on your local box, so you have to use this test bed to finish your test. 

But, there could be more than one developers using this bed in a same time, and the test version/branch could be different. So, we created a branch test for developers to merge their latest test code and won't pollute the master branch. 

How to test on https://qptest/ 
1. Finish your own build test on your local box. 
2. Push your code to remote. 
3. Create a pull request to merge your code into test branch. 
4. Wait for the build validation completed or click auto-completed. Note that you'd better not check the "Delete branch" box to avoid deleting your remote branch. 
5. After the pull request completed, there will trigger an auto-build process, auto-release process. 


Overview 

We have started re-writing some of the existing frontend code in TypeScript.  TypeScript is a strongly-typed language on top of JavaScript which compiles into JavaScript.  For more information about TypeScript, refer to http://www.typescriptlang.org/. 

Our strategy going forward is that when writing new frontend code, we will consider writing it in TypeScript first and only if there are compelling reasons to fall back on JavaScript, we will do so.  Please consult with the QP team before developing any new code in JavaScript. 

To make it easier and faster to get started with developing a plug-in in TypeScript, we have created a set of VS templates under[repo root]\src\queryprobe.product\src\Templates 

 

How To Install A Template 

 
Each template has been packaged into a .zip archive.  For convenience, a batch file called InstallQPTemplate.bat will install a .zip template for you.  It takes the name of the .zip archive as its only argument.  You must run the batch file from within a VS 'Developer Command Prompt' (under All Programs -> Microsoft Visual Studio xxxx -> Visual Studio Tools). 

 

Current Templates 

 

Basic Plugin 

A template for a plug-in that handles all the plug-in events in a minimal fashion 

Comes complete with all the files needed to quickly create a minimal plug-in 

Create a folder for your plug-in, add a new item to the folder and choose the template under C# Web 



How to write a frontend view 

Each view lives entirely in its own directory. You will not need to touch any code outside of this directory for your view. If you think you do, talk to us. These plugin directories are at: [repo root]\src\queryprobe.product\src\QueryProbeClient\src\QueryProbe\Views\ 

 

The best way to learn how to write a view is to read this documentation, and look at other views. 

 

We are moving to typescript.  Please follow these setup instructions: Developing front-end code in TypeScript 

 

Using Open Source software 

If you want to use any Open Source software not already in the QueryProbe codebase, please contact qpbel before doing so. We need to notify LCA of the usage, and copy the license into our LICENCE.txt 

 

Useful libraries we already use 

jQuery - Super easy HTML manipulation, event handling, animations, and AJAX calls 

jQuery UI - library for interactions (dragging, dropping, resizing, sorting) and widgets (accordion, autocomplete, datepicker, dialog, tabs, etc) 

flot.js - a simple, but powerful javascript charting library 

 

Manifest 

At a minimum, a view must contain a manifest.xml file.  This provides a list of other resources that are necessary to render this view.  Each view should be in its own self-contained directory under [repo root]\src\queryprobe.product\src\QueryProbeClient\src\QueryProbe\Views, and the manifest must be named "manifest.xml."  It has the following format: 

 

<?xml version="1.0" encoding="utf-8" ?> 

<View> 

  <Title>View Name</Title> 

  <Description>View Description</Description> 

  <Content>ViewContent.html</Content> 

  <Thumbnail>Thumbnail.jpg</Thumbnail> 

  <Beta>True</Beta> 

  <EntryClassName>MyView</EntryClassName> 

  <EntryNamespace>QueryProbe</EntryNamespace> 

  <Events> 

    <Event> 

      <Name>CartChange</Name> 

      <Handler>onCartChange</Handler> 

    </Event> 

    <Event> 

      <Name>GetResultSet</Name> 

      <Handler>getResultSet</Handler> 

    </Event> 

    <Event> 

      <Name>QueryContextChange</Name> 

      <Handler>onQueryContextChange</Handler> 

    </Event> 

    <Event> 

      <Name>ShowView</Name> 

      <Handler>onShow</Handler> 

    </Event> 

    <Event> 

      <Name>TabHide</Name> 

      <Handler>onViewBlur</Handler> 

    </Event> 

    <Event> 

      <Name>UnloadView</Name> 

      <Handler>onUnload</Handler> 

    </Event> 

  </Events> 

  <Groups> 

    <Group>Filter1</Group> 

    <Group>Filter2</Group> 

  </Groups> 

  <Scripts> 

    <Script>ViewName.js</Script> 

    <Script>HelperClass.js</Script> 

  </Scripts> 

  <StyleSheets> 

    <StyleSheet>ViewName.css</StyleSheet> 

  </StyleSheets> 

  <Resources> 

    <Resource>LogicalResource1</Resource> 

    <Resource>LogicalResource1</Resource> 

  </Resources> 

</View> 

 

The required fields are (marked in bold): Title, Content, EntryClassName, EntryNamespace and at least one Resource.  The others are optional.  <Scripts> contains a list of one or more .js files that represent the code behind a view, and <StyleSheets> contains the list of .css files that help drive the visual style of the view. 

 

Most views will have one .css file and one .js file.  The .js file will contain the implementation of the "View object." 

If you have a TypeScript file, still use the .js extension (as TypeScript will generate JavaScript on compilation). 

If you have more than one new .js file, make sure to add all of them to the Scripts group, not just the one that controls the view. 

 

View object/event model 

For a view to do anything meaningful, it must be backed by a view controller.  This is a class that the content metadata points to that responds to the events produced by the QueryProbe framework. 

 

The most basic view controller will be referenced as follows: 

 

  <EntryClassName>MyView</EntryClassName> 

  <EntryNamespace></EntryNamespace> 

  <Events> 

    <Event> 

      <Name>ShowView</Name> 

      <Handler>onShow</Handler> 

    </Event> 

  </Events> 

 

Name "MyView" something appropriate given the purpose of your view.  Then provide an implementation of MyView, for example: 

 

function MyViewClass(clientContext, viewEntry) { 

var $containingElement = viewEntry.getContainingElement(); 

 

this.onShow = function() { 

$containingElement.find("myDiv1").append("my content1"); 

$containingElement.find("myDiv2").append("my content1"); 

} 

}; 

 

ClientContext clientContext: a collection of system objects that enable you to interact with the platform. 

ViewEntry viewEntry: parent object which represents the view instance and enable you to interact with the data. 

JQuery $containingElement: a jQuery collection pointing to the root HTML div on the page in which this view's content was injected.  All HTML manipulation should be relative to this root selector. 

 

All communication with views happens through events, which usually have no arguments. 

 

onShow() is triggered every time, when a view is shown to user. 

 

State management 

The QueryProbe platform uses state objects to help store and repopulate views based on their previous state.  For example, when navigating back in the browser history stack, state may be used to restore views to how they were displayed for a previous query.  Also notably, states are saved when generating permalinks.  Finally, state is used for saving a user's default settings. 

 

Views can create an arbitrary JavaScript object to represent their current state, by creating an "asSerializable" method.  This should return an object that can be serialized using JSON.  You can always obtain the previous state from parent viewEntry object, sometimes it could be null (for the newly created view). 

 

For example, if I have a view that has a text entry field, I might want to save the contents of that text entry field so it is repopulated when a permalink is generated.  So I'll add an asSerializable method, and also handle the state in constructor: 

 

function MyViewClass(clientContext, viewEntry) { 

var $containingElement = viewEntry.getContainingElement(); 

 

 

init = function() { 

// Using a default object helps protect you against backwards compatability. 

// As you add new fields, you can add defaults here, and $.extend fill in 

// any missing fields from initialState with your defaults. 

var DefaultState = { 

TextInput: null 

}; 

var state = $.extend({}, true, DefaultState, viewEntry.getPreviousState()); 

$containingElement.find(".my-text-input").val(state.TextInput); 

} 

 

this.asSerializable = function() { 

var state = { 

TextInput: $containingDiv.find(".my-text-input").val() 

}; 

return state; 

} 

 

init(); 

}; 

 

Note that this mechanism is used for the "Save Default State" feature of QueryProbe as well.  This means that it may be used for users of your view every time they load QueryProbe.  As a consequence, take care about what values you save.  If something you save is dependent on the query context, you may want a mechanism that will allow you to override the saved state with a value from the query context when the two disagree. 

 

Best Practices 

DOM 

Never manipulate the DOM outside of the $containingDiv sent into your event handlers.  All HTML manipulation should happen within your <div>, and all jQuery operations should start with this collection object. 

 

Never use: $(".my-class") 

Always use: $containingDiv.find(".my-class") 

 

Class Selectors 

In addition to selecting relative to the plugin's containing element, by convention, all selections of elements within a plugin should be done by class & element (selection by id is discouraged). 

 

Context Changes 

When the "query context" is changed, views are notified of this event so they can update to reflect the new query or settings under analysis.  This helps avoid an expensive full-page reload, and keeps the current state and views. 

 

Views do not have to override this event, but if they don't, our "coarse" reload logic will go into effect.  This will fully un-load the view and then re-initialize it with the next context.  If your view is query context dependent, it is best to respond to this event; your users will get a much more seamless experience, and your view should update faster. 

 

Unload and Async Operations 

When a tab is closed, "UnloadEvent" is triggered.  Plugin writers should use this to invalidate any asynchronous callbacks that are currently in flight, as the completion callbacks for these operations may operate on portions of the DOM that have been removed since the view has been removed. 

 

We provide "SafeCallbackController" utility that will ease the process of dealing with this.  Its use is as follows: 

 

var callbackController = new SafeCallbackController(); 

var callback = callbackController.CreateSafeCallback(function() { 

    $div.find(".my-div").val("foo"); 

}); 

asyncOperation(callback); 

// at this point, the callback function will be called normally if it  

// completes before this next line of code 

callbackController.Invalidate(); 

// now, any calls to callback() will be ignored (callback() becomes a no-op) 

 
 
 View Event Model 

The following events are currently supported by the QueryProbe framework.  

All events are optional.  Most views will need to at least provide an implementation for LoadViewEvent and QueryContextChangeEvent.  See the notes in UnloadEvent if you do any asynchronous operations. 

 

LoadViewEvent 

This event is fired when a view is first loaded.  You can view this event as your "constructor," as it's called exactly once after your view object is created. 

 

CartChangeEvent 

This event is fired when an item is added or removed from the analysis set, or when additional data has been fetched for an item in the analysis set.  It has no additional parameters.  You can use the Cart item in clientContext to fetch details about the current state of the cart. 

 

ShowViewEvent 

This event is fired when a tab is brought into view.  This allows for expensive operations to be delayed until a user is actually looking at your view, and allows you to do any redrawing that can't be done while your div is in a hidden state.  It has no additional parameters. 

 

TabHideEvent 

Probably should be renamed "HideViewEvent", as this is its corollary.  If you display any invasive UI (e.g. dialog-style popups), this is a good place to clean them up before you are hidden.  It has no additional parameters. 

 

QueryContextChangeEvent 

This event is fired when the global "Query Context" has been changed.  This generally means the user has changed the query or settings in the query box on the top of the page.  clientContext.QueryContext contains the updated query context, and can be used to fetch the updated query or settings. 

 

If you do not provide an implementation for this event, your tab will be fully re-loaded when the query context changes.  To avoid this, if you aren't dependent on the query context, create an empty event handler for this event. 

 

UnloadEvent 

This event fires once when your view is destroyed.  You can view this event as your "destructor."  Do any clean-up you need to do here.  Of particular note: if you triggered any async calls, you should invalidate them here so they don't trigger errors when they complete by trying to write to your <div>, since that <div> no longer exists.  It has no additional parameters. 

 

Working with data 

 

You can find additional info about how to work with data in the blog post Verticals and Logical Resources in QueryProbe  









QueryProbe Data Access Basics 

The QueryProbe data access model is designed to provide a plugin-based model for providing new data providers.  These providers can act as new sources of existing data, as well as providers of new pieces of data entirely.  Partners looking to plug in their own functionality can do so simply by adding a new assembly that exports key data types which represent these primitives in the system.  Their new types should then be accessible through the REST protocol through which QueryProbe data is accessed. 

 

Primitives 

The QueryProbe data access model has three primitives: 

 

Data: This represents a piece of data produced by the QueryProbe data service.  Examples of data are "Result" and "FeatureVector."  These are abstract types, separated as much as possible from the specific data sources that can fulfill them.  It is expected that more than one data source can produce any given data type. 

DataParameter: This is a type that represents any details necessary to describe a piece of Data being requested.  This should represent any details about how to fetch a piece of data that is not data source specific.  For example, when requesting a list of Results, specifying the query text and how many results are desired would be a sensible field in a "SearchResultsDataParameter" type sent along with that request. 

DataSource: This is a provider that can produce a requested Data type given a list of DataParameters and a DataSourceSetting object that's tightly bound to the corresponding DataSource.  There may be many DataSource implementations that can produce the same piece of Data given the same DataParameters, but using a different mechanism of data access. 

 

The data service's fundamental role is to dispatch a Data/DataParameter[] pair to a requested DataSource.  The DataSource is then responsible for producing one or more instances of Data to fulfill the request. 

 

REST API 

NOTE: This REST API is currently being re-considered and cleaned up.  This section covers the "old" behavior, and is subject to change. 

 

These primitives directly map to a REST API that provides the ability to create Data types.  The resource /QueryProbeDataService.svc/DataComposition is a fixture of the data service, and accepts a POST request to create a new DataComposition with the following JSON request object: 

 

{ 

"DataParameters": [] 

"DataSourceSetting": { } 

} 

 

Where DataParameters is a list of JSON-serialized DataParameter objects and DataSourceSettings is a JSON-serialized DataSourceSetting object, which can serve as a factory to create a new DataSource type. 

 

All these types are serialized/deserialized using WCF.  So [DataContract] and [DataMember] is used on the types, and all [DataMember] properties should be specified in the JSON serialized object.  An extra __type property on each object determines the derived type that is represented by the containing JSON object. 

 

When receiving this request, the data service will create a new DataSource object through the factory method on DataSourceSetting, then pass in the DataParameters.  The DataSource is responsible for producing one or more Data objects based on the input parameters, which are then serialized and returned as the response body of the /DataComposition request. 

 

Introducing New Primitives with MEF 

MEF is a .NET technology for dynamically detecting implementations of target types through reflection.  It uses a simple [Import] and [Export] model to do binding based on specific exported interfaces.  The QueryProbe platform uses MEF to find new implementations of the 3 key primitives involved in exposing new data: Data, DataParameter, and DataSourceSetting. 

 

DataSourceSetting, as a factory type, is responsible for getting a corresponding implementation of DataSource to allow data requests to be fulfilled by the composition. 

 

See the walkthrough OneNote document for full details on how to work within this framework to add your own data to the QueryProbe data service. 


New Data Source Walkthrough 

NOTE: Starred items  indicate areas of desired follow-up from the QueryProbe team.  These processes may change in the future. 

 

Adding a new data source assembly enables you to add a new source of information to be surfaced by the QueryProbe data service.  There are two potential categories of data sources: 

 

Building a new data source for existing data (e.g. providing an alternate source for Result objects). 

Building a new data source for new data.  This means you're providing data that only has one source, and isn't in the existing QueryProbe data access model. 

 

Before you begin, please contact qpbel and let us know what you're interested in doing.  We can help you navigate our existing library of types and ensure there isn't already a good fit for what you're trying to accomplish. 

 

Creating Your Assembly 

First, follow the steps in "Running the Project" to get the QueryProbe development environment set up. 

 

Create a new directory under private\relevancetools\QueryProbe\Backend\Partners\<YourDataSource>.  Our convention is to have a "src" and "unittest" directory, housing your main data source assembly and its associated unit tests, respectively. 

Create a new project file.  The pre-amble should be edited to look like the following: 
 

<?xml version="1.0" encoding="utf-8"?> 

<Project ToolsVersion="4.0" DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003"> 

  <PropertyGroup> 

    <RootNamespace>%YourNamespace%</RootNamespace> 

  </PropertyGroup> 

  <Import Project="$(EnvironmentConfig)" /> 

  <Import Project="..\..\..\msbuild\Probe.settings" /> 

  <PropertyGroup> 

    <ProjectGuid>{%NEW_GUID%}</ProjectGuid> 

    <OutputType>Library</OutputType> 

    <AssemblyName>%YourAssemblyName%</AssemblyName> 

  </PropertyGroup> 
 
With the %% variables replaced appropriately.  After all the Compile and ProjectReference/References, the end of the file should only need the following: 
 

  <Import Project="$(QueryProbeBuildRoot)\Probe.DataSource.targets" /> 

  <Import Project="$(QueryProbeBuildRoot)\Probe.CSharp.targets" /> 

</Project> 

 

It is easier to copy an existing project as a template, but you can also do this manually. 
 

For unit tests, you'll also want to follow the steps in Creating a Test Assembly to hook up the unit tests to the automated test run. 

ProjectGUID can be generated at http://www.guidgenerator.com/ 

Add your assembly to private\relevancetools\QueryProbe\msbuild\dirs.proj so it is included in the build and the central QueryProbe solution. 

Add an entry for your project to private\relevancetools\QueryProbe\Backend\DataSources.include.  This will ensure that your data source is built before the data service that must consume it, and in conjunction with including Probe.DataSource.targets, will ensure your data source .dll, and all direct references, are deployed to the right place. 
 

    <ProjectReference Include="$(QueryProbeRoot)\Backend\Partners\AresDataSource\src\AresDataSource.csproj"> 

      <Project>{5FD17D29-4EE8-46B2-8980-FCC9C7BA8BBF}</Project> 

      <Name>AresDataSource</Name> 

    </ProjectReference> 

 

 

 

 

 

 

 

 

Your assembly is now wired into the build and available to be included in the DataService data composition. 

 

Creating Your Data Source 

In your project, add a reference to the "Common" project.  This includes the abstract base class types for all major primitives.  At a minimum, to produce data, you'll need to create a DataSource type, and optionally a DataSourceSetting type if you require configurable settings (e.g. market).  You must use attributes to expose these to the Queryprobe composition engine, which is powered by MEF.  See below for the necessary attributes. 

 

Your data source class should inherit from QueryProbe.Common.ClassContracts.DataSource.  It must have a constructor with the attribute [ImportingConstructor] that takes as arguments all of your DataSource's dependencies. 

 

DataSource must meet the following criteria: 

Inherit from QueryProbe.Common.ClassContracts.DataSource 

Use the [DataSourceExport(typeof(YOURTYPE))] to publish itself to QP 

Have a constructor with [ImportingConstructor] that accepts all of the class's dependencies 

Have at least one method with [Resource] that defines data published by the QP backend web service.  Your client code will consume this [Resource] data. 

 

An example data source type is as follows: 

 

    [Export(typeof(DataSourceSetting))] 

    public class SampleDataSource: DataSource 

    { 

        [ImportingConstructor] 

        public SampleDataSource(SampleDataSourceSetting settings, ActionLogger actionLogger ) 

            : base(actionLogger) 

        { 

        }         

         

        [Resource] 

        public YourData YourResourceMethod(YourParamater parameter) { ... } 

    } 

 

DataSourceSetting must meet the following criteria: 

 

Inherit from QueryProbe.Common.ClassContracts.DataSourceSetting. 

Provide [DataContract] attribute on the class, and [DataMember] on any property that needs to be sent from callers of the REST API. 

Provide a [MetadataProvider] attribute that helps provide default values and enumeration options for your [DataMember] fields.  In many cases, you can simply use DefaultMetadataProvider<T>; see the example below. 

Provide an [Export(typeof(DataSourceSetting))] attribute that makes this type available to MEF. 

 

An example setting type is as follows: 

 

    [DataContract] 

    [MetadataProvider(typeof(DefaultMetadataProvider<DocInfoDataSourceSetting>))] 

    [Export(typeof(DataSourceSetting))] 

    public class DocInfoDataSourceSetting : DataSourceSetting 

    { 

        [DataMember] 

        public string Index { get; set; } 

    } 

 

Optional - Expose a strongly typed interface for API use 

Create a new interface named IYourDataSource, as follows 

[DataSource] 

[DataSourceSettings(typeof(YourDataSourceSetting))] 

public interface IYourDataSource : IDataSource 

{ 

    // one method per [Resource] in your DataSource implementation 

} 

In your DataSource implementation from above, make the following changes 

Remove the [DataSourceSettings] attribute, if any 

Change [DataSourceExport(typeof(YourDataSource))] to [DataSourceExport(typeof(IYourDataSource))] (i.e. use the interface instead of the implemented type) 

That's it!  Your datasource can now be used with the strongly typed QP client library in C# 

 

Create Data/DataParameters 

In some cases, you'll be using existing, common Data and DataParameters types for your new data source.  In this case, this step is unnecessary.  But if you need to introduce your own unique Data or DataParameters types, you can do that as well within your assembly. 

 

The process is similar to creating your own DataSourceSetting.  However, a MetadataProvider is not necessary for Data types. 

 

     
    public class DocumentDetails : Data 

    { 

        public DocumentDetails() 

        { 

            CrawlEvents = new List<CrawlEvent>(); 

        } 

 

        [DataMember] 

        public IList<CrawlEvent> CrawlEvents { get; set; } 

    } 

 

    [DataContract] 

    [MetadataProvider(typeof(DefaultMetadataProvider<FeaturesParameter>))] 

    [Export(typeof(DataParameter))] 

    public class DocumentDetailsParameter : DataParameter 

    { 

        /// <summary> 

        /// SourceEnvironment field from ExtendedResult. 

        /// </summary> 

        [DataMember] 

        public string SourceEnvironment { get; set; } 

 

        /// <summary> 

        /// DocId field from ExtendedResult. 

        /// </summary> 

        [DataMember] 

        public long DocId { get; set; } 

    } 

 

DataSource Configuration 

A DataSource may optionally have its own configuration file.  This will be managed by the platform, and passed into the data source in two ways: 

 

As a parameter to your importing constructor (simply add a ConfigReader configReader argument) 

As a constructor parameter for IMetadataProvider 

 

To add a configuration type, add a new file in your data source project called <YourAssemblyName>.dll.config.  For example, for "DocInfoDataSource", we create a DocInfoDataSource.dll.config.  Add it as a content item, set to copy to the output directory if newer: 

 

Pr oper bes 
BingDataSource.dILconfig File Proper tes 
Suild Action 
Copy to Output Director Cop,' if newer 
Custom Tool 
Custom Tool Namespace 
aingDa taSour ce dll con fig 
Full Path 
 

Next, edit your DataSource .csproj file and add a propery group like the following before the inclusion of Probe.DataSource.targets: 

 

  <PropertyGroup> 

    <DataSourceConfiguration>$(OutputPath)\BingDataSource.dll.config</DataSourceConfiguration> 

  </PropertyGroup> 

 

This will ensure that the config file is seen and managed by the data service.  Now you can access any keys in the configuration file using the ConfigReader passed into your data source as described above. 




Installation Requirements/Recommendations 

VS2017 Enterprise, with all C/C++ and Windows SDK options installed. May also need to install MVCRT 14 

CodeFlow 

Developing front-end code in TypeScript 

ReSharper (optional) 

Git - tool installations and important reading 

https://git-scm.com/doc 

https://www.bingwiki.com/Learning_Git 

https://askms.cloudapp.net/9085/what-git-workflow-should-asg-engineers-use 

https://github.com/Microsoft/Git-Credential-Manager-for-Windows/releases/latest 

Set up a local copy of the QueryProbe git repository 

QueryProbe code is in its own git repository. Here is the link to VSO: https://msasg.visualstudio.com/Bing_and_IPG/_git/QueryProbe 

If you haven't already, create a directory like d:\git that contains all of your git repositories 

The following steps assume you are in a powershell window and have git installed (and guide you in that direction). If you are a windows shell person, feel free to extend/augment the instructions and tooling to support this. 

In d:\git, run the below. This will create a directory d:\git\QueryProbe 

The main QP source is under d:\git\QueryProbe\src\** 

git clone https://msasg.visualstudio.com/DefaultCollection/Bing_and_IPG/_git/QueryProbe 

Run the following to create a shortcut that automatically initializes corext and powershell. After it is created, change its settings to always "Run As Administrator" (Properties => Shortcut tab => Advanced). For advanced usage and argument help, use the command-line help or run the command with no arguments 

pWindows shell (run from d:\git\queryprobe) 

init.cmd&&corext shortcut /name QueryProbe /runasadmin 

Note: follow any install steps, resolve any standard errors/warnings that the standard tools tell you. If you don't, any subsequent instructions are wasted. 

Close/reopen the shortcut until all installs, errors, and warnings go away 

Set the following git cf configurations to ensure a clean master branch in the repo: 

[gitcf] 

           deleteremotesourcebranch = yes 

           squashmergepullrequest = yes 

Decrypt the qptestAzureKey.txt.encr file located in \src\QueryProbe.Product\src\DataService\src by using SsTool. Contact qpbel if you need permissions to use the SsTool key 

Add a function in your powershell profile to run cleanWebConfigs.ps1 and copy the decrypted key file (see below) after a successful build. It should resemble this: 

function qppb { 

    d:\src\queryprobe\src\QueryProbe.Product\src\msbuild\cleanWebConfigs.ps1 

    copy d:\qptestAzureKey.txt $env:INETROOT\target\distrib\debug\amd64\app\QueryProbe\QueryProbeClient 

    copy d:\qptestAzureKey.txt $env:INETROOT\target\distrib\debug\amd64\app\QueryProbe\QueryProbeDataService 

} 

Run the following and ensure you get a clean build. If so, your code repo is ready! 

Quickbuild & qppb 

There are a number of git and powershell setup steps that will make your life easier. Talk to your resident git expert for these, they should all play nicely with your QueryProbe repo! Quick ones: difftool, mergetool, default editor, personal powershell macros, a personal cross-repo "dev tools" repo for environment customization 

 

Enlist in searchgold and APGold 

In the command prompt, in your work directory, create a new sub-directory for the searchgold and APGold projects. (such as c:\src\ searchgold and c:\src\APGold) 

Change to your searchgold directory and then enlist in the searchgold project by running the following command:   

\\codeinfo\enlist\data\enlistme.cmd queryprobe 

 

Machine generated alternative text:
Command prompt searchgold 
: ir searc hgo Id 
: Sspc>cd searchgo Id 
searchgold 
[Copen I [Enlistment ] Running 
ing 
heck in g v 
reSS any key to C Ontinue . 
Like before, the enlistment for searchgold will create a new shortcut on your desktop called “corext searchgold bsdsearchgold 7727”.   

You will need to run this shortcut to open the power tools command shell for the searchgold enlistment. 

Run the shortcut to launch power tools for your searchgold  enlistment.  This opens a new command prompt window. 

NOTE: Do not run the searchgold enlistment as an administrator. If run as an admin, it will open a command prompt into system32 instead of the searchgold folder. 

Optional Exclusions - some folders are large and used for a subset of plugins. Include one or more lines of the below to your sd client in order to prune your enlistment: 

Cd  

NOTE: If you don't enlist at c:\src\searchgold, consider creating a symbolic link to make this path valid.  From a cmd.exe prompt, issue the command: 
sd sync 
mklink /d c:\src\searchgold <your searchgold root> 
 
Once this is done, you should see that c:\src\searchgold points to your searchgold enlistment directory. + 

Now run “sd sync” to make sure your searchgold enlistment is up to date. 

Get an APGold enlistment by navigating to your new APGold directory and running 
\\codeinfo\enlist\data\enlistme.cmd  apgold 
As before, this will create a new shortcut window. Run "sd sync" after opening it so everything is up-to-date 

Create another symbolic link to point your searchgold\AutopilotService directory to APGold\AutopilotService (the autopilot service files were moved to APGold recently, but QP expects to find them in the old location). The command will be similar to 
mklink /d c:\src\searchgold\AutopilotService c:\src\APGold\AutopilotService 
This will work even if your searchgold folder is itself a syk (if the physical path is on the D: drive, for instance) 

 

Setup TypeScript 

Front-End TypeScript 

 

Setup IIS  

A number of web hosting components are required, that aren't installed with IIS by default, and whose defaults may vary by OS (windows 10, windows server, etc). Apply the settings seen below. 

 

To get to this on many systems: 

Select Programs and Features (first item on list) 

Select Turn Windows Features on or off on the left 

 

Some known important gotchas for this step: 

Be sure to install the *.svc handlers. They are under .Net Framework 4.5 Advanced Services => WCF Services => HTTP Activation 

The symptom of these missing will be 404 or NotFound responses from QueryProbeDataService 

 

(need to do this with unique port# for each repo clone) 

Go to Control Panel > Programs and Features > Turn Windows features on or off. Make the settings match these 

Run quickbuild command in the repo root 

Open "inetmgr" from the Start button 

Create two new websites and change the settings under "Binds and Basic Settings" 

QueryProbeClient 

Site name: QueryProbeClient 

Bindings type: https 

Bindings port: 52300 

SSL Certificate: IIS Express Development Certificate 

Physical path: [path to repo]\target\distrib\debug\amd64\app\QueryProbe\QueryProbeClient 

Connect as: REDMOND\ [your alias] (I needed to use alias@microsoft.com -darren) 

QueryProbeDataService 

Site name: QueryProbeDataService 

Bindings type: https 

Bindings port: 52400 

SSL Certificate: IIS Express Development Certificate 

Physical path: [path to repo]\target\distrib\debug\amd64\app\QueryProbe\QueryProbeDataService 

Connect as: REDMOND\ [your alias] (I needed to use alias@microsoft.com -darren) 

Add http binding to QueryProbeDataService 

Select QueryProbeDataService under Sites 

In Actions pane on the right, select Bindings… 

Add an http binding to any available port (I used 52401 -anlowe) 

Go to Application Pools and make the following changes under Advanced Settings 

QueryProbeClient 

Framework version: v4.0 

Run as "REDMOND\[your alias]"  (ProcessModel -> Identity field using  alias@microsoft.com -darren) 

Disable 32-bit applications 

QueryProbeDataService 

Framework version: v4.0 

Run as "Network Service" (ProcessModel --> Identity --> choose from drop down) 

Can choose to run as "REDMOND\[your alias]" instead.  You may run into some permissions issues with Network Service if you are accessing files outside the repo on your local machine. 

Disable 32-bit applications 

Enable Windows Authentication and disable Anonymous Authentication for QueryProbeClient 

On the "Features view" with the site selected, double-click Authentication 

Select anonymous authentication and click "Disable" under the actions bar to the right 

"Enable" windows authentication 

With Windows Authentication selected, click "Providers" and add "Negotiate:Kerberos" to the list of providers.  You can ignore any warnings you get here. 

Add localhost certificate to Trusted Root Certification Authorities Store 

Open Run (Win --> Type "run" --> Enter) 

Type "mmc" and Enter 

Add certificates snap in: File --> Add/Remove Snap In 

Select Certificates and click Add 

Select Computer Account and Next 

Select Local Computer and Finish 

Click OK 

In tree on left, navigate to Certificates --> Personal --> Certificates 

Export localhost certificate (Friendly Name: IIS Express Development Certificate) 

Right click localhost certificate --> All Tasks --> Export… --> Next 

Do not export private key 

Format: DER encoded binary X.509 (.CER) 

Save anywhere on drive 

Import certificate into Trusted Root Certification Authorities Store 

In tree on left, right click Trusted Root Certification Authorities --> Certificates 

All Tasks --> Import… 

Browse and select certificate exported in previous step and click Next 

Select Place all certificates in Trusted Root Certi fication Authorities store 

 

Develop  

If using VS 2012, depending on the OS that is running your machine, you may need to install .NET framework 4.5.1 and/or Microsoft Build Tools 2013. 

Open your corext shortcut  

go to queryprobe\src\QueryProbe.Product\src\msbuild folder and run vsmsbuild dirs.proj 

Use ReSharper or Nunit Test Adapter to easily run NUnit tests from within VS. They will also be run when doing quickbuild from the command line 

You can also view the sites without launching VS by going to http://localhost:52300/ in any browser. Chrome is the recommended browser for this: Edge tends to have authentication problems for the selfhosted site 

 

Running the Project 

Right click on the solution node in solution explorer, then select properties.  On the "Startup Project" tab, select "Multiple startup projects", then set both DataService and QueryProbeClient to "Start".  These settings should generally persist unless you delete the intermediate files created by vsmsbuild. 

Create a link from c:\src\searchgold to your actual searchgold root, using mklink.  This ensures that the various references to searchgold in config and code work w/o modification. 

QueryProbeClient 

Right click on QueryProbeClient and click Properties 

Go to Web, and select "Use Custom Web Server" or "External Host" (depends on Visual Studio version) 

Change Server URL to: http://localhost:52300/ 

DataService 

Right click on DataService and click on Properties 

Go to Web, and select "Use Custom Web Server" 

Change Server URL to: http://localhost:52400/ 

Hit F5, and both sites will launch. 

If you'd like to point the frontend at another data source, disable startup of the DataService project, and update web.config in QueryProbeClient to point to another instance of the running service. 

Alternate method: JoshM's AutoIIS Method 

 

Access localhost from a different machine 

If you would like to access your localhost (via http://YourMachineName:52300) from a different machine, you would need add firewall inbound rules for your ports 52300 and 52400. 
 

Steps: 

Open “Window firewall with advanced security”  

Select “Inbound rules” 

Under “Action” tab, choose “New rule” 

In “Rule Type”, choose “Port” and click “Next” 

Add your ports “52300, 52400” 

Keep clicking “Next”, and then add a name (such as “Query Probe”) in “Name” field. 

 
