# Introduction

Welcome to the NoHassls API. You can use our API to access NoHassls API endpoints, which can provide information on lead prospects, requests, and insurance demands from customers, check your current billing, stats and other amazing things.

We have language bindings in Shell, PHP, and Javascript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This documentation is organised to mimic the workflow of making a NoHassls API request.

The first step is to query for any new insurance profiles (QuoteRequest Object). These are the details of an insurance request made by a customer. This contains all the details they have supplied, allowing an anonymous insurance quotation to be provided. This does not contain any identifying information about the client.

Pinnacle Life will return a QuoteReply Object that contains information about the quote. This includes data such as the cost per month, annual cost, product details, availability and other data concerning the quote.

A notification is then sent to the user that the Pinnacle quotation has been supplied and is available for scrutiny. 

If the customer decides to accept the quotation, then a ContactRequest object will be added to the Pinnacle ContactRequestQueue API notifying you that a customer wishes to be contacted.  The ContactRequest Object object will contain the preferred contact time and date supplied by the customer and any additional questions or information.

Pinnacle Life will create a ContactReply Object and post it to the ContactReplyQueue. The customer will then be notifed that the time slot requested is available and Pinnacle are ready to contact them.

The customer agrres to release his/her private information to Pinnacle and a CustomerProfileObject will be added to the CustomerProfileQueue for retrieval by Pinnacle Life.


The CustomerProfileObject will contain the ID of the original Insurance Pofile, the ID of the Pinnacle profile response and the ID of the User Profile record. A UserProfile record will contain the users contact information, name, address and any other private information including email address and telephone number.



Pinnacle Life now have all the information required to initiate contact with the customer and continue the application process.

![](http://nohassls.co.nz/static/images/qrs.png)


# Authentication

> To authorize, use this code:

```ruby
require 'NoHassls'

api = NoHassls::APIClient.authorize!('PINNACLE-KEY')
```

```php
import NoHassls

api = NoHassls.authorize('PINNACLE-KEY')
```

```Javascript
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: PINNACLE-KEY"
```

> Make sure to replace `PINNACLE-KEY` with your API key.

NoHassls uses API keys to allow access to the API. These keys are generated and maintained by the NoHassls Development Team. For more information about your key please visit our [developer portal](http://developers.nohassls.co.nz).

NoHassls expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: PINNACLE-KEY`

Any request that does not contain this key will be refused with a 403 FORBIDDEN return code.
<aside class="notice">
You must replace <code>PINNACLE-KEY</code> with your Pinnacle API key.
</aside>

# Insurance Requests



### HTTP Request

`GET http://nohassls.co.nz/api/insurance/type

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
type      | NULL  | Request the type of insurance quotes to retreieve. See the table below for a list of available types
since     | false | Send a datetime and all quotes after this will be returned irregardless if they have been previously retrieved. (2015-12-06T11:02:21+11:00)
unread    | true  | Will return all quotes in the systen that have not been previously requested. I.E all NEW quote requests (true or false)
query     | NULL  | Filter. e.g. col1:val1,col2:val2 (id:1) or postcode:2205,pool:no to retrieve all quote requests that have a postcode value of 2205 and the value no for a pool.
fields    | NULL  | Fields returned. e.g. Id,Pool will return only those fields. NOTE the fields are case sensitive, passing the incorrect value will result in an error. See the schema definitions for field naming information.
sortby    | NULL  | Sorted-by fields. e.g. col1,col2 (Must be used with order param. See Below)
order     | NULL  | Order corresponding to each sortby field, if single value, apply to all sortby fields. e.g. desc,asc  e.g &sortby=Id,Pool&order=asc,desc
limit     | NULL  | Limit the size of result set. Must be an integer
offset    | NULL  | Start position of result set. Must be an integer



### Insurance Type Parameters
Most are quite obvious and can easily be remembered. Rather than a cryptic string or number reference, we use a simple string value in the paramater to retrieve quotation requests of that type.

Parameter | Description
--------- | ------- |
life      | Life Insurance
health    | Health Insurance
funeral   | Funeral Insurance
mortgage  | Mortgate Insurance
trauma    | Trauma Insurance
disability| Disability Insurance

#### The following insurance types are also provided by NoHassls, and can be made available to Pinnacle Life upon request.

Parameter | Description
--------- | ------- |
car       | Car Insurance
bike      | Motorcycle Insurance
boat      | Boat Insurance
caravan   | Caravan Insurance
collectors| Collector Car Insurance
home      | Home & Contents Insurance
income    | Income Insurance
landlord  | Landlord Insurance
pet       | Pet Insurance
travel    | Travel Insurance


## Get All Requests for insurance type

```ruby
require 'NoHassls'

api = NoHassls::APIClient.authorize!('PINNACLE-KEY')
api.type="life"
api.insurance.get
```

```python
import NoHassls

api = NoHassls.authorize('PINNACLE-KEY')
api.lifeinsurance.get()
```

```bash
curl "http://nohassls.co.nz/api/insurance/type"
  -H "Authorization: PINNACLE-KEY"
```

> The above command returns JSON structured like this:

```json
[
{
    "Id": 1,
    "Postcode": 2205,
    "ProposedStartDate": "2015-12-01T00:00:00+11:00",
    "EstimatedYearConstruction": 2010,
    "NumberStoreys": 7,
    "NumberBathrooms": 2,
    "NumberBedrooms": 3,
    "BedroomSize": "30",
    "HomeDesc": "nice unit",
    "ExteriorWalls": "concrete",
    "RoofMaterial": "another unit",
    "HomeOccupied": "yes",
    "StrataPlan": "yes",
    "Pool": "no",
    "Name": "our home",
    "DobOldestPolicyHolder": "2015-10-01T00:00:00+10:00",
    "EntitledNoClaim": 1,
    "AnyBelowGround": 1,
    "SecurityDevices": "security building",
    "DoorSecurityDevices": "locks",
    "WindowSecurityDevices": "7 floor drop",
    "Alarm": 1,
    "LandLargerNormal": 0,
    "HadPolicyCancelled": 0,
    "NumTimesClaimDeclined": 0,
    "NumClaimsTotal": 0,
    "NumConvictions": 1,
    "CoverStormDmgGatesFences": 1,
    "CoverAccidentalDmg": 1,
    "Over50Discount": 1,
    "Carports": 2,
    "BalconiesDecks": 2,
    "Verandahs": 2,
    "EuroKitchenAppliances": 1,
    "GraniteMarbleTiling": 1,
    "LargeGlazedAreas": 1,
    "PlantationShutters": 0,
    "CurvedWalls": 0,
    "DuctedAircon": 1,
    "TennisCourt": 0,
    "IngroundPool": 0,
    "Watertight": 1,
    "NewHomeUnderConstruction": 0,
    "UnderRenovation": 0,
    "HomeUse": "home",
    "Mortgate": 1,
    "InsureNameOrCompany": "no current insurer",
    "NumOwnersNamedPolicy": 2,
    "Comments": "some comments",
    "Status": 0
  },
  {
    "Id": 2,
    "Postcode": 2205,
    "ProposedStartDate": "2015-12-01T00:00:00+11:00",
    "EstimatedYearConstruction": 2010,
    "NumberStoreys": 7,
    "NumberBathrooms": 2,
    "NumberBedrooms": 3,
    "BedroomSize": "30",
    "HomeDesc": "nice unit",
    "ExteriorWalls": "concrete",
    "RoofMaterial": "another unit",
    "HomeOccupied": "yes",
    "StrataPlan": "yes",
    "Pool": "no",
    "Name": "our home",
    "DobOldestPolicyHolder": "2015-10-01T00:00:00+10:00",
    "EntitledNoClaim": 1,
    "AnyBelowGround": 1,
    "SecurityDevices": "security building and first floor",
    "DoorSecurityDevices": "locks",
    "WindowSecurityDevices": "7 floor drop",
    "Alarm": 1,
    "LandLargerNormal": 0,
    "HadPolicyCancelled": 0,
    "NumTimesClaimDeclined": 0,
    "NumClaimsTotal": 0,
    "NumConvictions": 1,
    "CoverStormDmgGatesFences": 1,
    "CoverAccidentalDmg": 1,
    "Over50Discount": 1,
    "Carports": 2,
    "BalconiesDecks": 2,
    "Verandahs": 2,
    "EuroKitchenAppliances": 1,
    "GraniteMarbleTiling": 1,
    "LargeGlazedAreas": 1,
    "PlantationShutters": 0,
    "CurvedWalls": 0,
    "DuctedAircon": 1,
    "TennisCourt": 0,
    "IngroundPool": 0,
    "Watertight": 1,
    "NewHomeUnderConstruction": 0,
    "UnderRenovation": 0,
    "HomeUse": "home",
    "Mortgate": 1,
    "InsureNameOrCompany": "no current insurer",
    "NumOwnersNamedPolicy": 2,
    "Comments": "some comments",
    "Status": 0,
  }
]
```

This endpoint retrieves all  insurance quotation requests.

<aside class="success">
Remember — all requests must include your key. 
</aside>


## Get a Specific Insurance Request

```ruby
require 'nohassls'

api = nohassls::APIClient.authorize!('meowmeowmeow')
api.insurancerequest.get(2)
```

```python
import nohassls

api = nohassls.authorize('meowmeowmeow')
api.insurancerequest.get(2)
```

```bash
curl "http://nohassls.co.nz/api/insurance/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
    "Id": 1,
    "Postcode": 2205,
    "ProposedStartDate": "2015-12-01T00:00:00+11:00",
    "EstimatedYearConstruction": 2010,
    "NumberStoreys": 7,
    "NumberBathrooms": 2,
    "NumberBedrooms": 3,
    "BedroomSize": "30",
    "HomeDesc": "nice unit",
    "ExteriorWalls": "concrete",
    "RoofMaterial": "another unit",
    "HomeOccupied": "yes",
    "StrataPlan": "yes",
    "Pool": "no",
    "Name": "our home",
    "DobOldestPolicyHolder": "2015-10-01T00:00:00+10:00",
    "EntitledNoClaim": 1,
    "AnyBelowGround": 1,
    "SecurityDevices": "security building",
    "DoorSecurityDevices": "locks",
    "WindowSecurityDevices": "7 floor drop",
    "Alarm": 1,
    "LandLargerNormal": 0,
    "HadPolicyCancelled": 0,
    "NumTimesClaimDeclined": 0,
    "NumClaimsTotal": 0,
    "NumConvictions": 1,
    "CoverStormDmgGatesFences": 1,
    "CoverAccidentalDmg": 1,
    "Over50Discount": 1,
    "Carports": 2,
    "BalconiesDecks": 2,
    "Verandahs": 2,
    "EuroKitchenAppliances": 1,
    "GraniteMarbleTiling": 1,
    "LargeGlazedAreas": 1,
    "PlantationShutters": 0,
    "CurvedWalls": 0,
    "DuctedAircon": 1,
    "TennisCourt": 0,
    "IngroundPool": 0,
    "Watertight": 1,
    "NewHomeUnderConstruction": 0,
    "UnderRenovation": 0,
    "HomeUse": "home",
    "Mortgate": 1,
    "InsureNameOrCompany": "no current insurer",
    "NumOwnersNamedPolicy": 2,
    "Comments": "some comments",
    "Status": 0
  }
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve
