# MonoBehavior类

必然事件
必然事件是从MonoBehavior继承而来，就是Monobehavior的生命周期，我们先来了解MonoBehavior函数的
执行顺序
Awake()-OnEnable()-Start()-Update()-LateUpdate()-OnDisable()-Destory()

Awake函数

Awake（）函数是加载场景是运行的，一般用于初始化游戏变量或游戏状态。

Start函数

Start（）函数在第一次启动时执行，一般用于游戏对象的初始化，在Awake（）函数之后。

Update函数

Update（）函数是在运行时每一帧执行的函数，用于更新游戏场景或者状态。

FixedUpdate函数

FixedUpdate（）函数与Update（）相似，但是每一个固定的物理时间间隔调用一次，用于物理状态的更新。

LateUpdate函数

LateUpdate（）函数在Update（）函数被执行后再次被执行。

2.Collision事件
OnCollisionEnter（）函数

当碰撞体与其他碰撞提或刚体开始接触时调用。

OnCollisionStay（）函数

当碰撞体与其他碰撞提或刚体保持接触时调用。

OnCollisionExit（）函数

当碰撞体与其他碰撞提或刚体停止接触时调用。

3.Trigger事件
OnTriggerEnter（）函数

当其他碰撞体与进入触发器时调用。

OnTriggerStay（）函数

当其他碰撞体与停留在触发器时调用。

OnTriggerExit（）函数

当其他碰撞体与离开触发器时调用。

Tirgger事件和Collision事件的区别：
使用Trigger事件时必须具备以下条件。
双方都要有碰撞，至少一个有刚体组件，双方碰撞器至少开启一个IsTrigger，只要开启，就会
触发Tigger事件而不会触发Collision事件，但是会穿透另一个碰撞器，若不开启IsTrigger，只
触发Collision时间而不会相互穿透。

GameObject类
1. Instantiate（）实例化
instantiate（）是克隆游戏对象的方法，应用非常广泛，
格式：
Instantiate（GameObject，position，rotation）
（1）GameObject是指生成克隆的游戏对象，也可以是Prefeb预制体。
（2）position是游戏对象的初始位置，类型是Vector3.
（3）rotation是生成对象的初始角度，类型是Quaternion。

2. Destroy（）销毁
格式：
Destroy（GameObject，time）；
（1)Gameobject是销毁的游戏对象，time是时间；
下面写一个简单的案例：

	void OnCollisionEnter(Collision collision){
		Destroy(collision.gameObject,2);
	}

//代码控制接触碰撞体后两秒销毁

在Unity中创建几个物体：


给小球附上代码并且加上刚体（rigidbody）使其能自由落体。
我们就会得到这样的效果：


3.GetComponent（）获取组件
GetComponent（）是访问游戏对象组件的方法
格式：
GameObject.GetComponent< type >（）
（1）GameObject是定义GameObject的变量名。
（2）type是组建名称，类型是string。

例如：Ball.GetComponent< Rigidbody >.mass = 20 ;//把rigidbody组件的质量改为20

4.SetActive（）显示/隐藏游戏对象
格式：
GameObject.SetActive（value）；
value是让物体是否隐藏，类型是bool。

Transform类
场景里的每一个对象都有Transform，用来存储物体的大小，旋转，缩放，Transform组件的成员变量有：
（1）transform.position：指物体在世界坐标下的位置。
（2）transform.Translate：指物体的相对移动单位。
（3）transform.Rotate：指物体旋转。
（4）transform.eulerAngels：指物体的角度。
（5）transform.localScale：指物体的缩放（三个轴都不为0，否则会消失）

我们先把小球隐藏掉，写一段代码控制方块的旋转和移动。
```C#
	void Update () {
		if(Input.GetKey(KeyCode.W)){
			transform.Translate(0, 0, 2f * Time.deltaTime);
		}
		if(Input.GetKey(KeyCode.S)){
			transform.Translate(0, 0, -2f * Time.deltaTime);
		}
		if(Input.GetKey(KeyCode.A)){
			transform.Rotate(0, -30 * Time.deltaTime, 0);
		}
		if(Input.GetKey(KeyCode.D)){
			transform.Rotate(0, 30 * Time.deltaTime, 0);
		}
	}
```
运行得到以下效果：

这是一个类似汽车运行的移动控制。

下面实现一组类似泡泡堂中的角色移动控制：
泡泡堂中的角色一般是随着键盘的输入直接改变面向，所以直接用eulerAngels就可以完成！
下面是更改后的代码：
```C#
void Update () {
		if(Input.GetKey(KeyCode.W)){
			transform.Translate(0, 0, 2f * Time.deltaTime);
			transform.eulerAngles = new Vector3(0,0,0);
		}
		if(Input.GetKey(KeyCode.S)){
			transform.Translate(0, 0, 2f * Time.deltaTime);
			transform.eulerAngles = new Vector3(0,180,0);
		}
		if(Input.GetKey(KeyCode.A)){
			//transform.Rotate(0, -30 * Time.deltaTime, 0);
			transform.Translate( 0, 0, 2f * Time.deltaTime);
			transform.eulerAngles = new Vector3(0,270,0);
		}
		if(Input.GetKey(KeyCode.D)){
			//transform.Rotate(0, 30 * Time.deltaTime, 0);
			transform.Translate(0, 0 ,2f * Time.deltaTime);
			transform.eulerAngles = new Vector3(0,90,0);
		}
	}
```
我们就能得到这个效果。注：蓝色标记是面向，注意面向的变化

这里还存在一个问题，就是同时按下两个方向键史面向不会改变。

那么我们需要实现王者荣耀类的角色移动方式该如何做呢，这里我们就应该把移动和转向功能分开，分别实现：
代码如下：
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Control : MonoBehaviour {
	float ver = 0;
	float hor = 0;
	public float turnspeed = 10f;
	public float speed = 6f;
	// Use this for initialization
	void Start () {

	}
	// Update is called once per frame
	void Update () {
		hor = Input.GetAxis ("Horizontal");
		ver = Input.GetAxis ("Vertical");
	}
	void Rotating (float hor, float ver) {
		//获取方向
		Vector3 dir = new Vector3 (hor, 0, ver);
		//将方向转换为四元数
		Quaternion quaDir = Quaternion.LookRotation (dir, Vector3.up);
		//缓慢转动到目标点
		transform.rotation = Quaternion.Lerp (transform.rotation, quaDir, Time.fixedDeltaTime * turnspeed);
	}
	void move (float hor, float ver) {
		transform.Translate (hor * speed * Time.deltaTime, 0, ver * speed * Time.deltaTime, Space.World);
	}
	void FixedUpdate () {
		if (hor != 0 || ver != 0) {
			//转身
			Rotating (hor, ver);
			move (hor, ver);
		}
	}

}
```
最终我们可以得到想要实现的效果了！


RigidBody类
Rigidbody组件可以使物体在物理系统的控制下运动，更加灵活的模拟对象的物理性，通常是在FixedUpdate（）函数中执行，物理仿真一般在固定的间隔时间内计算。
rigidbody类里面有以下几个方法：

1.AddForce
此方法被调用时给物体一个瞬时的力，会产生一个加速运动。

2.AddTorque
给缸体添加一个扭矩

3.Sleep
可以使刚体进入休眠状态（至少休眠一帧），一般在Awake（）函数里面。

4.WakeUp
使刚体从休眠状态唤醒，例如 rigidbody.WakeUp()  
————————————————  
版权声明：本文为CSDN博主「王有种」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/ChinaNinjaQAQ/article/details/88999411