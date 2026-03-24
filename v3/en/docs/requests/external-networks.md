# Request for Communication with External Networks

Communications between ABCI and external services/servers are restricted. The permitted communications are:

| Source | Destination | Port | Service Name |
|:--|:--|:--|:--|
| ANY | Compute nodes | DENIED | - |
| Compute nodes | ANY | 22/tcp | ssh |
| Compute nodes | ANY | 53/tcp | dns |
| Compute nodes | ANY | 80/tcp | http |
| Compute nodes | ANY | 443/tcp | https |


For the outbound (Compute nodes -> external) communications which are not permitted above, we will accept an application for permission. If the application is examined and approved, we will permit the communication until the end of the requested period or, at the latest, the end of the fiscal year. However, depending on the outcome of the review, we may not be able to meet your request.

Before submitting the application, please inform us the following information by email addressed to abci3-qa@abci.ai so that we can consider whether the application is acceptable. Please understand that we may ask for additional information.

```
Full Name:
ABCI Account Name:
ABCI Group Name:
Organization:
Registered email address:

* Destination server information, IP address/hostname, role, and administrator (or legal entity that manages the server). 
* Port number of the destination. 
* Period of communication(start date - end date).
* Purpose of communication. 
* Document or URL, etc. explaining that the server name and port number are necessary to achieve the purpose. 
```

After we notify you of your request's eligibility, the responsible person of the ABCI Group should submit the application for permission to communicate to ABCI support. Or, please attach a document showing that you have received approval from the responsible person of the ABCI Group and add the responsible person's email address to the cc of your email, then submit the application. The header part of the application email should be as follows.

```
To: abci3-qa@abci.ai
CC: (email address of the responsible person of the ABCI Group)
Subject: ABCI: Application for external network communication (ABCI Group name)
```

Please [contact](../contact.md) us for the application procedures. Note that the review process may take approximately two to four weeks, so we kindly ask that you submit your application well in advance.

The outbound communication will be permitted only for the ABCI groups whose ABCI application are submitted and approved. However, you can submit an application on the communication permission before submitting the ABCI application.
