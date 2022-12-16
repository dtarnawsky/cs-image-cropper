# Image Cropper
This Capacitor project uses `ngx-image-cropper`. In a web view there is a limit to the size of images that can be managed. So, in this example when a photo is selected it will resize the image first before continuing.

The following function `toImageBase64` will take in a file obtained from an input and set the variable `imageBase64` which is displayed by `ngx-image-cropper`.

```typescript
toImageBase64(file: Blob) {
    const that = this;
    const resize_width = 1000;
    const reader = new FileReader();
    reader.readAsDataURL(file);
    reader.onload = (event) => {
      const img = new Image(); //create a image
      img.src = event?.target?.result as string; //result is base64-encoded Data URI
      img.onload = (el) => {
        const elem = document.createElement('canvas'); //create a canvas

        //scale the image to to keep aspect ratio
        const scaleFactor = resize_width / (el?.target as any).width;
        elem.width = resize_width;
        elem.height = (el?.target as any).height * scaleFactor;

        //draw in canvas
        const ctx = elem.getContext('2d');
        ctx?.drawImage(el.target as CanvasImageSource, 0, 0, elem.width, elem.height);

        //get the base64-encoded Data URI from the resize image
        const srcEncoded = ctx?.canvas.toDataURL('image/png', 1);

        that.imageBase64 = srcEncoded as string;
      }
    }
  }
```