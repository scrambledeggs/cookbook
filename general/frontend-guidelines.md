# Frontend Guidelines

There are cases when BRD/UI provided is not complete. Here is a non-exhaustive list of things an engineer must implement even if not explicitly stated in the requirements:

## Forms
**DOs**
- use the right type of text input `type` (email/password/search/text/etc) - [Web](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#input_types), [React Native](https://reactnative.dev/docs/textinput#keyboardtype)
- use the correct `autocomplete` type for text inputs [Web](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete#values) , [React Native](https://reactnative.dev/docs/textinput#autocomplete)
- add placeholders in text inputs
- use the right order of tab index [Web](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex)
  - React Native - [onSubmitEditing](https://reactnative.dev/docs/textinput#onsubmitediting)
- make sure form is submittable using enter key (web and mobile)
  - web - wrap `<button type="submit">` in a `form` element
  - React Native - [onSubmitEditing](https://reactnative.dev/docs/textinput#onsubmitediting)
- React native - use proper [Enter key hints](https://reactnative.dev/docs/textinput#enterkeyhint) on text inputs
- disable related forms elements when in loading/submitting state such as:
  - checkbox/radio buttons
  - buttons
  - textboxes
  - dropdown/select 
  - slider/swiper
- show error messages when an error has occured - [read more](#error-messages)


**DON'Ts**
- replace the whole page/form with a loading state

## Error Messages

**DOs**
- show a generic error message for unhandled errors i.e "Sorry, an unknown error has occured"
  - if there is no design for an error message, you can do the following:
    - show a text with red text or red background above the submit button
    - [toast](https://developer.android.com/guide/topics/ui/notifiers/toasts)
    - [snackbar](https://m3.material.io/components/snackbar/overview)
- (React specific) Create an [Error Boundary](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary) in App level or Page level for unhandled errors to prevent showing white screen

**DON'Ts**
- show sensitive information in error messages

## Modals
**DOs**
- close modal when clicking outside the modal
- close modal when pressing `Esc` key 

**DON'Ts**
- use fixed values for width/height - use relative units such as `vh` `vw`

## Images

### Accessibility 
- Add `alt` prop on images to describe images
- if the image has readable text, use that as the `alt` text
- add `alt=""` (empty string) and `role="presentation"` for images that only serve a visual purpose (like icons)
  - exceptions:
    - button with only an icon without text - put the function of the icon in `alt` -- but argue this isn't a good usability practice

refer to [this page](https://www.w3.org/WAI/tutorials/images/decision-tree/) if you can't decide on the `alt` text

### Image format

use the right image format for the images

Rough guidelines:
- icons should be in SVG if applicable, otherwise PNG
- photos like something taken by a camera - JPG
- illustrations with flat colors - PNG
- if you can't choose between SVG and PNG - try comparing file size

### Sizing

- add `width` and `height` properties - this is to prevent weird moving shifting of elements while the image is loading on slow networks
  - web - values are automatically in `px` unit , `width="100"` means 100px
  - the dimensions doesn't need to be exact on how they are gonna be displayed but they should have the same aspect ratio
  - you can still style image dimensions using css, `width`/`height` only serves as hint for the browser to know how it will display the image
- Resize images properly, save in 3 sets - 1x, 2x and 3x aspect ratios for best quality on different devices
  - web: display using `srcset` property or `picture` element - see [Responsive Images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)
  - react-native: [follow naming scheme](https://reactnative.dev/docs/images)

- compress images before deployment
  - use [tinyjpg](https://tinyjpg.com/) or [tinypng](https://tinypng.com/)
  - you can also add compress images in your build process using `imagemin`
- if applicable - use our SIH service to resize images + serving webp versions


[Reference](https://www.w3.org/WAI/tutorials/images/)

## Buttons

- add appropriate `type` of button