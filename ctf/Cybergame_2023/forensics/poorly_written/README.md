# Poorly written
## Description
Your business runs an application that has been attacked and compromised. You were given the task of finding out what actually happened. https://drive.google.com/file/d/1-7m5aO4B5JWz79xhNeTnOvt2m_pS4bjc/view?usp=share_link

## Task #1: Compromise
First we need to find the time of compromise. Let's look at http logs `access.log`. Attacker first tries to change the password. Once he does, he uploads a malicious file with `POST /api/upload`.

```
198.19.243.43 - - [26/Feb/2023:12:35:00 +0000] "GET /api/auth/reset?email=administrator@victim.com&token=0&password=newpassword1234 HTTP/1.1" 403 257 "-" "curl/7.86.0"
198.19.243.43 - - [26/Feb/2023:12:35:00 +0000] "POST /api/auth/forgot HTTP/1.1" 200 284 "-" "curl/7.86.0"
198.19.243.43 - - [26/Feb/2023:12:35:00 +0000] "GET /api/auth/reset?email=administrator@victim.com&token=0&password=newpassword1234 HTTP/1.1" 200 285 "-" "curl/7.86.0"
198.19.243.43 - - [26/Feb/2023:12:35:00 +0000] "POST /api/auth/login HTTP/1.1" 200 1324 "-" "curl/7.86.0"
198.19.243.43 - - [26/Feb/2023:12:35:01 +0000] "POST /api/upload HTTP/1.1" 200 264 "-" "curl/7.86.0"
```

It's the last line of the log
### Flag #1
**2023-02-26 12:35:01**

## Task #2: What went wrong
There is a problem with token generation. We drill down to `./laravelapp/app/Http/Controllers/AuthController.php`

There is a method `forgot()` that generates new tokens. It looks random but in the end it just generates an 8 char alphanum "pincode" as a token that an attacker can easily bruteforce.

```
$token = $request["email"].$this->generateRandomString();
$hashed = substr(md5($token), 0, 8);
```

### Flag #2
**AuthController.php:45**

## Task #3: It worked?
I honestly just guessed this one as a type confussion attack. Instead of using `===` they just used `==` which doesn't account for data type of variables.

```
if ($user->token == $request['token']) {
```

### Flag #3
**AuthController.php:58**

## Task #4: Technique
Once attacker logged in, he uploaded a file using `./laravelapp/app/Http/Controllers/UploadFileController.php`.

### Flag #4
**UploadFileController.php:upload**

## Task #5: Escalation
For root escalation, attacker used `log_stats.py`. It takes an argument `log_type` which is directly passed into a shell command without sanitization basically allowing attacker to run any command as root.

```
result = subprocess.check_output('grep "'+log_type+'" '+log+' 2>/dev/null', shell=True, text=True)
```

### Flag #5
**log_type:31**