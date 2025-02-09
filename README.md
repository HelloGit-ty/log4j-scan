<h1 align="center">log4j-scan</h1>
<h4 align="center">A fully automated, accurate, and extensive scanner for finding vulnerable log4j hosts</h4>


# Features

- Support for lists of URLs.
- Fuzzing for more than 60 HTTP request headers (not only 3-4 headers as previously seen tools).
- Fuzzing for HTTP POST Data parameters.
- Fuzzing for JSON data parameters.
- Supports DNS callback for vulnerability discovery and validation.
- WAF Bypass payloads.

# Description

I have been researching the Log4J RCE (CVE-2021-44228) since it was released, and I worked in preventing this vulnerability with oTHER OPEN-SOURCE contributors. I am open-sourcing an open detection and scanning tool for discovering and fuzzing for Log4J RCE CVE-2021-44228 vulnerability. This shall be used by security teams to scan their infrastructure for Log4J RCE, and also test for WAF bypasses that can result in achiving code execution on the organization's environment.

It supports DNS OOB callbacks out of the box, there is no need to setup a DNS callback server.





# Usage

```python
$ python3 log4j-scan.py -h
[•] CVE-2021-44228 - Apache Log4j RCE Scanner
usage: log4j-scan.py [-h] [-u URL] [-l USEDLIST] [--request-type REQUEST_TYPE] [--headers-file HEADERS_FILE] [--run-all-tests] [--exclude-user-agent-fuzzing]
                     [--wait-time WAIT_TIME] [--waf-bypass] [--dns-callback-provider DNS_CALLBACK_PROVIDER] [--custom-dns-callback-host CUSTOM_DNS_CALLBACK_HOST]

optional arguments:
  -h, --help            show this help message and exit
  -u URL, --url URL     Check a single URL.
  -p PROXY, --proxy PROXY
                        Send requests through proxy. proxy should be specified in the format supported by requests
                        (http[s]://<proxy-ip>:<proxy-port>)
  -l USEDLIST, --list USEDLIST
                        Check a list of URLs.
  --request-type REQUEST_TYPE
                        Request Type: (get, post) - [Default: get].
  --headers-file HEADERS_FILE
                        Headers fuzzing list - [default: headers.txt].
  --run-all-tests       Run all available tests on each URL.
  --exclude-user-agent-fuzzing
                        Exclude User-Agent header from fuzzing - useful to bypass weak checks on User-Agents.
  --wait-time WAIT_TIME
                        Wait time after all URLs are processed (in seconds) - [Default: 5].
  --waf-bypass          Extend scans with WAF bypass payloads.
  --test-CVE-2021-45046
                        Test using payloads for CVE-2021-45046 (detection payloads).
  --custom-waf-bypass   Use your own custom payload.
  --dns-callback-provider DNS_CALLBACK_PROVIDER
                        DNS Callback provider (Options: dnslog.cn, interact.sh) - [Default: interact.sh].
  --custom-dns-callback-host CUSTOM_DNS_CALLBACK_HOST
                        Custom DNS Callback Host.
  --disable-http-redirects
                        Disable HTTP redirects. Note: HTTP redirects are useful as it allows the payloads to have higher chance of reaching vulnerable systems.
```

## Scan a Single URL

```shell
$ python3 log4j-scan.py -u https://log4j.lab.secbot.local
```

## Scan a Single URL using all Request Methods: GET, POST (url-encoded form), POST (JSON body)


```shell
$ python3 log4j-scan.py -u https://log4j.lab.secbot.local --run-all-tests
```

## Discover WAF bypasses on the environment.

```shell
$ python3 log4j-scan.py -u https://log4j.lab.secbot.local --waf-bypass
```

## Scan a list of URLs

```shell
$ python3 log4j-scan.py -l urls.txt
```

# Installation

```
$ pip3 install -r requirements.txt
```

# Docker Support

```shell
git clone https://github.com/fullhunt/log4j-scan.git
cd log4j-scan
sudo docker build -t log4j-scan .
sudo docker run -it --rm log4j-scan

# With URL list "urls.txt" in current directory
docker run -it --rm -v $PWD:/data log4j-scan -l /data/urls.txt
```


# License
The project is licensed under MIT License.
