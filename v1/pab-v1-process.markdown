<style>@import url("//readme.codeadam.ca/readme.css");</style>

## Process

This applciation will provide a directory with the following information:

1. A list of LEGO® themes.
2. Information on a theme and a list of sets within a chosen theme.
3. Information on a set and parts and minifigures inlcuded in the chosen set.
4. Information on individual parts.
5. Information on individual minifigures.

There is no backend for this project. Currently all backend functionality in completed by importing the raw [Rebrickable](https://rebrickable.com/downloads/) SQL files. In the future the import can be automated and run once a month using [CRON](https://en.wikipedia.org/wiki/Cron).

*** 

### Completed by Instructor

The [parts-v1](https://github.com/BrickMMO/parts-v1) includes a basic PHP core also used in [flow-v1](https://github.com/BrickMMO/flow-v2) and [stop-start-continue-v1](https://github.com/BrickMMO/stop-start-continue-v1). The core is available in the [php-w3css-core](https://github.com/codeadamca/php-w3css-core) repo.

There are twelve tables for this project provided by [Rebrickable](https://rebrickable.com/downloads/):

- colors
- elements
- inventories
- inventory_minifigs
- inventory_parts
- inventory_sets
- minifigs
- parts
- part_categories
- part_relationships
- sets
- themes

The database structure is as follows:

![BrickMMO Parts Schemea](/images/v1-schema.png)

> [!NOTE]  
> Review the database structure. Understand how themes are connected to sets, sets connected to pieces, etc...

The repo currently includes a rough scafolding:

| File | Description |
| - | - | 
| index.php | This file lists all the current themes. Each theme links to `theme.php` with a theme `id`. |
| theme.php | This file includes details about the theme and a list of all sets within the theme. Each set links to `set.php` wtih a set `id`. |
| set.php | This file includes details about the set and a list of all parts within the set. Each part links to `part.php` wtih a part `id`. |
| part.php | This file includes details about the part and a list of sets using that part and colours the part is avaialable in. This page can link back to `set.php` with a set `id`. |

These files include no design. They rach output only a few fields with functional links. 

*** 

### Steps

1. **Design**

    Using [Figma](https://www.figma.com/) (or your design tool of choice) mock up the application. The application should include the five pages (the four described above and one more display a minifigure).

    1. **Home Page**

        A list of all themes including theme name, a sample set image from this theme, and any other additional fields. This page content should somewhat mimic [https://rebrickable.com/sets/](https://rebrickable.com/sets/).

    2. **Theme Page**

        A theme profile including theme name, an image, and any other additional fields. Follwed by a list of all sets within the theme including set name, image, and any other additional fields. This page content should somewhat mimic [https://rebrickable.com/sets/star-wars/](https://rebrickable.com/sets/star-wars/).

    3. **Set Page**

        A set profile including set name, an image, and any other additional fields. Follwed by a list of all parts within the set including part name, image, ID, and any other additional fields along with all minifigures within the set. This page content should somewhat mimic [https://rebrickable.com/sets/75359-1/332nd-ahsokas-clone-trooper-battle-pack/](https://rebrickable.com/sets/75359-1/332nd-ahsokas-clone-trooper-battle-pack/).

    4. **Parts Page**

        A part profile including part name, an image, ID, and any other additional fields. Follwed by a list of sets the part is included in and a list of colours the part is aviaable in. This page content should somewhat mimic [https://rebrickable.com/parts/79389/bracket-1-x-1-1-x-2/](https://rebrickable.com/parts/79389/bracket-1-x-1-1-x-2/).

    5. **Minifigure Page**

        A minifigure profile including minifigure name, an image, ID, and any other additional fields. Follwed by a list of sets the minifigure is included in and a list of parts the minifigure is made of. This page content should somewhat mimic [https://rebrickable.com/minifigs/fig-002476/archer-black-falcon-black-legs-black-hips-pointed-shield-chin-guard/](https://rebrickable.com/minifigs/fig-002476/archer-black-falcon-black-legs-black-hips-pointed-shield-chin-guard/).

    The design style should mimic that of the [Passive Aggressive Password Machine](https://trypap.com/):

    - Nice large fonts
    - Simple layout
    - Well spaced out
    - Simple forms

    This project will use [W3.CSS](https://www.w3schools.com/w3css/). Review the W3.CSS website and avalable components and incorporate these into your design. 

    When complete, present your design to your instructor before moving on to the next step. 

2. **Local Server**

    Install a tool such as [MAMP](https://www.mamp.info/) on your computer. Open the MAMP progam and start the server. 

    Open the MAMP home page. It should be something like [http://localhost:8888](http://localhost:8888) on a Mac or [http://localhost](http://localhost] on a PC.

    In another tab open up [phpMyAdmin](https://www.phpmyadmin.net/). If you're using MAMP it will be something like [http://localhost:8888/phpMyAdmin/](http://localhost:8888/phpMyAdmin/) on a MAC or [http://localhost/phpMyAdmin/](http://localhost/phpMyAdmin/) on a PC.
    
3. **Database**

    Using phpMyAdmin create a new database named `brickmmo_parts` and import eleven of the SQL files in the `/imports` folder. Do not import `inventory_parts.sql.gz` just yet.

4. **Large SQL Imports**

    Try to import the `inventory_parts.sql.gz` file. Your browser will likely timeout. This is because browsers servers have a maximum file upload size of 2MB and the `inventory_parts.sql.gz` file is over 12MB. You will need to find a different method to import this file. Google the term `phpmyadmin inport large file` and see if you can implement a solution. 

5. **Source Code**

    Clone the [parts-v1](https://github.com/BrickMMO/parts-v1) repo into your MAMP root folder (or point MAMP to your cloned directory). Restart the server if needed. 

    Copy the `/.env.sample` file amd name it `/.env`. Change the database variables to `brickmmo_parts` and the MAMP defaults:

    ```php
    DB_HOST=localhost
    DB_DATABASE=brickmmo_parts
    DB_USERNAME=root
    DB_PASSWORD=root
    ```

    At this point you can open open the BrickMMO Colours home page. It will be empty, but there should be no PHP errors. 

        > [!TIP]  
        > Steps six through thirteen have a project task in GitHub. Make sure to assign these tasks when starting. And then use these tasks to create your branch and a pull request when done.

6. **Home Page**
    
    Modify the home page. Review the [Rebrickable Database Structure](https://rebrickable.com/api/v3/docs/) and the tables in phpMyAdmin to see what data is avaiable to put on the home page. Make these changes to the `/index.php` file. Don't worry too much about the design yet. 

        > [!NOTE]  
        > At this point do not worry about the design. Just use basic HTML, black text, and Times New Roman. No CSS needed. 

7. **Theme Page**

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

8. **Set Page**

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
    
9. **Sample Brick Image**

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

10. **Part Page**

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

11. **Minifigure Page**

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

12. **Apply Design**

    Apply the designs from step one!

13. **About Parts** 

    Update the [parts-about](https://github.com/BrickMMO/parts-about) Markdown! Add your names to the `v1.markdown` page. 

[&#10132; Back to V2](/coloupartsrs-about/v2)

---

<a href="https://brickmmo.com">
<img src="https://brickmmo.com/images/brickmmo-logo-horizontal.jpg" width="100">
</a>
