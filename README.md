Experee Public Facing API v2.
--

The base url for all requests is https://api.experee.com/v2 and data received in response to a request can be formatted in xml or json by sending the appropriate Accept header with your request; namely `'Accept:application/xml'` or `'Accept:application/json'`.

Mandatory Arguments
--
access-token. The access token you were provided when api access was granted. this should be passed to every request in the form of a GET argument, for example

https://api.experee.com/v2/endpoint?access-token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

the access token is used to authenticate your requests; if this authentication fails the response you receive will have a status field with the status code of 401. If you receive a response like this, please double check your access-token matches what you were provided when api access was granted.

    <response>
       <name>Unauthorized</name>
       <message>Your request was made with invalid credentials.</message>
       <code>0</code>
       <status>401</status>
       <type>yii\web\UnauthorizedHttpException</type>
    </response>

Request Rate Limiting
--
requests are limited to 4 per second. if your application exceeds this limit the response you receive will have a status field with the status code 429. If you receive a response like this, you must wait until your request allowance is again sufficient and then re-make your request.

    <response>
       <name>Too Many Requests</name>
       <message>Rate limit exceeded.</message>
       <code>0</code>
       <status>429</status>
       <type>yii\web\TooManyRequestsHttpException</type>
    </response>

Other Response Fields
--
A response to a successful request to a 'list entities' (eg Products) style endpoint will also include a _meta field which contains meta data about the type of entity being queried.

_meta
 - totalCount - the total number of this type of entity (with any optional filters applied) in the experee system.
 - pageCount - the number of 'pages' of data when totalCount is divided by the current number of items per page
 - currentPage - the number of the 'page' of data returned in response to this request.
 - perPage - the number of items returned in reponse to this request.

 A response to a successful request to a 'list entities' style endpoint will also include a _links field which contains a set of hyperlinks which, in relation to HATEOAS, contain information which allow clients to discover actions supported.

 _links
  - self - A url to the page of data which has been returned in response to this request.
  - first* - A url to the first 'page' of data (not available if the current page is the first).
  - prev* - A url to the previous 'page' of data (not available if the current page is the first).
  - next* - A url to the next 'page' of data (not available if the current page is the last).
  - last* - A url to the last 'page' of data (not available if the current page is the last).

Available Endpoints
--
/products

List products, get data for a predetermined number of products from the experee catalogue. the response from this endpoint can be paginated and (optionally) filtered by country.

Optional GET Arguments

- country - the two character iso country code to filter results.
- town-city - the name of a town or city to filter results.
- per-page - an integer dictating how many products should be returned in response to this request (max 50 items, default 20 items).
- page - an integer dictating which page of paginated results should be returned in response to this request.


Example Request

- https://api.experee.com/v2/products?page=3&country=GB&access-token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

This response to the request will contain a collection of 20 products which are situated in the United Kingdom and are in the 3rd page of results (where a page is the default size of 20 items).

Example Response

    <response>
      <products>
        <item>
	  <ID>10000</ID>
	  <name>Test Product Name</name>
	  <description>Description of product. Can potentially contain html.</description>
	  <prices>
	    <item>
	      <tier_name>Adult</tier_name>
	      <guests>1</guests>
	      <advertised_tier>true</advertised_tier>
	      <price_as_decimal>199.00</price_as_decimal>
	      <currency>GBP</currency>
	      <price_string>£199.00</price_string>
	    </item>
	    <item>
	      <tier_name>Child</tier_name>
	      <guests>1</guests>
	      <advertised_tier>false</advertised_tier>
	      <price_as_decimal>149.00</price_as_decimal>
	      <currency>GBP</currency>
	      <price_string>£149.00</price_string>
	      <description>Special child price</description>
	      <from_age>4</from_age>
	      <to_age>16</to_age>
	    </item>
	  </prices>
	  <product_duration>
	    <unit>seconds</unit>
	    <duration>4200</duration>
	  </product_duration>
	  <permalink>//www.experee.com/united-kingdom/edinburgh/edinburgh-ghost-tour</permalink>
	  <last_updated>1448381328</last_updated>
	  <location>
	    <timezone>Europe/London</timezone>
	    <latitude>55.94976491</latitude>
	    <longitude>-3.19076657</longitude>
	    <town_city>Edinburgh</town_city>
	    <country>GB</country>
	  </location>
	  <media>
	    <item>//www.experee.com/media/EileanDonanCastle1.jpg</item>
	    <item>//www.experee.com/media/Cuillin-Mountains.jpg</item>
	    <item>//www.experee.com/media/Urquhart-Castle1.jpg</item>
	    <item>//www.experee.com/media/cullodenstock1.jpg</item>
	    <item>//www.experee.com/media/WhiskyTasting.jpg</item>
	  </media>
	</item>
	<item>... <!-- further products ommited for brevity --></item>
      </products>
      <_links>
        <self>
	  <href>
	    http://api.experee.com/v2/products?q=%2Fv2%2Fproducts&access-token=08f12b40a924004d48628abb079b1e8d194cc9bb&country=GB&page=3
	  </href>
	</self>
	<first>
	  <href>
	    http://api.experee.com/v2/products?q=%2Fv2%2Fproducts&access-token=08f12b40a924004d48628abb079b1e8d194cc9bb&country=GB&page=1
	  </href>
	</first>
	<prev>
	  <href>
	    http://api.experee.com/v2/products?q=%2Fv2%2Fproducts&access-token=08f12b40a924004d48628abb079b1e8d194cc9bb&country=GB&page=2
	  </href>
	</prev>
	<next>
	  <href>
	    http://api.experee.com/v2/products?q=%2Fv2%2Fproducts&access-token=08f12b40a924004d48628abb079b1e8d194cc9bb&country=GB&page=4
	  </href>
	</next>
	<last>
	  <href>
	    http://api.experee.com/v2/products?q=%2Fv2%2Fproducts&access-token=08f12b40a924004d48628abb079b1e8d194cc9bb&country=GB&page=83
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

--
/products/{id}

get data a single product.

mandatory get arguments
--
- id - an integer representing the unique identifier for a particular product.

example request
-- 
https://api.experee.com/v2/products/1234?access-token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

will return either, data relating to the product with the unique identifier 1234 or, a 404 error if the unique identifier does not exist.

example responses
--
<response>
	<ID>10000</ID>
	<name>Test Product Name</name>
	<description>Description of product. Can potentially contain html.</description>
	<fact1>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</fact1>
	<fact2>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</fact2>
	<fact3>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</fact3>
	<prices>
		<item>
			<tier_name>Adult</tier_name>
			<guests>1</guests>
			<advertised_tier>true</advertised_tier>
			<price_as_decimal>199.00</price_as_decimal>
			<currency>GBP</currency>
			<price_string>£199.00</price_string>
		</item>
		<item>
			<tier_name>Child</tier_name>
			<guests>1</guests>
			<advertised_tier>false</advertised_tier>
			<price_as_decimal>149.00</price_as_decimal>
			<currency>GBP</currency>
			<price_string>£149.00</price_string>
			<description>Special child price</description>
			<from_age>4</from_age>
			<to_age>16</to_age>
		</item>
	</prices>
	<product_duration>
		<unit>seconds</unit>
		<duration>4200</duration>
	</product_duration>
	<permalink>//www.experee.com/united-kingdom/edinburgh/edinburgh-ghost-tour</permalink>
	<last_updated>1448381328</last_updated>
	<location>
		<timezone>Europe/London</timezone>
		<latitude>55.94976491</latitude>
		<longitude>-3.19076657</longitude>
		<town_city>Edinburgh</town_city>
		<country>GB</country>
	</location>
	<media>
		<item>//www.experee.com/media/EileanDonanCastle1.jpg</item>
		<item>//www.experee.com/media/Cuillin-Mountains.jpg</item>
		<item>//www.experee.com/media/Urquhart-Castle1.jpg</item>
		<item>//www.experee.com/media/cullodenstock1.jpg</item>
		<item>//www.experee.com/media/WhiskyTasting.jpg</item>
	</media>
</response>

<response>
	<name>Not Found</name>
	<message/>
	<code>0</code>
	<status>404</status>
	<type>yii\web\NotFoundHttpException</type>
</response>
