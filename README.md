# **NeverlandBeats**

## **使用 Python，Adapter EIM 插件和 Scratch 实时绘制音乐频谱**

这部分目前有以下两个项目，原理都是用 python 对音频数据做实时分析，同时经 Adapter EIM 插件将分析结果发给 Scratch，再使用画笔功能动态绘制图形。

音频流的配置与数据提取依赖 pyaudio 实现，音频数据分析就是将信号从时域经傅立叶转换到频域。P1 与 P2 的区别在于，P2 只是对每次从音频流中提取的数据做 FFT（Fast Fourier transform）分析，然后将 0-22050Hz 的频率按对数关系分为 10 个频段呈现对应的振幅强度，在 Scratch 中类似 bar 图效果；P1 相比 P2 多了时间的维度，做的是 STFT（Short-time Fourier transform） 分析，依赖 madmom 包获取 spectrogram 结果，对应 Scratch 中 12 * 10 的圆点矩阵，每 1 列是一个时间点，12 行数据分别对应 12 个频段的振幅强度。


### **P1：Scratch 代码[在此](https://create.codelab.club/projects/9942/)，Python 代码[在此](AA_madmomspectrogram.ipynb)**


**待优化：**

+ Scratch 中圆点的颜色用来反映该频段振幅强度的大小，目前颜色与数值的映射关系比较简单，视觉效果一般，下面可尝试参照 matplotlib 在 Scratch 中实现 colormap

+ 相比 P2 多了时间的维度，但只是 1 帧呈现 10 个时间点，然后靠屏幕刷新反映时间的变化，考虑是否将视觉效果做成图形自右向左流动呈现

### **P2：Scratch 代码[在此](https://create.codelab.club/projects/9943/)，Python 代码[在此](AA_realtime_audiofft.ipynb)** 

**待优化：**

+ Scratch Addon 支持更高的 fps（=60）、高清画笔以及舞台大小自定义，视觉效果应该会更好


## **依赖**

+ **[pyaudio](https://people.csail.mit.edu/hubert/pyaudio/docs/)**

    Ubuntu 可能需要先安装依赖： ```sudo apt install portaudio19-dev```

+ **[madmom](https://github.com/CPJKU/madmom)**

    最好参照[官方文档](https://madmom.readthedocs.io/en/latest/installation.html#install-from-source)复制仓库源码安装开发版，因为后面实时分析节拍时可能要使用稳定版中没有的脚本  

    不支持 ```pip install -e git+https://github.com/CPJKU/madmom#egg=madmom``` 这种安装方式

+ **[CodeLab Adapter](https://adapter.codelab.club/get_start/gs_install/)**

+ **[numpy](https://numpy.org/)**

