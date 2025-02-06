# Reflected Cross-Site Scripting (XSS)

In this lesson we will look at how to exploit and remediate a Reflected Cross-Site Scripting vulnerability. A Reflected Cross-Site Scripting vulnerability occurs when data from a user's request is returned in a page context where it is treated as code and executed in the user's browser.

Cross-Site Scripting is a type of code injection vulnerability, where data is accidentally treated as code by the browser. To exploit it, we have to find a part of the application that will reflect user input into the HTML output. To fix a Cross-Site Scripting vulnerability you must find the location in the code where the attacker controlled data is being mixed with the HTML, determine the context of that portion of the HTML, and apply the proper defense for that HTML context.

Security folks will often refer to the three main types of Cross-Site Scripting as: Reflected, Stored and DOM Based. Another way to label these vulnerabilities is using the location where the data is mixed with code, which would give us Client Cross-Site Scripting or Server Cross-Site Scripting. The vulnerability in this application is a Reflected Server Cross-Site Scripting vulnerability because the attacker controlled data will be mixed with the HTML on the server before sending it to the browser.

Let's take a look at how we can exploit the security bug in this application.

## Attack Surface
To exploit this Reflected Cross-Site Scripting vulnerability we need to find a piece of data from the request that is also included in the output HTML. Some common locations for this type of data are: query parameters, form data, cookies, URL paths, and various HTTP headers.

Attempt to login to the application using any random user name and see if you can find any of your input reflected back in the HTML.

## Exploit
You may have noticed that the when you fill out the `Username` and `Password` and click the `Sign in` button an HTTP POST request is sent to the application. The data you input in the form is sent with the content type `application/x-www-form-urlencoded` and the body of the request contains the string `username=this&password=that`. You can view the request by toggling Intercept Requests on and submitting the form.

This application is vulnerable to Reflected Cross-Site Scripting because the username data can be found in the response HTML.

Copy 
```
<p class=error style="color:red;"><strong>Error:</strong> No username: this
```
You can see the request and response data over in the Proxy History tab.

If you modify the `Username` value and include HTML that will execute a script, like `<img src=x onerror=console.log('hacked') />` , you should see that message in the browser's JavaScript console.

For an attacker to use this exploit they would need to convince a user to login with a payload the attacker can control for the `Username`. It could be difficult for an attacker to convince a user to put an attack payload in the form field and submit it, but a more common tactic is to share the payload via an attacker controlled URL that includes the payload in the URL query parameters.

Try to exploit the vulnerability using URL query parameters to submit the form data.

NOTE: For help viewing your browser's console, follow this link

## Weaponize
Try the following URL and notice the exploit also writes a message `hacked` to the JavaScript console.

Copy 
```
https://app.sb.my.securityjourney.com/signin?username=%3Cimg+src%3Dx+onerror%3Dconsole.log(%27hacked%27)+%2F%3E&password=a
```
Using this exploit an attacker could execute any JavaScript they choose in the user's browser, if they can get them to click on the malicious link. For example, the attacker might share a link with the target user where the JavaScript they include in the exploit URL could add an event listener to the login form, sending all keystrokes to an attacker controlled collection endpoint on the internet. Allowing the attacker to steal the user's login details while the page would behave normally for the user and they would not notice the attack occurring.

Let's look at how we can remediate this vulnerability.

## Remediation is Contextual
To remediate a Reflected Cross-Site Scripting vulnerability you must find the context where the attacker controlled data is being mixed with the output HTML.

Some HTML contexts should never include attacker controlled data, others can safely contain attacker controlled data if the proper defensive technique is used to prevent that data from being treated as code by the browser.

You must never put attacker controlled data into the the following locations: script text content, style text content, comments, tag names, attribute names, unquoted attribute values, or any nested combination of these contexts.

Copy 
```
<script> ... here ... </script>
<style> ... here ... </style>
<!-- ... here ... -->
< here  ... />
<div ...here...=value />
<div key=...here... />
<script> document.createElement(...here...) </script>
```
The only two safe locations for attacker controlled data in an HTML document are in an HTML elements text content or in a quoted attribute value. Both of these contexts require defensive code to make sure the attacker controlled data can not break out of those contexts or execute code using special attribute values.

Let's look at how we can fix this particular Reflected Cross-Site Scripting vulnerability.

## Remediation with Contextual Encoding HTML Entities
This application is vulnerable because the attacker controlled data in the `Username` is added to the HTML document, in an HTML elements text content, without any contextual encoding.

Copy 
```
<p class="error" style="color:red;">
  <strong>Error:</strong> No username: 
  ... attacker controlled Username data from the request ... 
</p>
```
An attacker can simply define a new HTML tag with a malicious script in the request data and that new HTML will be added to the output HTML.

Copy 
```
<p class="error" style="color:red;">
  <strong>Error:</strong> No username: 
  <img src="x" onerror="console.log('hacked')"> <- This is attacker controlled.
</p>
```
To prevent this vulnerability we need to apply contextual output encoding, using HTML entity encoding. By encoding the following characters with their HTML entity equivalent characters the browser will no longer treat the attackers data as code, and instead render it as benign text.

Copy 
```
<p class="error" style="color:red;">
  <strong>Error:</strong> No username: 
  &lt;img src=x onerror=console.log('hacked') /&gt; <- This is attack controlled.
</p>
```
Notice that the characters `<` and `>` have been replaced with their HTML entity equivalents. To protect against Cross-Site Scripting in the HTML text content it is best to escape all of the following characters: `<`, `>`, `'`, `"`, and `&`. It is often sufficient to only escape `<`, but to protect against some attacks the other characters should also be entity encoded.

## Remediate the Code
Attempt to remediate the code by adding contextual output encoding using an HTML encoding library in your language of choice. Be aware that many libraries use the term escaping to refer HTML entity encoding.

## Verify Remediation
Now that you have remediated the Reflected Cross-Site Scripting vulnerability, try to exploit it using the URL from earlier in the lesson and notice that the attack controlled data doesn't execute any code in your browser.

Copy 
```
https://app.sb.my.securityjourney.com/signin?username=%3Cimg+src%3Dx+onerror%3Dconsole.log(%27hacked%27)+%2F%3E&password=a
```
Check the browser console and inspect the element to verify that the data from URL didn't get treated as code.

Did your remediation code work? If so, great job!

If you are interested in learning more about other types of Cross-Site Scripting vulnerabilities take a look at our Stored Cross-Site Scripting and DOM Based Cross-Site Scripting lessons.

## Solution:
```
import html

def encode_for_html(error_message):
    error_message = html.escape(error_message)
    return error_message
```
