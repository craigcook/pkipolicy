# Mozilla Root Store Policy #

*Version 2.8*

*[Effective June 1, 2022][Policy-Archive]*

## 1. Introduction ##

When distributing binary and source code versions of Firefox, Thunderbird, and
other Mozilla-related software products, Mozilla includes with such software
a set of X.509v3 root certificates from various Certification
Authority (CA) operators. The included certificates have their "trust bits"
set for various purposes, so that the software in question can use the CA
certificates to anchor a chain of trust for certificates used by TLS servers
and S/MIME email users without having to ask users for further permission or
information.

This policy covers how the default set of certificates and associated trust
bits is maintained for software products distributed by Mozilla. Other entities
distributing software based on ours are free to adopt their own policies. In
particular, under the terms of the relevant Mozilla license(s) distributors of
such software are permitted to add or delete CA certificates and modify the
values of the trust bits in the versions that they distribute. However,
as with other software modifications, by making such changes a distributor may
well affect its ability to use Mozilla trademarks in connection with its
versions of the software. See the [Mozilla trademark policy][Trademark-Policy] for more
information.

### 1.1 Scope ###

This policy applies, as appropriate, to certificates matching any of the
following (and to the CA operators* that control or issue them):

1.  CA certificates included in, or under consideration for inclusion in, the
    Mozilla root store;

2.  intermediate certificates that have at least one valid, unrevoked chain up
    to such a CA certificate and that are technically capable of issuing 
    working server or email certificates. Intermediate certificates that are not considered to be technically capable will contain either:

    * an Extended Key Usage (EKU) extension that does not contain any of
      these KeyPurposeIds: anyExtendedKeyUsage, id-kp-serverAuth,
      id-kp-emailProtection; or
    * name constraints that do not allow Subject Alternative Names (SANs) of
      any of the following types: dNSName, iPAddress, SRVName, or rfc822Name; *and*

3.  end entity certificates that have at least one valid, unrevoked chain up
    to such a CA certificate through intermediate certificates that are all in
    scope, such end entity certificates having either:

    * an Extended Key Usage (EKU) extension that contains one or more of these
      KeyPurposeIds: anyExtendedKeyUsage, id-kp-serverAuth,
      id-kp-emailProtection; or
    * no EKU extension.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be
interpreted as described in RFC 2119.

\* A CA operator is an organization or legal entity that is in possession or control of a CA certificate and associated keys, which are capable of being used to issue new certificates.

### 1.2 Policy Ownership ###

Mozilla has appointed a [CA Certificate module owner][CA-Cert-Module]
and peers to evaluate new CA requests on our behalf and to make decisions
regarding all matters relating to CA certificates included in our root store.

Further, Mozilla has appointed a [Mozilla CA Certificate Policy module
owner][CA-Policy-Module] and peers to maintain this policy. The policy will
only be changed after public consultation with the Mozilla community, in order
to ensure that all views are taken into account. This policy MAY be updated periodically in accordance with the [Process for Updating the Root Store Policy][Policy-Update-Process]. CA operators MUST adhere to the current version of this policy. You can contact the Mozilla CA
Certificate Policy module team at [certificates@mozilla.org][Email-Us] if you
have questions about this policy.

CA operators or others objecting to a particular decision by either team MAY appeal to
the [Firefox Technical Leadership Module Committee][Gov-Module] who will make a final
decision.

## 2. Certificate Authorities ##

### 2.1 CA Operations ###

CA operators whose certificates are included in Mozilla's root store MUST:

1.  provide some service relevant to users of our software
    products;
2.  follow industry best practice for securing their networks, for example
    by conforming to the [CA/Browser Forum's Network and Certificate System Security Requirements][NSRs] or a
    successor document;
3.  enforce multi-factor authentication for all accounts capable of causing
    certificate issuance or performing Registration Authority or Delegated
    Third Party functions, or implement technical controls operated by the CA
    to restrict certificate issuance through the account to a limited set of
    pre-approved domains or email addresses;
4.  prior to issuing certificates, verify certificate
    requests in a manner that we deem acceptable for the stated
    purpose(s) of the certificates;
5.  verify that all of the information that is included in server certificates remains current and correct at intervals of 825 days or less;

     5.1. for server certificates issued on or after October 1, 2021, each dNSName or IPAddress in a SAN or commonName MUST have been validated in accordance with section 3.2.2 of the CA/Browser Forum's Baseline Requirements within the preceding 398 days;
6.  otherwise operate in accordance with published criteria that we
    deem acceptable; *and*
7.  ensure that all certificates within the scope of this policy, 
    as described in Section 1.1, adhere to this policy.

CA operators MUST follow and be aware of discussions in 
[Mozilla's dev-security-policy][MDSP] forum, where Mozilla's root store is
coordinated. They are encouraged, but not required, to contribute to those
discussions.

### 2.2 Validation Practices ###

We consider verification of certificate signing requests to be acceptable if it
meets or exceeds the following requirements:

1.  all information that is supplied by the certificate subscriber
    MUST be verified by using an independent source of information
    or an alternative communication channel before it is included in
    the certificate;
2.  for a certificate capable of being used for digitally signing or encrypting
    email messages, the CA operator MUST take reasonable measures to verify that
    the entity submitting the request controls the email account
    associated with the email address referenced in the certificate
    *or* has been authorized by the email account holder to act on
    the account holder’s behalf. The CA operator SHALL NOT delegate validation 
    of the domain portion of an email address. The CA MAY rely 
    on validation the CA has performed for 
    an Authorization Domain Name (as specified in the Baseline Requirements) 
    as being valid for subdomains of that Authorization Domain Name. 
    The CA operator's CPS (or, if applicable, the CP or CP/CPS) MUST clearly specify the procedure(s) 
    that the CA employs to perform this verification;
3.  for a certificate capable of being used for TLS-enabled servers, the CA
    MUST ensure that the applicant has registered all domain(s) referenced
    in the certificate or has been authorized by the domain registrant to
    act on their behalf. This MUST be done using one or more of the
    methods documented in section 3.2.2.4 of the CA/Browser Forum Baseline Requirements. The CA operator's
    CPS (or, if applicable, the CP or CP/CPS) must clearly specify the procedure(s) that the CA employs, and
    each documented procedure SHOULD state which subsection of 3.2.2.4 it is
    complying with. CAs are not permitted to use 3.2.2.5 (4) ("any other method") 
    to fulfill the requirements of method 3.2.2.4.8 (IP Address);
4.  for a certificate capable of being used for TLS-enabled servers, the CA
    MUST ensure that the applicant has control over all IP Address(es) referenced
    in the certificate. This MUST be done using one or more of the
    methods documented in section 3.2.2.5 of the CA/Browser Forum Baseline Requirements. The CA operator's
    CPS (or, if applicable, the CP or CP/CPS) MUST clearly specify the procedure(s) that the CA employs, and
    each documented procedure SHOULD state which subsection of 3.2.2.5 it is
    complying with; *and*
5.  for certificates marked as Extended Validation, CA operators MUST comply with the
    latest version of the [Guidelines for the Issuance and Management of
    Extended Validation Certificates][EVGLs].

Validation methods are occasionally found to contain security flaws. When this happens, 
Mozilla expects CA operators to evaluate their practices and respond appropriately to mitigate the risk. 
Mozilla MAY require CAs to make disclosures or modifications, up to and including 
immediately discontinuing use of a method.

### 2.3 Baseline Requirements Conformance ###

CA operations relating to issuance of certificates capable of being used for
TLS-enabled servers MUST also conform to the latest version of the [CA/Browser
Forum Baseline Requirements for the Issuance and Management of Publicly-Trusted
Certificates][BRs] ("Baseline Requirements"). In the event of inconsistency
between this policy's requirements and the Baseline Requirements,
this policy's requirements take precedence. The following is a list of known
places where this policy takes precedence over the Baseline Requirements. If
you find an inconsistency that is not listed here, notify Mozilla so the item
can be considered for addition or clarification.

*   Insofar as the Baseline Requirements attempt to define their own scope, the
    scope of this policy (section 1.1) overrides that. CA
    operations relating to issuance of **all** TLS server certificates in the scope of
    this policy SHALL conform to the Baseline Requirements.

*   Mozilla MAY accept audits by auditors who do not meet the
    qualifications given in section 8.2 of the Baseline Requirements, or refuse
    audits from auditors who do.
    
*   Mozilla MAY restrict permitted algorithms to a subset of those allowed by the 
    Baseline Requirements.

### 2.4 Incidents ###

When a CA operator fails to comply with any requirement of this policy - whether it be
a misissuance, a procedural or operational issue, or any other variety of 
non-compliance - the event is classified as an incident and MUST be reported to Mozilla as soon as the CA operator is made aware. At a minimum, CA operators MUST promptly report all incidents to Mozilla in the form of an [Incident Report][Incident-Report]. Any matter documented in an audit as a qualification, a modified opinion, or a major non-conformity is also considered an incident and MUST have a corresponding Incident Report. CA operators 
MUST regularly update the Incident Report until the corresponding bug 
is marked as resolved in the mozilla.org [Bugzilla][Bugzilla] system by a Mozilla representative. 
CA operators SHOULD cease issuance until the problem has been prevented from reoccurring.  

Mozilla expects the timely remediation of the problems that caused or gave rise to the incident. In response to incidents, Mozilla MAY require the CA operator to submit a plan of action with milestones or to submit one or more additional audits to provide sufficient assurance that the incident has been remediated. Such audits MAY be expected sooner than the CA operator’s next scheduled audit, and thus MAY be expected to be for a period less than a year.

## 3. Documentation ##

### 3.1 Audits ###

Before being included and at least annually thereafter, CA operators MUST obtain certain
audits for their root certificates and all intermediate certificates
that are technically capable of issuing working server or email certificates. 
This section describes the requirements for those audits.

#### 3.1.1 Audit Criteria ####

We consider the criteria for CA operations published in the
following documents to be acceptable:

*   WebTrust "[Principles and Criteria for Certification Authorities - Version
    2.2.1][WebTrust-2.2.1]" or later in [WebTrust Program for Certification
    Authorities][WebTrust-For-CAs];
*   WebTrust "[Principles and Criteria for Certification Authorities – SSL
    Baseline with Network Security - Version 2.5][WebTrust-BRs]" or later in
    [WebTrust Program for Certification Authorities][WebTrust-For-CAs];
*   WebTrust "[Principles and Criteria for Certification Authorities -
    Extended Validation SSL 1.7.3][WebTrust-EV]" or later in
    [WebTrust Program for Certification Authorities][WebTrust-For-CAs];
*   “Trust Service Providers practice” in ETSI EN 319 411-1 v1.3.1 or
    later version [Policy and security requirements for Trust Service Providers
    issuing certificates; Part 1: General requirements][ETSI-319-411-1],
    specifying a policy or policies appropriate to the trust bit(s) being
    applied for; *and*
*   “Trust Service Providers practice” in ETSI EN 319 411-2 v2.4.1 or
    later version [Policy and security requirements for Trust Service Providers
    issuing certificates; Part 2: Requirements for trust service providers
    issuing EU qualified certificates][ETSI-319-411-2], specifying a
    policy or policies appropriate to the trust bit(s) being applied for.

#### 3.1.2 Required Audits ####

##### 3.1.2.1 WebTrust #####

If being audited to the WebTrust criteria, the following audit requirements
apply (see section 3.1.1 for specific version numbers):

*   For the websites trust bit, a CA and all intermediate CAs technically capable
    of issuing server certificates MUST have all of the following audits:

    * [WebTrust for CAs][WebTrust-2.2.1];
    * [WebTrust for CAs - SSL Baseline with Network Security][WebTrust-BRs]; *and*
    * [WebTrust for CAs - EV SSL][WebTrust-EV] if [capable of issuing EV certificates][Capable-of-EV].

*   For the email trust bit, a CA and all intermediate CAs technically capable
    of issuing email certificates MUST have all of the following audits:

    * [WebTrust for CAs][WebTrust-2.2.1].

##### 3.1.2.2 ETSI #####

If being audited to the ETSI criteria, the following audit requirements apply
(see section 3.1.1 for version numbers):

*   For the websites trust bit, a CA and all intermediate CAs technically
    capable of issuing server certificates MUST have one of the
    following audits, with at least one of the noted policies or sets of
    policies:

    * [ETSI EN 319 411-1][ETSI-319-411-1] (LCP and (DVCP or OVCP)) and/or (NCP
      and EVCP); *or*
    * [ETSI EN 319 411-2][ETSI-319-411-2] (QCP-w).

    An audit showing conformance with the EVCP policy is REQUIRED if a CA is [capable of issuing EV certificates][Capable-of-EV].

*   For the email trust bit, a CA and all intermediate CAs technically
    capable of issuing email certificates MUST have one of the
    following audits, with at least one of the noted policies:

    * [ETSI EN 319 411-1][ETSI-319-411-1] (LCP, NCP, or NCP+); *or*
    * [ETSI EN 319 411-2][ETSI-319-411-2] (QCP-l, QCP-l-qscd, QCP-n, or
      QCP-n-qscd)

#### 3.1.3 Audit Parameters ####
Full-surveillance period-of-time audits MUST be conducted and updated audit
information provided no less frequently than **annually** from the time of CA key pair generation until the CA public key is no longer trusted by Mozilla's root store. This cradle-to-grave audit requirement applies equally to intermediate CAs as it does to root CAs. Successive period-of-time audits
MUST be contiguous (no gaps).

Point-in-time audit statements MAY be used to confirm that all of the problems
that an auditor previously identified in a qualified audit statement have been
corrected. However, a point-in-time audit does not replace the
period-of-time audit.

Audit reports that are being supplied to maintain a certificate within the
Mozilla root store MUST be provided to Mozilla via the CCADB within three
months of the point-in-time date or the end date of the period.

#### 3.1.4 Public Audit Information ####

The publicly-available documentation relating to each audit MUST contain at
least the following clearly-labelled information:

1.  name of the company being audited;
2.  name and address of the organization performing the audit;
3.  name of the lead auditor and [qualifications of the team][Auditor-Qualifications] performing the audit, as required by section 3.2; 
4.  Distinguished Name and SHA256 fingerprint of each root and intermediate
    certificate that was in scope;
5.  audit criteria (with version number) that were used to audit each of
    the certificates;
6.  a list of the CA policy documents (with version numbers) referenced during
    the audit;
7.  whether the audit is for a period of time or a point in time;
8.  the start date and end date of the period, for those that cover a period
    of time;
9.  the point-in-time date, for those that are for a point in time;
10. the date the report was issued (which will necessarily be after the end
    date or point-in-time date); 
11. all incidents (as defined in section 2.4) disclosed by the CA, discovered by the auditor, or reported by a third party, that, at any time during the audit period, occurred or were open in Bugzilla;
12. the [CA locations that were or were not audited][Audited-Location]; *and*
13. for ETSI, a statement to indicate if the audit was a full audit, and which
    parts of the criteria were applied, e.g. DVCP, OVCP, NCP, NCP+, LCP, EVCP,
    EVCP+, QCP-w, Part1 (General Requirements), and/or Part 2 (Requirements for
    trust service providers).

An authoritative English language version of the publicly-available audit information MUST be supplied by the Auditor.

If Mozilla determines that an audit provided does not meet the requirements of this policy, then Mozilla MAY require that the CA operator obtain a new audit, at the CA operator's expense, for the period of time in question. Additionally, depending on the nature of concerns with the audit, Mozilla MAY require that the CA operator obtain such an audit from a new auditor.

### 3.2 Auditors ###

In normal circumstances, Mozilla requires that audits MUST be performed
by a Qualified Auditor, as defined in the Baseline Requirements, section 8.2.

A Qualified Auditor MUST have relevant IT Security experience, or have audited a number of CAs, and be independent. ETSI Audit Attestation Letters MUST follow the Audit Attestation Letter template on the [ACAB'c website](https://www.acab-c.com/downloads), and ETSI auditors MUST be members of the [Accredited Conformity Assessment Bodies' Council][ACAB'c] and follow the ACAB'c Charter and Code of Conduct. WebTrust audit statements MUST follow the practitioner guidance, principles, and illustrative assurance reports on the [CPA Canada website](https://www.cpacanada.ca/en/business-and-accounting-resources/audit-and-assurance/overview-of-webtrust-services/principles-and-criteria), and WebTrust auditors MUST be listed as [enrolled WebTrust practitioners][WebTrust Practitioners] on the CPA Canada website. Mozilla MAY, at its sole discretion, decide to temporarily waive membership or enrollment requirements.

Each Audit Report MUST be accompanied by documentation provided to Mozilla of the [audit team qualifications][Auditor-Qualifications] sufficient for Mozilla to determine the competence, experience, and independence of the auditor. 

If a CA operator wishes to use auditors who do not fit the definition of Qualified Auditor, then it MUST
receive written permission from Mozilla to do so in advance of the start
of the audit engagement. Mozilla will make its own determination as to
the suitability of the suggested party or parties, at its sole discretion.


### 3.3 CPs and CPSes ###

We rely on publicly disclosed documentation (e.g., in a Certificate Policy and
Certification Practice Statement) to ascertain that our requirements are met.
Therefore:

1.  the publicly disclosed documentation MUST provide sufficient
    information for Mozilla to determine whether and how the CA operator
    complies with this policy, including a description of the steps
    taken by the CA to verify certificate requests;

2.  the publicly disclosed documentation MUST be available from the CA operator’s official website;

3.  the documentation MUST be made available to Mozilla under one
    of the following Creative Commons licenses (or later versions):

       * Attribution ([CC-BY]) 4.0;
       * Attribution-ShareAlike ([CC-BY-SA]) 4.0;
       * Attribution-NoDerivs ([CC-BY-ND]) 4.0; *or*
       * Public Domain Dedication ([CC-0]) 1.0; 

    or a set of equally permissive licensing terms accepted by Mozilla in
    writing. If no such license is indicated, the fact of application is
    considered as permission from the CA operator to allow Mozilla and the public to
    deal with these documents, and any later versions for root certificates
    that are included in Mozilla's root store, under CC-BY-ND 4.0;

4.  all CPs, CPSes, and combined CP/CPSes MUST be reviewed and updated as necessary at least once every
year, as required by the Baseline Requirements. CA operators MUST indicate that this has
happened by incrementing the version number and adding a dated changelog entry,
even if no other changes are made to the document;

5.  all CPs, CPSes, and combined CP/CPSes MUST be structured according to RFC 3647 and MUST:

       * include at least every section and subsection defined in RFC 3647; 
       * only use the words "No Stipulation" to mean that the particular document 
imposes no requirements related to that section; and
       * contain no sections that are blank and have no subsections; 

6.  CA operators MUST provide a way to clearly determine which CP, CPS, or combined CP/CPS 
applies to each of its root and intermediate certificates; *and*

7.  CA operators SHALL maintain links to older versions of each CP and CPS (or CP/CPS), regardless of changes in ownership or control of the root CA, until the entire root CA certificate hierarchy operated in accordance with such documents is no longer trusted by the Mozilla root store. 

## 4. Common CA Database ##

Mozilla manages its root store using the Common CA Database (CCADB). CA operators with
certificates in Mozilla’s root store MUST use the CCADB, and are bound by the
latest published version of the [Common CCADB Policy][CCADB-Policy], which is
incorporated here by reference.

When required by the CCADB Policy, Mozilla’s root store may be contacted
[by email][Email-Us].

Mozilla has requirements for the use of the CCADB above and beyond those in the
CCADB Policy, as indicated below in this section 4.

### 4.1 Additional Requirements ###

* Effective October 1, 2022, CA operators with intermediate CA certificates that are capable of issuing TLS certificates chaining up to root certificates in Mozilla's root store SHALL populate the CCADB fields under "Pertaining to Certificates Issued by This CA" with either the CRL Distribution Point for the "Full CRL Issued By This CA" or a "JSON Array of Partitioned CRLs"; *and*
* if the revocation of an intermediate certificate chaining up to a root in
Mozilla’s root store is due to a security concern, as well as performing the
actions defined in the CCADB Policy, a [security bug MUST be filed in
Bugzilla][Sec-Bugs].

### 4.2 Surveys ###

Mozilla MAY conduct a survey of CA operators from time to time using the CCADB. CA operators are
REQUIRED to respond to the surveys with accurate information, within the
timescale defined in the survey.

## 5. Certificates ##

### 5.1 Algorithms ###

Root certificates in our root store, and any certificate that
chains up to them, MUST use only algorithms and key sizes from the following
set:

*   RSA keys whose modulus size in bits is divisible by 8, and is at
    least 2048 bits; *or*
*   ECDSA keys using one of the following curves:
    * P-256; *or*
    * P-384.

The following sections detail encoding and signature algorithm requirements for
each of these keys. The encoding requirements on signature algorithms apply to
any contexts where the algorithm is encoded as an AlgorithmIdentifier,
including:

* The signatureAlgorithm field of a Certificate;
* The signature field of a TBSCertificate;
* The signatureAlgorithm field of a CertificateList;
* The signature field of a TBSCertList; *and*
* The signatureAlgorithm field of a BasicOCSPResponse.


#### 5.1.1 RSA

When RSA keys are encoded in a SubjectPublicKeyInfo structure, the algorithm
field MUST consist of an rsaEncryption OID (1.2.840.113549.1.1.1) with a NULL
parameter, as specified by [RFC 8017, Appendix A.1](https://tools.ietf.org/html/rfc8017#appendix-A.1)
and [RFC 3279, Section 2.3.1](https://tools.ietf.org/html/rfc3279#section-2.3.1).
The encoded AlgorithmIdentifier for an RSA key MUST match the
following hex-encoded bytes:
`300d06092a864886f70d0101010500`.

CAs MUST NOT use the id-RSASSA-PSS OID (1.2.840.113549.1.1.10) within a
SubjectPublicKeyInfo to represent an RSA key.

When a root or intermediate certificate's RSA key is used to produce a
signature, only the following algorithms MAY be used, and with the following
encoding requirements:

  * RSASSA-PKCS1-v1_5 with SHA-1.

    The encoded AlgorithmIdentifier MUST match the following hex-encoded bytes:
    `300d06092a864886f70d0101050500`.

  **See section 5.1.3 for further restrictions on the use of SHA-1.**

  * RSASSA-PKCS1-v1_5 with SHA-256.

    The encoded AlgorithmIdentifier MUST match the following hex-encoded bytes:
    `300d06092a864886f70d01010b0500`.

  * RSASSA-PKCS1-v1_5 with SHA-384.

    The encoded AlgorithmIdentifier MUST match the following hex-encoded bytes:
    `300d06092a864886f70d01010c0500`.

  * RSASSA-PKCS1-v1_5 with SHA-512.

    The encoded AlgorithmIdentifier MUST match the following hex-encoded bytes:
    `300d06092a864886f70d01010d0500`.

  * RSASSA-PSS with SHA-256, MGF-1 with SHA-256, and a salt length of 32 bytes.

    The encoded AlgorithmIdentifier MUST match the following hex-encoded bytes:

    ```
    304106092a864886f70d01010a3034a00f300d0609608648016503040201
    0500a11c301a06092a864886f70d010108300d0609608648016503040201
    0500a203020120
    ```

  * RSASSA-PSS with SHA-384, MGF-1 with SHA-384, and a salt length of 48 bytes.

    The encoded AlgorithmIdentifier MUST match the following hex-encoded bytes:

    ```
    304106092a864886f70d01010a3034a00f300d0609608648016503040202
    0500a11c301a06092a864886f70d010108300d0609608648016503040202
    0500a203020130
    ```

  * RSASSA-PSS with SHA-512, MGF-1 with SHA-512, and a salt length of 64 bytes.

    The encoded AlgorithmIdentifier MUST match the following hex-encoded bytes:

    ```
    304106092a864886f70d01010a3034a00f300d0609608648016503040203
    0500a11c301a06092a864886f70d010108300d0609608648016503040203
    0500a203020140
    ```

The above RSASSA-PKCS1-v1_5 encodings consist of the corresponding OID,
e.g. sha256WithRSAEncryption (1.2.840.113549.1.1.11), with an explicit NULL
parameter, as specified in [RFC 3279, Section 2.2.1](https://tools.ietf.org/html/rfc3279#section-2.2.1).
Certificates MUST NOT omit this NULL parameter. Note this differs
from ECDSA, which omits the parameter.

The above RSASSA-PSS encodings consist of the RSASSA-PSS OID
(1.2.840.11.3549.1.1.10) with a corresponding RSASSA-PSS-params structure as
parameter. The trailerField MUST be omitted, as it is unchanged from the default
value. The AlgorithmIdentifier structures describing the hash functions in the
hashAlgorithm field and in the maskGenAlgorithm's parameter MUST themselves
include an explicit NULL in the parameter field, as specified by [RFC 4055, Section 6](https://tools.ietf.org/html/rfc4055#section-6).

Note: as of Firefox version 100, [RSASSA-PSS encodings are supported](https://bugzilla.mozilla.org/show_bug.cgi?id=1088140).

#### 5.1.2 ECDSA

When ECDSA keys are encoded in a SubjectPublicKeyInfo structure, the algorithm
field MUST be one of the following, as specified by [RFC 5480, Section 2.1.1](https://tools.ietf.org/html/rfc5480#section-2.1.1):

  * the encoded AlgorithmIdentifier for a P-256 key MUST match the following
  hex-encoded bytes: `301306072a8648ce3d020106082a8648ce3d030107`; *or*

  * the encoded AlgorithmIdentifier for a P-384 key MUST match the following
  hex-encoded bytes: `301006072a8648ce3d020106052b81040022`.

The above encodings consist of an ecPublicKey OID (1.2.840.10045.2.1) with a
named curve parameter of the corresponding curve OID. Certificates MUST NOT use
the implicit or specified curve forms.

When a root or intermediate certificate's ECDSA key is used to produce a
signature, only the following algorithms MAY be used, and with the following
encoding requirements:

  * If the signing key is P-256, the signature MUST use ECDSA with SHA-256. The
  encoded AlgorithmIdentifier MUST match the following hex-encoded bytes:
  `300a06082a8648ce3d040302`.

  * If the signing key is P-384, the signature MUST use ECDSA with SHA-384. The
  encoded AlgorithmIdentifier MUST match the following hex-encoded bytes:
  `300a06082a8648ce3d040303`.

The above encodings consist of the corresponding OID with the parameters field
omitted, as specified by [RFC 5758, Section 3.2](https://tools.ietf.org/html/rfc5758#section-3.2).
Certificates MUST NOT include a NULL parameter. Note this differs from
RSASSA-PKCS1-v1_5, which includes an explicit NULL.

#### 5.1.3 SHA-1 ####

Effective July 1, 2022, CAs SHALL NOT sign SHA-1 hashes over end entity certificates with an EKU extension containing the id-kp-emailProtection key purpose.

Effective July 1, 2023, CAs SHALL NOT sign SHA-1 hashes over:
  * certificates with an EKU extension containing the id-kp-ocspSigning key purpose; 
  * intermediate certificates that chain up to roots in Mozilla's program; 
  * OCSP responses; *or*
  * CRLs.

CAs MAY sign SHA-1 hashes over end entity certificates which chain
up to roots in Mozilla's program only if all the following are true:

1. the end entity certificate:

     * is not within the scope of the Baseline Requirements;
     * contains an EKU extension which does not contain either of the
     id-kp-serverAuth or anyExtendedKeyUsage key purposes; *and*
     * has at least 64 bits of entropy from a CSPRNG in the serial number; *and*

2. the issuing certificate:

     * contains an EKU extension which does not contain either of the
     id-kp-serverAuth or anyExtendedKeyUsage key purposes; *and*
     * has a pathlen:0 constraint.

Point 2 does not apply if the certificate is an OCSP signing certificate
manually issued directly from a root.

CAs MAY sign SHA-1 hashes over intermediate certificates that
chain up to roots in Mozilla's root store only if the certificate to be signed
is a duplicate of an existing SHA-1 intermediate certificate with the
only changes being all of:

*   a new key (of the same size);
*   a new serial number (of the same length); *and/or*
*   the addition of an EKU and/or a pathlen constraint to meet the
    requirements outlined above. 

CAs MAY sign SHA-1 hashes over OCSP responses only if the signing
certificate contains an EKU extension which contains only the
id-kp-ocspSigning EKU.

CAs MAY sign SHA-1 hashes over CRLs for roots and intermediates
only if they have issued SHA-1 certificates. 

CAs MUST NOT sign SHA-1 hashes over other data, including CT pre-certificates.

### 5.2 Forbidden and Required Practices ###

CA operations MUST at all times be in accordance with the applicable CP
and CPS (or combined CP/CPS).

CA operators MUST maintain a certificate hierarchy such that an included
root certificate does not directly issue end entity certificates to
customers (i.e. a root certificate signs intermediate
issuing certificates), as described in section 6.1.7 of the
[Baseline Requirements][BRs].

CA operators MUST maintain current best practices to prevent
algorithm attacks against certificates. As such, all new certificates
MUST have a serial number greater than zero, containing at least 64 bits of
output from a CSPRNG.

CA operators MUST NOT issue certificates, CRLs, or OCSP responses, that have:

*   ASN.1 DER encoding errors;
*   invalid public keys (e.g., RSA certificates with public exponent
    equal to 1); *or*
*   missing or incorrect extensions (e.g., TLS certificates with no subjectAltName extension, delegated OCSP responders without the id-pkix-ocsp-nocheck extension, partial/scoped CRLs that lack a distributionPoint in a critical issuingDistributionPoint extension).

CA operators MUST NOT issue certificates that have:    

*   duplicate issuer names and serial numbers (except that a Certificate
    Transparency pre-certificate is allowed to match the corresponding
    certificate); *or*
*   cRLDistributionPoints or OCSP authorityInfoAccess extensions for
    which no operational CRL or OCSP service exists.    
    
CA operators MUST NOT generate the key pairs for end entity certificates that have an
EKU extension containing the KeyPurposeIds id-kp-serverAuth or anyExtendedKeyUsage, unless the certificate is being issued to the CA itself.

Effective for certificates with a notBefore date of July 1, 2020 or later, 
end entity certificates MUST include an EKU extension containing KeyPurposeId(s) 
describing the intended usage(s) of the certificate, and the EKU extension MUST NOT 
contain the KeyPurposeId anyExtendedKeyUsage.

### 5.3 Intermediate Certificates ###

All certificates that are capable of being used to issue new certificates and that directly or transitively chain to a CA certificate included in Mozilla’s root store MUST be operated in accordance with this policy and MUST either be technically constrained or be publicly disclosed and audited.

A certificate is deemed as capable of being used to issue new
certificates if it contains an [X.509v3 basicConstraints extension][5280-6.1.4]
with the cA boolean set to true. 

A certificate is deemed to directly or transitively chain to a CA certificate included in Mozilla's root store if: 
(1)	the certificate's Issuer Distinguished Name matches (according to the name-matching algorithm specified in RFC 5280, section 7.1) the Subject Distinguished Name in a CA certificate or intermediate certificate that is in scope according to section 1.1 of this Policy, *and*
(2)	the certificate is signed with a Private Key whose corresponding Public Key is encoded in the SubjectPublicKeyInfo of that CA certificate or intermediate certificate.

Intermediate certificates created after January 1, 2019, with the exception of cross-certificates that share a private key with a corresponding root certificate:

*   MUST contain an EKU extension;
*   MUST NOT include the anyExtendedKeyUsage KeyPurposeId; *and*
*   MUST NOT include both the id-kp-serverAuth and id-kp-emailProtection KeyPurposeIds in the same certificate.

#### 5.3.1 Technically Constrained #### 
We encourage CA operators to technically constrain all intermediate
certificates. For an intermediate certificate to be considered technically
constrained, the certificate MUST include an [Extended Key Usage
(EKU)][5280-4.2.1.12] extension specifying the extended key usage(s) allowed for the type of end entity certificates that the
intermediate CA is authorized to issue. We also encourage CA operators to include only a single KeyPurposeID in the EKU extension of intermediate certificates. The anyExtendedKeyUsage
KeyPurposeId MUST NOT appear within this extension. 
 
If the intermediate CA certificate includes the id-kp-serverAuth extended key usage,
then to be considered technically constrained, the certificate MUST be Name Constrained as described in section
7.1.5 of version 1.3 or later of the [Baseline Requirements][BRs]. The id-kp-clientAuth EKU MAY also be present.
The conformance requirements defined in section 2.3 of this policy also apply to 
technically constrained intermediate certificates.

If the intermediate CA certificate includes the id-kp-emailProtection extended key
usage, then to be considered technically
constrained, it MUST include the Name Constraints X.509v3 extension with
constraints on rfc822Name, with at least one name in permittedSubtrees,
each such name having its ownership validated according to section
3.2.2.4 of the [Baseline Requirements][BRs]. The values id-kp-serverAuth and anyExtendedKeyUsage MUST NOT be present. id-kp-clientAuth MAY be present. Other values that the CA is allowed to use and are documented in the CA’s CP, CPS, or combined CP/CPS MAY be present.

#### 5.3.2 Publicly Disclosed and Audited ####

The operator of a CA certificate included in Mozilla’s root store MUST publicly disclose in the CCADB all CA certificates they issue that chain up to that CA certificate trusted in Mozilla’s root store that are technically capable of issuing working server or email certificates, including those CA certificates that share the same key pair whether they are self-signed, doppelgänger, reissued, cross-signed, or other roots. The CA operator with a certificate included in Mozilla’s root store MUST disclose  such CA certificate within one week of certificate creation, and before any such CA is allowed to issue certificates. Name-constrained CA certificates that are technically capable of issuing working server or email certificates that were exempt from disclosure in previous versions of this policy MUST be disclosed in the CCADB prior to July 1, 2022. 

All disclosure MUST be made freely available and without additional requirements, including, but not limited to, registration, legal agreements, or restrictions on redistribution of the certificates in whole or in part.

We recognize that technically constraining intermediate
certificates as described above may not be practical in some cases.
All certificates that are capable of being used to issue new
certificates, that are not technically constrained, and that
directly or transitively chain to a certificate included in
Mozilla’s root store MUST be audited in accordance with this policy. 
If the CA operator has a currently valid audit report at the time of creation 
of the intermediate certificate, then the new intermediate certificate MUST appear on the 
CA operator's next periodic audit reports.

### 5.4 Precertificates ###
The logging of a precertificate in a Certificate Transparency log is considered by Mozilla to be a binding intent to issue a final certificate, as described in [section 3.1 of RFC 6962][6962-3.1]. "Final certificate" means a certificate that is not a precertificate. Precertificates are in-scope for enforcing compliance with these requirements. A final certificate is "based on" a precertificate if they have the same serial and issuer, or they have the same serial and the final certificate's issuer matches the precertificate's issuer's issuer. Thus,
* it is mississuance to issue a final certificate based on a precertificate if they do not exactly match each other according to RFC 6962, section 3.1; *and*
* if a precertificate implies the existence of a final certificate that does not comply with this policy, it is considered misissuance of the final certificate, even if the certificate does not actually exist.

Effective October 1, 2022,

* a CA MUST be able to revoke a certificate presumed to exist, if revocation of the certificate is required under this policy, even if the final certificate does not actually exist; *and*
* a CA MUST provide CRL and OCSP services and responses in accordance with this policy for all certificates presumed to exist based on the presence of a precertificate, even if the certificate does not actually exist.

## 6. Revocation ##

CA operators MUST maintain an online 24x7 repository mechanism whereby
application software can automatically check online the current
status of all unexpired certificates issued by the CA.

For end entity certificates, CRLs MUST be updated and reissued at least
every seven days, and the value of the nextUpdate field MUST NOT be
more than ten days beyond the value of the thisUpdate field.

For end entity certificates, if the CA provides revocation information
via an Online Certificate Status Protocol (OCSP) service:

*   it MUST update that service at least every four days; 
*   responses MUST have a defined value in the nextUpdate field, and it
    MUST be no more than ten days after the thisUpdate field; *and*
*   the value in the nextUpdate field MUST be before or equal to the
    notAfter date of all certificates included within the
    BasicOCSPResponse.certs field or, if the certs field is omitted,
    before or equal to the notAfter date of the CA certificate which
    issued the certificate that the BasicOCSPResponse is for.

Section 4.9.12 of a CA operator's CPS (or, if applicable, the CP or CP/CPS) MUST clearly specify the methods that parties may use to demonstrate private key compromise.

### 6.1 TLS ###

For any certificate in a hierarchy capable of being used for 
TLS-enabled servers, CAs MUST revoke certificates that they have 
issued upon the occurrence of any event listed in the appropriate 
subsection of section 4.9.1 of the Baseline Requirements, 
according to the timeline defined therein. CAs MUST also revoke 
any certificates issued in violation of the then-current version 
of these requirements according to the timeline defined in 
section 4.9.1 of the Baseline Requirements.

#### 6.1.1 End Entity TLS Certificate CRLRevocation Reasons ####

This section applies to revocations that are performed after October 1, 2022. Revocation entries that appeared on a CRL prior to October 1, 2022, do NOT need to be changed as a result of this section.

When an end entity TLS certificate (i.e. a certificate capable of being used for TLS-enabled servers) is revoked for one of the reasons below, the specified CRLReason MUST be included in the reasonCode extension of the CRL entry corresponding to the end entity TLS certificate. When the CRLReason code is not one of the following, then the reasonCode extension MUST NOT be provided:
*   keyCompromise (RFC 5280 CRLReason #1);
*   privilegeWithdrawn (RFC 5280 CRLReason #9)**;
*   cessationOfOperation (RFC 5280 CRLReason #5);
*   affiliationChanged (RFC 5280 CRLReason #3); *or*
*   superseded (RFC 5280 CRLReason #4).

The CA operator's subscriber agreement for TLS end entity certificates MUST inform certificate subscribers about the revocation reason options listed above and [provide explanation about when to choose each option][Revocation-Reasons]. Tools that the CA operator provides to the certificate subscriber MUST allow for these options to be easily specified when the certificate subscriber requests revocation of their certificate, with the default value being that no revocation reason is provided (i.e. the default corresponds to the CRLReason “unspecified (0)” which results in no reasonCode extension being provided in the CRL). 

** The privilegeWithdrawn reasonCode does not need to be made available to the certificate subscriber as a revocation reason option, because the use of this reasonCode is determined by the CA operator and not the subscriber.

If the certificate is revoked for a reason not listed below, then the reasonCode extension MUST NOT be provided in the CRL.

**keyCompromise** 

The CRLReason keyCompromise MUST be used when one or more of the following occurs:
*   the CA operator obtains verifiable evidence that the certificate subscriber’s private key corresponding to the public key in the certificate suffered a key compromise;
*   the CA operator is made aware of a demonstrated or proven method that exposes the certificate subscriber’s private key to compromise;
*   there is clear evidence that the specific method used to generate the private key was flawed;
*   the CA operator is made aware of a demonstrated or proven method that can easily compute the certificate subscriber’s private key based on the public key in the certificate (such as a Debian weak key, see https://wiki.debian.org/TLSkeys); *or*
*   the certificate subscriber requests that the CA operator revoke the certificate for this reason, with the scope of revocation being described below.

The scope of revocation depends on whether the certificate subscriber has proven possession of the private key of the certificate. A CSR alone does not prove possession of the certificate’s private key for the purpose of initiating a revocation.
*   If anyone requesting revocation for keyCompromise has previously demonstrated or can currently [demonstrate possession of the private key of the certificate](https://wiki.mozilla.org/CA/Revocation_Reasons#Possession_of_Private_Key), then the CA operator MUST revoke all instances of that key across all subscribers. 
*   If the certificate subscriber requests that the CA operator revoke the certificate for keyCompromise, and has not previously demonstrated and cannot currently demonstrate possession of the associated private key of that certificate, the CA operator MAY revoke all certificates associated with that subscriber that contain that public key. The CA operator MUST NOT assume that it has evidence of private key compromise for the purposes of revoking the certificates of other subscribers, but MAY block issuance of future certificates with that key.

When the CA operator obtains verifiable evidence of private key compromise for a certificate whose CRL entry does not contain a reasonCode extension or has a reasonCode extension with a non-keyCompromise reason, the CA operator SHOULD update the CRL entry to enter keyCompromise as the CRLReason in the reasonCode extension.  Additionally, the CA operator SHOULD update the revocation date in a CRL entry when it is determined that the private key of the certificate was compromised prior to the revocation date that is indicated in the CRL entry for that certificate. 

Note: [Backdating the revocationDate field](https://wiki.mozilla.org/CA/Revocation_Reasons#Updating_CRL_Entries) is an exception to best practice described in RFC 5280 (section 5.3.2); however, this policy specifies the use of the revocationDate field to support TLS implementations that process the revocationDate field as the date when the certificate is first considered to be compromised.

Otherwise, the keyCompromise CRLReason MUST NOT be used.

**privilegeWithdrawn**

The CRLReason privilegeWithdrawn is intended to be used when there has been a subscriber-side infraction that has not resulted in keyCompromise, such as the certificate subscriber provided misleading information in their certificate request or has not upheld their material obligations under the subscriber agreement or terms of use.

Unless the keyCompromise CRLReason is being used, the CRLReason privilegeWithdrawn MUST be used when:
*   the CA operator obtains evidence that the certificate was misused;
*   the CA operator is made aware that the certificate subscriber has violated one or more of its material obligations under the subscriber agreement or terms of use;
*   the CA operator is made aware that a wildcard certificate has been used to authenticate a fraudulently misleading subordinate fully‐qualified domain name;
*   the CA operator is made aware of a material change in the information contained in the certificate; 
*   the CA operator determines or is made aware that any of the information appearing in the certificate is inaccurate; *or*
*   the CA operator is made aware that the original certificate request was not authorized and that the Subscriber does not retroactively grant authorization.

Otherwise, the privilegeWithdrawn CRLReason MUST NOT be used.

**cessationOfOperation**

The CRLReason cessationOfOperation is intended to be used when the website with the certificate is shut down prior to the expiration of the certificate, or if the subscriber no longer owns or controls the domain name in the certificate. This revocation reason is intended to be used in the following circumstances:
*   the certificate subscriber no longer controls, or is no longer authorized to use, all of the domain names in the certificate;
*   the certificate subscriber will no longer be using the certificate because they are discontinuing their website; *or*
*   the CA operator is made aware of any circumstance indicating that use of a fully‐qualified domain name or IP address in the certificate is no longer legally permitted (e.g. a court or arbitrator has revoked a domain name registrant’s right to use the domain name, a relevant licensing or services agreement between the domain name registrant and the applicant has terminated, or the domain name registrant has failed to renew the domain name).

Unless the keyCompromise CRLReason is being used, the CRLReason cessationOfOperation MUST be used when:
*   the certificate subscriber has requested that their certificate be revoked for this reason; *or* 
*   the CA operator has received verifiable evidence that the certificate subscriber no longer controls, or is no longer authorized to use, all of the domain names in the certificate. 

Otherwise, the cessationOfOperation CRLReason MUST NOT be used.

**affiliationChanged** 

The CRLReason affiliationChanged is intended to be used to indicate that the subject's name or other subject identity information in the certificate has changed, but there is no cause to suspect that the certificate’s private key has been compromised. 

Unless the keyCompromise CRLReason is being used, the CRLReason affiliationChanged MUST be used when:
*   the certificate subscriber has requested that their certificate be revoked for this reason; *or* 
*   the CA operator has replaced the certificate due to changes in the certificate’s subject information and the CA has not replaced the certificate for the other reasons: keyCompromise, superseded, cessationOfOperation, or privilegeWithdrawn. 

Otherwise, the affiliationChanged CRLReason MUST NOT be used.

**superseded**

The CRLReason superseded is intended to be used to indicate when:
*   the certificate subscriber has requested a new certificate to replace an existing certificate; or
*   the CA operator obtains reasonable evidence that the validation of domain authorization or control for any fully‐qualified domain name or IP address in the certificate should not be relied upon; *or*
*   the CA operator has revoked the certificate for compliance reasons such as the certificate does not comply with this policy, the CA/Browser Forum's Baseline Requirements, or the CA operator’s CP or CPS.

Unless the keyCompromise CRLReason is being used, the CRLReason superseded MUST be used when:
*   the certificate subscriber has requested that their certificate be revoked for this reason; *or* 
*   the CA operator has revoked the certificate due to domain authorization or compliance issues other than those related to keyCompromise or privilegeWithdrawn. 

Otherwise, the superseded CRLReason MUST NOT be used.

### 6.2 S/MIME ###

For any certificate in a hierarchy capable of being used for 
S/MIME, CA operators MUST revoke certificates upon the occurrence of 
any of the following events:

1. the subscriber indicates that the original certificate request 
was not authorized and does not retroactively grant authorization;
2. the CA operator obtains reasonable evidence that the subscriber’s 
private key (corresponding to the public key in the certificate) 
has been compromised or is suspected of compromise;
3. the CA operator obtains reasonable evidence that the certificate 
has been used for a purpose outside of that indicated 
in the certificate or in the CA operator's subscriber agreement;
4. the CA operator receives notice or otherwise becomes aware that a 
subscriber has violated one or more of its material obligations 
under the subscriber agreement;
5. the CA operator receives notice or otherwise becomes aware of any circumstance 
indicating that use of the email address in the certificate 
is no longer legally permitted;
6. the CA operator receives notice or otherwise becomes aware of a material change 
in the information contained in the certificate;
7. a determination that the certificate was not issued in accordance 
with the CA operator’s Certificate Policy or Certification Practice Statement;
8. the CA operator determines that any of the information 
appearing in the certificate is not accurate;
9. the CA operator ceases operations for any reason and has not arranged 
for another CA operator to provide revocation support for the certificate;
10. the CA private key used in issuing the certificate is suspected 
to have been compromised;
11. such additional revocation events as the CA operator publishes 
in its policy documentation; *or*
12. the certificate was issued in violation of the then-current 
version of these requirements.

## 7. Root Store Changes ##

Changes that are motivated by a security concern such as certificate
misissuance or a root or intermediate compromise MUST be treated as a
security-sensitive, and a [secure bug filed in Bugzilla][Sec-Bugs].

### 7.1 Inclusions ###

We will determine which CA certificates are included in Mozilla's root store
based on the risks of
such inclusion to typical users of our products. We will consider adding
additional CA certificates to the default certificate set upon request only by
an authorized representative of the subject CA. We will make such decisions
through a public process.

We will not charge any fees to have a CA operator’s certificate(s)
included in Mozilla's root store.

We reserve the right to not include certificates from a particular CA operator in
our root store. This includes (but is not limited to) cases
where we believe that a CA operator has caused undue risks to users’
security, e.g. by knowingly issuing certificates without the knowledge of the
entities whose information is referenced in those certificates ('MITM certificates'). 
Mozilla is under no obligation to explain the reasoning behind any inclusion decision.

Before being included, CA operators MUST provide evidence that their CA certificates fully comply with the current Mozilla Root Store Requirements and Baseline Requirements, and have continually, from the time of CA private key creation, complied with the then-current Mozilla Root Store Policy and Baseline Requirements. 

To request that its certificate(s) be added to Mozilla's root store, a CA operator
SHOULD submit a formal request by submitting a [bug report][CA-Cert-Bug]
into the mozilla.org Bugzilla system, filed against the "CA
Certificate Root Program" component of the "NSS" product. Mozilla’s wiki
page, "[Applying for root inclusion in Mozilla products][How-To-Apply]", provides
further details about how to submit a formal request. The request
MUST be made by an authorized representative of the subject CA operator, and
MUST include the following:

1.  the certificate data (or links to the data) for the CA
    certificate(s) requested for inclusion;
2.  for each CA certificate requested for inclusion, whether or not
    the CA issues certificates for each of the following purposes
    within the certificate hierarchy associated with the CA
    certificate:
    * TLS-enabled servers
    * digitally-signed and/or encrypted email;
3.  for each CA certificate requested for inclusion, whether the CA
    issues Extended Validation certificates within the certificate hierarchy
    associated with the CA certificate and, if so, the EV policy
    OID associated with the CA certificate;
4.  a Certificate Policy and Certification Practice Statement (or
    links to a CP and CPS) or equivalent disclosure document(s)
    for the CA or CAs in question; 
5.  an auditor-witnessed root key generation ceremony report and contiguous 
    period-of-time audit reports performed thereafter no less frequently than 
    annually; *and*
6.  information as to how the CA operator has fulfilled the requirements
    stated above regarding its verification of certificate signing
    requests and its conformance to a set of acceptable operational
    criteria.

We will reject requests where the CA operator does not provide such
information within a reasonable period of time after submitting its
request.

### 7.2 Updates ###

Changes MAY be made to CA certificates that are included in
Mozilla's root store as follows:

1.  enabling a trust bit in a CA certificate that is currently
    included, MAY only be done after careful consideration of the
    CA operator’s current policies, practices, and audits,
    and MAY be requested by a representative of the CA or a
    representative of Mozilla by submitting a bug report into the
    mozilla.org Bugzilla system, as described in Mozilla’s wiki
    page, "[Applying for root inclusion in Mozilla products][How-To-Apply]";
2.  enabling EV in a CA certificate that is currently included,
    MAY only be done after careful consideration of the CA operator’s current
    policies, practices, and audits,
    and MAY be requested by a representative of the CA operator or a
    representative of Mozilla by submitting a bug report into the
    mozilla.org Bugzilla system, as described in Mozilla’s wiki
    page, "[Applying for root inclusion in Mozilla products][How-To-Apply]";
3.  disabling a CA certificate is the act of turning off one or more of the
    trust bits (websites or email), and MAY be
    requested by a representative of the CA operator or a representative of
    Mozilla by submitting a bug report into the mozilla.org Bugzilla
    system, as described in the [Root Change Process][Root-Changes]; *and*
4.  a representative of the CA operator or a representative of Mozilla MAY
    request that a CA certificate be removed by submitting a bug
    report into the mozilla.org Bugzilla system, as described in the
    [Root Change Process][Root-Changes].

### 7.3 Removals ###

Mozilla MAY, at its sole discretion, decide to disable (partially or fully) or
remove a certificate at any time and for any reason. This MAY happen
immediately or on a planned future date. Mozilla will
disable or remove a certificate if the CA operator demonstrates ongoing or
egregious practices that do not maintain the expected level of service
or that do not comply with the requirements of this policy.

Mozilla will take any steps we deem appropriate to protect our users
if we learn that a CA operator has knowingly or intentionally mis-issued one
or more certificates. This MAY include, but is not limited to,
disablement (partially or fully) or removal of all the CA operator’s
certificates from Mozilla’s root store.

The category of mis-issued certificates includes (but is not limited to) those
issued to someone who should not have received them, those containing
information which was not properly validated, those having incorrect technical
constraints, and those using algorithms other than those permitted.

A failure to provide notifications or updates in the CCADB or
as otherwise required in a timely manner SHALL also be grounds for
disabling a CA operator’s root certificates or removing them from Mozilla's root
store. For this policy and the CCADB policies, "a timely manner" means
within 30 days of when the appropriate data or documentation becomes
available to the CA operator, unless a Mozilla policy document specifies a different
rule.

If Mozilla disables or removes a CA operator’s certificate(s) from Mozilla’s
root store based on a CA operator’s actions (or failure to act) that are
contrary to this policy, Mozilla will publicize 
that fact (for example, in newsgroups on the
news.mozilla.org server, and on our websites) and MAY also alert 
relevant news or government organizations such as US-CERT.

## 8. CA Operational Changes ##

CA operators SHALL NOT assume that trust is transferable. All CA operators whose certificates
are included in Mozilla's root store MUST [notify Mozilla][Email-Us] before:

* ownership or control of the CA’s certificate(s) changes;
* an organization other than the CA operator obtains control of an unconstrained 
intermediate certificate (as defined in section 5.3 of this policy) that 
directly or transitively chains to a certificate included in Mozilla's root store - see [Process for non-Technically-Constrained Subordinate CAs][Process-for-External-CAs];
* ownership or control of the CA’s operations changes; *or*
* there is a change in the CA's operations that could affect the CA's ability to comply with the requirements of this Policy.

CA operators SHOULD err on the side of notification if there is any doubt. Mozilla will
normally keep commercially sensitive information confidential. Throughout any
change, CA operations MUST continue to meet the requirements of this policy. If
one of the above events occurs, Mozilla MAY require additional audit(s) as a
condition of remaining in the root store. CA operators are encouraged to notify Mozilla in
advance in order to avoid unfortunate surprises.

In addition, one or more of the following sections MAY apply.

### 8.1 Change in Legal Ownership ###

This section applies when one company buys or takes a controlling stake in
a CA or CA operator, or when an organization obtains control of a CA key pair that is 
within the scope of Mozilla's root store, unless it is constrained in 
compliance with section 5.3.1 of this policy.

Mozilla MUST be notified of any resulting changes in the CA operator's CP, CPS, or combined CP/CPS.

If the receiving or acquiring company is new to the Mozilla root store, 
it MUST demonstrate compliance with the entirety of this policy. There
MUST be a public discussion regarding its admittance to the root store. 
If Mozilla reaches a positive conclusion after public discussion, then the affected certificate(s) MAY remain in the root store. If the entire 
CA operation is not included in the scope of the transaction, issuance is not
permitted until the discussion has been resolved with a positive conclusion.

### 8.2 Change in Operational Personnel ###

This section applies when operation of a CA certificate that is within the 
scope of Mozilla's root store and not constrained in compliance with section 
5.3.1 of this policy is transferred to a different organization, 
whether by acquisition or contract.

The transferor MUST ensure that the transferee is able to fully comply with
this policy. The transferor will continue to be responsible for the root
certificate's private key until Mozilla has been provided with an audit
statement (or opinion letter) confirming successful transfer of the root
certificate and key. Issuance MUST NOT occur until the transferee
has provided all the information required by the CCADB, and demonstrated to
Mozilla that they have all the appropriate audits, CP/CPS documents, and other
systems in place.

The transferor MUST notify Mozilla about any necessary changes to EV status or
trust bits in Mozilla's root store. If the transferee will be technically capable of issuing EV certificates, the transferor MUST confirm that the
transferee has or will get the relevant audits before issuing EV certificates.

### 8.3 Change in Secure Location ###

This section only applies when section 8.1 and/or section 8.2 applies, and when the
cryptographic hardware related to a CA certificate that is within the scope of 
Mozilla's root store and not constrained in compliance with section 
5.3.1 of this policy is consequently moved from one secure location to another.

This policy and the relevant WebTrust or ETSI requirements apply at all times,
even during the physical relocation of a CA's online operations to a new data
center and moving parts of an offline root certificate from one location to
another. As such, a CA operator MUST always ensure that physical access to CA equipment
is limited to authorized individuals, the equipment is operated under multiple
person control, and unauthorized CA system usage is able to be detected at all
times. The auditor MUST confirm that there are appropriate procedures in place
to ensure that the requirements are met and that those procedures are followed.

The following steps MUST be taken by the organization(s) concerned:

* ensure that annual audit statements are current;
* notify Mozilla of the pending change;
* create a transfer plan (and legal agreement if more than one organization is
involved) and have it reviewed by the auditors;
* stop new certificate issuance at the current site before the transfer begins;
* have an audit performed at the current site to confirm when the root
certificate is ready for transfer, and ensure that key material is
properly secured;
* have the transfer ceremony witnessed by auditors and video recorded,
with a physical exchange of the HSM or ciphertext containing the associated key
material and certificates, and the multi-party authorization keys;
* perform an audit at the new site to confirm that the transfer was successful,
that the private key remained secure throughout the transfer, and that the root
certificate is ready to resume issuance. This requirement MAY be met by
including the transferred root certificate and key in the new owner's regular
audits or by getting a point-in-time audit; *and*
* send links to the updated CP, CPS, and the updated audit statements, opinion
letter, or point-in-time audit statement to Mozilla.

The regular annual audit statements MUST still happen in a timely manner.

The organization(s) concerned MUST immediately [send a security report to
Mozilla][Sec-Bugs] if a problem occurs.

## 8.4 Externally-Operated Subordinate CAs

The operator of a root CA certificate that is included in Mozilla’s root store is at all times completely and ultimately accountable for every certificate signed under that root CA certificate, whether directly or through subordinate CAs or cross-certified CAs. The operator of the root CA certificate SHALL ensure that the operator of each subordinate or cross-certified CA fully and continually adheres to this policy.

The root CA operator MUST complete Mozilla’s [Process for non-Technically-Constrained Subordinate CAs][Process-for-External-CAs] (including successful review and approval by Mozilla) before a new externally-operated subordinate CA begins issuing certificates under any of the following conditions:

* the subordinate CA operator will obtain a unconstrained (per section 5.3.1 of this policy) CA certificate, and the subordinate CA operator is not approved by Mozilla to issue the type of certificates (email, TLS, or EV TLS), which they will be able to issue under the new CA certificate;
* the root CA operator is cross-signing a CA certificate of a CA operator who is not currently in Mozilla’s root store; *or*
* the root CA operator is cross-signing a CA certificate of another CA operator who is currently in Mozilla’s root store, but the other CA operator has not been approved for the same trust bits (email or websites) or EV, and those trust bits or EV will be recognized under the cross-signed certificate that it will be receiving.

We reserve the right to not approve subordinate CA certificates. This includes (but is not limited to) cases where we believe that approval of a subordinate CA operator would cause undue risks to users’ security. Mozilla is under no obligation to explain the reasoning behind such decisions.

When any of the following conditions apply, the root CA operator is not required to perform Mozilla’s [Process for non-Technically-Constrained Subordinate CAs][Process-for-External-CAs] before the subordinate CA certificate begins issuing certificates:

* the subordinate CA will be operated directly by the root CA operator under the exact same policies and practices of the root CA operator and within the same scope of audit reporting, and no new organizations will be involved in the management or operation of the CA;
* the CA certificate is technically constrained as described in section 5.3.1 of this policy;
* the subordinate CA operator:
  - has previously undergone the [Process for non-Technically-Constrained Subordinate CAs][Process-for-External-CAs]; 
  - has been approved for the type of certificates to be issued (email, TLS, or EV TLS); *and*
  - will operate under the same policies and practices as the previous review, and under the same scope of audit reporting as the prior subordinate CA certificate. (Newer versions of policies and practices MAY be used, provided that the subordinate CA operator follows the same versions of the policies for both the existing and new CA certificates.)
* as of June 1, 2022, the subordinate CA operator was already trusted for issuing the same type of certificates under an existing subordinate CA certificate that directly or transitively chains to a certificate included in Mozilla’s root store; *or*
* the root CA operator is cross-signing a CA certificate of another CA operator that is currently in Mozilla’s root store, and that other CA operator:
  - will only be able to issue the same type of certificate (email, TLS, or EV TLS) that they are already approved for in Mozilla’s root store; *and* 
  - will operate both the cross-signed certificate and their CA certificate(s) under the same policies, practices, and scope of audit that their CA certificate was approved for. Newer versions of policies and practices MAY be used, provided that the cross-signed CA operator follows the same versions of the policies for both the cross-signed certificate and their CA certificate(s).

-----

Any copyright in this document is [dedicated to the Public Domain][CC-0].

[Email-Us]:                 mailto:certificates@mozilla.org
[Bugzilla]:                 https://bugzilla.mozilla.org
[Trademark-Policy]:         https://www.mozilla.org/foundation/trademarks/policy/
[CA-Cert-Module]:           https://wiki.mozilla.org/Modules/Activities#CA_Certificates
[CA-Policy-Module]:         https://wiki.mozilla.org/Modules/Activities#Mozilla_CA_Certificate_Policy
[Gov-Module]:               https://wiki.mozilla.org/Modules/Firefox_Technical_Leadership
[MDSP]:                     https://groups.google.com/a/mozilla.org/g/dev-security-policy 
[EVGLs]:                    https://cabforum.org/extended-validation/
[BRs]:                      https://cabforum.org/baseline-requirements-documents/
[NSRs]:                     https://cabforum.org/network-security-requirements/
[ETSI-319-411-1]:           https://www.etsi.org/deliver/etsi_en/319400_319499/31941101/01.03.01_60/en_31941101v010301p.pdf
[ETSI-319-411-2]:           https://www.etsi.org/deliver/etsi_en/319400_319499/31941102/02.04.01_60/en_31941102v020401p.pdf
[WebTrust-2.2.1]:           https://www.cpacanada.ca/-/media/site/operational/ms-member-services/docs/webtrust/wt100awebtrust-for-ca-221-110120-finalaoda.pdf
[WebTrust-BRs]:             https://www.cpacanada.ca/-/media/site/operational/ms-member-services/docs/webtrust/wt100bwtbr-25-110120-finalaoda.pdf
[WebTrust-For-CAs]:         https://www.cpacanada.ca/en/business-and-accounting-resources/audit-and-assurance/overview-of-webtrust-services/principles-and-criteria
[WebTrust-EV]:              https://www.cpacanada.ca/-/media/site/operational/ms-member-services/docs/webtrust/wt100cwtev-173-110120-finalaoda.pdf
[CC-BY]:                    https://creativecommons.org/licenses/by/4.0/
[CC-BY-SA]:                 https://creativecommons.org/licenses/by-sa/4.0/
[CC-BY-ND]:                 https://creativecommons.org/licenses/by-nd/4.0/
[CC-0]:                     https://creativecommons.org/publicdomain/zero/1.0/
[CCADB-Policy]:             https://www.ccadb.org/policy
[CCADB-Revocation]:         https://www.ccadb.org/cas/fields#revocation-information
[5280-6.1.4]:               http://tools.ietf.org/html/rfc5280#section-6.1.4
[5280-4.2.1.12]:            http://tools.ietf.org/html/rfc5280#section-4.2.1.12
[6962-3.1]:                 https://datatracker.ietf.org/doc/html/rfc6962#section-3.1
[CA-Cert-Bug]:              https://bugzilla.mozilla.org/enter_bug.cgi?product=NSS&component=CA%20Certificate%20Root%20Program
[How-To-Apply]:             https://wiki.mozilla.org/CA/Application_Process
[Root-Changes]:             https://wiki.mozilla.org/CA/Certificate_Change_Process
[Sec-Bugs]:                 https://bugzilla.mozilla.org/enter_bug.cgi?product=NSS&component=CA%20Certificate%20Compliance&groups=crypto-core-security
[Policy-Update-Process]:    https://wiki.mozilla.org/CA/Updating_Root_Store_Policy
[Policy-Archive]:           https://wiki.mozilla.org/CA/Root_Store_Policy_Archive
[Incident-Report]:          https://wiki.mozilla.org/CA/Responding_To_An_Incident
[Capable-of-EV]:            https://wiki.mozilla.org/CA/EV_Processing_for_CAs#EV_TLS_Capable
[Audited-Location]:         https://wiki.mozilla.org/CA/Audit_Statements#Audited_Locations 
[Auditor-Qualifications]:   https://wiki.mozilla.org/CA/Audit_Statements#Auditor_Qualifications
[Process-for-External-CAs]: https://wiki.mozilla.org/CA/External_Sub_CAs
[ACAB'c]:                   https://www.acab-c.com/members/
[WebTrust Practitioners]:                 https://www.cpacanada.ca/en/business-and-accounting-resources/audit-and-assurance/overview-of-webtrust-services/licensed-webtrust-practitioners-international
[Revocation-Reasons]:        https://wiki.mozilla.org/CA/Revocation_Reasons

