﻿# Implementing Validations
1. System.ComponentMOdel.DataAnnotations.dll assemly
	- Provides a 'ValidationAttribute' abstract base class
		- ErrorMeesage Property for Showing Error MEssage
		- IsValid(Object value) method
			- BAsed on 'Vaidation-Rule' in IsValid() method the restlt ill be true or false
	- RequiredAttribute
	- StringLengthAttriute
	- ComparerAttribute
	- RegularExpressionAttribute
2. APply Valduation Rules on the Prperties of Entity Class
3. To Validate the Posted Model in HttpPost methods, use 'ModelState' Property of the 'Controller' class
	-  ModelStateDictionary ModelState
		- ModelStateDictionary: Contains Key:value pair for validation error Messages
			- Key is the Proerty NAme of Model class and Value is Error Message that occures pn the proeperty
		- Error messages will be shown on the View using FOllowing Tag Helpers
			- asp-validation-for
				- For each field
			-  asp-validation-summary
				- For all properties of the Molde class passed to the view
- FActs of MVC
	- One View Can accept only One Model 
	- If Multiple Models are to be shown on Single View,
		- Draw a View Diagram
		- FInd out number of Models required i.e. Check if Collection is reqiored or a Single Objet or a SIngle Primitive property
	- The View can by-default show data to user or accept data from user for all those properties present into the Model class passed to view
	- To show data to View which is not in Model (only show not write), MVC have provided following objects
		- ViewData["KEY"] = VALUE;
			- Of the type ViewDataDictionary
		- ViewBag.<KEY> = VALUE;
			- A Dynamic Object
		- ViewBag and ViewData has life only for the SPecific or Declaring method

# Mvc Programming Fundamentals
- MOdifying View by Providing Additional Data than Models
	- ViewBag, ViewData
- Modify View by passing Collection of data for REad/Write OPerations
	- SelectList (IEnumerable of SelectListItem, "DataValueField", "DataTextField", Object Value)
		- Collection of SelectListItem
		- DataValueField
			- The Value that is posted when user select data from the COllection UI
		- DataTextField
			- USer Friendly data that will be selected by End-USer and then corresponding DataValueField will be posted 
		- Value
			- The default value to be displayed
- If a View is passed with ViewBag/ViewData, then all action methods returning thew same View MUST PASS the ViewBag/ViewData to the view
- While Handling Expceptions during an execution of action method tae create of follwing
	- Throw Exception from anaction method
	- Catch it wiht following ways
		- USe Try..Catch in each action method
		- in catch BLock redirect to Error View
	- Avoid Hard-Coding of the Data e.g. Controller NAme, Action Name , if this is to be shown on Error View
		- USe Route Object instead  to read Current ControllerNAme and ActionName
			-  public RouteData RouteData from the 'ControllerBase'
			- public RouteValueDictionary Values, proeprty of 'RouteData' class
				- USed to REad Route Expression that is defined in 
					- USeEndpoints() middleware and its MapCOntrollerRoute() method
						- {controller}, {action}, {id}
- Use the Action Filters for Implemting COmmon logic across various action methods
	- ActionFilter is a GLobal class that implements IActionFilter interface
		- Execute() (Deprecated) and ExecuteAsync(ActionContext ) methods
			- ActionContext: Represents a Current Action that is Executing
		- ActionExecutingContext
			- The Action is Found and is taken for Execution
			- OnActionExecuting(ActionExecutingContext)
		- ActionExecutedContext
			- Action is Executed (Success / Execption)
			- OnActionExecuted(ActionExecutedContext)
		- ResultExecutingContext
			- Result is Generated with IActionResult
			- OnResultExecuting(ResultExecutingContext)
		- ResultExecutedContext
			- Result Generation is completed
			- OnREsultExecuted(ResultExecutedContext)
	- Global 
		- 0 OnActionExecuting
	- Controller
		- 1 OnActionExecuting
	- Action
		- 2 OnActionExecuting
	- Action is Executed
		- 3 OnActionExecuted
	- Controller
		- 4 OnActionExecuted
	- Global
		- 5 OnActionExecuted
- Handling Exceptions using Exception Filter
	- IExceptionFilter interface
	- ExceptionFilterAttribute class
		- OnException(ExceptionContext)
			- ExceptionContext, represents an exception thrown in Current Request in HTTP Pipeline for MVC Apps
				- ExceptionHandled
					- The process for catching Exception is Successful
				- Exception
					- The Exception thrown during the execution
				- Result
					- IActionResult
						- ViewResult in General
							- To PAss data to View for Exception use VeiwDataDictioary
						- VeiwDataDictioary
							- The class that Accepts following parameters in ctor
								- IModelMetadataProvider
									- MUST be injeted in ctor of Exception Filter
									- THis is REsolved by by HTTP PIpeline whie Processing MVC Request
								- ModelStateDictionary
									- ExceptionContext.ModelState
- Using Session State
	- ISession
		- THe Interface for Session Information  Contract 
			- Set(Key,byte[] value)
				- Set the Session Data
				- Key, the identification of Data in Session State on Server 
				- Value, the Data as Binary Array
			- TryGetValue(Key, out object value)
				- BAse on 'Key' the 'value' will be extracted from the session	
	- The session data is stored using
		- HttpContext.Session.Set
			- HttpContext, is the Current Active HTTP Request
			- HttpContext.Session
				- SessionExtension class for Customizing Session Data
					- int, string
	- The Session only stores the primitive types, to store the CLR objects, use the Custom Session Extension 	
- Async Data VAidations
	- The 'Proeprty-Based' data vaidation where a 'Single Property Value' will be posted back to the server asynchronously by an impliccit Async call
	- The Server has an advanced information of the property that il be validated asynchronouslys
	- Microsoft.AspNetCore.Mvc.RemoteAttribute class
		- The Action Method that will be invoked asychronously
		- The Controller NAme which contains Action Method
		- THis will be an async HTTP Call that will POST a Single Property Value
		- The Action Method has to return JSON data with Error Message
	- GetUrl(ClientModelValidationContext) method of Remote Attribute class
		- Execute the jquery.unobstrusive.validate.js on Client
			- THis will make an async call
				- /cotroller/action

- File Uploads
	-  IFormFile
		- Represents a File Object accepted by the Action Method over the Http POST Request
			- Form-Data
			- Content-Type
			- COntent-Disposition

# ASP.NET Core 5/6 Identity
- IdentityDbContext<IdentityUser>
	- The class that manages connection to store using EF COre Code-First Approach
	- IdentityUser
		- Class that reptesents USer Information and this information will be stored into the AspNet_users Table in Database
	- IdetityRole
		- Class for Role Information that will be stored into the Database 
- Microsoft.AspNetCore.Identity.EntityFrameworkCore
	- PAcakge for Identity + EF COre 
- Microsoft.AspNetCore.Identity.UI
	- A PAcake that represents UI library that provides UI for Identity Managamet
		- e.g. Login, Register, ForgetPAssword, etc.
- RoleManager<IdentityRole>
	- USed for CReating and Managing Roles for the Application
	- Methods of this class are async
		- CreateAsync(IdentityRole)
	- Resolve the RoleManager from Dependency
		- services.AddIdentity<IdentityUser, IdentityRole>();
- USe Apropriate Policies for Roles and their Access 

- TagHelper
	- Attribute(s) passed to HTML Elements to Set its behavior and Provide data to it
		-asp-for, asp-action,asp-controller, asp-items, asp-validation-for, etc.
	- CReating Custom Tag Helpers
		- Derive class from TagHelper
		- Override ProcessAsync method
			- Write a Logic for HTML Generation
				- Create Custom Tag
				- Encapsulate the Standard HTML to geneate UI for the Custom Tag
		- DEfine a Property that will accept data for the Custom Tag Helper
		- DEclare the namespace in Views/ViewImports.cshtml 
			- addTagHelper *, {NAMESPACE}
- View BAsed AUthorization
	- IAuthorizationHandler
		- COntract USed by AddAuthorization() Service and UseAuthorization() Middleware for Verifying the CLiam of the Current Login User
	- IAuthorizationRequirement
		- A COntract That is impemented by a Class thet set rule for Cutom Authorization Policy Check (aka Requirements)