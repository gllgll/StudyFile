## 相对布局RelativeLayout

> 必须有一个参考点，其他组件的摆放相对于参考点的位置

|         属性          |                     功能                     |
| :-------------------: | :------------------------------------------: |
|      paddingTop       |                     顶部                     |
|     paddingBottom     |                     底部                     |
|      paddingLeft      |                     左边                     |
|     paddingRight      |                     右边                     |
|    android:gravity    |   用于设置布局管理器当中各子组件的摆放方式   |
| android:ignoreGravity | 用于指定哪个组件不受前面组件的影响（组件id） |

**主要用于指定组件相对于参考组件位置的**

- 内部类：ReaLativeLayout.LayoutParams：提供的xml属性，可以很好地控制相对布局管理器当中各组件的分布方式

|           属性           |         功能         |
| :----------------------: | :------------------: |
|   android:layout_above   | 位于另一个组件的上方 |
|   android:layout_below   | 位于另一个组件的下方 |
| android:layout_toLeftof  | 位于另一个组件的左侧 |
| android:layout_toRightof | 位于另一个组件的右侧 |

**用于设置组件与布局管理器哪边对齐的，下面四个属性的属性值都是布尔类型**

|               属性               |        功能        |
| :------------------------------: | :----------------: |
|  android:layout_alignParentTop   | 组件与父容器顶对齐 |
| android:layout_alignParentBottom | 组件与父容器底对齐 |
|  android:layout_alignParentLeft  | 组件与父容器左对齐 |
| android:layout_alignParentRight  | 组件与父容器右对齐 |

**用于设置组件与哪个组件的山下左右的边界对齐**

|            属性            |        功能        |
| :------------------------: | :----------------: |
|  android:layout_alignTop   | 与某一个组件顶对齐 |
| android:layout_alignBottom | 与某一个组件底对齐 |
| android:layout_alignRight  | 与某一个组件右对齐 |
|  android:layout_alignLeft  | 与某一个组件左对齐 |

**用于设置组件位于布局管理器的哪个位置**

|              属性               |                    功能                    |
| :-----------------------------: | :----------------------------------------: |
| android:layout_centerHorizontal | 表示这个组件位于布局管理器的水平居中的位置 |
|  android:layout_centerInParent  |    表示这个组件位于布局管理器的中间位置    |
|  android:layout_centerVertical  | 表示这个组件位于布局管理器的垂直居中的位置 |



## 线性布局LinearLayout

- 定义：将放入的组件进行水平或者垂直进行排列的

|        属性         | 用法                                                         |
| :-----------------: | :----------------------------------------------------------- |
| android:orientation | ①当属性值为：horizontal水平排列                                                             ②当属性值为：vertical垂直排列 |
|   android:gravity   | ①用于设置布局管理器内组件的位置的（居中显示，居右显示右下角显示通过                                                                                        ②layout_weight属性：用于设置组件所占的权重，就是组件占父容器剩余的比例，默认值是0，如果默认表示组件有多大就占多大的控件，当设置一个大于0的组件时，每个组件将对父容器的剩余控件进行分割，分割大小取决于layout_weight的值 |

## 表格布局TableLayout

- 不可以跨行和跨列显示

|           属性            |                   用法                   |
| :-----------------------: | :--------------------------------------: |
| android:collapseColumns=1 | 隐藏第二列，想隐藏不同列，可以用(, )隔开 |
| android:stretchColumns=1  | 拉伸第二列，想要拉伸多列，可以用(, )隔开 |
| android:shrinkColumns = 1 |           收缩第二列，原理同上           |

**注**：

1. 拉伸的意思是将剩余屏幕全部补充完整
2. 收缩的意思是当屏幕上内容宽度超过屏幕宽度，自动收缩起来

## 网格布局:GridLayout

- 屏幕被虚拟的细线划分为行列和单元格，每个单元格放置一个组件

- 可以跨行和跨列显示

  |        属性         |                             功能                             |
  | :-----------------: | :----------------------------------------------------------: |
  | android:columnCount |                    用于指定网格的最大列数                    |
  | android:orientation | 用于指定当我们没有为放置的组件分配行和列的时候，这些组件的排列方式 |
  |  android:rowCount   |                    用于指定网格的最大行数                    |

  **内部类提供的方法**：GridLayout.LayoutParams，提供xml属性，通过这些属性，可以控制网格布局管理器当中各子组件的分布

  |            属性             |                          功能                          |
  | :-------------------------: | :----------------------------------------------------: |
  |    android:layout_column    |             用于指定子组件位于网格的第几列             |
  |  android:layout_columnSpan  |               用于指定子组件横向跨几列的               |
  | android:layout_columnWeight | 用于指定子组件在水平方向的权重，分配水平剩余空间的比例 |
  |   android:layout_gravity    |     用于设置子组件采用什么方式占据这个网格的空间的     |
  |     android:layout_raw      |             用于指定子组件位于网格的第几行             |
  |   android:layout_rawSpan    |                用于指定子组件纵向跨几行                |
  |  android:layout_rowWeight   |            用于设置在子组件在垂直方向的权重            |

  

## 帧布局FrameLayout

|           属性            |                用法                |
| :-----------------------: | :--------------------------------: |
|    android:foreground     | 为当前帧布局管理器设置一个前景图像 |
| android:foregroundGravity |         为前景图像设置位置         |

> 前景图像：始终位于位置最上层的图像

