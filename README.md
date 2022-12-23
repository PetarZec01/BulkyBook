# BulkyBook

This is tutorial project for ASP.NET C# Core MVC <br />

Tutorial link: https://www.youtube.com/watch?v=hZ1DASYd9rk&t=0s<br />

I built using this tutorial a category crud project.<br />
Tools used: .NET 6, Visual Studio 2022, SSMS 2018, SQL Server<br />
For database creation EntityFrameworkCore and EntityFrameworkCore.SqlServer were used.<br />
To customise the look of our project we will use Bootstap and download a theme from Bootswatch and add icons from Bootstrap Icons. Also we will use custom notifictations from toastr <br />
Theme: https://bootswatch.com/morph/
Icons: https://icons.getbootstrap.com/
Alerts: https://github.com/CodeSeven/toastr

<b>1. .NET Core:</b> <br />
This is the biggest change Microsoft .NET made. First introduced Webforms then .NET MVC but it was only runnable on Windows platforms.<br />
To keep up with changing technology Microsoft introduced ASP.NET core which is multiplatform and they had cloud in mind.<br />

<b>1.1 Advantages of .NET Core: </b>

- fast and open source
- cross platform
- built in supprt for dependency injection
- easy updates
- cloud friendly
- phenomenal performance

<b>2. Dependency injection</b> (integral part of ASP.NET Core):<br />
.NET Core inject objects of dependency classes through constructors by using the built in IoC container.<br />
Without this it there would be duplicate code if we want to implement database connections or an email service (a lot of opening, closing etc.)<br />
With dependency injection we get a container which will contain interfaces for what we want to implement on a certain page.<br />
If multiple pages want to access database for example it will ask the container to create an object of this functionality which can be used for easier implementation.<br />
Dependency injection will automaticly manage, dispose, implement and instance of objects so we don't have to do manually.<br />
It will be easier to modfy the instace class, we modify it only in the class and it will automaticly use the new implementation on all pages<br />
<br />
<b>3. FILES AND FOLDERS:</b><br />

- <b>Project file</b> contains the .NET version in the PropertyGroup and what packages we are using in the project are stored in the ItemGroup
- <b>Properties\launchSettings.json</b> which contains different profiles in which we can run our application (what port is used, where is it hosted)
- <b>wwwroot</b> contains static files (css, images, js code, libraries), does not contain C# files
- <b>appsettings.json</b> we put all secrets in here of our project (connection strings, API keys, SendGrid keys, stripe payment keys) and if we need other appsettings for different enviroments (staging, production)
- <b>Program.cs</b> file running for the application. We have the builder. If we want to register with our dependency injection we do it here. Also we configure the HTTP pipeline. The pipeline specifies how application should respond to a web request. (when browser requests something it goes back and forth in the pipeline). We put items that we want into the pipeline and is made with multiple middlewares (MVC is a middleware that we are using) (you can use other middleware for authentication, authorisation). What is in the pipeline it gets modified by the middleware and gets passed till the last and the response is returned to the server. Our application is using to se if it is in Development mode so it can send you Developer friendly exceptions so you can solve problem easier. Next middleware is to use redirection with http. Then middleware of our static files. We have a rouitng middleware for routing through pages. Also there is a Authorisation middleware and without the Authentication middleware which you can add. Thre is MapControllerRoute this will route requests to corresponding controllers and action. Lastly the Run middleware which runs the app. The order is really important because it is exactily how the request will be passed.
- <b>Models </b>it will contain all the data related models that we want in the project (example products) (it will be explained in MVC Architecture chapter 4.)
- <b>Views </b>
- <b> Controllers</b>

<b>4. MVC Arhitecture (Model / View / Controller):</b>
<b>Model</b> represents the shape of the data. C# is used to describe a model. The model corresponds with all the data related logic that user works with. It can be a table that we are storing in the SQL Server or it can be a model that contains multiple tables. This model can represent the data being transfered between views and controllers or any business related data model that will represent all the tables of the database. (if we have 10 tables in our database we will have 10 models that correspond to them). All of the tables will be a class file which will be a model and all the properties of the class file will represent the columns of that table.
<b>View</b> is the user interface. Whatever you see on the website it is the view being displayed to you.
<b>Controller</b> acts as an interface between model and view to process all the business logic and incoming requests. It manipulates all the data using the model and interacts it with the view to render the final output. Controller can be treated like the heart of the application.
Example: If we have an interactive componenet (example button) and the user clicks it controller will be the first to recieve the request and the controller will have a lot of action methods. The controller will redirect the request to one of them and will use the model to fetch all the data that it needs to display inside the view to render. Once all of it is rendered the view will pass that to the controller and it will send a response back to the user.<br />
So in the Routing middleware in the pattern settings ('pattern:"{controller=Home}/{aciton=Index}/{id}"') we can see the default controller and action method. (we user enters the website it goes to the Home controller and accesses the Index action method) <br />

<b>5. ROUTING in MVC</b><br/>

Let's start with an example: <br/>
If we have a general pattern of routing like https://localhost:55555/Category/Index/3 (https://localhost:55555/{controller}/{action}/{id?}). <br /> The first comes the domain of the URL. This is an example if it's run on the local computer. It will give you the local adress and the port where the page is accessed, after the port number we call the controller which is named Category then we call its action method named Index and of the the parameters we can pass to the action method. This parameter is optional (this indicated by the symbol ?). When calling the routing middleware we have to specify that it is the default route and the pattern of the default route (example. {controller=Home}/{action=Index}/{id?}). You can also set the default action method of the controller (example. we set that Index is the default action metod we can then only call https://localhost:55555/Category/ without Index in front)<br/>

<b>5.1 Connection between Controller and View</b><br/>
If we have a controller all of its view and UI pages will be placed inside the Views folder inside the folder with the same name as the controller. Also convention of name controllers is that they should end with the keyword Controller (example. HomeContoller.cs). If we enter a controller file we will see the class of the controller we are using and it is implementing the default Controller class (example. HomeController : Controller). Some of the controller can register interfaces with dependency injection (example. Logger). We have action methods inside the controllers. If we search the URL home/privacy/ it will go inside home controller and look for the Privacy() action method it will load the content we have and the return type of these action method is IActionResult and it can return the View() of the controller or it can redirect to another page. The View is located in the Views folder and access the controller view folder and inside it will find a view corresponding with the action method and it will return that as the result of the action method.<br/>

<b>6. Understanding Views</b><br/>
We have controller folders inside the Views and they contain Views for your action methods as previously mentioned. Now we have a Shared folder this is similar to user components in classic C#. It is basically views that you can call within a view in multiple places in your application. We have a special partial View that is called <b>\_Layout.cshtml</b> it is the default master page of your application. So if open that up it at first we will have a header, footer, basic javascript, the head where we write our titles, css, etd, and we will have a div where we will render the body (@RenderBody()) if the action method views don't specify their own defualt master page \_Layout will be used. Next we have <b>\_ValidationScriptsPartial.cshtml</b> which is the partial view where we are adding scripts for validation. If we want to use validations on one of the action method we will include these partial views on those pages. We also have an error partial view <b>(Error.cshtml)</b> it will be used to display error if we encoutner them. Now if we go out of the Shared folder. We are left with two files \_ViewImports and \_ViewStart. <b>ViewImports</b> we put global namespaces here so if we want to access a namespace in all of the pages we add it here so it can accessable. But the important thing we have here is <b>TagHelpers</b>. These are bindings provided that look like HTML tags. But they are special tags adopted from other languages. (example. when clicking on a button Privacy (whose route needs to be /home/privacy) this link is provided by the special anchor tag that has taghelpers asp-area="" asp-controller="Home" asp-action="Privacy"). The last file is ViewStart where we define the master page which when starting a project by default is \_Layout. We can set different default master pages for seperate pages we just define them in the page file.

<b>7. Tag Helpers</b><br/>
Microsoft looked at languages like Angular or React. So they wanted to implement Angular type experiences in the new .NET Core. Tag-Helpers enable server-side rendering, server-side code participates in creating and rendering HTML elements in Razor files. They are natural to use and focused around the HTML element. <br/>
Documentation: https://learn.microsoft.com/en-us/aspnet/core/mvc/views/tag-helpers/intro?view=aspnetcore-7.0 <br />

<b>8. Action Result</b><br/>
IActionResult is the generic type that implements all of the other return types. (for Views in MVC or for Pages in Razor).
ActionResult is a result of action methods/pages or return types of action methods/page handlers.
Action result is a parent class for many of the derived classes that have associated helpers.
The IActionResult return type is appropriate when there is multiple ActionResult return types that are possible in an action method.
Working with Razor we can return Content, File, NotFound, Page, Partial, Redirection. The MVC Application can return View, PartialView, Redirect, RedirectToAction, RedirectToRoute, Content, Json, Javascript, File, (empty). And all can be returned to IActionResult, but if use each Result interfaces seperately it will not be compatible for every case of return.<br/>

<b>9. Database</b><br/>
Our project will need a database for our data. SQL Server will be used to create one and we will use EntityFramework which is a spot editor that will help us with all data related operations and to manipulate the data layer. Every table in the database needs a corresponding model for the code first <b>migration</b> (Database migrations, also known as schema migrations, database schema migrations, or simply migrations, are controlled sets of changes developed to modify the structure of the objects within a relational database. Migrations help transition database schemas from their current state to a new desired state, whether that involves adding tables and columns, removing elements, splitting fields, or changing types and constraints). .NET team made special annotations with which we can add restrictions to the sql script. Over the property we put in [] (example [Key]) we add these annotations with using System.ComponentModel.DataAnnotations. We need to create a Data folder where we will put our context inside the <b>DbContext</b> (A DbContext instance represents a combination of the Unit Of Work and Repository patterns such that it can be used to query from a database and group together changes that will then be written back to the store as a unit.) we need to create our Application DbContext class which will have a base class of DbContext this class belongs to EntityFrameworkCore. A constructor is required to create the DbContext, our application context has to accept DbContextOptions<{class_name_of_our_dbcontext}> and those options need to be passed to the base class of DbContext. After that we need to make a DbSet method for our model (DbSet<{name_of_model}>). We use code first creation so we don't have to do much database managment behind the scenes. We connect our connection string and DbContext in the Program.cs where we define a service that our application will use for creation and managing the database. We will need to install EntityFrameworkCore.SqlServer package then we add the service: <br/>

builder.Services.AddDbContext<ApplicationDBContext>(options => options.UseSqlServer(
builder.Configuration.GetConnectionString("DefaultConnection")
));<br/>

We get our connection string with builder.Configuration.
After we need our migrations we will need an EntityFrameworkCore.Tools package to run migrations in the package manager console which can be accessed (Tools -> NuGetPackageManager -> Console). After we make a migration with command add-migration {name_of_migration}, a folder will be created called Migrations. Inside the file that is contained in the Migrations folder we have two methods called Up and Down. Up method defines what needs to happen inside the migration and the Down is to rollback the changes. EntityFramework will automatically make a sql query and execute everything we need<br/>

<b>10. Temp data</b><br/>
If we want to display some alerts when someone executes an operation on some data in our database we have temp data. Temp data has one purpose only to store something for one request which is perfect for displaying alerts. (sintax: TempData["{key}"]). To display data on every page without calling it on everypage we can create a partial view (usually in Shared folder).<br/>
