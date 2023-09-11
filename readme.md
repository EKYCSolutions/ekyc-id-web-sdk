![image](https://user-images.githubusercontent.com/81238558/175767662-be4dc9ba-a6bd-459d-aaa3-f8ad0c96aa37.png)

# EkycID SDK for Web
![](https://img.shields.io/badge/platform-flutter-blue) ![](https://img.shields.io/github/v/tag/EKYCSolutions/ekyc-id-flutter?label=version)

The EkycID Web SDK lets you build a factastic OCR and Face Recognition experienced in your Website.

With one quick scan, your users will be able to extract information from thier identity cards, passports, driver licenses, license plate, vehicle registration, covid-19 vaccinate card, and any other document by government-issued.


EkycID is:
* Easy to integrate into existing ecosystems and solutions through the use of SDKs that supported both native and hybrid applications.
* Better for user experience being that the document detections and liveness checks are done directly offline on the device.
* Great for cutting down operations cost and increasing efficiency by decreasing reliance on human labor and time needed for manual data entry. 


EkycID can:
* Extract information from identity documents through document recognition & OCR.
* Verify whether an individual is real or fake through liveness detection, and face recognition. 
* Verify the authenticity of the identity documents by combining the power of document detection, OCR, liveness detection, and face recognition.

## 1. Requirements

- `nodejs`
- `npm` or `pnpm` or `yarn`
- ekyc proxy server setup, refer [here](https://github.com/EKYCSolutions/node-sdk) for setup instruction for `nodejs`

## 2. Installation

**Step 1:** Create a web project

for quick start

run `pnpm create vite`, select `svelte` then `typescript`.

NOTE: you can use it with any framework other than `svelte`

**Step 2:** Install the web sdk

install the sdk `pnpm add @ekycsolutions/ml-js-sdk`

**Step 3:** Configure the sdk

update `src/App.svelte` to add a div element with id `kyc` or any value as you like to be used to instantiate `Kyc` component
  ```svelte
  <main>
    <div id="kyc" />
  </main>

  <script lang='ts'>
  import { onMount } from 'svelte';
  import {
    Kyc,
    DocumentScannerDefaultOverlay,
    LivenessDetectionDefaultOverlay,
  } from '@ekycsolutions/ml-js-sdk';

  onMount(() => {
    const kyc = new Kyc({
      containerId: 'kyc',
      isEnableDebug: true,

      videoWidth: 640 * 1.5,
      videoHeight: 480 * 1.5,

      // true to use manual kyc flow
      // false to use express ekyc flow
      isLivenessDetectFaceSideOnly: false,

      // liveness config from local
      // if `isUseLivenessChecksFromServer` is true
      // will override this
      livenessChecks: ['left', 'right', 'blinking'],

      // document scanner overlay, by default we provide one
      // but you could also bring your own ui
      DocumentScannerOverlayClass: DocumentScannerDefaultOverlay,

      // liveness detection overlay, by default we provide one
      // but you could also bring your own ui
      LivenessDetectionOverlayClass: LivenessDetectionDefaultOverlay,
      documentScannerOverlayOpts: {
        lang: 'en',
        wrapperId: 'wrapper',
        overlayId: 'overlay',
      },
      livenessDetectionOverlayOpts: {
        lang: 'en',
        wrapperId: 'wrapper',
        overlayId: 'overlay',
      },

      // access to all events within each flow
      onChange: (e) => {
        console.log('==== manual kyc on changed ====');
        console.log(e);
      },

      // after all kyc flow completed
      // there is callback that contains all data
      // and another callback within to call ekyc api
      // the types of the argument
      // {
      //  event: Kyc.CallbackEvent;
      //  runKyc: Function();
      //  data: {
      //    livenessChecks: { checks: string; canvasImageData: Canvas.ImageData; }[];
      //    faceImagesData: { frontSize: Canvas.ImageData; leftSide: Canvas.ImageData; rightSide: Canvas.ImageData; };
      //    documentScanned: { [DocumentSide.Main = 0]: Canvas.ImageData; [DocumentSide.Secondary = 1]: Canvas.ImageData };
      //   };
      // }
      onKYCCompleted: (e) => {
        console.log('KYC Done');
      },
    });

    kyc.init();
  });
  </script>
```

**Step 4:** Run the app

run `pnpm dev` to start the development server

NOTE: we also provided default overlay for you from importing `DocumentScannerDefaultOverlay` and `LivenessDetectionDefaultOverlay`

# 4. Contact
<p>For any other questions, feel free to contact us at 
  <a href="https://ekycsolutions.com/">ekycsolutions.com</a>
</p>

# 5. License
© 2022 EKYC Solutions Co, Ltd. All rights reserved.
