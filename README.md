# Pinterest SDK for Python [Beta]
[![build](https://github.com/pinterest/pinterest-python-sdk/actions/workflows/build.yml/badge.svg)](https://github.com/pinterest/pinterest-python-sdk/actions/workflows/build.yml)

### Introduction

The Pinterest SDK currently offers a Python library that supports campaign management and simplifies authentication and error handling. We will be adding functionality supporting organic Pins, shopping, analytics, and more over time. If you have specific feedback about the SDK or requests for additional functionality, please [let us know](https://docs.google.com/forms/d/e/1FAIpQLSf2bA8gyC7kCp_Mgt1jCOvgp22K2EQWg3SEcMxyVRVzddYeMw/viewform?usp=sf_link).

## Pre-requisites
  * Python 3.7+
  * a registered application (see below)
  * an access token (see below)

### Register an App

In order to use the SDK, you must have registered an app on [developers.pinterest.com](https://developers.pinterest.com)

The steps to create an app can be found in the [Set up app](https://developers.pinterest.com/docs/getting-started/set-up-app/) section of the [docs](https://developers.pinterest.com/docs/) on the [Developers' Site](https://developers.pinterest.com/).

### Get Access Token

Follow the instructions outlined on the Pinterest Developer Platform's [Authentication](https://developers.pinterest.com/docs/getting-started/authentication/) Section to retreive an **Access Token** and **Refresh Token**

## Install package

**NOTE**: For Python3, use ``python3`` and ``pip3`` instead.

**NOTE**: If the commands below result in a permissions error (which may happen if you are using a system-installed Python), use ``sudo``.

To install pip, please refer to [pip installation guide](https://pip.pypa.io/en/stable/installation/).

[_Recommended_] Create a virtual environment:

```bash
# Create environment
$ python -m venv .venv

# Activate environment
$ source .venv/bin/activate

```

Install SDK:

```bash
$ pip install pinterest-api-sdk
```

Alternatively, you can check out the repository from GitHub. Once the package is downloaded and unzipped, install it:

```bash
$ python setup.py install
```

You can now use the SDK.

## Getting Started

### Setting up environment variables

To configure the client using environment variables, you must create a **.env** file using [.env.example](https://github.com/pinterest/pinterest-python-sdk/blob/main/.env.example)
as a template. For basic configuration and usage you need to set the following environment variables in the **.env** file:

```
PINTEREST_ACCESS_TOKEN='<access token>'
```
_or_
```
PINTEREST_APP_ID=<app id>
PINTEREST_APP_SECRET=<app secret>
PINTEREST_REFRESH_ACCESS_TOKEN='<refresh token>'
```

Once you have established the environment variables, the client will be instantiated for you automatically. 

**NOTE**: 
 * Setting the `PINTEREST_ACCESS_TOKEN` (which is valid for thirty days) will require the token value to be replaced when it expires. You will need to manually reinsantiate the client when the **access_token** expires. 
 * Setting the `PINTEREST_REFRESH_ACCESS_TOKEN` (which is valid for a year) will allow the SDK to regenerate the new access token whenever it is required. 

For more information visit the [Authentication](https://developers.pinterest.com/docs/getting-started/authentication/#Refreshing%20an%20access%20token) page.

## Samples

### Initializing Models

**Use Case**: 

* Initialize a Campaign object using an existing Ad Account ID and Campaign ID.

```python
from pinterest.ads.campaigns import Campaign

campaign = Campaign(
    ad_account_id="123456789",
    campaign_id="987654321",
)
```

### Examples of Campaign Management using SDK

**Use Case**:

* Create a new Ad
* Assign the Ad to an existing Ad Group
* Activate the Ad Group's parent Campaign
* Change the Campaign's budget

```python
from pinterest.ads.campaigns import Campaign
from pinterest.ads.ad_groups import AdGroup
from pinterest.ads.ads import Ad

## Create a new Ad
new_ad = Ad.create(
    ad_account_id="123456789",
    ad_group_id="999999999",
    creative_type="REGULAR",
    pin_id="111111111",
    name="SDK Example Ad",
    status="ACTIVE",
    is_pin_deleted=False,
    is_removable=False,
)

## Initialize existing paused Campaign
campaign = Campaign(
    ad_account_id="123456789",
    campaign_id="987654321",
)

## Activate campaign
getattr(campaign, '_status')
>>> 'PAUSED'

campaign.activate()
>>> True

getattr(campaign, '_status')
>>> 'ACTIVE'

## Change campaign's lifetime budget
campaign.set_lifetime_budget(
    new_spend_cap=250000000
)
>>> True
```

**Note**: More examples of usage are located in the ``examples/`` folder.

## Documentation For Models

* [pinterest ads](https://github.com/pinterest/pinterest-python-sdk/blob/main/docs/markdown/pinterest.ads.md)

* [pinterest client](https://github.com/pinterest/pinterest-python-sdk/blob/main/docs/markdown/pinterest.client.md)

## Exceptions

See `pinterest.utils.sdk_exceptions` for a list of exceptions which may be thrown by the SDK.

## Debugging

If the SDK is not working as expected there might be an issue with the SDK or the Pinterest API server itself. In order to debug and identify the issue, the environment variables for debugging and logging can be enabled.

```bash
PINTEREST_DEBUG = True
PINTEREST_LOG_FILE = /tmp/log.txt
PINTEREST_LOGGER_FORMAT = '%(asctime)s %(levelname)s %(message)s'
```

When `PINTEREST_DEBUG` is enabled, all the API raw requests and responses will be printed to the console and to the log file in the requested format.

## Issues

For any issues or questions related to the SDK you are welcome to submit them through [GitHub Issues](https://github.com/pinterest/pinterest-python-sdk/issues) using the following templates:
  * [Bug Report Template](https://github.com/pinterest/pinterest-python-sdk/blob/main/.github/ISSUE_TEMPLATE/bug_report.md)
  * [Feature Request Template](https://github.com/pinterest/pinterest-python-sdk/blob/main/.github/ISSUE_TEMPLATE/feature_request.md)

**Note**: There is currently no guaranteed SLA for responding to or resolving issues while the SDK is in beta.

For any general issues related to the Pinterest API (or other Pinterest products) you can contact support at [help.pinterest.com](https://help.pinterest.com)

## Other Resources

Additional information on the Pinterest SDK can be found [here](https://developers.pinterest.com/docs/sdk/intro/).
Additional information about campaigns and campaign management can be found in:
  * The [ads management](https://developers.pinterest.com/docs/features/ads-management/) section of the API documentation
  * The [campaign structure](https://help.pinterest.com/en/business/article/campaign-structure) help article
  * The [create and edit a campaign](https://help.pinterest.com/en/business/article/set-up-your-campaign) help article
  * The [campaign objectives](https://help.pinterest.com/en/business/article/campaign-objectives) help article
  * The [campaign budgets](https://help.pinterest.com/en/business/article/set-up-campaign-budgets) help article

## License

Pinterest Python SDK is licensed under the [LICENSE](https://github.com/pinterest/pinterest-python-sdk/blob/main/LICENSE) file in the root directory of this source tree.
