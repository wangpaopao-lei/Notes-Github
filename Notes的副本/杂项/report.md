## 数据科学导论实验报告（一）

题目: 宿舍管理程序
姓名：王磊
班级：2020211321
学号：2020211538
提交日期：2022年3月30日

### 一 总体设计

程序分为两个文件，main.py和functions.py。main.py为程序主函数，包含一个死循环，用于呈现菜单界面，每次操作完成都会回到菜单，输入”0“时结束程序；functions.py包含程序的主要功能函数，用于实现学生信息的新建、显示、查询操作。

### 二 详细设计

1. 主模块

   1. 导入模块

      ```python
      import functions
      ```

   2. 主函数

      ```python
      while True:
          print('*'*50)
          print(' ')
          print('欢迎使用<宿舍管理系统>')
          print(' ')
          print('1. 查找学生')
          print('2. 新增学生')
          print('3. 显示全部')
          print('0. 退出系统')
          print('*'*50)
          print('')
          action=input('请选择希望执行的操作：')
          print('-'*50)
      
          if action=='0':
              break
          elif action=='1':
              functions.find_student()
          elif action=='2':
              functions.new_student()
          elif action=='3':
              functions.show_all()
      
      ```

2. 功能模块

   1. 新增学生new_student()

      ```python
      def new_student():
          """新增学生信息"""
          print('新增学生')
          print(' ')
          id = input('请输入学号：')
          print(' ')
          name = input('请输入姓名：')
          print(' ')
          gender = input('请输入性别：')
          print(' ')
          room = input('请输入房间号：')
          print(' ')
          tel = input('请输入电话：')
      
          student = {
              'id': id,
              'name': name,
              'gender': gender,
              'room': room,
              'tel': tel
          }
          studentlist.append(student)
          print('添加 %s 成功' % id)
      ```

   2. 显示全部show_all()

      ```python
      def show_all():
          """显示全部学生信息"""
          if len(studentlist) == 0:
              print('当前没有学生信息，请添加')
              return
          print('显示所有学生')
          print('学号\t姓名\t性别\t房间号\t电话')
          print('=' * 50)
          for student in studentlist:
              print('%s\t\t%s\t\t%s\t\t%s\t\t%s' % (
                  student['id'], student['name'], student['gender'], student['room'], student['tel']))
      ```

   3. 查找学生find_student()

      ```python
      def find_student():
          """根据学号搜索学生"""
          id = input('请输入要搜索的学号：')
          print(' ')
          for student in studentlist:
              if id==student['id']:
                  print('学号\t姓名\t性别\t房间号\t电话')
                  print('=' * 50)
                  print('%s\t%s\t%s\t%s\t%s' % (
                      student['id'], student['name'], student['gender'], student['room'], student['tel']))
                  break
              else:
                  print('抱歉，没有找到学生%s' % id)
      ```

### 三 调试及运行结果

* 运行环境：pycharm python3.8

* 运行结果：

  1. 主界面

     ![image-20220329213901553](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220329213901553.png)

  2. 退出系统

     ![image-20220329214008406](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220329214008406.png)

  3. 新增学生

     ![image-20220329214134535](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220329214134535.png)

  4. 显示全部

     ![image-20220329214333978](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220329214333978.png)

  5. 查找学生

     * 查找成功

       ![image-20220329221952695](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220329221952695.png)

     * 查找失败

       ![image-20220329222018100](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220329222018100.png)

### 四 源代码

* main.py

```python
import functions

while True:
    print('*'*50)
    print(' ')
    print('欢迎使用<宿舍管理系统>')
    print(' ')
    print('1. 查找学生')
    print('2. 新增学生')
    print('3. 显示全部')
    print('0. 退出系统')
    print('*'*50)
    print('')
    action=input('请选择希望执行的操作：')
    print('-'*50)

    if action=='0':
        break
    elif action=='1':
        functions.find_student()
    elif action=='2':
        functions.new_student()
    elif action=='3':
        functions.show_all()


```



* Functions.py

  ```python
  studentlist = []
  
  
  def new_student():
      """新增学生信息"""
      print('新增学生')
      print(' ')
      id = input('请输入学号：')
      print(' ')
      name = input('请输入姓名：')
      print(' ')
      gender = input('请输入性别：')
      print(' ')
      room = input('请输入房间号：')
      print(' ')
      tel = input('请输入电话：')
  
      student = {
          'id': id,
          'name': name,
          'gender': gender,
          'room': room,
          'tel': tel
      }
      studentlist.append(student)
      print('添加 %s 成功' % id)
  
  
  def find_student():
      """根据学号搜索学生"""
      id = input('请输入要搜索的学号：')
      print(' ')
      for student in studentlist:
          if id==student['id']:
              print('学号\t姓名\t性别\t房间号\t电话')
              print('=' * 50)
              print('%s\t%s\t%s\t%s\t%s' % (
                  student['id'], student['name'], student['gender'], student['room'], student['tel']))
              break
          else:
              print('抱歉，没有找到学生%s' % id)
  
  
  def show_all():
      """显示全部学生信息"""
      if len(studentlist) == 0:
          print('当前没有学生信息，请添加')
          return
      print('显示所有学生')
      print('学号\t姓名\t性别\t房间号\t电话')
      print('=' * 50)
      for student in studentlist:
          print('%s\t\t%s\t\t%s\t\t%s\t\t%s' % (
              student['id'], student['name'], student['gender'], student['room'], student['tel']))
  
  ```

  