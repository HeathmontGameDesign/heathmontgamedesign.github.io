# Setting up your latptop for PyGame Zero

## Step 1: Install the Mu Code Editor

The Mu code editor is a simple code editor that is designed for beginners. It is a great tool for writing Python code and is perfect for PyGame Zero. You can download the Mu code editor from the following link:

[Download Mu Code Editor](https://codewith.mu/en/download)

Choose the version that is appropriate for your operating system (Windows for most of you, Mac for some, Linux if you are that kind of fun).

Run the installer and follow the instructions to install the Mu code editor on your laptop.

## Step 2: Setting the mode and environment variables

Once you have installed the Mu code editor, you need to set the mode to PyGame Zero. This is done by clicking on the mode button in the top right corner of the Mu code editor and selecting PyGame Zero from the list of options. This is not actually required, but it provides extra help (like the easy link through to the images folder).

### Environment Variables

When you load a PyGame Zero window, by default it opens quite a long way down the screen, which on small laptop screens can be quite annoying. We can fix this in the code each time, but it is easier to set the default variable inside Mu Code. To do so:

- Click on the Settings cog in the bottom right corner of the screen
- Choose the 'Python Environment' tab
- In the 'Environment Variables' box, type `SDL_VIDEO_WINDOW_POS=50,100` 
    - *(you can change the numbers if you want the window to open in a different position)*
- Click 'OK' and test your program to see if it opens in the right spot.
