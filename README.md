# CISA Vulnrichment

The CISA Vulnrichment project is the public repository of CISA's enrichment of public CVE records through CISA's ADP (Authorized Data Publisher) container. In this phase of the project, CISA is assessing new and recent CVEs and adding key [SSVC](https://www.cisa.gov/stakeholder-specific-vulnerability-categorization-ssvc) decision points. Once scored, some higher-risk CVEs will also receive enrichment of [CWE](https://cwe.mitre.org/), [CVSS](https://www.first.org/cvss/), and [CPE](https://csrc.nist.gov/publications/search?keywords-lg=CPE) data points, where possible.

Producers and consumers of this CVE data should already be familiar with the current [CVE Record Format](https://www.cve.org/AllResources/CveServices#CveRecordFormat) and can access this data in the normal ways, including the [GitHub API](https://docs.github.com/en/rest/quickstart) and the [CVE Services API](https://cveawg-test.mitre.org/api-docs/).

This project is under active development, so keep an eye on this [README.md](https://github.com/cisagov/vulnrichment/blob/develop/README.md) for updates.

## How it works

First, CISA will take each CVE through an SSVC scoring process.

Next, for those CVEs that are rated as "Total Technical Impact," "Automatable," or have "Exploitation" values of "Proof of Concept" or "Active Exploitation," further analysis will be conducted. CISA will determine if there is enough information to assert a specific CWE identifier, a CVSS score, or a CPE string. In some cases, CISA will provide these metrics even to vulnerabilities that do not rate as high risk on any of these decision points.

For those CVEs that do not already have these fields populated by the originating CNA, CISA will populate the associated ADP container with those values when there is enough supporting evidence to do so. At no point will CISA overwrite the originating CNA's data in the original CNA container in the CVE record.

This [flowchart](assets/vulnrichment_big.dot.svg) ([dot source](assets/vulnrichment_big.dot)) illustrates the Vulnrichment process. Please note that the details in the flowchart are subject to change as Vulnrichment processes are refined.

### Some example CVEs

Let's take a moment to look at some CVE records for each kind of vulnrichment you can expect from the CISA ADP.

All of the CVEs taken as examples below were chosen at random among those that fit the demonstrated criteria.

#### SSVC decision points

Every CVE analyzed by the CISA ADP will have three SSVC decision points listed. For these examples, we'll take a look at CVE-2024-34974, CVE-2024-25522, and CVE-2024-35057. We'll also look at CVE-2024-33666, which is a low-risk SSVC score.

CVE-2024-25522 has a "poc" value for Exploit on [line 82](2024/25xxx/CVE-2024-25522.json#L82) indicating there was a public proof-of-concept available at the time of analysis:

```json
                "options": [
                  {
                    "Exploitation": "poc"
                  },
                  {
                    "Automatable": "yes"
                  },
                  {
                    "Technical Impact": "total"
                  }
                ]
```

CVE-2024-34974 has a "yes" value for "Automatable" on [line 75](2024/34xxx/CVE-2024-34974.json#L75) indicating that an attacker could generally exploit this vulnerability at will, without having to worry about recon, weaponization, delivery, or exploitation prevention techniques.

```json
                "options": [
                  {
                    "Exploitation": "none"
                  },
                  {
                    "Automatable": "yes"
                  },
                  {
                    "Technical Impact": "partial"
                  }
                ]
```

CVE-2024-35057 has a "total" value for "Technical Impact" on [line 78](2024/35xxx/CVE-2024-35057.json#L78) indicating that the exploiting this vulnerability generally will give the attacker total control over the impacted software.

```json
                "options": [
                  {
                    "Exploitation": "none"
                  },
                  {
                    "Automatable": "no"
                  },
                  {
                    "Technical Impact": "total"
                  }
                ]
```

#### KEV flag

For those CVEs that are on the [KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog), the CISA ADP will add a [KEV block](assets/kev_metrics_schema-1.0.json). For those that aren't, no update will occur.

CVE-2024-4947 is one such CVE, and contains the KEV block starting at [line 102](2024/4xxx/CVE-2024-4947.json#L102-L107):

```json
            "other": {
              "type": "kev",
              "content": {
                "dateAdded": "2024-05-20",
                "reference": "https://www.cisa.gov/known-exploited-vulnerabilities-catalog?search_api_fulltext=CVE-2024-4947"
              }
```

#### CWE identifiers

CVE-2024-3477 is an example CVE which the originating CNA did not provide a CWE, and a CISA analyst was able to determine one from the context of the vulnerability information available. That metric starts on [line 98](2024/3xxx/CVE-2024-3477.json#L98-L109) under the `problemTypes` node:

```json
        "problemTypes": [
          {
            "descriptions": [
              {
                "lang": "en",
                "type": "CWE",
                "cweId": "CWE-352",
                "description": "CWE-352 Cross-Site Request Forgery (CSRF)"
              }
            ]
          }
        ]
```

#### CVSS calculations

CVE-2024-0043 is an example CVE that had a CVSS calculation added by CISA, starting on [line 64](2024/0xxx/CVE-2024-0043.json#L64-L77). Again, this is based on the context of the vulnerability information available at the time of analysis.

```json
            "cvssV3_1": {
              "scope": "UNCHANGED",
              "version": "3.1",
              "baseScore": 7.8,
              "attackVector": "LOCAL",
              "baseSeverity": "HIGH",
              "vectorString": "CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H",
              "integrityImpact": "HIGH",
              "userInteraction": "REQUIRED",
              "attackComplexity": "LOW",
              "availabilityImpact": "HIGH",
              "privilegesRequired": "NONE",
              "confidentialityImpact": "HIGH"
            }
```

#### CPE strings

CVE-2024-1347 is an example CVE that had a CPE string added by CISA, starting on [line 153](2024/1xxx/CVE-2024-1347.json#L153-L155).

```json
            "cpes": [
              "cpe:2.3:a:gitlab:gitlab:*:*:*:*:*:*:*:*"
            ]
```

We have more to say about CPE strings below.

#### No additional updates

CVE-2024-2905 is an example CVE that already had CWE, CVSS, and CPE metrics (see [here](2024/2xxx/CVE-2024-2905.json)) when CISA performed the SSVC triage step, and it's not on the KEV, so nothing more needed to be added. Good job, Red Hat CNA!

### A note about CPE

Of all the enriched data types, consistent and universal software identification, currently in the form of CPE, is the most difficult to accurately generate and maintain. CISA will assess and improve CPE enrichment as this project progresses. There are three sets of CPE data, all of which conform to the [CPE Specification](https://nvlpubs.nist.gov/nistpubs/Legacy/IR/nistir7695.pdf):

1. The official [NVD CPE Dictionary](https://nvd.nist.gov/products/cpe)
2. CPE entries that are present in NVD data but not in the Dictionary
3. CPE entries created by CISA

### A note about updated CVE entries

Since the CISA ADP is committed to encouraging CNAs to Do The Right Thing and provide their own CWE, CVSS, and CPE data, if a CVE entry is updated to include that data after the CISA ADP has made their assessment, the CISA ADP will drop its own assessments from the CVE entry. This approach will reduce duplicate (and conflicting) data within the CVE record. In the rare event that there is CWE, CVSS, or CPE data provided by the originating CNA *and* the CISA ADP, this should be treated as an error in the CISA ADP container, and the originating CNA's data should take precedence.

## Issues and Pull Requests

We want to hear from you, the IT cybersecurity professional community, about Vulnrichment and ADP! If you see something, please feel free to say something in the [Issues](https://github.com/cisagov/vulnrichment/issues), or even better, open a [Pull Request](https://github.com/cisagov/vulnrichment/pulls) with your suggested fix. Note that if you have an issue with the data from the CNA container, you are encouraged to take that issue up with the [responsible CNA](https://www.cve.org/PartnerInformation/ListofPartners) directly.
