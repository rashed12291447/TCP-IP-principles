# Technical interview preparation for Group 04

Prepared for Md Rashed Bepari, 12291447, on 6 October 2026. This guide explains the concepts generally, then connects them to the submitted Group 04 project. It is a study resource, not an interview script or a replacement for your own understanding.

The review uses the four supplied A3 documents and the uploaded repository ZIP. It does not establish the current running state of the lab, the authors of individual GitHub commits, or the contents of the separate Drive-hosted GNS3 exports. Your submitted repository has not been changed.

## 1 What the assessment actually requires

Your A2 project was not wasted work. A2 is the build; A3 checks whether you personally understand what you built and can reason about changes to it. You are not expected to recite every configuration line from memory.

The interview is individual, about 15 minutes, and worth 30% of the unit. The minimum is 15/30; the brief states that scoring below this minimum prevents passing the unit, subject to the University's supplementary-assessment rules. Prepare for understanding and strong explanations rather than aiming only at that threshold.

**Practical arrangements from the supplied documents:**

- Submit Part B activity evidence to qualify for an interview slot. It is a prerequisite, not separately scored. Check Moodle for your submission status and allocated time.
- Interviews take place in Weeks 12–13. A scheduled interview has no automatic 72-hour grace period. Do not infer your appointment from these week numbers; use your actual Moodle booking.
- Keep your repository open and be ready to navigate it throughout. For Zoom, join ready, camera on and screen shared. The interview is recorded.
- The hints explicitly say that GNS3 does not need to be running. This differs from the A2 demonstration video.
- You may look up your own configurations and evidence. Port numbers, exact command syntax and RFC numbers need not be memorised.
- Identity is checked; the hints clarify that you do not need to hold up a student card.
- Expect follow-up questions even after correct answers. Ask for rephrasing or a smaller question when needed. Technical reasoning matters more than polished English.
- The assessment is No AI. Do not use this assistant or AI-generated live assistance during it. The hints say to bring your work, not your notes. Use these materials to learn beforehand, and follow any additional preparation restrictions from your teaching team.

The formal brief describes two to three conceptual questions; the preparation hints give a more specific typical sequence: one concept question and one project-extension question. Treat the timing below as guidance, not a guarantee of exact wording or counts.

| Typical stage | Approximate time | Your task |
| --- | --- | --- |
| Start | 30 seconds | Be ready and confirm the interview can proceed |
| Show me | 2 minutes | Open one item of your own work and explain what it does and why |
| Concept and extension | 6½ minutes | Reason about a concept and a change to your project |
| Reflection | 5 minutes | Evaluate a technical decision and the way the group worked |

## 2 Where the marks come from

| Criterion | Maximum | Preparation that addresses it |
| --- | --- | --- |
| Own contribution walkthrough | 6 | Identify your work, explain it, and use actual GitHub history |
| Concepts across the unit | 7 | Explain mechanisms, limits, evidence and trade-offs |
| Project extension | 6 | Apply a sensible design to the given scenario and your existing boundaries |
| Reflection | 8 | Evaluate a real decision and a real collaboration experience |
| Communication and professionalism | 3 | Answer clearly, navigate evidence, listen and respond to follow-ups |

Reflection is the largest individual category. Do not spend all your preparation memorising cryptographic terms while leaving your own decisions and group experiences unexplored.

For most technical answers, practise this sequence: **purpose → how it works → evidence → limitation**. For a design question, add the alternative you rejected and its trade-off. These are thinking prompts, not sentences to memorise.

Example reasoning: a DMZ reduces the paths available to a compromised web server; the firewall controls traffic crossing its boundary; an allowed HTTPS test and a denied DMZ-to-LAN test show different decisions; same-LAN traffic and application vulnerabilities still need separate controls.

## 3 How to study this without becoming overwhelmed

Work through one session at a time. Each session can take 45–60 minutes: learn for 15 minutes, find your evidence for 10, explain out loud for 10, and answer follow-ups for 10. Finish by recording what you could not yet explain.

| Session | Study focus | You are ready to move on when you can… |
| --- | --- | --- |
| 1 | Sections 4–6: topology, packets and firewall | Trace a local and a cross-site flow and explain which boundary sees it |
| 2 | Sections 7–8: PKI, TLS, SSH and passwords | Separate identity, encryption, hashing and access control |
| 3 | Sections 9–10: Tailscale, IPsec and Kerberos | Explain the two tunnel layers and the ticket exchange without mixing them |
| 4 | Sections 11–12: IDS and packet evidence | Explain what your rule detects and what each capture actually proves |
| 5 | Sections 13–14: wireless and cloud | Propose a change, limit its access, identify a cost and specify a test |
| 6 | Sections 15–17: contribution and reflection | Find your records quickly and discuss two real decisions honestly |
| 7 | Companion practice workbook | Complete a 15-minute mock with follow-ups and then correct your gaps |

If you have only one day, prioritise your opening artefact, firewall/IDS evidence, the three packet journeys, Kerberos versus IPsec, one wireless scenario and two genuine reflections. Then cover cloud and weaker concepts. This is triage, not a prediction of the questions.

Your full weekly lecture material was not part of this upload. The guide covers the domains named in the A3 brief and the supplied project; use the lecture topic list to catch any additional unit concepts.

## 4 The foundations in plain language

A **host** is an endpoint such as a client or server. A **service** is a program listening for requests, such as SSH or HTTPS. A **port** identifies a transport-layer service endpoint; it does not identify a person or guarantee that traffic is safe.

A **subnet** groups addresses that hosts ordinarily treat as reachable on the same local link. A /24 fixes the first 24 bits of the address: 10.11.1.10 and 10.11.1.30 are in the same 10.11.1.0/24 subnet. An Ethernet host normally uses ARP to find the destination's local MAC address. For a remote subnet, it sends the Ethernet frame to its gateway's MAC address, while the IP destination remains the remote host.

A **switch** forwards Ethernet frames within the local network. A **router** forwards IP packets between networks. A **firewall** applies access policy to traffic it processes. OPNsense performs routing and stateful filtering in this project. A route tells it where a packet should go; a rule determines whether the packet is allowed to go there.

A **DMZ** is a separate security zone for exposed services. Its value comes from enforced boundaries, not its name. A VLAN can separate layer-2 broadcast domains, but access between routed VLANs still needs policy.

**Confidentiality** limits who can read data. **Integrity** helps detect unauthorised alteration. **Availability** keeps services usable. A strong lockdown can harm availability if it blocks necessary traffic or locks out legitimate users.

**Authentication** checks identity; **authorisation** determines permitted actions. **Accounting/auditing** records activity. A VPN connection, a successful ping and a valid identity each answer different questions.

**Least privilege** means allowing only needed access. **Defence in depth** uses controls at different points so one failure does not remove all protection. It also increases configuration and maintenance work; more tools alone do not guarantee a safer design.

## 5 Your actual topology and three packet journeys

### The addresses worth recognising

| Role | Site A | Site B |
| --- | --- | --- |
| LAN and gateway | 10.11.1.0/24; 10.11.1.1 | 10.12.1.0/24; 10.12.1.1 |
| DMZ and gateway | 10.11.2.0/24; 10.11.2.1 | 10.12.2.0/24; 10.12.2.1 |
| Transit | 10.11.9.0/24 | 10.12.0.0/24 |
| OPNsense WAN | 10.11.9.1 | 10.12.0.1 |
| VPN router transit interface | 10.11.9.2 | 10.12.0.2 |
| Admin or test host | 10.11.1.10 | 10.12.1.10 |
| Browser or management host | 10.11.1.100 | 10.12.1.100 |
| CA host | 10.11.1.20 | 10.12.1.30 |
| Hardened SSH host | 10.11.1.30 | 10.12.1.40 |
| Password-policy host | 10.11.1.40 | 10.12.1.50 |
| Web server | 10.11.2.20 | 10.12.2.20 |
| Passive IDS | 10.11.2.50 | 10.12.2.30 |

The shared realm is EXAMPLE.COM. **kdc.example.com is 10.11.1.60. server.example.com is 10.11.1.70.** They are different machines. Client-B is 10.12.1.60. Also distinguish hardened SSH-A at .30 from the Kerberos SSH server at .70.

Recognise the roles and be able to find the table; the interviewer does not require you to memorise every address.

### Journey A Local HTTPS from Admin-A to Web-A

1. Admin-A recognises that 10.11.2.20 is outside its own /24 and sends the packet via gateway 10.11.1.1.
2. It enters OPNsense on LAN. The explicit LAN-to-Web-A TCP 443 rule allows it and creates state.
3. OPNsense routes it out the DMZ interface through the monitoring hub and DMZ switch to Web-A.
4. The server's reply traverses the firewall and can match the existing state. The DMZ block against new unsolicited LAN connections does not automatically prohibit this reply.
5. TLS protects the client-to-server session. The passive IDS can observe the traffic crossing the hub, but normally cannot read encrypted HTTP content.

### Journey B Local hardened SSH from Admin-A to SSH-A

Both hosts are in 10.11.1.0/24. Normal traffic is switched directly between them. OPNsense interzone rules are not the explanation for rejecting a password login on this path. SSH configuration and host controls such as Fail2ban enforce those restrictions.

### Journey C Cross-site Kerberos service connection

Client-B sends traffic to its local OPNsense-B gateway. For matching LAN-to-LAN traffic, the firewall applies the IPsec policy and protects the inner packet. The resulting outer IPsec packet uses firewall endpoints 10.12.0.1 and 10.11.9.1. It passes through VPNRouter-B, the Tailscale overlay and VPNRouter-A to OPNsense-A. OPNsense-A decapsulates it, applies the relevant IPsec-interface permission and routes it to the A LAN service.

Keep three address layers distinct: the client/server addresses belong to the application flow; the firewall WAN addresses belong to the inner IPsec tunnel; the Tailscale routers provide the outer transport. The routers' reported Tailscale addresses are 100.109.57.73 and 100.111.168.7, not replacements for the firewall peer addresses.

There is a return path as well as a forward path. Missing return routes, unwanted source NAT or a blocked reply can break the connection even when the request arrives.

## 6 Firewalls and zones

Open `sites/site-a/configs/firewall-site-a-config.txt`. It describes a September-stage export: useful for the local firewall walkthrough, but not a complete final IPsec configuration.

The Site A rules identify approved administrators with SITE_A_ADMIN, permit those hosts to manage the firewall on TCP 443, permit LAN HTTPS to WEB_A, allow limited diagnostic ping and then block other traffic. The DMZ has an explicit logged block towards the LAN. An alias is a reusable named set of addresses; it is not a separate authentication mechanism.

**Rule direction:** an ordinary interface rule is evaluated when traffic enters that interface. A connection initiated by a LAN host is checked on LAN. A new connection initiated by the DMZ is checked on DMZ. Decrypted policy-based IPsec traffic requires appropriate IPsec rules; a rule for plaintext arriving at WAN is not automatically interchangeable.

**Order:** in your quick interface rules, the first matching decision matters. A narrow allow below a matching block is ineffective. Your progress record documents this exact failure for cross-site ping. State tables also matter: established traffic may retain an existing state after a rule edit, so a clean new connection is useful during testing.

**No matching permission:** in your intended/default-deny policy, a new unpermitted connection is blocked. Do not generalise that every firewall product has this default, or ignore an earlier broad allow or existing state.

**Stateful filtering:** the firewall remembers permitted flows and recognises related replies. Stateless filtering evaluates packets independently and requires explicit attention to the return direction. Neither means that all traffic on a permitted port is harmless.

**Block versus reject:** a drop silently discards the packet; a reject gives an appropriate refusal. A timeout can be consistent with dropping, but may also result from routing failure or an unavailable host. Your matching block log provides stronger evidence of the policy decision.

**NAT:** source NAT changes the source address; destination NAT changes the destination. NAT is not encryption. Your intended intersite no-NAT configuration preserves original source addresses for rules, logs and return routing. Disabling subnet-router SNAT creates a requirement for proper return routes.

**Management trade-off:** limiting firewall administration to .10 and .100 narrows exposure but depends on trusted admin endpoints and correct permissions. The Site A export disables the anti-lockout rule, so an incorrect replacement permission could lock you out; console access is the recovery route.

**Know the real site difference:** Site B's submitted firewall reference retains a broad LAN allow rule and broad WAN permissions. Do not claim both sites have identical least-privilege rules. Its documented IPsec subnet rule direction also needs careful interpretation: a B-LAN-to-A-LAN source/destination pair is not evidence of a correctly scoped A-to-B inbound rule. These are review limitations, not proof of a specific runtime failure.

Self-check: explain why HTTPS replies are allowed while a new DMZ SSH attempt is denied, and why your Admin-A-to-SSH-A test does not test the interzone firewall.

## 7 Cryptography PKI and HTTPS

### Encryption hashing and signatures

Symmetric encryption uses shared secret key material and is efficient for bulk data. Asymmetric cryptography uses public/private key pairs for operations such as signatures and authenticated key establishment. A hash produces a digest; hashing alone does not keep a password safe from guessing and does not authenticate who supplied a file. A digital signature can bind data to possession of a private signing key, subject to trust in the corresponding public key.

Do not describe signing as simply encrypting all data with a private key. The operations and security purposes differ.

A public key is designed to be shared. A private key must remain protected. A certificate contains identity information and a public key, signed by an issuer; it does not contain the subject's private key.

### Your certificate chain

On Site A, the Root CA signs the Intermediate CA certificate, and the Intermediate signs Web-A's server certificate. The client trusts the root through its trust store. It can then validate the chain, hostname and validity periods under the client's validation policy. A root's self-signature alone does not cause a client to trust it.

The hierarchy separates a trust anchor from routine signing, but both CA roles reside on CA-A in this lab. Do not call this an offline root. Site B's report says its live signing keys were actually created on DMZhost, while CAhost holds a separate unused pair. That increases the impact of a compromised web server.

Open `intermediate-ca.ext` and `www.12291447.lab.ext` under Site A configs. Understand these fields:

| Field | Meaning in your files |
| --- | --- |
| CA:TRUE | The intermediate is a CA certificate |
| pathlen:0 | No further non-self-issued intermediate CA is allowed beneath it; it can still sign end-entity certificates |
| keyCertSign and cRLSign | Permit certificate and revocation-list signing within the certificate's constraints |
| CA:FALSE | Web-A's certificate is not a CA certificate |
| serverAuth | Intended extended usage includes TLS server authentication |
| subjectAltName | Includes www.12291447.lab, the name the client should validate |

A CSR is a certificate signing request containing the public key and requested identity information, signed to demonstrate possession of the matching private key. Sending a CSR does not require sending that private key.

### How HTTPS uses the keys

For a typical certificate-based TLS 1.3 connection, peers negotiate parameters and exchange ephemeral key shares. The server proves its identity using its certificate and a signature. They derive symmetric traffic keys for authenticated encryption. The certificate's public key is not used to encrypt every byte of the web page, and the shared traffic secret is not simply sent openly.

A successful `curl` without `-k` supports the client's certificate validation under its configured trust. `--resolve` supplies a chosen address for a hostname while retaining that hostname for TLS and HTTP. `-k` disables certificate verification, so success with it would not prove trusted server identity. Supplying an intended CA with `--cacert` can validate a private CA without globally installing it.

**Important correction to your report:** a normal passive TLS 1.3 capture shows ClientHello and ServerHello, but the subsequent handshake, including the server Certificate message, is encrypted. Do not repeat the report's E8 claim that the certificate exchange is readable in this TLS 1.3 capture. Certificate inspection and chain verification need endpoint output or authorised decryption secrets. RFC 8446 section 2 documents the encrypted-handshake change.

If asked what happens after key compromise, separate cases: loss of a server private key affects that server; loss of a CA signing key can permit fraudulent certificates trusted under that CA. Revocation, replacement, trust-store changes and investigation are different parts of the response. Ephemeral key exchange can provide forward secrecy for recorded past sessions, but it does not make an actively compromised endpoint safe.

## 8 SSH local passwords and authentication factors

Your hardened SSH-A host uses the student account and the administrator's Ed25519 key. The private key is held by the client; the matching public key is placed in the server account's authorized_keys. The server also has a host key that identifies the server to clients. A user's authorised key and the server's host key are different things.

In `ssh-a-config.txt`, explain PermitRootLogin no, PasswordAuthentication no, KbdInteractiveAuthentication no, AllowUsers student and the disabled forwarding options. Removing password fallback reduces remote password-guessing exposure. It does not stop someone who steals a usable private key or compromises an already-authorised client.

MaxAuthTries limits attempts within an SSH connection. Fail2ban separately watches logs and applies temporary source bans across failures. Your values are three failures in 600 seconds and a 600-second ban. Admin-A is exempt, creating a recovery convenience and an exposure if that host is compromised. The custom filter recognises the image's sshd-session log format; a ban system that does not match the real log format may never trigger.

PAM is a framework through which local applications apply authentication and account policies. Password-A uses a minimum of 12 characters, at least one uppercase letter and one digit, with root-initiated changes also checked. It locks an account after five failures within 900 seconds for a configured 300 seconds. Dictionary checking is disabled. These settings apply to the configured local PAM paths, not automatically to Kerberos or every host in the network.

Distinguish **online guessing**, where a live service can rate-limit or lock accounts, from **offline guessing**, where an attacker holding password hashes can test guesses without contacting that service. A unique salt prevents efficient shared precomputation and makes identical passwords have different stored representations. It is not a secret. A deliberately costly password-hashing algorithm increases the work per guess; ordinary fast hashing is not a substitute.

Site B includes MD5-crypt, SHA512-crypt and yescrypt comparison accounts. Treat those as laboratory comparisons, not recommendations to deploy weak hashes. Avoid displaying actual password hashes or secrets just to explain the concept.

MFA uses distinct factor categories, such as something known plus something possessed. A username is not a second factor; two passwords are still the same category. A stolen password alone should not suffice when a valid additional factor is required. MFA does not automatically stop malware using an already-authenticated session, and ordinary one-time codes can still be phished. Do not claim your project deployed MFA: it did not.

## 9 Tailscale and the additional IPsec tunnel

Tailscale supplies encrypted connectivity between the remote lab routers. Subnet routers let devices without their own Tailscale client use routes to the other site's networks. Your advertised ranges are 10.11.0.0/16 and 10.12.0.0/16. Route advertisement, administrative approval, accepting routes and access permission are distinct requirements. IP forwarding and return routes are also needed.

An exit node carries general Internet traffic through another device. That is different from advertising these specific lab subnets; your router is documented with exit-node use disabled.

Your IPsec tunnel adds protection between the two OPNsense firewalls, inside that transport. It fulfils the project's separate tunnel task and protects traffic on the firewall-to-router transit links that lie outside Tailscale's encryption endpoints. It also adds operational complexity, encapsulation overhead and potential MTU/fragmentation problems. Do not claim that traffic was unencrypted across the Internet before IPsec: Tailscale was already present.

The protected selectors are only 10.11.1.0/24 and 10.12.1.0/24. Advertising a /16 through Tailscale does not expand the IPsec selectors to include DMZ or future wireless networks.

Your settings distinguish peer negotiation from protected data:

- IKEv2 with mutual PSK authenticates peers and negotiates security associations. Keep the PSK private.
- ESP tunnel mode protects matching inner packets. ESP is IP protocol 50, not TCP or UDP port 50. NAT traversal can carry ESP in UDP 4500; IKE commonly starts on UDP 500.
- Your documented proposal uses AES-GCM with a 256-bit key and 128-bit authentication tag, SHA-256 PRF and DH group 14; the child uses PFS group 14. GCM provides authenticated encryption, while the PRF is involved in derivation. These labels do not all describe the same operation.
- An established IKE SA does not alone prove an installed child SA or successful application delivery. Check child state, counters and an end-to-end test.

The project documented an ESP transport failure despite successful negotiation. Your tailnet retained a broad initial grant and added explicit `esp:*` permissions between firewall peers. Tailscale's grants documentation distinguishes the bare `*` selector's TCP/UDP/ICMP coverage from protocol-specific selectors. Explain the packet/counter observations that motivated the change; do not state that a wildcard universally means every IP protocol.

To prove the inner tunnel, compare the same HTTP flow at the firewall WAN-to-transit link. Before: readable HTTP. After: successful client delivery plus ESP visible there. A capture between the router and NAT would already show Tailscale protection in both cases and would not isolate the added tunnel.

## 10 Kerberos from first principles

Kerberos provides centralised, ticket-based authentication. The core password-based exchange uses symmetric keys; it is not the same certificate system as your HTTPS PKI. Some Kerberos extensions can use public-key methods, but those were not the mechanism shown in your project.

| Term | Meaning | Your example |
| --- | --- | --- |
| Realm | An administrative authentication domain | EXAMPLE.COM |
| Principal | A named identity | student@EXAMPLE.COM |
| KDC | Trusted service with authentication and ticket-granting functions | kdc.example.com, 10.11.1.60 |
| TGT | Ticket used to request further service tickets | Ticket for krbtgt/EXAMPLE.COM |
| Service ticket | Ticket for a particular service identity | host/server.example.com@EXAMPLE.COM |
| Keytab | File containing long-term secret keys for service principals | /etc/krb5.keytab on Server-A |
| Credential cache | Client storage for acquired tickets and associated credentials | Inspected with klist |

Follow this exchange rather than memorising protocol names alone:

1. Client-B requests initial credentials from the KDC. In your preauthentication-based setup, it proves knowledge of appropriate secret key material without sending the password as plaintext.
2. The client receives a TGT and client-side session-key material. The TGT's protected contents are for the KDC's ticket-granting service; they are not simply encrypted with the user's password. The corresponding client portion is protected using the applicable initial-authentication mechanism.
3. When SSH needs the server's service ticket, the client presents the TGT and an authenticator to the ticket-granting service.
4. The KDC issues a service ticket protected for the service's long-term key, plus the corresponding client material.
5. The client presents the service ticket and fresh authentication data to Server-A. The server uses its keytab to process the ticket and verify the proof. The service still applies authorisation, such as whether the authenticated identity may use the local account.

`kinit` obtains initial credentials; `klist` shows the cache; `kdestroy` removes cached tickets. A TGT alone does not prove SSH worked. Your test additionally returns the remote hostname/account and obtains the host service ticket. It forces GSSAPI and disables password, keyboard-interactive and public-key fallback in that invocation, which makes the positive and negative tests meaningful.

After destroying the cache, a fresh restricted SSH connection was denied. This does not prove an existing SSH session is terminated, or that every alternative authentication method on the server is globally disabled.

Names matter because a requested service principal must match the service's configured identity and key. Time matters because authenticators and tickets have freshness and lifetime checks. Replay resistance uses these checks and replay handling; it does not mean a ticket is a public, harmless token.

A keytab is secret, even though its filename looks like a configuration file. Mode 600 limits local access but does not encrypt it. Compromising the KDC can undermine the realm's trust. One KDC simplifies the lab but reduces resilience: new ticket acquisition depends on it; some already-issued valid tickets can remain useful while it is unavailable.

## 11 Suricata and what your two rules really do

Your IDS-A observes the DMZ boundary through a hub. It is passive, so it can alert without being responsible for forwarding or dropping packets. Its placement does not expose every same-LAN connection or wireless radio frame. An IPS would need an inline/enforcement arrangement; using an alert action on a mirrored feed does not create that arrangement.

Open `sites/site-a/configs/custom.rules`:

```text
alert tcp 10.11.2.0/24 any -> 10.11.1.0/24 22 (msg:"GROUP04 - DMZ to LAN SSH attempt"; flags:S,CE; flow:stateless; priority:2; sid:1000001; rev:1;)
```

| Part | Explain it in ordinary words |
| --- | --- |
| alert | Record an alert when the condition matches |
| tcp | Inspect TCP traffic for this rule |
| 10.11.2.0/24 any | DMZ source addresses with any source port |
| -> 10.11.1.0/24 22 | Traffic towards the LAN's SSH destination port |
| msg | Human-readable description, not proof of an attack outcome |
| flags:S,CE | Match SYN with the specified C/E flag mask; it does not mean SYN, C and E must all be set |
| flow:stateless | Do not require an established tracked connection, useful for blocked attempts |
| sid and rev | Rule identifier and revision |
| priority | Alert severity metadata |

The second rule changes the destination port to 4444 and SID to 1000002. A port number is a clue, not proof of a reverse shell or malware. SYN retransmissions can produce several alerts for one attempted connection; alert count is not automatically a count of separate attackers.

Your inspected alert screenshot shows three SSH-port alerts and two port-4444 alerts from 10.11.2.20 to 10.11.1.30. The packet capture contains the matching five SYN attempts. Your benign HTTPS check adds useful contrast, but one clean request cannot establish a general false-positive rate.

A firewall log answers which policy decision was made for traffic. IDS rules can identify additional patterns in traffic they can see, including allowed traffic. In your particular A test, both controls observe the prohibited attempt at the boundary; do not pretend that IDS-A demonstrated an application exploit inside TLS.

Site B's actual submitted rule file has FOUR rules: BadBot HTTP and broad TCP 4444 signatures, plus two A-like DMZ-to-LAN signatures using SIDs 1000003 and 1000004. The report describes only the first two. The additional definitions are present, but definition alone does not prove they were triggered. The HTTP User-Agent rule needs visible HTTP content; ordinary encrypted HTTPS prevents that inspection without decryption.

## 12 Reading your evidence carefully

These counts were checked directly from the supplied PCAP files using Ethernet/IP/TCP record parsing. This checks recorded traffic, not today's runtime state, and does not independently decrypt SSH or ESP.

| File in the submitted snapshot | Observation | What you may reasonably infer |
| --- | --- | --- |
| A ipsec-before-site-a.pcap | 14 packets; HTTP request, HTTP response and proof body present | The test was readable at that observation point before IPsec |
| A after capture, filename starts with a space | 26 packets; 10 ESP; no inner TCP flow in this file | Encrypted outer packets present; pair with client delivery evidence |
| A firewall-site-a.pcap | 94 packets; TLS traffic, ICMP and TCP | Different allowed and attempted flows; correlate denials with firewall logs |
| A suricata-site-a.pcap | 36 packets; targeted SYN attempts and TLS traffic | The sensor could see the triggering packets; pair with alert logs |
| B ipsec-before-site-b.pcap | 12 packets; readable HTTP proof exchange | Plaintext baseline at B's recorded link |
| B ipsec-after-site-b.pcap | 12 packets, including 10 ESP | Tunnelled packets recorded at B's corresponding link |
| B kerberos-crosssite.pcap | 166 packets with port-88 and SSH traffic | Authentication/service exchanges occurred; encrypted SSH does not expose the login result directly |
| B dmz-tls-site-b.pcap | 25 packets, clear hello messages and encrypted TLS records | TLS activity, not a readable TLS 1.3 server certificate |
| B suricata-site-b.pcap | 32 packets including HTTP and port-4444 traffic | Relevant traffic reached the sensor; corroborate rule outcomes with logs |

Useful Wireshark display filters for study are `esp`, `tls`, `tcp.port == 22`, `kerberos || tcp.port == 88 || udp.port == 88`, and a filter combining the proof endpoints with `tcp.port == 8080`. A display filter changes what is shown from saved data; a capture filter changes what was recorded in the first place.

A TCP SYN without a reply does not isolate a firewall cause. An empty filtered capture may mean no test traffic, wrong interface, wrong filter or capture timing. An established tunnel without delivered data does not prove the application works. A shorter SSH session is compatible with rejection but does not prove its exact authentication error. Explain these limits without apologising for them.

## 13 Wireless extensions

Wireless is proposed, not built. Your existing wired authentication and firewalls do not automatically secure radio access. Start by asking who the users are, which services they need and whether their devices can be managed.

Your proposal separates staff VLAN 30, guest VLAN 40 and AP-management VLAN 99. Site A uses 10.11.30.0/24, 10.11.40.0/24 and 10.11.99.0/24; B uses the corresponding 10.12 ranges. Staff initially need only approved HTTPS to the local web service. Guests need controlled Internet access with internal and remote-site access denied. AP management is limited to approved administrators.

**Staff authentication:** WPA3-Enterprise with 802.1X and EAP-TLS uses device/user certificates and a RADIUS authentication service. The client is the supplicant, the AP is the authenticator, and the RADIUS server is the authentication server. Clients must validate the approved CA and RADIUS server name; trusting any certificate or accepting a copied SSID undermines the design. There is a cost in certificate issuance, renewal, revocation and device support.

**Guests:** WPA3-SAE with a separately managed passphrase makes onboarding simpler, but shared access is harder to revoke per person. SAE is designed to resist passive offline password guessing; it does not make weak credentials and vulnerable implementations harmless. Guest isolation also needs network rules and client isolation, not only a separate SSID.

| Threat | What goes wrong | Appropriate response and limit |
| --- | --- | --- |
| Evil twin | An attacker imitates the network name to lure clients | Validate enterprise server identity; a familiar SSID alone is not trustworthy |
| Spoofed deauthentication | Forged management traffic disconnects clients | Require PMF where supported; radio jamming and some pre-association disruption remain |
| Captured WPA2-Personal handshake | Weak passwords may be guessed offline | Prefer suitable modern authentication, strong credentials and managed devices |
| KRACK | Vulnerable handshake handling reinstalls keys and reuses nonces | Patch affected clients/APs; changing the password alone does not fix implementation behaviour |
| Rogue AP | Unauthorised equipment creates an unintended access path | Wired port controls, restricted trunks, inventory and radio monitoring |

Your wired Suricata hub cannot observe rogue beacons or radio-only deauthentication. A wireless sensor provides that visibility. EAP-TLS controls network admission; Kerberos controls access to a service. HTTPS and SSH still protect beyond the radio link.

If staff Wi-Fi needs cross-site Kerberos, update routes, approved source ranges and matching firewall permissions, and decide whether to extend both IPsec selectors. Simply advertising a /16 or creating a VLAN is insufficient. Specify tests for valid access, invalid/revoked credentials, false RADIUS identity, guest isolation and the intended failure behaviour.

## 14 Cloud and other extension scenarios

Treat cloud as a change in where controls run and who operates them. It is not an automatic security upgrade. Start with assets, users, allowed flows, trust boundaries, identity, encryption, observation and failure recovery.

| Current role | Possible AWS analogue for reasoning | Important qualification |
| --- | --- | --- |
| Site address space and subnets | VPC and subnets | A subnet's routing and controls determine exposure; its label alone does not |
| Stateful workload access control | Security groups | Stateful allow rules; not an ordered OPNsense deny-rule list |
| Subnet-level stateless filtering | Network ACLs | Ordered allow/deny rules; return traffic must also be permitted |
| Dedicated network inspection | AWS Network Firewall or a managed appliance | Routing must actually send traffic through it |
| HTTPS termination and certificates | TLS endpoint/load balancer plus certificate management | Decide whether backend traffic also needs TLS and private trust |
| Site-to-site tunnel | Cloud VPN termination | Routes, selectors where applicable, authentication and access scope still matter |
| Cloud resource authorisation | IAM roles and policies | Not a drop-in replacement for a Kerberos host-service keytab |
| Traffic/audit visibility | Flow logs, audit logs and suitable inspection | Metadata logs are not automatically packet payloads or Suricata-equivalent alerts |

For moving Web-A to cloud, identify how users reach it, avoid unnecessary public administrative access, retain appropriate TLS trust, restrict backend flows and add monitoring. Weigh operational burden, cost, outage dependencies and data requirements. The provider secures parts of the platform; your team still configures identities, access, applications and data protection according to the service model.

For a third site, allocate a non-overlapping range, agree security policy, establish routes/tunnel trust, register necessary service identities and test both intended access and denials. Consider KDC availability and policy drift as the group grows.

For a contractor or vulnerable camera, place it in a restricted segment and authorise only the necessary destination/service. A VPN is an entry mechanism, not permission to reach everything. Give access an owner, expiry/revocation process and logs. Prioritise the business need rather than adding tools without specifying traffic.

For ransomware or a compromised LAN host, recognise that same-LAN traffic may bypass your firewall and DMZ sensor. Host controls, backups with tested restoration, reduced privileges, endpoint monitoring and further segmentation address different parts of that risk. None is a feature you should claim was built unless it was.

## 15 Preparing the opening walkthrough

A manageable first choice is your Site A Suricata rule file, backed by the alert screenshot and capture. It is short enough to understand fully and connects to segmentation, TCP, passive detection and evidence limitations. Use it only if you can explain it in your own words; a firewall rule file or the diagram is also allowed.

Practise a two-minute explanation using five prompts:

1. Which part did you personally implement or adapt?
2. What risk did it address?
3. Where does it sit and what traffic can it observe?
4. What does the rule mean, and what evidence shows it matched?
5. What can it not detect or prevent?

Then open GitHub History for that file and identify your real commit. The ZIP contains a source snapshot, not GitHub commit history, PR conversations, Project board activity or Teams messages. None of those individual contribution records was verified from this archive. Use the live repository to find them; do not invent dates, commit IDs or review events.

### Repository navigation practice

| If asked about… | Open in your repository |
| --- | --- |
| Overall zones and endpoints | docs/network.drawio or sites/site-a/README.md |
| Local firewall policy | sites/site-a/configs/firewall-site-a-config.txt |
| Actual block evidence | sites/site-a/screenshots/18-firewall-block-log.png..png |
| IDS detection and placement | sites/site-a/configs/custom.rules; suricata-site-a.yaml; screenshots/19-suricata-topology.png |
| IDS outcomes | sites/site-a/screenshots/20-suricata-alerts.png; captures/suricata-site-a.pcap |
| HTTPS/PKI | sites/site-a/configs/site-a-https.conf; intermediate-ca.ext; www.12291447.lab.ext |
| SSH and password controls | sites/site-a/configs/ssh-a-config.txt; password-a-config.txt |
| Federation | sites/site-a/configs/tailscale-site-a-config.txt |
| Shared identity | sites/site-a/configs/kerberos-config.txt |
| IPsec policy and state | sites/site-a/configs/ipsec-config.txt; screenshots/29-ipsec-site-a-established.png |
| Wireless reasoning | wireless-design.md |
| Technical history | sites/site-a/PROGRESS.md, followed by the actual GitHub commits |
| Group process | Your real PRs/issues and Project board, plus permitted Teams records if relevant |

Practise finding five of these within about 20 seconds each. That is a study target, not an assessment rule.

## 16 Reflection that shows ownership

Prepare facts and judgements, not polished personal speeches. The two required areas are a technical decision/problem and the way your group worked. Be precise about what you did, what Rezwan did, and what you integrated together.

For each real incident, fill in: **situation; your action; observed evidence; alternative; cost; result; what you would change next time**. If a detail is not remembered or recorded, say so rather than making a tidy story.

### Candidate technical reflection A Rule ordering

Your progress record documents a LAN block preceding the cross-site ICMP allow. Explain how you checked addresses/routes and where the attempted packet was observed; which evidence directed you towards rule order; why moving a narrow allow above the block was preferable to removing the block; and which retest established recovery. A sensible process improvement is a shared flow matrix plus positive/negative tests after policy changes. Tie that suggestion to what actually went wrong.

### Candidate technical reflection B IPsec negotiation without useful traffic

The report describes successful negotiation while ESP traffic failed to reach the peer. Explain how counters, capture points and the distinction between IKE and ESP narrowed the issue. Explain the protocol-specific grants fix and what it did not change. Do not claim a ping alone proved ESP transport. One future improvement is a layered verification checklist covering routes, outer protocols, child SAs, decrypted policy and application delivery.

### Candidate technical reflection C Persistence and restart behaviour

The project used persistent directories and startup scripts because stopping nodes could remove runtime state or leave services inactive. The IDS startup script validates configuration and removes a stale PID only when no Suricata process is present. Explain the difference between preserving a file, starting a process, recovering connectivity and restoring a complete export. A script that exists is not proof that automatic startup is configured.

### Candidate design reflection D Shared KDC or passive monitoring

One KDC reduces setup work but creates an availability dependency. Passive monitoring avoids placing another forwarding device in the traffic path but cannot block. For either choice, explain why it fitted the lab and what requirement would justify spending time on redundancy or inline prevention later.

### Group reflection requires your own evidence

You worked remotely and coordinated shared endpoints, routes, principals and policy. To prepare, find one genuine PR review, interface agreement, board delay or integration error. Explain whether the process helped, what it missed and what you personally would change. Do not convert “we used GitHub” into a claim that every change was reviewed. Do not invent a PR correction if none happened.

If asked about AI use in A2, be honest about assistance with commands, troubleshooting or drafting and explain what you personally tested, corrected and now understand. A2 permitted AI collaboration; that does not permit AI help during this individual A3 interview.

## 17 Documentation differences you should understand

These observations help you answer accurately; they are not a prediction of marks or a request to change the frozen submission. If submission is complete, do not silently alter it. Navigate the actual files and acknowledge historical documentation where necessary.

1. **Historical A firewall export.** The supplied sanitised XML has seven local rules and no IPsec configuration section. Use it for the earlier local stage and use the IPsec reference, captures and later screenshots for the shared tunnel. Do not present that XML as a full final live export.
2. **Earlier wording remains.** Some START-STOP, federation, README and report text still says work is pending although later files/evidence exist. Explain chronology; avoid treating every statement as the final state. Some progress dates are inconsistent, so use real commit history to establish dates.
3. **File names differ from links.** A's after capture is actually named with a leading space before `ipsec-after-site-a.pcap`. Several screenshot links omit their numeric prefixes or extra `.png` suffix. Open actual files from their folders; a broken link is not proof the underlying evidence is absent.
4. **Site B policy is broader.** Its firewall reference retains a stock LAN allow and broad WAN permissions. Consistency of goals is not evidence of identical enforcement. Explain this as a least-privilege review need.
5. **Four B IDS rules exist.** The report describes two, while the actual file also contains two DMZ-to-LAN signatures. Separate installed definitions from verified triggering evidence.
6. **TLS 1.3 visibility is overstated in E8.** The certificate message is encrypted after ServerHello. Use endpoint certificate-verification evidence for identity claims.
7. **The sites' live CA arrangements differ.** B's signing keys were on its DMZ web host, according to its own documentation. Do not claim a protected offline CA at either site.
8. **Password policy is host-specific.** Dictionary checking is disabled. B does not explicitly set A's fail_interval. Do not claim network-wide or perfectly identical enforcement.
9. **Wireless remains design only.** RADIUS, APs, wireless VLANs and radio monitoring were not implemented.
10. **Archive scope is limited.** The ZIP does not include online collaboration history or the Drive export contents. Your repository evidence does not prove today's lab availability.

## 18 Troubleshooting without random changes

Use the symptom to choose the next observation. Start with endpoint/service state, then addressing and local routes, cross-site transport, firewall decisions, tunnel state and application authentication. Not every symptom requires every check.

| Symptom | Useful next observation | What it distinguishes |
| --- | --- | --- |
| Local HTTP request fails on the server itself | Listener, process, bind address and file path | Service problem before investigating the remote network |
| Local request succeeds, remote request times out | Relevant interface captures and firewall logs during the same retry | Where the packet stops; not merely whether a router is online |
| Ping works but SSH fails | TCP reachability and exact SSH error | ICMP reachability versus service availability/authentication |
| IKE established, no usable data | Child SA, selectors, counters and outer ESP capture | Negotiation versus data transport and policy |
| kinit cannot contact KDC | Name mapping, port 88 reachability, KDC service and route | Discovery/reachability problem rather than automatically a bad password |
| kinit works but SSH fails | Service principal, keytab/KVNO, GSSAPI settings, hostname, clocks and authorisation | Initial ticket acquisition versus service authentication |
| No IDS alert | Sensor interface/placement, loaded rules, packet visibility and matching conditions | Missing traffic, rule mismatch or encrypted content |
| HTTPS returns a certificate error | Hostname, chain, trust anchor and time | Identity validation problem rather than necessarily a firewall fault |
| A restart breaks the lab | Saved addresses/routes, persistent data and service startup | Runtime state versus durable configuration |

Read-only study commands include `ip addr`, `ip route`, `ss -lntup`, `tailscale status`, `ipsec statusall`, `klist`, `sshd -T` and inspection of logs. Commands differ between Linux hosts and FreeBSD-based OPNsense; for example, OPNsense routing can be inspected with `route -n get <destination>`. Look up syntax rather than pretending it is identical everywhere. None of these commands needs to be executed during the interview unless specifically requested.

## 19 Last practice and interview day

Run the companion mock once without notes and with your repository open. Answer out loud. For every claim, ask yourself: where is the evidence, what is the alternative explanation, and what is the limitation? Then practise only the gaps you found.

Before the appointment, confirm Part B, your booking/timezone, connection/camera/audio, repository and board access. Have your own opening artefact and an actual contribution record ready. Follow any assessor-specific instructions. GNS3 need not be running under the supplied hints.

During the interview, begin with the main point and use the repository naturally. You can say “Let me open the rule to check its scope,” “Could you rephrase the scenario?” or “I am not certain about that detail; here is what I can establish.” These are ways of communicating honestly, not answers to memorise. A follow-up is normal. Do not use AI or hide outside help.

## Sources and verification

Assessment rules and marking: the uploaded A3 specification, version-2 rubric, hints and sample questions. The hints explicitly say the published concept/extension samples will not be the interview questions. The reflection examples show the kinds of personal questions to prepare.

Project facts: uploaded `a2-SYD-Group-04.zip`, particularly site configuration files, site READMEs, report, progress records, actual rule files and PCAPs. Three A screenshots were directly inspected for the matching firewall block, five IDS alerts and established/installed IPsec with nonzero counters. Repository files can document historical states and were reconciled rather than assumed identical.

Primary technical references for checking details:

- [OPNsense firewall rules](https://docs.opnsense.org/manual/firewall.html)
- [Tailscale site-to-site networking](https://tailscale.com/docs/features/site-to-site)
- [Tailscale grants syntax](https://tailscale.com/docs/reference/syntax/grants)
- [MIT Kerberos keytabs](https://web.mit.edu/kerberos/krb5-latest/doc/basic/keytab_def.html)
- [MIT Kerberos application servers](https://web.mit.edu/~kerberos/krb5-1.21/doc/admin/appl_servers.html)
- [TLS 1.3 RFC 8446](https://www.rfc-editor.org/rfc/rfc8446.html)
- [strongSwan IPsec protocol introduction](https://docs.strongswan.org/docs/latest/howtos/ipsecProtocol.html)
- [Suricata TCP header keywords](https://docs.suricata.io/en/latest/rules/header-keywords.html)
- [Cisco Meraki WPA3 guidance](https://documentation.meraki.com/Wireless/Design_and_Configure/Configuration_Guides/Encryption_and_Authentication/WPA3_Encryption_and_Configuration_Guide)
- [Microsoft EAP and server validation](https://learn.microsoft.com/windows-server/networking/technologies/extensible-authentication-protocol/network-access)
- [Original KRACK research](https://www.krackattacks.com/)
- [AWS security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [AWS network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)

Use these as reference material when learning, not as a list of pages you must memorise.
