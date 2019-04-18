远程操作
===============

网络系统具有在网络上执行操作的方法。这些类型的操作有时称为“远程过程调用”。在网络系统中有两种类型的 RPC：命令（从客户端调用并在服务器上运行）和 ClientRpc 调用（在服务器上调用并在客户端上运行）。

下图显示了执行远程操作的方向：

![](../uploads/Main/UNetDirections.jpg) 


命令
--------

命令从客户端上的玩家对象发送到服务器上的玩家对象。为了安全起见，命令只能从您自己的玩家对象发送，因此您无法控制其他玩家的对象。要将函数转换为命令，请为其添加 [Command] 自定义属性，并添加“Cmd”前缀。现在，在客户端上调用此函数时，将在服务器上运行此函数。所有参数都将通过该命令自动传递到服务器。

Command 函数必须具有前缀“Cmd”。这是读取调用命令的代码时的提示 - 此函数比较特殊，不像普通函数那样在本地调用。


````
class Player : NetworkBehaviour
{

	public GameObject bulletPrefab;

	[Command]
	void CmdDoFire(float lifeTime)
	{
		GameObject bullet = (GameObject)Instantiate(
			bulletPrefab, 
			transform.position + transform.right,
			Quaternion.identity);
			
		var bullet2D = bullet.GetComponent<Rigidbody2D>();
		bullet2D.velocity = transform.right * bulletSpeed;
		Destroy(bullet, lifeTime);

		NetworkServer.Spawn(bullet);
	}

	void Update()
	{
		if (!isLocalPlayer)
			return;

		if (Input.GetKeyDown(KeyCode.Space))
		{
			CmdDoFire(3.0f);
		}

	}
}
````

务必注意每帧都从客户端发送命令的情况！这种情况可能会导致大量网络流量。

默认情况下，命令在通道零上发送（此通道默认的可靠通道）。因此，默认情况下，所有命令都能可靠发送到服务器。可使用 [Command] 自定义属性的“Channel”参数对该通道进行自定义。此参数应为整数，表示通道编号。

默认情况下，通道 1 也设置为不可靠的通道，因此要使用该通道，请在 Command 属性中使用值 1 作为参数，如下所示：

````
	[Command(channel=1)]
````


从 Unity 版本 5.2 开始，可从具有客户端授权的非玩家对象发送命令。这些对象必须是使用 [NetworkServer.SpawnWithClientAuthority](../ScriptReference/Networking.NetworkServer.SpawnWithClientAuthority.html) 生成的对象，或者使用 [NetworkIdentity.AssignClientAuthority](../ScriptReference/Networking.NetworkIdentity.AssignClientAuthority.html) 设置了授权。从这些对象发送的命令在服务器的对象实例上运行，而不是在客户端的关联玩家对象上运行。

<a name="ClientRPC"></a>
ClientRpc 调用
-----------

ClientRpc 调用从服务器上的对象发送到客户端上的对象。可从任何具有已生成的 NetworkIdentity 的服务器对象发送这些调用。由于服务器具有授权，因此能够发送这些调用的服务器对象不会存在安全问题。要将函数转换为 ClientRpc 调用，请为其添加 [ClientRpc] 自定义属性，并添加“Rpc”前缀。现在，在服务器上调用此函数时，将在客户端上运行此函数。所有参数都将通过该 ClientRpc 调用自动传递到客户端。

ClientRpc 函数必须具有前缀“Rpc”。这是读取调用方法的代码时的提示 - 此函数比较特殊，不像普通函数那样在本地调用。

````
class Player : NetworkBehaviour
{

	[SyncVar]
	int health;

	[ClientRpc]
	void RpcDamage(int amount)
	{
		Debug.Log("Took damage:" + amount);
	}

	public void TakeDamage(int amount)
	{
		if (!isServer)
			return;

		health -= amount;
		RpcDamage(amount);
	}
}
````

当游戏作为具有 LocalClient 的主机运行时，将在 LocalClient 上调用 ClientRpc 调用，即使与服务器处于同一进程中也会执行此操作。因此，对于 ClientRpc 调用，LocalClient 和 RemoteClient 的行为是相同的。

远程操作的参数
---------------------------
传递给命令和 ClientRpc 调用的参数经过序列化并通过网络发送。参数可以是：

* 基本类型（byte、int、float、string、UInt64 等等）
* 基本类型的数组
* 包含允许类型的结构
* Unity 内置数学类型（Vector3、Quaternion 等等）
* NetworkIdentity
* NetworkInstanceId
* NetworkHash128
* 附加了 NetworkIdentity 组件的游戏对象

远程操作的参数不能是游戏对象的子组件，例如脚本实例或变换。这些参数不能是无法通过网络进行序列化的其他类型。
