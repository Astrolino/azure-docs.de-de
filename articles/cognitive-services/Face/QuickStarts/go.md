---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit der REST-API und Go'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die Gesichtserkennungs-API mit Go, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 06/25/2018
ms.author: pafarley
ms.openlocfilehash: a66d50ac6984ea50eef1e34cc53db4d7e5bbdcad
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956203"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-go"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit der REST-API und Go

In dieser Schnellstartanleitung führen Sie die Gesichtserkennung für menschliche Gesichter in einem Bild durch, indem Sie die Gesichtserkennungs-API verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen des Beispiels benötigen Sie einen Abonnementschlüssel. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie Abonnementschlüssel für eine kostenlose Testversion abrufen.

## <a name="face---detect-request"></a>„Face – Detect“-Anforderung

Verwenden Sie die [Face – Detect](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)-Methode, um Gesichter in einem Bild zu erkennen und Gesichtsattribute zurückzugeben, z.B.:

* Gesichtserkennungs-API: Eindeutige ID, die in verschiedenen Gesichtserkennungs-API-Szenarien verwendet wird.
* Gesichtsrechteck: Die Position des linken und oberen Rands sowie die Breite und Höhe, um die Position des Gesichts im Bild anzugeben.
* Besondere Merkmale: Ein Array mit 27 Punkten zu Gesichtszügen, die auf die wichtigen Positionen von Gesichtskomponenten hinweisen.
* Dies können Gesichtsattribute wie Alter, Geschlecht, Intensität des Lächelns, Kopfhaltung und Gesichtsbehaarung sein.

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in einen Editor.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie den Wert von `uriBase` ggf. in die Region, in der Sie Ihre Abonnementschlüssel erhalten haben.
1. Ändern Sie optional den Wert von `imageUrl` in das Bild, das Sie analysieren möchten.
1. Speichern Sie die Datei mit der Erweiterung `.go`.
1. Öffnen Sie auf einem Computer, auf dem Go installiert ist, eine Eingabeaufforderung.
1. Erstellen Sie die Datei, z.B.: `go build detect-face.go`.
1. Führen Sie die Datei aus, z.B.: `detect-face`.

```go
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strings"
    "time"
)

func main() {
    const subscriptionKey = "<Subscription Key>"

    // You must use the same location in your REST call as you used to get your
    // subscription keys. For example, if you got your subscription keys from
    // westus, replace "westcentralus" in the URL below with "westus".
    const uriBase =
      "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect"
    const imageUrl =
      "https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg"

    const params = "?returnFaceAttributes=age,gender,headPose,smile,facialHair," +
        "glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise"
    const uri = uriBase + params
    const imageUrlEnc = "{\"url\":\"" + imageUrl + "\"}"

    reader := strings.NewReader(imageUrlEnc)

    // Create the Http client
    client := &http.Client{
        Timeout: time.Second * 2,
    }

    // Create the Post request, passing the image URL in the request body
    req, err := http.NewRequest("POST", uri, reader)
    if err != nil {
        panic(err)
    }

    // Add headers
    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)

    // Send the request and retrieve the response
    resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer resp.Body.Close()

    // Read the response body.
    // Note, data is a byte array
    data, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        panic(err)
    }

    // Parse the Json data
    var f interface{}
    json.Unmarshal(data, &f)

    // Format and display the Json result
    jsonFormatted, _ := json.MarshalIndent(f, "", "  ")
    fmt.Println(string(jsonFormatted))
}
```

## <a name="face---detect-response"></a>„Face – Detect“-Antwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Hier sehen Sie ein Beispiel:

```json
[
  {
    "faceId": "ae8952c1-7b5e-4a5a-a330-a6aa351262c9",
    "faceRectangle": {
      "top": 621,
      "left": 616,
      "width": 195,
      "height": 195
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 6.8,
        "yaw": 3.7
      },
      "gender": "male",
      "age": 37,
      "facialHair": {
        "moustache": 0.4,
        "beard": 0.4,
        "sideburns": 0.1
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.999,
        "sadness": 0.001,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.89
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.51
      },
      "noise": {
        "noiseLevel": "medium",
        "value": 0.59
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.04,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.98
          },
          {
            "color": "brown",
            "confidence": 0.87
          },
          {
            "color": "gray",
            "confidence": 0.85
          },
          {
            "color": "other",
            "confidence": 0.25
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.02
          }
        ]
      }
    }
  },
  {
    "faceId": "b1bb3cbe-5a73-4f8d-96c8-836a5aca9415",
    "faceRectangle": {
      "top": 693,
      "left": 1503,
      "width": 180,
      "height": 180
    },
    "faceAttributes": {
      "smile": 0.003,
      "headPose": {
        "pitch": 0,
        "roll": 2,
        "yaw": -2.2
      },
      "gender": "female",
      "age": 56,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0.001,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.003,
        "neutral": 0.984,
        "sadness": 0.011,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.83
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.41
      },
      "noise": {
        "noiseLevel": "high",
        "value": 0.76
      },
      "makeup": {
        "eyeMakeup": false,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.06,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.99
          },
          {
            "color": "gray",
            "confidence": 0.89
          },
          {
            "color": "other",
            "confidence": 0.64
          },
          {
            "color": "brown",
            "confidence": 0.34
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.03
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Gesichtserkennungs-APIs, die verwendet werden, um in einem Bild menschliche Gesichter zu erkennen, die Gesichter mit Rechtecken zu kennzeichnen und Attribute wie Alter und Geschlecht zurückzugeben.

> [!div class="nextstepaction"]
> [Gesichtserkennungs-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
