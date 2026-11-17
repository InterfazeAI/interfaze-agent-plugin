---
name: object-detection
description: Use when the user wants to find, count or locate objects in an image or PDF, or find buttons, inputs and links in a screenshot. Uses the Interfaze object_detection and gui_detection tools.
---

# Object detection with Interfaze

Use `object_detection` to find objects in an image or PDF. Use `gui_detection` to find user-interface elements in a screenshot.

## When not to use this skill

- The user wants the text in the image. Use the `ocr` skill.
- The user wants to sort whole images into labels. Use the `classification` tool.

## Steps

1. Get a URL for the image. If the image is local, follow "Send a file" in the `interfaze` skill.
2. Call the tool. Put the image URL in `files`.
   - Objects: call `object_detection`. In `prompt`, name the objects, for example "Find every car and person."
   - Screenshot elements: call `gui_detection`. In `prompt`, say what to find, for example "Find the login button and the email input."
3. Give the user each object with its label and bounding box.

## Tips

- Check the coordinate scale in the result before you draw boxes or click. Some results use pixels. Other results use a 0 to 1000 grid.
- To count objects, count the items in the result. Do not estimate.
