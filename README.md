## Running the project

Open index.html in any modern web browser (Google Chrome recommended).

## Optimizations made:
	
index.html and related components:
	- Resized views/images/pizzeria.jpg to 100x75 pixels.  Previously, the browser was performing the resizing itself, starting from a large image (over 2mb in size).  Resizing the image file itself took the size down to 30.7 kb, ensuring faster asset delivery and no longer requiring the browser to perform the work of resizing the image.
	
	- Minified the CSS in css/print.css and css/style.css
	
	- Moved the render-blocking css/print.css and css/style.css references to the bottom of the page, before the </body> tag, so that the page can begin rendering immediately instead of waiting for the CSS files to finish downloading.
	
Other CSS optimizations:
	- Minified the CSS in views/css/bootstrap-grid.css and views/css/style.css

changePizzaSizes() function in main.js:
	- Instead of making dozens of queries to document.querySelectorAll(), only a single call is now made, as only a single call was necessary.
	- This brings the time to resize down by over 99%, from over 103ms to just ~1.209 ms.
	
updatePositions() function in main.js:
	- Instead of calculating (document.body.scrollTop / 1250) once per item, it now is calculated once before the loop (with its value just being reused instead of recalculated on every loop iteration).  The value doesn't change between loop iterations, so there was no reason to calculate it more than once.
	- This took the time to calculate down from around 20-30ms to usually around 0.8ms.
	- Next, I realized that the "phase" variable could really only have five possible values in any given call to the updatePositions() function, so I decided to precalculate those possible values and store them in an array called "phaseTable".  In the loop, I looked up the corresponding value and used that.
	- This took the calculation time down even further, to usually around 0.7ms.
