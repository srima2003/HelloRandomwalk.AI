Description of the Code
This code is designed to extract text from an image using Optical Character Recognition (OCR) with the pytesseract library. It organizes the extracted text into a structured format, specifically identifying headings and their corresponding subheadings. The code is broken down into several key components:

1. Library Imports
python
Copy code
from PIL import Image 
import pytesseract
import re
PIL (Pillow): A Python Imaging Library used to open and manipulate images.
pytesseract: A Python wrapper for Google’s Tesseract-OCR Engine, which allows for the extraction of text from images.
re: The regular expression module used for matching text patterns.
2. Function to Clean Text
python
Copy code
def clean_text(text):
    # Remove unwanted characters and symbols
    text = re.sub(r'[_\|\-—]', '', text)  # Remove underscores, pipes, dashes
    return text
Purpose: This function processes the raw text output from OCR to remove specific unwanted characters (underscores, pipes, dashes).
Input: A string of text.
Output: The cleaned text with unwanted characters removed.
3. Function to Extract Text from Image
python
Copy code
def extract_text_from_image(image_path):
    # Load the image
    img = Image.open(image_path)
    
    # Perform OCR to extract raw text
    raw_text = pytesseract.image_to_string(img)

    # Clean the raw text
    clean_raw_text = clean_text(raw_text)
    
    # Split the cleaned text into lines
    lines = [line.strip() for line in clean_raw_text.split('\n') if line.strip()]
    
    # Initialize dictionary to store organized text
    organized_output = {}
Purpose: This function handles the entire process of loading the image, performing OCR, and organizing the extracted text into a structured format.
Input: The file path of the image.
Output: A dictionary containing headings as keys and their corresponding subheadings as values.
4. Pattern Recognition for Headings
python
Copy code
    heading_pattern = re.compile(r'^[A-Z][a-z\s]+$|^[A-Z][a-z]+\s[A-Z][a-z]+$')  # Simple heuristic for headings
    current_key = None
Purpose: This regular expression pattern is designed to identify potential headings in the text. It looks for lines that start with a capital letter, followed by lowercase letters or spaces.
Usage: current_key is used to keep track of the current heading.
5. Looping Through Lines to Organize Text
python
Copy code
    for line in lines:
        # Check if the current line is a heading
        if heading_pattern.match(line) or re.match(r'^[A-Z ]+$', line):
            current_key = line
            organized_output[current_key] = []  # Initialize the heading with an empty list for subheadings
        else:
            # This is a subheading (description) related to the current heading
            if current_key:
                organized_output[current_key].append(line)
Purpose: This loop iterates through each line of the cleaned text.
Functionality:
If the line matches the heading pattern, it is considered a heading, and a new entry is created in the organized_output dictionary.
If it does not match, it is treated as a subheading and appended to the list of descriptions corresponding to the current heading.
6. Finalizing the Output
python
Copy code
    return organized_output
Purpose: The function returns the organized dictionary containing headings as keys and lists of subheadings as values.
7. Example Usage
python
Copy code
image_path = '/content/sample1.png' or '/content/sample.jpeg'
organized_output = extract_text_from_image(image_path)

# Reorganizing the data to match a more structured dictionary format
final_output = {key: ', '.join(value) for key, value in organized_output.items()}
Purpose: This section demonstrates how to use the extract_text_from_image function. It specifies an image path, extracts the text, and then reorganizes the output into a final dictionary format where each heading maps to a comma-separated string of its subheadings.
Summary
This code effectively captures text from an image and organizes it into a dictionary format that differentiates between headings and their associated descriptions. By using regular expressions and systematic processing, it provides a robust method for text extraction in a structured manner, making it useful for applications that require the conversion of printed or handwritten content into a digital format.

Further Considerations
Image Quality: The accuracy of text extraction depends on the quality of the input image. Images should be clear and well-lit for best results.
Regex Adjustments: Depending on the specific formatting of your text, regex patterns may need adjustments to capture all headings accurately.
Performance: Large images or images with extensive text may require optimization for performance and speed.





