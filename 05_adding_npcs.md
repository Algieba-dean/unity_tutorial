# 添加NPC

## 添加npc贴图
- 按照之前的方法将蓝色和绿色小人的贴图导入
- 将小人拖到Scene中，在Inspector的Spirate Renderer->Additional Settings->Sorting Layer改为player

## 修改相机设置
- 当角色穿过npc图层时，发现npc会像飘起来一样，浮在图层上面，我们要让当角色在背后时，角色被npc挡住，在前方时，角色挡住npc
- 点击菜单栏的Edit->Project Settings -> Graphics -> Camera Settings 将Transparency Sort Mode改为Custom Axis， 将Y设为1，其余为0

## 添加碰撞体积
- 给npc加上刚体，碰撞选Box Collider 2D框好大小
- 添加新的Layer Interactable
- 再到PlayerControler.cs中添加新的LayerMask，并添加检测逻辑到IsWalkable()中
    - 注意`Physics2D.OverlapCircle`可以直接用|检测多个图层
    `if (Physics2D.OverlapCircle(pos, Mathf.Epsilon, solidObjectsLayer|interactableLayer) == null)`

## 添加交互检测
- 在PlayerControler中添加交互检测逻辑
```C#
    protected void CheckInteractable()
    {
        if (Input.GetKeyDown(KeyCode.E))
        {
            var facingDirectoin = new Vector3(animator.GetFloat("x"), animator.GetFloat("y"));
            var interactPosition = transform.position + facingDirectoin;
            // Debug.DrawLine(transform.position, interactPosition,Color.red, 1f);
            var collider = Physics2D.OverlapCircle(interactPosition, Mathf.Epsilon, interactableLayer);
            if (collider != null)
            {
                Debug.Log("something interactable");
            }
        }
    }
```

- 其原理是检测E键按下，然后通过animator的x,y参数获取一个运动的方向，再通过当前位置+运动方向的位置去检测是否有可交互层
    - Debug小技巧`Debug.DrawLine(transform.position, interactPosition,Color.red, 1f);`
    可以在Sence模式下画一条线，用于debug