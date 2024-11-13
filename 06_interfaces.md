# 接口

目前的PlayerConroler形式，每新增一种人物就需要到交互逻辑里添加，所以我们要对可交互对象进行抽象，此后的战斗对象，对话对象，交易对象都可以自可交互对象派生

## 定义接口
可交互对象的接口必须包含一个interface方法
- C#和java一样有专门的interface关键字
- C#不支持多继承，但是可以实现多个接口
- C#子类可以在继承父类的同时，实现（继承）多个接口
- C#的类成员变量和成员变量默认是private的
- interface的方法默认public，且只能是public的，因为接口其本身就是一个公共契约
- 子类实现接口的时候也必须是按照public进行实现
- C#的函数重载和C++相同，条件为方法名相同，参数列表（即数量，类型，顺序至少一个不同）不同，返回类型不相关，访问修饰符不相关
- C#中的?
    - 空合并, `var value = obj?.ToString();`如果obj部不为null才调用ToString()
    - 空条件成员访问， `var length = obj?Length`同上
    - 三目运算符

- 在Scripts中定义一个Interactable接口,包含一个Interact方法
```C#
using UnityEngine;

public interface Interactable
{
   void Interact();
    
}
```
- 然后在Scripts中定义一个ChallegeNpcControler和一个DialogNpcControler去实现Interact方法，并且将这两个controler拖动给对应的npc
```C#
using UnityEngine;

public class DialogNpcControler : MonoBehaviour, Interactable
{
    public void Interact()
    {
        Debug.Log("Dialog is started, Let's have a short talk");
    }
}
```
```C#
using UnityEngine;

public class ChallegeNpcControler : MonoBehaviour, Interactable
{
    public void Interact()
    {
        Debug.Log("Battle is started, Let's do it!");
    }
}
```
- 在PlayerControler中加入从碰撞器获取Interactable接口，再去调用Interact方法
```C#
            if (collider != null)
            {
                // Debug.Log("something interactable");
                collider.GetComponent<Interactable>()?.Interact();
            }```