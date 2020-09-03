Dependency injection addresses these problems through:
=====

The use of an interface or base class to abstract the dependency implementation.
Registration of the dependency in a service container. ASP.NET Core provides a built-in service container, IServiceProvider. Services are typically registered in the app's Startup.ConfigureServices method.
Injection of the service into the constructor of the class where it's used. The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.

ASP.NET Web Pages
======
ASP and ASP.NET are server side technologies.

Both technologies enable computer code to be executed by an Internet server.

When a browser requests an ASP or ASP.NET file, the ASP engine reads the file, executes any code in the file, and returns the result to the browser.
======

ASP.NET pages have the extension .aspx and are normally written in C# (C sharp).

ASP.NET Web Pages is an SPA application model (Single Page Application).

The SPA model is quite similar to PHP and Classic ASP.

ASP.NET Web Pages is being merged into the new ASP.NET Core.


ASP.NET Web Pages
=====
Web Pages is one of many programming models for creating ASP.NET web sites and web applications.

Web Pages provides an easy way to combine HTML, CSS, and server code:

Easy to learn, understand, and use
Uses an SPA application model (Single Page Application)
Similar to PHP and Classic ASP
VB (Visual Basic) or C# (C sharp) scripting languages
In addition, Web Pages applications are easily extendable with programmable helpers for databases, videos, graphics, social networking and more.


Because ASP.NET code is executed on the server, you cannot view the code in your browser. You will only see the output as plain HTML.

Razor Markup
Razor is a simple markup syntax for embedding server code (C# or VB) into ASP.NET web pages.
======
Razor Syntax for C#
C# code blocks are enclosed in @{ ... }
Inline expressions (variables or functions) start with @
Code statements end with semicolon
Variables are declared with the var keyword, or the datatype (int, string, etc.)
Strings are enclosed with quotation marks
C# code is case sensitive
C# files have the extension .cshtml


Content Blocks
=====
Many websites have content that is displayed on every page (like headers and footers).

With Web Pages you can use the @RenderPage() method to import content from separate files.

Content block (from another file) can be imported anywhere in a web page, and can contain text, markup, and code, just like any regular web page.

Another approach to creating a consistent look is to use a layout page. A layout page contains the structure, but not the content, of a web page. When a web page (content page) is linked to a layout page, it will be displayed according to the layout page (template).

The layout page is just like a normal web page, except from a call to the @RenderBody() method where the content page will be included.

Each content page must start with a Layout directive.

Hiding Sensitive Information
====
With ASP.NET, the common way to hide sensitive information (database passwords, email passwords, etc.) is to keep the information in a separate file named "_AppStart".

URLs and Paths
========
URLs are used to access files from the web: https://www.w3schools.com/html/html5_intro.asp

The URL corresponds to a physical file on a server: C:\MyWebSites\w3schools\html\html5_intro.asp

A virtual path is shorthand to represent physical paths. If you use virtual paths, you can move your pages to a different domain (or server) without having to update the paths.
