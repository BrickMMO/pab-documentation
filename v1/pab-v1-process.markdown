<style>@import url("//readme.codeadam.ca/readme.css");</style>

## Process

This applciation will provide a place for the LEGO® to post the inventory of Pick-a-Brick wall in LEGO® stores. In this first phase we will create a directory of LEGO® stores that scrapes the LEGO® website for location information. This phase wil include:

1. A list of LEGO® stores.
2. Store profiles including store name, address, photo, hours, etc...

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

    If writting a query in PHP is a hard step, start by just outputting the region name. Then write and output your query. And once you are happy with it, try running it.

8. **Countries**

    Still within the first loop, use the data in the `$country` variable to add a record to the countries table.

   Make sure to use the region ID from the previous step as the value for the `region_id` field. 

   On the home page you will link each theme to `\theme.php?id=1`. The number one will be replaced with the `id` value form the theme table. It will look something like this:

    ```php
    <?php while($theme = mysqli_fetch_assoc($result)): ?>

        <a href="theme.php?id=<?=$theme['id']?>"><?=$theme['name']?></a>
        
    <?php endwhile; ?>
    ```

    Modify the `profile.php` page. Review the [Rebrickable Database Structure](https://rebrickable.com/api/v3/docs/) and the tables in phpMyAdmin to see what data is avaiable to put on the theme page.

    Add some of the following data:

    - Colour ID
    - Number of themes
    - A sample image (you can grab one from a set)
        
    Include a list of sets including:

    - Set name
    - Set ID
    - Image
    - Number of parts
    - Release date

10. **Set Page**

    On the theme page you will link each set to `\set.php?id=1`. The number one will be replaced with the `id` value form the set table. 

    Modify the `set.php` page. Review the [Rebrickable Database Structure](https://rebrickable.com/api/v3/docs/) and the tables in phpMyAdmin to see what data is avaiable to put on the set page.

    Add some of the following data:

    - Set ID
    - Number of parts
    - Image
        
    Include a list of parts including:

    - Part name
    - Part ID
    - Image
    
11. **Sample Brick Image**

    You can add a sample brick by using the [Rebrickable](https://rebrickable.com/downloads/) media. For example, the following URL:
    
    ```
    https://cdn.rebrickable.com/media/thumbs/parts/ldraw/4/3003.png/85x85p.png
    ```

    Will display the following image:

    ![Red 3001 Brick](https://cdn.rebrickable.com/media/thumbs/parts/ldraw/4/3003.png/85x85p.png)

    The image URL consists of the following parts:

    - Rebrickable media URL: `https://cdn.rebrickable.com/media/thumbs/parts/ldraw/`
    - The colour ID using the `rebrickable_id` value from the `colours` table: `<?=$colour['rebrickable_id']?>`
    - The LEGO® brick ID using, you can look these up at the [LEGO® Pick-a-Brick Store](https://www.lego.com/en-ca/pick-and-build/pick-a-brick)
    - And the width and height of the image

12. **Part Page**

    On the set page you will link each part to `\part.php?id=1`. The number one will be replaced with the `id` value form the part table. 

    Modify the `part.php` page. Review the [Rebrickable Database Structure](https://rebrickable.com/api/v3/docs/) and the tables in phpMyAdmin to see what data is avaiable to put on the part page.

    Add some of the following data:

    - Part ID
    - Image
    
    Include a list of sets the part is included in:

    - Part name
    - Part ID

    Link each part back to `/part.php?id=1`.

    Include a list of minifigures the part is included in:

    - Minifigure name
    - Minifigure ID

    Link each set back to `/minifigure.php?id=1`.

13. **Minifigure Page**

    On the set and part page you will link each minifigure to `\minifigure.php?id=1`. The number one will be replaced with the `id` value form the minifigure table. 

    Create a file named `minifigure.php` page. Review the [Rebrickable Database Structure](https://rebrickable.com/api/v3/docs/) and the tables in phpMyAdmin to see what data is avaiable to put on the minifigure page.

    Add some of the following data:

    - Minifigure ID
    - Minifigure Name
    - Image
        
    Include a list of sets the minifigure is included in:

    - Set name
    - Set ID

    Link each set back to `/set.php?id=1`.

    Include a list of parts the minifigure requires:

    - Part name
    - Part ID

    Link each part back to `/part.php?id=1`.

14. **Apply Design**

    Apply the designs from step one!

15. **About Parts** 

    Update the [parts-about](https://github.com/BrickMMO/parts-about) Markdown! Add your names to the `v1.markdown` page. 

[&#10132; Back to V2](/coloupartsrs-about/v2)

---

<a href="https://brickmmo.com">
<img src="https://brickmmo.com/images/brickmmo-logo-horizontal.jpg" width="100">
</a>
