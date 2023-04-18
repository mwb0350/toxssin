# toxssin  [【中文】] (https://github.com/mwb0350/toxssin/blob/d3f0b513c344f9878fa490a5511d4d9f44368fc7/%E3%80%90%E7%A5%9E%E5%85%B5%E5%88%A9%E5%99%A8%E3%80%91XSS%E6%BC%8F%E6%B4%9E%E6%89%AB%E6%8F%8F%E5%88%A9%E7%94%A8%E5%B7%A5%E5%85%B7%20--%20toxssin.md)

[![Python 3.x](https://img.shields.io/badge/python-3.x-yellow.svg)](https://www.python.org/) <img src="https://img.shields.io/badge/vanilla-JavaScript-blue"> [![License](https://img.shields.io/badge/license-MIT-red.svg)](https://github.com/t3l3machus/toxssin/blob/main/LICENSE) [![Linux](https://svgshare.com/i/Zhy.svg)](https://svgshare.com/i/Zhy.svg)
<img src="https://img.shields.io/badge/Maintained%3F-Yes-CD8335">
## Purpose

toxssin is an open-source penetration testing tool that automates the process of exploiting Cross-Site Scripting (XSS) vulnerabilities. It consists of an https server that works as an interpreter for the traffic generated by the malicious JavaScript payload that powers this tool (toxin.js).  

This project started as (and still is) a research-based creative endeavor to explore the exploitability depth that an XSS vulnerability may introduce by using vanilla JavaScript, trusted certificates and cheap tricks.

**Disclaimer**: The project is quite fresh and has not been widely tested.  
## 免责声明

本文中涉及到的工具、代码和软件应用仅面向合法授权的企业安全建设行为，如您需要测试本工具的可用性，请自行搭建靶机环境。
在使用本工具进行检测时，您应确保该行为符合当地的法律法规，并且已经取得了足够的授权。请勿对非授权目标进行扫描。
如您在使用本工具的过程中存在任何非法行为，您需自行承担相应后果，我们将不承担任何法律及连带责任。

### Video Presentation  
https://www.youtube.com/watch?v=Z9I4UJUBrrY



## Screenshots
![usage_example_png](https://raw.github.com/t3l3machus/toxssin/master/Screenshots/toxssin-1.png)
  
Find more screenshots [here](Screenshots/).

## Capabilities  
By default, toxssin’s JavaScript poison automatically spreads across the elements and information of a webpage, abusing the XMLHttpRequest object to intercept:
- cookies (if HttpOnly not present),
- keystrokes (technically, an active keylogger),
- paste events,
- input change events,
- file selections,
- form submissions,
- server responses (to form submissions or clicking hyperlinks that target different pages and not internal parts of the same page),
- table data (static as well as updates on tables after a page has finished loading),

Most importantly, toxssin:
- attempts to create XSS persistence while the user browses the website by intercepting http requests & responses and re-writing the document, creating the illusion of navigating when actually the document’s location never changes,
- supports session management (you can use it to exploit multiple targets at the same time e.g., by running an XSS-based phishing campaign or exploiting stored XSS),
- supports custom JS script execution against sessions (after a browser gets hooked, you can run custom JS scripts against it),
- automatically logs every session.

## Installation & Usage
```
git clone https://github.com/t3l3machus/toxssin
cd ./toxssin
pip3 install -r requirements.txt
```  
To start toxssin.py, you will need to supply ssl certificate and private key files.

If you don't own a domain with a trusted certificate, you can issue and use self-signed certificates with the following command (although this won't take you far):  
```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365
```

It is strongly recommended to run toxssin with a trusted certificate (see [How to get a Valid Certificate](#How-to-get-a-Valid-Certificate) in this document). That said, you can start the toxssin server like this:
```
# python3 toxssin.py -u https://your.domain.com -c /your/certificate.pem -k /your/privkey.pem
```
Visit the project's [wiki](https://github.com/t3l3machus/toxssin/wiki) for additional information.

## XSS Exploitation Obstacles
In my experience, there are 4 major obstacles when it comes to Cross-Site Scripting attacks attempting to include external JS scripts:
1. the "Mixed Content" error, which can be resolved by serving the JavaScript payload via https (even with a self-signed certificate).
2. the "NET::ERR_CERT_AUTHORITY_INVALID" error, which indicates that the server's certificate is untrusted / expired and can be bypassed by using a certificate issued by a trusted Authority.
3. Cross-origin resource sharing (CORS), which is handled appropriately by the toxssin server.
4. `Content-Security-Policy` header with the `script-src` set to specific domain(s) only will block scripts with cross-domain src from loading. Toxssin relies on the `eval()` function to deliver its poison, so, if the website has a CSP and the `unsafe-eval` source expression is not specified in the `script-src` directive, the attack will most likely fail (i'm working on a second poison delivery method to work around this). 

**Note**: The "Mixed Content" error can of course occur when the target website is hosted via http and the JavaScript payload via https. This limits the scope of toxssin to https only webistes, as (by default) toxssin is started with ssl only.


## How to get a Trusted Certificate
First, you need to own a domain name. 
### Register a domain for free
You can search for free options on [freenom](https://my.freenom.com). It's a bit tricky to do it correctly. I suggest you follow this instructional [video](https://www.youtube.com/watch?v=3Uopc4AFjOY&t=324s). Also, if you create an account for the first time, make sure the Country you select matches your IP address or you might get errors. 

### Standard (paid) method
Purchase a domain from a registrar service (e.g.  https://www.namecheap.com/). The most economic way is to search for a random string domain name (e.g. "fvcm98duf") and check the less popular TLDs, like .xyz, as they will probably cost around 3$ per year.

### Get a trusted certificate
After you purchase a domain name, you can use certbot (Let's Encrypt) to get a trusted certificate in 5 minutes or less:
1. Append an A record to your Domain's DNS settings so that it points to your server ip,
2. Follow certbots [official instructions](https://certbot.eff.org/instructions).  

**Tip**: Don't install and run certbot on your own, you might get unexpected errors. Stick with the instructions.

## Changelog
`2022-06-19` - Added the **exec** prompt command (you can now execute custom JS scripts against a session).  
`2022-06-23` - I added two simple, dirty scripts as templates for testing the **exec** prompt command. I also fixed the cmd prompt's backward history access and made some improvements.
## Future 
The idea is to make it sharper, more reliable and expand its capabilities. Currently, i'm working on improving file captures.
