## How To Coding OCR With GUI
Hello everyone! I am Yair and I want to tell you hou to create Image OCR Tool With GUI in Python.
**Without further ado let's get started!**
![code](https://lh3.googleusercontent.com/vZWFq7IdKH_BAI_rukIyvOhpD46v8NfL7fNedW6wWFlbiaYJJWBBadEKfj5ZaC29QQTJlaLWZvgLfCdkK-vsxCClSK_dQZEK0eHQig=w1442-h714-rw-sm-pa-nu-v0 "code")
------------


To start, you will need to install the OCR engine that we will use. that in fact it will perform the conversion between the image and the text.
You can install it from [here](https://tesseract-ocr.github.io/tessdoc/Downloads.html "here"), but the Its source code is [here](https://github.com/tesseract-ocr/tesseract "here").
Choose your operating system type, and download. After that, courted here of course.

------------

Well, now we can continue.
Make sure you have PIP installed using the command   ` pip` in the terminal. If you get an error search the web for information to install pip.

Now we will install some libraries that we will use for the project. Using a command like this in the terminal: 

    pip install pytesseract PySimpleGUI pyperclip Pillow
This command will install the following libraries:
pytesseract - this is a bridge between the Tesseract software and Python.
Pillow - will be used by us to open the image.
pyperclip - will be used to create a button to copy the text that will be created.
PySimpleGUI - will be used by us to create the user interface.

------------
After we have downloaded the necessary libraries, we will start to code:
Open a new folder where you will save your code called 'My OCR Tools' or any other name you like. But don't forget that's what you called it otherwise you'll get into trouble with various problems... (if you copy from my code and the folder won't be with you, for example).
Then, in this folder, open a new file called 'main.py'. where we will enter our code.
Open the file and let's start coding!
First, we will import the libraries we installed earlier.

```python
import pytesseract
import PySimpleGUI as sg
import tkinter.filedialog #
import pyperclip
import webbrowser
from PIL import Image
```
Note that the 'Pillow' library we installed earlier is called 'PIL' in its import.
In addition, I imported two more libraries here:
* tkinter - to create a file saving window, which cannot be done in "PySimpleGUI".
* webbrowser - to open a browser window for the "About" option (you'll see after this what it means).
_______
Now we will define the path of the Tesseract software as follows:
```python
# Setting the path to the tesseract executable.
pytesseract.pytesseract.tesseract_cmd = 'C:\Program Files\Tesseract-OCR/Tesseract/tesseract.exe'
```
Anyway, as I hinted earlier in another matter, if you installed the Tesseract software in a different path, you will have to change it to the path you installed it in. I originally installed it in the folder I was working in (you can look at my Github), but you can install it wherever you want and change the path accordingly.

Now we will add a short piece of code for the continuation, you don't really need to refer to it unless you want there to be other types of files in which you can save the text (although it is unlikely that you will want that): 
```python
# A list of tuples. Each tuple is a file type. The first item in the tuple is the name of the file
# type, and the second item is the file extension.
filetypes = [
    ('Text', '*.txt'),
    ('All files', '*'),
]
```
The first thing we want to do now, is to create the responsible logic of the software, which is actually to convert the image to text. And in this way it will be done:
```python
def ocr_image(image_path, language):
    """
    It opens an image, converts it to grayscale, and then runs OCR on it
    :param image_path: The path to the image to be OCR'd
    :param language: The language to use for OCR
    :return: The text that was extracted from the image.
    """
    # Open the image and convert it to grayscale
    image = Image.open(image_path).convert('L')
    # Run OCR on the image
    text = pytesseract.image_to_string(image, lang=language)
    return text
```
This function, which receives two parameters that we will define later: image_path - in order to open the image and convert it to grayscale for scanning.
language - for the OCR engine to know which language to scan from the image.
The function returns the value "text". Which, in fact, is what his name is. The scanned text from the image.
___
Now, we will create the design of the user window. Following form,
We create a variable named "layout" that contains:
Title inside the "OCR Tool" window, a button to select a file, an option to select a language (you can add more languages to it...), a button to convert, a button to save the text, a button to copy the text, an "About" button and a cancel (close) button.

Then we create a variable called "window" that will display the "layout" variable. and add an icon to the window (can be downloaded from my code repository, or you can search for your own icon):
```python
# Creating the GUI.
layout = [
    [sg.Text("OCR Tool", font=('', 16))],
    [sg.Text("Select an image file: "), sg.Input(
        key="image_path"), sg.FileBrowse()],
    [sg.Text("Select a language: "), sg.InputCombo(
        ["En", "He"], key="language")],
    [sg.Button("Convert"), sg.Button("Save"), sg.Button("Copy"),
     sg.Button("About"), sg.Button("Cancel")]
]


# It creates a window with the title "OCR Tool" and the layout that was defined earlier.
window = sg.Window("OCR Tool", layout)
# It sets the icon of the window to the icon.ico file.
window.SetIcon("icon.ico")
```
And finally we'll add the option for the window to perform some action...

```python
while True:
    # Reading the window and getting the event and values.
    event, values = window.read()
    # It closes the window when the user clicks the "Cancel" button.
    if event in (None, "Cancel"):
        break
    # Checking if the user clicked the "Convert" button. If they did, it gets the image path and
    # language from the window, and then it converts the image to text.
    if event == "Convert":
        image_path = values["image_path"]
        language = values["language"]
        if language == 'En':
            language = 'eng'
        if language == 'He':
            language = 'heb'
        text = ocr_image(image_path, language)
        sg.popup('', "The text is now available. You can save it using the 'Save' button, or copy it using the 'Copy' button .")
    # Saving the text to a file.
    if event == "Save":
        filesave = tkinter.filedialog.asksaveasfilename(
            defaultextension='.txt', filetypes=filetypes)
        if filesave:
            f = open(str(filesave), 'w')
            f.write(text)
            f.close()
            sg.popup('', "Text saved to file!")
    # It copies the text to the clipboard.
    if event == "Copy":
        pyperclip.copy(text)
        sg.popup('', "The text has been copied!")
    # It opens a new tab in the default browser with the link https://github.com/Yair-T/Image-OCR-Tool,
    # and then it shows a popup with the text.
    if event == "About":
        webbrowser.open_new_tab('https://github.com/Yair-T/Image-OCR-Tool')
        sg.popup('About', "Programmed by YairT. The Open Source OCR engine is used: https://github.com/tesseract-ocr/tesseract")
```

In fact, always when the software is running, it will follow the following logic:
1. The software always reads the screen to check if the user performs an action (`event, values = window.read()`).
2. If the user clicks "Cancel" the software exits (`if event in (None, "Cancel"): break`).
3. If the user clicks "Convert" the software receives the values of "image_path" and "language" (`image_path = values["image_path"] language = values["language"]`), and then passes these values to the "ocr_image" function.
4. If the user clicks on "Save" the software opens a selection window for the location of saving the file with the scanned text and saves it.
5. If the user clicks on "Copy" the software copies the scanned text to the board.
6. If the user clicks on "About" the software transfers him to my Github repository and creates a popup with information about the software.

Well, we're almost done, just one more line of code left:
```python
window.close()
```

Excellent! We finished the program! Now you have your very own image to text conversion software.


For the full code: [https://github.com/Yair-T/Image-OCR-Tool/blob/main/main.py](https://github.com/Yair-T/Image-OCR-Tool/blob/main/main.py "https://github.com/Yair-T/Image-OCR-Tool/blob/main/main.py")

See you in the next post!

![Image OCR Tool](https://lh3.googleusercontent.com/o31-nAv-H7czKA9kg2WbXeX91PKWocWFj7zCeEwM7sYVDqlPVY5X9leoVoNpScoG4SLd6lnX23N7QZ0184K04qx5ibecw5WnskGyWA=w1494-h718-rw-sm-pa-nu-v0 "Image OCR Tool")
__________________________________________________________________________
credit:
The user interface of the software would not have been built if it were not for the [PySimpleGUI](https://www.pysimplegui.org/ "PySimpleGUI") package, many thanks to its developers.

Also, if there was no OCR engine (tesseract) the software would not perform its operation, many thanks to its developers as well!






