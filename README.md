# Installation Instructions

- `python3 -mvenv chessEnv`
- `. chessEnv/bin/activate`
- `python -mpip install label-studio`
- `git clone git@github.com:PerfLab-EXaCT/SamIAm-LabelStudio.git`
- `cd SamIAm-LabelStudio`
- `python -mpip install -r requirements.txt`
- `wget https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth`
- `wget https://dl.fbaipublicfiles.com/segment_anything/sam_vit_l_0b3195.pth`
- `wget https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth`
- `wget https://cdn.githubraw.com/ChaoningZhang/MobileSAM/01ea8d0f/weights/mobile_sam.pt`
- Open two terminals (#1 and #2)
  - #1: `label-studio` 
  - #2: `export SAM_MODEL=SAM` 
    - or `export SAM_MODEL=SAM_B`
    - or `export SAM_MODEL=SAM_L`
    - or `export SAM_MODEL=MobileSAM`
  - #2: `export VITH_CHECKPOINT=sam_vit_h_4b8939.pth`
    - or `export VITB_CHECKPOINT=sam_vit_b_01ec64.pth`
    - or `export VITL_CHECKPOINT=sam_vit_l_0b3195.pth`
    - or `export MOBILESAM_CHECKPOINT=mobile_sam.pt`
  - #2: `python _wsgi.py`

# User Manual
## Import Images
- Unzip the folder containing the images to be labeled.
- In the browser, navigate to the tab that `label-studio` opened (should be on `localhost:8080`).
- Sign up (only the first time).
- Create a project for labeling and import the image folder.
## Setup ML
- Navigate to "Settings" in the top right corner.
- Navigate to "Machine Learning".
- Click on "Add Model".
- Add a title, e.g., "SAM".
- Put the url for the SAM server (returned by _wsgi.py) into the field (usually runs on 127.0.0.1:9090).
- Toggle "use for interactive pre-annotations", only.
- Click on "Validate and Save".
- Navigate to "Labeling Interface".
- Switch to the "Code" view.
- Paste the following code.

    ```XML
    <View>
    <Image name="image" value="$image" zoom="true"/>
    <Header value="Brush Labels"/>
    <BrushLabels name="tag" toName="image">
    <Label value="GE" background="#FF0000"/>
    <Label value="STO" background="#0d14d3"/>
    <Label value="UNK" background="#a1ffa1"/>
    </BrushLabels>
    <Header value="Keypoint Labels"/>
    <KeyPointLabels name="tag2" toName="image" smart="true">
    <Label value="GE" smart="true" background="#FF0000" showInline="true"/>
    <Label value="STO" smart="true" background="#0d14d3" showInline="true"/>
    <Label value="UNK" smart="true" background="#a1ffa1" showInline="true"/>
    </KeyPointLabels>
    <Header value="Rectangle Labels"/>
    <RectangleLabels name="tag3" toName="image" smart="true">
    <Label value="GE" background="#FF0000" showInline="true"/>
    <Label value="STO" background="#0d14d3" showInline="true"/>
    <Label value="UNK" background="#a1ffa1" showInline="true"/>
    </RectangleLabels>
    </View>
    ```

- Save.
- Click on "Projects" and select the project to start labeling.

## Annotation Instructions
- Click on an image for labeling.
- Turn on “Auto-Annotation” toggle at the bottom of the page.
- DO NOT click on "Auto accept annotation suggestions" checkbox at the top of the page.

### Best Practices

- Use the keypoint and/or the rectangle labels as needed to get some level of mask quality. This will create a temporary label in the right pane.
- Click check mark (☑) on bottom to accept the AI assisted annotation. This will solidify the label in the right pane (makes it permanent).
- Click the permanent label (in the right pane) you wish to refine.
- Now that the label is selected, you can use the brush tool to color more area or use the eraser tool to erase some of it. Here, you can increase the size of the brush tool or eraser using the vertical sliding toggle. You can also zoom in and zoom out as needed using the two buttons (🔍) above the brush tool.
- When you are done refining, click on the blue update button in the bottom region.



# More resources

The label-studio backend plugin was developed here --> https://github.com/HumanSignal/label-studio-ml-backend. Here are more resouces for on the tool.

- https://www.youtube.com/watch?v=mUnvYTZdShk&t=218s
- https://www.youtube.com/watch?v=5tGQAEdKwe4
- https://labelstud.io/guide/ml.html