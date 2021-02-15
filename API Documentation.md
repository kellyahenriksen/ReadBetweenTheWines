# QUINI API documentation (Confidential)

## Profile
### Version

2.00 20160714

### Change Log

* 2.00 Add web search API (IFrame) and SOMM API (IFrame) for website embedding
* 1.40 Add default content for no review of Wine aggregate Summary and Wine Aggregate Reviews
* 1.30 Add Taste API
* 1.20 Change user group privileges requirement
* 1.10 Change Wine Search, add Wine Categories
* 1.04 Add Wine Aggregate Reviews, Review Bloom Icon, and Individual Review
* 1.03 Change Wine Bloom Icon to SVG graphic, change Wine Search and Wine Aggregate Summary output
* 1.02 Add Wine Bloom Icon, Wine Aggregate Summary
* 1.01 Add Authentication, Wine Search
* 1.00 Generate document, usage and authentication algorithm

### Author

Alfred Lu (alfred@quiniwine.com)

### API Authentication

To use Quini API (JSON) services, a Quini API account is required.

For other Quini API (IFRAME or SVG) services, please do not use your API account tor fetching data.

We use OAuth 2.0 authentication token to provide private Quini API.

To associate a new account you need to provide an email address and password for which you will receive an API token.

Subsequent API (JSON) requests can then made using the returned token by:
- In HTTP Header: Add Authentication Header like 'Authorization:Bearer fYeSO-ChogAFnqAvDHv1'
- In Request URI as parameter: Add '?access_token=fYeSO-ChogAFnqAvDHv1' to the Request URI
- In a Form for POST request (not recommended)

### HTTP response

All successful queries will response HTTP status 200.
When a request fails, Quini server responds using the appropriate HTTP status code (typically, 400, 401, 403, or 405)

### Query for Pagination

Resources generally take the optional parameters `limit` and `skip` which can be used for pagination.

### Developing & Testing

## API

### Authentication

#### Definition

>POST https://quiniwine.com/signin

#### Parameters

- email: registered user account
- password: user password

#### Example Request

	$ curl https://quiniwine.com/signin \
		-H Accept:application/json \
		-d email=YourEmailAddress \
		-d password=YourPassword

#### Example Response

```
  {
    "id": "5f357cf083ae3713eb569bde",
    "_id": "5f357cf083ae3713eb569bde",
    "name": "Test",
    "email": "YourEmailAddress",
    "token": "YourToken",
    "creationDate": "2020-08-13T17:48:32.832Z",
    "accountType": 2,
    "appStatus": 0,
    "s3AvatarBucket": "rachis",
    "somm": 0,
    "hideHistory": false,
    "tastExperience": null,
    "wineExperience": []
}
```

- - -

### Wine Search (JSON)

#### Definition

>GET https://quiniwine.com/api/pub/wineKeywordSearch/(keywords)/(skip)/(limit)?winetype=

#### Parameters

- keywords: query words to search wines, **at least 2 letters** required, use space (%20) to seperate the query words, can use 'wine name, winery, varietals, vintage, location etc.'
- limit: optional, returned number of matching wines
- skip: optional, skip the leading number of wines, for pagination
- winetype: optinal, can be 'red', 'white', 'rose' or 'none' for all types

#### Example Request

	$ curl 'https://quiniwine.com/api/pub/wineKeywordSearch/Bollinger%20rose/0/1?winetype=rose' \
		-H 'Accept:application/json' \
		-H 'Authorization:Bearer YourToken'

#### Example Response

```
{
    "nextskip": 1,
    "category": "ROSES",
    "items": [
        {
            "_id": "54566e1d65ade0020000001f",
            "id": "54566e1d65ade0020000001f",
            "Name": "Bollinger Rose",
            "Winery": "Bollinger",
            "Area": "Champagne",
            "Province": "",
            "Country": "France",
            "Varietal": "Pinot Noir, Chardonnay",
            "vintage": "nv",
            "Style": "",
            "Type": "Rose"
        }
    ]
}
```

- - -

### Wine Web Search (IFRAME)

#### Definition

A landing page to show Quini wines and provide web search function to show more specific wines.
This iframe first display 'featured wine list', 'most recent' and 'top rated' pre-populated lists and can be replaced by search result wine list when user perform keywords search.
This iframe can be navigated to Wine Aggregate Reviews (IFRAME).

>IFRAME https://quiniwine.com/api/pub/embed/(customtoken)

#### Example Iframe Code

	<iframe src="https://quiniwine.com/api/pub/embed/YourCustomToken" style="border: none; margin:0 auto; display:block; width:960px; height:auto; overflow-x:hidden; overflow-y:scroll"></iframe>

#### Iframe Parameters

- customtoken: a unique string which represent the specific user's IFrame page, different users have different tokens.

- iframe style settings
	- width: percent or pixels of the width, recommended value >768px or 100%
	- height: pixels of the iframe height, recommened value auto
	- overflow-x: hidden or scroll, recommended value hidden
	- overflow-y: hidden or scroll, recommended value scroll


Note: You need request us for your customToken
- - -

### SOMM API (IFRAME)

#### Definition

A landing page to show SOMM lists for Quini SOMM clients.
This iframe can be navigated to Wine Aggregate Reviews (IFRAME).

>IFRAME https://quiniwine.com/api/pub/sommlist/(token)

#### Example Iframe Code

	<iframe src="https://quiniwine.com/api/pub/sommlist/%23w90Ad8DKNpy0PYJbjYox34pQ%23" style="border: none; margin:0 auto; display:block; width:960px; height:auto; overflow-x:hidden; overflow-y:scroll"></iframe>

#### Iframe Parameters

- token: a unique string which represent the specific SOMM client's SOMM API IFrame page.

- iframe style settings
	- width: percent or pixels of the width, recommended value >768px or 100%
	- height: pixels of the iframe height, recommened value auto
	- overflow-x: hidden or scroll, recommended value hidden
	- overflow-y: hidden or scroll, recommended value scroll


Note: We will provide you the link for your some api on your request, you need to replace link in src with one we will share with you. 
- - -

### Wine Bloom Icon (SVG)

#### Definition

>GET https://quiniwine.com/api/pub/wineBloom?wine_id=(wine_id)

#### Example embedded Code

	<img src="https://quiniwine.com/api/pub/wineBloom?wine_id=53ab5f2adab5f0020000001f" width="160px" height="160px"/>
	<object data="https://quiniwine.com/api/pub/wineBloom?wine_id=53ab5f2adab5f0020000001f" type="image/svg+xml" />

#### Parameters

- wine_id: get the bloom graphic using Quini's wine id

- embedded settings:
	- recommend using &lt;img /&gt; element
	- or using &lt;object /&gt; for IE &lt; 9.0 or Firefox &lt; 4.0
	- place wine bloom svg into a square block div with defined width and height for best result
  - if no review with this wine, a wine type related bottle image will return

- - -

### Review Bloom Icon (SVG)

#### Definition

>GET https://quiniwine.com/api/pub/reviewBloom?review_id=(review_id)

#### Example embedded Code

	<img src="https://quiniwine.com/api/pub/reviewBloom?review_id=53ab87e8dab5f00200000055" width="160px" height="160px"/>
	<object data="https://quiniwine.com/api/pub/reviewBloom?review_id=53ab87e8dab5f00200000055" type="image/svg+xml" />

#### Parameters

- review_id: get the individual review bloom graphic using Quini's review id

- embedded settings:
	- recommend using &lt;img /&gt; element
	- or using &lt;object /&gt; for IE &lt; 9.0 or Firefox &lt; 4.0
	- place review bloom svg into a square block div with defined width and height for best result

- - -

### Wine Aggregate Summary (IFRAME or JSON)

#### Definition
When there is no review, then wine information displays with an 'open for tasting' button

>IFRAME https://quiniwine.com/api/pub/wineSummary?wine_id=(wine_id)

>GET https://quiniwine.com/api/pub/wineSummary.json?wine_id=(wine_id)

#### Example Iframe Code

	<iframe src="https://quiniwine.com/api/pub/wineSummary?wine_id=53ab5f2adab5f0020000001f" style="border: none; margin:0 auto; display:block; width:768px; height:320px; overflow-x:hidden; overflow-y:scroll"></iframe>

#### Iframe Parameters

- wine_id: get the wine aggregate summary iframe using Quini's wine id
- mode: can be 'n' or 'e'. As the case no review, will display 'open for tasting' button, mode=e will hide the button, and mode=n will open a new tasting window after click the button rather than open in the current windown.

- iframe style settings
	- width: percent or pixels of the width, recommended value >768px or 100%
	- height: pixels of the iframe height, recommened value >380px
	- overflow-x: hidden or scroll, recommended value hidden
	- overflow-y: hidden or scroll, recommended value scroll

#### Example Request

	$ curl https://quiniwine.com/api/pub/wineSummary.json?wine_id=53ab5f2adab5f0020000001f \
		-H 'Accept:application/json' \
		-H 'Authorization:Bearer YourToken'

#### Example Response

```
  {
    "aggregate": {
      "scoreAvg": [
        100,//Eye score
        88.33333333333333,//Nose score
        93.33333333333333,//Mouth score
        93.66666666666667,//Finish score
        91//Overall score
      ],
    "wine": {
      "id": "53ab5f2adab5f0020000001f",
      "Name": "Prima Perla Prosecco NV",
      "Winery": "Prima Perla",
      "vintage": "nv",
      "Country": "Italy ",
      "Area": "Veneto ",
      "Style": "",
      "Varietal": "Glera ",
      "Type": "White"
    },
      "count": 3//Total review number of this wine
    },
    "agg_summary": {
      "textReviews": {
        "eye": "Clear rim, Clear depth",
        "nose": "Citrus, Fruity, Floral aromas",
        "mouth": "Melon, Grapefruit, Citrus, Fruity, Orange Blossom, Floral flavours, Fresh sweetness, Fresh acidity, Rich tannins, Mild alcohol",
        "finish": "Medium duration, Good quality, Early peaktime",
        "overall": "Simple complexity, Pleasant interest, Expected typicity, Harmonious balance"
      }
    }
  }
```

- - -

### Wine Aggregate Reviews (IFRAME)

#### Definition

Click on the reviews block to change page to Individual Review page in the same iframe
When there is no review, then wine information displays with an 'open for tasting' button

>IFRAME https://quiniwine.com/api/pub/wineReviews?wine_id=(wine_id)

#### Example Iframe Code

	<iframe src="https://quiniwine.com/api/pub/wineReviews?wine_id=53ab5f2adab5f0020000001f" style="border: none; margin:0 auto; display:block; width:960px; height:auto; overflow-x:hidden; overflow-y:scroll"></iframe>

#### Iframe Parameters

- wine_id: get the wine aggregate reviews iframe using Quini's wine id
- mode: can be 'n' or 'e'. For the 'open for tasting' button, mode=e will hide the button, and mode=n will open a new tasting window after click the button rather than open in the current windown.

- iframe style settings
	- width: percent or pixels of the width, recommended value >768px or 100%
	- height: pixels of the iframe height, recommened value auto
	- overflow-x: hidden or scroll, recommended value hidden
	- overflow-y: hidden or scroll, recommended value scroll

- - -

### Individual Review (IFRAME or JSON)

#### Definition

Using the 'go back' button on top to return to the Wine Aggregate Reviews page.

>IFRAME https://quiniwine.com/api/pub/review?review_id=(review_id)

>GET https://quiniwine.com/api/pub/review.json?review_id=(review_id)
>GET https://quiniwine.com/api/pub/review.json?review_id=(wind_id)

#### Example Iframe Code

	<iframe src="https://quiniwine.com/api/pub/review?review_id=53ab87e8dab5f00200000055" style="border: none; margin:0 auto; display:block; width:768px; height:320px; overflow-x:hidden; overflow-y:scroll"></iframe>

#### Iframe Parameters

- review_id: get the individual review detail iframe using Quini's review id
- mode: can be 'n' or 'e'. For the 'open for tasting' button, mode=e will hide the button, and mode=n will open a new tasting window after click the button rather than open in the current window.

#### Iframe Optional Parameters
- Below are the optional parameters which one can pass with value 'true' inorder to hide any section from the iframe. 
- hideMoreOptions
- hideProfile
- hideBloom
- hideWineName
- hideTasteNote
- hideSensoryData
- hidePersonalNotes
- hideOpenForTasting

Example: 

	<iframe src="https://quiniwine.com/api/pub/review?review_id=53ab87e8dab5f00200000055&hidePersonalNotes=true&hideBloom=true" style="border: none; margin:0 auto; display:block; width:768px; height:320px; overflow-x:hidden; overflow-y:scroll"></iframe>

- iframe style settings
	- width: percent or pixels of the width, recommended value >768px or 100%
	- height: pixels of the iframe height, recommened value auto
	- overflow-x: hidden or scroll, recommended value hidden
	- overflow-y: hidden or scroll, recommended value scroll

- - -

### Tasting Note/Review Note API (JSON)
This API requires: 
1. access-token - Use signIn api, this needs to passed as header parameter. 
2. wine_id - Id of a particular wine whose tasting note one wants to get, pass this in querystring.

Sample Request Curl: 
curl -X GET \
'https://quiniwine.com/api/pub/reviewNote?wine_id=5f236cfbc9483d19f85e805d' \
-H 'access-token: #YourToken#'

Sample Response:
{
    "reviewNote": [
        {
            "reviewId": "525ee8799e6e880200000013",
            "SubmissionDate": "2013-10-16T19:26:49.200Z",
            "tastingNote": "Emulating a big white burgundy.  A touch too sweet and too oaky. Fairly priced."
        }
    ]
}

If Token is not passed then response:
{
    "message": "Validation errors.",
    "errors": [
        {
            "param": "access-token",
            "msg": "Token is required"
        }
    ]
}

if Invalid token passed then response:
{
    "message": "Validation errors.",
    "errors": [
        {
            "param": "access-token",
            "msg": "Invalid Token"
        }
    ]
}

### Wine Categories (JSON)

#### Definition

>GET https://quiniwine.com/api/pub/wineCategory/(categorycode)/(skip)/(limit)

#### Parameters

- limit: optional, returned number of matching wines
- skip: optional, skip the leading number of wines, for pagination
- categorycode: code for pre-populated wine categories, see the list below

```
't': 'TOP RATED',
'm': 'MOST RECENT',
'p': 'POPULAR VARIETALS',
'tre': 'TOP RATED REDS',
'twh': 'TOP RATED WHITES',
'tro': 'TOP RATED ROSES',
'mre': 'MOST RECENT REDS',
'mwh': 'MOST RECENT WHITES',
'mro': 'MOST RECENT ROSES',
'pre': 'POPULAR VARIETALS REDS',
'pwh': 'POPULAR VARIETALS WHITES',
'pro': 'POPULAR VARIETALS ROSES'
```

#### Example Request

	$ curl 'https://quiniwine.com/api/pub/wineCategory/mre/0/2' \
		-H 'Accept:application/json' \
		-H 'Authorization:Bearer YourToken'

#### Example Response

```
{
  "nextskip": 2,
  "category": "MOST RECENT REDS",
  "items": [
    {
      "id": "546e01fe4c64580200000002",
      "Name": "Menage a Trois Red",
      "Winery": "Folie A Deux ",
      "Area": "California ",
      "Country": "U.S.A. ",
      "Varietal": "Zinfandel, Merlot, Cabernet Sauvignon ",
      "vintage": "2012",
      "nameClean": "Menage a Trois Red Folie A Deux  Zinfandel, Merlot, Cabernet Sauvignon",
      "Style": "",
      "Type": "Red",
      "count": 2,
      "rating": 75.21515728
    },
    {
      "id": "5244ae5ae1d839020000001e",
      "Name": "Merlot",
      "Winery": "Paloma",
      "Area": "Spring Mountain",
      "Country": "Usa ",
      "Varietal": "Merlot ",
      "vintage": "2006",
      "nameClean": "Merlot Paloma Merlot",
      "Style": "",
      "Type": "Red",
      "count": 2,
      "rating": 73.60516528000001
    }
  ]
}
```

- - -

### Taste API (IFRAME)

#### Definition

An independent Quini Taste Process which can be integrated into customer's website.

There are 2 types of Taste API
- Search and Taste. API provide a keywords search box for searching any wine in Quini's database, then choose a wine to start the taste.
- Taste a Particular Wine. Start to taste a particular wine by passing a Quini wine id, the API will start the taste directly from taste expectation page.

For the best experience, please make sure the width of the iframe container is larger than 960px, and the height has no limit or is bigger than 1550px.

>Search and Taste API
>IFRAME https://quiniwine.com/app/taste.html

>Taste a Particular Wine API
>IFRAME https://quiniwine.com/app/taste.html#/wine/(wine_id)

#### Example Iframe Code
	<iframe src="https://quiniwine.com/app/taste.html" style="border: none; width: 100%; height: 1550px;" scrolling="no"></iframe>
	<iframe src="https://quiniwine.com/app/taste.html#/wine/53c844980a30f2020000000e" style="border: none; width: 100%; height: 1550px;" scrolling="no"></iframe>

#### Iframe Parameters

- wine_id: start a particular wine taste by using Quini's wine id

- - -
