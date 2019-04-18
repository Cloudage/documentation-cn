# C# Job System Overview

## How the C# Job System works

The Unity C# Job System allows users to write [multithreaded code](https://en.wikipedia.org/wiki/Multithreading_(computer_architecture)) that interacts well with the rest of Unity and makes it easier to write correct code.

Writing multithreaded code can provide high-performance benefits. These include significant gains in frame rate and improved battery life for mobile devices.

An essential aspect of the C# Job System is that it integrates with what Unity uses internally (Unity’s native job system). User-written code and Unity share [worker threads](https://docs.microsoft.com/en-us/cpp/parallel/multithreading-creating-worker-threads). This cooperation avoids creating more threads than [CPU cores](https://en.wikipedia.org/wiki/Multi-core_processor), which would cause contention for CPU resources. 

For more information, watch the talk [Unity at GDC - Job System & Entity Component System](https://www.youtube.com/watch?v=kwnb9Clh2Is&t=1s).

---

* <span class="page-edit">2018-06-15  Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2018.1](https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin20181) 版中公开了 C# 作业系统 <span class="search-words">NewIn20181</span></span>
