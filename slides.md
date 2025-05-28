
Lessons Learned from Building

The Earth Engine Plugin<!-- .element: class="r-fit-text" -->

<img src="assets/qgis-icon_anita02.png" alt="QGIS Logo" style="height: 220px;" />
<img src="assets/earth-engine.svg" alt="Earth Engine Logo" style="height: 200px;" />

NOTE: This is only visible to the presenter.

Press <kbd>S</kbd> to open speaker view.

Drag this tab to a different window or screenshare the other tab. When you change slides on this presenter-view tab, the other tab with audience focused tabs will change as well.

Brings Earth Engine into QGIS — cloud + desktop
Load satellite data (Sentinel, Landsat, etc.)
Filter and style imagery interactively
Export data from Earth Engine for your own analysis

---

Small team working on Earth Observation and Cloud-Native tooling for high-impact projects

<img src="assets/devseed-logo.png" alt="DevSeed Logo" style="height: 220px;" />

---

The Goal

Bringing Earth Engine to the QGIS ecosystem <!-- .element: class="r-fit-text" -->

---

```python
import ee
from ee_plugin import Map

image = ee.Image('USGS/SRTMGL1_003')
vis_params = {
    'min': 0, 'max': 4000,
    'palette': ['006633', 'E5FFCC', '662A00', 'D8D8D8', 'F5F5F5']
}
Map.addLayer(image, vis_params, 'Digital Elevation Model')
Map.setCenter(-121.753, 46.855, 9)
```

---
![SRTM Coast Mountains](assets/SRTM-image.png)

NOTE:

Example using the SRTM elevation dataset, users can add data to the map via QGIS python editor in a similar way to EE's code editor.
We wanted to increase the number of users who could use the plugin without having to write Python code. 

---

The **New** Goal

Bringing Earth Engine to more QGIS users <!-- .element: class="r-fit-text" -->

---

## What's new in this iteration?

- Maintainability
- Processing toolbox integration
- Graphical user interfaces

---

![Add Image GUI](assets/add-srtm-gui.png)

---

![model-builder](assets/model-builder.png)

---

## Features

- Add, filter, style:
  - Image
  - Image Collection
  - Feature Collection
- Exporting assets
- Processing Toolbox and Model Designer Integration

NOTE: Demo can be live or use screenshots/GIFs to stay lightweight

---

<img src="assets/esa-world-cover.gif" alt="ESA World Cover" style="transform: scale(1.2);" />

---

## Lessons learned

---

![](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZThrYmEyOHoxMDFidmV2YjRlNjh5emRxZm9uMnh0YTA5azNsMWJ4MyZlcD12MV9naWZzX3NlYXJjaCZjdD1n/lPF1CyJXXcTZmUrP2J/giphy.gif)

---

**Lesson 1: Use modern development practices** <!-- .element: class="r-fit-text" -->

debugger, testing, continuous integration (CI), separate environments for testing

NOTE: Give a quick node to dependency bloat and plugins being limited to 25MB

---

### Some Core Tools

- [pytest-qgis](https://github.com/GispoCoding/pytest-qgis/tree/main)
- [DebugVS](https://plugins.qgis.org/plugins/debug_vs/)
- [GitHub Actions](https://github.com/features/actions)

---

![DebugVS](assets/debugvs.png)

---

<img src="assets/ci-demo.gif" alt="CI Demo" style="transform: scale(1.5);" />

---

**Lesson 2: Use `QgsProcessingAlgorithm` until you can't!** <!-- .element: class="r-fit-text" -->

Built-in documentation, logging, processing toolbox and model builder integration

---

`QgsProcessingAlgorithm` definition:
- `shortHelpString` (built in docs!)
- some basic methods (`name`, `displayName`, `group`, `groupId`)

---

Core algorithm logic:

```python 
  def processAlgorithm(
        self,
        parameters: dict,
        context: QgsProcessingContext,
        feedback: QgsProcessingFeedback,
    )
  ```
---

Parameter definition:
```python
initAlgorithm(self, config: dict):
  self.addParameter(...)
  ...
  self.addOutput(...)
```

---

![Export TIF](assets/export-tiff.png)

---

![Built in logs](assets/built-in-logs.png)

---

QGIS has default ways of rendering each parameters <!-- .element: class="r-fit-text" -->

<span class="fragment">What if you need a complex data type?</span>  
<span class="fragment">Or to dynamically populate data like a dropdown?</span>

NOTE: Probably the hardest and most interesting part of the development of the plugin

---

Lesson 3: Use custom algorithm dialogs when `QgsProcessingAlgorithm` isn't enough!

---

`BaseAlgorithmDialog` structure:
- Subclass of `QgsProcessingAlgorithmDialogBase`
- Lets you create a custom Qt layout
- Used for more interactive or complex parameter inputs

---

Core entry points:

```python
def buildDialog(self) -> QWidget:
    # Build and return your custom layout here
```

```python
def getParameters(self) -> Dict:
    # Return cleaned parameters for the algorithm
```

NOTE: 

- getParameters is where we can enable support for much more complex data types
- buildDialog is where we can control more advanced formatting of the UI

---

Benefits:
- Full control over UI elements and layout
- Dynamically populate dropdowns (band selection, filters, available layers, etc.)
- Embed custom widgets (percentile slider, lists of filters, etc.)

---

![Custom Filter Dialog](assets/custom_filter.png)

---

## What’s Next

- 🔍 Earth Engine Catalog Integration
- 💬 Listening to Users and Integrating Feedback

---

📘 A QGIS Plugin Dev Guide? Come say hello:

<img src="https://api.qrserver.com/v1/create-qr-code/?data=https://github.com/developmentseed/qgis-dev-guide/discussions/1&size=300x300" alt="QR Code" />

---

### Acknowledgments

- Ujaval Gandhi - Early feedback for last release
- Gena - Initial plugin development
- Alicia - Project management and user feedback
- Anthony Lukach - Core Developer

---

## Thank you!

Try out the plugin:

<img src="https://api.qrserver.com/v1/create-qr-code/?data=https://github.com/gee-community/qgis-earthengine-plugin&size=300x300" alt="QGIS Earth Engine Plugin QR Code" />