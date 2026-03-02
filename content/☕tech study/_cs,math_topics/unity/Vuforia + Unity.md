### Setup
- Website
	- Download Vuforia for Unity package on the [website](https://developer.vuforia.com/downloads/sdk)
	- My Account > Plan & Licenses > Basic Plan > Generate basic license
	- Copy the key
- Unity
	- Open any 3D project in Unity
	- Import the downloaded package to Unity project folder (just drag and drop) 
	- Delete the Main Camera and add Vuforia's AR camera
		- GameObject > Vuforia > AR camera
		- Click AR camera > Vuforia Behavior > Open Vuforia Engine Configuration > Global > paste the license key
	- Change build settings (ex. Android)
### Image Targets
- Vuforia Website
	- My Account > Target Manager > Add Database
		- Type - Device,  name it whatever
	- Go to the generated database and add images
		- png gives `Wrong_Color_Model` error, but if you use jpg/jpeg it works
		- use a image compressor
		- You can click "Show Features" under the images
	- Download the database
- Unity
	- GameObject > Vuforia > ImageTarget - create new ImageTarget
		- ImageTargetBehavior Script > change type "From Image" to "From Database"
	- Import the unity package into unity project
	- Under database dropdown list, you can select the image from database
	- You have to place child gameobjects that you want to view when image is tracked *under ImageTarget*
	- Under `VuforiaConfiguration` > PlayMode
		- You can change PlayMode Type and Camera Device
		- PlayMode Type - Webcam: Using Simulator window, you can use ur pc's webcam
	- If using Android
		- Change build settings to Android
		- Project Settings > Player
			- Scripting Backend: to IL2CPP
			- Target Architectures: ARMv7 and ARM64
			- Identification > Minimum API level: Android 8.0 'Oreo' (26)

### BLACK SCREEN ERROR
- Using Unity 2022.3.4f1
- Project Settings > Other Settings > Rendering > Add OpenGLES2
	- Change necessary settings to low quality to build successfully

### 영상 띄우기
- [Unity docs](https://docs.unity3d.com/ScriptReference/Video.VideoPlayer.html)
- ImageTarget gameobject
	- Plane gameobject 
		- VideoPlayer component
		- VideoController script
```C#
public class VideoController : MonoBehaviour
{
    VideoPlayer videoPlayer;
    void Start()
    {
        videoPlayer = GetComponent<VideoPlayer>();
        if (videoPlayer == null)
        {
            Debug.Log("videoPlayer null!");
        } 
    }

    public void OnTargetFound()
    {
        videoPlayer.Play();
    }

    public void OnTargetLost()
    {
        videoPlayer.Stop();
    }
}

```

### Play Button
- Under `ImageTarget` gameobject
	- `OnTargetFound()`
		- Button - GameObject.SetActive
	- `OnTargetLost()`
		- Plane - VideoController.OnTargetLost
- Canvas - World Space
	- Button's OnClick
		- Button - GameObject.SetActive(unchecked)
		- Plane - VideoController.OnTargetFound
### Tutorial list
- https://www.youtube.com/playlist?list=PLhHZ6CAe8dA3JiG62bwWb3NmCROVkoZ2S