# Attack

![[Pasted image 20250623114817.png]]

## How to attack

### attack type and idea
![[Pasted image 20250623153933.png]]

### calculate divergence
![[Pasted image 20250623154245.png]]

### optimize input
![[Pasted image 20250623154709.png]]

## White-box attack and Black-box attack
> if the model is not access to the attackers

### with training data
![[Pasted image 20250623155325.png]]

### without training data
#### data collection
generate **input** -> model API -> responsed **output**
----> get (input, output) data
#### ensemble attack
![[Pasted image 20250623155727.png]]

### one-pixel attack
![[Pasted image 20250623160256.png]]
### universal attack
> one noise can haddle all images

![[Pasted image 20250623160320.png]]

## Beyond image
![[Pasted image 20250623160554.png]]

## Attack in the physical world
![[Pasted image 20250623160916.png]]

![[Pasted image 20250623161112.png]]

## Adversarial Reprogramming
“Adversarial Reprogramming”常见释义为“对抗性重编程” 。从专业领域来讲，在计算机科学尤其是人工智能安全相关领域，它指通过特定的对抗手段，对系统（如机器学习模型）进行重编程。攻击者可能**利用对抗样本等方式，让原本正常运行的模型执行非预期的任务**，或者改变其行为模式，以达到恶意目的。例如在图像识别系统中，攻击者通过精心设计的对抗样本，使模型将猫的图片错误识别为狗，同时改变模型内部一些处理逻辑，实现这种对抗性重编程。 

## Backdoor in model
![[Pasted image 20250623162233.png]]

# Defence
## Passive
> fit the model
### filter
![[Pasted image 20250623162346.png]]
### compression and generator
![[Pasted image 20250623162646.png]]
### Randomization
![[Pasted image 20250623162830.png]]

## Proactive
> Adversarial training

![[Pasted image 20250623163229.png]]