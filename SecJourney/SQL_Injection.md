# SQL Injection - CWE-89
SQL Injection remains one of the oldest cataloged vulnerabilities, yet it continues to be a significant threat in the digital landscape of the 21st century.

In this lesson, you will be tasked with identifying and rectifying an SQL Injection vulnerability within an e-commerce application.

## Reconnaissance
Let's see if we can discover candidate features for a SQL Injection attack.

In the Sandbox tab:

Select a store
Add a product to your shopping cart
You'll be presented with a typical shopping cart page.

You may notice that the shopping cart page allows customers to input a coupon code. Since this entry is text it might be a candidate for a SQL Injection attack.

Let's input a character that is part of the SQL language, such as the single quote character '.

What are the results?

You should see an error message that indicates a SQL syntax error. Let's see if we can exploit the vulnerability.

## Exploit
When exploiting a SQL Injection attack, you need to think of what the underlying SQL statement might look like. A SQL statement to look up a coupon code might look like:

Copy 
```
SELECT * FROM coupons WHERE code = 'CODE';
```
The system could be using string concatenation to dynamically create the SQL statement such as:

Copy 
```
sql_statement = "SELECT * FROM coupons WHERE code = '" + CODE + "'";
```
If we have the ability to inject SQL into the entered code, we might try something like this:

Copy 
```
a' OR 'x' = 'x
```
This would result in the SQL statement:

Copy 
```
SELECT * FROM coupons WHERE code = a' OR 'x' = 'x'
```
Let's try a' OR 'x' = 'x and see the results.

As you can see a coupon code was applied successfully to our order.

But can we do better?

## Exploit: Part 2
A $10 discount is great—but could we do better? Is there a way we could make sure the maximum discount is applied?

If we could sort the SQL results based on the discount amount (descending) we might be able to find a bigger discount.

Let's try this for the coupon code:

Copy 
```
a' OR 1 = 1 ORDER BY discount DESC; -- 
```
Note: you need to have a space after the --.

What are the results?

As you can see you found a test coupon code for $1000 which made the total negative. The store owes you money!

In the next section will we eliminate the vulnerability.

## Defense
Open the Code Editor tab. You should recognize that the code is generating the SQL statement using string concatenation. We generate the SQL statement while protecting against SQL Injection.

Using parameterized queries statements is a common technique. Update the code to use a parameterized query.

## Solution:
```
import pymysql

def getConnection():
    conn = pymysql.connect(host="db", port=3306, user="root", passwd="letmein", db="EcommerceApp")
    return conn

def getCouponCode(coupon_code):
    try:
        conn = getConnection()
        cursor = conn.cursor()
        cursor.execute("SELECT code, discount FROM coupon_code WHERE code = %s", (coupon_code))
        row = cursor.fetchone()
        conn.commit()

        if row:
            return {"success": True, "code": str(row[0]), "discount" : int(row[1]), "message": f"Coupon code {str(row[0])} successfully applied."}
        else:
            return {"success": False, "code" : coupon_code, "discount" : 0, "message": "Invalid coupon code: " + coupon_code}
    except Exception as e:
        return {"success": False, "message": "Internal Error Occurred"}
```
