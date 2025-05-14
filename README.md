# conditionBrepGen

添加了命令序列为条件的Brepgen模型

[Brepgen论文](https://github.com/samxuxiang/BrepGen)

[DeepCAD论文](https://github.com/ChrisWu1997/DeepCAD)

## 一. dataset.py

- dataset.py 添加了对没有相应文件的处理
- 在SurfPosData类中根据三维CAD模型的索引，输出相应命令序列的隐编码，完成数据集的加载

## 二. trainer.py

- 对命令序列隐编码使用[Cascaded Diffusion Models](https://arxiv.org/abs/2106.15282)进行数据增强
- 调用SurfPosData类的实例输入给torch.utils.data.DataLoader()，将经过数据增强的隐编码输入给网络

## 三. network.py

- 加入一个MLP处理隐编码，和整个网络一起训练，MLP将隐编码嵌入到与其他向量同一维度，进行相加。类似[Transformer](https://arxiv.org/abs/1706.03762)模型加入位置编码。

## 四. sample.py

- 无条件生成过程中， 基于 GAN 模型生成潜在向量后， 去噪器以该潜在向量为条件， 从噪声中还原出 CAD 模型的面信息， 然后逐步生成 CAD 模型的边与顶点信息. 再利用 B 样条曲线曲面拟合，最终得到完整的 B-Rep 表示 。

## 五. conditionsample.py

- 给定[DeepCAD数据集](https://github.com/ChrisWu1997/DeepCAD)三维CAD模型提取到的隐编码，进行条件生成

---

8_step文件夹为无条件生成结果，其中8.step为GAN模型生成的隐编码经过[DeepCAD模型](https://github.com/ChrisWu1997/DeepCAD)解码，生成的三维CAD模型；其他文件为同一隐编码该方法生成的三维CAD模型。

00981499文件夹为条件生成结果，00981499_474f71ccbbef9330eda51445_step_010.step为原文件，其中981499为该模型在ABC数据集的索引，其他文件为该方法条件生成结果。

粗略比较，该方法对比[Brepgen](https://github.com/samxuxiang/BrepGen) 无条件生成结果JSD指标提升10%。
