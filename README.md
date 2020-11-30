Registration and Authentication
In this assignment you will create a set of files that allow users to register, authenticate, and view a protected resource.  You will use html, js, nodejs, nginx, promises, sessions and json files just to name a few.

To submit you will need to give me the server.js file as well as a URL to test your site in.  

I have provided a zip file (.tar.gz actually) that you will retrieve during the setup process below.  Use that to get started.  There are 8 instances of the word "TODO" in the given zip file. You are responsible for filling in the required code in those 8 spots.  

There's also a postman suite of tests that you are free to download and run though this is not a requirement.  I will be using them to guide my grading, but I will also be looking at your code and poking your website manually.  The postman tests will only test the nodejs process and not your HTML.  You can download my test suite here (Links to an external site.) and then import it into postman.  You will need to set up a HOST environment variable with a value of http://youripaddress for the tests to appropriately run.

In server/server.js (45 pts)
complete computeHash function
given a password and salt uses crypto libraries returns a sha256 hash of password+salt base64 encoded
Complete the handler for POST /user/auth
get userid and password from request body
validate that username and password exist and aren't empty strings
return appropriate error message to login
if the username is admin, use the adminObj thats available in global scope instead of calling getUser(userid) to get the userObj
validate getUser returns a user for given userid
return appropriate error message to login
validate hashed given password using salt from getUser object (or adminObj) matches passwordHash on getUser obj
return appropriate error message to login
if all that's good save the userid into the request session and send user to /dynamic/user/info
Complete the handler for GET /user/exists
get userid from request query
validate userid exists and is not empty
return json {exists: false} if empty or doesn't exist and {exists: true} if getUser returns an object
Complete the handler for POST /user
get userid, password and name from request body
validate that userid, password and name exist and aren't empty strings
validate lengths
userid can't be > 25
password can't be > 100
name can't be > 100
validate userid doesn't already exist in getUser
on any error redirect to registration page with appropriate error message
otherwise...
generate a salt using current timestamp
generate passwordHash using salt and password
create user object containing userid, name, salt and passwordHash
DO NOT SAVE PASSWORD IN USER OBJECT
save user object
redirect user to login page with errorMessage.REGISTRATION_SUCCESS message
in documentRoot/login/index.html (15 pts)
create a form that
POST to ../dynamic/user/auth
has 2 inputs named userid and password but labelled as username and password
submit button
documentRoot/registration/index.js (30 pts)
validate function
get values from the elements username, password, confirm_password and name
do validation to make sure each is not empty
that username is not > 25, password and name are not > 100
password matches confirm_password
and password is at least scoring a 2 or higher from zxcvbn
if any validation fails, use showError and the correct errorMessages key and return false
otherwise showError(false) to clear errors and return true
onUsernameChange function
get the value of the username field and use axios to send a request to '../dynamic/user/exists?userid='+value
use the response data's exists key to call setUsernameStatus
onPasswordChange function
get the value of the password field and use zxcvbn to determine the score
call setScore with the score returned from zxcvbn
