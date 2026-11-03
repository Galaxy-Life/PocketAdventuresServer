# Galaxy Life: Pocket Adventures Server
This repository contains all the code relative to Galaxy Life Pocket Adventures Server.  
If there is a large enough desire for a commercial server ran by Phoenix Network, we will consider it of there is funding to support it. We are currently (March 2024 and onwards) focusing on the Unreal Engine port for mobile instead of continuing work on Pocket Adventures.

If ever we obtain more resources and financial stability, we might look into further developing this project.

## Licensing
The license (Business Source License 1.1) can be found in `LICENSE.md`. This code can **NOT** be used for production or commercial purposes. It is purely here for those who want to tinker or play with the old Pocket Adventures while we are working to release our mobile version with Unreal Engine!

If Phoenix Network (the licensor) ever goes under or stops working on any Galaxy Life related product, this project will receive an MIT license.

## Setup
### Install dependencies
Install needed modules using the following command:
> python -m pip install -r requirements.txt

### Create the database
Edit DATABASE_URI in config.py if needed then enter the following command to create db schema
> flask db init

> flask db migrate

> flask db upgrade

## Running the backend
Either run it using:
> python app.py 

Or setup the backend using a WSGI server if you are running it in prod

## Connecting the app
Now we got the server running, we still need to modify the app to connect to it.

> [!NOTE]  
> This part is written by Michielvde. I tried my best to make it as dummy-friendly as possible, but if you have questions or if I made a mistake: please contact me directly via the Discord server :​)

1. Find a .apk file for the game (you can find one on most online APK stores, e.g. [on APKPure](https://apkpure.com/galaxy-life%E2%84%A2-pocket-adventures/com.digitalchocolate.igalaxy)). We'll call it GLPA.apk here.
2. Using [apktool](https://apktool.org/), run `apktool d GLPA.apk`
3. In `/lib/armeabi-v7a/`, find `libGalaxyLifePocketAdventures.so`. This is where the server URL is defined. Open this library using a hex editor, for example [HexEd](https://hexed.it/) (browser-based).
4. Search for `ios.galaxylifegame.com` (assuming your apk is from the original game and not from GL Reborn) at around address 0x0062AA9E.
5. Replace the server URL. Your replacement cannot be longer than 22 characters, which is the length of `ios.galaxylifegame.net`. If it is shorter, fill up towards 22 characters with null characters (00). You should always end with at least one null character! Be careful in this process, as messing this up will cause your app to crash. It is possible to connect to localhost servers (given that you run the apk on the same device as the one running the server of course, e.g. with an Android emulator).
6. Export and replace the library in the generated apktool folder.
7. Rebuild the .apk using `apktool b GLPA`.
8. Sign your app (recommended). This can be easiest done with a tool like [uber-apk-signer](https://github.com/patrickfav/uber-apk-signer) by running `java -jar uber-apk-signer.jar --apks GLPA.apk` on the output apk in the /dist folder (uses a default debug key, more advanced users probably want to provide their own key).
9. Install the app and test it!
