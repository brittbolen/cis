---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: WAF Custom Rules, Custom WAF Rules

subcollection: cis

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:generic: data-hd-programlang="generic"}


# WAF Custom Rules
{:#waf-custom-rules}

Navigate to Custom WAF Rules through **Security > Web Application Firewall > Custom rules**. WAF Rules are available on Enterprise plans and are created based on that customer's unique requirements and/or their website's traffic patterns. This means that you can ask us to block virtually any combination of characteristics of a request.
{: shortdesc}

Custom WAF Rules cater to situations where the IBM WAF doesn’t have a rule in place already, and the attacker uses a specific pattern or user agent that is targeted specifically for your website's structure. In these situations, you can create a custom rule for your web property.

## Request a Custom Rule
{:#request-a-custom-rule}

To request a rule, email wafsup@us.ibm.com with rule requirements or traffic patterns.

Provide as much information as possible, including:
* Access logs showing a sample of about 100 alleged malicious requests.
* A sample POST request including the POST data (if the requests have a POST body) to determine whether the request contains anything we can block or trigger on.
* Whether you want the requests to be outright blocked, or to be challenged, in case of potential false positives.
* The specific domains to which you want these rules applied.
* Whether you want the rules turned on or off by default.

After your custom WAF rules are created, you must enable all custom rules for them to take effect.
{:note}
