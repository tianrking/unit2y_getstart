# Unity 2D跑酷游戏基础教程

## 一、创建项目
1. 打开Unity Hub
2. 点击"New Project"
3. 选择"2D Core"
4. 设置项目名称和位置
5. 点击"Create project"

## 二、场景基础设置

### 1. 创建地面(Ground)
- 在Hierarchy中右键 → 2D Object → Sprite → Square
- 重命名为"Ground"
- Transform组件设置：
  * Position: X=0, Y=-3, Z=0
  * Scale: X=10, Y=1, Z=1
- 添加组件：
  * Add Component → Box Collider 2D
- 设置标签：
  * Inspector顶部点击Tag
  * Add Tag...
  * 点击+号添加"Ground"标签
  * 给Ground对象设置Ground标签

### 2. 创建玩家(Player)
- 在Hierarchy中右键 → 2D Object → Sprite → Square
- 重命名为"Player"
- Transform组件设置：
  * Position: X=0, Y=0, Z=0
  * Scale: X=1, Y=1, Z=1
- Sprite Renderer组件：
  * 修改Color为红色（方便识别）
- 添加组件：
  * Add Component → Rigidbody 2D
    - Body Type: Dynamic
    - Gravity Scale: 1
    - Mass: 1
    - Linear Drag: 0
    - Angular Drag: 0.05
    - Constraints: 勾选"Freeze Rotation Z"
  * Add Component → Box Collider 2D

## 三、脚本创建与设置

### 1. 创建脚本
- 在Project窗口右键 → Create → C# Script
- 命名为"PlayerController"
- 双击打开脚本

### 2. 完整代码
```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    private Rigidbody2D rb;
    public float speed = 5f;        // 移动速度
    public float jumpForce = 5f;    // 跳跃力度
    private bool isGrounded;        // 地面检测

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        // 左右移动
        float moveX = Input.GetAxisRaw("Horizontal");
        rb.velocity = new Vector2(moveX * speed, rb.velocity.y);

        // 跳跃
        if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            rb.AddForce(Vector2.up * jumpForce, ForceMode2D.Impulse);
            isGrounded = false;
        }
    }

    // 检测是否接触地面
    void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = true;
        }
    }
}
```

### 3. 添加脚本到玩家
- 将Project窗口中的PlayerController脚本拖到Hierarchy中的Player对象上
- 或在Player的Inspector中Add Component → PlayerController

## 四、最终检查清单

### 1. Ground对象检查：
- [x] 有Box Collider 2D组件
- [x] 位置正确（Y=-3）
- [x] 已设置Ground标签
- [x] Scale X值为10（足够长）

### 2. Player对象检查：
- [x] 有Rigidbody 2D组件（Dynamic）
- [x] 有Box Collider 2D组件
- [x] 有PlayerController脚本
- [x] Rigidbody 2D的Z轴旋转已锁定
- [x] 位置在地面上方

### 3. 游戏控制：
- A/D或左右方向键：左右移动
- 空格键：跳跃（需在地面上）

## 五、参数调整建议
1. 如果移动太快/慢：
   - 在Player的PlayerController组件中调整Speed值
   
2. 如果跳跃太高/低：
   - 在Player的PlayerController组件中调整Jump Force值

## 六、常见问题解决
1. 物体穿透：
   - 检查两个物体是否都有Collider组件
   - 确认Ground有正确的标签

2. 无法移动：
   - 确认Player的Rigidbody 2D是Dynamic类型
   - 检查PlayerController脚本是否正确附加

3. 无法跳跃：
   - 确认Ground标签设置正确
   - 检查Jump Force数值是否过小

这个教程涵盖了制作一个基础2D跑酷游戏所需的所有步骤。按照这个流程，应该能完整复现一个可以移动和跳跃的2D角色。

