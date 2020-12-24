# Cubiio 2 Notes, Links, Resources

## Abstract  
  - Back in Spring 2020, I have backed a project on Kickstarter – an attempt at making a thin desktop laser cutter/engraver – that project was [Cubiio 2](https://www.kickstarter.com/projects/cubiio/cubiio-2-laser-cutter-and-metal-engraver-with-autofocus). They succeeded at raising a ton of money from multiple crowd-funding platforms and the project has started shipping in December 2020.  
  - I have received my unit on December 18th, 2020 and immediately started seeing a lot of rough edges around software, documentation and tooling for what seemed like a really nice device.  
  - After fighting through a number of issues, working with support and a ton of experimentation, I have ended up with a set of notes on my experience. To help the community cope with the lack of official resources, I have decided to publish my [Roam Research](https://roamresearch.com/) notes in more-less raw format on this page. I'm going to keep the page up to date with my experiences and new information I find on the project or figure out myself. If you have any additional resources you think would make sense adding here, feel free to raise a Pull Request.  
  - **Last update:** December 23rd, 2020  

## Setup and installation  
  - Installed the app from Apple's App Store  
    - The app is useless on anything smaller than an iPas mini  
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

## GCode  
  - ### Inkscape  
    - Documentation: http://cubiio.muherz.com/file_convert_text.html  
    - **GCode Generation parameters (from official docs):**  
      - Laser ON command: M03  
      - Laser OFF command: M05  
      - Travel Speed : 600（the max of speed）  
      - Laser Speed : 600（the max of speed）  
      - Laser Power : 255（the max of power）  
      - Suggestion:  
        - Setting the max power(255)/speed(600mm/min).   
        - You can adjust speed and power in the APP.  
    - **Notes:**  
      - Did not work, generated gcode crashed the app reliably when loaded  
  - ### LaserWeb  
    - Open-source software for controlling Laser and CNC machines, allows you to use all kinds of vector files with machines working on GCode  
    - Official site: https://laserweb.yurl.ch/  
    - I've used the hosted version so far: https://laserweb.github.io/LaserWeb4/  
    - **Notes:**  
      - After opening it for the first time, you need to set up device parameters in the settings view:  
        - Machine section:  
          - MACHINE WIDTH: 300  
          - MACHINE HEIGHT: 220  
        - Gcode section:  
          - TOOL ON: M03  
          - TOOL OFF: M05  
          - PWM MIN S VALUE: 0  
          - PWM MAX S VALUE: 255  
      - ### GCode generation from SVG files:  
        - Add svg files  
        - Select objects  
        - Set up gcode for laser engraving (without fill)  
          - Click "Create Single"  
          - Select "Laser Cut"  
          - Enter laser power: 100% (the app will override this)  
          - Enter cut rate: 600 (the app will override this)  
        - Set up gcode for filling  
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

## Useful Resources  
  - [Official App Manual](https://cubiio.com/wp-content/uploads/2020/11/%E8%BB%9F%E9%AB%94%E8%AA%AA%E6%98%8E%E6%9B%B8%E8%8B%B1-s.pdf)  
  - [Official Quick guide](https://cubiio.com/support/guide/cubiio2-quick-guide/)  
  - [Recommended parameters for the laser](https://cubiio.com/wp-content/uploads/2020/12/Recommended-parameters-for-common-materials.pdf)  
  - **Inkscape**  
    - https://github.com/KnoxMakers/KM-Laser  
    - http://cubiio.muherz.com/file_convert_text.html  
