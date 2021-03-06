#Batch Destruction Releases Memory

###### *version :2.0.2beta   Update:2019-5-8*

Scene3D, Sprite3D are calling`destroy()`After that, the material, texture and mesh referenced by the wizard will not be destroyed together with the wizard's destruction. These remaining resources need to be used separately`Laya.loader.getRes(url:String)`After the interface obtains the resource object, it calls the resource object's`destroy()`Methods Destruction. But this method is very troublesome. LayaAir2.0 is available for developers`Resource.destroyUnusedResources()`Unified interface destruction.

​**Tip**Note that when it comes to cleaning up a Scene 3D or Sprite 3D related resource, it is not only his own genie that needs to be destroyed, but also the cloned genie that needs to be destroyed. here`destroyUnusedResources`Method automatically releases all unused and unused**Lock up**Resources.

! [] (IMG / 1. PNG) < br > (Figure 1)

As shown in Figure 1, a button is added to our map. And add the following events to the button.

>The following code is excerpted from the official example（[demo地址](https://layaair.ldc.layabox.com/demo2/?language=ch&category=3d&group=Resource&name=GarbageCollection))


```typescript

function(e:Event):void {
    _castType++;
    _castType %= 2;
    switch (_castType) {
    case 0: 
    	(e.target as Button).label = "释放显存";
    	loadScene();
    break;
    case 1: 
    	(e.target as Button).label = "加载场景";
    	if (_scene)//_scene不为空表示场景已加载完成
    	garbageCollection();
    break;
}
    
/**
 * @private 销毁场景并且释放资源
 */
public function garbageCollection():void {
   _scene.destroy();//销毁场景
   _scene = null;
   Resource.destroyUnusedResources();//销毁无用资源(没有被场景树引用,并且没有加资源锁的)
}

/**
 * @private 加载场景，并且将该场景添加到stage上
 */
public function loadScene():void {
    Scene3D.load("res/threeDimen/scene/ParticleScene/Example_01.ls", Handler.create(this, function(scene:Scene3D):void {
      	_scene = Laya.stage.addChildAt(scene, 0) as Scene3D;
     	var camera:Camera = scene.addChild(new Camera(0, 0.1, 100)) as Camera;
    	camera.transform.translate(new Vector3(0, 1, 0));
     	camera.addComponent(CameraMoveScript);
    }));
 }
```


Let's click on the loading scenario to see the status.

![] (img/2.png)<br> (Figure 2)

After the scene is displayed, we can see that in the**Stat**Panel**GPUMemory**There is a more obvious increase.

Then we click to release the memory.

![] (img/3.png) < br > (fig. 3)

####On Resource Locking

Because`destroyUnusedResources`Release mechanism of interface. So we need resources that are not used and cannot be released.**Lock up**。 Currently, there are several locking methods because of the difference between 2D, 3D single resource loading and 3D batch loading.

​**Be careful:**Locking is actually for the parent class`Resource`The resource object is locked.

#####Resource Locking When 3D Single Resource Loading

When loading resources with the corresponding resource loading mode, it is realized by directly locking the resource object obtained by the callback. The way the corresponding resources are loaded can be viewed**Resource Loading Paper**Of**Resource loading**Section, excluding scenarios and preset loads. The single scenario and the preset load callback value types are Scene3D and Sprite3D, respectively. They are not resource objects inherited from Resources.


```typescript

//加载Mesh
Mesh.load("res/threeDimen/skinModel/LayaMonkey2/Assets/LayaMonkey/LayaMonkey-LayaMonkey.lm", Handler.create(this, function(mesh:Mesh):void {
    //给该网格资源上锁
    mesh.lock = true;
	........
    //中间省略，我们再网格加载完成3秒后对该网格进行销毁
    Laya.timer.once(3000,this,function ():void 
    {
        //销毁了使用该网格的节点
        layaMonkey.destroy();
        //对使用资源进行销毁,注意调用资源destroy的话，就算加锁也是会被销毁的。
        Resource.destroyUnusedResources();
    });
}));
```


![] (img/4.png)<br> (Figure 4) before calling automatic release

![] (img/5.png)<br> (Fig. 5) Called after automatic release

As you can see, GPUMemory is not reduced after calling the resource after locking.

Loading`.lh`And`.ls`The file actually loads the relevant files it uses automatically, such as`.lmat`Material documents,`.lani`Animation files,`.lm`Grid files, etc. Resource locking for such resources requires that the resource nodes be locked by Scene3D and Sprite3D in callbacks.


```typescript

//加载精灵
Sprite3D.load("res/threeDimen/skinModel/LayaMonkey/LayaMonkey.lh", Handler.create(this, function(sp:Sprite3D):void {
    //获取蒙皮网格精灵
    var skinnedmesh:SkinnedMeshSprite3D = sp.getChildAt(0).getChildAt(0) as SkinnedMeshSprite3D;
    //对预设的网格上锁
    skinnedmesh.meshFilter.sharedMesh.lock = true;
    //对预设所有的材质上锁
    for (var i:int = 0; i < skinnedmesh.skinnedMeshRenderer.sharedMaterials.length;i++ ){
    	skinnedmesh.skinnedMeshRenderer.sharedMaterials[i].lock = true;
    }
}));
```


#####Resource Locking in 3D Batch Loading

Callback values cannot be obtained when resources are loaded in batches. At this time, developers need to pass the`Laya.loader.getRes(url)`To get the corresponding resources to lock.


```typescript

//批量预加载方式
public function PreloadingRes():void {
//预加载所有资源
var resource:Array = [
    "res/threeDimen/scene/TerrainScene/XunLongShi.ls",
    "res/threeDimen/skyBox/skyBox2/skyBox2.lmat",
    "res/threeDimen/texture/earth.png", 
    "res/threeDimen/skinModel/LayaMonkey/Assets/LayaMonkey/LayaMonkey-LayaMonkey.lm",
    "res/threeDimen/skinModel/LayaMonkey/LayaMonkey.lh", 
    "res/threeDimen/skinModel/BoneLinkScene/PangZiNoAni.lh",
    "res/threeDimen/skinModel/BoneLinkScene/Assets/Model3D/PangZi-Take 001.lani"
];
    
    //开始加载
    Laya.loader.create(resource, Handler.create(this, onPreLoadFinish));
}

//加载完成回调
public function onPreLoadFinish():void {
    //获取需要上锁的资源
    var skyboxMaterial:BaseMaterial = Loader.getRes("res/threeDimen/skyBox/skyBox2/skyBox2.lmat") as BaseMaterial;
    //资源上锁
    skyboxMaterial.lock = true;
    .......
    //获取需要上锁的资源
    var mesh:Mesh = Loader.getRes("res/threeDimen/skinModel/LayaMonkey/Assets/LayaMonkey/LayaMonkey-LayaMonkey.lm") as Mesh;
    //资源上锁 
	mesh.lock = true;
	.......
}
```


#####2D resource loading, resource locking

In 2D, the picture uses`Texture`Texture (note not in 3D`Texture2D`) But in essence, Texture is the re-encapsulation of Texture 2D, Texture's`bitmap`The attribute is the texture2d to which he belongs. The texture itself records the UV attribute of texture2 to realize the single image display in the graph set.

So in 2D, multiple different textures in the same graph set are a common bitmap. Such a mechanism may lead to a misunderstanding in memory management: "If a 2D Texture is destroyed, the memory it occupies should also be released".

​**It's wrong to think so. Since multiple Textures refer to the same bitmap, and Texture is not real memory, bitmap is the real object of memory. Destroying a texture is not the same as destroying a bitmap, so it is impossible to release the explicit memory at this time.**

Therefore, the locking of 2D atlas and image resources is actually a bit map locking of the corresponding Texture. After loading the atlas, use the`Laya.loader.getRes(url)`Take a texture texture in the atlas, and lock it through the texture's bitmap attribute.


```typescript

private function init():void {
	//加载场景
	Laya.loader.load('res/atlas/comp.atlas',Handler.create(this,onComplete));
}

private function onComplete():void{
	//获取图集中的一个单图
	var a:Texture = Laya.loader.getRes('comp/button.png');
	//通过单图的bitmap对图集加锁
	a.bitmap.lock = true;
}
```

