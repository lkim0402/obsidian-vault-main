- https://docs.unity3d.com/Packages/com.unity.cinemachine@2.3/manual/CinemachineOverview.html
	- Window > Package Manager > Packages: Unity Registry > Search Cinemachine
	- A package that allows us to manage multiple cameras in our scene and create rules for our cameras
		![[Pasted image 20240804180319.png]]
	- Virtual camera
		- Cinemachine does not create new cameras. Instead, it **directs a single Unity camera for multiple shots**. You compose these shots with Virtual Cameras. Virtual Cameras move and rotate the Unity camera and control its settings.
		- The Virtual Cameras are separate GameObjects from the Unity Camera, and behave independently
		- A good rule of thumb is to use a single Virtual Camera for a single shot.
	- Brain camera
		- Main Camera > Add Component (on the right) > Add `CinemachineBrain` Component 
	- Add Virtual Camera
		- Gameobject > Cinemachine > Virtual Camera
	- After adding Virtual Camera:
		- Body > Framing Transposer
		- Follow > select player
