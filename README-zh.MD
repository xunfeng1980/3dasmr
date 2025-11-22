# 为什么搞这个
在听书的时候，发现很困，正好最近在研究空间音频，就做了个类似空间音频的效果，因为使用的是人耳的模型，所以最好使用耳机来体验，
然后在听 Youtube ASMR 的时候，发现加了这个玩意更加沉浸和酥麻了，如果是一些奇怪的影片估计效果也比较离谱。。。

# 使用
1. 安装 BlackHole 2CH 和设置默认音频输出设备为 BlackHole 2CH 
```
brew install --cask blackhole-2ch
```
![alt text](assets/image.png)

2. 下载 main.bin ，执行并开启 Auto Rotate，也可以重新选择输出设备
![alt text](assets/image1.png)

// todo
3. Youtube 3D ASMR 效果演示



# 原理
1. 使用 blackhole 获取系统音频输出，再通过 hrtf 进行空间音频合成，然后再送到扬声器

