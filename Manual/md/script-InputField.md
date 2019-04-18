#输入字段 (Input Field)

__输入字段__是一种使[文本 (Text) 控件](script-Text.html)的文本可编辑的方法。与其他交互控件一样，输入字段本身不是可见的 UI 元素，必须与一个或多个可视 UI 元素组合才能显示。

![空的输入字段。](../uploads/Main/UI_InputFieldExample.png)

![在输入字段中输入的文本。](../uploads/Main/UI_InputFieldExample2.png)

![](../uploads/Main/UI_InputFieldInspector.png) 

##属性

|**_属性：_**||**_功能：_** |
|:---|:---|:---|
|__**Interactable**__ ||一个用于确定输入字段是否可交互的布尔值。|

|__**Transition**__ ||[Transitions](script-SelectableTransition.html) are used to set how the input field transitions when ***Normal***, ***Highlighted***, ***Pressed*** or ***Disabled***. |
|__**Navigation**__ || Properties that determine the sequence of controls. See [Navigation Options](script-SelectableNavigation.html).|

|__**TextComponent**__ ||对用作_*输入字段*_内容的[文本](script-Text.html)元素的引用|

|__**Text**__ ||起始值。开始编辑前置于字段中的初始文本。|

|__**Character Limit**__ ||可在输入字段中输入的最大字符数的值。|

|__**Content Type**__ ||Define the type(s) of characters that your input field accepts|
| |__Standard__ |Any charcter can be entered.|
| |__Autocorrected__ |The autocorrection determines whether the input tracks unknown words and suggests a more suitable replacement candidate to the user, replacing the typed text automatically unless the user explicitly overrides the action.|
| |__Integer Number__ |Allow only whole numbers to be entered.|
| |__Decimal Number__ |Allow only numbers and a single decimal point to be entered.|
| |__Alphanumeric__ |Allow both letters and numbers. Symbols cannot be entered.|
| |__Name__ |Automatically capitalizes the first letter of each word. Note that the user can circumvent the capitalization rules using the __Delete__ key.|
| |__Email Address__ |Allows you to enter an Alphanumeric string consisting of a maximum of one @ sign. periods/baseline dots cannot be entered next to each other. |
| |__Password*__ |Conceals the characters inputed with an asterisk. Allows symbols.|
| |__Pin__ |Conceals the characters inputed with an asterisk. Only allows only whole numbers to be entered.|
| |__Custom__ |Allows you to customise the Line Type, Input Type, Keyboard Type and Character Validation.|

|__**Line Type**__ ||Defines how test is formatted inside the text field.|
| |__Single Line__ |Only allows text to be on a single line.|
| |__Multi Line Submit__ |Allows text to use multiple lines. Only uses a new line when needed.|
| |__Multi Line Newline__ |Allows text to use multiple lines. User can use a newline by pressing the return key.|

|__**Placeholder**__ ||这是一个可选的“空”[图形](../ScriptReference/UI.Graphic.html)，用于表明_*输入字段*_不包含文本。请注意，即使选择了_*输入字段*_（即获得焦点），仍会显示此“空”图形。如：“Enter text...”。|

|__**Caret Blink Rate**__ ||定义该行上的标记的闪烁速率（用于指示建议插入文本）。|

|__**Selection Color**__ ||所选文本部分的背景颜色。|

|__**Hide Mobile Input** (iOS only)__ ||Hides the native input field attached to the onscreen keyboard on mobile devices. Note that this only works on iOS devices.|
| | | |

##事件

|**_属性：_** |**_功能：_** |
|:---|:---|
|__On Value Change__ | 输入字段的文本内容发生变化时调用的 [UnityEvent](UnityEvents.html)。该事件可将当前文本内容作为 `string` 类型动态参数发送。 |
|__End Edit__ | 用户完成文本内容的编辑（通过提交操作或单击某个位置以将焦点移出输入字段）时调用的 [UnityEvent](UnityEvents.html)。该事件可将当前文本内容作为 `string` 类型动态参数发送。 |


##详细信息

可从菜单 (__Component &gt; UI &gt; Input Field__) 中将输入字段 (Input Field) 脚本添加到任何现有的文本控件对象。完成此操作后，还应将该对象拖动到输入字段的 _Text_ 属性以便启用编辑。

文本控件本身的 _Text_ 属性将随用户输入而变化，并可在编辑后从脚本中检索值。请注意，可编辑的文本控件有意不支持富文本 (Rich Text)；该字段将在输入时立即应用富文本标记，但标记基本上会“消失”，并且没有后续方法可更改或删除样式。


##提示

* 要获取输入字段的文本，请使用 InputField 组件本身的 Text 属性，而不是使用显示文本的文本组件的 Text 属性。文本组件的 Text 属性可能会被裁剪，也可能包含隐藏密码的星号。
