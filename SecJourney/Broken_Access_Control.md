# Broken Access Control
Unlike authentication which only "proves" a user is who they say they are, authorization is what defines what a user can and cannot access. Access controls are the methods in place to prevent a user from accessing something that they are not authorized to access.

An example of this is an ATM. Your card and pin are used to authenticate you to the bank. You are authorized to access your account. There are methods in place to control your access to only your account.

A broken access control vulnerability is a violation of what you are authorized to do. For example, access other users' accounts or transfer funds from other accounts.

Broken access control is when the application does not ensure that the user is authenticated and is authorized to access data or perform a function.

## Reconnaissance
First, make sure `Intercept Requests` is disabled. We do not need it for this lesson.

In the sandbox browser, try going to the URL:

Copy 
```
https://app.sb.my.securityjourney.com/account/1
```
Ensure that you do not have a trailing slash (`/`) in the URL.

What happens?

## Reconnaissance
It happens quickly, but it appears that there is a redirect to the signin page. It looks like this page is live, but we are not authorized to view it.

Log in using username: `alice` and password: `monkey1`. You will be redirected to the account page for Alice.

Looking at the URL, it is of the form `/account/#`. In the last step you tried `account/1` which showed that we need to be authorized in order to view that route.

Can you think of a way to try to access another account?

## Exploitation
What happens if you try changing the number in the URL?

For example try:

Copy 
```
https://app.sb.my.securityjourney.com/account/17
```
Ensure that you do not have a trailing slash (`/`) in the URL.

It looks like you can steal other users information because of this broken access control. Are there other accounts you can access?

AT&T had a famous case with a broken access control vulnerability. Users signed up with their email addresses for the brand new iPad and their email was indexed by an ICC-ID number. Two security researchers wrote a program to iterate through ICC-ID numbers using GET requests and they obtained 120,000 email addresses. These included those of famous iPad early adopters (New York Mayor Michael Bloomberg, the White House Chief of Staff Rahm Emanuel, anchorwoman Diane Sawyer, New York Times CEO Janet Robinson and many others).

## Defense
To protect against broken access control vulnerabilities check whether a user is authorized to access data or perform a function every time it is requested.

Also, use indirect object references so attackers do not know how to access certain resources. A direct object reference is a file name, account number, etc. An indirect object reference is a random number that references an object and specifically, UUIDs are great to use. A universally unique identifier (UUID) is a random number that defined in an industry standard. For example, instead of using a database key directly that a user can view, use a large random number (e.g. UUID) and map that to the database key.

Before returning account information it is important to check if the user is allowed to access the requesting data. In the case of account balance information the user should only be able to access their own account balance.

## Patch the Vulnerabilities
Click on **Code Editor** to the right of Proxy History to open the Code Editor, select your programming language, and fix the Broken Access Control vulnerability.

Alice should only be able to see their own account information. The code needs to check to make sure that Alice is the user and it is Alice's account information before sending the data to the user.

Each language has a JSON Web Token (JWT) library installed that is used. The JWT that is passed to the function includes a username field (or claim) of the authenticated user. This can be used to compare with the account that is being looked up in the database via the number in the URL. Review the given code to see how JWT claims are accessed.

If the `username` in the JWT does not match the username returned from the database, raise an exception/error with the following message: `You do not have access`.

Once you have edited the code successfully, hit the **Save Code** button. This will update the running application.

Try exploiting the vulnerability again by trying to access Bob's account (17) to ensure you fixed it correctly.

## Show Fixing Broken Access Control
What is a JWT and JWT Authentication?
A JSON Web Token (JWT) is a simple yet efficient way to transfer information between two parties securely. Information in the JWT is digitally-signed which means every trusted party that can handle a JWT can tell whether or not the token has been changed in any way.

If you are familiar with OAuth2 and OpenID frameworks, JWT is used to represent all kinds of tokens: Access tokens, Identify tokens, etc. However, many people confuse JWT with Oauth2 which is an authorization framework that allows applications to obtain access to data. Oauth uses JWT to represent tokens, but that is all they have in common.

JSON Web Token (JWT) is a JSON-based open standard for creating access tokens that assert some number of claims. For example, a server could generate a token that has the claim "logged in as admin" and provide that to a client. The client could use that token to prove that it is logged in as admin.

The tokens are signed by one party's key (usually the server's), so that both parties are able to verify that the token is legitimate. JWT libraries can help verify token signatures either with a shared secret for symmetric signature algorithms or with public keys for asymmetric signature algorithms.

Here is an example of a JWT:

Copy 
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTYiLCJuYW1lIjoiaGFja2VkdV9hZG1pbiIsIml0ZW1zIjpbMSw1XX0.BW5r9bh4ZNfLL01uQkcaT8xPvSDB4ywdh1MuE7cOvyI
```
Each JWT is composed of three parts encoded with base64url. The header, payload, and signature.

The header component, which in the above example is `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`, contains information related to the signing algorithm used and usually includes the type of token. Once decoded it has the following format:

Copy 
```
{
 "alg": "HS256",
 "typ": "JWT"
}
```
This JSON object contains two claims: `alg` which describes the cryptographic algorithm to be used to sign the JWT and `typ` which specifies the token type. Claims are statements about an entity -typically, the user- and in this context the claims are actually JSON object keys.

The payload component is also a JSON object when decoded and it stores any information about the user which is relevant to the web application. It is the part in between the two dots. A decoded example is:

Copy 
```
{
 "sub": "123456",
 "name": "hackedu_admin",
 "items": [1,5]
}
```
Here we have three claims, but we can add as many as we want depending on the requirements of the application. Also, several pre-defined claims can be used such as:

`sub` - specifies the Subject (whom the token refers to)

`iss` - specifies the Issuer (who created and signed this token)

`aud` - specifies the Audience (who or what the token is intended for)

The last part of a JWT is the signature and is separated from the payload by a dot. Its purpose is to allow one or more parties to establish the authenticity of the JWT. The server can quickly validate the integrity of the token (i.e. who issued the token and identify whether the token has been modified or not during the transfer). The signature is created based on the following steps:

1. The header component is encoded using base64URL (a variation of the base64 algorithm that transforms `+` and `/` symbols into `_` so they can be carried into HTTP requests without altering the request)
2. The payload component is encoded using base64URL
3. The encoded header and payload are joined together and delimited by a dot
4. The resulting string is signed with the secret key using the algorithm specified in the JWT header
5. The hashed data is also encoded using base64URL and the resulting string represents the signature

jwt.io is a great site to manually decode JWTs, verify signatures, and generate JWTs.

You can see the JWT example above decoded on jwt.io:

## Fig. 1 - The encoded and decoded version of our JWT

JWT can be used outside of authentication as well. We may want to use JWT to transfer information but without signing the token with a hashing algorithm. To achieve this, we can set the `alg` claim on the header component to `none`:

Copy 
```
{
 "alg": "none"
}
```
Since the token will be created without a signature or encryption, the last component will be missing and the JWT will end with a dot. Here is an example:

Copy 
```
eyJhbGciOiJub25lIn0.eyJzdWIiOiIxMjM0NTYiLCJuYW1lIjoiaGFja2VkdV9hZG1pbiIsIml0ZW1zIjpbMSw1XX0.
```
The following figure describes the way this token has been created so you can get a better understanding of the whole process. Notice that although the signature component is missing, the delimiter symbol (`.`) is still appended at the end of the string.

## Fig. 2 - Serialization process for an insecure token

Since JWT content is encoded with base64 it means anyone can decode and read it. Therefore, signing can help you verify that the JWT was created by someone you trust, but it does not ensure the confidentiality of the data. Fortunately, JWT provides a method to encrypt the header and payload components making the content unreadable to third parties and ensuring confidentiality.

For even more detail about JWTs, please visit our JWT Lesson.

Please complete the coding exercise to finish this lesson

## Solution:
```
import jwt
import pymysql


class LoggedOutException(Exception):
    """Raising a LoggedOutException will redirect the user to the login screen
    in the app.
    """
    pass

def account_lookup(account_id, jwt_token):
    try:
        token = jwt.decode(jwt_token, "luBEK(P$x[%ZeQ4HAD5Ji1Z*;0Gcz583yP!v|KCmNEDDmQF/9P)>GpJK>Cx}3;R", algorithms="HS256")
    except Exception as e:
        raise LoggedOutException("User is not logged in")

    if "logged_in" in token.keys() and token["logged_in"] == True:
        conn = pymysql.connect(
            host="mysql",
            port=3306,
            user="root",
            passwd="letmein",
            db="BankApp"
        )
        cursor = conn.cursor()

        statement = "SELECT username FROM tbl_user WHERE id = " + account_id + ";"
        cursor.execute(statement)
        username_results = cursor.fetchone()

        if username_results and token["username"] == username_results[0]:
            username = username_results[0]

            statement = "SELECT balance, dob FROM tbl_account WHERE user_id = " + account_id + ";"
            cursor.execute(statement)
            account_results = cursor.fetchone()

            conn.commit()
            cursor.close()
            conn.close()

            return {
                "balance": account_results[0],
                "dob": account_results[1],
                "username": username
            }
        else:
            raise Exception("Account not found")
    else:
        raise LoggedOutException("User is not logged in")
```
