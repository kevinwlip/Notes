# Introduction to Command Injection
Command injection is a critical security vulnerability where an attacker can execute arbitrary commands on a system by manipulating input fields in an application.

In this lesson, we will examine a command injection vulnerability, understand how attackers can exploit it, and explore effective strategies to mitigate it. By the end, you will learn how to protect your applications through proper input validation, sanitization, and secure coding practices.

## Interrogate
In the **Sandbox** tab, we will find a simple application that allows users to `ping` hosts. The user is expected to input a hostname, and the application returns the results of the ping command.

Let's begin by trying to ping our application server using the hostname 'app' and see the response:

Copy 
```
PING app (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=127 time=0.012 ms

--- app ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.012/0.012/0.012/0.000 ms
```
If we are familiar with Linux, you will recognize this as the standard output from the ping command. This suggests that the application executes the ping command and directly returns its output. This behavior could introduce a command injection vulnerability without proper safeguards, potentially allowing attackers to execute arbitrary commands on the system.

Let's test this by crafting a hostname that includes additional Linux commands. We will start with a simple example:

Copy 
```
app && echo hello, world
```
Upon examining the response, you should see hello, world included in the output. This indicates that the system executed the additional echo command.

Next, let's try a more harmful injection to display the system's passwd file:

Copy 
```
app && cat /etc/passwd
```
As demonstrated, the contents of the passwd file are returned. Depending on the user's privileges running the commands, this vulnerability could be exploited to execute more dangerous commands, posing a significant risk to the system.

## Mitigation -- Input Sanitization
Using the Code Editor let's look at the code. You can see that the function passes a hostname parameter, which is used in the ping command.

For the security-minded, this should be an immediate red flag. Since the hostname is coming from a user, we have no reason to trust this input. When accepting uncontrolled input, we should always validate or sanitize it.

For our Ping application, we are expecting a hostname. Since hostnames have a standard, we can use this standard to determine if the input passed is in a valid format for a hostname. Let's look at a couple of ways to validate the hostname.

One option for validating a hostname is using Regex. Regex is a powerful tool for matching input to a predefined pattern. Since we know the standard for a hostname, we can craft a regex that will validate if the hostname argument confirms the hostname standard. For example, here is one that should work:

Copy 
```
^[A-Za-z0-9]([A-Za-z0-9-]{0,61}[A-Za-z0-9])?(\.[A-Za-z]{2,})?$|^[A-Za-z0-9]{1,63}$
```
We can use this pattern to check whether the hostname is valid before attempting to execute the ping command.

Note: As you may know, ping accepts IP addresses in addition to hostnames. For this exercise, we will ignore validating IP addresses.

## Defense — Validation with Regex
The following are examples of using regex:

C++
Go
Java
JavaScript
Python
TypeScript

## Defense — Validation Using Library Functions
Many programming languages offer built-in or third-party libraries to validate hostnames and IP addresses, reducing the need to implement custom validation logic. These libraries ensure the validation process adheres to established standards and helps prevent common errors.

For instance, the javascript validator library includes a straightforward method for validating hostnames:

Copy 
```
const validator = require('validator');
validator.isFQDN(hostname, { require_tld: false })
```
Utilizing a library function for validation simplifies the code, reduces the potential for mistakes, and ensures compliance with relevant standards. This approach offers a more reliable alternative to writing complex regular expressions and results in cleaner and more maintainable code.

C++
Go
Java
JavaScript
Python
TypeScript

## Defense — Direct Command Execution
If you have successfully implemented the regex filtering, your solution should pass all vulnerability checks.

But you might have noticed some code that could be improved.

For instance, consider the Go code snippet:

Copy 
```
command_str := fmt.Sprintf("ping -c 1 %s", hostname)
command := exec.Command("/bin/sh", "-c", command_str)
```
Here, the code constructs a command string with the ping command and then executes it by passing the command to the shell (/bin/sh). The problem with this approach is that shells, such as bash, allow for chaining commands together, which opens the door to command injection attacks.

But why involve the shell in the first place? A safer approach is to bypass the shell entirely and execute the command directly:

Copy 
```
command := exec.Command("ping", "-c", "1", hostname)
```
In this case, the hostname is passed directly to the ping command as an argument. If the hostname is invalid, the ping command will return an error, significantly reducing the risk of command injection by avoiding the shell altogether. Direct execution promotes cleaner, more secure code by eliminating unnecessary reliance on shell features.

C++
Go
Java
JavaScript
Python
TypeScript

## Completing the Exercise
Which method should you choose? While a single mitigation strategy may be sufficient to pass the tests, implementing multiple layers of protection is generally considered a best practice. A defense-in-depth approach ensures better security, reducing the chances of exploitation.

Implementing any of the strategies mentioned will be enough to meet the requirements for this exercise. However, layering different mitigation techniques is often the preferred approach to ensuring more robust security in real-world scenarios.

## Solution:
```
import subprocess
import re

def ping(hostname):
    try:
        hostname_regex = r'^(?!-)[A-Za-z0-9-]{1,63}(?<!-)(\.[A-Za-z]{2,})?$|^[A-Za-z0-9-]{1,63}$'
        if re.match(hostname_regex, hostname) is None:
            return f"Invalid Hostname: {hostname}"

        command = f"ping -c 1 {hostname}"
        result = subprocess.check_output(command, shell=True, universal_newlines=True, stderr=subprocess.STDOUT,)
        return result
    except subprocess.CalledProcessError as e:
        return f"Error: {e.output}"
```
