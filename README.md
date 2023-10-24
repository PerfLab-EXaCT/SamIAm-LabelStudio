# Installation Instructions

- `python3 -mvenv chessEnv`
- `. chessEnv/bin/activate`
- `python -mpip install label-studio`
- move the zip file to the current folder
- `unzip segment_anything_model.zip`
- `cd segment_anything_model`
- `python -mpip install -r requirements.txt`
- `wget https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth`
- `wget https://dl.fbaipublicfiles.com/segment_anything/sam_vit_l_0b3195.pth`
- `wget https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth`
- `wget https://cdn.githubraw.com/ChaoningZhang/MobileSAM/01ea8d0f/weights/mobile_sam.pt`
- open two terminals
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
- unzip the folder with the labeling images
- in the browser, navigate to the tab that `label-studio` opened (should be on `localhost:8080`)
- sign up (only the first time)
- create a project for labeling and import the image folder.
- navigate to "Settings" in the top right corner.
- navigate to "Machine Learning"
- click on "Add Model"
- Add a title, e.g., "SAM"
- put the url for the SAM server (returned by _wsgi.py) into the field (usually runs on 127.0.0.1:9090).
- toggle "use for interactive pre-annotations"
- click on "Validate and Save".
- navigate to "Labeling Interface"
- switch to the "Code" view
- paste the following code

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

- Save
- click on Projects and select the project to start labeling