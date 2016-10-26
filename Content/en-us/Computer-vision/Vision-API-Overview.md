#Computer Vision API Version 1.0

Computer Vision cloud-based API provides developers with access to advanced algorithms for processing images and returning information. By uploading an image or specifying an image URL, Microsoft Computer Vision algorithms can analyze visual content in different ways based on inputs and user choices. With the Computer Vision API users can analyze images to:
- Tag images based on content.
- Categorize images.
- Identify the type and quality of images. 
- Recognize domain specific content.
- Generate descriptions of the content. 
- Use optical character recognition to identify text found in images.
- Distinguish color schemes.
- Flag adult content.
- Detect human faces and return their coordinates in the image.   
- Crop photos to be used as thumbnails. 

##Tagging Images
Computer Vision API returns tags based on more than 2000 recognizable objects, living beings, scenery, and actions. In cases where tags may be ambiguous or not common knowledge, the API response provides “hints” to clarify the meaning of the tag in context of a known setting. Tags are not organized as a taxonomy and no inheritance hierarchies exist. A collection of content tags forms the foundation for an image “description” displayed as human readable language formatted in complete sentences. Note, that at this point English is the only supported language for image description.

After uploading an image or specifying an image URL, Computer Vision API’s algorithms output a number of tags based on the objects, living beings and actions identified in the image. Tagging is not limited to the main subject, such as a person in the foreground, but also includes the setting (indoor or outdoor), furniture, tools, plants, animals, accessories, gadgets etc. 
- Supported input methods: Raw image binary in the form of an application/octet stream or image URL.
- Supported image formats: JPEG, PNG, GIF, BMP.
- Image file size: Less than 4MB.
- Image dimension: Greater than 50 x 50 pixels.

###Example
![House_Yard](./house_yard.jpg)  

``` 
Returned Json
{
“tags”: [
          {
            "name": "grass",
              "confidence": 0.999999761581421
          },
          {
            "name": "outdoor",
              "confidence": 0.999970674514771
          },
          {
              "name": "sky",
                "confidence": 999289751052856
          },
          {
              "name": "building",
                "confidence": 0.996463239192963
          },
          {
            "name": "house",
              "confidence": 0.992798030376434
          },
          {
            "name": "lawn",
              "confidence": 0.822680294513702
          },
          {
            "name": "green",
              "confidence": 0.641222536563873
          },
          {
            "name": "residential",
              "confidence": 0.314032256603241
          },
        ],
} 
``` 
##Categorizing Images
In addition to tagging and descriptions, Computer Vision API returns the taxonomy-based categories defined in previous versions. These categories are organized as a taxonomy with parent/child hereditary hierarchies. All categories are in English.They can be used alone or in combination with our new models.

###The 86-category concept
Based on a list of 86 concepts seen in the below diagram, visual features found in an image can be categorized ranging from broad to specific. For the full taxonomy in text format, see [Image Categories](https://www.microsoft.com/cognitive-services/en-us/Computer-Vision-API/documentation/Images/86categories). 

![Analyze Categories](./analyze_categories.jpg)  

Image                                           | Response
----------------------------------------------- | ----------------
![Woman Roof](./woman_roof.jpg)                 | people
![Family Photo](./family_photo.jpg)             | people_crowd
![Cute Dog](./cute_dog.jpg)                     | animal_dog
![Outdoor Mountain](./mountain_vista.jpg)       | outdoor_mountain
![Vision Analyze Food Bread](./bread.jpg)       | food_bread

##Identifying Image Types
There are several ways to categorize images. Computer Vision API can set a boolean flag to indicate whether an image is black and white or color and use the same method to indicate whether an image is a line drawing or not. It can also indicate whether an image is clipart or not and indicate its quality as such on a scale of 0-3. 

##Recognizing Domain-Specific Content: Celebrities Model

In addition to tagging and top-level categorization, Computer Vision API also supports specialized (or domain-specific) information. Specialized information can be implemented as a standalone method or in combination with the high level categorization. It functions as a means to further refine the 86-category taxonomy through the addition of domain-specific models.

Currently, the only specialized information supported is celebrity recognition, and it is essentially a domain-specific refinement for the people and people group categories. 

There are two options for making use of the domain-specific models:


###Option One - Scoped Analysis
Analyze only a chosen model, by invoking an HTTP POST call. For this option, if you know which model you want to use, you just specify the model’s name, and you only get information relevant to that model. For example, you can use this option to only look for celebrity-recognition; the response will contain a list of potential matching celebrities, accompanied by their confidence scores.

###Option Two - Enhanced Analysis
Analyze to provide additional details related to categories from one of the 86-category taxonomy. This option is available for use in applications where users want to get generic image analysis in addition to details from one or more domain-specific models. When this method is invoked, the 86-category taxonomy classifier is called first. If any of the categories match that of known/matching models, a second pass of classifier invocations will follow. For example, if “details=all” or "details" include “celebrities”, the method will call the celebrity classifier after the 86-category classifier is called and the result includes “object_people_celebrities”. 

##Generating Descriptions
Computer Vision API’s algorithms analyze the content found in an image, which in turn forms the foundation for a “description” displayed as human readable language in complete sentences. The description summarizes what is found in the image. More than one description will be generated for each image ordered by their confidence score as seen in below illustration. 
After uploading an image or specifying an image URL, Computer Vision API’s algorithms generate a number of descriptions based on the objects identified in the image. The descriptions are each evaluated and a confidence score generated. A list is then returned ordered from highest confidence score to lowest.

###Example
![B&W Buildings](./bw_buildings.jpg)  

```
Returned Json 

 “description”: 
{
    "captions": 
[
{
"type": "phrase",
“text”: “a black and white photo of a large city”,
          “confidence”: 0.607638706850331
}
]
"captions": 
[
{
"type": "phrase",
“text”: “a photo of a large city”,          “confidence”: 0.577256764264197
    }
]
g"captions": 
[
{
"type": "phrase",
“text”: “a black and white photo of a city”,
          “confidence”: 0.538493271791207
}
]

“description”: 
a[
"tags": 

{
      "outdoor", "city", "building", "photo", "large", 
}
]
 }
```

{
m"type": "phrase",
“text”: “a black and white photo of a city”,
          “confidence”: 0.538493271791207
}
]

“description”: 
[
"tags": 
{
      "outdoor", "city", "building", "photo", "large", 
}
]
 }
```

##Perceiving Color Schemes
The Computer Vision algorithm extracts colors from an image. The colors are analyzed in three different contexts, foreground, background, and whole, and colors are grouped into twelve 12 dominant accent colors (black, blue, brown, gray, green, orange, pink, purple, red, teal, white, and yellow). Depending on the colors in an image, simple black and white or accent colors may be returned in hexadecimal color codes.

##Flagging Adult Content
 Among the various visual categories is the adult and racy group, which enables detection of pornographic materials and restricts the display of images containing sexual content. The filter for adult and racy content detection can be set on a sliding scale to accommodate the user’s preference.

##Detecting Faces
Computer Vision API detects human faces within a picture and generates face coordinates, draws the bounding box around the face, indicates gender and age. These visual features are a subset of metadata generated for Face API. For more extensive metadata generated for faces, such as facial identification, pose detection, and more, use the Face API.

##Optical Character Rrcognition (OCR)
OCR technology detects text content in an image and subsequently extracts the identified text into a machine-readable character stream used for search and numerous other purposes ranging from medical records to security and banking. It automatically detects the language. OCR saves time and provides convenience for users by allowing them to simply take photos of text instead of transcribing text. 

The 21 languages supported by OCR are Chinese Simplified, Chinese Traditional, Czech, Danish, Dutch, English, Finnish, French, German, Greek, Hungarian, Italian, Japanese, Korean, Norwegian, Polish, Portuguese, Russian, Spanish, Swedish, and Turkish. 

If needed, OCR corrects the rotation of the recognized text, in degrees, around the horizontal image axis. OCR provides the frame coordinates of each word as seen in below illustration.

![OCR Overview](./Ivision-overview-ocr.png)
Requirements for OCR:
- The size of the input image must be between 40 x 40 and 32000 x 32000 pixels. 
- The image cannot be bigger than 100 megapixels.

Input image can be rotated by any multiple of 90 degrees plus a small angle of up to ±40 degrees.

The accuracy of text recognition depends on the quality of the image. An inaccurate reading may be caused by the following: 
- Blurry images
- Handwritten or cursive text
- Artistic font styles
- Small text size
- Complex backgrounds, shadows or glare over text or perspective distortion
- Oversized or missing capital letters at the beginnings of words
- Subscript, superscript, or strikethrough text

Limitations: On photos where text is dominant, false positives may come from partially recognized words. On some photos, especially photos without any text, precision can vary a lot depending on the type of image. 
- Small text size
- Complex backgrounds, shadows or glare over text or perspective distortion
- Oversized or missing capital letters at the beginnings of words
- Subscript, superscript, or strikethrough text

Limitations: On photos where text is dominant, false positives may come from partially recognized words. On some photos, especially photos without any text, precision can vary a lot depending on the type of image.

##Generating Thumbnails
A thumbnail is a small representation of a full-size image. Varied devices such as phones, tablets, and PCs create a need for different user experience (UX) layouts and thumbnail sizes. Using smart cropping, this Computer Vision API feature helps solve the problem.

After uploading an image, a high quality thumbnail gets generated and the Computer Vision API algorithm analyzes the objects within the image, then crops it to fit the requirements of the “region of interest” (ROI). The output gets displayed within a special framework as seen in below illustration. The generated thumbnail can be presented in a different aspect ratio than that of the original image to accommodate a user’s needs.

The thumbnail algorithm works as follows:
1. Removes distracting elements from the image and recognizes the main object, the “region of interest” (ROI).
2. Crops the image based on identified “region of interest”.
3. Changes the aspect ratio to fit the target thumbnail dimensions.

![Thumbnails](./Images/thumbnail-demo.png) 
