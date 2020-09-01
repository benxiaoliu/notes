Dependency injection addresses these problems through:

The use of an interface or base class to abstract the dependency implementation.
Registration of the dependency in a service container. ASP.NET Core provides a built-in service container, IServiceProvider. Services are typically registered in the app's Startup.ConfigureServices method.
Injection of the service into the constructor of the class where it's used. The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.
