---
title: "Best Practice - 组件构建"
permalink: /docs/ios/element-best_practice
excerpt: "Best Practice - 组件构建"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "iOS 使用指南"
  nav: ios-docs
---

## 组件样例 ： WebView 组件

嵌套了一个WebView 里面传入一个contentUrl , 展示对应地址内容

### 数据结构:

```

{
    "type":"102",
    //内部的Url
    "contentUrl":"https://www.google.com.hk",
    //跳转地址
    "action":"https://iope.tmall.com/campaign-3734-0.htm",
    "style":{
        "width":"100",
        "height":"200"
    }
}

```

##代码

### TangramYosemiteWebViewElement.h


````objc

#import <UIKit/UIKit.h>
#import "TangramEasyElementProtocol.h"
#import "TangramDefaultItemModel.h"
#import "TangramEvent.h"

@interface TangramYosemiteWebViewElement : UIWebView<TangramEasyElementProtocol,UIWebViewDelegate>

//url
@property (nonatomic, strong) NSString *contentUrl;
//element所在的Layout
@property (nonatomic, weak) UIView<TangramLayoutProtocol>     *atLayout;

@end

````

### TangramYosemiteWebViewElement.m

````objc
#import "TangramYosemiteWebViewElement.h"
#import "TangramLayoutProtocol.h"
#import "TMMuiLazyScrollViewCellProtocol.h"

@interface TangramYosemiteWebViewElement()<UIGestureRecognizerDelegate,TMMuiLazyScrollViewCellProtocol>
// 收到reload请求的次数
@property   (atomic, assign)    int                     numberOfReloadRequests;
// 首次收到reload请求的时间点，毫秒级
@property   (atomic, assign)    NSTimeInterval          firstReloadRequestTS;
@property   (atomic, assign)    BOOL                    shouldReload;
@property (nonatomic, strong) TangramDefaultItemModel *itemModel;
@property (nonatomic, strong) UITapGestureRecognizer *tapGestureRecognizer;
@property (nonatomic, assign) BOOL finishLoad;
@property (nonatomic, assign) BOOL finishInit;
@property   (nonatomic, weak)   TangramBus   *tangramBus;
@end

@implementation TangramYosemiteWebViewElement
@synthesize itemModel = _itemModel;

-(instancetype)init
{
    if (self= [super init]) {
        self.multipleTouchEnabled = NO;
        self.scrollView.scrollEnabled = NO;
        [(UIScrollView *)self.subviews[0] setShowsHorizontalScrollIndicator:NO];
        [(UIScrollView *)self.subviews[0] setShowsVerticalScrollIndicator:NO];
        [(UIScrollView *)self.subviews[0] setBounces:NO];
        self.opaque = NO;
        self.backgroundColor = [UIColor clearColor];
        self.finishInit = YES;
        self.delegate = self;
        [self addGestureRecognizer:self.tapGestureRecognizer];
    }
    return self;
}

#pragma mark - Gesture

-(UITapGestureRecognizer *)tapGestureRecognizer
{
    if (_tapGestureRecognizer == nil) {
        _tapGestureRecognizer = [[UITapGestureRecognizer alloc]initWithTarget:self action:@selector(elementClicked:)];
        _tapGestureRecognizer.delegate = self;
    }
    return _tapGestureRecognizer;
}
- (void)elementClicked:(UITapGestureRecognizer *)gestureRecognizer
{
    if (self.tangramBus) {
        TangramView *tangramView = nil;
        if (self.atLayout && [self.atLayout.superview isKindOfClass:[TangramView class]]) {
            tangramView = (TangramView *)self.atLayout.superview;
        }
        TangramEvent *event = [[TangramEvent alloc]initWithTopic:@"TangramEventTopicJumpAction" withTangramView:tangramView posterIdentifier:@"webview" andPoster:self];
        [event setParam:self.itemModel.action forKey:@"action"];
        [self.tangramBus postEvent:event];
    }
}

#pragma mark - Getter & Setter
-(void)setFrame:(CGRect)frame
{
    [super setFrame:frame];
    if (self.finishInit && frame.size.width > 0 && frame.size.height > 0) {
         self.scrollView.contentSize = frame.size;
        if (![self.gestureRecognizers containsObject:self.tapGestureRecognizer]) {
            [self addGestureRecognizer:self.tapGestureRecognizer];
        }
    }

}

-(void)setHtmlContent:(NSString *)htmlContent
{
    _htmlContent = htmlContent;
    if (htmlContent.length > 0) {
        [self loadHTMLString:htmlContent baseURL:nil];
        self.finishLoad = NO;
    }
}

-(void)setContentUrl:(NSString *)contentUrl
{
    //这个方法中设置内部的url
    if ([contentUrl isEqualToString:self.contentUrl]) {
        return;
    }
    else if(contentUrl.length > 0){
        NSURL *targetURL = [NSURL URLWithString:contentUrl];
        NSURLRequest *request = [NSURLRequest requestWithURL:targetURL];
        [self loadRequest:request];
        self.finishLoad = NO;
    }
    _contentUrl = contentUrl;
}
-(TangramDefaultItemModel *)itemModel
{
    return _itemModel;
}
-(void)setItemModel:(TangramDefaultItemModel *)itemModel
{
    _itemModel = itemModel;
}

#pragma mark - TMMuiLazyScrollViewCellProtocol
- (void)mui_prepareForReuse
{
    //在这个方法里可以在组件被复用之前做一些动作，比如清空数据
    //[self stopLoading];
}
- (void)mui_afterGetView
{
    //这个方法里面已经获取到了组件的宽，高，model等能拿到的全部信息
    //这个方法里面可以做组件的内部布局

}
- (void)mui_didEnterWithTimes:(NSUInteger)times
{
    //这里可以做曝光的处理
    if (times == 0)
    {
        // do some thing here ...
    }
}

#pragma mark - WebView

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType
{
     NSString *action = (NSString *)[self.itemModel bizValueForKey:@"action"];
    if ([action isKindOfClass:[NSString class]] && action.length > 0
        && (navigationType == UIWebViewNavigationTypeLinkClicked || navigationType == UIWebViewNavigationTypeOther)
        && self.finishLoad == YES) {
        return NO;
    }
    return YES;
}
- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer
{
    return YES;
}


- (void)webViewDidFinishLoad:(UIWebView *)webView
{
    self.finishLoad = YES;
}


@end
````