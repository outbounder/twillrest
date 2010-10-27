# Simple rest functional testing extension to twill (http://twill.idyll.org/) #

## based/uses/credits ##
* urllib2
* urllib2_file (Fabien SEISEN, http://fabien.seisen.org/python/)
* xml2obj [recipe from ActiveState code, lost the link unfortunately]

## Features ##
* assert get response contains/not contains string
* assert post response contains/not contains string
* assert file upload response contains/not contains string
* maintain functional test session/context and support for custom parsing of every successful REST request

## Limits ##
* currently test files should be placed in the same folder as the extension files
* returned cookies are not kept, ie not send in further requests

## Usage examples ##
### basic functions ###
    extend_with rest
    extend_with reststate
    
    assertGetContains something http://domain/user/1
    assertGetNotContains something http://domain/user/2
    assertPostContains something http://domain/user/1 field1=value1&field2=value2
    assertPostNotContains something http://domain/user/2 field1=value&field2=value&field3=value
    assertFileUploadContains something http://domain/user/picture file ./picture.png
    assertFileUploadNotContains something http://domain/user/picture file ./picture.png
### maintaining state ###
_file test_
    assertGetContains something http://domain/auth
    xmlparseLastResponseWith pythonModuleName moduleMethodName
    assertGetContains something http://domain/add?session=%sessionkey%
_file pythonModuleName.py_
    def moduleMethodName(response,keyvalues):
        if response.success == "OK":
            keyvalues['%sessionkey%'] = response.sessionkey
            
*Note that response is object*
The response from the last assertGet* or assertPost* is piped to xml2obj method and then its result is given to module's method in question

keyvalues is a Dictionary within the executing rest context, and its key->values pairs are used to replace any found matches on every assert* statements

## TODO/Roadmap ##
* provide support to separete the test files with the python code and extensions to twill
* provide python documentation(?)
* use true RESTclient python implementation (http://code.google.com/p/python-rest-client2/)
