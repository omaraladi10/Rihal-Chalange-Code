# Rihal-Chalange-Code
The program developed to solve the problem using python 

Code: 


#The program was developed using python, most of the lines  explanations to understand how the code is working.
#All required packages were pre installed in the notebook or environment used to write and run the code.
#importing required packages
import re #regular expressions
from datetime import datetime
from pytesseract import image_to_string
import cv2



img = cv2.imread('image.png') #Reading the image with opencv package. Image is saved in the path as (image.png)
email_img = cv2.resize(img, None, fx=2, fy=2, interpolation=cv2.INTER_CUBIC) #Resizing the image to get better quality

email_text = image_to_string(email_img,lang='eng') #using pytesseract to OCR the image (converting image to string)


dates = re.findall(r'\d{2}/\d{2}/\d{4}',email_text) #finding dates using regular expressions and store them in a list
final_date=[] #declaring a list to store final dates

#looping through the dates in the list to change the format
for date in dates:
    string_to_datetime = datetime.strptime(date, "%d/%m/%Y") #changing the variable type from string to date time
    date_final_format = string_to_datetime.strftime("%Y-%m-%d") #Changing the format to the required YYYY-MM-DD
    final_date.append(date_final_format) #appending final dates in the list to prepare for output


#Finding rooms names, room rates, and emails by Regular Expressions and storing each in a list

room_names= re.findall(r"Room: (\w+)", email_text) #list of room names

room_rates = re.findall(r'Rate: \$(\d+)',email_text) #list of room rates

emails = re.findall(r'[\w\.-]+@[\w\.-]+',email_text) #list of emails (From, To, and CC)




#using RE to find the names (From, To, and CC) 

name_pattern = ['(?<=From:)[^\n]*','(?<=To:)[^\n]*','(?<=Cc:)[^\n]*'] #RE patterns to look for names
final_names = [] #declare a list to store final names output

for pattern in name_pattern:
    name_with_email = re.findall(pattern,email_text) #Storing all string after (From, To, and CC)
    
    name_split = re.split('<|;',name_with_email[0]) #Split the names and emails
    
    names = [name for name in name_split if "@" not in name] #Deleting the emails from the list
    
    for name in names:
        names_stand_split = re.split(',',name) #Split the first and last name
        names_stand_split.reverse() #Reverse the string to keep the required format (Firstname Lastname), assuming the email gives last name first
    
        joined_names = ''.join([name.lstrip() for name in names_stand_split]) #joining the list to prepare for output
     
        final_names.append(joined_names) #appending each name in the final list 
    


