---

copyright:
  years: 2019
lastupdated: "2019-09-13"

keywords: IBM Cloud Internet Services, subdomain, CNAME

subcollection: cis

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:generic: data-hd-programlang="generic"}

# Getting {{site.data.keyword.cis_short_notm}} running with a new Subdomain

Follow these steps to "practice" using {{site.data.keyword.cis_short_notm}} with a subdomain or CNAME. This example uses the GoDaddy domain registrar. Your case may be different, depending on which registrar you use.
{: shortdesc}

1.	Deploy a {{site.data.keyword.cis_short_notm}} instance via the {{site.data.keyword.cloud_notm}} console or API
    1.	We recommend that you mirror the name of your {{site.data.keyword.cis_short_notm}} instance with the domain or subdomain.
2.	Click the **Let’s get started** button
3.	Enter the subdomain in the **Domain name** field and click **Connect and continue**
4.	For new subdomains click **Next Step**, for existing subdomains click **Import records**
5.	Make note the **New NS records** for entry into the GoDaddy DNS Management system
6.	Login to GoDaddy domain Registrar
7.	Navigate to the DNS Management page for the candidate {{site.data.keyword.cis_short_notm}} domain. {{site.data.keyword.cloud}} must be updated through the API.
8.	Click **ADD** to create a NS Host record for each Name Server entry provided from {{site.data.keyword.cis_short_notm}}
9.	Setup a DNS record with the supplied {{site.data.keyword.cis_short_notm}} NS Records
    1.	Type **Nameserver**
    1.	Host **subdomain** (this is name of the subdomain, for example, `subdomain.example.com`)
    1.	Points to `ns004.name.cloud.ibm.com`
    1.	TTL ½ hour
10.	The DNS Management screen should now reflect the two additional NS records for the subdomain
11.	Navigate back to the {{site.data.keyword.cis_full_notm}} instance
12.	After the NS Records have DNS propagated, it is reflected in the DNS tools and you are ready to proceed
13.	With completed NS propagation click **Check name servers**
14.	{{site.data.keyword.cis_short_notm}} begins to check NS configuration for the subdomain and may be in a **Pending State**

    Any typos in this section produce a "silent" failure. If your subdomain is in the pending state for more than 30 minutes, there may be an error.
    {:note}

Upon successful subdomain Name Server / NS record confirmation, the {{site.data.keyword.cis_short_notm}} instance reports **Active**. Customers are now ready to start configuring the service and routing traffic to their services.
