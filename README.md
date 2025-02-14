# User Scene Texture BUG Happens by Changing Resolution of Game

I tried to implement extended difference of gaussians from Acerola's Github code inside Unreal Engine: https://github.com/GarrettGunnell/Post-Processing

It worked in the end but when I tried to scale the viewport in both editor and standalone play mode, spaghetti lines were appeared on edges of screen. I followed down the error to the "UserSceneTexture" nodes. I am thinking this could be a bug, because when I test the "SceneTexture:PostProcessInput0" node, spaghetti lines don't happen. Probably it happens because of UV of "UserSceneTexture" node doesn't scale with resolution.

I want to note that someone also encountered same issue as mine, but I wanted to write my point of view, because I think this could be an engine bug. Here is other thread: https://forums.unrealengine.com/t/problem-sampling-user-scene-texture-at-high-resolution/2261645/1

Note: I am using "SceneColorBeforeDOF" as blendable location for my materials. I also tested other blendable location enums.

This is eigenvectors pass with default view inside editor.
![Editor Default Viewport Eigenvectors Pass|690x371](https://raw.githubusercontent.com/Warwlock/ImagesForUserSceneTextureBUG/refs/heads/main/Screenshot%202025-02-14%20113748.png)


And this one is fullscreen mode inside editor. There are spaghetti lines at the end of UVs.
![Fullscreen Editor Spaghetti Lines|690x372](https://github.com/Warwlock/ImagesForUserSceneTextureBUG/blob/main/Screenshot%202025-02-14%20113918.png?raw=true)

Interesting thing is when I enter play mode, those spaghetti lines go away. I decreased exposure level in this image.
![Fullscreen Play Mode Inside Editor|690x372](https://github.com/Warwlock/ImagesForUserSceneTextureBUG/blob/main/Screenshot%202025-02-14%20114412.png?raw=true)


But when I try to play in standalone game, and changing resolution in the game, those spaghetti lines come back. And some glitching happens in viewport when spaghetti lines appear.
![720p windowed Standalone Game Mode|690x370, 50%](https://github.com/Warwlock/ImagesForUserSceneTextureBUG/blob/main/Screenshot%202025-02-14%20114821.png?raw=true)

![FullScreen 1080p Standalone Game Mode|690x387, 50%](https://github.com/Warwlock/ImagesForUserSceneTextureBUG/blob/main/Screenshot%202025-02-14%20114842.png?raw=true)


Those are my material passes. rgb2lab and eigenvectors, respectively. Also you can see post process material settings for each material.
![Rgb2Lab Pass|690x371, 75%](https://github.com/Warwlock/ImagesForUserSceneTextureBUG/blob/main/Screenshot%202025-02-14%20115855.png?raw=true)
![Eigenvectors pass|690x371, 75%](https://github.com/Warwlock/ImagesForUserSceneTextureBUG/blob/main/Screenshot%202025-02-14%20115919.png?raw=true)


And no, my hlsl code doesn't have error. When I directly connect "UserSceneTexture:tex_rgb2lab" node to "EmmisiveColor" socket, spaghetti lines are there. It is hard to see in lab color space but look at the sun, it stretches to the end of screen.
![Rgb2Lab image but using two passes with UserSceneTexture|690x370, 100%](https://github.com/Warwlock/ImagesForUserSceneTextureBUG/blob/main/Screenshot%202025-02-14%20120229.png?raw=true)

Removing second pass from post processing volume, and only using first pass, those lines go away.

![Rgb2Lab image but only one pass without any UserSceneTexture|690x370, 100%](https://github.com/Warwlock/ImagesForUserSceneTextureBUG/blob/main/Screenshot%202025-02-14%20120513.png?raw=true)
