import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import numpy as np

# 创建3D图
fig = plt.figure(figsize=(12, 8))
ax = fig.add_subplot(111, projection='3d')

# 定义网格范围
x = np.linspace(0, 1, 50)
y = np.linspace(0, 1, 50)
X, Y = np.meshgrid(x, y)

# 定义攻击目标区域 G（红色区域）
# G 是位于左上角的锥形区域
G = 1 - X - Y
G[G < 0] = np.nan  # 设置无效值

# 绘制攻击目标区域 G
ax.plot_surface(X, Y, G, alpha=0.7, color='red', label="Attack target set G")

# 定义初始攻击-防御屏障 X0（绿色曲线）
Z_X0 = np.maximum(0, 1 - X - Y - 0.2)  # 初始屏障稍低于 G 的顶部
ax.plot_surface(X, Y, Z_X0, alpha=0.7, color='green', label="Initial attack-defense barrier X0")

# 定义最终攻击-防御屏障 X*（蓝色区域）
Z_Xstar = np.maximum(0, 1 - X - Y - 0.4)  # 最终屏障比 X0 更低
ax.plot_surface(X, Y, Z_Xstar, alpha=0.7, color='blue', label="Final attack-defense barrier X*")

# 绘制动态安全状态演化轨迹（黑色虚线）
trajectory_x = np.linspace(0.2, 0.8, 10)
trajectory_y = np.linspace(0.2, 0.8, 10)
trajectory_z = 1 - trajectory_x - trajectory_y - 0.3  # 演化轨迹接近于 G 和 X* 之间
ax.plot(trajectory_x, trajectory_y, trajectory_z, color='black', linestyle='dashed', linewidth=2, label="Security state trajectory")

# 设置轴标签
ax.set_xlabel("Density of infected nodes in access subnetwork X1")
ax.set_ylabel("Density of infected nodes in service subnetwork X2")
ax.set_zlabel("Density of infected nodes in data subnetwork X3")

# 设置图例
ax.legend(
    loc='upper right',
    bbox_to_anchor=(1.2, 1),  # 将图例放在图外
    fontsize=10
)

# 调整视角
ax.view_init(elev=25, azim=-60)

# 显示图形
plt.tight_layout()
plt.show()
