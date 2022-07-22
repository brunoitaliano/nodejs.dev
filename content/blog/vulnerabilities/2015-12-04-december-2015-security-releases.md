---
title: December Security Release Summary
blogAuthors: ['rvagg']
category: 'vulnerabilities'
---

Last week we [announced](https://groups.google.com/d/msg/nodejs-sec/Zf7Nxtg230E/eX4UCWf0BAAJ) the planned release of patch updates to the v0.12.x, v4.x and v5.x lines to fix two vulnerabilities. That was further amended by the [announcement](https://mta.openssl.org/pipermail/openssl-announce/2015-November/000045.html) of OpenSSL updates with fixes for vulnerabilities labelled _medium_ severity. The OpenSSL update impacts all active release lines, including v0.10.x.

Today we have released Node.js [v0.10.41 (Maintenance)](/en/blog/release/v0.10.41/), [v0.12.9 (LTS)](/en/blog/release/v0.12.9/), [v4.2.3 "Argon" (LTS)](/en/blog/release/v4.2.3/) and [v5.1.1 (Stable)](/en/blog/release/v5.1.1/) with fixes for the announced vulnerabilities and updates to OpenSSL.

For the purpose of understanding the impact that the fixed vulnerabilities have on your Node.js deployment and the urgency of the upgrades for your circumstances we are providing details below.

### CVE-2015-8027 Denial of Service Vulnerability

This critical denial of service (DoS) vulnerability impacts all versions of v0.12.x through to v5.x, inclusive. The vulnerability was discovered by Node.js core team member Fedor Indutny and relates to HTTP pipelining. Under certain conditions an HTTP socket may no longer have a parser associated with it but a pipelined request can trigger a pause or resume on the non-existent parser thereby causing an `uncaughtException` to be thrown. As these conditions can be created by an external attacker and cause a Node.js service to be shut down we consider this a critical vulnerability. It is recommended that users of impacted versions of Node.js exposing HTTP services upgrade to the appropriate patched versions as soon as practical.

* Versions 0.10.x of Node.js are not affected.
* Versions 0.12.x of Node.js are **vulnerable**, please upgrade to [v0.12.9 (LTS)](/en/blog/release/v0.12.9/).
* Versions 4.x, including LTS Argon, of Node.js are **vulnerable**, please upgrade to [v4.2.3 "Argon" (LTS)](/en/blog/release/v4.2.3/).
* Versions 5.x of Node.js are **vulnerable**, please upgrade to [v5.1.1 (Stable)](/en/blog/release/v5.1.1/).

### CVE-2015-6764 V8 Out-of-bounds Access Vulnerability

A bug was discovered in V8's implementation of `JSON.stringify()` that can result in out-of-bounds reads on arrays. The patch was included in this week's [update of Chrome Stable](http://googlechromereleases.blogspot.nl/2015/12/stable-channel-update.html). While this bug is high severity for browsers, it is considered lower risk for Node.js users as it requires the execution of third-party JavaScript within an application in order to be exploitable.

Node.js users who expose services that process untrusted user-supplied JavaScript are at obvious risk. However, we recommend that all users of impacted versions of Node.js upgrade to the appropriate patched version in order to protect against malicious third-party JavaScript that may be executed within a Node.js process by other means.

* Versions 0.10.x of Node.js are not affected.
* Versions 0.12.x of Node.js are not affected.
* Versions 4.x, including LTS Argon, of Node.js are **vulnerable**, please upgrade to [v4.2.3 "Argon" (LTS)](/en/blog/release/v4.2.3/).
* Versions 5.x of Node.js are **vulnerable**, please upgrade to [v5.1.1 (Stable)](/en/blog/release/v5.1.1/).

### CVE-2015-3193 OpenSSL BN\_mod\_exp may produce incorrect results on x86\_64

A bug exists in OpenSSL v1.0.2 in the [Montgomery squaring](https://en.wikipedia.org/wiki/Exponentiation_by_squaring#Montgomery.27s_ladder_technique) procedure on the x64 architecture that expose potential attack vectors. Attacks against RSA and DSA are considered possible but with a very high degree of difficulty. Attacks against DHE key exchange is considered feasible but difficult. EC algorithms are not vulnerable. Node.js TLS servers using DHE key exchange are considered at highest risk although it is believed that Node.js' existing use of `SSL_OP_SINGLE_DH_USE` may make [DHE attacks impractical](https://blog.fuzzing-project.org/31-Fuzzing-Math-miscalculations-in-OpenSSLs-BN_mod_exp-CVE-2015-3193.html). Details are available at <http://openssl.org/news/secadv/20151203.txt>.

OpenSSL v1.0.2 is used in Node.js v4.x LTS and v5.x. It is strongly recommended that Node.js users exposing TLS servers upgrade to patched versions as soon as practical.

* Versions 0.10.x of Node.js are not affected.
* Versions 0.12.x of Node.js are not affected.
* Versions 4.x, including LTS Argon, of Node.js are **vulnerable**, please upgrade to [v4.2.3 "Argon" (LTS)](/en/blog/release/v4.2.3/).
* Versions 5.x of Node.js are **vulnerable**, please upgrade to [v5.1.1 (Stable)](/en/blog/release/v5.1.1/).

### CVE-2015-3194 OpenSSL Certificate verify crash with missing PSS parameter

A bug exists in OpenSSL v1.0.1 and v1.0.2 that may cause a crash during certificate verification procedures when supplied with a malformed ASN.1 signature using the RSA PSS algorithm. This may be used as a the basis of a denial of service (DoS) attack against Node.js TLS servers using client authentication. Node.js TLS clients are also impacted if supplied with malformed certificates for verification. Details are available at <http://openssl.org/news/secadv/20151203.txt>.

OpenSSL v1.0.0 is used in Node.js v0.10.x and v0.12.x. OpenSSL v1.0.2 is used in Node.js v4.x LTS and v5.x. It is strongly recommended that Node.js users employing either TLS client or server code upgrade as soon as practical.

* Versions 0.10.x of Node.js are **vulnerable**, please upgrade to [v0.10.41 (Maintenance)](/en/blog/release/v0.10.41/).
* Versions 0.12.x of Node.js are **vulnerable**, please upgrade to [v0.12.9 (LTS)](/en/blog/release/v0.12.9/).
* Versions 4.x, including LTS Argon, of Node.js are **vulnerable**, please upgrade to [v4.2.3 "Argon" (LTS)](/en/blog/release/v4.2.3/).
* Versions 5.x of Node.js are **vulnerable**, please upgrade to [v5.1.1 (Stable)](/en/blog/release/v5.1.1/).

**Note:** Node.js users are not considered vulnerable to the two additional announced OpenSSL vulnerabilities: CVE-2015-3195 "X509\_ATTRIBUTE memory leak" and CVE-2015-3196 "Race condition handling PSK identify hint". However, fixes for these bugs are included with the new versions of OpenSSL bundled with the newly patched versions of Node.js.