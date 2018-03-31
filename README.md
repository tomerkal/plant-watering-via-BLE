
# Plant watering via BLE
This is the final project in the Advanced Computer Systems course (IOT)  in Tel-Aviv University. 
by **omer brach & yuval ailer** 

**special thanks to:** 
- Prof. Sivan Toledo - course lecturer and project guidance.
- [Netafim company](http://www.netafimusa.com/) - for donating the gear needed to enable this project.
-  Dov Ailer - Electrical Engineering guidance and support.

### objectives
The purpose of the project is to create a plant watering system manageable via a blue-tooth connection. Do to the nature of battery enabled watering systems, the system needs to preform a high voltage operation (opening and closing the tap) for a short period of time, and for the most part of the day, to remain inactive and preserve battery power. For this reason we choose to use the TI [cc1350](http://www.ti.com/product/CC1350) low-power micro-controller. The following documentation will provide the needed information to recreate our project and use it on your own. 

## Getting Started

**The project demands work on three levels:** 
 1. Android app for controlling the system (code provided).
 2. TI cc1350 on-board development (code provided).
 3. build an electric circle (sketch and hardware list provided).  

### 


### list of materials:
#### hardware:

|gear                |info                          |more                         |
|----------------|-------------------------------|-----------------------------|
| [TI cc1350 Launchpad](http://www.ti.com/tool/LAUNCHXL-CC1350-4) |`luch pad`            |![](http://www.ti.com/diagrams/med_launchxl-cc1350-4_launchxl-cc1350-4-topsideprof.jpg)            |
|android enabled device          |`prefertably 27 api`            |![](http://www.media-kom.com/wp-content/uploads/2015/10/android.jpg)            |
|2 X resistors          |`410 Ohm`|![not the same part, image for reference only](https://media.rs-online.com/t_large/R0132494-01.jpg)|
|2 X relays          |5V enabled, can transfer up to 12V, 1A. we uesd: m45h| ![not the same part, image for reference only](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR_A1x2YZGwe1IPF8xiabJIDGi9yS8xuDPEZj3YIaNZiZ8MJKOfkw) |
| battery          |standard 9V|![](https://cdn.aws.toolstation.com/images/141020-UK/800/91814.jpg)|
|2 X trasistors          |`NPN`, we used: 2n2222A|![image](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTkEW9aSJbzSFaZdxWrgoBPG_ceJttPhXfLmSmVyiYYoqhiTSFtSQ) |
|voltage regulator          |`from 9V to 5V. we used L7805CV`|![not the same part, image for reference only](https://cdn.sparkfun.com//assets/parts/4/4/2/4/12766-01.jpg)|
|soile muister sensor          |we used the [Sparkfun one](https://www.sparkfun.com/products/13322)|![](https://cdn.sparkfun.com//assets/parts/1/0/6/1/0/13322-01.jpg)|
|breadboard          |`or ready to soldier one`|![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/400_points_breadboard.jpg/220px-400_points_breadboard.jpg)|
|wires          |`diffrent colors and lengths`|![](http://www.dfliq.net/wp-content/uploads/2014/03/Electrical-wirings-768x326.jpg)
|Electric tap         |We used the [Netafim Aqua-Pro](http://www.netafimusa.com/) that was generously donated to us by the company|![](https://growshop.co.il/wp-content/uploads/2017/06/NETAFIM_IRRIGATION_COMPUTER.jpg)|
|name          |`what`|![]()|
|name          |`what`|![]()|

#### software:
1. [Android studio](https://developer.android.com/studio/index.html) - for creating and editing android apps
2. [TI Code composer studio](http://www.ti.com/tool/CCSTUDIO) - for developing software on the cc1350
3. [TI Sensor controller studio](http://www.ti.com/tool/SENSOR-CONTROLLER-STUDIO) - to test and verify the soil moisture sensor  

## developing process

### Android application:

­We used **Android Studio 3.0.1** for developing the application.

For implementing a BLE application we were assisted by this [example](https://github.com/netanelyo/AdvancedComputerSystems), 
And this [manual](https://www.allaboutcircuits.com/projects/how-to-communicate-with-a-custom-ble-using-an-android-app/).

- We changed the Read / Write functions in the following manner:

- We used the write function to open and close the tap, and the read function to receive information from the soil moisture sensor.

#### Using the application:

 - After opening the application, a connection window will appear, with available BLE devices. Find the TI-RTOS board by the name **SimpleBlePeripheral**.
 - After pressing on it, the application will connect to the board and open a window with three buttons – **Open tap**, **Close tap**, **Read humidity**. When pressing the **open **/ **close tap** buttons, the application will write to the board via BLE, and the board will then open / close the tap.
 - When pressing the **Read humidity button** – the application will read information from the soil moisture sensor, and will show one of the three messages:

 - -  If the moisture is too low – **"Please water me!"**.
 -  - If the moisture it too high – **"I'm flooding! Stop the water!" **.
 -  - If the moisture is just right – **"I feel sooo good!"**.

- Upon reading these messages, the user will know if needed to open, close, or do nothing with the tap.

#### About the process:

First of all, this was our first time dealing with an android application, so we spent some time learning the **Android studio** software and implementing some basic apps.

After that, using the example and the manual that were mentioned above, we built the write functions that open and close the tap using different characters to represent each action and transmitting them over **BLE**.

Finally, we added the read function that receives data from the soil moisture sensor, going through the **TI-RTOS** board and transmitted via **BLE** to the app, and according to the character transmitted shows a different message upon the screen.

### TI-RTOS application:

We used **Code Composer Studio 7.3.0** for developing the application running on the board.

For implementing the TI-RTOS application, we were assisted by two examples from the **SimpleLink SDK: adcsinglechannel** and **simple_peripheral**.

 For the **BLE** connection, we used the **simple_peripheral** example.

We adjusted the write function to send **200ms** signals to the data outputs **(DIO12, DIO15)** of the board, in order to open and close the tap when receiving the command from the board via **BLE**.

The read function was adjusted based on the **adcsinglechannel** example to read data from the soil moisture sensor **(DIO23)** and send characters over **BLE** to the mobile application *(sending "3" represents low moisture, "4" represents good moisture, and "7" represents too much moisture)*.

#### About the process:

In **HW5**, we learned how to use the **BLE** protocol to read and write to the **TI-RTOS** board using the **simple_peripheral** example. We used this example to try and write characters that will send signals through the different data outputs of the board, and for the first stage turn on / off LEDs.

After completing that, we connected the data outputs of our choice **(DIO12, DIO15)** to the electrical circuit that will open and close the tap *(further explanations regarding how the electric circuit works will be discussed below).*

We then were assisted with the **adcsinglechannel** example to read data from the soil moisture sensor. We used the **Sensor Controller Studio** software to learn about the values returned from the sensor, and then again used LEDs connected to the board to understand the different levels that we reach in different moisture situations (putting the sensor in the air, in dry soil, wet soil and in a glass of water). After understanding that, we adjusted the read function in the code to read data from the sensor, and then send characters representing each level of moisture to the mobile app via **BLE**.

##########################
End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone who's code was used
* Inspiration
* etc




# Welcome to StackEdit!

Hi! I'm your first Markdown file in **StackEdit**. If you want to learn about StackEdit, you can read me. If you want to play with Markdown, you can edit me. If you have finished with me, you can just create new files by opening the **file explorer** on the left corner of the navigation bar.


# Files

StackEdit stores your files in your browser, which means all your files are automatically saved locally and are accessible **offline!**

## Create files and folders

The file explorer is accessible using the button in left corner of the navigation bar. You can create a new file by clicking the **New file** button in the file explorer. You can also create folders by clicking the **New folder** button.

## Switch to another file

All your files are listed in the file explorer. You can switch from one to another by clicking a file in the list.

## Rename a file

You can rename the current file by clicking the file name in the navigation bar or by clicking the **Rename** button in the file explorer.

## Delete a file

You can delete the current file by clicking the **Remove** button in the file explorer. The file will be moved into the **Trash** folder and automatically deleted after 7 days of inactivity.

## Export a file

You can export the current file by clicking **Export to disk** in the menu. You can choose to export the file as plain Markdown, as HTML using a Handlebars template or as a PDF.


# Synchronization

Synchronization is one of the biggest features of StackEdit. It enables you to synchronize any file in your workspace with other files stored in your **Google Drive**, your **Dropbox** and your **GitHub** accounts. This allows you to keep writing on other devices, collaborate with people you share the file with, integrate easily into your workflow... The synchronization mechanism takes place every minute in the background, downloading, merging, and uploading file modifications.

There are two types of synchronization and they can complement each other:

- The workspace synchronization will sync all your files, folders and settings automatically. This will allow you to fetch your workspace on any other device.
	> To start syncing your workspace, just sign in with Google in the menu.

- The file synchronization will keep one file of the workspace synced with one or multiple files in **Google Drive**, **Dropbox** or **GitHub**.
	> Before starting to sync files, you must link an account in the **Synchronize** sub-menu.

## Open a file

You can open a file from **Google Drive**, **Dropbox** or **GitHub** by opening the **Synchronize** sub-menu and clicking **Open from**. Once opened in the workspace, any modification in the file will be automatically synced.

## Save a file

You can save any file of the workspace to **Google Drive**, **Dropbox** or **GitHub** by opening the **Synchronize** sub-menu and clicking **Save on**. Even if a file in the workspace is already synced, you can save it to another location. StackEdit can sync one file with multiple locations and accounts.

## Synchronize a file

Once your file is linked to a synchronized location, StackEdit will periodically synchronize it by downloading/uploading any modification. A merge will be performed if necessary and conflicts will be resolved.

If you just have modified your file and you want to force syncing, click the **Synchronize now** button in the navigation bar.

> **Note:** The **Synchronize now** button is disabled if you have no file to synchronize.

## Manage file synchronization

Since one file can be synced with multiple locations, you can list and manage synchronized locations by clicking **File synchronization** in the **Synchronize** sub-menu. This allows you to list and remove synchronized locations that are linked to your file.


# Publication

Publishing in StackEdit makes it simple for you to publish online your files. Once you're happy with a file, you can publish it to different hosting platforms like **Blogger**, **Dropbox**, **Gist**, **GitHub**, **Google Drive**, **WordPress** and **Zendesk**. With [Handlebars templates](http://handlebarsjs.com/), you have full control over what you export.

> Before starting to publish, you must link an account in the **Publish** sub-menu.

## Publish a File

You can publish your file by opening the **Publish** sub-menu and by clicking **Publish to**. For some locations, you can choose between the following formats:

- Markdown: publish the Markdown text on a website that can interpret it (**GitHub** for instance),
- HTML: publish the file converted to HTML via a Handlebars template (on a blog for example).

## Update a publication

After publishing, StackEdit keeps your file linked to that publication which makes it easy for you to re-publish it. Once you have modified your file and you want to update your publication, click on the **Publish now** button in the navigation bar.

> **Note:** The **Publish now** button is disabled if your file has not been published yet.

## Manage file publication

Since one file can be published to multiple locations, you can list and manage publish locations by clicking **File publication** in the **Publish** sub-menu. This allows you to list and remove publication locations that are linked to your file.


# Markdown extensions

StackEdit extends the standard Markdown syntax by adding extra **Markdown extensions**, providing you with some nice features.

> **ProTip:** You can disable any **Markdown extension** in the **File properties** dialog.


## SmartyPants

SmartyPants converts ASCII punctuation characters into "smart" typographic punctuation HTML entities. For example:

|                |ASCII                          |HTML                         |
|----------------|-------------------------------|-----------------------------|
|Single backticks|`'Isn't this fun?'`            |'Isn't this fun?'            |
|Quotes          |`"Isn't this fun?"`            |"Isn't this fun?"            |
|Dashes          |`-- is en-dash, --- is em-dash`|-- is en-dash, --- is em-dash|


## KaTeX

You can render LaTeX mathematical expressions using [KaTeX](https://khan.github.io/KaTeX/):

The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$

> You can find more information about **LaTeX** mathematical expressions [here](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference).


## UML diagrams

You can render UML diagrams using [Mermaid](https://mermaidjs.github.io/). For example, this will produce a sequence diagram:

```mermaid
sequenceDiagram
Alice ->> Bob: Hello Bob, how are you?
Bob-->>John: How about you John?
Bob--x Alice: I am good thanks!
Bob-x John: I am good thanks!
Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

Bob-->Alice: Checking with John...
Alice->John: Yes... John, how are you?
```

And this will produce a flow chart:

```mermaid
graph LR
A[Square Rect] -- Link text --> B((Circle))
A --> C(Round Rect)
B --> D{Rhombus}
C --> D
```
