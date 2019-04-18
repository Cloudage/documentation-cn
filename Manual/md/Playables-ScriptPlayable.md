#ScriptPlayable 和 PlayableBehaviour

要创建自定义的可播放项，必须从 PlayableBehaviour 基类继承。
```
public class MyCustomPlayableBehaviour : PlayableBehaviour
{
    // 自定义可播放项行为的实现
    // 根据需要重载 PlayableBehaviour 方法
}
```
 
要将 PlayableBehaviour 用作自定义可播放项，还必须将其封装在 ScriptPlayable&lt;&gt; 对象内。如果不具备自定义可播放项的实例，可通过调用以下函数为对象创建 ScriptPlayable&lt;&gt;：
 
```
ScriptPlayable<MyCustomPlayableBehaviour>.Create(playableGraph);
```
 
如果已有自定义可播放项的实例，可通过调用以下函数用 ScriptPlayable&lt;&gt; 来包裹该实例：
 
```
MyCustomPlayableBehaviour myPlayable = new MyCustomPlayableBehaviour();
ScriptPlayable<MyCustomPlayableBehaviour>.Create(playableGraph, myPlayable);
```
 
此情况中将克隆该实例，然后将实例分配给 ScriptPlayable&lt;&gt;。实际上，此代码与先前代码执行的操作完全相同；不同之处在于 `myPlayable` 可能是将要在 Inspector 中配置的公有属性，然后可为脚本的每个实例设置行为。
 
您可以使用 `ScriptPlayable<T> .GetBehaviour()` 方法从 ScriptPlayable&lt;&gt; 获取 PlayableBehaviour 对象。

---

* <span class="page-edit">2017-07-04  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">Unity [2017.1](../Manual/30_search.html?q=newin20171) 中的新功能 <span class="search-words">NewIn20171</span></span>
