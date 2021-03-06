Experee Public Facing API v2.
--

The base url for all requests is https://api.experee.com/v2 and data received in response to a request can be formatted in xml or json by sending the appropriate Accept header with your request; namely `'Accept:application/xml'` or `'Accept:application/json'`.

Mandatory Arguments
--
- `access-token`. The value of this argument should be the access token you were provided when api access was granted. This should be passed to every request in the form of a `GET` argument by appending a key/value pair to the request url, for example

https://api.experee.com/v2/endpoint?access-token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

The access token is used to authenticate your requests; if this authentication fails the response you receive will have a `status` field with the status code of `401`.

If you receive a response like this, please double check your access-token matches what you were provided when api access was granted.

    <response>
       <name>Unauthorized</name>
       <message>Your request was made with invalid credentials.</message>
       <code>0</code>
       <status>401</status>
       <type>yii\web\UnauthorizedHttpException</type>
    </response>

Request Rate Limiting
--
Requests are limited to 4 per second. If your application exceeds this limit the response you receive will have a `status` field with the status code `429`.

If you receive a response like this, you must wait until your request allowance is again sufficient and then re-make your request.

    <response>
       <name>Too Many Requests</name>
       <message>Rate limit exceeded.</message>
       <code>0</code>
       <status>429</status>
       <type>yii\web\TooManyRequestsHttpException</type>
    </response>

Other Response Fields
--
A response to a successful request to a 'list entities' (eg `/products`) style endpoint will also include a `_meta` field which contains meta data about the type of entity being queried.

`_meta`
 - `totalCount` - the total number of this type of entity (with any optional filters applied) in the experee system.
 - `pageCount` - the number of 'pages' of data when totalCount is divided by the current number of items per page
 - `currentPage` - the number of the 'page' of data returned in response to this request.
 - `perPage` - the number of items returned in reponse to this request.

 A response to a successful request to a 'list entities' style endpoint will also include a `_links` field which contains a set of hyperlinks which, in relation to [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS), contain information which allow clients to discover actions supported.

 `_links`
  - `self` - A url to the page of data which has been returned in response to this request.
  - `first`* - A url to the first 'page' of data (not available if the current page is the first).
  - `prev`* - A url to the previous 'page' of data (not available if the current page is the first).
  - `next`* - A url to the next 'page' of data (not available if the current page is the last).
  - `last`* - A url to the last 'page' of data (not available if the current page is the last).

Available Endpoints
--
`GET` `/products`
--

List products, get data for a predetermined number of products from the Experee catalogue. The response from this endpoint can be paginated and (optionally) filtered by country and town/city and vary in the number of results returned.

Optional GET Arguments

- `country` - [ISO two-letter country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) to filter results.
- `town-city` - The name of a town or city to filter results. Use of this filter also requires of the `country` filter (see above).
- `per-page` - An integer dictating how many products should be returned in response to this request (max 50 items, default 20 items).
- `page` - An integer dictating which page of paginated results should be returned in response to this request.


Example Request
- https://api.experee.com/v2/products?page=3&country=GB&access-token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

The response to this request will contain a collection of 20 products which are situated in the United Kingdom and are in the 3rd page of results (where a page is the default size of 20 items).

Example Response

    <response>
      <products>
        <item>
          <ID>10000</ID>
          <name>Test Product Name</name>
          <description>Description of product. Can potentially contain html.</description>
          <prices>
            <item>
              <ID>101</ID>
              <tier_name>Adult</tier_name>
              <guests>1</guests>
              <advertised_tier>true</advertised_tier>
              <price_as_decimal>199.00</price_as_decimal>
              <currency>GBP</currency>
              <display_price>£199.00</display_price>
            </item>
            <item>
              <ID>102</ID>
              <tier_name>Child</tier_name>
              <guests>1</guests>
              <advertised_tier>false</advertised_tier>
              <price_as_decimal>149.00</price_as_decimal>
              <currency>GBP</currency>
              <display_price>£149.00</display_price>
              <description>Special child price</description>
              <from_age>4</from_age>
              <to_age>16</to_age>
            </item>
          </prices>
          <product_duration>
            <unit>seconds</unit>
            <duration>4200</duration>
          </product_duration>
          <permalink>https://www.experee.com/united-kingdom/edinburgh/edinburgh-ghost-tour</permalink>
          <last_updated>1448381328</last_updated>
          <location>
            <timezone>Europe/London</timezone>
            <latitude>55.94976491</latitude>
            <longitude>-3.19076657</longitude>
            <town_city>Edinburgh</town_city>
            <country>GB</country>
          </location>
          <media>
            <item>https://www.experee.com/media/EileanDonanCastle1.jpg</item>
            <item>https://www.experee.com/media/Cuillin-Mountains.jpg</item>
            <item>https://www.experee.com/media/Urquhart-Castle1.jpg</item>
            <item>https://www.experee.com/media/cullodenstock1.jpg</item>
            <item>https://www.experee.com/media/WhiskyTasting.jpg</item>
          </media>
        </item>
        <item>... <!-- further products ommited for brevity --></item>
      </products>
      <_links>
        <self>
          <href>
            https://api.experee.com/v2/products?q=%2Fv2%2Fproducts&access-token=xxxxxxxxxxxxxxx&country=GB&page=3
          </href>
        </self>
        <first>
          <href>
            https://api.experee.com/v2/products?q=%2Fv2%2Fproducts&access-token=xxxxxxxxxxxxxxx&country=GB&page=1
          </href>
        </first>
        <prev>
          <href>
            https://api.experee.com/v2/products?q=%2Fv2%2Fproducts&access-token=xxxxxxxxxxxxxxx&country=GB&page=2
          </href>
        </prev>
        <next>
          <href>
            https://api.experee.com/v2/products?q=%2Fv2%2Fproducts&access-token=xxxxxxxxxxxxxxx&country=GB&page=4
          </href>
        </next>
        <last>
          <href>
            https://api.experee.com/v2/products?q=%2Fv2%2Fproducts&access-token=xxxxxxxxxxxxxxx&country=GB&page=83
          </href>
        </last>
      </_links>
      <_meta>
        <totalCount>1649</totalCount>
        <pageCount>83</pageCount>
        <currentPage>3</currentPage>
        <perPage>20</perPage>
      </_meta>
    </response>



`GET` `/products/{id}`
--
Get data for a single product.

Mandatory GET Arguments

- `id` - an integer representing the unique identifier for a particular product.

Example Request
- https://api.experee.com/v2/products/1234?access-token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

The response to this request will contain either, data relating to the product with the unique identifier 1234 or, a 404 error if the unique identifier does not exist.

Example Responses

Successful

    <response>
      <ID>10000</ID>
      <name>Test Product Name</name>
      <description>Description of product. Can potentially contain html.</description>
      <fact1>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</fact1>
      <fact2>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</fact2>
      <fact3>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</fact3>
      <prices>
        <item>
          <ID>101</ID>
          <tier_name>Adult</tier_name>
          <guests>1</guests>
          <advertised_tier>true</advertised_tier>
          <price_as_decimal>199.00</price_as_decimal>
          <currency>GBP</currency>
          <display_price>£199.00</display_price>
        </item>
        <item>
          <ID>102</ID>
          <tier_name>Child</tier_name>
          <guests>1</guests>
          <advertised_tier>false</advertised_tier>
          <price_as_decimal>149.00</price_as_decimal>
          <currency>GBP</currency>
          <display_price>£149.00</display_price>
          <description>Special child price</description>
          <from_age>4</from_age>
          <to_age>16</to_age>
        </item>
      </prices>
      <product_duration>
        <unit>seconds</unit>
        <duration>4200</duration>
      </product_duration>
      <permalink>https://www.experee.com/united-kingdom/edinburgh/edinburgh-ghost-tour</permalink>
      <last_updated>1448381328</last_updated>
      <location>
        <timezone>Europe/London</timezone>
        <latitude>55.94976491</latitude>
        <longitude>-3.19076657</longitude>
        <town_city>Edinburgh</town_city>
        <country>GB</country>
      </location>
      <media>
        <item>https://www.experee.com/media/EileanDonanCastle1.jpg</item>
        <item>https://www.experee.com/media/Cuillin-Mountains.jpg</item>
        <item>https://www.experee.com/media/Urquhart-Castle1.jpg</item>
        <item>https://www.experee.com/media/cullodenstock1.jpg</item>
        <item>https://www.experee.com/media/WhiskyTasting.jpg</item>
      </media>
    </response>

Unsuccesful

    <response>
      <name>Not Found</name>
      <message/>
      <code>0</code>
      <status>404</status>
      <type>yii\web\NotFoundHttpException</type>
    </response>

`POST` `/vouchers`
--
Create a new voucher on the Experee.com system.

Manadatory POST arguments

- `productID` (`Integer`) The ID of the product this voucher is for.
- `prices` (`Array`) A map from pricing tier ID to quantity of pricing tier required for voucher.

eg. 

    {
        'productID': 100,
        'prices': {
            '101' => 1,
            '102' => 2
        }
    }

Posting a json object with a valid productID and appropriate prices object to this endpoint will create and return a new voucher.


`GET` `/vouchers/{code}`
--
Get details about a specific voucher.

Mandatory GET Arguments

- `code` - an alphanumeric unique voucher code.

Example Request

- https://api.experee.com/v2/vouchers/abc123?access-token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

The response to this request will contain either, data relating to the voucher with the unique identifier or, a 404 error if the unique identifier does not exist.

Example Responses

Successful

    <response>
      <voucher_code>OVjKBsuMN8</voucher_code>
      <expiry_date>1546521797</expiry_date>
      <redeemed>false</redeemed>
      <product>Descent and Discover Edinburgh, Self Guided Tour</product>
      <permalink>
        https://www.experee.com/united-kingdom/edinburgh/tours/descent-and-discover-edinburgh-self-guided-tour
      </permalink>
    </response>

Unsuccessful

    <response>
      <name>Not Found</name>
      <message>
        A voucher with the code OVjKBsu8 could not be found
      </message>
      <code>0</code>
      <status>404</status>
      <type>yii\web\NotFoundHttpException</type>
    </response>

Entities
--
`Product`

A `product` entity consists of the following attributes.
- `ID` (`Integer`) The unique identifier of a particular product.
- `name` (`String`) The name of a particular product.
- `description` (`String`) The description of a particular product. This may contain html or markdown.
- `fact[1-5]` (`String`) (optional) A short fact about the product; up to five facts per product.
- `useful_information` (`String`) (optional) Other useful information regarding the product.
- `prices` (`Price`) A collection of one or more price entities (see below).
- `product_duration` (`Product Duration`) A compound entity which describes the duration of a particular product (see below).
- `permalink` (`String`) The url to a particular product on the main Experee.com website.
- `last_updated` (`Integer`) The unix timestamp for the last time a particular product was updated on the Experee.com system.
- `location` (`Location`) A compound entity which describes the location and address of a particular product.
- `media` (`String`) A collection of urls for the full size images associated with a particular product.

`Price`

The `prices` field is made up of one or more `price` entities, grouped together as items and consisting of the following attributes where some are marked as optional.

- `tier_name` (`String`) The name of this pricing tier; Adult, Child, Per Person etc.
- `guests` (`Integer`) The number of people a single purchase of this price tier would provide.
- `advertised_tier` (`String`) (`true` or `fale`) Should this tier be used in calculating 'from price'?
- `price_as_decimal` (`Float`) The numerical representation of the price for this pricing tier. Useful for calculation of multiple purchases etc.
- `currency` (`String`) The [ISO currency code](https://en.wikipedia.org/wiki/ISO_4217) for the currency this pricing tier is baselined in.
- `display_price` (`String`) A representation of the symbol of the currency this pricing tier is baselined in and the numerical part of this pricing tier. Useful for display purposes.
- `description` (`String` - optional) A more detailed description of this pricing tier. May include restrictions according to age etc.
- `from_age` (`Integer` - optional) A lower limit for age restrictions for this pricing tier.
- `to_age` (`Integer` - optional) An upper limit for age restrictions for this pricing tier.

`Product Duration`

the `product_duration` entity consists of the following attributes.
- `unit` (`String`) The type of unit the `duration` field is counting. This can be either `seconds` (for products with a duration of less than a day) or `days` (for products with a duration of a day or more).
- `duration` (`Integer`) The number of `unit`s which make up amount to the product's duration.

`Location`

The `location` entity consists of the following attributes.

- `timezone` (`String`) The name of the [timezone](https://en.wikipedia.org/wiki/Tz_database) the product is situated in, in the form `Area/Location`, for example `Europe/London`.
- `latitude` (`Float`) The latitude of the product.
- `longitude` (`Float`) The longitude of the product.
- `town_city` (`String`) The name of the town or city the product is situated in.
- `country_code` (`String`) The [ISO two-letter country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) of the country the product is situated in.
- `country` (`String`) The full name of the country the product is situated in.

`Voucher`

The `voucher` entity consists of the following attributes.
- `voucher_code` (`String`) The voucher code to be entered during checkout.
- `expiry_date` (`Integer`) The unix timestamp for when this voucher will expire.
- `valid` (`String`) (`true` or `false`) Whether or not this voucher has expired or not.
- `redeemed` (`String`) (`true` or `false`) Whether or not this voucher has been used before.
- `product` (`String`) The name of the product this voucher is valid for.
- `permalink` (`String`) The premalink to the product on Experee.com this voucher is for.
