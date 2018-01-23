# Pdf-Image-to-Txt-Converter
This program converts any pdf in a directory to a text document whether an image or machine readable. 


# Part I:  Set Current and Target Directory

import os

os.chdir('/Volumes/insight/Legal Analytics Sprint-S18/Team Folders/Team Wang/Chris')
Target_dir = os.getcwd()
Dir_list = os.listdir()



# Part II:  Traverse Target Dir and Identify Pdf docs

def get_list_pdf_Files(Target_dir, Dir_list):
    '''The purpose of this function is to change to the correct working director & obtain list of pdf files
    a.) Change the directory to the target directory where the files are located. 
    b.) Obtain list of pdf files
    c.) Return list of pdf files in the directory. 
    '''    
    import os
    
    os.chdir(Target_dir)
    File_list = [x for x in Dir_list if '.pdf' in x]
    
    return File_list
    
    
    
# Part III:  Convert Pdf files to PNG Images

def convert_pdf_to_png(Pdf_File):
    '''The purpose of this function is to convert each page of a pdf to a png file. 
    a.) Pass pdf file to Image.  After testing tesseract output, tesolution has been modified to 400 pixels.    
    b.) Return None as the function will create the jpeg files in the target directory. 
    '''
    from wand.image import Image
    from wand.color import Color
    
    with Image(filename = Pdf_File, resolution = 400) as img:
        with img.convert('png') as converted:
            converted.save(filename = 'converted.png')
    
    return None


# Part IV: Convert PNG Images to Single Text Doc  

def create_textFile_from_images(Pdf):
    '''The purpose of this function is to convert each png file to text and concatenate them into a single doc.  
    a.) Define the list of files in dir
    b.) Create the txt file to write to
    c.) Convert each PNG file to a string using pytesseract
    e.) Concatenate text files. 
    f.) Return None as this function writes the output directly to the target directory. 
    Note:  If write text gen a unicode error we'll need to set pytesseract config to only recognize string characters.
    config= "-c tessedit_char_whitelist=0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!#$%&*+,-.:;~`_=^<>?@(){}[]|/\\\"\'' -psm 6")
    '''   
    import PIL
    import pytesseract
    import os
    
    Dir_list = os.listdir()
      
    New_File = open(Pdf + '.txt','w')  

    for file in Dir_list:
        if 'converted' in file:      
            im = PIL.Image.open(file)
            Text = pytesseract.image_to_string(im)           
            New_File.write(Text)

    return None




# Part V: Clean up dir

def cleanUpDir_remove_png():
    '''The purpose of this function is to remove the jpeg and zip files creates as part of the former functions
    a.) Traverse the directory where the jpeg and zip files were created
    b.) Remove any files ending in .jpeg and .zip
    c.) Return None as there is no output to this function. 
    '''
    import os
    
    Dir_list = os.listdir()  # Needs to be called a second time after the files have been created. 
    
    for File in Dir_list:
        if '.png' in File:
            os.remove(File)
    
    return None
    
    
    
    
    
#Part VI:  Run Pipeline


directory_pdf_file_conversion_2text_pipeline(Target_dir)


    
    
    
    
