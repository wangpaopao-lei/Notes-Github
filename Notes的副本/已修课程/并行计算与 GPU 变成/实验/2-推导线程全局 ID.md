# 作业 2：推导线程全局 ID











<center>
  课程——并行计算与 GPU 编程
</center>

<center>
  班级——202011321

<center>
  学号——2020211538
</center>

<center>
  姓名——王磊
</center>

---

## 多维坐标转换为一维

设坐标（x,y,z) 所在的维度大小为 (Dx,Dy,Dz)

对应的一维表示是 id=x+Dx\*y+Dx\*Dy*z

## 一维 grid，一维 block

- blockSize = blockDim.x
- blockID = blockIdx.x
- threadID = threadIdx.x

Id = blockIdx.x * blockDim.x + threadIdx.x\

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/0D5375659CA6D856600DC5010A46E3BC.png" alt="shadow-0D5375659CA6D856600DC5010A46E3BC" style="zoom:50%;" />



## 一维 grid，三维 block

- blockSize = blockDim.x * blockDim.y \* blockDim.z
- blockID = blockIdx.x
- threadID = blockDim.x \* blockDim.y \* threadIdx,z + blockDim.x * threadIdx.y + threadIdx.x

Id = (blockDim.x * blockDim.y \* blockDim.z) \* blockIdx.x + (blockDim.x \* blockDim.y \* threadIdx,z + blockDim.x * threadIdx.y + threadIdx.x)

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/215A56C69430074D6AC43C08CE018F59.png" alt="shadow-215A56C69430074D6AC43C08CE018F59" style="zoom:50%;" />

## 三维 grid，一维 block

- blockSize = blockDim.x
- blockID = gridDim.x \* gridDim.y * blockIdx.z + gridDim.x \* blockId.y + blockIdx.x
- threadID = threadIdx.x

Id = (gridDim.x \* gridDim.y * blockIdx.z + gridDim.x \* blockId.y + blockIdx.x) \* blockDim.x + threadIdx.x

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/1F10290441F2D9D4F02AABF6DA4AEFDB.png" alt="shadow-1F10290441F2D9D4F02AABF6DA4AEFDB" style="zoom:50%;" />

## 三维 grid，三维 block

- blockSize = blockDim.x * blockDim.y \* blockDim.z
- blockID = gridDim.x \* gridDim.y * blockIdx.z + gridDim.x \* blockId.y + blockIdx.x
- threadID = blockDim.x \* blockDim.y \* threadIdx,z + blockDim.x * threadIdx.y + threadIdx.x

Id = (gridDim.x \* gridDim.y * blockIdx.z + gridDim.x \* blockId.y + blockIdx.x) \* (gridDim.x \* gridDim.y * blockIdx.z + gridDim.x \* blockId.y + blockIdx.x) + blockDim.x \* blockDim.y \* threadIdx,z + blockDim.x * threadIdx.y + threadIdx.x

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/5120F4A3A6A7FE16C3BEE29DD352DB74.png" alt="shadow-5120F4A3A6A7FE16C3BEE29DD352DB74" style="zoom:50%;" />