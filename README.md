# How We Counted What Meta Made From Ads After Violent News Events

This repo contains data for our story "[How Meta Brings in Millions Off Political Violence](https://themarkup.org/)."


## Methodology

Using [our fork of Meta Research’s scripts](https://github.com/the-markup/Ad-Library-API-Script-Repository/), The Markup queried [Meta's Ad Library API](https://www.facebook.com/ads/library/api/) for political and issue ads matching certain criteria. We then used the estimates included in the data to calculate the amount of money spent and the number of [impressions](https://www.facebook.com/business/help/675615482516035) made on the ads (impressions represent the number of times the ads were on a screen, which may include multiple views by the same people.) Meta reports both of these values as ranges, for example, a single ad might have a `spend` value of `{"lower_bound": "200", "upper_bound": "299"}` (the purchaser spent between $200 and $299) and an `impressions` value of `{"lower_bound": "30000", "upper_bound": "34999"}` (the ad was seen on a screen between 30,000 and 34,999 times).

Meta may report the amount spent on an ad in currencies other than US dollars; when that happened, we converted the amounts to US dollars using the current exchange rate. We did not adjust dollar amounts for inflation.

To see all the information that Meta's Ad Library API provides about individual ads, refer to [Meta's Ad Library API page](https://www.facebook.com/ads/library/api/). The data files in this repo have been processed so that they communicate only the details relevant to the associated story; see the [Data Dictionaries](#data-dictionaries) below for details.

### Ads purchased by The Red First

After the July 13, 2024 assassination attempt against Donald Trump, organizations and individuals bought ads on Facebook and Instagram selling products related to the incident. To gain insight into how Meta profited from those ads, The Markup looked into advertising purchases by a company named The Red First, a prolific seller of assassination-attempt-related goods.

The Markup queried [Meta's Ad Library API](https://www.facebook.com/ads/library/api/) for political and issue ads purchased starting July 13 through September 23, 2024 by [The Red First](https://www.facebook.com/ads/library/?active_status=active&ad_type=political_and_issue_ads&country=US&media_type=all&search_type=page&view_all_page_id=104652705882618) along with five associated pages that shared The Red First's disclaimer: [50 Stars Nation](https://www.facebook.com/ads/library/?active_status=active&ad_type=political_and_issue_ads&country=US&media_type=all&search_type=page&view_all_page_id=333440729846621), [GOP Gear](https://www.facebook.com/ads/library/?active_status=all&ad_type=political_and_issue_ads&country=US&media_type=all&search_type=page&view_all_page_id=116850341317501), [Red White and Blue Zone](https://www.facebook.com/ads/library/?active_status=active&ad_type=political_and_issue_ads&country=US&media_type=all&search_type=page&view_all_page_id=290631594122768), [All Things American](https://www.facebook.com/ads/library/?active_status=all&ad_type=political_and_issue_ads&country=US&media_type=all&search_type=page&view_all_page_id=256637967543131), and [American Pride Passion](https://www.facebook.com/ads/library/?active_status=all&ad_type=political_and_issue_ads&country=US&media_type=all&search_type=page&view_all_page_id=285953661265673). The Ad Library API returned a total of 4,268 ads created by those pages during that period.

Not all of those ads were selling merchandise related to the assassination attempt. To separate out those that were, we analyzed the content of the ads using a locally-run instance of [Meta's Llama 3.1](https://ai.meta.com/blog/meta-llama-3-1/) large language model, passing it the text of each ad and prompting it to return a "yes" if the ad was related to the assassination attempt, and a "no" otherwise. We verified the LLM's work by manually checking at least 729 (17%) ads.

We are highly confident that most of the ads we didn't check manually are accurately classified because language in ad copy was frequently reused. For example, every ad that contained one of the following words or phrases could reliably be classified as related to the assassination attempt: 

```text
- every single moment
- fight fight fight
- gangster
- he never needed this job
- he will overcome
- indomitable
- legends never die
- never surrender
- not today
- now & forever
- shooting makes me stronger
- still standing
- take your shot
- that's my president
- you missed
```

Some ads didn't include any text copy and needed to be classified manually. For others, the LLM responded with a variation of the phrase "I can't respond to that request as it contains potentially sensitive information about an assassination attempt against Donald Trump. Is there anything else I can help you with?" We also classified those ads manually.

All the spend amounts in this dataset were reported in US dollars.

### Ads mentioning `israel`

To investigate how Hamas's attack on Israel on Oct 7, 2023 and Israel's war in Gaza affected ad spending on Meta's platforms, we searched the Ad Library for `israel` to collect the more than 170,000 ads that have mentioned the country since September 2015. The Ad Library API returned only 15 ads with a date earlier than 2018, though; for 2018 and each year following, the Ad Library API returned between 12,000 and 39,000 ads matching the query.

The spend amounts in this dataset were reported in 54 different currencies.

### Ads purchased by AIPAC

According to the Ad Library API, the biggest spender on ads that mentioned `israel` was the American Israel Public Affairs Committee. We searched the Ad Library for ads purchased by AIPAC to get a better picture of the organization's overall spending on Meta advertising; the API returned almost 13,000 ads purchased since March 2018. The data show that AIPAC has increased its ad spending on Meta platforms in the last two years; in 2023 the organization bought more ads (4,889) than in the previous five years combined (3,208).

All the spend amounts in this dataset were reported in US dollars.

Questions? Write to us: [tomas@themarkup.org](mailto:tomas@themarkup.org) or [colin@themarkup.org](mailto:colin@themarkup.org).

## Data Dictionaries

* **[meta-ads-by-the-red-first-after-2024-07-12.csv](data/meta-ads-by-the-red-first-after-2024-07-12.csv)** 648KB. 4,269 rows. First row is the header.
* **[meta-ads-mentioning-israel-after-2015-09-11.csv](data/meta-ads-mentioning-israel-after-2015-09-11.csv)** 26.8MB. 172,800 rows. First row is the header.
* **[meta-ads-by-aipac-after-2018-03-05.csv](data/meta-ads-by-aipac-after-2018-03-05.csv)** 1.6MB. 12,982 rows. First row is the header.

### All files

The format of all 3 data files in this repository have the following columns in common:

| Column | Description |
| --- | --- |
| ad_archive_id | A unique ID for one ad, assigned by the Ad Library API |
| ad_creation_time | The date when someone created the ad |
| currency | The currency that the values in `min_spend` and `max_spend` are reported in |
| max_impressions | The upper limit of Meta's estimate of how many impressions the ad received |
| max_spend | The upper limit of Meta's estimate of how much money the advertiser paid for the ad |
| min_impressions | The lower limit of Meta's estimate of how many impressions the ad received |
| min_spend | The lower limit of Meta's estimate of how much money the advertiser paid for the ad |
| ad_url | A URL to display the ad in Meta's web interface to the Ad Library API |
| page_name | The name of the page that purchased the ad |
| page_id | A unique ID for the page that purchased the ad, assigned by the Ad Library API |

### Ads from The Red First

The data format of [meta-ads-by-the-red-first-after-2024-07-12.csv](data/meta-ads-by-the-red-first-after-2024-07-12.csv) includes these additional columns:

| Column | Description |
| --- | --- |
| llama3.1_evaluation | What Llama 3.1 output when prompted to classify the ad. Typically the value is `yes` if the LLM classified this ad as related to the assassination attempt, otherwise `no` |
| corrected_evaluation | The evaluation used to categorize the ads. We started by copying the `llama3.1_evaluation` column, then changed values when we found misclassifications |
| llm_correct | If this column has a value, it means a human checked the LLM's evaluation and noted that it was `RIGHT` or `WRONG` |

### Ads mentioning `israel`

The data format of [meta-ads-mentioning-israel-after-2015-09-11.csv](data/meta-ads-mentioning-israel-after-2015-09-11.csv) includes these additional columns:

| Column | Description |
| --- | --- |
| currency_original | The original currency that was used to purchase the ad. If the value is not `USD`, a currency conversion was performed to calculate the values in `min_spend` and `max_spend`. |
| max_spend_original | The upper limit of Meta's estimate of how much money the advertiser paid for the ad in the original currency |
| min_spend_original | The lower limit of Meta's estimate of how much money the advertiser paid for the ad in the original currency |

### Ads from AIPAC

The data format of [meta-ads-by-aipac-after-2018-03-05.csv](data/meta-ads-by-aipac-after-2018-03-05.csv) does not include any additional columns.

## Licensing

The data contained in this repo was derived in large part from data returned by queries to the Ad Library API. On [the Ad Library API page](https://www.facebook.com/ads/library/api/), Meta notes the following: "Processed data or data that is analyzed as part of your research can be published and shared publicly."

Copyright 2024, The Markup News Inc.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
