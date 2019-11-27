---
title: NSNumber And NSDictionary
date: 2016-12-28 00:12:43
categories: iOS #文章文类
tags: [Objective-C] #文章标签，多于一项时用这种格式
---
NSNumber and NSDictionary are the basic class and most useful class in objective c.
<!--more-->

``` c
//实时获取下载进度百分比
- (void) dataDownloadAtPercent:(PvUrlSessionDownload*)downloader withVid:(NSString *)vid percent: (NSNumber *) aPercent{
    
    //  [[FMDBHelper sharedInstance] updateDownloadPercent:vid percent:aPercent];
    if(nil == map){
        map = [NSMutableDictionary dictionaryWithCapacity:10];
    }
    
    dispatch_async(dispatch_get_main_queue(), ^{
        int x = 0;
        int y = [aPercent intValue];
        if(nil == [map objectForKey:vid]){
           [map setObject:[[NSNumber alloc] initWithInt:y] forKey:vid];
        } else{
            if([[map objectForKey:vid] isKindOfClass:[NSNumber class]]){
                x = [[map objectForKey:vid] intValue];
            }
        }
        
        RCTLogInfo(@"来自Xcode的信息－下载了:%@|%d|%d",aPercent,y,x);
        if(x!=y){
            [map setObject:[[NSNumber alloc] initWithInt:y] forKey:vid];
            [self.bridge.eventDispatcher sendAppEventWithName:@"DownloadEvent"
                                                         body:@{@"what": @4,
                                                                @"status": [NSNumber numberWithInt:y],
                                                                @"name": @"downloadAtPercent",
                                                                @"vid":vid}];
        }

    });
}

```

``` c
//基本数据类型
//专门用来装基础类型的对象
NSNumber * intNumber = [[NSNumber alloc] initWithInt:5];
NSNumber * floatNumber = [[NSNumber alloc] initWithFloat:3.14f];
NSNumber * doubleNumber = [[NSNumber alloc] initWithDouble:6.7];
NSNumber * charNumber = [[NSNumber alloc] initWithChar:'A'];
//这种比较也是可以跨不同对象的，比如：比较intNumber和floatNumber
BOOL ret = [intNumber isEqualToNumber:intNumber2]; 
//比较两个整型的NSNumber的大小
if ([intNumber compare:intNumber] == NSOrderedAscending) {
    NSLog(@"<");
}else if([intNumber compare:intNumber2] == NSOrderedSame){
    NSLog(@"=");
}else if([intNumber compare:intNumber2] == NSOrderedDescending){
    NSLog(@">");
}
//通过以下方法可以还原这些基本数据类型的数据
NSLog(@"%d", [intNumber intValue]);
NSLog(@"%f", [floatNumber floatValue]);
NSLog(@"%f", [doubleNumber doubleValue]);
NSLog(@"%c", [charNumber charValue]);

```