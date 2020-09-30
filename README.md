# push-pop-coding-challenge
Coding Challenge for the SIDESTREAM GmbH

This was my first project working with frontend development. I have enjoyed and learned lot of new things by doing this project and thanks for oppertunity to work in this project.
Used Visual Studio Code for the frontend development.

Frontend:
The Error array manangement is mainly done in the Frontend except the intersection of the errors code section. For the intersection the array is passed over to API.
	- Vue good table : for table like display
	- Buttons : Request Error Change ,  Undo Buttons , Intersection display buttons 
	- Vue Notification for the notification display
	
Packages Used:
	vue-notification --> npm install vue-notification   #Module to handle the notification
	vue-good-table --> npm install vue-good-table 	  #Module to handle the table
	
Functions for managing these fuctionlaties:

	buttonclick() : This Function is called when a error request button is clicked. It handles removing a row and adding it into other error group. 
					Also informs the backend with the error details that this button is clicked. Post Axios request is used.
	undobutton() : This Function is for Undo Button. All the 3 Undo Buttons do the same task. Added 3 buttons for accessibility.
					Undo till the first action is handled and the Invalid Undo button press is also handled. Post Axios request is used.	
					
HideIntersection(): A Function that will erase the data about the intersection details. 

Intersection () and 
Intersection_display() : A Function that will call the API for the current intersection details by the current array from Frontend to API. 
						 Get Axios request is used.

API:
Added two additional packages:
	starlette.middleware.cors :  There were problems(No Access-Control-Allow-Origin) while using axios in connecting with the API. So inorder to overcome this issue used this module to get the access.
					     json :  Used to convert a string into Dictionary

Additional functions: get_error_request() ,  get_undo_request() :post request
	Two functions are added to log in the button press from the Frontend side and count the number of requests from the Frontend. These functions are called by axios post requests.


The Following requirements are done:

Implemented at the Frontend UI
Done:
		- Nice Overview of all Error in the order `unresolved`, then `resolved` and then `backlog` errors.
		- show index text and code of each errors.
		- Buttons to change the error context and hover over properties for the buttons.
		- Undo Button to undo the last done actions till the very first action. (All 3 Undo Buttons do the same job)
		- Intersection of Errors is displayed in the UI with a Button.
		- Added the notification view when a button is clicked.
	Sceeenshots for the UI are added into the screenshots folder
		
Implemented at the API
Done:
		- The intersection function to find the intersection between error classes.
		- Added two additional API calls for logging of the Error Change Request Button and the Undo Button.
		- Intersection function getting the current error list from the frontend and returns the intersection to the frontend.
	Sceeenshot for the LOGGER is added into the screenshots folder 
		

		

