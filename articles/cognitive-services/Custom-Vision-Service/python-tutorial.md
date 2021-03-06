---
title: 'Tutorial: Erstellen eines Bildklassifizierungsprojekts mit dem Custom Vision SDK für Python'
titlesuffix: Azure Cognitive Services
description: Erstellen Sie ein Projekt, fügen Sie Kategorien hinzu, laden Sie Bilder hoch, trainieren Sie Ihr Projekt, und machen Sie eine Vorhersage, die den Standardendpunkt verwendet.
services: cognitive-services
author: areddish
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: 96125ba1c54f742bb9ddf32a1588173217be0766
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953111"
---
# <a name="tutorial-create-an-image-classification-project-with-the-custom-vision-sdk-for-python"></a>Tutorial: Erstellen eines Bildklassifizierungsprojekts mit dem Custom Vision SDK für Python

Erfahren Sie, wie ein Projekt zur Bildklassifizierung mit dem Custom Vision Service und einem einfachen Python-Skript erstellt wird. Nach der Erstellung können Sie Tags hinzufügen, Bilder hochladen, das Projekt trainieren, die Standardendpunkt-URL für Vorhersagen des Projekts abrufen und Bilder programmgesteuert überprüfen. Verwenden Sie dieses Open-Source-Beispiel als Vorlage zum Erstellen Ihrer eigenen App mithilfe der Custom Vision-API.



## <a name="prerequisites"></a>Voraussetzungen

- Python 2.7 oder Python 3.5.+
- Das Pip-Tool

## <a name="get-the-training-and-prediction-keys"></a>Abrufen der Trainings- und Vorhersageschlüssel

Besuchen Sie die [Custom Vision-Webseite](https://customvision.ai), und klicken Sie in der oberen rechten Ecke auf das __Zahnradsymbol__, um die in diesem Beispiel verwendeten Schlüssel abzurufen. Kopieren Sie aus dem Abschnitt __Accounts__ (Konten) die Werte der Felder __Training Key__ (Trainingsschlüssel) und __Prediction Key__ (Vorhersageschlüssel).

![Abbildung der Schlüsselbenutzeroberfläche](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Installieren des Custom Vision Service-SDKs

Verwenden Sie den folgenden Befehl, um das Custom Vision Service SDK zu installieren:

```
pip install azure-cognitiveservices-vision-customvision
```

## <a name="get-example-images"></a>Abrufen von Beispielbildern

Dieses Beispiel verwendet die Bilder aus dem `Samples/Images`-Verzeichnis des [https://github.com/Microsoft/Cognitive-CustomVision-Windows](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images)-Projekts. Klonen Sie das Projekt, oder laden Sie es herunter, und extrahieren Sie es dann in Ihre Entwicklungsumgebung.

## <a name="create-a-custom-vision-service-project"></a>Erstellen eines Custom Vision Service-Projekts

Um ein neues Custom Vision Service-Projekt zu erstellen, erstellen Sie eine neue Datei mit dem Namen `sample.py`. Verwenden Sie als Dateiinhalt den folgenden Code:

> [!IMPORTANT]
> Legen Sie den `training_key` auf den Wert des Trainingsschlüssels fest, den Sie zuvor abgerufen hatten.
>
> Legen Sie den `prediction_key` auf den Wert des Vorhersageschlüssels fest, den Sie zuvor abgerufen hatten.

```python
from azure.cognitiveservices.vision.customvision.training import training_api
from azure.cognitiveservices.vision.customvision.training.models import ImageUrlCreateEntry

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"

trainer = training_api.TrainingApi(training_key)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My Project")
```

## <a name="add-tags-to-your-project"></a>Hinzufügen von Kategorien zu Ihrem Projekt

Um Ihrem Projekt Kategorien hinzuzufügen, fügen Sie den folgenden Code am Ende der `sample.py`-Datei hinzu:

```python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-images-to-the-project"></a>Hochladen von Bildern ins Projekt

Um dem Projekt die Beispielbilder hinzuzufügen, fügen Sie nach der Erstellung der Kategorien den folgenden Code ein. Dieser Code lädt das Bild mit der entsprechenden Kategorie hoch:

> [!IMPORTANT]
>
> Ändern Sie den Pfad zu den Bildern auf der Grundlage des Downloadpfads des Cognitive-CustomVision-Windows-Projekts.

```python
base_image_url = "https://raw.githubusercontent.com/Microsoft/Cognitive-CustomVision-Windows/master/Samples/"

print ("Adding images...")
for image_num in range(1,10):
    image_url = base_image_url + "Images/Hemlock/hemlock_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ hemlock_tag.id ] ) ])

for image_num in range(1,10):
    image_url = base_image_url + "Images/Japanese Cherry/japanese_cherry_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ cherry_tag.id ] ) ])


# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following:
#
#import os
#hemlock_dir = "Images\\Hemlock"
#for image in os.listdir(os.fsencode("Images\\Hemlock")):
#    with open(hemlock_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ hemlock_tag.id ])
#
#cherry_dir = "Images\\Japanese Cherry"
#for image in os.listdir(os.fsencode("Images\\Japanese Cherry")):
#    with open(cherry_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ cherry_tag.id ])
```

## <a name="train-the-project"></a>Trainieren des Projekts

Um die Klassifizierung zu trainieren, fügen Sie am Ende der `sample.py`-Datei den folgenden Code hinzu:

```python
import time

print ("Training...")
iteration = trainer.train_project(project.id)
while (iteration.status != "Completed"):
    iteration = trainer.get_iteration(project.id, iteration.id)
    print ("Training status: " + iteration.status)
    time.sleep(1)

# The iteration is now trained. Make it the default project endpoint
trainer.update_iteration(project.id, iteration.id, is_default=True)
print ("Done!")
```

## <a name="get-and-use-the-default-prediction-endpoint"></a>Abrufen und Verwenden des Standardendpunkts für Vorhersagen

Um ein Bild an den Vorhersageendpunkt zu senden und die Vorhersage abzurufen, fügen Sie am Ende der `sample.py`-Datei den folgenden Code hinzu:

```python
from azure.cognitiveservices.vision.customvision.prediction import prediction_endpoint
from azure.cognitiveservices.vision.customvision.prediction.prediction_endpoint import models

# Now there is a trained endpoint that can be used to make a prediction

predictor = prediction_endpoint.PredictionEndpoint(prediction_key)

test_img_url = base_image_url + "Images/Test/test_image.jpg"
results = predictor.predict_image_url(project.id, iteration.id, url=test_img_url)

# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following.
#
# Open the sample image and get back the prediction results.
# with open("Images\\test\\test_image.jpg", mode="rb") as test_data:
#     results = predictor.predict_image(project.id, test_data, iteration.id)

# Display the results.
for prediction in results.predictions:
    print ("\t" + prediction.tag_name + ": {0:.2f}%".format(prediction.probability * 100))
```

## <a name="run-the-example"></a>Ausführen des Beispiels

Führen Sie die Lösung aus. Die Ergebnisse der Vorhersage werden in der Konsole angezeigt.

```
python sample.py
```

Die Ausgabe der Anwendung ähnelt dem folgenden Text:

```
Creating project...
Adding images...
Training...
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```