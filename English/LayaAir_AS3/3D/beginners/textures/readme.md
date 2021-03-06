## LayaAir3D material light color and texture

### Material color and texture attributes

The standard of material properties and light color properties have similar, reflected in the diffuse, high light, color and other environmental attributes, but the material including more full, more convenient adjustment object art effects, including reflectance, diffuse color and diffuse, specular and specular color, environment and environment texture, color and reflection reflection mapping, normal map. Here we give a detailed description of these properties.



#### Albedo

Albedo(reflectivity) is the main reflection is the brightness and color of the material, the higher the value, the brighter the material.

The value of the reflectivity is a four-dimensional vector. Look at the following code. The four elements in the vector represent red, green, blue, transparent alpha, respectively.

Transparent alpha effect is defined in percentage, 0 is fully transparent, 1 is opaque, if you need to set to translucent or fully transparent display, only adjust reflectivity is not enough, also need to set the material rendering mode for mixed type to achieve the purpose (Figure 1).

Modify the code in the “Quickstart guide for 3D project” article as follows in Figure 1 effect:

```java
	//Create a standard material
	var material:StandardMaterial = new StandardMaterial();
	//Create a diffuse 2D texture map
	material.diffuseTexture = Texture2D.load("res/layabox.png"); 	

	//Transparent effects can be achieved only by setting the render mode as transparent mixed type
	//Set the material blue stained and 30% translucent
	material.albedo=new Vector4(1,1,2,0.3);
	//Rendering mode (you can also set the value, 5-13, etc. for the mixed type, you can observe the effect of change)
	material.renderMode=StandardMaterial.RENDERMODE_DEPTHREAD_TRANSPARENTDOUBLEFACE;;
	//Assign material to box model
	box.meshRender.material = material;
```

![图片1](img/1.png)<br>（Picture 1）Reflectance staining and transparency



#### Diffuse color reflection and diffuse map

Diffuse reflectance (diffuseColor) refers to the single color of the material, the art industry can be called the object of the solid color. The diffuse reflectance map (diffuseTexture) refers to the 2D inherent texture of the material, such as wood materials need to use wood texture images, brick wall materials need to use brick wall texture pictures.

The most popular thing in the game is diffuse mapping, and the most powerful one in game art mapping is diffuse reflection, which basically reflects the basic texture of objects.

Diffuse color and texture can also mix in the LayaAir 3D engine, they will have the fusion effect, diffuse color will face light on the model of staining (backlight surface not change), a diffuse light source color similar to light, produce more overall tone (Figure 2).

Modify the code in the “Quickstart guide for 3D project” code in the course is as follows, create a blue diffuse color, by the blue side of the light, see (Figure 2) effect:

```java
	//Add directional light (light color and material color fusion, changing light color to black and white gray, and can not exposure excessive)
	var directionLight:DirectionLight = scene.addChild(new DirectionLight()) as DirectionLight;
	directionLight.ambientColor = new Vector3(0.5, 0.5, 0.5);
	directionLight.specularColor = new Vector3(0, 0, 0);
	directionLight.diffuseColor = new Vector3(1, 1, 1);
	directionLight.direction = new Vector3(0.5, -1, 0);	

	//Create a standard material
	var material:StandardMaterial = new StandardMaterial();
	//Create a diffuse color
	material.diffuseColor=new Vector3(.5,.5,2);
	//Create a diffuse 2D texture map
	material.diffuseTexture = Texture2D.load("res/layabox.png");
	//Assign the material to the box model
	box.meshRender.material = material;
```

![图片2](img/2.png)<br>（Picture 2）Diffuse color and texture mixed blend



#### High light color and specular map

High light color (specularColor) is the same as light with high light color, which refers to the high light color at the corner of the model object or the light source.

The high color of the model is influenced by both light and material. If there is no high light color of light or light, then even if the material is set with high light color and high light texture, the model of Gao Guang will not appear.

The specularTexture is a 2D texture image in which the pixel color represents the highlight color and brightness at the corresponding model. The brighter the pixel color, the brighter the model's highlights at the pixel.

Of course, highlight and highlight maps can be used together, the effect may be better, developers can repeatedly test to adjust the desired effect.

Modify the “Quickstart guide for 3D project” course code, loading a truck in the scene, you can observe the use of high-light color, high-light map, and the default light highlight just use the light to make a comparison, the code is as follows

```java
	......
	//Create parallel light -------------------
	var light:DirectionLight = scene.addChild(new DirectionLight()) as DirectionLight;
	//Change the lighting direction
	light.direction = new Vector3(0.3, -1, 0);
	//Set the highlight color to white
	light.specularColor=new Vector3(1,1,1);
			
	//Load exported truck model
	var role3D:Sprite3D=Sprite3D.load("LayaScene_truck/truck.lh");
	//Model and material loaded after the callback
	role3D.on(Event.HIERARCHY_LOADED,null,function():void
	{
		//Get the model
		var meshSprite3D:MeshSprite3D=role3D.getChildAt(0).getChildAt(0) as MeshSprite3D;
		//Get shared material from the model
		var sharedMaterial:StandardMaterial=meshSprite3D.meshRender.sharedMaterial;
		//Modify the high light color of the material, so that the high light slants red
		sharedMaterial.specularColor=new Vector4(1,0,0,1);
		//Load specular maps (consistent with diffuse reflections, or make specular maps alone)
        sharedMaterial.specularTexture=sharedMaterial.diffuseTexture;
      	//sharedMaterial.specularTexture=Texture2D.load("LayaScene_truck/Assets/texture/t0200.png");
	});
	scene.addChild(role3D);
```

Compile the above code, (Figure 3) using high color material and high light map, the effect is better. (Figure 4) only using the default white high light color of light, the effect is general.

![图片3](img/3.png)<br>（Picture 3）

![图片4](img/4.png)<br>（Picture 4）



#### Environment color and environment map

The environment color (ambient) is the same as the light environment color, which is the color fusion dyeing of materials, so that the material tends to a certain color tone, but also to improve the material, simulate the luminous effect of the lamp box.

The environment map (ambientTexture) is also a 2D texture image. In the LayaAir engine, the environment map is temporarily cancelled, and the subsequent version will increase the self luminous map for replacement. Now, use the environment color to process the model temporarily.

Modify the “Quickstart guide for 3D project”  the code in the course as follows (Figure 6):

```java
	//添加方向光（灯光色会与材质色融合，因此改灯光色为黑白灰色，且不能曝光过度）
	var directionLight:DirectionLight = scene.addChild(new DirectionLight()) as DirectionLight;
	directionLight.ambientColor = new Vector3(0.5, 0.5, 0.5);
	directionLight.specularColor = new Vector3(0, 0, 0);
	directionLight.diffuseColor = new Vector3(1, 1, 1);
	directionLight.direction = new Vector3(0.5, -1, 0);	

	//创建标准材质
	var material:StandardMaterial = new StandardMaterial();
	//创建漫反射二维纹理贴图
	material.diffuseTexture = Texture2D.load("res/layabox.png");
	//创建漫反射二维纹理贴图
	material.diffuseTexture = Texture2D.load("res/layabox.png");
	//设置环境色，提亮模型
	material.ambientColor =new Vector3(2,2,2);
	//为box模型赋材质
	box.meshRender.material = material;
```

![图片5](img/5.png)<br>（Picture 5）Under fixed lighting, the material does not use the ambient color

![图片6](img/6.png)<br>（Picture 6）Under the fixed lighting, the material uses the environment color to brighten



#### Reflection color and reflection map

Reflective color (reflectColor) is similar to ambient color, and it can be used for color fusion and coloring of the whole material.

Reflective mapping (reflectTexture), we usually use a set of box texture mapping TextureCube, it will model the overall package, simulate the surrounding environment reflected in the model effect.

The reflection map is also associated with reflectance albedo and rendering mode renderMode.

The rendering mode must be changed to double opaque to display the reflection map (StandardMaterial.RENDERMODE_OPAQUEDOUBLEFACE).

The higher the reflectance albedo value, the smaller the reflection map effect, the stronger the diffuse reflection effect, and can be adjusted according to the actual model material effect, such as water surface, mirror surface, metal surface can adjust the different reflectivity to meet the demand.

 Modify the “Quickstart guide for 3D project” The code in the course is as follows. In order to better observe the reflection effect, the spherical model is used. After operation, the effect can be obtained (Figure 7):

```java
	//添加方向光
	var directionLight:DirectionLight = scene.addChild(new DirectionLight()) as DirectionLight;
	directionLight.ambientColor = new Vector3(0.5, 0.5, 0.5);
	directionLight.specularColor = new Vector3(0.5, 0.5, 0.5);//为球体增加高光
	directionLight.diffuseColor = new Vector3(1, 1, 1);
	directionLight.direction = new Vector3(0.5, -1, 0);	

	//添加自定义模型
	var sphere:MeshSprite3D = scene.addChild(new MeshSprite3D(new SphereMesh())) as MeshSprite3D;
	sphere.transform.rotate(new Vector3(0,45,0),false,false);
			
	//创建标准材质
	var material:StandardMaterial = new StandardMaterial();
	//创建漫反射二维纹理贴图
	material.diffuseTexture = Texture2D.load("res/layabox.png");
			
	//降低反射率，加强反射贴图反射
	material.albedo=new Vector4(0.2,0.2,0.2,0);
	//设置渲染模式为双面不透明(否者无法显示反射贴图)
	material.renderMode=StandardMaterial.RENDERMODE_OPAQUEDOUBLEFACE;
	//创建反射贴图，用立方体全视角贴图进行赋值（类似于360全景包裹）
	material.reflectTexture = TextureCube.load("skyBox/skyCube.ltc");
			
	//为球型模型赋材质
	sphere.meshRender.material = material;
```

![图片7](img/7.png)<br>（Picture 7）Reflection Mapping



#### normal map

Normal bump mapping (normalTexture) plays a very important role in 3D. The fine degree of model performance is mainly affected by normal map. It can simulate the details of high precision, high surface model, and greatly enhance the quality of the game picture. Of course, if you use the normal map, the hardware performance requirements will be improved.

Provide the normal map for the art designer, making them by two methods, one is the huge workload in the fabrication of 3D model high modulus software baking a normal map, low mold use; one is the use of diffuse through some tools into a normal map, the workload is small, but the effect slightly worse than baking method. The normal mapping effect, as shown in Figure 8, is converted from diffuse reflectance maps.

![图片8](img/8.png)<br>（Picture 8） 



If you need to use normal mapping, you need to pay attention to the following issues in the development process:

1．Normal mapping has certain requirements for model data. If there is no tangent information on the model, the effect of normal concave convex can not be produced. For example, the LayaAir 3D engine comes with various Mesh mesh types, such as BoxMesh, SphereMesh, CylinderMesh, etc., and there is no tangent information. Even if the normal mapping is used, the concave and convex will not appear in the view.

2．If you need to use the normal map and the model is exported through the unity plug-in of LayaAir, you need to be careful not to tick the "ignore tangent" option in the Mesh Setting grid setting, such as (Figure 8).

![图片9](img/9.png)<br>（Picture 9）

3．If you need to use normal mapping, the game scene must use lights, otherwise the model does not produce bump effect.



We create a Cube model from unity3D (the model created in unity with tangent information), and with diffuse mapping and normal mapping, export the data with the LayaAir export plug-in, and use the normal map to automatically load onto the model. Modify the "quick open 3D journey" course code is as follows, compile and run see (Figure 10) bump effect.

```java
	//Adding directional light
	var directionLight:DirectionLight = scene.addChild(new DirectionLight()) as DirectionLight;
	directionLight.ambientColor = new Vector3(0.5, 0.5, 0.5);
	directionLight.specularColor = new Vector3(0.5, 0.5, 0.5);//Adding high light to the sphere
	directionLight.diffuseColor = new Vector3(1, 1, 1);
	directionLight.direction = new Vector3(0.5, -1, 0);	

	//Creating a model exported from unity
	var box:Sprite3D=Sprite3D.load("layaScene_box/box.lh");
	//You can also load normal maps with code
//	box.on(Event.HIERARCHY_LOADED,null,function():void
//	{
		// Rotation model by angle
//		box.transform.rotate(new Vector3(0,0,180),true,false);
		//Getting meshSprite3D pairs from the model
//		var meshSprite3D:MeshSprite3D=box.getChildAt(0) as MeshSprite3D;
		//Gets the material instance of the model
//		var material:StandardMaterial=meshSprite3D.meshRender.material as StandardMaterial;
		//Adding normal maps to the material
//		material.normalTexture=Texture2D.load("layaScene_box/Assets/texture/layabox_normal.png");
//	});
	//Load into scene
	scene.addChild(box);
			
```

![图片10](img/10.png)<br>（Picture 10）normal map



### Export material file .lmat modification

Through the understanding of the material color light and mapping properties, we learned how to modify the material properties and mapping of the object through the code, flexible control to achieve the desired effect.

But at present the effect of art is basically through the unity editor to achieve, and then export data to use. Because the current LayaAir 3D engine is being perfected, and because of the characteristics of H5 game engine, can not fully include all types of materials and attributes in unity, so the effect in unity is different from the actual game effect.

In the future version, we will use plug-in to create special LayaAir standard materials for developers in unity, so that the art effects in unity can be exactly the same as that of the game, which is convenient for developers to use.

So how do we modify and achieve some artistic effects during the waiting period? In unity, each material in export will generate.Lmat format file, we can modify the material.Lmat file to adjust the effect of art.

For example, we load a scene through the next code, and then the night effect is first set by lighting (Figure 11).

```java
		......	
		//添加3D场景
		var scene:Scene = Scene.load("LayaScene_loveScene/loveScene.ls");
		Laya.stage.addChild(scene);
			
		//创建摄像机(横纵比，近距裁剪，远距裁剪)
		var camera:Camera = new Camera( 0, 0.1, 1000);
		//加载到场景
		scene.addChild(camera);
		//移动摄像机位置
		camera.transform.position=new Vector3(-8, 4, 20);
		//旋转摄像机角度
		camera.transform.rotate(new Vector3( -8, -25, 0), true, false);
		//设置摄像机视野范围（角度） 
		camera.fieldOfView=35;
		//加入摄像机移动控制脚本
		camera.addComponent(CameraMoveScript);
			
		//创建平行光 -------------------
		var light:DirectionLight = scene.addChild(new DirectionLight()) as DirectionLight;
		//修改灯光方向
		light.direction = new Vector3(0.3, -1, 0);
		//设置为无高光
		light.specularColor=new Vector3(0,0,0);
		//设置环境光偏暗蓝
		light.ambientColor=new Vector3(0.2,0.2,1);
		//设置漫反射光偏暗
		light.diffuseColor=new Vector3(0.5,0.5,0.5);

```
![图片11](img/11.png)<br>（Picture 11）

According to the diagram, if you need to “Love” shape model turns into the brightness of the lamp box, The code changes relatively trouble, relatively simple way is to modify the corresponding material file.Lmat, file open material observation, can be found in the file storage material is a variety of color and texture attributes (Figure 12), we modify the ambient light color cover material for "1.5,1.2,1.2", refresh the page file, you can see the effect, the model lights up to achieve the effect of the light box (Figure 13).

![图片12](img/12.png)<br>（Picture 12）

![图片13](img/13.png)<br>（Picture 13）

Through the above method can adjust the scene of a variety of material properties, so as to achieve the desired effect of art. Of course, we're looking forward to the following LayaAir exclusive material in unity, editing directly in unity and consistent with the final results in the game.

