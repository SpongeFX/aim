# <img src="https://static.sidefx.com/images/apple-touch-icon.png" width="25" height="25" alt="Hbuild Logo"> AIM 1.0

![version](https://img.shields.io/badge/version-1.0-blue) 

### <img src="https://github.com/SpongeFX/aim/assets/152398516/361be279-c40d-44fe-a0c4-20df42bc6dcd" width="35" height="35" align="center" alt="Aim Logo" >  AIM 1.0 is a viewport utility that helps HDA Simshot develop and create scenes with various types of weapon shots accompanied by different hit effects.


https://github.com/SpongeFX/aim/assets/152398516/3ec073d5-fc53-4217-aec3-e499646f0a81


The asset creates points with special attributes necessary for the proper functioning of [**HDA simshot**](https://github.com/SpongeFX/simshot). Creating points using the asset can be similar to a first-person shooter game, and if you have ever played something similar, it will be easier for you to understand how it works.


## 💾  Installation 
[Download the HDA file](otls/aim.1.0.hdalc) and install it to your `houdini19.5/otls/` folder. For detailed instructions, please refer to the [Houdini documentation](https://www.sidefx.com/docs/houdini/assets/install.html)

## 📃  How to use it:
1. In the "*Camera*" parameter of HDA, specify the path to the camera.
2. Select the camera view specified in the "*Camera*" parameter in the viewport settings.
3. Adjust other [parameters](#parameters) if necessary. 
4. Select the hda aim in the Network View, select "Show Handle" in the selector and handles control menu.
5. Move the mouse cursor in the viewport to the position where you want to create the target point, and click the LMB.

   If you hold down the LMB, target points will be created continuously until the number of created points reaches the limit specified in the "*Number Of Points*" parameter.

   When the limit is reached, a message appears informing you about it.

   Reset all added points with the "*Reset*" button or disable them. Do not use the "*Clear*" button to reset, as it resets the maximum limit of the number of created points.

   Points are created at the moment when you change the "*Number Of Points*" parameter. This feature is related to the correct operation during an active timeline, and if you enter a large number, it may cause your computer to freeze for some time until all the points are created.

## ☑️ Features
+ All parameters remain editable after adding.

+ Creating target points is available during simulation.
  
+ When creating a target point, it is assigned the attribute "*creation frame*". Other attributes necessary for movement initiation are assigned to the target point when the current frame is equal to or greater than the value of the "*creation frame*" attribute.

+  If you want to use multiple hda aim with different emitter positions obtained from *output1* assets, connect the outputs of *output2* hda aim with the merge node, and connect it to the corresponding input of hda simshot. Set a unique value for the "*emitter*" parameter in each hda, connect all outputs of *output1* with the merge node, and connect it to the corresponding input of hda simshot. The order of connecting emitters to the merge node matters because the emitter number corresponds to the point number from which the position for trajectory calculation is obtained.

## 🔀 Description of Inputs and Outputs:

<table border="0">
    <tr>
        <td width="17%" valign="top" >📥 Inputs:</td>
        <td><p>Hda aim has one input for connecting polygons on which you can create target points. If any polygons are connected to the input, creating points is only possible on the connected polygons visible in the viewport.</p></td>
    </tr>
    <tr><td rowspan="2" valign="top">📤 Outputs:</td>
        <td><p><i>outpout1</i> - outputs the point in the global position of the camera, which can be conveniently used as the emitter point position if the &quot;<i>Camera</i>&quot; parameter specifies the path to the camera. If the &quot;<i>Camera</i>&quot; parameter is left empty, the point position will be (0,0,0). Connect this to the corresponding input of hda simshot.,</p></td>
    </tr>
    <tr>
        <td><p><i>outpout2</i> - outputs the target points with special attributes necessary for the proper functioning of hda simshot. Connect this to the corresponding input of hda simshot.</p></td>
    </tr>
</table>
<a id="parameters"></a>

## 🔧 Parameters:

<table border="1">
    <tr>
        <td width="17%">Camera</td>
        <td><p>Camera view for adding target points. If a camera path is specified in the parameter, the global position of this camera is translated to the point in &quot;<i>output1</i>&quot; and can be used as the emitter position.</p></td>
    </tr>
    <tr>
        <td><p>Use Timeline</p></td>
        <td><p>Enabling this allows adding target points during an active timeline. The current frame is recorded in the &quot;<i>creation frame</i>&quot; attribute and all necessary attributes for motion are assigned.</p></td>
    </tr>
    <tr>
        <td><p>Direction Correction</p></td>
        <td><p>Improves the accuracy of calculating the motion trajectory of a point when the emitter position is actively changing. Adds a &quot;<i>Start Position</i>&quot; parameter and creates an emitter point position attribute for each target point using the parameter values. Enabling this parameter can increase simulation time if &quot;<i>Use Timeline</i>&quot; is enabled.</p></td>
    </tr>
    <tr>
        <td><p>Scale Target Geometry</p></td>
        <td><p>Geometry size for intersection search. This parameter should be increased if, at high animation position speed of the emitter, target points are not added when left mouse button (LMB) is clicked.</p></td>
    </tr>
    <tr>
        <td><p>Click interval limit</p></td>
        <td><p>Pause between adding target points. If LMB is held down in the Viewport while adding target points and this parameter is enabled, target points will be added with a pause, the duration of which is set in this parameter in seconds. This may not work correctly if the &quot;<i>Use Timeline</i>&quot; option is enabled, as it uses the “event.wait” method.</p></td>
    </tr>
    <tr>
        <td><p>Start Frame</p></td>
        <td><p>Default start frame. If &quot;<i>Use Timeline</i>&quot; is disabled, the parameter value serves as the starting point for calculating the &quot;<i>creation frame</i>&quot; attribute for each new target point.</p></td>
    </tr>
    <tr>
        <td><p>Inactivity Period</p></td>
        <td><p>The number of inactive frames between the creation frames of target points. If &quot;<i>Use Timeline</i>&quot; is disabled, starting from the &quot;<i>Start Frame</i>&quot; parameter value, points will be added at intervals specified in the parameter.</p></td>
    </tr>
    <tr>
        <td><p>Speed</p></td>
        <td><p>The parameter value is written to an attribute and used in HDA Simshot as the point movement speed.</p></td>
    </tr>
    <tr>
        <td><p>Emitter</p></td>
        <td><p>Default emitter number. The parameter value will be applied as an attribute for each new target point. Used in HDA Simshot to determine and separate emitter positions. This can be useful if you want to use multiple emitters. Assign a unique number to each emitter.</p></td>
    </tr>
    <tr>
        <td><p>Distance to Stop</p></td>
    <td><p>Adds an attribute with a stopping distance value for a moving point if the corresponding setting is enabled in HDA Simshot. You can find enabling and randomizing attribute values in the &quot;<i>Setup</i>&quot; tab of HDA Simshot.</p></td>
    </tr>
    <tr>
        <td><p>Number of Points</p></td>
        <td><p>Maximum number of target points.</p></td>
    </tr>
    <tr>
        <td><p>Reset</p></td>
        <td><p>Removes all added target points</p></td>
    </tr>
</table>

## 💬 Feedback
If you have any feedback or run into issues, please feel free to open an issue on this GitHub project. I appreciate your support!
