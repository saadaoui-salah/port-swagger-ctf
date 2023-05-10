# XSS

# Lab: Reflected XSS into HTML context with nothing encoded

after searching for any key word the website will redirected to other url that contains the keyword in it and also it appears in the html code so simply we can search for:

    <script>alert(1)</script>

an alert will pop up in the page

# Lab: Stored XSS into HTML context with nothing encoded

in this lab see that when you go to post details you'll find form where you can submit your comment it will solved by commenting with:

    <script>alert(1)</script>

this payload will be stored as an comment each time when someone see post details he will recieve an alert bcz browser read this comment as html code
