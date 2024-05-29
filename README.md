# CISA Vulnrichment

The CISA Vulnrichment project is the public repository of CISA's enrichment of public CVE records through CISA's ADP (Authorized Data Publisher) container. In this phase of the project, CISA is assessing new and recent CVEs and adding key [SSVC](https://www.cisa.gov/stakeholder-specific-vulnerability-categorization-ssvc) decision points. Once scored, some higher-risk CVEs will also receive enrichment of [CWE](https://cwe.mitre.org/), [CVSS](https://www.first.org/cvss/), and [CPE](https://csrc.nist.gov/publications/search?keywords-lg=CPE) data points, where possible.

Producers and consumers of this CVE data should already be familiar with the current [CVE Record Format](https://www.cve.org/AllResources/CveServices#CveRecordFormat) and can access this data in the normal ways, including the [GitHub API](https://docs.github.com/en/rest/quickstart).

## How it works

First, CISA will take each CVE through an SSVC scoring process.

Next, for those CVEs that are rated as "Total Technical Impact," "Automatable," or have "Exploitation" values of "Proof of Concept" or "Active Exploitation," further analysis will be conducted. CISA will determine if there is enough information to assert a specific CWE identifier, a CVSS score, or a CPE string.

For those CVEs that do not already have these fields populated by the originating CNA, CISA will populate the associated ADP container with those values when there is enough supporting evidence to do so.  In some cases, CISA may also add reference URLs. At no point will CISA overwrite the originating CNA's data in the original CNA container in the CVE record.

### An example CVE

Take a look at [CVE-2024-3931](2024/3xxx/CVE-2024-3931.json), which was fairly recently assigned, but otherwise chosen at random.

For this CVE, the CISA ADP starts on [line 119](2024/3xxx/CVE-2024-3931.json#L119). CISA has determined that a proof-of-concept exploit is available for this vulnerability, so that's noted on [line 130](2024/3xxx/CVE-2024-3931.json#L130). CISA has also enriched this CVE ID with a CPE string on [line 147](2024/3xxx/CVE-2024-3931.json#L147) , `"cpe2.3:a:totara:enterprise_lms:*:*:*:*:*:*:*:*"`. However, since the originating CNA already provided CWE and CVSS data, CISA has not updated or copied those values into the ADP container.

### A note about CPE

Of all the enriched data types, consistent and universal software identification, currently in the form of CPE, is the most difficult to accurately generate and maintain. CISA will assess and improve CPE enrichment as this project progresses. There are three sets of CPE data, all of which conform to the [CPE Specification](https://nvlpubs.nist.gov/nistpubs/Legacy/IR/nistir7695.pdf):

1. The official [NVD CPE Dictionary](https://nvd.nist.gov/products/cpe)
2. CPE entries that are present in NVD data but not in the Dictionary
3. CPE entries created by CISA

### A note about updated CVE entries

Since the CISA ADP is committed to encouraging CNAs to Do The Right Thing and provide their own CWE, CVSS, and CPE metrics, if a CVE entry is updated to include those metrics after the CISA ADP has made their assessment, the CISA ADP will drop its own assessments from the CVE entry. This approach will reduce duplicate (and conflicting) data within the CVE record. In the rare event that there is a CWE, CVSS, or CPE string provided by the originating CNA *and* the CISA ADP, this should be treated as an error in the CISA ADP container -- the originating CNA's data should take precedence for any decision making.

## Learn more

This project is expected to evolve quickly over the next several weeks, so please keep an eye on this [README.md](https://github.com/cisagov/vulnrichment/blob/develop/README.md).

## Issues and Pull Requests

We want to hear from you, the IT cybersecurity professional community, about our ADP enrichment! If you see something, please feel free to say something in the [Issues](https://github.com/cisagov/vulnrichment/issues), or even better, open a [Pull Request](https://github.com/cisagov/vulnrichment/pulls) with your suggested fix. Note that if you have an issue with the original CVE data, you are encouraged to take that issue up with the [responsible CNA](https://www.cve.org/PartnerInformation/ListofPartners) directly.
