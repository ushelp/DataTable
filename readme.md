# DataTable AJAX pagination plugin manual

 
Notice:It's same toEasyDataTable,This plugin is EasyDataTable the English version, the Chinese version please refer to:https://github.com/ushelp/EasyDataTable
说明:该插件与EasyDataTable插件为同一插件，是EasyDataTable的英文文版本，提示消息和使用手册均为英文版。 


Use Ajax pagination can improve data load and display speed and reduce network traffic, improve the customer experience degrees; same time be able to refresh only partially solve when there are multiple data displayed on the page table area, the traditional paging will cause the entire page to refresh. 

EasyDataTable AJAX pagination plug-in is based on the best pure jQuery Ajax pagination plug-in, enhanced support for HTML and JS programming are two ways to implement paging, the biggest feature is: Simple, easy to use and flexible. Supports a variety of data sources to load (dynamic data source server, static data source, JSON file data sources); own tabs; support custom pagination; Custom Loading prompt; spreadsheet UI can be customized; support column click sort, support static data sorting; supports dynamic query conditions, static data filtering query conditions, support scope, methods and mode support, built-in 8 kinds of static data that match the query pattern; built-EasyDataTable expression language, event support, can enhance paging through JavaScript programming.


**Compatibility**: EasyDataTable fully compatible with IE6 and above, Firefox, Chrome, Safari, Opera and other kernel (Trident, Gecko, Webkit, Presto) browser, and is compatible with multiple platforms and systems (PC, TabletPC, Mobile) .


And other relatively EasyDataTable Ext main features:
1. Lightweight:. EasyDataTable require fewer resources to load and very **lightweight**
2. easy to use:. **HTML enhanced support**, almost without JS code, comprehensive package, easy to use, JS zero programming can achieve powerful Ajax paging function
3. flexible: unrestricted, comes the expression language, event support, from UI to function **can achieve the flexibility to customize and extend**
4. comprehensive: dynamic and static load **full variety of data sources**, and supports a variety of different scope, approach, mode of static data filtering query


> EasyDataTable currently supports two versions 1.X and 2.X version
> 2.X adds support for static data source, JSON data source paging file is loaded, and supports the above query static data filtering and sorting.


----------


## 1, Introducing core js and css files EasyDataTable
	
	<link rel="stylesheet" href="css/datatable.css" type="text/css" />
	<script type="text/javascript" src="js/jquery-1.10.2.min.js"></script>
	<script type="text/javascript" src="js/easy.datatable.js"></script>




## 2, Page table structure

EasyDataTable is structured into form, EasyDataTable data table (table header row, the data show rows), paging div three parts, creating a data table structure as follows:

	<!-- Create a form action is handled dynamically paging request address -->
	<form action="Paging server address">
	
		<!--  EasyDataTable data show the form, you must have id -->
		<table id="DataTableID">

			<!--  Header row  -->
			<tr><th></th>    …… </tr>

			<!--  Data show the row  -->
			<tr style="display: none;"><td></td>    …… </tr>

		</table>
		
		<!--  Paging Line  -->
		<div class="panelBar"  size="5,10,30,50">	</div>
	
	</form>

### Paging form instance:

	<!-- Create a form action is paged address requests processed -->
	<form action="doPage.jsp" name="myform">
	    <!-- Use DIV to wrap EasyDataTable data display table, when the data exceeds the DIV height, the scroll bar can be a reality -->
	   	<div style="height: 260px;overflow:auto;width: 780px;">
	  			<!--Table shows the data, you must specify the id, EasyDatatable paging initialization data by id-->
			    <table class="datatable"  id="datatable"  width="100%" align="center">
	            	<!-- Header row-->
			      	<tr>
			      		<th width="40">
							<!-- Call DataTable.checkAll (this, 'checkbox name') function or use HTML enhancements add check = "checkbox name" attribute can be achieved checkbox Select All / Clear All function -->
							<input type="checkbox" onclick="DataTable.checkAll(this,'mychk')" />
				   		</th>
				   		<th width="80">count</th>
				   		<th width="100">userId</th>
				   		<th width="100">userName</th>
				   		<th width="100">userInfo</th>
				   		<th >operation</th>
			   		</tr> 
					<!-- Data show the row, Use {property} property expression to get data -->
				   	<tr  style="display: none;">
				   		<td style="text-align:center;height: 45px;">
	            			<!--Custom box, value for the object id-->
				   			<input type="checkbox" name="mychk" value="{id }"/>
				   		</td>
	            		<!--Use the built datatableCount property to display the number of data-->
				   		<td align="center"  style="text-align:center;height: 45px;">
							{datatableCount }
						</td>
				   		<td style="text-align:center;color:#00f">No.{id}</td>
				   		<td align="center">{name}</td>
				   		<td>{info}</td>
				   		<td align="center" style="width: 120px">
				   			<a href="doUser.jsp?o=show&id={id }">show</a>
					   		<a href="doUser.jsp?o=edit&id={id }">edit</a>
					   		<a href="doUser.jsp?o=delete&id={id }">delete</a>
					   	</td>
				   	</tr>
			   </table>
	    </div>

		<!--Tabs section, use the size attribute to set the number of drop-down menus per page optional value, use, separated-->
	    <div class="panelBar" style="width: 780px;" size="5,10,30,50">
		</div>
	</form>






## 3, Ajax paging tables for data initialization

EasyDataTable support HTMl two ways to enhance and JS paging table initialization.

### 3.1, Method One: html enhanced use easydatatable class styles 

Adding to the table style class `easydatatable` initialize (form must have id)

	<table class="datatable easydatatable" id="datatable3" width="780px" align="center">

### 3.2, Method Two: JavaScript, use DataTable.load () function

Use `DataTable.load (table id, parameter information)` function on the table you want to initialize Ajax pagination  

**`DataTable.load( tableid [，easydataParameters] );`**



Parameter Description:  
`tableid`: Must parameters, data display table id
`easydataParameters`: Optional parameter that specifies ** EasyDataTable initialization parameters ** paging information to support arguments:
	{
		`pagetheme`: 'Paging theme'
		`loading`: 'When paging loading data, Loading prompt manner'
		`language`: 'tabs language'
		`start`: 'data starts to load event handler'
		`end`: 'end of the data load event handler'
		`row`: 'initial page load shows the number'
	}


#### easydataParameters Initialization Parameters page 

- #### pagetheme——Optional tabs theme
Supports two optional paging themes, and custom paging (values ​​are not case-sensitive)

	`"FULL"` (full theme, the default display all the pages option, the default value)
	 
	`"SIMPLE"` (a simple theme, do not show quick jump page)
	
	`"NO"` (Cancel theme, using custom paging, refer to Section 11 "custom paging")

	**This parameter supports HTML enhancements:**

	`pagetheme` parameter can be paged through `div` a `pagetheme property` setting. Loading sequence as html, javascript, loading parameters will overwrite the previous value, refer to Section 10 "Paging theme settings."



- #### loading——Optional,Paging mode when loading data Loading
	
	Because Ajax program during execution does not refresh the page, so when performing operations such as paging, Ajax applications should be given to the customer when the appropriate prompts and feedback loaded for execution, or the user may think the task is not performed or failed, affecting the degree of experience .

     EasyDataTable support when the page loads carried Loading configuration, optional value: `" default "`, `" show "`, `" none "`, `" hide "` or `" any value "`. (Values ​​are not case-sensitive)

	`" default "`: default is disabled when the page loads the operating table (disabled hyperlinks, buttons), the data displayed in gray
	
	`" show "`: display text prompts to load, display the contents of the page is loaded loading tips DataTable.loading_msg defined (can be modified) "Data is loading ......"
	
	`" none "`: hidden data show lines show when the page is loaded hidden data line data + cells showed complete blank
	
	`" hide "`: hidden data content, the page is loaded only when the data show hidden rows, reserved display cell borders
	
	`" Any value "`: direct the contents as a prompt to load the page content (equivalent to show the way), support for HTML content
	
	Example:

		loading:"<div><img src=\"images/loading.gif\"/><br/>Data is loading ......</div>"  

	**This parameter supports HTML enhancements:**
   
	`loading` data `parameter can also be loading attribute table` setting. Loading sequence as html, javascript, loading parameters will overwrite the previous value.


- #### language——Language options, set tabs displayed
 
	Default paging configuration is defined in `DataTable.default_lang` attribute, the default value:

		DataTable.default_lang={
		    first : "first",
			previous : "previous",
			next : "next",
			last : "last",
			totalCount : "total {0} rows",
			totalPage : "total {0} pages",
			rowPerPage : "page for {0} rows"
		}

	Can directly modify and redefine the value of this attribute as a global default paging text and language.
May also need to redefine the language to specify the paging data table in the initialization parameter, `{0}` placeholder to display the corresponding data must exist.

		  <script type="text/javascript">
		  $(function(){
					//Re-custom paging language
		  			var pageLanguage={
						"first":'first',
						"previous":'previous',
						"next":'next',
						"last":'last',
						"totalPage":'total {0} pages',
						"totalCount":'total {0} rows',
						"rowPerPage":'page for {0} rows',
				    };
				    
					// Initialization data table datatable
		  			DataTable.load("datatable",{
		  				"pagetheme":"SIMPLE",
		  				"loading":true,
		  				"language":pageLanguage  //Language id specified data table for the datatale
		  			});

					//Initialization data table datatable2
		  			DataTable.load("datatable2");
		  });
		  </script>



- #### start——Optional, the data set is loaded each time the handler at the start of
 
		/*
		o:Now DataTable
		initFlag:true representative of the first load data (initialization tables), false representatives of the page to load
		*/
		"start":function(o,initFlag){ 
		  			if(initFlag){ //The first load (uninitialized)
		  				console.info('init start...');
		  			}else{ 
			  			console.info('load start...');
		  			}
		  		}


- #### end——Optional, the handler is set each time data is loaded at the end of

		/*
		o:Now DataTable
		initFlag:true representative of the first load data (initialization tables), false representatives of the page to load
		*/
		"end":function(o,initFlag){ 
		  		if(initFlag){ //The first load (uninitialized)
		  			console.info('init end...');
		  		}else{ 
			  		console.info('load end...');
		  		}		
		  	}

- #### row——Optional, set the number of initial page loads
 
	In the initialization parameter specifies the number of pages of data loaded by default. If you do not set this parameter, the value of the property `DataTable.default_row` default value of 5 (can be modified).
     
	**This parameter supports HTML enhanced:**   
	row parameters can also be paged through `div` of `row` attribute set, load order of html, javascript, loading parameters will overwrite the previous value.






## 4, EasyDataTableExpression Language Use

### 4.1 EasyDataTable Property Expression Property expression `{data attributes }`
** Property expressions for data display line, display the specified property values. **
In the property expression can be directly referenced data attributes to get the data for the specified property. And supports a variety of mathematical, comparison operators perform basic arithmetic JavaScript.

	{id}    {name}

### 4.2 EasyDataTable Statement Expression `% {expression statement}% 
** Statement expression data for display in line with the programming statements to control programming. **
Support the use of JavaScript written statement expression expression code; supports direct call data attributes; also supports the use of EasyDataTable property expression (must be used to define the use of quotation marks in the string).
The results statement expression must be used to perform the standard output method statement expression EasyDataTable output: ** `DataTable.out (" Content ");` **

	<%--
	Support JavaScript language expressions.
	Attribute name: the variable reference and handling
	EasyDataTable property expression: You must define the use of quotation marks in strings
	--%>
	%{
		var res=name+"   {name}";
		DataTable.out(res);   
	}% 
					   		
	%{ 
		if(id%2==0){ 
			var op='<a href="doUser.jsp?o=show&id='+id+'">show</a>&nbsp;&nbsp;'; 
			op+='<a href="doUser.jsp?o=edit&id={id }">edit</a>';
			
			DataTable.out(op);  
		}else{ 
			 DataTable.out('<a href="doUser.jsp?o=show&id={id }">show</a>&nbsp;&nbsp;<a href="doUser.jsp?o=edit&id={id }">edit</a>&nbsp;&nbsp;<a href="doUser.jsp?o=delete&id={id }">delete</a>');  
		} 
	}%






## 5, EasyDataTableBuilt-in data attribute
**EasyDataTable built-related data and paging built-in properties can be used directly in the data show tabs row or rows.**

**Only valid data show the line:**    
`datatableIndex`: available data in the current index page
`datatableCount`: the number of available data on the current page
`key`: When Map data set can be used to obtain the data for the key

**Data show lines, tabs can be used:**   
`pageNo`: now page NO.
`maxPage`: max Pages
`rowPerPage`: the number per page
`totalCount`: the number of total data
`sort`: sort field
`order`: Sort, `desc` or `asc`  

** For example: **
All the current data index data: `{datatableIndex + (pageNo-1) * rowPerPage}`
The number of all current data in the data: `{datatableCount + (pageNo-1) * rowPerPage}`







## 6, Sort Support
In order to add the header row needs to `sort =" sort field name "` attribute to the field corresponding to the cell.

	<tr>
		<th width="80">count</th>
		<th width="80">index</th>
		<th width="80">{index+1}</th>
		<th width="100" sort="id">id</th>
		<th width="100" sort="name">name</th>
		<th width="100" sort="info">info</th>
		<th >operate</th>
	</tr>
 
Sortable column displays the sort arrow. Click to send server sorting information required: sort field sort (values ​​sort of) and sort order (asc or desc) to the server.
After the server receives and returns the new data processing can be realized Ascending Descending switching.
 
Access to information via the server-side sorting and sort order parameter name.   

	String sort = request.getParameter("sort");
	String order = request.getParameter("order");

**Refresh Form :** 
Use `DataTable.reload ('tableid');` can refresh the data table is specified, no sorting restored to the state.

	<div onclick="DataTable.reload('datatable3')">Refresh</div>


### Custom sort indicator

EasyDataTable For sortable columns to be **sorted in appearance reminded by sorting indicator**. The default up and down arrows ↑ ↓ (`&uarr;&darr;`) indicates that the column can be sorted click on sortable column names; Show ↑ (`&uarr;`) when ascending, descending ↓ display when (`&darr;`) to indicate the current rise in descending order.

EasyDataTable support for sortable column sort indicator for **global** and customized **by DataTableID** of two ways:

**1. Global Custom: default edit all DataTable objects sort indicator**

- `DataTable.order_default` The default indicator sortable columns
- `DataTable.order_up` Indicator can be sorted in ascending column
- `DataTable.order_down` Can sort the column in descending indicator


	//Redefine EasyDataTable sort indicator
	DataTable.order_default="<img src='images/order_default.gif'/>";
	DataTable.order_up="<img src='images/order_up.gif'/>";
	DataTable.order_down="<img src='images/order_down.gif'/>";
	
**2. By DataTableID Custom: only edit the current sort DataTableID DataTable object corresponding indicator**

`DataTable.sort["DataTableID List"]`You can specify more than one at the same time set the sort indicator DataTable table object, use commas multiple DataTable table id `,` separated

	DataTable.sort["datatable,datatable2,datatable4"]={
		order_default:"<img src='images/order_default.gif'/>",
		order_up:"<img src='images/order_up.gif'/>",
		order_down:"<img src='images/order_down.gif'/>"
	};


## 7, Server-side data requirements    

**EasyDataTable can automatically parse JSON results returned by the server, and implement paging.**  

- **JSON output paging server must name the following results:**    
	`data`: data collection, support for List and Map Collections (built-in properties available Map key key, Map value without the use of value prefix) for the collection of JSON
	`pageNo`: the current first few pages, digital
	`rowPerPage`: Showing the number of digital
	`totalCount`: the number of total data, digital
	Optional parameters:
	`[sort]`: sort field
	`[order]`: Sort, desc or asc

- **If the server is encapsulated in PageBean paging parameters, such as:** 

		public class PageBean {
			private List data;
			private int pageNo;
			private int rowPerPage;
			private int totalCount;
		   //setters&getters ……
		}

	For example, JSON object back to the server-side output in PageBean name is `pb`, `value` attribute is added to the data table `tag` table, specify the paging JSON object name `pb`:

		<table class="datatable" id="datatable3"  width="780px" align="center" value="pb">






## 8, Refresh specified data table

	DataTable.reload(“tableId”);  //Cancel sorting effect, Refresh table, reload the data






## 9, Set the number of default paging

Use `row paging parameters` in the **initialization parameter** can specify the number of the default paging. If you do not set this parameter, the value of the property `DataTable.default_row` default value of 5 (can edit).

`row attribute` support in the page using HTML div tag enhancements:
    
	<div class="panelBar" style="width: 780px;" size="5,10,30,50" row="10">
				
	</div>





## 10, Paging theme settings

DataTable built-in paging achieved and provides two sets of topics:

`FULL`(Full theme, the default display all the pages option, the default value)

`SIMPLE`(Simple theme, do not show quick jump page)
 
### 10.1, Specify the page using JavaScript theme

	DataTable.load("datatable",{
	  	"pagetheme":'SIMPLE'
	 });


### 10.2, Paging Paging specified topic in the DIV tag

	<div class="panelBar" style="width: 780px;height: 40px;" size="5,10,30,50" pagetheme="FULL">
	
	<div class="panelBar" style="width: 780px;height: 40px;" size="5,10,30,50" pagetheme="SIMPLE">

### 10.3, Cancel paging and themes

Use display: none to hide, or you can directly delete tabs section.

	<div class="panelBar" style="width: 780px;height: 40px;display: none;" size="5,10,30,50" >







## 11, Custom Paging
EasyDataTable support custom paging, the paging div tag can write custom paging code. Custom paging code supports: EasyDataTable built-in properties, attributes, expressions and statements expressions.

` Pagination div tag ` Add `pagetheme= "no" ` 
Properties (or parameters set by EasyDataTable initialization), call **`DataTable.go ('load data tables id', 'pages', [page displays the number of])`** function, you can implement custom paging jump.
You can also use `row` attribute specifies the default page displays the number of:

	<div class="panelBar" style="width: 780px;height: 40px; line-height: 40px;" size="5,10,30,50"
				pagetheme="no" row="8">

Example:

	<div class="panelBar" style="width: 780px;height: 40px;" size="5,10,30,50" pagetheme="no"  row="8">
		now {pageNo}/ {maxPage} page Now {rowPerPage} row/{totalCount} row	
		<input type="button" value="first" onclick="DataTable.go('datatable7',1)"/>
		<input type="button" value="previous" onclick="DataTable.go('datatable7', '{pageNo-1}')"/>
		<input type="button" value="next" onclick="DataTable.go('datatable7', '{pageNo+1}')"/>
		<input type="button" value="last" onclick="DataTable.go('datatable7', '{maxPage}')"/>
	</div>

When the JavaScript function call, if the parameter used EasyDataTable property expressions (such as pages `{pageNo-1}`) is required by single quotation marks, as a string parameter, otherwise there will be a warning under IE8 (does not affect the actual the results).
For example, the following code '{pageNo-1}' must use quotes:

	onclick="DataTable.go('datatable7', '{pageNo-1}')"/>。






## 12, With a search page

Form into the search box form, with the following two ways to search submission page.

**Search Search button to submit two methods of data**

Method One: Use HTML to enhance the search button into the current form, added directly to the search button style class `data_search`

	<input type="button" class="data_search" value="search"/>

Method Two: Use `DataTable.load ('data tables id')`, registered for the event by clicking on the search button

	<input type="button" class="btn_test" onclick="DataTable.load('DataTableID')" value="search"/>

Server to obtain the search criteria submitted by the client.







## 13, EasyDataTable load support multiple data sources

**EasyDataTable supports three kinds of data sources:**    
1. server dynamic data sources (1.X, 2.X support)
2. static data sources (2.X support)
3. file data source (2.X support)


_Note: EasyDataTable currently supports two versions 1.X and 2.X. EasyDataTable 1.X only supports dynamic loading of data sources; EasyDataTable 2.X also supports static data sources and data source file is loaded。_


### 13.1,  Server dynamic data sources (1.X, 2.X)
**Specify the server-side paging address in the form of action** to dynamically obtain JSON paging data to achieve paging.
	
	DataTable.load( 'tableid' [，easydataParameters] );


### 13.2,  Static data sources (2.X)
**JSON supports direct loading** of the specified data objects, implement paging. Static data sources (2.X) 
 
    DataTable.staticLoad('tableid' , jsonDataObject [,easydataParams]);


### 13.3,  File data sources (2.X)
Supports direct loading specified **JSON data files**, implement paging.
 
    DataTable.fileLoad('tableid' , 'jsonFile' [,easydataParams]);







## 14, Static sorting support (2.X)

EasyDataTable support for static sort data in the header row to add `staticSort need to sort on a field corresponding cells =" Sort Field Name "` attribute.

	<th width="100" staticSort="id">id</th>
	<th width="100" staticSort="name">name</th>





## 15,Static filtering query (2.X)

**EasyDataTable support for static data filtering query, and has the following three characteristics:**

1. **Query Range**: Support (static data of the current page) for All (all static data), and static data in two ranges NowPage filter query.

2. **Query Type**: Support multi-criteria when screening by `AND` or `OR` method of data filtering.

3. **Query mode:** Built-in 8 kinds MatchMode (query matching mode) support, each individual can be configured independently screened table MatchMode. 
	
	 **EasyDataTable support 8 kinds MatchMode name and role models that match the query:**
		
		extra:Exact match     
		extra_i:Exact match and ignore case 

		like:Fuzzy Matching
		like_i:Fuzzy Matching and ignore case 

		sql:SQL wildcard matching (percent sign '%' wildcard support, matches any character; support underscore `_` wildcard matches any single character; support `[0-9]`, `[AZ]`, `[az]`, `[0-9A-Za-z] `wildcard such as range, matching a single character range)   
		sql_i:SQL wildcard matching and ignore case 

		reg:Regular expression matching
		reg_i:Regular expression matching and ignore case 


###Specify MatchMode (query matching mode) to form elements:###
EasyDataTable by eight kinds of data that match the query pattern MatchMode, provides the user with a full range of query support; support for each form separate option to specify a different query matching mode, which greatly enhances the flexibility of screening.

**Usage:**
Using HTML enhancements, add `mode` attribute in the corresponding text box, and the value is specified as EasyDataTable `support MathMode name`.

	username: 
	<input type="text" name="name" class="txt_test" mode="sql_i"/> 

	userinfo:
	<input type="text" name="info" class="txt_test" mode="extra"/>
	
The default matching mode Description:If there is no mode `attribute to the` form elements, use ** `like_i` (ignoring fuzzy matching sensitive) ** processing, the default matching mode can `DataTable.default_matchMode = 'like_i'` modified.




###Static data filtering query two ranges:###

1. **NowPage**——Current page data range of data filtering query: Query the current page qualified data (while support for dynamic data sources and static data sources NowPage screening)

2. **All**——All static data range of data filtering query: Query load static data in compliance with all the conditions of the data (only supports static data sources All screening)

###Both methods achieve data filtering:###


**Method One: Using HTML enhancements**

NowPage current page static data range of data filtering query: to the search button to add `static_data_search[_or]` class styles:

	`data_static_search` Support multi-mode conditions AND query.
	
		<input type="button" class="data_static_search" value="static search and" />
	
	`data_static_search_or` Support multi-mode conditions OR query.
	
		<input type="button" class="data_static_search_or" value="static search or"/>

ALL all static data range of data filtering query: to the search button to add `static_data_searchAll[_or]` class styles:

	`data_static_searchAll` Support multi-mode conditions AND query.
	
		<input type="button" class="data_static_search" value="static searchAll and" />
	
	`data_static_searchAll_or` Support multi-mode conditions OR query.
	
		<input type="button" class="data_static_searchAll_or" value="static searchAll or"/>


**Method Two: Use the built-in functions to achieve static filters:**   


NowPage current page static data range of data filtering query: `DataTable.staticSearch('tableid' [,true] )`

	If the second argument is true, press OR way queries; otherwise press AND the way queries.

	<input type="button" value="static searchAll" onclick="DataTable.staticSearch('datatableStatic')"/>


ALL all static data range of data filtering query:`DataTable.staticSearchAll('tableid' [,true] )`


	If the second argument is true, press OR way queries; otherwise press AND the way queries.

	<input type="button" value="static searchAll" onclick="DataTable.staticSearchAll('datatableStatic')"/>






## 16, Form AJAX pagination instances

### 16.1, FULL Paging default theme +checkbox + index, count the use of built-in properties 

	<form action="easydatatable/en/doPage2.jsp" name="myform">
		<div style="height: 260px;overflow:auto;width: 780px;" class="dataTableScrollDiv">
			<table class="datatable easydatatable" id="datatable" width="100%" align="center">
				<tr>
					<!-- checkbox -->
					<th width="40"><input type="checkbox" check="mychk" /> <!-- CheckAll --></th>
					<!-- datatableIndex,datatableCount -->
					<th width="80">count</th>
					<th width="80">index</th>
					<th width="80">{index+1}</th>
					<th width="100">id</th>
					<th width="100" sort="name">name</th>
					<th width="100" sort="info">info</th>
					<th>operation</th>
				</tr>
				<!-- Data Show Row-->

				<tr style="display: none;">
					<td style="text-align:center;height: 45px;"><input type="checkbox" name="mychk" value="{id }" />
					</td>
					<td align="center">{datatableCount }</td>
					<td align="center">{datatableIndex }</td>
					<td align="center">{datatableIndex+1 }</td>
					<td style="text-align:center;color:#00f">No.{id}</td>
					<td align="center">{name}</td>
					<td>{info}</td>
					<td align="center"><a href="doUser.jsp?o=show&id={id }">show</a> <a
						href="doUser.jsp?o=edit&id={id }">edit</a> <a href="doUser.jsp?o=delete&id={id }">delete</a>
					</td>
				</tr>
			</table>
		</div>
		<div class="panelBar" style="width: 780px;" size="5,10,30,50"></div>
	</form>


### 16.2, Specify paging theme SIMPLE + Loading = "show" with tips + Sort Properties

	<script type="text/javascript">
	  DataTable.load("datatable2",{
	  				"pagetheme":'SIMPLE',
	  				"loading":"show"
	  			}); 
	</script>

	<form action="easydatatable/en/doPage_slow.jsp" name="myform">
		<div style="height: 260px;overflow:auto;width: 780px;" class="dataTableScrollDiv">
			<table class="datatable" value="pb" id="datatable2" width="100%" align="center">
				<tr>
					<!-- checkbox -->
					<th width="40"><input type="checkbox" check="mychk" /> <!-- CheckAll --></th>
					<!-- datatableIndex,datatableCount -->
					<th width="80">count</th>
					<th width="100">id</th>
					<th width="100" sort="name">name</th>
					<th width="100" sort="info">info</th>
					<th width="100" >sort、order</th>
					<th>operation</th>
				</tr>
				<!-- Data Show Row-->

				<tr style="display: none;">
					<td style="text-align:center;height: 45px;"><input type="checkbox" name="mychk" value="{id }" /></td>
					<td align="center">{datatableCount }</td>
					<td style="text-align:center;color:#00f">No.{id}</td>
					<td align="center">{name}</td>
					<td>{info}</td>
					<td>
					sort={sort}<br/>
					order={order}
					
					</td>
					<td align="center"><a href="doUser.jsp?o=show&id={id }">show</a> <a
						href="doUser.jsp?o=edit&id={id }">edit</a> <a href="doUser.jsp?o=delete&id={id }">delete</a></td>
				</tr>
			</table>
		</div>
		<div class="panelBar" style="width: 780px;" size="5,10,30,50"></div>
	</form>


### 16.3, Judge sentences DataTable expression + Loading = "none" + row = "10"

	<form action="easydatatable/en/doPage2.jsp" name="myform">
		<div style="height: 260px;overflow:auto;width: 780px;" class="dataTableScrollDiv">
			<table class="datatable easydatatable" loading="none" id="datatable3" width="100%" align="center">
				<tr>
					<!-- checkbox -->
					<th width="40"><input type="checkbox" check="mychk" /> <!-- CheckAll --></th>
					<!-- datatableIndex,datatableCount -->
					<th width="80">count</th>
					<th width="80">index</th>
					<th width="100">id</th>
					<th width="100">name</th>
					<th width="100">info</th>
					<th>operation</th>
				</tr>
				<!-- Data Show Row-->

				<tr style="display: none;">
					<td style="text-align:center;height: 45px;"><input type="checkbox" name="mychk" value="{id }" />
					</td>
					<td align="center">{datatableCount+(pageNo-1)*rowPerPage}</td>
					<td align="center">{datatableIndex+(pageNo-1)*rowPerPage}</td>
					<td style="text-align:center;color:#00f">No.{id}</td>
					<td align="center">{name}</td>
					<td>{info}</td>
					<td align="center">
						<!-- DataTable expression --> %{ if(id%2==0){ var op='<a href="doUser.jsp?o=show&id='+id+'">show</a>&nbsp;&nbsp;';
						op+='<a href="doUser.jsp?o=edit&id={id }">edit</a>'; DataTable.out(op); }else{ DataTable.out('<a
						href="doUser.jsp?o=show&id={id }">show</a>&nbsp;&nbsp;<a href="doUser.jsp?o=edit&id={id }">edit</a>&nbsp;&nbsp;<a
						href="doUser.jsp?o=delete&id={id }">delete</a>'); } }%</td>
				</tr>
			</table>
		</div>
		<div class="panelBar" style="width: 780px;" size="5,10,30,50" row="10"></div>
	</form>



### 16.4, With start and end data loading paging event handler + Custom sort indicator

	<script type="text/javascript">

	//Custom sort indicator for the datatable4
	DataTable.sort["datatable4"]={
		order_default:"<img src='easydatatable/imagesorder_default.gif'/>",
		order_up:"<img src='easydatatable/images/order_up.gif'/>",
		order_down:"<img src='easydatatable/images/order_down.gif'/>"
	};
	$(function(){
		//4. With start and end data loading paging event handler
		//Achieve initialization loading effect, after loading is complete hide loading, display the initial data
		DataTable.load(
					"datatable4",
					{
						loading : "<div><img src=\"easydatatable/images/loading.gif\"/><br/>Data is loading ......</div>",
						//Every time when loading data execution begins (table object; true representative of the first load, false representatives paging load)
						/*
						o：Now DataTable
						 initFlag：true representative of the first load data (initialization tables), false representatives of the page to load
						 */
						"start" : function(dataTableObj, initFlag) {
							if (initFlag) { //First loaded, uninitialized
								//	console.info('init start...');
								$("#loading").show(); //Shows the initial loading tips
								$("#dataDiv").hide(); //Hidden data div
								$("#dataPageDiv").hide(); //Hidden page div
							} else {
								//console.info('load start...');

							}

						},
						//Data execution every time when loading end (table object; true representative of the first load, false representatives paging load)
						"end" : function(dataTableObj, initFlag) { //true representative of the first waiting to load, false representatives of the page to load
							if (initFlag) { //First loaded, uninitialized
								//console.info('init end...');
								$("#loading").hide(); //Hide initial load tips
								$("#dataDiv").show(); //Display data div
								$("#dataPageDiv").show(); //Showing page div
							} else {
								//console.info('load end...');
							}
						}

					});
	 });

	<form action="easydatatable/en/doPage_slow.jsp" name="myform">
		<!-- loading tips DIV, displayed when you first load data -->
		<div id="loading"
			style="border:1px solid #efefef; text-align: center;width: 780px;height: 285px;display: none;font-size: 14px;">
			<img src="easydatatable/images/loading.gif" /><br />Data is loading ......
		</div>
		<div style="height: 260px;overflow:auto;width: 780px;" class="dataTableScrollDiv" id="dataDiv">
			<table class="datatable" id="datatable4" width="100%" align="center" value="pb">
				<tr>
					<th width="40"><input type="checkbox" onclick="DataTable.checkAll(this,'mychk')" /> <!-- CheckAll -->
					</th>
					<!-- datatableCount -->
					<th width="80">countPerTotal</th>
					<th width="100">id</th>
					<th width="150" sort="name">name</th>
					<th width="150">info</th>
					<th width="100" >sort、order</th>
					<th>operation</th>
				</tr>
				<!-- Data Show Row-->
				<tr style="display: none;">
					<td style="text-align:center;height: 45px;"><input type="checkbox" name="mychk" value="{id }" />
					</td>
					<td align="center" style="text-align:center;height: 45px;">{datatableCount+(pageNo-1)*rowPerPage}</td>
					<td style="text-align:center;color:#00f">No.{id}</td>
					<td align="center">{name}</td>
					<td>{info}</td>
					<td>
					sort={sort}<br/>
					order={order}
					
					</td>
					<td align="center"><a href="doUser.jsp?o=show&id={id }">show</a> <a
						href="doUser.jsp?o=edit&id={id }">edit</a> <a href="doUser.jsp?o=delete&id={id }">delete</a>
					</td>
				</tr>
			</table>
		</div>
		<div class="panelBar" style="width: 780px;" size="5,10,30,50" pagetheme="FULL" id="dataPageDiv"></div>

	</form>



### 16.5 Cancel tabs

	<form action="easydatatable/en/doPage.jsp" name="myform">
		<div style="height: 280px;overflow:auto;width: 780px;">
			<table class="easydatatable datatable" id="datatable5" width="100%" align="center" value="pb">
				<tr>
					<th width="40"><input type="checkbox" onclick="DataTable.checkAll(this,'mychk')" /> <!-- CheckAll -->
					</th>
					<!-- datatableCount -->
					<th width="80">count</th>
					<th width="100">id</th>
					<th width="150" sort="name">name</th>
					<th width="150">info</th>
					<th>operation</th>
				</tr>
				<!-- Data show row -->
				<tr style="display: none;">
					<td style="text-align:center;height: 45px;"><input type="checkbox" name="mychk" value="{id }" /></td>
					<td align="center" style="text-align:center;height: 45px;">{datatableCount}</td>
					<td style="text-align:center;color:#00f">No.{id}</td>
					<td align="center">{name}</td>
					<td>{info}</td>
					<td align="center"><a href="doUser.jsp?o=show&id={id }">show</a> <a href="doUser.jsp?o=edit&id={id }">edit</a>
						<a href="doUser.jsp?o=delete&id={id }">delete</a></td>
				</tr>
			</table>
		</div>
		<div class="panelBar" style="width: 780px;display: none" size="5,10,30,50" pagetheme="no" row="10"></div>
	</form>



### 16.6, Custom Paging Example 1

	<form action="easydatatable/en/doPage2.jsp" name="myform">
		<div style="height: 280px;overflow:auto;width: 780px;">
			<table class="easydatatable datatable" id="datatable6" width="100%" align="center">
				<tr>
					<th width="40"><input type="checkbox" onclick="DataTable.checkAll(this,'mychk')" /> <!-- CheckAll -->
					</th>
					<!-- datatableCount -->
					<th width="80">pageNo</th>
					<th width="100">id</th>
					<th width="150" sort="name">name</th>
					<th width="150">info</th>
					<th>operation</th>
				</tr>
				<!-- Data show row -->
				<tr style="display: none;">
					<td style="text-align:center;height: 45px;"><input type="checkbox" name="mychk" value="{id }" />
					</td>
					<td align="center" style="text-align:center;height: 45px;">{pageNo}</td>
					<td style="text-align:center;color:#00f">No.{id}</td>
					<td align="center">{name}</td>
					<td>{info}</td>
					<td align="center"><a href="doUser.jsp?o=show&id={id }">show</a> <a href="doUser.jsp?o=edit&id={id }">edit</a>
						<a href="doUser.jsp?o=delete&id={id }">delete</a>
					</td>
				</tr>
			</table>
		</div>
		<div class="panelBar" style="font-size:14px; width: 780px;height: 40px;line-height: 40px;"
			size="5,10,30,50" pagetheme="no">
			Now {pageNo}/{maxPage}  page&nbsp;&nbsp; Now {rowPerPage}/{totalCount} row {order} {sort} &nbsp;&nbsp; <a href="#"
				onclick="DataTable.go('datatable6',1,5);return false;">first</a> &nbsp;&nbsp;<a href="#"
				onclick="DataTable.go('datatable6','{pageNo-1}',5);return false;">previous </a> &nbsp;&nbsp;<a href="#"
				onclick="DataTable.go('datatable6','{pageNo+1}',5);return false;">next</a> &nbsp;&nbsp;<a href="#"
				onclick="DataTable.go('datatable6','{maxPage}',5);return false;">last</a>

		</div>
	</form>



### 16.7, Custom Paging Example 2

	<form action="easydatatable/en/doPage.jsp" name="myform">
		<div style="height: 280px;overflow:auto;width: 780px;">
			<table class="easydatatable datatable" id="datatable7" width="100%" align="center" value="pb">
				<tr>
					<th width="40"><input type="checkbox" check="mychk" /> <!-- CheckAll --></th>
					<!-- datatableCount -->
					<th width="80">pageNo</th>
					<th width="100">id</th>
					<th width="150" sort="name">name</th>
					<th width="150">info</th>
					<th>operation</th>
				</tr>
				<!-- Data show row -->
				<tr style="display: none;">
					<td style="text-align:center;height: 45px;"><input type="checkbox" name="mychk" value="{id }" /></td>
					<td align="center" style="text-align:center;height: 45px;">{pageNo}</td>
					<td style="text-align:center;color:#00f">No.{id}</td>
					<td align="center">{name}</td>
					<td>{info}</td>
					<td align="center"><a href="doUser.jsp?o=show&id={id }">show</a> <a href="doUser.jsp?o=edit&id={id }">edit</a>
						<a href="doUser.jsp?o=delete&id={id }">delete</a></td>
				</tr>
			</table>
		</div>
		<div class="panelBar" style="width: 780px;height: 40px; line-height: 40px;" size="5,10,30,50"
			pagetheme="no" row="8">
			Now{pageNo}/{maxPage} page Now{rowPerPage}/{totalCount} row {order} {sort} <input type="button" value="first"
				onclick="DataTable.go('datatable7',1)"> <input type="button" value="previous "
				onclick="DataTable.go('datatable7','{pageNo-1}')"> <input type="button" value="next"
				onclick="DataTable.go('datatable7','{pageNo+1}')"> <input type="button" value="last"
				onclick="DataTable.go('datatable7','{maxPage}')">
		</div>
	</form>



### 16.8, With a search page + loading="hide"

	DataTable.load("datatable8", {
			"loading" : "hide"
	});


	<form action="easydatatable/en/doPage2.jsp" name="myform">
		<div style="margin: 20px 0px;">
			username：<input type="text" name="user.name" class="txt_test" />
			 userinfo：<input type="text" name="user.info" class="txt_test" /> 
			 <!-- HTML Enhancement: Use data_search class styles-->
			<input type="button" class="btn_test data_search" value="search" />
			 <!-- JS function: DataTable.load ('DataTableID')  -->
			<input type="button" class="btn_test" onclick="DataTable.load('datatable8')" value="search2"/>
		</div>
		<div style="height: 260px;overflow:auto;width: 780px;" class="dataTableScrollDiv">

			<table class="datatable " id="datatable8" width="100%" align="center">
				<tr>
					<!-- checkbox -->
					<th width="40"><input type="checkbox" check="mychk" /> <!-- CheckAll --></th>
					<!-- datatableIndex,datatableCount -->
					<th width="60">count</th>
					<th width="180">condition</th>
					<th width="100">id</th>
					<th width="100">name</th>
					<th width="100">info</th>
					<th>operation</th>
				</tr>
				<!-- Data Show Row-->
				<tr style="display: none;">
					<td style="text-align:center;height: 45px;"><input type="checkbox" name="mychk" value="{id }" />
					</td>
					<td align="center">{datatableCount+(pageNo-1)*rowPerPage}</td>
					<td align="center">{user.name},{user.info}</td>
					<td style="text-align:center;color:#00f">No.{id}</td>
					<td align="center">{name}</td>
					<td>{info}</td>
					<td align="center">
						<!-- DataTable expression --> %{ if(id%2==0){ DataTable.out('<a href="doUser.jsp?o=show&id={id }"
						target="ajax">show</a>&nbsp;&nbsp;<a href="doUser.jsp?o=edit&id={id }" target="ajax">edit</a>'); }else{
						DataTable.out('<a href="doUser.jsp?o=show&id={id }" target="ajax">show</a>&nbsp;&nbsp;<a
						href="doUser.jsp?o=edit&id={id }" target="ajax">edit</a>&nbsp;&nbsp;<a
						href="doUser.jsp?o=delete&id={id }" target="ajax">delete</a>'); } }%</td>
				</tr>
			</table>
		</div>
		<div class="panelBar" style="width: 780px;" size="5,10,30,50"></div>
	</form>


### 16.9,  Dynamic range of data sources to load + NowPage sort of static data filtering query + default like_i query matching MatchMode

	<form action="easydatatable/en/doPage2.jsp" name="myform">
		<div style="margin: 20px auto;">
			username：<input type="text" name="name" class="txt_test" value="4" />
			userinfo：<input type="text" name="info" class="txt_test" value="INFO_2"/>
			<br/><br/>
			<div class="seaD">NowPageCurrent page data range static filters (also supports dynamic data sources and static data source page filtering)</div>
			<br/>
			<!-- Multiple conditions AND query, HTML and JS function to achieve enhanced -->
		    <input type="button" class="btn_test2 data_static_search" value="NowPage search AND" />
			<input type="button" class="btn_test2" value="NowPage search AND2" onclick="DataTable.staticSearch('datatable9')"/>
		    
		    <!-- Multiple conditions OR queries, HTML and JS function to achieve enhanced -->
		    <input type="button" class="btn_test2 data_static_search_or" value="NowPage search OR" />
			<input type="button" class="btn_test2" value="NowPage search OR2" onclick="DataTable.staticSearch('datatable9',true)" />
		
		</div>
		<div style="height: 260px;overflow:auto;width: 780px;" class="dataTableScrollDiv">
			<table class="datatable easydatatable" id="datatable9" width="100%" align="center">
				<tr>
					<!-- checkbox -->
					<th width="40"><input type="checkbox"  check="mychk"  /> <!-- CheckAll -->
					</th>
					<!-- datatableIndex,datatableCount -->
					<th width="80">count</th>
					<th width="80">index</th>
					<th width="100" staticSort="id">id</th>
					<th width="100" staticSort="name">name</th>
					<th width="100">info</th>
					<th>operation</th>
				</tr>
				<!-- Data Show Row-->

				<tr style="display: none;">
					<td style="text-align:center;height: 45px;"><input type="checkbox" name="mychk" value="{id }" />
					</td>
					<td align="center">{datatableCount+(pageNo-1)*rowPerPage}</td>
					<td align="center">{datatableIndex+(pageNo-1)*rowPerPage}</td>
					<td style="text-align:center;color:#00f">No.{id}</td>
					<td align="center">{name}</td>
					<td>{info}</td>
					<td align="center">
						<!-- DataTable expression --> %{ if(id%2==0){ DataTable.out('<a href="doUser.jsp?o=show&id={id }"
						target="ajax">show</a>&nbsp;&nbsp;<a href="doUser.jsp?o=edit&id={id }" target="ajax">edit</a>'); }else{
						DataTable.out('<a href="doUser.jsp?o=show&id={id }" target="ajax">show</a>&nbsp;&nbsp;<a
						href="doUser.jsp?o=edit&id={id }" target="ajax">edit</a>&nbsp;&nbsp;<a
						href="doUser.jsp?o=delete&id={id }" target="ajax">delete</a>'); } }%</td>
				</tr>
			</table>
		</div>
		<div class="panelBar" style="width: 780px;" size="5,10,30,50" row="30"></div>
	</form>


### 16.10, Static load JSON data source + NowPage range of static data filtering query without sorting + paging (2.X)


	<script type="text/javascript">
		//static JSON data
		var jsonData ={
			"rowPerPage": 10,
			"pageNo" :1,
			"data" : [ {
				"id" : 1,
				"name" : "USER_1",
				"info" : "INFO_1"
			}, {
				"id" : 2,
				"name" : "USER_2",
				"info" : "INFO_2"
			}, {
				"id" : 3,
				"name" : "USER_3",
				"info" : "INFO_3"
			}, {
				"id" : 4,
				"name" : "USER_4",
				"info" : "INFO_4"
			}, {
				"id" : 5,
				"name" : "USER_5",
				"info" : "INFO_5"
			}, {
				"id" : 6,
				"name" : "USER_6",
				"info" : "INFO_6"
			}, {
				"id" : 7,
				"name" : "USER_7",
				"info" : "INFO_7"
			}, {
				"id" : 8,
				"name" : "USER_8",
				"info" : "INFO_8"
			}, {
				"id" : 9,
				"name" : "USER_9",
				"info" : "INFO_9"
			}, {
				"id" : 10,
				"name" : "USER_10",
				"info" : "INFO_10"
			}, {
				"id" : 11,
				"name" : "USER_11",
				"info" : "INFO_11"
			}, {
				"id" : 12,
				"name" : "USER_12",
				"info" : "INFO_12"
			}, {
				"id" : 13,
				"name" : "USER_13",
				"info" : "INFO_13"
			}, {
				"id" : 14,
				"name" : "USER_14",
				"info" : "INFO_14"
			}, {
				"id" : 15,
				"name" : "USER_15",
				"info" : "INFO_15"
			}, {
				"id" : 16,
				"name" : "USER_16",
				"info" : "INFO_16"
			}, {
				"id" : 17,
				"name" : "USER_17",
				"info" : "INFO_17"
			}, {
				"id" : 18,
				"name" : "USER_18",
				"info" : "INFO_18"
			}, {
				"id" : 19,
				"name" : "USER_19",
				"info" : "INFO_19"
			}, {
				"id" : 20,
				"name" : "USER_20",
				"info" : "INFO_20"
			}, {
				"id" : 21,
				"name" : "USER_21",
				"info" : "INFO_21"
			}, {
				"id" : 22,
				"name" : "USER_22",
				"info" : "INFO_22"
			}, {
				"id" : 23,
				"name" : "USER_23",
				"info" : "INFO_23"
			}, {
				"id" : 24,
				"name" : "USER_24",
				"info" : "INFO_24"
			}, {
				"id" : 25,
				"name" : "USER_25",
				"info" : "INFO_25"
			}, {
				"id" : 26,
				"name" : "USER_26",
				"info" : "INFO_26"
			}, {
				"id" : 27,
				"name" : "USER_27",
				"info" : "INFO_27"
			}, {
				"id" : 28,
				"name" : "USER_28",
				"info" : "INFO_28"
			}, {
				"id" : 29,
				"name" : "USER_29",
				"info" : "INFO_29"
			}, {
				"id" : 30,
				"name" : "USER_30",
				"info" : "INFO_30"
			} ]
		};

		$(function(){
			DataTable.staticLoad("datatable10", jsonData,{"row":30});
		});
	</script>


	<form action="" name="myform">
		<div style="margin: 20px auto;">
			username（ sql_i MatchMode）： <input type="text" name="name" class="txt_test" value="USER_1%" mode="sql_i" />
			userinfo（ like_i MatchMode）：<input type="text" name="info" class="txt_test" />
			<br/><br/>
					<div class="seaD">NowPageCurrent page data range static filters (also supports dynamic data sources and static data source page filtering)</div>
			<br/>
			<!-- Multiple conditions AND query, HTML and JS function to achieve enhanced -->
		    <input type="button" class="btn_test2 data_static_search" value="NowPage search AND" />
			<input type="button" class="btn_test2" value="NowPage search AND2" onclick="DataTable.staticSearch('datatable10')"/>
		    <!-- Multiple conditions OR queries, HTML and JS function to achieve enhanced -->
		    <input type="button" class="btn_test2 data_static_search_or" value="NowPage search OR" />
			<input type="button" class="btn_test2" value="NowPage search OR2" onclick="DataTable.staticSearch('datatable10',true)" />
			
		</div>
		<div style="height: 260px;overflow:auto;width: 780px;" class="dataTableScrollDiv">


			<table class="datatable" id="datatable10" width="100%" align="center">
				<tr>
					<!-- checkbox -->
					<th width="40"><input type="checkbox" onclick="DataTable.checkAll(this,'mychk')" /> <!-- CheckAll -->
					</th>
					<!-- datatableIndex,datatableCount -->
					<th width="80">count</th>
					<th width="80">index</th>
					<th width="100" staticSort="id">id</th>
					<th width="100" staticSort="name">name</th>
					<th width="100">info</th>
					<th>operation</th>
				</tr>
				<!-- Data Show Row-->

				<tr style="display: none;">
					<td style="text-align:center;height: 45px;"><input type="checkbox" name="mychk" value="{id }" />
					</td>
					<td align="center">{datatableCount+(pageNo-1)*rowPerPage}</td>
					<td align="center">{datatableIndex+(pageNo-1)*rowPerPage}</td>
					<td style="text-align:center;color:#00f">No.{id}</td>
					<td align="center">{name}</td>
					<td>{info}</td>
					<td align="center">
						<!-- DataTable expression --> %{ if(id%2==0){ DataTable.out('<a href="doUser.jsp?o=show&id={id }"
						target="ajax">show</a>&nbsp;&nbsp;<a href="doUser.jsp?o=edit&id={id }" target="ajax">edit</a>'); }else{
						DataTable.out('<a href="doUser.jsp?o=show&id={id }" target="ajax">show</a>&nbsp;&nbsp;<a
						href="doUser.jsp?o=edit&id={id }" target="ajax">edit</a>&nbsp;&nbsp;<a
						href="doUser.jsp?o=delete&id={id }" target="ajax">delete</a>'); } }%</td>
				</tr>
			</table>
		</div>

	</form>



### 16.11, JSON file data source load + All range of static data filtering query sorting (2.X)

	<script type="text/javascript">
		$(function(){
			DataTable.fileLoad("datatable11","jsonData.json");
		});
	</script>


	<form action="" name="myform">
		<div style="margin: 20px auto;">
			username（ sql_i MatchMode）： <input type="text" name="name" class="txt_test" value="USER_1%" mode="sql_i" />
			userinfo（ like_i MatchMode）：<input type="text" name="info" class="txt_test" value="INFO_"/>
			<br/><br/>
					<div class="seaD">AllCurrent page data range static filters (support only static data source page filtering)</div>
			<br/>
			<!-- Multiple conditions AND query, HTML and JS function to achieve enhanced -->
			<input type="button" class="btn_test2 data_static_searchAll" value="All search AND" />
			<input type="button" class="btn_test2" value="All search AND2"  onclick="DataTable.staticSearchAll('datatable11')"/> 
			<!-- Multiple conditions OR queries, HTML and JS function to achieve enhanced -->
			<input type="button" class="btn_test2 data_static_searchAll_or" value="All search OR" />
			<input type="button" class="btn_test2" value="All search OR2" onclick="DataTable.staticSearchAll('datatable11',true)"/> 
			
		</div>
		<div style="height: 260px;overflow:auto;width: 780px;" class="dataTableScrollDiv">


			<table class="datatable" id="datatable11" width="100%" align="center">
				<tr>
					<!-- checkbox -->
					<th width="40"><input type="checkbox" onclick="DataTable.checkAll(this,'mychk')" /> <!-- CheckAll -->
					</th>
					<!-- datatableIndex,datatableCount -->
					<th width="80">count</th>
					<th width="80">index</th>
					<th width="100" staticSort="id">id</th>
					<th width="100" staticSort="name">name</th>
					<th width="100">info</th>
					<th>operation</th>
				</tr>
				<!-- Data Show Row-->

				<tr style="display: none;">
					<td style="text-align:center;height: 45px;"><input type="checkbox" name="mychk" value="{id }" />
					</td>
					<td align="center">{datatableCount+(pageNo-1)*rowPerPage}</td>
					<td align="center">{datatableIndex+(pageNo-1)*rowPerPage}</td>
					<td style="text-align:center;color:#00f">No.{id}</td>
					<td align="center">{name}</td>
					<td>{info}</td>
					<td align="center">
						<!-- DataTable expression --> %{ if(id%2==0){ DataTable.out('<a href="doUser.jsp?o=show&id={id }"
						target="ajax">show</a>&nbsp;&nbsp;<a href="doUser.jsp?o=edit&id={id }" target="ajax">edit</a>'); }else{
						DataTable.out('<a href="doUser.jsp?o=show&id={id }" target="ajax">show</a>&nbsp;&nbsp;<a
						href="doUser.jsp?o=edit&id={id }" target="ajax">edit</a>&nbsp;&nbsp;<a
						href="doUser.jsp?o=delete&id={id }" target="ajax">delete</a>'); } }%</td>
				</tr>
			</table>
		</div>
		<div class="panelBar" style="width: 780px;" size="5,10,30,50" row="15"></div>
	</form>



[Demo online](http://www.lightfeel.com/easy/easydatatable/en/index.jsp#demo 'Demo online')

Contact, feedback, custom Email:<in.think@gmail.com>

