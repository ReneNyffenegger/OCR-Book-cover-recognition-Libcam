# Libcam
Book cover text recognition


### Project Structure</br>
```
images
Libcam
 !--utils
 !   !--detection.py
 !   !--ner.py
 !   !--processing.py
 !
 !--main.py
 !--frozen_east_text_detection.pb
```
_cam.py_ →
_detection.py_ → extract text regions using pretrained east model and run tesseract on detected text regions</br>
_preprocessing.py_ → image processing on detected text regions </br>
_main.py_ →  run program by parsing arguments</br>
_ner.py_ → nlp functions to perform named entity recognition </br>

### Installation </br>
Create virtual environment : </br>
```conda create -n envname python=3.7   ```</br>
Activate virtual environment: </br>
```activate envname```</br>
Navigate to location you want to clone repository:</br>
```cd desired/location/```</br>
Clone this repository:</br>
```git clone https://github.com/ohumkar/Libcam.git```</br>
```cd Libcam```</br>
Install required libraries</br>
```pip install -r requirements.txt ```</br>
Download spacy small english model</br>
```python -m spacy download en_core_web_sm```

### Usage
cd into the Libcam repo & run the program</br>
1. To run with cam and east detector:</br>
```python main.py --east frozen_east_text_detection.pb --image cam --detector east --padding 0.1``` 
2. To run on pretrained image and east detector:</br>
```python main.py --east frozen_east_text_detection.pb --image images/8.jpg --detector east --padding 0.1``` 
</br>

#### Arguements : </br>
- --east : location of pretrained east model 
- --image : 
  - cam → for accesing webcam ('Space' to Capture / 'Esc' to exit)
  - image_path → for locally saved image 
- --detector :
  - east : Use EAST detector
  - tess : Use Original Pytesseract
- --padding : padding to give bouding boxes, (0.05 or 0.1 works best)
- --width : (default 320)Width of resized image which must be multiples of 32 
- --height : (default 320)Height of resized image which must be multiples of 32 
- --min-confidence : (default 0.5)Minimum confidence for region to be detected as text 
</br>

### Pipeline :</br>
<div align = "center">
<img align = "center" src = "Desktop - 3.jpg" >
</div>
</br>

- An images is resized and a forward pass is made through the EAST detector which outputs bouding boxes along with confidence scores. Important boxes are retained by   thresholding on the basis of confidence scores </br>
- Image processing is done on each of the detected text regions before passing it to the tesseract-ocr to recognize text</br>
- Named Entity REcognition is performed on the ocr output using spacy and PERSON entities are extracted as Author from the text, while remaining is marked as Title
</br>

### Challenges faced</br>
As opposed to the simple ocr task in which the text is usually over a plain background this task is a bit different as it needs to needs to detect text in natural scene images which contains much more noise in the background. Hence a robust text detector was to used.

OCR is never 100 % accurate and given the task of performing ocr on natural scene images the output is even of a lower quality. To help the tesseract to detect text, detection using another robust algorithm needed to be done. TExt regions detected by the east were then sent to the tesseract for recognition
Even though used output was not of desired quality. The output was mainly influenced by the following:</br>
- Performance of the EAST → fails to capture words → boxes include some part of other words → degrading the performance of the latter ocr task</br>
- Output of the tesseract →most of the times the ocr outputs garbage text strings or correct strings along with random characters. Such kind of output further makes the task of NER difficult finally resulting in a poor output of the program
</br>
### Improvements :</br>
- Image processing</br>
- OCR cleaing</br>

