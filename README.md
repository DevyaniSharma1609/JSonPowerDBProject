# JSonPowerDBProject
The project is built using html and JSon Power DataBase. The web page is able to accept the data from the user and then adds the provided data into the  JSon's database in the Talend API.

<!DOCTYPE html>
<!--
To change this license header, choose License Headers in Project Properties.
To change this template file, choose Tools | Templates
and open the template in the editor.
-->
<html lang="en">
	<head>
		<title>JSON PowerDB Example</title>
	
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<link rel="stylesheet"
		href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
		<script
			src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
		<script	
		src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
	</head>
	
	
	
<body style="background-color: LightGray";>
	
    <div class="container">
	<h2 style="text-align: center"; > Employee Information</h2>
	<br>
	<br>
	
	<form id="empForm" method="post">
	<div class="form-group">
		<span><label for="empId">Employee ID:</label> <label id="empIdMsg">
		</label></span>
		<input type="text" class="form-control" name="empId" id="empId"
		placeholder="Enter Employee ID" required>
	</div>
	<br>
	
	<div class="form-group">
		<label for="empName">Employee Name:</label>
		<input type="text" class="form-control" id="empName"
		placeholder="Enter Employee Name" name="empName" >
	</div>
	<br>
	
	
	<div  class="form-group">
		<label  for="empEmail">Email:</label>
		<input style="align: center"; type="email" class="form-control" id="empEmail"
		placeholder="Enter Employee Email" name="empEmail">
	</div>
	
	
<input type="button" style="background-color:DodgerBlue;margin-left:auto;margin-right:auto;display:block;margin-top:0%;margin-bottom:0%"; class="btn btn-primary" id="empSave" value="Submit"
onclick="saveEmployee();">
</form>

</div>
<script>

$("#empId").focus();
function validateAndGetFormData() {
var empIdVar = $("#empId").val();
if (empIdVar === "") {
alert("Employee ID Required Value");
$("#empId").focus();
return "";
}
var empNameVar = $("#empName").val();
if (empNameVar === "") {
alert("Employee Name is Required Value");
$("#empName").focus();
return "";
}
var empEmailVar = $("#empEmail").val();
if (empEmailVar === "") {
alert("Employee Email is Required Value");
$("#empEmail").focus();
return "";
}
var jsonStrObj = {
empId: empIdVar,
empName: empNameVar,
empEmail: empEmailVar,
};
return JSON.stringify(jsonStrObj);
}
// This method is used to create PUT Json request.
function createPUTRequest(connToken, jsonObj, dbName, relName) {
var putRequest = "{\n"
+ "\"token\" : \""
+ connToken
+ "\","
+ "\"dbName\": \""
+ dbName
+ "\",\n" + "\"cmd\" : \"PUT\",\n"
+ "\"rel\" : \""
+ relName + "\","
+ "\"jsonStr\": \n"
+ jsonObj
+ "\n"
+ "}";
return putRequest;
}
function executeCommand(reqString, dbBaseUrl, apiEndPointUrl) {
var url = dbBaseUrl + apiEndPointUrl;
var jsonObj;
$.post(url, reqString, function (result) {
jsonObj = JSON.parse(result);
}).fail(function (result) {
var dataJsonObj = result.responseText;
jsonObj = JSON.parse(dataJsonObj);
});
return jsonObj;
}
function resetForm() {
$("#empId").val("")
$("#empName").val("");
$("#empEmail").val("");
$("#empId").focus();
}
function saveEmployee() {
var jsonStr = validateAndGetFormData();
if (jsonStr === "") {
return;
}
var putReqStr = createPUTRequest("90937197|-31948800293621370|90933127",
jsonStr, "Employee", "Emp-Rel");
alert(putReqStr);
jQuery.ajaxSetup({async: false});
var resultObj = executeCommand(putReqStr,
"http://api.login2explore.com:5577", "/api/iml");
alert(JSON.stringify(resultObj));
jQuery.ajaxSetup({async: true});
resetForm();
}
</script>
</body>
</html>
