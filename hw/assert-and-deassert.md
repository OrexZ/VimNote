
## 这里说明一下在硬件设计中的常用术语'assert'与'de-assert'

> assert：

把信号变为active（可以理解为有效），根据系统需求不同，该有效电平可以是高电平（即高有效）也可以是低电平（即低有效）。


> de-assert：

解除active状态，信号变为非active状态，可以是高也可以是低。


英文解释：

    Assert：Set a signal to its “active” state；
    De-assert： Set a signal to its “inactive” state。

    If a signal is active-low，“asserting” that signal means setting it low and deasserting it means setting it high。

