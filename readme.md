## EKYC ML Web SDK

### requirements

- `nodejs`
- `npm` or `pnpm` or `yarn`

### Quick Start

1. create a web project `pnpm create vite`, select `svelte` then `typescript`. NOTE: you can use it with any framework other than `svelte`
2. install the sdk `pnpm add @ekycsolutions/ml-js-sdk`
3. update `src/App.svelte` to add a div element with id `kyc` or any value as you like to be used to instantiate `Kyc` component
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

      // true to use liveness config from node server
      isUseLivenessChecksFromServer: false,

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
      //  data: {
      //    livenessChecks: { checks: string; canvasImageData: Canvas.ImageData; }[];
      //    faceImagesData: { frontSize: Canvas.ImageData; leftSide: Canvas.ImageData; rightSide: Canvas.ImageData; };
      //    documentScanned: { [DocumentSide.Main = 0]: Canvas.ImageData; [DocumentSide.Secondary = 1]: Canvas.ImageData };
      //    runKyc: Function();
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
4. run `pnpm dev` to test the ekyc with our default overlay as demo
