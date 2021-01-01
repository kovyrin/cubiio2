# Cubiio 2
## Abstract  
  - Back in Spring 2020, I have backed a project on Kickstarter – an attempt at making a thin desktop laser cutter/engraver – that project was [Cubiio 2](https://www.kickstarter.com/projects/cubiio/cubiio-2-laser-cutter-and-metal-engraver-with-autofocus). They succeeded at raising a ton of money from multiple crowd-funding platforms and the project has started shipping in December 2020.  
  - I have received my unit on December 18th, 2020 and immediately started seeing a lot of rough edges around software, documentation and tooling for what seemed like a really nice device.  
  - After fighting through a number of issues, working with support and a ton of experimentation, I have ended up with a set of notes on my experience. To help the community cope with the lack of official resources, I have decided to publish my [Roam Research](https://roamresearch.com/) notes in more-less raw format on this page. I'm going to keep the page up to date with my experiences and new information I find on the project or figure out myself.   
  - If you have any additional resources you think would make sense adding here, feel free to [raise an issue](https://github.com/kovyrin/cubiio2/issues/new) or a pull request on GitHub.  

  

**Last update:** January 1st, 2021  
  - See Updates History below for more details.  

  

## Setup and installation  
  - Installed the app from Apple's App Store  
    - The app is pretty much useless on anything smaller than an iPas mini  
  - Connected it to wifi using the iOS app on the iPad  
  - Switched the device to a static IP address  
    - Checked my router to see what ip did it assign to the device  
    - Mapped the device to a static IP  
    - Restarted the device to make sure it gets the new IP  
  - Opened the Web UI by accessing device's IP address  
    - No auth!  
  - Updated the firmware:  
    - http://IP.ADD.RE.SS/fw_update.htm  
    - Downloaded the latest firmware  
    - Uploaded it to the device  
    - It has restarted automatically  
    - Checked the firmware page again to make sure it was running the new version  

  

## Known Issues  
  - `"System is not ready. \nPlease perform initialization first."`  
    - According to support:  
      - > This error message means that the machine did not execute the "Go Home" program after completing the connection, or the "Go Home" did not complete.<br/><br/>After clicking the "Go Home" button, you will see the laser head move in the x and y directions, and finally stop at the upper right corner of the machine.  
    - After some troubleshooting, we've realized, that the machine has two separate sensors to detect if the lid is closed and it refuses to execute any commands if it is not.  
      - In my case, the lid was not closed completely on one side (had to push it to the side a little bit for it to close properly).  
  - **No SVG support in iOS app**  
    - [According to the team](https://www.kickstarter.com/projects/cubiio/cubiio-2-laser-cutter-and-metal-engraver-with-autofocus/comments?comment=Q29tbWVudC0zMTAyNzAzNw%3D%3D&reply=Q29tbWVudC0zMTAzNDEzMA%3D%3D), it should be available soon  
      - > The app will support native SVG in 1 or 2 months. For now, SVG files can be converted to g-code via Inkscape. The following instructions can help you step by step. Thank you. <br/><br/>https://cubiio.muherz.com/file_convert_text.html  

  

## G-Code  
  - ### Inkscape  
    - **Resources:**  
      - [Official guide for using Inkscape to print vector graphics](http://cubiio.muherz.com/file_convert_text.html)  
      - [A4 Template for Cubiio 2](https://cubiio.com/wp-content/uploads/2020/11/cubiio2_template_A4.zip)  
    - **G-Code Generation parameters (from official docs):**  
      - Laser ON command: M03  
      - Laser OFF command: M05  
      - Travel Speed : 600（the max of speed）  
      - Laser Speed : 600（the max of speed）  
      - Laser Power : 255（the max of power）  
      - Suggestion:  
        - Setting the max power(255)/speed(600mm/min).   
        - You can adjust speed and power in the APP.  
    - **Notes:**  
      - Did not work, generated G-Code file crashed the app reliably when loaded  
  - ### LaserWeb  
    - Open-source software for controlling Laser and CNC machines, allows you to use all kinds of vector files with machines working on G-Code  
    - Official site: https://laserweb.yurl.ch/  
    - I've used the hosted version so far: https://laserweb.github.io/LaserWeb4/  
    - **Notes:**  
      - After opening it for the first time, you need to set up device parameters in the settings view:  
        - Machine section:  
          - MACHINE WIDTH: 300  
          - MACHINE HEIGHT: 220  
        - G-Code section:  
          - TOOL ON: M03  
          - TOOL OFF: M05  
          - PWM MIN S VALUE: 0  
          - PWM MAX S VALUE: 255  
      - ### G-Code generation from SVG files:  
        - Add svg files  
        - Select objects  
        - Set up G-Code for laser engraving (without fill)  
          - Click "Create Single"  
          - Select "Laser Cut"  
          - Enter laser power: 100% (the app will override this)  
          - Enter cut rate: 600 (the app will override this)  
        - Set up G-Code for filling  
          - Click "Create Single"  
          - Select "Laser Fill Path"  
          - Select black color for stroke and for fill  
          - Line distance: 0.5 mm (for wood)  
            - Official Inkscape docs say it should be 0.1, but wood ends up burning too much at that setting  
          - Enter laser power: 100% (the app will override this)  
          - Enter cut rate: 600 (the app will override this)  
        - Click "Generate"  
        - After it is generated, click the green "Save" icon  
        - Rename the resulting file to something with a .txt extension  
        - Select it from the iPad app, move around as needed, scale, etc  
        - Then proceed to engrave  
          - Params I used for wood  
            - Power: 30%  
            - Speed: 100 (I have a feeling this is not mm/sec, but % because it does not allow you to go higher than 100)  
            - Passes: 1  
  - ### Commands Reference  
    - G0 - TRAVEL XY  
    - G1 - LINEAR XY  
    - G2 - CW_ARC XYIJ  
    - G3 - CCW_ARC XYIJ  
    - G20 - UNIT_INCH  
    - G21 - UNIT_MM (default)  
    - G90 - ABSOLUTE (default)  
    - G91 - INCREMENTAL  
    - M03 - LASER ON  
    - M05 - LASER OFF  
    - F - SPEED 0-600 (mm/min)  
    - S - POWER 0-255  

  

## Tips and Tricks  
  - Here I try to collect different settings, processes, etc I've used for engraving/cutting different materials, different content, etc.  
  - ### Engraving Black Walnut  
    - Burns really quickly, which allows for very high-resolution detailed engravings  
    - 20% at 25 mm/sec with two passes produce a really detailed and stable/deep engraving  
  - ### Engraving Olive Wood  
    - Much harder to burn: sapwood is dry and burns easily, heartwood is really oily, which leads to lower details in engravings.  
    - 50% at 25 mm/sec produces pretty good results both on sapwood and heartwood  
  - ### Printing vector graphics  
    - So far, the best process I've found for this is pretty counter-intuitive  
      - Render the vector picture into a 300-600dpi PNG image  
      - Send it to your iOS device  
      - Save the image into your photos (Share -> Save image)  
      - Add the image to your Cubiio app by clicking the picture icon and selecting the image from the photos.  
        - This produces a really high-resolution engraving with a lot of small details that are often lost when converting vector to G-Code  
    - For high-detail engravings, I have found that using lower power initial pass (20-25%) creates a nice detailed foundation, that could be deepened by a subsequent higher-power pass without the loss of detail.  
      - Doing a higher-power pass in one go produces more burned wood and reduces the details visible in the final result.  

  

## Useful Resources  
  - [Official Downloads](https://cubiio.com/support/download/)  
    - [Official FAQ Document](https://cubiio.com/wp-content/uploads/2020/12/Cubiio-2-FAQ.pdf)  
  - [Official App Manual](https://cubiio.com/wp-content/uploads/2020/11/%E8%BB%9F%E9%AB%94%E8%AA%AA%E6%98%8E%E6%9B%B8%E8%8B%B1-s.pdf)  
  - [Official Quick guide](https://cubiio.com/support/guide/cubiio2-quick-guide/)  
  - [Recommended parameters for the laser](https://cubiio.com/wp-content/uploads/2020/12/Recommended-parameters-for-common-materials.pdf)  
  - **Inkscape**  
    - https://github.com/KnoxMakers/KM-Laser  
    - http://cubiio.muherz.com/file_convert_text.html  
  - [Facebook Group for Cubiio users](https://www.facebook.com/groups/175632133023402)  
  - [G-Code Generator](https://www.inosyan.com/gcodegenerator) - tons of resources on using Cubiio (the first model) with Adobe Illustrator.  

  

## Updates History  
  - January 1st, 2021  
    - Added the Updates History section  
    - Added a Tips and Tricks section  
    - Added the official FAQ link  
  - December 23rd, 2020  
    - Cleaned up the notes and published the initial version of the document  
