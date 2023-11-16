<style>@import url("//readme.codeadam.ca/readme.css");</style>

## Process

This applciation will provide a place for the LEGO® to post the inventory of Pick-a-Brick wall in LEGO® stores. In this first phase we focus on creating an import script to import all LEGO® store information into a database.

There is no backend for this project. Currently all backend functionality in completed by running the import script. In the future the import can be automated and run once a month using [CRON](https://en.wikipedia.org/wiki/Cron).

*** 

### Completed by Instructor

The [pab-v1](https://github.com/BrickMMO/pab-v1) repo includes a starting point for the import. The store JSON fle taken from the LEGO® website has been copied to the `/import.json` file. The `/import.php` file inports the JSON using `file_get_contents()`, loops through the list of stores, and uses `cURL` to scrapes store profile page from the LEGO® website:

```php
$url = 'import.json';
$stores = json_decode(file_get_contents($url), true);

foreach($stores['storesDirectory'] as $key => $country)
{

    foreach($country['stores'] as $key => $store)
    {

        $url = 'https://www.lego.com/en-my/stores/store/'.$store['urlKey'];

        $curl = curl_init($url);
        
        curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
        
        $response = curl_exec($curl);
        curl_close($curl);
     
        echo '<pre>';
        print_r($store);
        print_r(($response));
        echo '</pre>';

        sleep(10);

        echo '<hr>';

        die();

    }

}
```

![LEGO Stores](../images/lego-stores.png)

> [!NOTE]  
> Review the store directory to see what data is aviaable to be scraped and placed in the database.

*** 

### Steps

1. **Local Server**

    Install a tool such as [MAMP](https://www.mamp.info/) on your computer. Open the MAMP progam and start the server. 

    Open the MAMP home page. It should be something like [http://localhost:8888](http://localhost:8888) on a Mac or [http://localhost](http://localhost] on a PC.

    In another tab open up [phpMyAdmin](https://www.phpmyadmin.net/). If you're using MAMP it will be something like [http://localhost:8888/phpMyAdmin/](http://localhost:8888/phpMyAdmin/) on a MAC or [http://localhost/phpMyAdmin/](http://localhost/phpMyAdmin/) on a PC.
    
2. **Database**

    Using phpMyAdmin create a new database named `brickmmo_pab` and import the three SQL files in the `/imports` folder.

3. **Source Code**

    Clone the [parts-v1](https://github.com/BrickMMO/pab-v1) repo into your MAMP root folder (or point MAMP to your cloned directory). Restart the server if needed. 

    Copy the `/.env.sample` file amd name it `/.env`. Change the database variables to `brickmmo_parts` and the MAMP defaults:

    ```php
    DB_HOST=localhost
    DB_DATABASE=brickmmo_parts
    DB_USERNAME=root
    DB_PASSWORD=root
    ```

    At this point you can open open the BrickMMO PAB import page. It will output some location related data.

    > [!TIP]  
    > Steps six through thirteen have a project task in GitHub. Make sure to assign these tasks when starting. And then use these tasks to create your branch and a pull request when done.

4. **Regions**

    The stores are gouped by region (contenant) and country.

    Within the first loop (but outside the second loop), there is a `$country` variable. Inside this variable there is a region name (two capital letters), a country name (two capital letters), and a country ID (a long string of characters and numbers and dashes).

    Using this data add a record to the regions. However, because multiple countries can belong to a region, you will needd to check to see if the region has already been inserted. You code will be something like this:

    ```
    QUERY DATABASE FOR REGION
    
    IF REGION IS FOUND
        PUT FOUND REGION ID IN A VARIABLE
    
    ELSE IF REGINO IS NOT FOUND
        INSERT NEW REGION 
        PUT NEW REGION ID IN A VARIABLE
    ```

    If writting a query in PHP is a hard step, start by just outputting the region name:
    
    ```php
    foreach($stores['storesDirectory'] as $key => $country)
    {
        print_r($country);
        echo $country['region'];
    }
    ```
    
    Then write and output your query:

    ```php
    foreach($stores['storesDirectory'] as $key => $country)
    {
        $query = 'INSERT INTO regions (
                name
            ) VALUES (
                "'.$country['region'].'"
            )';
        echo $query;
    }
    ```
    
    And once you are happy with it, try running it.

    ```php
    foreach($stores['storesDirectory'] as $key => $country)
    {
        $query = 'INSERT INTO regions (
                name
            ) VALUES (
                "'.$country['region'].'"
            )';
        mysqli_query($connect, $query);
    }
    ```

5. **Countries**

    Still within the first loop, use the data in the `$country` variable to add a record to the countries table.

    Make sure to use the region ID from the previous insert step as the value for the `region_id` field. 

6. **Stores**

    Once you have the code inserting regions and countries (without inserting duplicates) use the second loop to insert stores. For now just use the data that is available in the `/import.json` file.

    ```php
    (
        [storeId] => 100
        [name] => LEGO® Store Mirdiff City Centre
        [phone] => 04 231 6321
        [state] => 
        [openingDate] => 
        [certified] => 1
        [additionalInfo] => This LEGO® Store is owned and operated by a licensed independent third-party. Offers, promotions, pricing, and inventory may vary, and the LEGO VIP loyalty program will not be available. Gift cards and product returns ordered through LEGO.com will not be accepted. Please contact the store directly for more information.
        [storeUrl] => https://www.lego.com/stores/stores/mirdiff-city-centre
        [urlKey] => mirdiff-city-centre
        [isNewStore] => 
        [isComingSoon] => 
        [__typename] => StoreInfo
    )
    ```

    Use this data to create a loation insert query. Fill the `name`, `phone`, `info`, `certified`, and `reference_id` fields.
    
7. **Scraped Store Data**

    There is already code in the `/import.php` file that scrapes the HTML from the official LEGO® locations website. You will need to use a variety of PHP string functions to extract important values such as:

    - address
    - hours
    - store features
    - image

    You will need to add fields to the `locations` table and possible a whole new table. 

8. **About PAB** 

    Update the [pab-about](https://github.com/BrickMMO/    Update the [pab-about](https://github.com/BrickMMO/parts-about) Markdown! Add your names to the `v1.markdown` page. 
-about) Markdown! Add your names to the `v1.markdown` page. 

[&#10132; Back to V2](/coloupartsrs-about/v2)

---

<a href="https://brickmmo.com">
<img src="https://brickmmo.com/images/brickmmo-logo-horizontal.jpg" width="100">
</a>
