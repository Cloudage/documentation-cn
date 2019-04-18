在运行时实例化预制件
================================


到此为止，应该从基础层面理解了__预制件__的概念。预制件是可在整个游戏中复用的预定义__游戏对象__和__组件__的集合。如果不知道预制件是什么，建议阅读[预制件](Prefabs.html)页面以查看更基本的介绍。

想要在运行时实例化复杂的游戏对象时，预制件非常方便。实例化预制件的替代方法是使用代码从头开始创建游戏对象。相对于替代方法，实例化预制件具有诸多优势：


* 可以从一行代码实例化具有完整功能的预制件。从代码创建等效的游戏对象平均需要五行代码，但可能需要更多代码行。
* 可以在场景和 Inspector 中快速轻松地设置、测试和修改预制件。
* 可以更改实例化的预制件，而无需更改将预制件实例化的代码。无需更改代码就能将简单火箭改为超级火箭。


常见情况
----------------


为了说明预制件的优势，让我们考虑一下适合使用预制件的一些基本情况：


1.通过在不同位置多次创建单块“砖”预制件来筑墙。
1.火箭发射器在发射时实例化飞行的火箭预制件。预制件包含网格、__刚体__、__碰撞体__以及自带轨迹__粒子系统__的子游戏对象。
1.一个机器人爆炸成大量碎片。完整正常的机器人被摧毁，然后替换为残骸机器人预制件。此预制件由分成许多部件的机器人组成，所有部件都有自己的刚体和粒子系统。此方法允许将机器人炸成许多碎片（只需一行代码），用一个预制件替换一个对象。


###筑墙

此示例旨在说明使用预制件相对于从代码创建对象的优势。

首先，利用代码来修建砖墙：



````
// JavaScript
function Start () {
    for (var y = 0; y < 5; y++) {
        for (var x = 0; x < 5; x++) {
            var cube = GameObject.CreatePrimitive(PrimitiveType.Cube);
            cube.AddComponent.<Rigidbody>();
            cube.transform.position = Vector3 (x, y, 0);
        }
    }
}


// C#
public class Instantiation : MonoBehaviour {

	void Start() {
		for (int y = 0; y < 5; y++) {
			for (int x = 0; x < 5; x++) {
				GameObject cube = GameObject.CreatePrimitive(PrimitiveType.Cube);
				cube.AddComponent<Rigidbody>();
				cube.transform.position = new Vector3(x, y, 0);
			}
		}
	}
}
````


* 要使用以上脚本，我们只需保存脚本并将其拖到空游戏对象上。
* 通过 __GameObject &gt; Create Empty__ 来创建空游戏对象。

如果执行该代码，则会在进入播放模式时看到已创建整个砖墙。有两行与每块砖的功能相关：`CreatePrimitive` 行和 `AddComponent` 行。现在还不算太差，但我们的每块砖都没有纹理。想要针对砖块执行的每个额外操作（如更改纹理、摩擦或刚体__质量__）都是额外的行。

如果事先创建预制件并执行所有设置，则使用一行代码来执行每块砖的创建和设置即可。这样一来，在决定要进行更改时，可以减轻维护和更改大量代码的工作量。对于预制件，只需进行更改和播放即可。无需更改代码。

如果将预制件用于每块砖，下面就是筑墙所需的代码。



````
// JavaScript

//Instantiate 接受任何组件类型，因为它会将附加到组件的游戏对象实例化。因此，在此处接受变换组件。

var brick : Transform;
function Start () {
    for (var y = 0; y < 5; y++) {
        for (var x = 0; x < 5; x++) {
            Instantiate(brick, Vector3 (x, y, 0), Quaternion.identity);
        }
    }
}


// C#
public Transform brick;

void Start() {
	for (int y = 0; y < 5; y++) {
		for (int x = 0; x < 5; x++) {
			Instantiate(brick, new Vector3(x, y, 0), Quaternion.identity);
		}
	}
}
````

此代码不仅很整洁，而且可复用性很强。没有任何信息表示我们要实例化立方体或者它必须包含刚体。所有这些都在预制件中定义，并可以在 Editor 中快速创建。

现在，我们只需创建预制件，为此我们将在 Editor 中执行此操作。以下是创建方法：


1.选择 __GameObject &gt; 3D Object &gt; Cube__
1.选择 __Component &gt; Physics &gt; Rigidbody__
1.选择 __Assets &gt; Create &gt; Prefab__
1.在 __Project 视图__中，将新预制件的名称更改为“Brick”
1.将__层级视图__中创建的立方体拖入 __Project 视图__中的“Brick”预制件上
1.创建预制件后，可以放心地从层级视图中删除立方体（Windows 上使用 __Delete__，Mac 上使用 __Command-Backspace__）

我们已创建 Brick 预制件，所以现在必须将此预制件附加到脚本中的 __brick__ 变量。选择包含脚本的空游戏对象时，Brick 变量将显示在 Inspector 中。

现在，将“Brick”预制件从 Project 视图拖到 Inspector 中的 __brick__ 变量上。按 Play，然后就会看到用预制件建造的墙。

这是一种可以在 Unity 中反复使用的工作流程模式。一开始，您可能想知道为什么此方法的效果会好得多，因为从代码创建立方体的脚本只增加了 2 行而已。

但是由于现在使用的是预制件，所以可以在几秒内调整预制件。想要改变所有这些实例的质量？仅在预制件中调整一次刚体即可。要将其他__材质__用于所有实例？仅将材质拖到预制件上一次即可。要更改摩擦力？将其他__物理材质__用于预制件的碰撞体。要将粒子系统添加到所有的这些盒体？仅将子项添加到预制件一次即可。


###将火箭和爆炸实例化

以下是将预制件用于此情况的方式：


1.用户按下发射时，火箭发射器将火箭预制件实例化。预制件包含网格、刚体、碰撞体以及含有轨迹粒子系统的子游戏对象。
1.火箭会影响爆炸预制件并将其实例化。爆炸预制件包含粒子系统、随着时间推移而淡出的光源以及对周围游戏对象施加伤害的脚本。

虽然可以完全从代码构建火箭游戏对象，但是如果手动添加组件和设置属性，实例化预制件会容易得多。无论火箭的预制件多复杂，都可以仅用一行代码就将火箭实例化。在实例化预制件之后，还可以修改实例化的对象的任何属性（例如，可以设置火箭刚体的速度）。

除了预制件更易于使用之外，还可以稍后更新预制件。因此，如果正在构建火箭，不必立即向其添加粒子轨迹。可以稍后执行此操作。一旦将轨迹作为子游戏对象添加到预制件，所有实例化火箭就会具有粒子轨迹。最后，可以在 Inspector 中快速调整火箭预制件的属性，从而更轻松地对游戏进行微调。

以下脚本显示了如何使用 __Instantiate()__ 函数来发射火箭。



````
// JavaScript

// 要求火箭是一个刚体。
// 这样，我们就无法在没有刚体的情况下分配预制件
var rocket : Rigidbody;
var speed = 10.0;

function FireRocket () {
    var rocketClone : Rigidbody = Instantiate(rocket, transform.position, transform.rotation);
    rocketClone.velocity = transform.forward * speed;
    // 您还可以访问克隆体的其他组件/脚本
    rocketClone.GetComponent.<MyRocketScript>().DoSomething();
}

// 按住 Ctrl 或鼠标时调用 fire 方法
function Update () {
    if (Input.GetButtonDown("Fire1")) {
        FireRocket();
    }
}


// C#

// 要求火箭是一个刚体。
// 这样，我们就无法在没有刚体的情况下分配预制件
public Rigidbody rocket;
public float speed = 10f;

void FireRocket () {
	Rigidbody rocketClone = (Rigidbody) Instantiate(rocket, transform.position, transform.rotation);
	rocketClone.velocity = transform.forward * speed;
	
	//您还可以访问克隆体的其他组件/脚本
	rocketClone.GetComponent<MyRocketScript>().DoSomething();
}

// 按住 Ctrl 或鼠标时调用 fire 方法
void Update () {
	if (Input.GetButtonDown("Fire1")) {
		FireRocket();
	}
}


````


###用布娃娃或残骸替换角色

假设有一个完备的敌人角色死亡。只需在角色上播放死亡动画，并禁用通常处理敌人逻辑的所有脚本。可能必须注意要删除几个脚本，添加一些自定义逻辑以确保没有人会继续攻击已死亡的敌人，以及处理其他善后任务。

一种好得多的方法是立即删除整个角色并将其替换为实例化的残骸预制件。这样可以提供很大的灵活性。可以将其他材质用作死亡角色、附加完全不同的脚本、生成预制件（包含破坏成大量碎片的对象以模拟支离破碎的敌人）或者只是将包含该角色版本的预制件实例化。

只需单次调用 __Instantiate()__ 就可以实现这些任意选项，只需将其连接到正确的预制件即可！

需要记住的重要事项是执行 __Instantiate()__ 的残骸可以由完全不同于原件的对象组成。例如，如果有一架飞机，可对两个版本建模。一个版本中，飞机包含具有__网格渲染器__的单个游戏对象以及用于飞机物理设置的脚本。通过将此模型仅保持在一个游戏对象中，游戏可以运行得更快，因为可以使用较少的三角形来制作模型，并且由于游戏包含的对象较少，因此渲染速度比使用许多小部件的情况下更快。此外，飞机四处自由飞行时，没有理由将其放在单独部件中。

要构建残骸飞机预制件，典型的步骤是：

1.在偏好的建模器中用大量不同部件对飞机建模
1.创建空场景
1.将模型拖入空场景
1.通过选中所有部件并选择 __Component &gt; Physics &gt; Rigidbody__，将刚体添加到所有部件
1.通过选中所有部件并选择 __Component &gt; Physics &gt; Box Collider__，将盒型碰撞体添加到所有部件
1.要获得额外的特殊效果，请将类似烟雾的粒子系统添加为每个部件的子游戏对象
1.现在有一架带有多个爆坏部件的飞机，这些部件会根据物理原理落到地面，并由于附加的粒子系统而产生粒子轨迹。按 Play 来预览模型的反应并进行必要调整。
1.选择 __Assets &gt; Create Prefab__
1.将包含所有飞机部件的根游戏对象拖动到预制件中

以下示例演示了如何在代码中对这些步骤建模。



````
// JavaScript

var wreck : GameObject;

// 例如，我们会 3 秒后自动将游戏对象变为残骸
function Start () {
    yield WaitForSeconds(3);
    KillSelf();
}

// 按住 Ctrl 或鼠标时调用 fire 方法
function KillSelf () {
    // 在我们所处的同一位置将残骸游戏对象实例化
    var wreckClone = Instantiate(wreck, transform.position, transform.rotation);

    // 有时我们需要将一些变量从此对象转移
    // 到残骸
    wreckClone.GetComponent.<MyScript>().someVariable = GetComponent.<MyScript>().someVariable;

    // 自行终止
    Destroy(gameObject);


// C#

public GameObject wreck;

// 例如，我们会 3 秒后自动将游戏对象变为残骸
IEnumerator Start() {
	yield return new WaitForSeconds(3);
	KillSelf();
}

// 按住 Ctrl 或鼠标时调用 fire 方法
void KillSelf () {
	// 在我们所处的同一位置将残骸游戏对象实例化
	GameObject wreckClone = (GameObject) Instantiate(wreck, transform.position, transform.rotation);
	
	// 有时我们需要将一些变量从此对象转移
	//到残骸
	wreckClone.GetComponent<MyScript>().someVariable = GetComponent<MyScript>().someVariable;
	
	// 自行终止
	Destroy(gameObject);
}

}

````


###在特定图案中放置一堆对象

假设想要将一堆对象放在网格或圆形图案中。一般可以按以下方式之一完成此操作：


1.完全从代码构建对象。这很乏味！从脚本输入值既慢又不直观，得不偿失。
1.创建完备的对象，复制该对象，并在场景中多次放置该对象。这很乏味，难以将对象准确放置在网格中。

因此最好是对预制件使用 __Instantiate()__！我们认为您已了解为什么预制件在这些场景中非常有用。以下是这些场景所需的代码：



````
// JavaScript

// 将圆形中的预制件实例化

var prefab : GameObject;
var numberOfObjects = 20;
var radius = 5;

function Start () {
    for (var i = 0; i < numberOfObjects; i++) {
        var angle = i * Mathf.PI * 2 / numberOfObjects;
        var pos = Vector3 (Mathf.Cos(angle), 0, Mathf.Sin(angle)) * radius;
        Instantiate(prefab, pos, Quaternion.identity);
    }
}


// C#
// 将圆形中的预制件实例化

public GameObject prefab;
public int numberOfObjects = 20;
public float radius = 5f;

void Start() {
	for (int i = 0; i < numberOfObjects; i++) {
		float angle = i * Mathf.PI * 2 / numberOfObjects;
		Vector3 pos = new Vector3(Mathf.Cos(angle), 0, Mathf.Sin(angle)) * radius;
		Instantiate(prefab, pos, Quaternion.identity);
	}
}


````



````
// JavaScript

// 将网格中的预制件实例化

var prefab : GameObject;
var gridX = 5;
var gridY = 5;
var spacing = 2.0;

function Start () {
    for (var y = 0; y < gridY; y++) {
        for (var x=0;x<gridX;x++) {
            var pos = Vector3 (x, 0, y) * spacing;
            Instantiate(prefab, pos, Quaternion.identity);
        }
    }
}


// C#

// 将网格中的预制件实例化

public GameObject prefab;
public float gridX = 5f;
public float gridY = 5f;
public float spacing = 2f;

void Start() {
	for (int y = 0; y < gridY; y++) {
		for (int x = 0; x < gridX; x++) {
			Vector3 pos = new Vector3(x, 0, y) * spacing;
			Instantiate(prefab, pos, Quaternion.identity);
		}
	}
}


````
