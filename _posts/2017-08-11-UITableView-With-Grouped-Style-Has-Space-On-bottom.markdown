---
layout: post
title:  "去掉Grouped TableView底部的空白"
date:   2017-08-11 13:22:32 +0800
categories: jekyll update
---
# 去掉Grouped TableView底部的空白

当UITableView的style为Grouped时，如果设置section footer的高度为0的话，会发现底部会多出高度为20点的空隙。
> section footer 的高度是通过委托方法`- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section`来设置的。

把tableView的style改成plain后发现此无此空隙，可见此问题是有Grouped Style引起的。

把sction footer的高度改成非0值的后，此空隙也不见了。由此推断是grouped tableView在setion footer高度为0的时候，使用了默认高度。可以通过将setcion footer 高度设置为如`0.01`的非零值的方式来解决。代码如下：
``` objc
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section
{
    if(section == CommentHostSection){
        return kCommentHostSectionFootrHeight;
    }
    return  tableView.style == UITableViewStyleGrouped ? 0.01f : 0.f; // tableView的Style为Grouped时，如果footer/header的高度返回为0，高度会默认为20的高度，所以传入0.01.
}
```

### plain style tableView禁止header和footer浮动的方法

继承UITableView类，在子类覆写方法`- (BOOL)allowsHeaderViewsToFloat`和`- (BOOL)allowsHeaderViewsToFloat`，代码如下：

``` objc
@implementation DQNoHeaderFloatingTableView

- (id)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (self) {
        // Initialization code
    }
    return self;
}

- (BOOL)allowsHeaderViewsToFloat {
    return NO;
}
- (BOOL)allowsFooterViewsToFloat {
    return NO;
}
@end
```
