# StudentData

<html>
    <head>
        <title>Student Data</title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css"><!-- comment -->
        <script src="https://ajax.googleeapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
        <script src="https://maxdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
        <!--<script src="https://ajax.googleaoi.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>-->
    </head>

    <body>
        <div class="container">
            <h2> Student Information form</h2>
            <form id="stdForm" method="post">
                <div class="form-group">
                    <span><label for="stdId">Student ID:</label><label id="stdIdMsg"></label></span>
                    <input type="text" class="form-control" name="stdId" id="stdId" placeholder="Enter Student ID" required>
                </div>
                <div class="form-group">
                    <label for="stdName">Student Name:</label>
                    <input type="text" class="form-control" id="stdName" placeholder="Enter Student Name" name="stdName">
                </div>
                <div class="form-group">
                    <label for="stdEmail">Student Email Id:</label>
                    <input type="email" class="form-control" id="stdEmail" placeholder="Enter Student Email" name="stdEmail">
                </div>
                <input type="button" class="btn btn-primary" id="stdSave" value="Save" onclick="saveStudent();">
            </form>   

        </div>

        <script>
            function validateAndGetFormData()
            {
                var stdIdVar = $("#stdId").val();
                if (stdIdVar === " ")
                {
                    alert("Student Id required a value");
                    $("#stdId").focus();
                    return " ";

                }

                var stdNameVar = $("#stdName").val();
                if (stdNameVar === " ") {
                    alert("Student Name required a value");
                    $("#stdName").focus();
                    return " ";

                }

                var stdEmailVar = $("#stdEmail").val();
                if (stdEmailVar === " ") {
                    alert("Student Email required a value");
                    $("#stdEmail").focus();
                    return " ";

                }
                var jsonStrObj = {
                    stdId : stdIdVar,
                    stdName : stdNameVar,
                    stdEmail : stdEmailVar
                };
                return JSON.stringify(jsonStrObj);

            }

            function createPUTRequest(connToken, jsonObj, dbName, relName) {
                var putRequest = "{\n"
                        + "\"token\" : \" "
                        + connToken
                        + "\","
                        + "\"dbName\" : \" "
                        + dbName
                        + "\",\n" + "\"cmd\" : \"PUT\" , \n"
                        + "\"rel\" : \" "
                        + relName + "\","
                        + "\"jsonStr\" : \n"
                        + jsonObj
                        + "\n"
                        + "}";
                return putRequest;


            }

            function executeCommand(reqString, dbBaseUrl, apiEndPointUrl) {
                var url = dbBaseUrl + apiEndPointUrl;
                var jsonObj;
                $.post(url, reqString, function(result) {
                    json = JSON.parse(result);
                }).fail(function(result) {
                    var dataJsonObj = result.responseText;
                    jsonObj = JSON.parse(dataJsonObj);

                });
                return jsonObj;

            }
            function resetForm() {
                $("#stdId").val(" ");
                $("#stdName").val(" ");
                $("#stdEmail").val(" ");
                $("#stdId").focus();
            }

            function saveStudent()
            {
                var jsonStr = validateAndGetFormData();
                if (jsonStr === " ")
                {
                    return;
                }
                var putReqStr = createPUTRequest("90939138|-31949293218687622|90940286", jsonStr, "StudentData", "Student");
                alert(putReqStr);
                jQuery.ajaxSetup({async: false});
                var resultObj = executeCommandAtGivenBaseUrl(putReqStr, "http://api.login2explore.com:5577", "/api/iml");
                
                alert(JSON.stringify(resultObj));
                jQuery.ajaxSetup({async: true});
                resetForm();

            }
        </script>
    </body>
</html>
