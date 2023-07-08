# 三体运动模拟器

这是一个三体运动模拟器的项目地址，项目仍处于开发状态。这里是 [项目地址](https://3BodyS.github.io)

## 设计方向/目标

最终的目标是设计一个可交互的三体运动模拟器，可以自己输入质量，速度，参数等。还要符合物理以及天文相关定律和规则，如牛顿运动定律，万有引力定律，轨道力学和开普勒定律，这样才能最大化模拟真实场景。

更进一步的目标是可以模拟碰撞，等更拟真的事件。还有一些细节，比如星球的尾迹，时间尺度加速等等....

## 三体运动的方程

对于三体问题，我们可以使用牛顿万有引力定律和牛顿第二定律来描述天体之间的相互作用。假设有三颗恒星分别为恒星1、恒星2和恒星3，它们的质量分别为 m₁、m₂ 和 m₃，位置分别为 (x₁, y₁, z₁)、(x₂, y₂, z₂) 和 (x₃, y₃, z₃)。

- 根据牛顿第二定律，恒星1的运动方程可以表示为：

x₁'' = G * (m₂ * (x₂ - x₁) / r₁² + m₃ * (x₃ - x₁) / r₂²)

y₁'' = G * (m₂ * (y₂ - y₁) / r₁² + m₃ * (y₃ - y₁) / r₂²)

z₁'' = G * (m₂ * (z₂ - z₁) / r₁² + m₃ * (z₃ - z₁) / r₂²)

恒星2和恒星3的运动方程可以类似地表示。

- 在上述方程中，G 是万有引力常数，r₁、r₂ 是恒星之间的距离，可以通过欧几里得距离公式计算： 

r₁ = sqrt((x₂ - x₁)² + (y₂ - y₁)² + (z₂ - z₁)²)

r₂ = sqrt((x₃ - x₁)² + (y₃ - y₁)² + (z₃ - z₁)²)

这些方程仅仅描述了每个恒星在三体系统中的运动规律。为了模拟三体运动，需要使用数值方法（如欧拉法、龙格-库塔法等）来逐步计算天体的位置和速度。

需要注意的是，由于三体问题的复杂性，对于长时间模拟或特定的初始条件，可能需要考虑其他因素，例如动量守恒、轨道稳定性等。此外，为了增加模拟的准确性，可能需要使用更精细的数值方法和调整时间步长。

## 三体运动初步代码框架

这里就是把上面的三体运动数学模型转换成JavaScript代码函数。

计算距离：

```javascript
// 计算距离的函数
function calculateDistance(x1, y1, z1, x2, y2, z2) {
  return Math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2 + (z2 - z1) ** 2);
} 
```

计算恒星在x轴方向的加速度：

```javascript
// 计算恒星在x轴方向的加速度
function calculateAccelerationX1(G, m1, m2, m3, x1, y1, z1, x2, y2, z2, x3, y3, z3) {
  const r1 = calculateDistance(x1, y1, z1, x2, y2, z2);
  const r2 = calculateDistance(x1, y1, z1, x3, y3, z3);

  return (
    (G * m2 * (x2 - x1)) / (r1 ** 3) + (G * m3 * (x3 - x1)) / (r2 ** 3)
  );
} 
```

计算恒星在y轴方向的加速度：

```javascript
// 计算恒星在y轴方向的加速度
function calculateAccelerationY1(G, m1, m2, m3, x1, y1, z1, x2, y2, z2, x3, y3, z3) {
  const r1 = calculateDistance(x1, y1, z1, x2, y2, z2);
  const r2 = calculateDistance(x1, y1, z1, x3, y3, z3);

  return (
    (G * m2 * (y2 - y1)) / (r1 ** 3) + (G * m3 * (y3 - y1)) / (r2 ** 3)
  );
} 
```

计算恒星在z轴方向的加速度：

```javascript
// 计算恒星在z轴方向的加速度
function calculateAccelerationZ1(G, m1, m2, m3, x1, y1, z1, x2, y2, z2, x3, y3, z3) {
  const r1 = calculateDistance(x1, y1, z1, x2, y2, z2);
  const r2 = calculateDistance(x1, y1, z1, x3, y3, z3);

  return (
    (G * m2 * (z2 - z1)) / (r1 ** 3) + (G * m3 * (z3 - z1)) / (r2 ** 3)
  );
} 
```

用法：

```javascript
const G = 6.67430e-11; // 万有引力常量
const m1 = 1.9885e30; // 恒星1的质量
const m2 = 1.9885e30; // 恒星2的质量
const m3 = 1.9885e30; // 恒星3的质量

const x1 = 0; // 恒星1的x坐标
const y1 = 0; // 恒星1的y坐标
const z1 = 0; // 恒星1的z坐标

const x2 = 1e11; // 恒星2的x坐标
const y2 = 0; // 恒星2的y坐标
const z2 = 0; // 恒星2的z坐标

const x3 = 0; // 恒星3的x坐标
const y3 = 1e11; // 恒星3的y坐标
const z3 = 0; // 恒星3的z坐标

// 使用函数计算恒星1在x轴方向的加速度
const accelerationX1 = calculateAccelerationX1(G, m1, m2, m3, x1, y1, z1, x2, y2, z2, x3, y3, z3);
console.log("恒星1在x轴方向的加速度：" + accelerationX1);

// 使用函数计算恒星1在y轴方向的加速度
const accelerationY1 = calculateAccelerationY1(G, m1, m2, m3, x1, y1, z1, x2, y2, z2, x3, y3, z3);
console.log("恒星1在y轴方向的加速度：" + accelerationY1);

// 使用函数计算恒星1在z轴方向的加速度
const accelerationZ1 = calculateAccelerationZ1(G, m1, m2, m3, x1, y1, z1, x2, y2, z2, x3, y3, z3);
console.log("恒星1在z轴方向的加速度：" + accelerationZ1); 
```

如需更多信息和更新，请前往我的 [B站专栏](https://member.bilibili.com/platform/text-read-list?id=728647)
