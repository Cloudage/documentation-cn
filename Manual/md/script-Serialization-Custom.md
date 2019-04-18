# 自定义序列化

序列化是将数据结构或对象状态转换为 Unity 可存储并在以后可重构的格式的自动过程。（请参阅有关[脚本序列化](script-Serialization.html)的文档以了解关于 Unity 序列化的更多信息。）

有时可能希望序列化 Unity 的序列化程序不支持的内容。在许多情况下，最好的办法是使用序列化回调。（请参阅 Unity 的脚本 API 参考：[ISerializationCallbackReceiver](../ScriptReference/ISerializationCallbackReceiver.html)，了解关于使用序列化回调进行自定义序列化的更多信息。）

序列化回调可让您在序列化程序从字段中读取数据之前以及在完成对字段的写入之后收到通知。使用序列化回调可在运行时为难以序列化的数据赋予不同于实际序列化时的表示形式。


为实现此目的，请在 Unity 要序列化数据之前将数据转换为 Unity 理解的表示形式。然后，在 Unity 将数据写入字段之后，可将序列化的形式转换回在运行时需要的数据形式。

例如：您需要一个树数据结构。如果让 Unity 直接序列化该数据结构，“不支持 null”限制将导致数据流变得非常大，从而在许多系统中引起性能下降。下面的示例 1 中显示了这一情况。

**示例 1**：Unity 的直接序列化，导致性能问题

````

using UnityEngine;
using System.Collections.Generic;
using System;

public class VerySlowBehaviourDoNotDoThis : MonoBehaviour {
	[Serializable]
	public class Node {
		public string interestingValue = "value";
		//下面的字段使序列化数据变得巨大，
		//因为它引入了“类周期”。
		public List<Node> children = new List<Node>();
	}
	//这将经过序列化
	public Node root = new Node();
	void OnGUI() {
		Display (root);
	}
	void Display(Node node) {
		GUILayout.Label ("Value: ");
		node.interestingValue = GUILayout.TextField(node.interestingValue, GUILayout.Width(200));
		GUILayout.BeginHorizontal ();
		GUILayout.Space (20);
		GUILayout.BeginVertical ();
		foreach (var child in node.children)
		Display (child);
		if (GUILayout.Button ("Add child"))
		node.children.Add (new Node ());
		GUILayout.EndVertical ();
		GUILayout.EndHorizontal ();
	}
}
````

相反，您告诉 Unity 不要直接序列化树，并创建单独的字段来以序列化的格式（适用于 Unity 序列化程序）存储树。下面的示例 2 中显示了这一情况。

**示例 2**：避免 Unity 直接序列化，并避免性能问题

````

using System.Collections.Generic;
using System;

public class BehaviourWithTree : MonoBehaviour, ISerializationCallbackReceiver {
	// 在运行时使用的 Node 类。
	//此类位于 BehaviourWithTree 类的内部，不会被序列化。
	public class Node {
		public string interestingValue ="value";
		public List<Node> children = new List<Node>();
	}
	// 我们将用于序列化的 Node 类。
	[Serializable]
	public struct SerializableNode {
		public string interestingValue;
		public int childCount;
		public int indexOfFirstChild;
	}
	//用于运行时树表示的根节点。不序列化。
	Node root = new Node();
	//这是我们提供给 Unity 进行序列化的字段。
	public List<SerializableNode> serializedNodes;
	public void OnBeforeSerialize() {
		//Unity 即将读取 serializedNodes 字段的内容。
		// 现在必须“及时”将正确的数据写入该字段。
		if (serializedNodes == null) serializedNodes = new List<SerializableNode>();
		if (root == null) root = new Node ();
		serializedNodes.Clear();
		AddNodeToSerializedNodes(root);
		// 现在 Unity 可自由地序列化这个字段，我们应该在稍后反序列化时
		// 找回预期的数据。
	}
	void AddNodeToSerializedNodes(Node n) {
		var serializedNode = new SerializableNode () {
			interestingValue = n.interestingValue,
			childCount = n.children.Count,
			indexOfFirstChild = serializedNodes.Count+1
		}
		;
		serializedNodes.Add (serializedNode);
		foreach (var child in n.children)
		AddNodeToSerializedNodes (child);
	}
	public void OnAfterDeserialize() {
		//Unity 刚刚将新数据写入 serializedNodes 字段。
		//让我们用这些新值填充我们的实际运行时数据。
		if (serializedNodes.Count > 0) {
			ReadNodeFromSerializedNodes (0, out root);
		} else
		root = new Node ();
	}
	int ReadNodeFromSerializedNodes(int index, out Node node) {
		var serializedNode = serializedNodes [index];
		//将反序列化的数据传输到内部 Node 类
		Node newNode = new Node() {
			interestingValue = serializedNode.interestingValue,
			children = new List<Node> ()
		}
		;
		// 以深度优先的方式读取树，因为这正是我们写入树的方式。
		for (int i = 0; i != serializedNode.childCount; i++) {
			Node childNode;
			index = ReadNodeFromSerializedNodes (++index, out childNode);
			newNode.children.Add (childNode);
		}
		node = newNode;
		return index;
	}
	// 此 OnGUI 在 Game 视图中绘制出节点树，其中包含用于添加新节点作为子项的按钮。
	void OnGUI() {
		if (root != null)
		Display (root);
	}
	void Display(Node node) {
		GUILayout.Label ("Value: ");
		// 允许修改节点的“有用值”。
		node.interestingValue = GUILayout.TextField(node.interestingValue, GUILayout.Width(200));
		GUILayout.BeginHorizontal ();
		GUILayout.Space (20);
		GUILayout.BeginVertical ();
		foreach (var child in node.children)
		Display (child);
		if (GUILayout.Button ("Add child"))
		node.children.Add (new Node ());
		GUILayout.EndVertical ();
		GUILayout.EndHorizontal ();
	}
}

````
<br/><br/> 

----

<span class="page-edit">• 2017-05-15  Page published with [editorial review](DocumentationEditorialReview.html)
</span><br/>
