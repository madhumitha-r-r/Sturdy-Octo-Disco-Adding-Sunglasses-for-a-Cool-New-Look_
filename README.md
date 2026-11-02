# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look_
# Experiment: Adding Sunglasses to an Image Using OpenCV

## Aim

To write a Python program using OpenCV to overlay sunglasses on a person's face using image masking and alpha blending techniques.

## Software Requirements

- Python
- Jupyter Notebook
- OpenCV (`cv2`)
- NumPy
- Matplotlib

## Input Images

- `girl.jpg` – Input face image.
- `sunglass(1).jpg` – Sunglasses image.

## Algorithm / Procedure

1. Import the required libraries: OpenCV, NumPy, and Matplotlib.
2. Load the input face image and sunglasses image using `cv2.imread()`.
3. Check whether both images are loaded successfully.
4. Crop the sunglasses image to remove unwanted background areas.
5. Convert the cropped image into grayscale.
6. Create a binary mask to separate the sunglasses from the white background.
7. Resize the sunglasses to fit the face.
8. Specify the position of the sunglasses using x and y coordinates.
9. Select the eye region from the face image.
10. Apply alpha blending using the mask to overlay the sunglasses.
11. Save the final output image.
12. Display the final image using Matplotlib.

## Python Program

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Load images
photo = cv2.imread("girl.jpg")
sunglasses = cv2.imread("sunglass(1).jpg")

# Check images
if photo is None:
    print("Photo not found. Check girl.jpg")
elif sunglasses is None:
    print("Sunglasses not found. Check sunglass(1).jpg")
else:
    # Crop sunglasses
    sunglasses = sunglasses[165:310, 70:470]

    # Create mask to remove white background
    gray = cv2.cvtColor(sunglasses, cv2.COLOR_BGR2GRAY)
    alpha = np.where(gray < 245, 255, 0).astype(np.uint8)

    # Resize sunglasses
    new_width = 235
    new_height = int(sunglasses.shape[0] * new_width / sunglasses.shape[1])

    sunglasses = cv2.resize(sunglasses, (new_width, new_height))
    alpha = cv2.resize(alpha, (new_width, new_height))

    # Position sunglasses
    x = 108
    y = 190

    h, w = sunglasses.shape[:2]

    # Select eye region
    roi = photo[y:y+h, x:x+w]

    # Blend using alpha mask
    alpha = alpha / 255.0
    alpha = np.stack([alpha] * 3, axis=2)

    result = (alpha * sunglasses + (1 - alpha) * roi).astype(np.uint8)

    photo[y:y+h, x:x+w] = result

    # Save output
    cv2.imwrite("final_sunglasses.jpg", photo)

    # Display output
    plt.figure(figsize=(6, 8))
    plt.imshow(cv2.cvtColor(photo, cv2.COLOR_BGR2RGB))
    plt.axis("off")
    plt.show()

    print("Sunglasses added successfully!")
```

## Output

The sunglasses are overlaid on the person's eyes using image masking and alpha blending.

**Output Image:** `
<img width="423" height="565" alt="image" src="https://github.com/user-attachments/assets/2b769afd-38e0-4205-af4a-1969cab0dcfd" />


## Result

Thus, sunglasses were successfully added to the input face image using OpenCV, NumPy, image masking, and alpha blending techniques.
