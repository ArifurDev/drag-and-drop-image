# drag-and-drop-image
image upload form with drag-and-drop and validates the image type, size, and aspect ratio before displaying a preview.

## Usage

To use this code in your project:

1. Copy the HTML code into your HTML file where you want the image upload form to appear.
2. Copy the CSS code into your CSS file or inside a `<style>` tag in the HTML `<head>`.
3. Copy the JavaScript code into your JavaScript file or inside a `<script>` tag at the end of the HTML `<body>`.

### HTML
```html
<div class="form-group">
    <label>Image *</label>
    <div class="drag-area" id="drag-area">
        <p>Drag & Drop your image here or click to upload</p>
        <input type="file" id="file-input" hidden>
        <div class="thumb-container"></div>
    </div>
</div>
```

### css
```css
.drag-area {
    border: 2px dashed #ccc;
    border-radius: 10px;
    padding: 20px;
    text-align: center;
    cursor: pointer;
}
.drag-area.dragging {
    background-color: #f1f1f1;
    border-color: #333;
}
.thumb {
    max-width: 100px;
    margin: 10px;
}
```

### javascript
```javascript
<script>
document.addEventListener("DOMContentLoaded", function() {
    const dragArea = document.getElementById("drag-area");
    const fileInput = document.getElementById("file-input");
    const thumbContainer = document.querySelector(".thumb-container");

    dragArea.addEventListener("click", () => fileInput.click());

    dragArea.addEventListener("dragover", (event) => {
        event.preventDefault();
        dragArea.classList.add("dragging");
    });

    dragArea.addEventListener("dragleave", () => {
        dragArea.classList.remove("dragging");
    });

    dragArea.addEventListener("drop", (event) => {
        event.preventDefault();
        dragArea.classList.remove("dragging");
        handleFiles(event.dataTransfer.files);
    });

    fileInput.addEventListener("change", (event) => {
        handleFiles(event.target.files);
    });

    function handleFiles(files) {
        // Only handle the first file in the array
        if (files.length > 0) {
            validateAndPreviewFile(files[0]);
        }
    }

    function validateAndPreviewFile(file) {
        const validTypes = ['image/jpeg', 'image/png', 'image/jpg'];
        const maxSize = 2 * 1024 * 1024; // 2 MB

        if (!validTypes.includes(file.type)) {
            alert('Only JPG, JPEG, and PNG files are allowed.');
            return;
        }

        if (file.size > maxSize) {
            alert('File size exceeds 2 MB.');
            return;
        }

        const reader = new FileReader();
        reader.readAsDataURL(file);

        reader.onloadend = () => {
            const img = new Image();
            img.src = reader.result;

            img.onload = () => {
                const { width, height } = img;
                if (width !== height) {
                    alert('Image ratio must be 1:1.');
                    return;
                }

                // Clear previous images
                thumbContainer.innerHTML = '';

                img.classList.add("thumb");
                thumbContainer.appendChild(img);
            };
        };
    }
});
</script>

```


