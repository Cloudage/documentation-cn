#Script Execution Order 设置

请参阅有关[事件函数执行顺序](ExecutionOrder.html)的文档，了解 Unity 在默认情况下如何处理事件函数。

可以使用 __Script Execution Order__ 设置（菜单：__Edit &gt; Project Settings &gt; Script Execution Order__）。

![](../uploads/Main/ScriptExecSet.png) 

可使用加号“+”按钮将脚本添加到 Inspector 中，并可通过拖动来更改其相对顺序。请注意，可以将脚本拖动到 __Default Time__ 栏的上方或下方；上方的脚本将在默认时间之前执行，而下方的脚本将在默认时间之后执行。对话框中从上到下的脚本顺序决定了脚本的执行顺序。不在对话框中的所有脚本都以任意顺序在默认时隙中执行。

对每个脚本显示的编号是脚本实际排序的值。将脚本拖动到新位置时，脚本的编号会相应地自动更改。当手动或自动更改编号时，会更改该脚本的元文件。因此，更改顺序后，最好尽可能减少编号更改。这就是为什么在可能的情况下，只有被拖动的脚本的编号才会更改，而不是为所有脚本分配新编号。
