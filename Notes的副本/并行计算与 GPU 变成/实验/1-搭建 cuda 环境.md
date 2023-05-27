## 搭建 cuda 开发环境

1. 安装 2022Ubuntu 版本的 wsl2

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325111639366.png" alt="image-20230325111639366" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325113131620.png" alt="image-20230325113131620" style="zoom:50%;" />

2. 检查是否安装成功

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325113739450.png" alt="image-20230325113739450" style="zoom:50%;" />

3. 进入 wsl 子系统后，查看系统能否识别 GPU（如果没有下面这行，需要更新显卡驱动）

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325151900322.png" alt="image-20230325151900322" style="zoom:50%;" />

4. 安装 gcc

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325152007818.png" alt="image-20230325152007818" style="zoom:50%;" />

5. 查看是否安装成功

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325152207882.png" alt="image-20230325152207882" style="zoom:50%;" />

6. 安装 cuda toolkit

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325205855969.png" alt="image-20230325205855969" style="zoom:50%;" />

7. 查看 cuda 编译器是否安装成功

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325212104712.png" alt="image-20230325212104712" style="zoom:50%;" />

8. 顺便看看适配系统和硬件的 cuda 版本

、<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325212129333.png" alt="image-20230325212129333" style="zoom:50%;" />

9. 安装 cuda

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325225648339.png" alt="image-20230325225648339" style="zoom:50%;" />



<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325225716119.png" alt="image-20230325225716119" style="zoom:50%;" />

10. 安装成功后，下载 samples 示例程序，编译并运行 6-transpose，运行成功

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325233324858.png" alt="image-20230325233324858" style="zoom:50%;" />

11. 运行1-utilizatoins 里的 bandwidth，运行成功

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325233504870.png" alt="image-20230325233504870" style="zoom:50%;" />

## 注意

1. 由于本次实验我通过 todesk 远程终端连接家里的 windows 具备显卡的台式机完成，因此部分截图分辨率较低
2. 实验中途安装了终端美化程序，因此前后视觉效果稍有不同