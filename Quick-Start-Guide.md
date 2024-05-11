This quick start guide will help you get a RomM instance up and running. It is split into 3 parts:

- Prepare - preparing the environment before executing any code.
- Build
- Configure

---

### Prepare

This guide will assume that you already have the following done, if not - stop here and come back when you do.

1. Docker and Docker Compose installed on your system of choice.
2. OpenSSL on the machine that will run RomM.
3. A Twitch.tv account.
4. 2 Factor Authentication set up on your Twitch account. *This is a requirement to get a dev account and an IGDB key*
5. A MobyGames account. *(optional)
6. Roms organized in the correct format.

##### Getting an IGDB Key

This will allow you to generate a key to pull the metadata for your roms using Twitch's IGDB.

1. Navigate to [Twitch's dev site](https://dev.twitch.tv/https:/).
2. Hit ****Log in with Twitch** in the top right of the screen and log in with your Twitch account.
3. Select **Applications** on the left hand menu, followed by the ***Register Your Application*** button.

![](assets\Register_Application.png)

4. You will now be brought to the registration screen, fill out the following information.

   1. Name: A **Globally Unique** name for your application.  If you try to enter a name that already exists, it won't work and gives you an unhelpful error message.  I recommend following this format: Romm-(MD5 Hash of current date in UTC), as it is nearly impossible that it exists already. e.g. *RoMM-f3e5102b2d4e87cd283fc65907e8a8f0*.
   2. OAuth Redirect URLs: The FQDN where your RomM instance will be hosted.  You only need to specify http://localhost if you intend to run/access it locally, otherwise you should enter the FQDN of your domain, this needs to be accesible via https.
   3. Category: Application Integration.
   4. Client Type: Confidential.

![](assets\IGDB_Creation.png)

5. Click ***Create*** to create the application.
6. You will now be taken back to the **Applications** screen, select ***Manage*** next to the application you just created.
7. Copy the ****Client ID**** near the bottom of the page.
8. Select **New Secret** to generate a client secret for us to use.
9. Copy both of these somewhere for safe keeping, we will need them later.  Obviously do not share these with anyone, the demo values here were removed after success.

![](assets\IGDB_Secret.png)

##### Getting a MobyGames Key

This will allow you to generate a key to pull the metadata for your roms using MobyGames. *Not Required*

> To Be Implemented

##### Generating the Secret Key

This step will generate a key that is used in the authorization of RomM.  Without this, you will be unable to log in and use the platform.  This step **must** be done on the machine that will be hosting RomM.

1. On the machine that will host RomM, launch the terminal.
2. Run the following command:

    ```sh
    openssl rand -hex 32
    ```
3. Copy the output and save it with your **Client ID** and **Client Secret** from the previous step.

![](assets\OpenSSL.png)

---

### Build

Now that we have everything gathered, we can begin getting your instance set up!

1. Download a copy of the latest docker-compose.example.yml file from GitHub.
2. Edit the file and modify the following values to configure the database.

   1. MYSQL_ROOT_PASSWORD: Sets the root password of the database. Use a unique and secure password. *use a password generator for simplicity*
   2. MYSQL_DATABASE: Sets the database name for RomM.  This can be modified - but it's not necessary.
   3. MYSQL_USER: User to connect to the database with. This can be modified - but it's not necessary.
   4. MYSQL_PASSWORD: Password for the user to connect to the database with.  Use a unique and secure password. *use a password generator for simplicity*
3. Modify the following values in the **environment** to configure the application. *-- Other values can be changed, but should not be done unless you know what you are doing, and are outside the scope of this guide*

   1. DB_NAME: Name of the database set in the database section.
   2. DB_USER: Name of the user to connect to the database.
   3. DB_PASSWD: Password of the user to connect to the database.
   4. IGDB_CLIENT_ID: Client ID obtained in the **Getting an IGDB Key** step.
   5. IGDB_CLIENT_SECRET: Client Secret obtained in the **Getting an IGDB Key** step.
   6. ROMM_AUTH_SECRET_KEY: Key generated in the **Generating the Secret Key** step.
   7. ROMM_AUTH_USERNAME: Default username you will access RomM with.
   8. ROMM_AUTH_PASSWORD: Password for the ROMM_AUTH_USERNAME user. Use a unique and secure password. *use a password generator for simplicity*
      **If you need to listen on a port other than 80, you can change it in the *port* section**
4. Modify the following values in the **volumes** to configure the application.

   1. /path/to/library: Path to the directory where your rom files will be stored.
   2. /path/to/assets: Path to the directory where you will store your saves, etc.
   3. /path/to/config: Path to the directory where you will store the config.yml
5. Save the file as *docker-compose.yml* instead of *docker-compose.example.yml*.  It should look like the image here, never reveal these values:
![](assets\docker-compose.png)
6. Open the terminal and navigate to the directory containing the docker-compose file.
7. Run ```docker compose up -d``` to kick off the docker pull.  You will see it pull the container and set up the volumes and network.
![](assets\docker_command.png)
8. Run ```docker ps -f name=romm``` to verify that the containers are running.
9. Open a web browser and navigate to ```http://localhost```. You should be greeted with the RomM login page.
![](assets\romm_login\screen.png)
10. Log in with the credentials you set in the docker-compose file.

### Configure

Now that the container is running, we will configure it by importing your roms.

##### Uploading Your Roms via Web Interface

This method is certainly viable, but not recommended if you have a lot of roms and/or multiple platforms.  It is good for adding after the fact as your collection grows, but wouldn't be recommended for the first set up, nor for multi-file roms.

1. Log into RomM with your user credentials.
2. Navigate to *Library* -> *Upload roms*.
3. Select the platform, then click *ADD ROMS* and select the roms you want to upload in the file selector that appears.
4. Click *UPLOAD* to begin uploading the roms.
![](assets\upload_rom_webui.png)
5. Repeat for all the roms/platforms you have.

##### Importing Your Roms via Scanner

This method is generally the fastest and recommended for first time setup.

1. Stop your RomM instance. ```docker compose down``` if you are in the terminal and directory containing the docker-compose file, otherwise ```docker stop romm```.
2. Go to the library folder created by RomM, set in the docker-compose file under *:/romm/library* and create a folder named ```roms```.
3. Copy your platform folders/rom files (in the [correct folder/file structure](\File-Structure.md)) into the ```roms``` folder you created.
4. Start the RomM instance back up. ```docker compose up -d``` if you are in the terminal and directory containing the docker-compose file, otherwise ```docker start romm```.
5. Log into RomM with your user credentials.
6. Navigate to *Library* -> *Scan*.
7. The system will now begin scanning the rom files and applying metadata to them.  You can click on any of the items that it has tagged to see the metadata it pulled without having to stop the scan.
![](assets\romm_scanning.png)
8. After the scan completes, click the RomM logo to go back to the main screen.  You should see the platforms and recent games it has scanned.  You are now ready to rock and RomM!
![](assets\romm_success.png)