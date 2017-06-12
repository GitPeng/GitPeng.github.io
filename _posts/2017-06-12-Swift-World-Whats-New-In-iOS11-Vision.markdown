---
layout: post
title:  "Swift World: What’s new in iOS 11 - Vision"
date:   2017-06-12 20:00:00
categories: tutorial
---

In my [previous article](https://medium.com/@NilStack/whats-new-in-ios-11-core-ml-44555927d750)， I introduced Core ML which is a general machine learning framework. Apple also provide frameworks for specific areas. In this article, I'll dive into Vision framework on computer vision. This framework is based on Core ML.

> ![CoreML](http://pengguo.xyz/resources/CoreML.png)
> - Core ML Document

Vision gives us several tools to analyze image or video to detect and recognize face, detect barcode, detect text, detect and track object, etc. I will explain each tool with example. The example has been uploaded to [GitHub - NilStack/HelloVision](https://github.com/NilStack/HelloVision).

Simply speaking, there are three roles in Vision’s usage. They are request, request handler and observation. There are different image analysis request types to use different tools in Vision. For example we define VNDetectFaceRectanglesRequest to detect face in an image. As request Handler, there are only two kinds of request handlers: VNImageRequestHandler and VNSequenceRequestHandler. One is for single image and the other is for “a sequence of multiple images”. The results are wrapped in “observations”. The informations in observation like bounding box of analysis result.

A simple template to use Vision is like the following code block.

```swift
let request = VNDetectXXXRequest(completionHandler: {(request, error) in
guard let observations = request.results as? [VNXXXObservation]
else { fatalError("unexpected result type from request") }
...
})
let handler = VNImageRequestHandler(cgImage: image, options: [:])
try? handler.perform([request])
```

In every feature, I only show parts of codes. Please refer to the complete project on [GitHub](https://github.com/NilStack/HelloVision).

### 1. Machine Learning Image analysis
This is to analyze image with Core ML model. The corresponding request is VNCoreMLRequest. I will use a new model MobileNets by Google. It is “for mobile and embedded vision applications”. You can download the model file which has been converted to Core ML format by [Matthijs Hollemans](https://github.com/hollance) from [awesome-CoreML-models](https://github.com/NilStack/awesome-CoreML-models).

```swift
 guard let visionModel = try? VNCoreMLModel(for: model.model) else {
            fatalError("Error")
        }

        let request = VNCoreMLRequest(model: visionModel) { request, error in

            if let observations = request.results as? [VNClassificationObservation] {
                let top5 = observations.prefix(through: 4)
                    .map { ($0.identifier, Double($0.confidence)) }
                // handle top 5 prediction result
                ...
            }
        }

        let handler = VNImageRequestHandler(cgImage: image.cgImage!)
        try? handler.perform([request])
```

Here is the result

![ImageAnalysis](http://pengguo.xyz/resources/ImageAnalysis.png)

### 2. Face detection

Face detection is to help find faces in an image. The corresponding request is VNDetectFaceRectanglesRequest. The bounding boxes for detected faces are wrapped in the result VNFaceObservations. In the example, rectangles are drawn around the faces.

```swift
 let detectFaceRequest: VNDetectFaceRectanglesRequest = VNDetectFaceRectanglesRequest(completionHandler: self.handleFaces)
 let detectFaceRequestHandler = VNImageRequestHandler(ciImage: facesCIImage, options: [:])

do {
     try detectFaceRequestHandler.perform([detectFaceRequest])
} catch {
     print(error)
}

```

The handleFaces is the handler XXX

```swift
 func handleFaces(request: VNRequest, error: Error?) {
        guard let observations = request.results as? [VNFaceObservation]
            else { fatalError("unexpected result type from VNDetectFaceRectanglesRequest") }

       // hanlde the observation
       ...
    }
```

![FaceDetection](http://pengguo.xyz/resources/FaceDetection.png)

### 3. Face Landmarks Detection

Face Landmarks Detection is to help find different facial features in the image. The corresponding request is VNDetectFaceLandmarksRequest.
The regions for different landmarks are wrapped in the results. In a region, the points will mark the landmarks like eyes, nose, mouth, etc.

```swift
let detectFaceRequest = VNDetectFaceLandmarksRequest { (request, error) in
            if error == nil {
                if let results = request.results as? [VNFaceObservation] {
                    print("Found \(results.count) faces")

                    for faceObservation in results {
                        guard let landmarks = faceObservation.landmarks else {
                            continue
                        }
                        let boundingRect = faceObservation.boundingBox
                        var landmarkRegions: [VNFaceLandmarkRegion2D] = []
                        if let faceContour = landmarks.faceContour {
                            landmarkRegions.append(faceContour)
                        }
                        if let leftEye = landmarks.leftEye {
                            landmarkRegions.append(leftEye)
                        }
                        if let rightEye = landmarks.rightEye {
                            landmarkRegions.append(rightEye)
                        }
...
//handle landmark regions
...

                    }
                }
            } else {
                print(error!.localizedDescription)
            }
            complete(resultImage)
        }

        let vnImage = VNImageRequestHandler(cgImage: source.cgImage!, options: [:])
        try? vnImage.perform([detectFaceRequest])
```

The result is below.

![FaceLandMarks](http://pengguo.xyz/resources/FaceLandMarks.png)

### 4. Text Detection

Text detection is for detecting text area in image. The request is VNDetectTextRectanglesRequest.

```swift
let textDetectionRequest = VNDetectTextRectanglesRequest(completionHandler: {(request, error) in

            guard let observations = request.results as? [VNTextObservation]
                else { fatalError("unexpected result type from VNDetectTextRectanglesRequest") }

            // handle resutl bounding box
            ...
        })

        let handler = VNImageRequestHandler(cgImage: textImage.cgImage!, options: [:])

        guard let _ = try? handler.perform([textDetectionRequest]) else {
            return print("error")
        }
```

The result is

![TextDetection](http://pengguo.xyz/resources/TextDetection.png)


### 5. Barcodes Detection
Barcodes Detection is to detect barcodes in image. But I always get nil with VNDetectBarcodesRequest and can’t find document or sample as reference. Please help me if you get right result with barcodes detection.

```swift
 let barcodeRequest = VNDetectBarcodesRequest(completionHandler: {(request, error) in

            for result in request.results! {

                if let barcode = result as? VNBarcodeObservation {

                    if let desc = barcode.barcodeDescriptor as? CIQRCodeDescriptor {
                        print(desc.symbolVersion)
                        let content = String(data: desc.errorCorrectedPayload, encoding: .utf8)

                        let resultStr = """
                                        Symbology: \(barcode.symbology.rawValue)\n
                                        Payload: \(String(describing: content))\n
                                        Error-Correction-Level:\(desc.errorCorrectionLevel)\n
                                        Symbol-Version: \(desc.symbolVersion)\n
                                        """
                        DispatchQueue.main.async {
                            self.result.text = resultStr
                        }
                    }
                }
            }
        })

        let handler = VNImageRequestHandler(cgImage: barcodeImage.cgImage!, options: [:])

        guard let _ = try? handler.perform([barcodeRequest]) else {
            return print("error")
        }
```


### 6. Object Tracking

We use VNImageRequestHandler in previous requests. But in object tracking, we  need to handle video, so it’s time to change to VNSequenceRequestHandler which is for “a sequence of multiple images”.

This example is from [jeffreybergier](https://github.com/jeffreybergier)’s blog [Getting Started with Vision on iOS 11](https://github.com/jeffreybergier/Blog-Getting-Started-with-Vision)

```swift
    private let visionSequenceHandler = VNSequenceRequestHandler()
...
 // create the request
        let request = VNTrackObjectRequest(detectedObjectObservation: lastObservation, completionHandler: self.handleVisionRequestUpdate)
        // set the accuracy to high
        // this is slower, but it works a lot better
        request.trackingLevel = .accurate

        // perform the request
        do {
            try self.visionSequenceHandler.perform([request], on: pixelBuffer)
        } catch {
            print("Throws: \(error)")
        }
```

At last, I will list official resources from Apple’s official document and WWDC session.

[Vision Document](https://developer.apple.com/documentation/vision)
[WWDC 2017 Session Vision Framework: Building on Core ML](https://developer.apple.com/videos/play/wwdc2017/506/)

I will keep updating this article and example. Thanks for your time. Please click the ❤ button to get this article seen by more people.
