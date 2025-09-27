# 概览
## Interpretable/Explainable and Powerful

![[Pasted image 20250624100429.png]]

![[Pasted image 20250624100919.png]]

## Goal of Explainable ML
![[Pasted image 20250624101619.png]]

![[Pasted image 20250624101717.png]]
# Local explaination
![[Pasted image 20250624102028.png]]

## Saliency map
> “Saliency map”指显著性图。它是一种在图像处理、计算机视觉等领域常用的工具。显著性图通过**突出图像中显著或吸引注意力的区域**，来显示图像中不同部分的重要程度。例如在一幅自然场景图像里，显著性图可能会让画面中的人物、动物等关键元素更加突出，而背景部分相对淡化，有助于计算机更快速聚焦关键信息，也可用于图像压缩、目标检测等任务。 

![[Pasted image 20250624102207.png]]

![[Pasted image 20250624102745.png]]

![[Pasted image 20250624102838.png]]

“Integrated gradient (IG)”指的是“集成梯度”。它是一种用于解释深度学习模型预测结果的技术。其基本原理是通过**计算从参考点到输入点的路径上梯度的积分，来衡量每个输入特征对模型输出的贡献程度**。在图像识别中，通过集成梯度可以知道图像中哪些像素区域对模型将图片分类为某一类别的决策起到了关键作用；在自然语言处理中，能明确文本中的哪些词汇对模型预测结果影响较大 。这种方法有助于理解模型的决策依据，增强模型的可解释性。 
![[Pasted image 20250624103030.png]]

## How a network processes the input data?
###  Visualization

![[Pasted image 20250624103511.png]]

### Probing
#probing
![[Pasted image 20250624103921.png]]
# Global explaination
## from local explaination
![[Pasted image 20250624105132.png]]

![[Pasted image 20250624105231.png]]

## from machine language to human language
![[Pasted image 20250624105433.png]]

![[Pasted image 20250624105900.png]]

# Conclusion
![[Pasted image 20250624110325.png]]


