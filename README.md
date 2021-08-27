# Tab napping

## More info:  
https://owasp.org/www-community/attacks/Reverse_Tabnabbing  
https://hackerone.com/reports/179568


## Desc

When you open a link in a new tab ( ```target="_blank"``` ), the page that opens in a new tab can access the initial tab and change it's location using the window.opener property. 

## Tab napping setup. 

You need 2 html forms.

1. A fished login form 
2. A form with redirect code in for the browser. 

## Example (hackthebox machine foothold used to test) (developers htb, hard machine)

1. Spoofed login.html 

```

```

2. Redirect index.html

```

```
