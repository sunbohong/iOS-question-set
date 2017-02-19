*  Xcode 调试时，如何显示一个基本变量类型的二进制？

通过 ```po/x 32``` 即可显示变量的二进制内容。

扩展阅读：http://lldb.llvm.org/lldb-gdb.html



* 一位小密圈的朋友遇到一个问题，当音频超过20s时，通过 AVURLAsset 读取的音频时长不准确。原始代码如下。

```

AVURLAsset  *audioAsset =[AVURLAsset URLAssetWithURL:[NSURL fileURLWithPath:urlStr] options:nil];

CMTime audioDuration = audioAsset.duration;

float audioDurationSeconds =CMTimeGetSeconds(audioDuration);

NSLog(@"播放   大小-== %f M   长度：   %f  地址： %@",Byte/1024.0/1024.0,audioDurationSeconds,urlStr);

```



答案： AV 库的很多内容是经过充分优化的（懒加载）。如果需要获取准确的音频时长，需要通过以下代码读取。

```

NSArray *keys = @[@"duration"];

AVURLAsset  *audioAsset =[AVURLAsset URLAssetWithURL:[NSURL fileURLWithPath:urlStr] options:@{AVURLAssetPreferPreciseDurationAndTimingKey:@YES}];

    [audioAsset loadValuesAsynchronouslyForKeys:keys completionHandler:^{

        CMTime audioDuration = audioAsset.duration;

        float audioDurationSeconds =CMTimeGetSeconds(audioDuration);

        NSLog(@"播放   大小-== %f M   长度：   %f  地址 ：%@",Byte/1024.0/1024.0,audioDurationSeconds,urlStr);

    }];

```
