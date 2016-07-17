## UITableVeiw
```


contrller <UITableViewDelegate>


UITableViewDelegate的方法

titleForHeaderInSection //每个section显示的标题

numberOfSectionsInTableView //指定有多少个分区(Section)，默认为1

numberOfRowsInSection //指定每个分区中有多少行，默认为1

cellForRowAtIndexPath //绘制cell

heightForHeaderInSection  //每个section底部标题高度（实现这个代理方法后前面 sectionHeaderHeight 设定的高度无效）

viewForHeaderInSection   //用以定制自定义的section头部视图－Header

heightForFooterInSection   //每个section头部标题高度（实现这个代理方法后前面 sectionFooterHeight 设定的高度无效）

viewForFooterInSection   //用以定制自定义的section底部视图－Footer

heightForRowAtIndexPath //改变行的高度（实现主个代理方法后 rowHeight 设定的高度无效）

didSelectRowAtIndexPath //选中Cell响应事件

willDisplayCell  //行将显示的时候调用，预加载行

willSelectRowAtIndexPath //选中之前执行,判断选中的行（阻止选中第一行）

... tableDelegate 有很多的的代理接口用到时候再看｀


```
