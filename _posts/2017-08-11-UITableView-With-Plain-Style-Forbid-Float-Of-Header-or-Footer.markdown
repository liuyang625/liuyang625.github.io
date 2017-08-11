---
layout: post
title:  "plain style tableView禁止header和footer浮动的方法"
date:   2017-08-11 14:45:32 +0800
categories: jekyll update
---

# plain style tableView禁止header和footer浮动的方法

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
