
[TOC]

## Swing

用于开发图形用户界面的包

![Swing概述.jpg](.\Photo\Swing概述.jpg)

### 窗体（JFrame）

### 对话框窗体（JDialog ）

### 标签（JLable）

主要作用就是展示一段文字

### 如何在窗体里添加图片

使用label标签
### 布局

#### 绝对布局（null布局）

硬性指定了组件在容器中的位置和大小
使用绝对布局的窗口通常都是固定大小，组件位置和形状不会随着窗体的改变而发生变化

#### 流布局（FlowLayout）

从左到右排列，默认居中对齐
像流水一样，向某个方向流动，遇到障碍就折回

#### 边界布局（borderLayout）
窗体容器的默认布局

*	添加组建时，需要指定区域，否则默认添加到CENTER区
*	同一区域的组件会相互覆盖

#### 网格布局（GridLayout）

### 面板（JPanel）

窗体中可以有多个容器，容器之间互不干扰

#### 滚动面板（JScrollPane）

#### 按钮（JButton）

#### 单选按钮（JRadioButton）

#### 复选框（JCheckBox）

#### 下拉框（JComboBox）

#### 列表框（Jlist）

#### 文本框（JTextField）

#### 密码框（JPasswordField）

#### 文本域（JTextArea）

### 动作事件监听器(ActionListener)

~~~java
addActionListener(ActionListener() {
	@Override
	public void actionPerformed(ActionEvent arg0) {//这里的ActionEvent会捕捉动作触发者，返回arg0参数
    // TODO Auto-generated method stub
    }
}
~~~

### 焦点事件监听器（FocusListener）

~~~java
void focusGained(FocusEvent e)//组件获取焦点时调用的方法
void focusLost(FocusEvent e)//组件失去焦点时调用的方法
~~~

可以使用自定义实现焦点监听器类，简化代码

### 事件

#### 键盘事件

KeyListener键盘监听（接口）

方法

~~~java
	public void keyTyped(KeyEvent e);//发生击键时触发
	public void keyPressed(KeyEvent e);//按键被按下时触发
    public void keyReleased(KeyEvent e);//按键被释放时触发
    //keyEvent表示键盘事件类
~~~

#### 鼠标事件

MouseListener鼠标事件监听（接口）

~~~java
	public void MouseEntered(MouseEvent e);//鼠标移入组件时被触发
	public void MosuePressed(MouseEvent e);//鼠标按键被按下时触发
    public void MouseReleased(MouseEvent e);//鼠标按键被释放时触发
    public void mouseClicked(MouseEvent e);//发生单击事件时被触发
    public void mouseExited(MouseEvent e);//鼠标移除组件时被触发
    //MouseEvent表示鼠标事件类
~~~

#### 窗体焦点事件

WindowFocusListener窗体焦点监听

~~~java
public void windowGainedFocus(WindowEvent e)//窗体获得焦点时被触发
public void windowLostFocus(WindowEvent e)//窗体失去焦点时被触发
~~~

#### 窗体状态事件

WindowStateListener窗体状态监听（接口）

~~~java
public void windowStateChanged(WindowEvent e)//窗体状态发生变化时被触发
getNewState()//获得窗体现在的状态
getOldState()//获得窗体以前的状态
~~~

#### 窗体事件

WindowListener（接口）

![窗体事件.jpg](.\Photo\窗体事件.jpg)

*点击关闭时不会触发失去激活状态*

#### 选项事件

ItemListener选项事件监听（接口）

~~~java
public void itemStateChanged(ItemEvent e);//在用户选定或取消某项时触发的事件
~~~

ItemEvent选项事件

~~~java
public object getItem()//选中返回事件
public int getStateChange()//返回选中的状态
public object getSource()//返回触发选项事件的组件
~~~

### 分割面板（JSplitPane）

JSplitPane构造方法

~~~java
JSplitPane()
JSplitPane(int newOrientation)//指定分割方向（水平JSplitPane.HORIZONTAL_SPLIT或垂直JSplitPane.VERTICAL_SPLIT）
JSplitPane(int newOrientation,boolean newContinuousLayout)//指定是否重绘
~~~

### 选项卡面板（JTabbedPane）

![选项卡面板.jpg](.\Photo\选项卡面板.jpg)

~~~java
setTabPlacement方法//设置选项卡标签位置
setTabLayoutPolicy方法//设置选项卡面板布局方式
~~~

### 桌面面板和内部窗体

JDesktopPane类：表示虚拟桌面
JInternalFrame类：表示内部窗体

### 菜单栏（JMenuBar）

创建菜单栏步骤

1.	创建菜单栏对象（JMenuBar类）
2.	创建菜单对象(JMenu类)
3.	创建菜单项对象(JMenuItem类)

### 弹出式菜单（JPopupMenu）

添加弹出式菜单

~~~java
JPopupMenu popupMenu=new JPopupMenu();//新建弹出式菜单对象
//添加鼠标事件监听
//调用对应的鼠标事件方法
//判断是否为该组件的弹出菜单触发事件
//判断在哪个组件中显示以及显示的位置
~~~

### 工具栏(JToolBar)

#### 文件选择器(JFileChooser)

![文件选择器.jpg](.\Photo\文件选择器.jpg)

![文件选择器2.jpg](.\Photo\文件选择器2.jpg)

==读文件==

1.	打开文件
2.	判断文件大小
3.	分配足够的内存空间
4.	把文件读入内存
5.	关闭文件

#### 进度条（JProgressBar）

~~~java
setIndeterminate(true)//不确定进度条
setIndeterminate(false)//确定进度条（默认）
setStringPainted(true)//设置显示提示信息
	setValue(66)//设置进度条进度值
	setString("66%")//设置进度条显示的文本
~~~

### 表格

创建表格

![表格.jpg](.\Photo\表格.jpg)

#### 表格模型(TableModel)

*	TableModel接口
	*	AbstractTableModel抽象类
		*	DefaultTableModel类
			*	添加行：addRow()/insertRow()
			*	修改行：setValueAt()
			*	删除行：removeRow()

JTable使用表格模型

*	构造方法中直接传入表格模型
*	调用设置表格模型的方法

#### 表格模型事件

TableModelListener表格模型事件监听

tableModel.addTableModelListener(TableModelListener listener);

public void tableChanged(TableModelEvent e);

TableModeEvent表格模型事件

getColumn()返回触发事件的列//TableModelEvent.INSERT插入事件

getFirstRow()返回第一个被更改的行//TableModelEvent.UPDATE修改事件

getType()返回事件类型//TableModelEvent.DELETE删除事件


