# **NeverlandBeats**

## **使用 Python，Adapter EIM 插件和 Scratch 实时绘制音乐频谱**

这部分目前有以下两个项目，原理都是用 python 对音频数据做实时分析，同时经 Adapter EIM 插件将分析结果发给 Scratch，再使用画笔功能动态绘制图形。

音频流的配置与数据提取依赖 pyaudio 实现，音频数据分析就是将信号从时域经傅立叶转换到频域。P1 与 P2 的区别在于，P2 只是对每次从音频流中提取的数据做 FFT（Fast Fourier transform）分析，然后将 0-22050Hz 的频率按对数关系分为 10 个频段呈现对应的振幅强度，在 Scratch 中类似 bar 图效果；P1 相比 P2 多了时间的维度，做的是 STFT（Short-time Fourier transform） 分析，依赖 madmom 包获取 spectrogram 结果，对应 Scratch 中 12 * 10 的圆点矩阵，每 1 列是一个时间点，12 行数据分别对应 12 个频段的振幅强度。

运行 python 代码前，需要先选择音源的输入设备，是来自电脑内置的麦克风、还是耳机等，当前代码不包含选择输入设备这一项功能（但依赖 pyaudio 其实可以做到），pyaudio 会使用默认的输入设备，而输入设备间的切换是通过 pavucontrol（ubuntu 系统）手动实现的。如果发现没有数据很可能就是音源输入设备的选择问题。因为 madmom 也是依赖 pyaudio，所以 P1、P2 都是如此。


### **P1：Scratch 代码[在此](https://create.codelab.club/projects/9942/)，Python 代码[在此](AA_madmomspectrogram.ipynb)**


**待优化：**

+ Scratch 实时画图过程中，每隔一段时间会有明显的卡顿，还不清楚原因

+ Scratch 中圆点的颜色用来反映该频段振幅强度的大小，目前颜色与数值的映射关系比较简单，视觉效果一般，下面可尝试参照 matplotlib 在 Scratch 中实现 colormap

+ 相比 P2 多了时间的维度，但只是 1 帧呈现 10 个时间点，然后靠屏幕刷新反映时间的变化，考虑是否将视觉效果做成图形自右向左流动呈现

### **P2：Scratch 代码[在此](https://create.codelab.club/projects/9943/)，Python 代码[在此](AA_realtime_audiofft.ipynb)** 

**待优化：**

+ Scratch Addon 支持更高的 fps（=60）、高清画笔以及舞台大小自定义，视觉效果应该会更好


## **依赖 ❤️**


+ **[pyaudio](https://people.csail.mit.edu/hubert/pyaudio/docs/)**

    Ubuntu 可能需要先安装依赖： ```sudo apt install portaudio19-dev```

+ **[madmom](https://github.com/CPJKU/madmom)**

    最好参照[官方文档](https://madmom.readthedocs.io/en/latest/installation.html#install-from-source)复制仓库源码安装开发版，因为后面实时分析节拍时可能要使用稳定版中没有的脚本  

    不支持 ```pip install -e git+https://github.com/CPJKU/madmom#egg=madmom``` 这种安装方式

+ **[CodeLab Adapter](https://adapter.codelab.club/get_start/gs_install/)**

+ **[numpy](https://numpy.org/)**


## **参考 ❤️**


**关于傅立叶转换**

+ [But what is the Fourier Transform? A visual introduction](https://www.youtube.com/watch?v=spUNpyF58BY)

+ [An Interactive Guide To The Fourier Transform](https://betterexplained.com/articles/an-interactive-guide-to-the-fourier-transform/)

+ [(Visual) Understanding the Fourier transform](https://web.archive.org/web/20120418231513/http://www.altdevblogaday.com/2011/05/17/understanding-the-fourier-transform/)

**关于音频流**

+ [Audio I/O: Buffering, Latency, and Throughput](https://in.mathworks.com/help/audio/gs/audio-io-buffering-latency-and-throughput.html)

    matlab audiotoolbox 系列文档有很清晰的解释

**关于音频的实时分析与频谱绘制**

+ [Frequency spectrum using FMOD and UE4](https://www.parallelcube.com/2018/03/10/frequency-spectrum-using-fmod-and-ue4/)

    如何 track the beat，作者写了一系列的文章，虽然用的不同软件，但是作者分享的思路非常重要。P2 中对频段的划分就是依据这篇文章。

+ [Recording Stereo Audio on a Raspberry Pi](https://makersportal.com/blog/recording-stereo-audio-on-a-raspberry-pi)

    这个网站的作者分享了多个音频相关的项目，是读过教程中对数据提取与分析流程最完整严谨的。

+ [Audio Handling Basics: Process Audio Files In Command-Line or Python](https://hackernoon.com/audio-handling-basics-how-to-process-audio-files-using-python-cli-jo283u3y)

+ [Realtime FFT Audio Visualization with Python](https://swharden.com/blog/2013-05-09-realtime-fft-audio-visualization-with-python/)

+ [Realtime FFT Audio Visualization with Python](https://blog.yjl.im/2012/11/frequency-spectrum-of-sound-using.html)

