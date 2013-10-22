本插件为DataTable的英文版本，中文版本请参考：https://github.com/ushelp/EasyDataTable


EasyDataTable AJAX Pagination jQuery Plugins_en


Using AJAX pagination can improve data loading and display speed, reduce network traffic, improve the customer experience degrees; while to refresh only partial, solution when there are multiple data displayed on the page table area, the traditional way paging causes a page refresh all.
EasyDataTable AJAX pagination plugin is based on jQuery pagination plugin best, no one, simple, clear, easy to use, flexible, comprehensive; own tabs; supports sorting; built EasyDataTable expression language can be enhanced through the JavaScript programming paging.


Rapid development steps:
1、On the web page introducing EasyDataTable js and css files

<link rel="stylesheet" href="css/datatable.css" type="text/css"></link>
<script type="text/javascript" src="js/jquery-1.10.2.min.js"></script>
<script type="text/javascript" src="js/easy.datatable.min.js"></script>

2、initialization tables for Ajax pagination data 

Method One：Add easydatatable class style to the table then, is equivalent to the 'Method Two' of DataTable.load ('tableid').
<table class="datatable easydatatable"  id="datatable3"  width="760px" align="center">

Method Two：Use DataTable.load (tableId, parameterInformation) method to the form for Ajax pagination for data initialization
DataTable.load( tableid [ , easydataParameters] );

Parameter Description:
tableid: mandatory parameters, display data table id

easydataParameters: optional parameter, that specifies EasyDataTable parameter information, supporting the argument currently supported parameters:
{
pagetheme: 'paging theme'
loading: 'whether to display the loading tip'
language: 'tabs language'
}

pagetheme: optional parameter(Default : DataTable.FULL_PAGE.),tables theme supports two optional paging topics:
DataTable.FULL_PAGE (full theme, the default display all pagination options)
DataTable.SIMPLE_PAGE (simple theme, do not show this page after page quickly jump label)


loading: optional parameter( defaults : false), is displayed when loading data loading prompt "the data is being read in the ......" optional value of true or false

language:Optional parameters, settings tabs display language, the default value
{
			"first":'first',
			"previous":'previous',
			"next":'next',
			"last":'last',
			"totalPage":'total {0} pages',
			"totalCount":'total {0} rows',
			"rowPerPage":'page for {0} rows'	
	} 
May need to be redefined, {0} is a placeholder for the corresponding data display, must exist.

  <script type="text/javascript">
  $(function(){
  			var pageLanguage={
				"first":'first',
				"previous":'previous',
				"next":'next',
				"last":'last',
				"totalPage":'total {0} pages',
				"totalCount":'total {0} rows',
				"rowPerPage":'page for {0} rows',
		    };
		    
  			DataTable.load("datatable",{
  				"pagetheme":DataTable.SIMPLE_PAGE,
  				"loading":true,
  				"language":pageLanguage
  			});
  			DataTable.load("datatable2");
  			DataTable.load("datatable3");
  			DataTable.load("datatable4");
  });
  </script>


3、Pagination table structure
<form action="server pagination URL">

<!--  show data TABLE  -->
<table id="tableId">
<!--  table header ROW  -->
<tr><th></th>    …… </tr>
<!--  data show ROW  -->
<tr><td></td>    …… </tr>
</table>

<!--  pagination label DIV  -->
<div class="panelBar" style="width: 760px;" size="5,10,30,50">	</div>
</form>


Example:

<!-- Create a form action is paged address requests processed -->
<form action="doPage.jsp" name="myform">

   	<div style="height: 260px;overflow:auto;width: 780px;">
  <!--Display data table, you must specify the table id, EasyDatatable use id initialize pagination-->
		     <table class="datatable"  id="datatable"  width="760px" align="center">
            <!-- table header ROW -->
		      	<tr>
		      	<th width="40">
<!-- DataTable.checkAll(this,'checkbox NAME')，can check all or discheck all -->
	<input type="checkbox" onclick="DataTable.checkAll(this,'mychk')" />
			   		</th>
			   		<th width="80">count</th>
			   		<th width="100">id</th>
			   		<th width="100">name</th>
			   		<th width="100">info</th>
			   		<th >operation</th>
		   		</tr> 
		<!-- data show ROW，use {property} EasyDataTable Property Expression Language get value -->
			   	<tr>
			   		<td style="text-align:center;height: 45px;">
            <!-- define checkbox -->
			   			<input type="checkbox" name="mychk" value="{id }"/>
			   		</td>
            <!--Use the built datatableCount property to display the number of data-->
			   		<td align="center"  style="text-align:center;height: 45px;">{datatableCount }</td>
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
<!-- pagination label DIV, use the size attribute to set the number of displayed per page drop-down menu selectable values-->
      	<div class="panelBar" style="width: 760px;" size="5,10,30,50">
			
		</div>
</form>

4、EasyDataTable Expression Language
4.1	EasyDataTable Property Expression  {property}
Property expressions are used in the data display line shows the value of the specified property.
In the property expression can be directly referenced data attributes for the specified attribute data, and supports a variety of mathematical, comparison operators such as JavaScript for basic computing.
{id}  {name}

4.2	EasyDataTable Statement Expression  %{expression statement}%	
Statement expressions are used in the data display line programming statements used to control programming, support the use of the expression in the statement written in JavaScript expression code; supports direct call data attributes; also supports the use of EasyDataTable property expressions.
Statement expression results must be performed using the standard output method statement expression EasyDataTable output:
DataTable.out("CONTENT");

<%--Support JavaScript language expressions, support the use of the property name (with variable references and processing methods) or EasyDataTable property expression (must use quotation marks are defined in the string) directly referenced property value --%>
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

5、EasyTableData Built-in data attribute
only use in data row:
datatableIndex:data on the current page index
datatableCount:data on the current page number
key:Map Data collection can be used to obtain data for the key
use in data row and pagination label DIV
pageNo:now page number
maxPage:max page
rowPerPage:the number of one page
totalCount:the total number of data
order: the sort field
sort:sort, desc or asc
For example:
All the data in the current data in the index: {datatableIndex+(pageNo-1)*rowPerPage}
All data in the current data number: {datatableCount+(pageNo-1)*rowPerPage}


6、Sort Support
In the table header row corresponding to the field you want to sort the cells to add sort = "sort field name" attribute.
<tr>
			   		<th width="80">count</th>
			   	   	<th width="80">index</th>
			   	   	<th width="80">{index+1}</th>
			   		<th width="100" order="id">id</th>
			   		<th width="100" order="name">name</th>
			   		<th width="100" order="info">info</th>
			   		<th >operation</th>
</tr>

Sortable column displays the sort arrow. Click to send the sort field and sort order sort, realize Ascending Descending switching.
 
Server-side and sort through the order parameter names for sorting info.

String sort = request.getParameter("sort");
String order = request.getParameter("order");

DataTable.reload(‘tableid’); can reload DataTable，recovery to no sort status。
<div onclick="DataTable.reload('datatable3')">refresh</div>


7、checkboxes, multiple choice function (Support All / Select)
Join check box in the header row, the click event of the check box called DataTable.checkAll (this, 'checkbox name') can be realized checkbox Select All / Clear All function:
<input type="checkbox" onclick="DataTable.checkAll(this,'mychk')" />
data show ROW：
<input type="checkbox" name="mychk" value="{id }"/>



8、the server-side data requirements
Server must output the following four names paging parameters JSON result:
data: data collection, support for List and Map Collection (built-in attribute key can access the Map key, Map value without the use value. prefix)
pageNo: Current first few pages, digital
rowPerPage: Show the number of digital
totalCount: the total number of data, digital
Optional parameters:
[order]: sort field
[sort]: Sort, desc or asc

If the server-side paging parameters encapsulated in PageBean, for example,
public class PageBean {
	private List data;
	private int pageNo;
	private int rowPerPage;
	private int totalCount;
   //setters&getters ……
}
PageBean object is returned, then the table to add value attribute that specifies the server-side output parameter contains the paging paging json object object name:
<table class="datatable easydatatable"  id="datatable3"  width="760px" align="center" value="pb">


9、 refresh the specified data table
DataTable.reload ("tableId"); / / cancel the sorting effect , refresh the table , reload the data

10、 custom paging
DataTable built-in paging implementation , and provide two sets of topics:
DataTable.FULL_PAGE ( full theme , the default display all pagination options )
 
DataTable.SIMPLE_PAGE ( simple theme , do not show this page after page quickly jump label )
 
10.1、 Using JavaScript initialization data table , you can specify the use of paging topics:
DataTable.load ("datatable", {
  "pagetheme": DataTable.SIMPLE_PAGE
  } ) ;

10.2、 DIV tag is specified in the paging paging topics:
<div class="panelBar" style="width: 760px;height: 40px;" size="5,10,30,50" pagetheme="FULL">

<div class="panelBar" style="width: 760px;height: 40px;" size="5,10,30,50" pagetheme="SIMPLE">

10.3、 cancel the paging and themes :
pagetheme = "no", use display: none hidden , or directly delete some tabs can be.
<div class="panelBar" style="width: 760px;height: 40px;display: none;" size="5,10,30,50" pagetheme="no">


10.4、 Custom paging :
Call DataTable.go (' loading data table id', pages [ Show number of ] ) function , you can implement custom paging jump .
You can also use <input type="hidden" name="rowPerPage" value="8"/> hidden field specifies the default number of displayed per page .


<div class="panelBar" style="width: 760px;height: 40px;" size="5,10,30,50" pagetheme="no">
<input type="hidden" name="rowPerPage" value="8"/>
Current {pageNo} Page / {maxPage} {rowPerPage} per page article / articles of {totalCount}
<input type="button" value="first" onclick="DataTable.go('datatable7',1)"/>
<input type="button" value="previous" onclick="DataTable.go('datatable7',{pageNo-1})"/>
<input type="button" value="next" onclick="DataTable.go('datatable7',{pageNo+1})"/>
<input type="button" value="last" onclick="DataTable.go('datatable7',{maxPage})"/>
</ div>

In the function call parameter if you use EasyDataTable expressions (such as {pageNo}) is required by the single quotation marks, as a string parameter, such as onclick = "DataTable.go ('datatable7', '{pageNo-1}') "/> the '{pageNo-1}'.




11、Forms AJAX Pagination instance...

11.1	DataTable.SIMPLE_PAGE+ Loading
  <script type="text/javascript">
  $(function(){
  			DataTable.load("datatable",DataTable.SIMPLE_PAGE,true);
  });
  </script>

<div style="margin: 20 0 10 0; font-size: 28px;">
DataTable.SIMPLE_PAGE+ Loading
</div>

<form action="doPage.jsp" name="myform">
   	<div style="height: 260px;overflow:auto;width: 780px;">
		     <table class="datatable"  id="datatable"  width="760px" align="center">
		      	<tr>
		      	<th width="40">
			   			<input type="checkbox" onclick="DataTable.checkAll(this,'mychk')" /> <!-- CheckAll -->
			   		</th>
			   	<!-- datatableCount -->
			   		<th width="80">count</th>
			   		<th width="100">id</th>
			   		<th width="100">name</th>
			   		<th width="100">info</th>
			   		<th >operation</th>
		   		</tr> 
		   		<!-- data show ROW -->
			   	<tr>
			   		<td style="text-align:center;height: 45px;">
			   			<input type="checkbox" name="mychk" value="{id }"/>
			   		</td>
			   		<td align="center"  style="text-align:center;height: 45px;">{datatableCount }</td>
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
      	<div class="panelBar" style="width: 760px;" size="5,10,30,50">
		</div>
</form>



11.2	DataTable Expression Language+DataTable.SIMPLE_FULL + Checkbox + Number

   <div style="margin: 40 0 10 0; font-size: 28px;">DataTableexpression</div>
   <form action="doPage.jsp" name="myform">
   	<div style="height: 260px;overflow:auto;width: 780px;">
		     <table class="datatable easydatatable"  id="datatable3"  width="760px" align="center">
		      	<tr>
			   	<!-- checkbox -->
			   		<th width="40">
			   			<input type="checkbox" onclick="DataTable.checkAll(this,'mychk')" /> <!-- CheckAll -->
			   		</th>
			   	<!-- datatableIndex,datatableCount -->
			   		<th width="80">count</th>
			   	   	<th width="80">index</th>
			   	   	<th width="80">{index+1}</th>
			   		<th width="100">id</th>
			   		<th width="100">name</th>
			   		<th width="100">info</th>
			   		<th >operation</th>
		   		</tr> 
		   		<!-- data show ROW -->
		
			   	<tr>
			   		<td style="text-align:center;height: 45px;">
			   			<input type="checkbox" name="mychk" value="{id }"/>
			   		</td>
			   		<td align="center"> {datatableCount+(pageNo-1)*rowPerPage}</td>
			   		<td align="center"> {datatableIndex+(pageNo-1)*rowPerPage}</td>
			   		<td align="center">{datatableIndex+(pageNo-1)*rowPerPage+1}</td>
			   		<td style="text-align:center;color:#00f">No.{id}</td>
			   		<td align="center">{name}</td>
			   		<td>{info}</td>
			   		<td align="center">
			   		<!-- DataTable expression -->
			   		%{ 
				   		if(id%2==0){ 
				   			var op='<a href="doUser.jsp?o=show&id='+id+'">show</a>&nbsp;&nbsp;'; 
				   			op+='<a href="doUser.jsp?o=edit&id={id }">edit</a>';
				   			DataTable.out(op);  
				   		}else{ 
				   			DataTable.out('<a href="doUser.jsp?o=show&id={id }">show</a>&nbsp;&nbsp;<a href="doUser.jsp?o=edit&id={id }">edit</a>&nbsp;&nbsp;<a href="doUser.jsp?o=delete&id={id }">delete</a>');  
				   		} 
			   		 }%
			   		
			   	
				   	</td>
			   	</tr>
		   </table>
    </div>
      	<div class="panelBar" style="width: 760px;" size="5,10,30,50">
			
		</div>

      </form>
   

11.3	Search Pagination

Search button data submission method:
Method one: to the search button to add onclick = "DataTable.load ('the current data table id')"
<input type="button" class="btn_test" onclick="DataTable.load('datatable4')" value="search"/>


Method Two: Give the search button added directly easydatatable_search class style.
<input type="button" class="btn_test easydatatable_search"  value="search"/>


  <div style="margin: 40 0 10 0; font-size: 28px;">Search Pagination</div>
   <form action="doPage.jsp" name="myform">
   <div style="margin: 20px 0px;">
				username：<input type="text" name="user.name" class="txt_test"/> 
				userinfo：<input type="text" name="user.info" class="txt_test"/> 
				<input type="button" class="btn_test easydatatable_search"  value="search"/>
			</div>

   	<div style="height: 260px;overflow:auto;width: 780px;">
	
			
		     <table class="datatable easydatatable"  id="datatable4"  width="760px" align="center">
		      	<tr>
			   	<!-- checkbox -->
			   		<th width="40">
			   			<input type="checkbox" onclick="DataTable.checkAll(this,'mychk')" /> <!-- CheckAll -->
			   		</th>
			   	<!-- datatableIndex,datatableCount -->
			   		<th width="80">count</th>
			   	   	<th width="80">index</th>
			   	   	<th width="80">{index+1}</th>
			   		<th width="100">id</th>
			   		<th width="100">name</th>
			   		<th width="100">info</th>
			   		<th >operation</th>
		   		</tr> 
		   		<!-- data show ROW -->
		
			   	<tr>
			   		<td style="text-align:center;height: 45px;">
			   			<input type="checkbox" name="mychk" value="{id }"/>
			   		</td>
			   		<td align="center">{datatableCount+(pageNo-1)*rowPerPage}</td>
			   		<td align="center">{datatableIndex+(pageNo-1)*rowPerPage}</td>
			   		<td align="center">{datatableIndex+(pageNo-1)*rowPerPage+1}</td>
			   		<td style="text-align:center;color:#00f">No.{id}</td>
			   		<td align="center">{name}</td>
			   		<td>{info}</td>
			   		<td align="center">
			   		<!-- DataTable expression -->
			   		%{ 
				   		if(id%2==0){ 
				   			DataTable.out('<a href="doUser.jsp?o=show&id={id }" target="ajax">show</a>&nbsp;&nbsp;<a href="doUser.jsp?o=edit&id={id }" target="ajax">edit</a>'); 
				   		}else{ 
				   			DataTable.out('<a href="doUser.jsp?o=show&id={id }" target="ajax">show</a>&nbsp;&nbsp;<a href="doUser.jsp?o=edit&id={id }" target="ajax">edit</a>&nbsp;&nbsp;<a href="doUser.jsp?o=delete&id={id }" target="ajax">delete</a>');  
				   		} 
			   		 }%
			   		
			   	
				   	</td>
			   	</tr>
		   </table>
    </div>
      	<div class="panelBar" style="width: 760px;" size="5,10,30,50">
		
		</div>

      </form>


12、EasyDataTable  International Support
EasyDataTable comes with tabs, you need to customize the display of text and language, the text label by language parameter adjustment and edit.
The default tab of words and language: 
{
			"first":'first',
			"previous":'previous',
			"next":'next',
			"last":'last',
			"totalPage":'total {0} pages',
			"totalCount":'total {0} rows',
			"rowPerPage":'page for {0} rows',
	} 
 
Customize tab of words and language:
	var pageLanguage={
				"first":'首页',
				"previous":'上一页',
				"next":'下一页',
				"last":'末页',
				"totalCount":'共{0}条',
				"totalPage":'共{0}页',
				"rowPerPage":'每页显示{0}条'		
		    };
		    
  			DataTable.load("datatable",{
  				"pagetheme":DataTable.SIMPLE_PAGE,
  				"loading":true,
  				"language":pageLanguage
  			});
Default paging configuration defined in the DataTable object MSG properties, through the edit and re-define, configure the default paging text and language.



Demo online：http://www.lightfeel.com/EasyDataTable_en/demo_en.jsp