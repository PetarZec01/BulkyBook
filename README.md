# BulkyBook

This is tutorial project for ASP.NET C# Core MVC <br />
For database creation EntityFrameworkCore and EntityFrameworkCore.SqlServer was used<br />

Tutorial link: https://www.youtube.com/watch?v=hZ1DASYd9rk&t=0s<br />

I built using this tutorial a category crud project.<br />
Tools used: .NET 6, Visual Studio 2022, SSMS 2018, SQL Server<br />

<b>.NET Core:</b> <br />
This is the biggest change Microsoft .NET made. First introduced Webforms then .NET MVC but it was only runnable on Windows platforms.<br />
To keep up with changing technology Microsoft introduced ASP.NET core which is multiplatform and they had cloud in mind.<br />

<b>Advantages of .NET Core: </b>

- fast and open source
- cross platform
- built in supprt for dependency injection
- easy updates
- cloud friendly
- phenomenal performance

<b>Dependency injection</b> (integral part of ASP.NET Core):<br />
.NET Core inject objects of dependency classes through constructors by using the built in IoC container.<br />
Without this it there would be duplicate code if we want to implement database connections or an email service (a lot of opening, closing etc.)<br />
With dependency injection we get a container which will contain interfaces for what we want to implement on a certain page.<br />
If multiple pages want to access database for example it will ask the container to create an object of this functionality which can be used for easier implementation.<br />
Dependency injection will automaticly manage, dispose, implement and instance of objects so we don't have to do manually.<br />
It will be easier to modfy the instace class, we modify it only in the class and it will automaticly use the new implementation on all pages<br />
<br />
<b>FILES AND FOLDERS:</b><br />

- <b>Project file</b> contains the .NET version in the PropertyGroup and what packages we are using in the project are stored in the ItemGroup
- <b>Properties\launchSettings.json</b> which contains different profiles in which we can run our application (what port is used, where is it hosted)
- <b>wwwroot</b> contains static files (css, images, js code, libraries), does not contain C# files
- <b>appsettings.json</b> we put all secrets in here of our project (connection strings, API keys, SendGrid keys, stripe payment keys) and if we need other appsettings for different enviroments (staging, production)
- <b>Program.cs</b> file running for the application. We have the builder. If we want to register with our dependency injection we do it here. Also we configure the HTTP pipeline. The pipeline specifies how application should respond to a web request. (when browser requests something it goes back and forth in the pipeline). We put items that we want into the pipeline and is made with multiple middlewares (MVC is a middleware that we are using) (you can use other middleware for authentication, authorisation). What is in the pipeline it gets modified by the middleware and gets passed till the last and the response is returned to the server. Our application is using to se if it is in Development mode so it can send you Developer friendly exceptions so you can solve problem easier. Next middleware is to use redirection with http. Then middleware of our static files. We have a rouitng middleware for routing through pages. Also there is a Authorisation middleware and without the Authentication middleware which you can add. Thre is MapControllerRoute this will route requests to corresponding controllers and action. Lastly the Run middleware which runs the app. The order is really important because it is exactily how the request will be passed.
- <b>Models </b>
- <b>Views </b>
- <b> Controllers</b>

<b>MVC Arhitecture (Model / View / Controller):</b>
<b>Model</b> represents the shape of the data. C# is used to describe a model. The model corresponds with all the data related logic that user works with. It can be a table that we are storing in the SQL Server or it can be a model that contains multiple tables. This model can represent the data being transfered between views and controllers or any business related data model that will represent all the tables of the database. (if we have 10 tables in our database we will have 10 models that correspond to them). All of the tables will be a class file which will be a model and all the properties of the class file will represent the columns of that table.
<b>View</b> is the user interface. Whatever you see on the website it is the view being displayed to you.
<b>Controller</b> acts as an interface between model and view to process all the business logic and incoming requests. It manipulates all the data using the model and interacts it with the view to render the final output. Controller can be treated like the heart of the application.
Example: If we have an interactive componenet (example button) and the user clicks it controller will be the first to recieve the request and the controller will have a lot of action methods. The controller will redirect the request to one of them and will use the model to fetch all the data that it needs to display inside the view to render. Once all of it is rendered the view will pass that to the controller and it will send a response back to the user.<br />
So in the Routing middleware in the pattern settings ('pattern:"{controller=Home}/{aciton=Index}/{id}"') we can see the default controller and action method. (we user enters the website it goes to the Home controller and accesses the Index action method) <br />

<b>ROUTING in MVC</b>
