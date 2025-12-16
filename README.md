# tesseract-ocr with --visible-pdf-image

This is a modified version of Tesseract: https://github.com/tesseract-ocr/tesseract

## Description
The modifications in the visible_pdf_image branch enable the user to input both
a "cleaned" image to be used for OCR and a "visible" image that the user will
see in the output PDF. For historical reference, here is the upstream feature
request (closed): https://github.com/tesseract-ocr/tesseract/issues/210

Cleaning an image helps OCR engines by removing background colors and patterns,
sharpening text, increasing contrast and brightness, fading wrinkles, etc. The
process usually makes the image look terrible, so the idea with these patches
is to give us the best of both worlds - a "cleaned" image for the OCR engine to
process and the original image that humans will see.

A couple useful tools you can use to clean an image:
- `textcleaner` from Fred's ImageMagick Scripts:
http://www.fmwconcepts.com/imagemagick/textcleaner/
- `unpaper` :  https://github.com/unpaper/unpaper

For the visible image you can use the original copy, but I suggest using a
compressed version instead to save space. **The only requirement is that the
dimensions of the "cleaned" and "visible" images are the same**.

Once you've built the visible_pdf_image branch along with the other Tesseract
dependencies, just add `--visible-pdf-image <image>` to the arguments. For
example:

    tesseract -l eng --visible-pdf-image compressed.webp cleaned.pnm out pdf

Add `txt` to the above command to have Tesseract output both the resulting PDF
and text. From there, you can grep for key information such as numbers (e.g.
currency) and dates. Scripting this to compare outputs using different cleaned
images can help improve accuracy.

Once you're happy with the OCR'd PDFs, you can merge them using a tool
such as PDFBox: https://pdfbox.apache.org/2.0/commandline.html

I've found this to be very helpful when creating searchable PDFs from scanned
documents, receipts, etc.

## Dependencies
* leptonica: (tested with leptonica 1.84.1)

## Alternatives
This Tesseract fork dates back to 2016. It's still useful if you want
fine-grained control over image cleaning, image formats, etc.
However there are now some more user-friendly tools packaged with distros that
might suite your needs without having to do each step manually:

- OCRmyPDF can do all steps in a single command and does a great job overall:
https://github.com/ocrmypdf/OCRmyPDF
